#system-design-v2 #design-questions 

### 1. Problem Scope
Design a Parking Lot Management System that supports parking and un-parking of vehicles of different types in a multi-level parking lot. 
The system should automatically assign parking spots based on vehicle type, track availability in real time, calculate parking fees, and generate parking tickets. 
The design should be extensible, maintainable, and capable of handling concurrent vehicle entries and exits.

### 2. Functional Requirements
- **Vehicle Management**
	- Support multiple vehicle types (e.g., Bike, Car, Truck).
	- Each vehicle has a unique registration number.
	- Each vehicle occupies exactly one parking spot.
- **Parking Spot Management**
	- Parking lot contains multiple floors.
	- Each floor has multiple parking spots.
	- Parking spots are categorized by vehicle type.
	- A spot can either be free or occupied.
	- A vehicle can be parked only in a compatible spot.
- **Ticketing System**
	- Generate a parking ticket at entry.
	- Ticket contains:
		- Ticket ID
		- Vehicle number & type
		- Entry time
		- Parking spot ID
	- Ticket must be required for exit.
- **Un-Parking & Payment**
	- On exit, calculate parking fees based on duration and vehicle type.
	- Payment must be completed before exit.
	- After exit, the parking spot becomes available again.
### 3. Entities and Responsibilites

| Class        | Type           | Responsibility         |
| ------------ | -------------- | ---------------------- |
| Vehicle Type | Enum           | TWO/FOUR/EIGHT WHEELER |
| Vehicle      | Abstract Class |                        |
| Parking Spot | Abstract Class |                        |
| Ticket       | Class          |                        |

### 4. Design Patterns
- 

### 5. UML Class Diagram


### 6. Code

```java
public enum VehicleType {
	TWO_WHEELER,
	FOUR_WHEELER,
	EIGHT_WHEELER
}

public abstract class Vehicle {
	private VehicleType type;
	private UUID vehicleNumber;
}

public class Bike implements Vehicle {
	
}

public class Car implements Vehicle {
	
}

public class Truck implements Vehicle {
	
}

public abstract class ParkingSpot {
	private VehicleType type;
	private UUID parkingSpotId;
	
	public ParkingSpot() {
		this.UUID = UUID.randomUUID();
	}
}

public class BikeParkingSpot implements ParkingSpot {
	public BikeParkingSpot() {
		this.type = VehicleType.TWO_WHEELER;
		super();
	}
}

public class CarParkingSpot implements ParkingSpot {
	public CarParkingSpot() {
		this.type = VehicleType.FOUR_WHEELER;
		super();
	}
}

public class TruckParkingSpot implements ParkingSpot {
	public TruckParkingSpot() {
		this.type = VehicleType.EIGHT_WHEELER;
		super();
	}
}

public interface ParkingSpotFactory {
	ParkingSpot getParking();
}

public class BikeParkingSpotFactory implements ParkingSpotFactory {
	@Override
	public ParkingSpot getParking() {
		return new BikeParkingSpot();
	}
}

public class CarParkingSpotFactory implements ParkingSpotFactory {
	@Override
	public ParkingSpot getParking() {
		return new CarParkingSpot();
	}
}

public class TruckParkingSpotFactory implements ParkingSpotFactory {
	@Override
	public ParkingSpot getParking() {
		return new TruckParkingSpot();
	}
}

public class Ticket {
	private UUID ticketId;
	private LocalDateTime entryTime;
	private LocalDateTime exitTime;
	private Vehicle vehicle;
	private ParkingSpot parkingSpot;
	
	public Ticket(Vehicle vehicle, ParkingSpot parkingSpot) {
		this.ticketId = UUID.randomUUID();
		this.entryTime = LocalDateTime.now();
		this.vehicle = vehicle;
		this.parkingSpot = parkingSpot;
	}
}

public enum GateType {
	ENTRY_GATE,
	EXIT_GATE
}

public abstract class Gate {
	private GateType gateType;
	private ParkingManager manager;
	private ParkingFactoryRegistry registry;
	
	abstract void action(Vehicle vehicle);	
}

public class EntryGate implements Gate {
	
	public EntryGate() {
		this.gateType = GateType.ENTRY_GATE;
		this.manager = new ParkingManager();
		this.registry = new ParkingFactoryRegistry();
	}
	
	@Override
	public void action(Vehicle vehicle) {
		//vehicle enters - check availability
		manager.allocate(vehicle);
		
		Parking
	}
}

public class ExitGate implements Gate {
	@Override
	public void action(Vehicle vehicle) {
	
	}
}

public class ParkingManager {
	private Map<VehicleType, Integer> parkingSpotPerVehicle;
	
	public void allocate(VehicleType vehicleType) {
		if(!containsSpot(vehicleType)) {
			throw new RuntimeException("Parking spot for vehicle type " + vehicleType + " not available");
		}
		
		Integer availableSpots = parkingSpotPerVehicle.get(vehicleType);
		parkingSpotPerVehicle.put(vehicleType, availableSpots-1);
	}
	
	private boolean containsSpot(VehicleType vehicleType) {	
		return parkingSpotPerVehicle.containsKey(vehicleType) && parkingSpotPerVehicle.get(vehicleType) > 0;
	}
	
	public void deallocate(VehicleType vehicleType) {
		Integer availableSpots = parkingSpotPerVehicle.get(vehicleType);
		parkingSpotPerVehicle.put(vehicleType, availableSpots+1);
	}
}


```
### 7. Interview Questions
