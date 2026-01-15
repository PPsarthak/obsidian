#invest-graph
### Current Endpoints

| Purpose   | Endpoint         | Method                  |
| --------- | ---------------- | ----------------------- |
| GET User  | `/user/{userId}` | Get user information    |
| POST User | `/user`          | Add user information    |
| DEL User  | `/user/{userId}` | Delete user information |

#### GET User -  /user/{userId}
Return basic user info from `users` table

#### POST User - /user 
Add the User 

#### DEL User - /user/{userId}
Set the user as inactive + add a job later to delete inactive user

> [!NOTE] Why soft delete is not sufficient?
> Because, if a user is soft deleted, the user name and email are still stored in database. Hence, if the user then re-registers, the unique constraint for name and email fails and hence user cannot be re-registered

