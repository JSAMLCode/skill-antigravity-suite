---
name: github-upload
description: A skill to upload local code to a new or existing GitHub repository using the github-mcp-server tools.
---

# GitHub Upload Skill

Use this skill when the user wants to push their local project code to GitHub.

## Instructions

### 1. Preparation
- **Identify the Project Root**: The root directory of the codebase to be uploaded.
- **Identify Target Repository**: Ask the user for the repository name and owner. If it doesn't exist, you will need to create it.
- **Ignore Sensitive Files**: DO NOT upload `.env`, `node_modules`, `.git`, or other sensitive/large files. Check for a `.gitignore` file and respect its rules if possible.

### 2. Creating the Repository (if needed)
If the repository does not exist on GitHub:
- Call `mcp_github-mcp-server_create_repository` with the desired `name` and optional `description`.
- Note the returned repository URL and name.

### 3. Pushing Files
Since you cannot run `git` commands directly to "push" a local commit in one go easily without a terminal, use the `push_files` tool from the GitHub MCP:
- Call `mcp_github-mcp-server_push_files`.
- **Arguments**:
  - `owner`: The GitHub username or organization.
  - `repo`: The repository name.
  - `branch`: Usually `main`.
  - `message`: A descriptive commit message (e.g., "Initial commit from Antigravity").
  - `files`: An array of objects, each containing `path` (relative to repo root) and `content`.

**Note**: To handle many files, you may need to slice the file list or perform multiple `push_files` calls if the total payload is too large, although usually, a single call is preferred for an initial commit.

### 4. Constraints
- **Verification**: After pushing, verify the files are there by calling `mcp_github-mcp-server_get_file_contents` or providing the user with the repository link.
- **Security**: Never push secrets. If you see an API key or password in the code, warn the user before uploading.

## Examples

### Creating and Pushing Initial Files
1. **Tool**: `mcp_github-mcp-server_create_repository`
   - Params: `{"name": "my-cool-project"}`
2. **Tool**: `mcp_github-mcp-server_push_files`
   - Params: 
     ```json
     {
       "owner": "username",
       "repo": "my-cool-project",
       "branch": "main",
       "message": "Initialize project",
       "files": [
         {"path": "index.js", "content": "..."},
         {"path": "package.json", "content": "..."}
       ]
     }
     ```
