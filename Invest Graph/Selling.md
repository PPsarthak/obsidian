#invest-graph

### Logic/Flow
Create a controller with 
POST - /user/{`userId`}/sell/sip/{`sipConfigId`} - To sell monthly SIP
POST - /user/{`userId`}/sell/investment/{`investmentId`} - To sell one time investments
Both endpoints will contain a request param variable - `unitsToSell` 

#### Selling monthly SIP
Say that the user has invested 1000 Rs over 7 months in a SIP and has obtained 64, 62, 65, 73, 68, 66, 65 units
Now the user is selling 150 units. According to the FIFO logic, the units that are bought first are sold first.
Hence, 64 then 62 and then 24 (of the 73) units should be sold to sell a total of 150 units.

When the user hits this endpoint, we will validate 
- the user exists
- the sip config is active
- the units that he has requested exists (can't sell more than what he has)

We are going to add 2 columns in `sip_installments`, which are `unitsSold` and `redeemed`
The `unitsSold` stores the units that are sold from that installment. When `unitsSold` = `unitsAllocated`, we will mark `redeemed` as true meaning, all the units from that sip installment are sold

Once done, we have 2 tables - 
1. to store transactions meta data     `sell_transactions`
2. to store transaction breakdown    `sell_transaction_breakdown`

We will store userId, sipConfigId, units to sell, current nav, total amount received, profit/loss, sell date (LocalDateTime) in 1st table.

Then we start processing each installment. We will fetch all the non-redeemed installments from `sip_installments` and start calculating the units to be sold. As we are calculating, we will keep updating the `sip_installments` table.
When we sell each installment, we will calculate profit/loss and store it in table 2 along with `installmentId`, units sold, purchase nav, sell nav. We will have a FK - `transactionId` from table 1

Once done, we will sum up the profit/loss from installments and add it in table 1

#### Selling One Time Investments
This is a lot easier than selling monthly sips.

Just like in `sip_installments` we will add a 2 columns in `user_one_time_investments` - again to facilitate partial selling of units.



Here we have all the data in `user_one_time_investments` that stores the `initialNav`, `units` and `amountInvested`
We will use the same 2 tables as above -  `sell_transactions` and  `sell_transaction_breakdown`

We will make an entry in `sell_transactions` for the One Time Investment
Against that `transactionId` we will store only 1 entry in `sell_transaction_breakdown` 
