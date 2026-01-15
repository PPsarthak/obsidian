#invest-graph 

Let me put this at the top so that I can always pickup the state of mind in which I wrote this and implemented it
> Deleting a SIP is very very different from deleting a OTI

But let's discuss first - *What can deleting an investment mean?*
- Deleting can mean that the user might have added wrong details and hence it is a wrong entry and hence it MUST be deleted without any traces left (If the entry is wrong, traces are not important)

However, for SIPs, deleting could also mean that the user wants to stop investing further
He will still hold the units purchased and can perhaps sell when he wants to in the future
This can't be done for OTIs because the user will invest in OTIs only once hence there is nothing like "stop investing" for OTIs
> This is the main difference between deleting SIP vs deleting OTI

*How to stop investing in SIPs then?*
For now, let's add a PUT Endpoint to stop investing in SIPs
**Deleting any investment now means that we are deleting the entirety of the investment
We will though add an important check - No units should be sold before deleting
If the user has honestly add the investment entry as mistake, then he cannot sell it**

Added - 
PUT Endpoint to pause/resume SIPs 
Updated DEL Endpoint logic for SIPs to delete the sip config and all the installments
DEL Endpoint logic for OTI remains same-  delete the OTI