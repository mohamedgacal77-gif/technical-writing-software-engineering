A) User Manual Procedure: Create and activate a Python virtual environment, then install a package
Audience: First-semester computing student with basic computer literacy, using Windows.

Prerequisites
Before you start, make sure you have:

Python 3.8 or newer installed (check with python --version or py --version in PowerShell).

A terminal you can use (PowerShell or Command Prompt on Windows).

Internet access to download packages from PyPI.

A project folder where you want to work (for example, C:\Users\You\projects\my_app).

Steps
Open your terminal (PowerShell or Command Prompt).
Expected result: You see a command prompt ready to accept commands.

Navigate to your project folder using cd. For example:
cd C:\Users\You\projects\my_app
Expected result: The prompt shows you are inside my_app.

Create a virtual environment named .venv by running:
py -m venv .venv (or python -m venv .venv if py is not available).
Expected result: A new folder .venv appears in your project directory.

Activate the virtual environment:

In PowerShell: .venv\Scripts\Activate.ps1

In Command Prompt: .venv\Scripts\activate.bat
Expected result: Your prompt shows (.venv) at the start, indicating the environment is active.

Upgrade pip inside the virtual environment (recommended):
python -m pip install --upgrade pip
Expected result: pip updates without errors.

Install your chosen package (example: requests):
pip install requests
Expected result: pip downloads and installs requests and its dependencies; you see a “Successfully installed” message.

Verify the installation by opening Python and importing the package:
python then import requests then exit()
Expected result: No error when importing; Python returns to the prompt cleanly.

(Optional but recommended) Create a requirements.txt file to record dependencies:
pip freeze > requirements.txt
Expected result: A requirements.txt file appears listing installed packages (e.g., requests==2.32.3).

Screenshot description
Include a screenshot of your terminal after Step 4, showing:

The prompt prefixed with (.venv) to confirm activation.

The directory path pointing to your project folder.
This visually confirms the environment is active before installing packages.

Troubleshooting note (most common error)
If you get an error like “cannot be loaded because running scripts is disabled on this system” in PowerShell when activating:

This is due to PowerShell’s execution policy.

Fix: Run Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser, then try activating again.

Alternatively, use Command Prompt (cmd) instead of PowerShell for this step.

B) API Reference Entry: Create a new task
Endpoint
Method: POST

Path: /projects/{projectId}/tasks

Description
Creates a new task in the specified project. The caller must be authenticated. The new task is returned with server-assigned fields such as its unique ID and creation timestamp.

Authentication
Header: Authorization: Bearer <access_token> (required)
Use an OAuth 2.0 or JWT access token with scope/permission to create tasks in the project.

Path parameters
projectId (string, required)
The unique identifier of the project in which to create the task.

Request body
Content-Type: application/json

Field	Type	Required	Description
title	string	Yes	Short title of the task (e.g., “Write login page”).
description| string	No	Longer description of what the task involves.	
assigneeId	string	No	User ID of the person assigned to the task.
dueDate	string	No	Due date in YYYY-MM-DD format (e.g., 2026-09-20).
priority	string	No	Priority level: "low", "medium", or "high". Defaults to "medium" if omitted.
Request headers
Authorization: Bearer <access_token> (required)

Content-Type: application/json (required)

Example request
text
POST /projects/proj_123/tasks HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
Content-Type: application/json

{
  "title": "Write login page",
  "description": "Implement user login UI and validation.",
  "assigneeId": "usr_456",
  "dueDate": "2026-09-20",
  "priority": "high"
}
Response codes
201 Created
The task was successfully created. The response body contains the new task object.

400 Bad Request
The request is invalid (for example, missing required title, invalid dueDate format, or priority not one of the allowed values).

401 Unauthorized
No valid authentication token was provided, or the token is expired/invalid.

403 Forbidden
The authenticated user does not have permission to create tasks in this project.

404 Not Found
The specified projectId does not exist.

409 Conflict
A business-rule conflict occurred (for example, the project is archived and no longer accepts new tasks).

500 Internal Server Error
An unexpected server error occurred; retry later or contact support.

Example success response (201)
json
{
  "id": "tsk_789",
  "projectId": "proj_123",
  "title": "Write login page",
  "description": "Implement user login UI and validation.",
  "assigneeId": "usr_456",
  "dueDate": "2026-09-20",
  "priority": "high",
  "status": "todo",
  "createdAt": "2026-09-12T17:30:00Z",
  "updatedAt": "2026-09-12T17:30:00Z"
}
Notes
status is set by the server (commonly "todo" for new tasks).

createdAt and updatedAt use ISO 8601 format in UTC.

If priority is omitted, the server defaults it to "medium".
