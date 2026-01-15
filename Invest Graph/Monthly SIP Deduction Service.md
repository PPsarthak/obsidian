#invest-graph 

Documentation - `deductSIP()`
This method is a scheduled job responsible for processing monthly Systematic Investment Plan (SIP) deductions for users based on their configured SIP settings. It creates and persists installment records for SIPs that are due on the current day.

The job runs based on a CRON expression configured through the `scheduling.sip-deduction-cron` property, and is designed to execute **once per day**.

---
#### Workflow Summary:
1. **Determine Current Date**: Captures today’s date and extracts the day of the month.
2. **Fetch Due SIPs**: Retrieves all **active** SIP configurations scheduled for deduction on today's date.
3. **Skip Pre-Start SIPs**: Ignores SIPs whose start date is later than today's date.
4. **Avoid Duplicate Installments**: Skips any SIP configuration that already has an installment for today’s investment date.
5. **Fetch NAV & Calculate Units**:
    - Retrieves the latest NAV (Net Asset Value) for the mutual fund scheme.
    - Calculates the number of units to allot based on the SIP amount and NAV.
6. **Persist Installment**:
    - Saves a new `UserMonthlySIPInstallment` record with the investment details.
    - Updates the SIP configuration with the latest `lastProcessedDate`.
7. **Logging**: Logs key actions and outcomes for traceability and observability.

##### Notes:
- SIP installments are only processed **on or after** the configured SIP start date.
- Each SIP installment is processed **once per month**; duplicate entries are prevented.
- NAV is assumed to be pre-fetched and available before the job runs (configured operationally).
- **No retry mechanism** is currently implemented in case of failure during installment creation. Failed entries will be **skipped**, which **requires future enhancement**.

#### Potential Enhancements:
- Implement a **retry mechanism or error queue** for failed SIP installments to ensure they are not lost permanently due to transient issues.
- Introduce **metrics and alerting** around failed installment processing to improve reliability and visibility.
- Consider **batch saving** or **transactional integrity** to handle partial failures more robustly.