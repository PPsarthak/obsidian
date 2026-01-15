#invest-graph
### API Endpoints

| Purpose                                 | Endpoint                                        | Method |
| --------------------------------------- | ----------------------------------------------- | ------ |
| Get user info                           | `/user/{userId}`                                | GET    |
| Add user                                | `/user`                                         | POST   |
| Delete user                             | `/user/{userId}`                                | DELETE |
| Get user's portfolio                    | `/user/{userId}/portfolio`                      | GET    |
| Get user's units                        | `/user/{userId}/portfolio/units/{configId}`     | GET    |
| Get one-time investment                 | `/user/{userId}/investments`                    | GET    |
| Get one-time investments for a user     | `/user/{userId}/investments/{investmentId}`     | GET    |
| Add one-time investment                 | `/user/{userId}/investments`                    | POST   |
| Delete one-time investment              | `/user/{userId}/investments`                    | DELETE |
| Delete one-time investments for a user  | `/user/{userId}/investments/{investmentId}`     | DELETE |
| Get all SIP configurations for a user   | `/user/{userId}/sip`                            | GET    |
| Get a SIP configurations                | `/user/{userId}/sip/{configId}`                 | GET    |
| Add SIP configuration                   | `/user/{userId}/sip`                            | POST   |
| Pause SIP                               | `/user/{userId}/sip/{configId}`                 | PUT    |
| Resume SIP                              | `/user/{userId}/sip/{configId}`                 | PUT    |
| Delete a SIP configuration              | `/user/{userId}/sip/{configId}`                 | DELETE |
| Delete all SIP configuration for a user | `/user/{userId}/sip`                            | DELETE |
| Get latest NAV for a scheme             | `/schemes/{schemeCode}/nav/latest`              | GET    |
| Get SIP units for a given configuration | `/sips/{sipId}/portfolio/units`                 | GET    |
| Add scheme for tracking                 | `/schemes`                                      | POST   |
| Add NAV data for all schemes            | `/schemes/populate`                             | POST   |
| Sell Monthly SIP                        | `/user/{userId}/sell/sip/{sipConfigId}`         | POST   |
| Sell One Time Investment                | `/user/{userId}/sell/investment/{investmentId}` | POST   |

---
###### APIs
FinnHub.io  API Key - d0jh4m9r01ql09hsej1gd0jh4m9r01ql09hsej20
**Yahoo works very well...**
- `currency`: INR — the values are in Indian Rupees.
- `symbol`: `^BSESN` — Sensex index symbol.
- `regularMarketPrice`: `82330.59` — Current price (live or last close).
- `regularMarketDayHigh`: Highest value of the day so far.
- `regularMarketDayLow`: Lowest value of the day.
- `chartPreviousClose`: Close value from the previous trading day.
- `timestamp`:  [1747389650]
	- ``` 
	  Instant.ofEpochSecond(1747389650)
       .atZone(ZoneId.of("Asia/Kolkata"))
       .toLocalDateTime(); 
       ```

NAV File - https://www.amfiindia.com/spages/NAVAll.txt
Historical NAV - https://portal.amfiindia.com/DownloadNAVHistoryReport_Po.aspx?tp=1&frmdt=01-Jan-2025&todt=03-Jan-2025
### ChatGPT:
Key Features You Could Include:
- Display current NAVs of all your schemes.
- See underlying stock holdings of each mutual fund scheme.
- Group and display holdings under each scheme with easy navigation.
- Schedule a background job with Spring Scheduler to auto-refresh.
- Notify you if NAV drops or rises beyond a threshold.
- Login/logout (Spring Security).
- Add/update/remove schemes you've invested in.
- Add selling mechanism and track profit/loss generated
- Track Annual Growth Rate on invested schemes

#### TODOs
- [ ] Delete User Endpoint needs to be fixed - now all investments are deleted hence soft deletion for user does not makes sense. We will completely delete the user instead of setting user as inactive
- [x] Add auditing
- [ ] Add JUnit test cases
- [ ] Add CI/CD Pipeline 
---
- [ ] Add message in all Response Status Exceptions because in vaadin these messages are displayed on UI
- [ ] Add a GET Endpoint to obtain units in OTIs. We currently only have GET for SIPs and not for OTIs (in Portfolio Controller)
- [ ] Add a service method in SIP Config to fetch the next installment date
- [x] Create a postman collection that can quickly add some dummy data 
- [ ] SIP Installment Deduction Cron Job when fails there is no retry mechanism, how can we handle retries there
- [ ] Perhaps change the table names from - 
	- `user_one_time_investments` to `onetime_investments`
	- `user_monthly_sip_config` to `monthly_sip_config` (and eventually to `sip_config`)
	- `user_monthly_sip_installments` to `monthly_sip_installments`  (and eventually to `sip_installments`)
	- `nav_details` to `schemes` (because we have endpoints with "scheme" in it that use this table)