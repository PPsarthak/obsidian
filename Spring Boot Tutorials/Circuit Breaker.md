---
status: To Review
last-reviewed: 2025-09-14
tags:
  - spring-boot
  - circuit-breaker
---
#placement-preparation #spring-boot #circuit-breaker

Also read - System Design: [[System Design/HLD/Circuit Breaker|Circuit Breaker]]
#### Cascading Failure in Microservices
Microservices architecture has grown in popularity in software development, however, it is essential to acknowledge that it does come with its own set of challenges - one such issue is cascading failures, wherein the failure of a single microservice can have an impact on others that depend on it, and a domino effect occurs causing a system-wide failure.
#### ⚡What is Circuit Breaker?
Circuit Breaker is a resilience pattern used to prevent a system from repeatedly trying to execute an operation that's likely to fail - especially useful in microservice architectures.

The idea of this pattern is to prevent calling a microservice in case of abnormal behavior in that service (service down or timeout…)

A Circuit Breaker monitors the interaction between services and **"breaks" the circuit** (i.e., stops further calls) if a service is failing consistently. It helps to:
- Avoid system overload
- Improve fault tolerance
- Allow failing services to recover
#### States of Circuit Breaker
1. **Closed State (normal)**  
	Initial state of circuit breaker - requests flow normally. 
	It keeps continuously monitoring the number of failures occurring within the configured time period.
	If the failure rate exceeds the specified threshold, Its state will change to *Open* state. 
2. **Open State (tripped)**  
    When failures cross a threshold, the circuit "opens" and all further calls are blocked (or redirected to fallback).
    Request callings will fail, and exceptions will be thrown. 
    Open state remains until the timeout ends, then change to *Half Open* state.
3. **Half-Open State (test)**  
    After a cool-down period, a limited number of requests are allowed to check if the service has recovered.
    If the failure rate is greater than the specified threshold, it switches again to Open state. Otherwise, it is Closed state.
---
#### 🔧How it works?
Let’s say we have 2 systems: S1 and S2 which are up and running and communicate with each other.
![[Circuit Breaker Images 1.png|850]]

But what happens with S1’s request if S2 is down or S2 is not responding? 
S1's request fails or hangs without a response till a timeout limit is reached.

But what happens if S1 continuously calls S2? 
S1 will continue to fail or hang

This is where a circuit breaker comes into action.
![[Circuit Breaker Images 2.png|950]]
Here, when S1 calls S2 and S2 is up and running, we say that the circuit breaker is in *CLOSED* state. In this case, the circuit breaker simply forwards the request to S2

When S2 responds with a failure or a timeout, the circuit breaker increases an internal counter. 
If the count of failures or timeouts exceeds a configured threshold, the circuit breaker switches to *OPEN* state. In this case, the circuit breaker immediately returns an error to S1 without even trying to call S2

After a configured time, the circuit breaker switches from *OPEN* to a *HALF-OPEN* state. In this case, it lets a few requests from S1 to pass through to the S2 to check if it’s still unavailable or slow. If S2 still returns failures or timeouts and their rates is above the configured threshold, it switches back to the *OPEN* state. If their rates is below the configured threshold, it switches to the *CLOSED* state and resume the normal operation.

**There are 2 types of circuit breakers:**
- count-based: switches from CLOSED to OPEN if the last N calls failed or were slow
- time-based: switches from CLOSED to OPEN if the responses in the last N seconds failed or were slow

### Resilience4J

##### Methods 
- `slidingWindowType()`: specifies the type of circuit breaker. It could take one of these values:
  `SlidingWindowType.COUNT_BASED` or  `SlidingWindowType.TIME_BASED.`
- `failureRateThreshold()`: configure the failure rate threshold
- `slowCallRateThreshold()`: configure the slow call rate threshold (%)
- `slowCallDurationThreshold()`: configures the time beyond which a call is considered slow (seconds)
- `minimumNumberOfCalls()`: specifies the minimum number of calls that are required before the circuit breaker can calculate the error rate or slow call rate
- `waitDurationInOpenState()`: specifies the time that the circuit breaker should wait before switching to a half-open state
- `permittedNumberOfCallsInHalfOpenState()`: configures the number of calls that will be allowed in the half-open state
- `maxWaitDurationInHalfOpenState()`: determines the amount of time a circuit breaker can stay in the half-open state before switching back to the open state

%% Incomplete... %%

---
Reference: https://medium.com/@danaprata/reliable-spring-boot-apps-intro-to-circuit-breakers-23eac85c8dab