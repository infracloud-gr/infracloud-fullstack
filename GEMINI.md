# Log activity feature


At backend when a new feature is created we need to add log activity for that feature.

- Add log activity for create, update, delete operations
- Add log activity for user operations

# Log activity entity

- id
- created_at
- created_by
- action
- module
- description

Using aspectj to log all CRUD operations in backend. the desription must be clearly about which is created, deleted or updated

In fe will show list of logs and filter action, module