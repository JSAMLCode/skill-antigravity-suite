---
name: netlify-deploy
description: A skill to deploy the local project to Netlify using the netlify-mcp-server tools.
---

# Netlify Deploy Skill

Use this skill when the user wants to deploy their web application to Netlify.

## Instructions

### 1. Preparation
- **Identify Build Directory**: Locate the directory containing the production-ready files (e.g., `dist`, `build`, `out`, or the project root for simple HTML projects). 
- **Identify Site ID**: Ask the user for their Netlify `siteId`. 
  - If the user doesn't have a `siteId`, inform them that they can use the Netlify CLI (`netlify link`) or the Netlify dashboard to find it. 
  - NEVER assume the user wants a new site without explicit confirmation.

### 2. Implementation Rules
- Always call `mcp_netlify_netlify-coding-rules` before performing any Netlify-related operation to ensure compliance with current standards.

### 3. Deploying the Site
Use the `mcp_netlify_netlify-deploy-services-updater` tool to perform the deployment.
- **Tool**: `mcp_netlify_netlify-deploy-services-updater`
- **Arguments**:
  - `selectSchema`:
    - `operation`: "deploy-site"
    - `params`:
      - `deployDirectory`: The absolute path to the directory to be deployed.
      - `siteId`: The unique identifier for the Netlify site.

### 4. Verification
After the tool reports success, provide the user with the deployment status or the site URL if available in the output.

## Constraints
- **Absolute Paths**: Ensure the `deployDirectory` provided is an absolute path.
- **Coding Rules**: You MUST call `netlify-coding-rules` first.
- **Confirm Destructive Actions**: If a site already exists and the user is overwriting a production deploy, briefly mention that you are deploying to the specified `siteId`.

## Examples

### Deploying a React project
1. **Tool**: `mcp_netlify_netlify-coding-rules`
   - Params: `{"creationType": "serverless"}`
2. **Tool**: `mcp_netlify_netlify-deploy-services-updater`
   - Params:
     ```json
     {
       "selectSchema": {
         "operation": "deploy-site",
         "params": {
           "deployDirectory": "/absolute/path/to/project/dist",
           "siteId": "your-netlify-site-id"
         }
       }
     }
     ```
