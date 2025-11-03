# Pipeline Showcase – GoCD “Pipeline as Code” Composition Example

## Overview  
This repository demonstrates a **composed pipeline architecture** built with GoCD’s “Pipelines as Code” capability. The aim is to illustrate how multiple pipelines can be structured and orchestrated to form **fan-in** and **fan-out** workflows, and how GoCD visualises those relationships via its Value Stream Map (VSM).

## Why this demo matters  
- GoCD supports defining pipelines via version-controlled configuration files (YAML/JSON) through its config-repo feature.  
- In larger delivery flows, independent pipelines often merge (“fan-in”) or branch out (“fan-out”) to handle micro-services, parallel feature streams, or downstream deployments.  
- The built-in Value Stream Map in GoCD gives a **clear, intuitive view** of how these pipelines inter-connect—something many CI/CD tools don’t provide as cleanly.  

## Key Concepts  
- **Pipeline as Code**: pipeline definitions live alongside source code (or in a dedicated config repo), tracked in version control, enabling easier auditing, branching and review.  
- **Fan-out scenario**: one upstream pipeline triggers multiple downstream pipelines (for example, once a feature branch build completes, multiple services start their own builds).  
- **Fan-in scenario**: multiple upstream pipelines converge into one downstream pipeline (for example, after several service build pipelines complete, a combined deployment pipeline picks up their artifacts).
- **[Value Stream Map (VSM)](https://docs.gocd.org/current/navigation/value_stream_map.html)**: GoCD’s dashboard renders pipeline materials and dependencies in a graph-style view, letting you trace full end-to-end flows visually.  

## How to Use This Demo  
1. Fork or clone this repository.  
2. In your GoCD instance:
   - Go to **New Pipeline → Use Pipelines as Code**.  
   - Go to **Step 3. Connect Your SCM Repo to This GoCD Server**, config your codebase url, set **Name this Connection**
   - Go to **Step 4. Add Your Config File to Your SCM Repo** and click **Scan Repository for Configs**
3. Once GoCD polls the repository, you should see the pipelines defined here appear in the dashboard.  
4. Un-pause the pipelines, trigger or commit to see the flow execute.  
5. On the GoCD dashboard, click on “Value Stream Map” for a pipeline of interest to visualise how upstream and downstream pipelines link.  

## What You Will See  
- A **source pipeline** (for example “Codebase”) representing code check-in and build.  
- **Downstream pipelines** that depend on the source build (fan-out).  
- A **converging pipeline** that aggregates artifacts from multiple upstream pipelines (fan-in).  
- And finally a **deployment pipeline** that triggers once the converging pipeline and other dependencies complete.  
- In GoCD’s VSM you can trace these flows end-to-end, showing branching and merging of pipelines.

## Why Use This Pattern in the Real World  
- It allows a **clear separation of concerns**: each component (service or module) can have its own pipeline.  
- It **scales**: you can add new components (services) that branch out or join in without rewriting a monolithic pipeline.  
- The VSM gives valuable visibility: you can see what’s blocking, what ran when, and how dependencies align.  
- Version-controlled pipeline configs = reproducibility, reviewability, and “infrastructure as code”.

## Customisation & Extending  
Feel free to adapt this demo to your context:  
- Rename pipelines/groups to match your team or service names.  
- Add more complex stages or jobs (e.g., lint, test, deploy, approve) as needed.  
- Add artifact/fetch tasks to actually model real upstream/downstream dependencies.  
- If your organisation uses environments (Dev/QA/Prod), you can model them here.  
- You might include dashboard or approval stages to reflect gated workflows.

## Notes & Considerations  
- This demo uses simple `exec: echo` tasks for illustration—replace them with your actual build/test commands.  
- Ensure the plugin version for YAML config is compatible with your GoCD server version.  
- If you introduce cross-repo pipeline dependencies, make sure you configure appropriate config-repo rules in GoCD.  
- Keep pipeline names unique and meaningful to avoid confusion in the VSM.

## Licence & Contribution  
This repository is open for adaptation and experimentation. Feel free to fork it, modify it to suit your pipelines, and contribute improvements back via pull requests.