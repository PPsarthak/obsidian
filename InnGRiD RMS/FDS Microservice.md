#inngrid-rms #fds #business-logic

The Casper Backend and PMS are connected via the FDS microservice
Casper is unaware of the details of a property/PMS and only deals with property-id, property-code and chain-code terms
The FDS acts as the source of truth; storing all the meta-data of the property.
It configures the property-code, chain-code and property-id for each property & manages the property as well (add/delete property)

![[InnGRiD RMS - FDS Overview.png|725]]
### Add Property Flow
Any property that will be registered in InnGRiD-RMS must be first added in FDS Microservice
Here the chain-code and property-code are configured amongst other details and a property-id is generated.
On successful configuration, the FDS internally calls Casper's Property controller and registers the property's property-id, chain-code and property-code details

> <span style="color:rgb(102, 207, 255)">TODO</span>- More brainstorming on whether room-class/room-types should be added in FDS
> Once property is added, `roomclass` must also be configured 
### Delete Property Flow
FDS receives the delete property call and calls Casper's Property controller to delete the property from `property` table
The `roomclass` should also be deleted from Casper
Once done, mark the property as de-activated in FDS and delete the property-code
If that property is the sole property in that chain, then clear the chain-code as well, or atleast delete the total-Properties count
### Room Class Configuration
FDS receives the property id and room classes to add. It stores the data in its table and call's Casper's room class service and saves the details
Currently, we DO NOT allow the request to create a new room type. So if the request contains a room type name that is not present in the DB then it throws error.

### Get BDE Data from PMS
Will use a cron job - run every day at 11.50PM
Create a controller and a service, attach a .xlsx file with controller's POST call
File contains - 
	Room Type Name
	Total Rooms Booked
	Total Rooms Cancelled
	Total Rooms Sold
	Total Revenue Generated
Here, Booked = Cancelled + Sold

Example:
Standard - 28 - 2 - 26 - 910
Deluxe - 5 - 0 - 5 - 550
Penthouse - 1-0-1-199

This data will be stored in DB at RC Level, DB Schema:
property id (request mapping)- room type id (xl file) = room class id

room class id | rooms booked | rooms cancelled | rooms sold | total revenue

---
Another cron job to fetch booking details of future date
	Booking Date
	Room Type Name
	Rooms Booked
	
Example:
02/03 - Standard - 5 Rooms booked
02/03 - Suite - 1 Room booked
03/03 - Deluxe - 1 Room booked
05/03 - Penthouse - 2 Rooms booked

Push BDE Data at RC Level to Casper - another cron jobs


**Next work**: Push BDE Data to Casper after adding in FDS, Add a cron job, fix the REST Endpoint of FDS' BDE Data