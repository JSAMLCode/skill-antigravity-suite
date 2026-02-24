---
name: supabase-db-manager
description: A skill to create, configure, and deploy databases and tables in Supabase using the supabase-mcp-server tools.
---

# Supabase Database Manager Skill

Use this skill when the user wants to set up a new project, create tables, or manage their database schema in Supabase.

## Instructions

### 1. Project Initialization
- **Organization Selection**: Always call `mcp_supabase-mcp-server_list_organizations` first to allow the user to choose where to create the project.
- **Cost Confirmation**: Before creating a project or branch, you MUST:
  1. Call `mcp_supabase-mcp-server_get_cost`.
  2. Present the cost to the user.
  3. Call `mcp_supabase-mcp-server_confirm_cost` to get a `confirm_cost_id`.
- **Project Creation**: Use `mcp_supabase-mcp-server_create_project` with the `confirm_cost_id`. Provide the project ID to the user once it's ready (check status with `get_project`).

### 2. Schema and Table Management
- **SQL Execution**: Use `mcp_supabase-mcp-server_apply_migration` for DDL operations (creating tables, indexes, RLS policies).
- **CRUD Operations**: Use `mcp_supabase-mcp-server_execute_sql` for running raw queries or inserting data for testing.
- **Discovery**: Use `mcp_supabase-mcp-server_list_tables` and `mcp_supabase-mcp-server_list_migrations` to understand the current state of a project.

### 3. Verification and Security
- **Security Check**: After creating tables, call `mcp_supabase-mcp-server_get_advisors` with `type="security"` to check for missing RLS policies or vulnerabilities.
- **Types Generation**: Once the schema is stable, call `mcp_supabase-mcp-server_generate_typescript_types` so the user can use them in their frontend.

## Constraints
- **Mandatory Cost Flow**: Never skip the `get_cost` -> `confirm_cost` flow for billable actions.
- **Data Safety**: Do not hardcode generated IDs in data migrations.
- **RLS Enforcement**: Always recommend enabling Row Level Security (RLS) for public tables.

## Examples

### Creating a "profiles" table
1. **Tool**: `mcp_supabase-mcp-server_apply_migration`
   - Params:
     ```json
     {
       "project_id": "your-project-ref",
       "name": "create_profiles_table",
       "query": "CREATE TABLE profiles (id uuid REFERENCES auth.users PRIMARY KEY, username text UNIQUE);"
     }
     ```
2. **Tool**: `mcp_supabase-mcp-server_get_advisors`
   - Params: `{"project_id": "your-project-ref", "type": "security"}`
