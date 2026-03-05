# Agents.md file for organizer-legacy Next.JS app

## Development instructions

### Next.JS and React
- Typescript
- Use latest best practices

### Authentication
- Application authenticates with Azure Entra directory using MSAL libraries
- Application has no public/anonymous views, redirect-type auth-flow is triggered automatically if there is no active authentication
- Entra settings:
   * Client id: 1eef30df-4081-448a-91b0-d0804aa0db9d
   * Tenant id: 91165598-7831-46bb-9f62-20c6b4ca2348
   * Scope: api://1eef30df-4081-448a-91b0-d0804aa0db9d/organizer

### Database & storage
- Azure CosmosDB and Blob-storage

### Backend requests
- Data-fetching is primarily managed with react-query

### State-management
- Use Zustand to manage global or complex/shared local states

### Localization
- Primary UI language is Finnish
- Use d.m.yyyy for date and h:mm:ss for time
- Use dayjs for date/time handling

### UI components
- Fully utilize MUI v7 components, icons and styles where ever possible
- For data-grids use AG-Grid React community edition
- Create stylish professional looking views and components
- Keep consistent style and ui conventions over all views and components 

### Forms
- Forms are implemented with react-hook-form
- Validation rules are implemented with Zod
- Use MUI input components with forms

## Database
- Database is multitenant where data is partitioned by tenants id
- Event data contains ids which refer to items in tenant-document
- If auth-user has role (MSAL) "Role.Admin" they can access all tenants
- Otherwise auth-user had access to tenants which have a user with matching email

### Tenants
- Tenant id, name, roles, groups, statuses, priorities, types, suppliers, resources etc
- Partition key /id
- [Example tenant-document](./examples/tenants.json)

### Users
- User name, tenant, roles, groups, email, supplier, is_active etc
- Partition key /tenant
- [Example users-document](./examples/users.json)

### Events
- Events (main data with references to tenant items)
- Partition key /tenant
- [Example events-document](./examples/events.json)

### Logs
- Log-entries
- Partition key /tenant
- [Example logs-document](./examples/events.json)

## Application structure
- No public views
- First iteration has read-only browse views to existing data
- /: main/home page lists all tenants accessible to user for select
- /t/{tenantdId}: root for tenant view, list and filter events
    - events/{eventId}: view event details
    - logs: browse logs
    - users: browse users
- /admin: global admin features

### Tenant store
- Implement zustand store to hold tenant-document when entering the tenant-route
- Implement store methods for easy access to lookups referenced in event data

## Deployment

### Azure
- Deployed as Azure Static Webapp
- Cosmos database (ask for connection-string)
- Blob-storage account (ask for connection-string)
