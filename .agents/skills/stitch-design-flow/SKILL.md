---
name: stitch-design-flow
description: A skill to design frontends using Stitch and integrate the workflow with GitHub for version control and Netlify for deployment.
---

# Stitch Design Flow Skill

Use this skill when the user wants to design UI screens using Stitch and ensure they are properly stored in GitHub and deployed to Netlify.

## Instructions

### 1. Phase 1: UI Design with Stitch
- **Project Setup**: If no Stitch project exists, use `mcp_Stitch_create_project`.
- **Screen Generation**: Use `mcp_Stitch_generate_screen_from_text` to create initial designs based on the user's description.
- **Iteration**: Use `mcp_Stitch_edit_screens` or `mcp_Stitch_generate_variants` to refine the design until the user is satisfied.
- **Retrieval**: Use `mcp_Stitch_get_screen` to get the final metadata and potentially the code/structure.

### 2. Phase 2: GitHub Integration
Once the design is ready and code is generated (or if you are building the implementation based on the design):
- **Initialize Repo**: Use the `github-upload` skill logic.
- **Push Files**: Ensure the generated frontend code is committed to GitHub using `mcp_github-mcp-server_push_files`.
- **Structure**: Organize the repository following best practices (e.g., `src/`, `public/`, `index.html`).

### 3. Phase 3: Netlify Deployment
- **Deployment**: Use the `netlify-deploy` skill logic.
- **Workflow**: 
  1. Call `mcp_netlify_netlify-coding-rules`.
  2. Deploy the build directory to the user's `siteId` using `mcp_netlify_netlify-deploy-services-updater`.

### 4. Full Pipeline Constraint
- You MUST ensure the workflow follows this order: **Design (Stitch) -> Version Control (GitHub) -> Live (Netlify)**.
- Always ask for the GitHub repository name and the Netlify `siteId` before starting the implementation phase.

## Examples

### Complete Workflow Request
1. **User**: "Design a landing page for my bakery and host it."
2. **AgentAction**: 
   - Create Stitch project: `mcp_Stitch_create_project`.
   - Generate Screen: `mcp_Stitch_generate_screen_from_text`.
   - After approval, write code to local files.
   - Use `github-upload` skill to push to a new repo `bakery-site`.
   - Use `netlify-deploy` skill to host it on Netlify.

## Best Practices
- **Stitch ID Handling**: Keep track of the `projectId` and `screenId` throughout the session.
- **Atomic Commits**: If designs change significantly, make a fresh commit to GitHub before redeploying to Netlify.
- **Wow Factor**: Use the `generate_image` tool to supplement Stitch designs with premium assets if needed.
