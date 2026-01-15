#invest-graph 

The plan is to create a table that stores the actions done by the user.
If possible, we are planning to add a *rollback-type* feature as well in future which allows the user t*o undo a non-critical action*
For example, this rollback will not allow user to buy back the units he has sold, instead it will be for small changes.
### Scheme of the Audit Log Table
|Column Name|Type|Description|
|---|---|---|
|`id`|`BIGINT` (PK, auto-increment)|Unique ID for each audit entry|
|`user_id`|`BIGINT`|The ID of the user who performed the action|
|`table_name`|`VARCHAR(100)`|Name of the table where the change occurred|
|`entity_id`|`VARCHAR(255)`|The primary key(s) of the affected row(s); use JSON or delimited string for composite keys|
|`action`|`ENUM('CREATED', 'UPDATED', 'DELETED')`|Type of action performed|
|`change_summary`|`JSON` or `TEXT`|Optional. A diff or snapshot of the change, useful for rollback or auditing|
|`created_at`|`TIMESTAMP`|Time at which the audit entry was recorded|
> Add an index on `user_id`, `table_name`, `entity_id` for faster querying.
#### Additional columns to add to include rollback feature
| Column Name          | Type          | Description                                                                                                                                                                                                                                                             |
| -------------------- | ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `old_value`          | `JSON`        | The state of the entity before the change (very useful for rollback)                                                                                                                                                                                                    |
| `new_value`          | `JSON`        | The state of the entity after the change                                                                                                                                                                                                                                |
| `rollback_supported` | `BOOLEAN`     | Mark whether this change can be rolled back                                                                                                                                                                                                                             |
| `transaction_id`     | `VARCHAR(64)` | Useful if you want to group multiple changes into a single logical operation (e.g., batch updates). For this, we can create a service method that generates a Id for each request received and then set that id for all columns that have been added in audit log table |

### Endpoints to Audit

|  **Operation**   |  Fields we have prior execution   | Fields we have post execution |
| :--------------: | :-------------------------------: | :---------------------------: |
|     Add OTI      |      action, table, user-id       |           entity-id           |
|    Delete OTI    | action, table, user-id, entity-id |                               |
|     Add SIP      |      action, table, user-id       |           entity-id           |
| Pause/Resume SIP | action, table, user-id, entity-id |                               |
|    Delete SIP    | action, table, user-id, entity-id |                               |
|   Sell SIP/OTI   | action, table, user-id, entity-id |                               |
