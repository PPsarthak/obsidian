#inngrid-rms 

let's discuss the IP microservice. 
Here will host different forecasting models that will predict the each room class's price for next business day. 
I am thinking about the inputs first. The model will deal with room class ids and not property ids. This avoids dependency on property
Now input data will be the past 90-365 days of bookings data. This includes how many rooms were booked, of which how many rooms got cancelled, how many rooms were thus finally sold (remember, rooms booked = rooms cancelled + rooms sold in current context)
We will also provide total revenue generated for each room class, so if 10 rooms were sold and total revenue = 900, that means 90 for 1 room. This 90 value is not stored in database, instead direct total revenue generated is stored.
We will provide the future bookings data...like for tomorrow how many rooms are already booked? So if not many rooms are booked in the near future, maybe a cheaper price should be set, else if rooms are booked, a higher price can be set

**Questions:**
What are the different input parameters that we have?
PSP param - 90 -365 days
PSP Model to use 
Bookings data
Which different models can we think of here for the job? I am planning on designing and working on atleast 2-3 models
