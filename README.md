# Release Tracker

## 1. Introduction

Release Tracker is a simple, browser-based tool for visualizing deployments of multiple microservices in a single release.  
Each row represents one service and its deployment stages. The tool also lets you define and visualize **dependencies** between services, so you can see which services should be deployed first and which ones are safe to deploy based on the state of their dependencies.

---

## 2. How to use

### Opening the tool

1. Save the HTML file (for example: `release-tracking.html`).
2. Open it in any modern web browser.

Alternatively, you can use the copy hosted in GitHub pages that always has the latest version: https://hali.github.io/test-management/release-tracking.html

### Adding services and dependencies

- Use the **“Add service”** input and **➕** button (or press **Enter** in that input) to add a new service row.
- In each row:
  - **Service**: type the name of your microservice.
  - **Depends on**: list other services this one depends on, separated by commas.  
    - Example: `auth-service, billing-service`  
    - A dependency token such as `auth-service` will match services named:
      - `auth-service`
      - `auth-service (attempt 2)`
      - `test auth-service`  
      but not `auth-service-gui`.

### Stages

Each service has checkboxes for these stages:

- RC cut  
- STG deploy  
- STG signed off  
- PRD deploy  
- Done  

Tick the boxes as the service progresses through environments. The **PRD deploy** checkbox is especially important for dependency status and coloring.

### Sorting by deployment order

- Use the **⬇️ Sort** button to reorder services:
  - Services are first sorted **alphabetically** by name.
  - Then, where possible, services are ordered so that **dependencies appear before the services that depend on them**.

This helps you see a reasonable deployment sequence based on your declared dependencies.

### Highlights and status dot

For each service, the tool gives two visual cues:

1. **Row highlight**  
   Services that other services depend on are highlighted:
   - **Orange**: at least one dependent service exists, and this service is **not yet in PRD**.
   - **Green**: at least one dependent service exists, and this service **is in PRD**.

2. **Status dot (leftmost column)**  
   A small red or green dot indicates whether it is safe to deploy this service to production from a dependency point of view:
   - **Green** if:
     - The service has **no dependencies**, or  
     - All dependencies that appear in the table are **already in PRD**.
   - **Red** otherwise (some dependency in the table is not yet in PRD).

If a dependency name does not match any service in the table, it is treated as external and does not prevent the dot from being green.

### Adding links

Each row has an **Actions** column:

- **🔗 Edit link**: lets you store a URL associated with this service (for example, a CI/CD pipeline run, release plan, or service dashboard).
- **↗ Open**: opens the stored link in a new browser tab (enabled once a link is set).

These links are stored in the in-browser state and included when you export to a file.

---

## 3. Limitations

- The tool is **fully client-side**. It does **not** connect to any external systems, APIs, or CI/CD tools. It only reflects the data you enter manually.
- The tool does **not** automatically save your data. If you close or refresh the page, changes are lost unless you explicitly export them.

You can manage persistence via the built-in file actions:

- **💾 Save**: exports the current state (services, stages, dependencies, links, and notes) into a plain text file on your machine.
- **📂 Load**: imports a previously saved text file and restores the tracker to that state.

Use these options to keep a history of your releases or to resume work later.
