#system-design-v2 #design-questions 

### 1. Problem Scope
The system should allow users to subscribe to notifications when a specific condition is met, such as:
- A product that is currently out of stock becomes available.
- A price drops below a certain threshold.
- A feature, content, or service becomes active.

### 2. Functional Requirements
1. Users can register a Notify Me request for a specific item or condition.
2. The system should track all pending notification subscriptions.
3. When the specified condition becomes true, all subscribed users must be notified.
4. A user should receive the notification only once per subscription.
5. After successful notification, the subscription should be removed or marked as completed.
6. The system should handle multiple users subscribing to the same item or event.
7. Notification channels can include email, SMS, or push (abstracted for now).
8. Real-time notification is preferred but eventual notification is acceptable.
9. The system should be scalable to handle a large number of subscriptions.
10. Failure handling and retries are desirable but can be simplified initially.

### 3. Entities and Responsibilites

| Class                | Responsibility                                                                                                   |
| -------------------- | ---------------------------------------------------------------------------------------------------------------- |
| Observer / User      | The actual user who will call `subscribe()` & `unsubscribe()`                                                    |
| Observable / Channel | The observable who will push the notifications to subscribed users                                               |
| Orchestrator         | Mediator between Orchestrator and Observer - maintains the subscriptions list and handles requests from both end |
| Router               | Handles actual routing - pushes the notification to subcribers                                                   |
| User Data            | Data Holder - holds data of user/observer                                                                        |
| Notification Data    | Data Holder - holds data of the notification that will be sent                                                   |

### 4. Design Patterns
- Observer
- Strategy
- Mediator

### 5. UML Class Diagram

![[UML Class Diagram for Notify-Me Button Functionality.png|1150]]
### 6. Code
```java
public class UserData {
	private String userId;
	private String userName;
}

public class NotificationData {
	private String notificationId;
	private String notificationMessage;
	private String topic;
}

public class Router {
	public boolean routeNotification(NotificationData data, Set<Observer> subscribers) {
		try {
			for(Observer subscriber : subscribers) subscriber.handleNotification(data);
			return true;
		} catch (Exception e) {
			System.out.println("Error while pushing notification: " + e);
			return false;
		}
	}
}

public class Orchestrator {
	private final Router router;
	private final Map<String, Observable> topicToObservableMapping = new HashMap<>();
	private final Map<Observable, Set<Observer>> subscriptionMapping = new HashMap<>();

	public Orchestrator(Router router) {
		this.router = router;
	}

	public boolean addSubscription(String topicName, Observer subscriber) {
		if(!topicToObservableMapping.containsKey(topicName)) {
			return false;
		}
		Observable subscriptionObservable = topicToObservableMapping.get(topicName);
		Set<Observer> subscribersOfObservable = subscriptionMapping.get(subscriptionObservable);
		subscribersOfObservable.add(subscriber);
		subscriptionMapping.put(subscriptionObservable, subscribersOfObservable);
		return true;
	}
	
	public boolean removeSubscription(String topicName, Observer subscriber) {
		if(!topicToObservableMapping.containsKey(topicName)) {
			return false;
		}
		Observable subscriptionObservable = topicToObservableMapping.get(topicName);
		Set<Observer> subscribersOfObservable = subscriptionMapping.get(subscriptionObservable);
		subscribersOfObservable.remove(subscriber);
		subscriptionMapping.put(subscriptionObservable, subscribersOfObservable);
		return true;
	}
	
	public boolean routeNotification(String topicName, NotificationData data) {
		if(!topicToObservableMapping.containsKey(topicName)){
			System.out.println("<<ERROR>> TOPIC " + topicName + " is not registered");
			return false;
		}
		Observable subscriptionObservable = topicToObservableMapping.get(topicName);
		Set<Observer> subscribersOfObservable = subscriptionMapping.get(subscriptionObservable);
		return router.routeNotification(data, subscribersOfObservable);
	}
	
	public boolean addTopic(String topicName, Observable observable) {
		if(topicToObservableMapping.containsKey(topicName)) {
			return false;
		}
		
		topicToObservableMapping.put(topicName, observable);
		subscriptionMapping.put(observable, new HashSet<>());
		return true;
	}
}

interface Observer {
	boolean subscribe(String topicName, Observer subscriber);
	boolean unSubscribe(String topicName, Observer subscriber);
	void handleNotification(NotificationData data);
}

interface Observable {
	boolean pushNotification(Notification Data);
}

public class Observable1 implements Observable{
    private final String TOPIC_NAME;
    private final Orchestrator orchestrator;

    public Observable1(Orchestrator orchestrator, String topicName) {
        this.orchestrator = orchestrator;
        this.TOPIC_NAME = topicName;
    }
    @Override
    public boolean pushNotification(NotificationData data) {
        return orchestrator.routeNotification(TOPIC_NAME, data);
    }
    public NotificationData generateNotificationData(){
        return new NotificationData("Message Details: " + Math.random(), TOPIC_NAME);
    }
}

public class Observer1 implements Observer {
    private final UserData user;
    private final Orchestrator orchestrator;

    public Observer1(UserData user, Orchestrator orchestrator) {
        this.user = user;
        this.orchestrator = orchestrator;
    }

    @Override
    public boolean subscribe(String topicName) {
        return orchestrator.addSubscription(topicName, this);
    }

    @Override
    public boolean unSubscribe(String topicName) {
        return orchestrator.removeSubscription(topicName, this);
    }

    @Override
    public void handleNotification(NotificationData data) {
        System.out.println("User " + user.getUserId() + " " + user.getUserName() + 
        " has received the notification " + data.toString() + "successfully");
    }
}
```


### 7. Interview Follow-up Questions
