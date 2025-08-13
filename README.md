# N8n Workflow Backup Automation

This workflow automatically backs up your n8n workflows to a GitHub repository, creating new files for new workflows and updating existing ones with a timestamped commit history.

## ⚙️ Workflow Overview
! [Workflow Visualization] (https://github.com/ChetanLandge/n8n-workflows/blob/main/Overview.png)

### Key Features:
- Automated GitHub backups with timestamp tracking
- New workflow detection and creation
- Existing workflow updates with version history
- File naming standardization (spaces → hyphens, lowercase)

 
 🔗 Node Connections
mermaid
graph LR
A[Manual Trigger] --> B [Set Date]
B --> C [Format Date]
C --> D [Set Commit Date]
D --> E [List GitHub Files]
E --> F [Combine Filenames]
F --> G [Retrieve n8n Workflows]
G --> H [JSON to Binary]
H --> I [Split Workflows]
I --> J {File Exists?}
J --> |Yes| K [Update GitHub File]
J --> |No| L [Upload GitHub File]
K --> I
L --> I




📦 Nodes Breakdown
1. Date Handling
Set Date: Captures current timestamp

Format Date: Converts to dd-MM-yyyy/H:mm format (e.g. 13-08-2025/14:30)

Set Commit Date: Stores formatted date as commitDate

2. GitHub Operations
List Files: Fetches all files from your_Github/your_Github_repo

Combine Filenames: Aggregates filenames for comparison

Update/Upload File: Uses binary data with commit message:
backup-{{commitDate}} (e.g. backup-13-08-2025/14:30)

3. n8n Workflow Processing
Retrieve Workflows: Fetches all workflows via n8n API

JSON to Binary: Converts workflows to files with naming:
{{workflow-name}}.json → example-workflow.json

Split Items: Processes workflows individually

4. Conditional Logic
File Check: Compares workflow filename against repository contents

Branching:

Existing file → Update

New file → Upload



🔐 Required Credentials
GitHub OAuth2 (githubOAuth2Api)

Scope: repo (full repository control)

n8n API (n8nApi)

Requires access to local n8n instance



⚠️ Special Notes
GitHub repository must pre-exist

File paths use strict naming conventions:

js
workflowName.replace(/\s+/g, '-').toLowerCase() + '.json'
Commit messages include backup timestamp for version tracking

The loopback connection allows batch processing of all workflows

   


🌟 Benefits
Version Control: Track workflow changes via GitHub history

Disaster Recovery: Offsite backup of n8n configurations

Automated Maintenance: Scheduled backups via n8n scheduler

Consistency: Standardized naming across all workflows




