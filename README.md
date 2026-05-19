# AI Recruiter Resume Parser (n8n Workflow)

This repository contains an automated [n8n](https://n8n.io/) workflow designed to streamline the recruitment process. It automatically monitors Google Drive folders for new resumes, extracts text from the files, uses OpenAI (GPT-4o-mini) to parse candidate information, and smartly logs the extracted details into categorized Google Sheets.

## 🚀 Features

- **Automated Triggering**: Monitors specific Google Drive folders (e.g., Accounts, Digital Marketing, Customer Service) for new file uploads (`fileCreated` event).
- **PDF Extraction**: Automatically downloads and extracts text content from PDF resumes.
- **AI-Powered Parsing**: Leverages `gpt-4o-mini` to accurately extract candidate details such as:
  - Name
  - Email & Phone
  - Location
  - Highest Qualification
  - Current Role
  - Total Experience
- **Smart Routing**: Evaluates which Google Drive folder triggered the workflow and routes the data to the appropriate Google Sheet tab.
- **Automated Data Entry**: Appends the structured candidate information as a new row in Google Sheets.

## 📋 Prerequisites

To run this workflow, you will need:
- An active [n8n instance](https://n8n.io/) (Self-hosted or n8n Cloud).
- **Google Cloud Console API Credentials** for connecting [Google Drive](https://docs.n8n.io/integrations/builtin/credentials/google/) and [Google Sheets](https://docs.n8n.io/integrations/builtin/credentials/google/).
- An **OpenAI API Key** to connect the OpenAI integration.

## 🛠️ Installation & Setup

### 1. Import the Workflow
1. Download the `recruiters_resume_workflow.json` file from this repository.
2. In your n8n workspace, create a new workflow.
3. Click the three dots (menu) in the top right corner and select **Import from File**, or just copy the file contents and paste them directly into the n8n canvas.

### 2. Configure Credentials
You will need to update the credential nodes inside n8n for your own accounts:
- **Google Drive Trigger & Download Nodes**: Select or create your Google Drive OAuth2 API credential.
- **OpenAI Node**: Select or create your OpenAI API credential.
- **Google Sheets Nodes**: Select or create your Google Sheets OAuth2 API credential.

### 3. Set Up Your Google Drive Folders
1. Create dedicated folders in your Google Drive for different roles (e.g., "Accounts", "Digital Marketing", "Customer Service").
2. Update the `Trigger` nodes with the specific **Folder IDs** you created.
3. Update the `If` nodes to match your new Folder IDs so the routing logic correctly matches the triggered folder.

### 4. Set Up Your Google Sheet
1. Create a Google Sheet with the following columns:
   - `Name`
   - `Phone`
   - `Email`
   - `Location`
   - `Qualification`
   - `Current role`
   - `Experience`
2. Create separate tabs in this sheet for different roles (matching your Drive folders).
3. Update the **"Append row in sheet"** nodes to point to your specific Document ID and select the corresponding Sheet tabs.

## 🧠 How it Works

1. **Trigger Phase**: The workflow checks connected Google Drive folders every minute for newly created files.
2. **Read Phase**: When a file is found, the workflow downloads it and extracts the text content (handling PDFs natively).
3. **AI Processing Phase**: The raw text gets cleaned up and sent to an OpenAI `gpt-4o-mini` conversational model via the Message a model node. Using targeted prompt engineering, the model pulls exact profile metrics (while calculating total experience).
4. **Data Formatting**: An **Edit Fields (Set)** node parses the AI's response text into a valid JSON object.
5. **Conditional Logic (If)**: Depending on the Google Drive Origin Folder ID, the workflow branches towards a specific department's tracker.
6. **Data Storage Phase**: The parsed JSON metrics are appended as a neat row to the correct Google Sheet.

## 📝 License

This workflow is open-source. Feel free to modify and expand the nodes for your custom recruitment processes!
