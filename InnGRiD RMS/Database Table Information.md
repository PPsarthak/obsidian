### Property Table
The main table that stores the data for each property

### Room Type Table
Contains names of different types of room types in hotel industry - Suite, Deluxe, etc. Common amongst all "properties"

### Room Class Table
Combination of (Property Id, Room Type Id)
Casper addresses almost all data at Room Class Level
RC Id is auto-incremented PK of this table

Pricing → (PRICED: Apply the rate rule and push output of IP as decision
		NON_PRICED: DO *NOT* apply rate rule and push output of IP decision
		OFFSET: Apply rate rule and add the offset)
		So all in all, hierarchy is OFFSET > PRICED > NON_PRICED

> The above 3 tables have 2 cols - 'createdOn' and 'updatedOn'
### PMS Inv Type Table
Stores the BDE Data of PMS Table 

| Room Class Id | Occupancy Date | Rooms Sold | Room Rate |
| ------------- | -------------- | ---------- | --------- |
| 1             | 01-01-2025     | 11         | 100.00    |

### Rate Rule Table
Applicable only for PRICED and OFFSET based RC 
Contains rules that affect the forecasted decisions
##### Example Scenarios:
- **Occupancy-based Rule**: If `OccupancyPercentage > 80`, increase price by 20%.
- **Last-Minute Booking Rule**: If `BookingLeadTime < 1 day`, increase price by 15%.
- **Weekday Discount Rule**: If `DayOfWeek = Monday`, decrease price by 5%.

### 