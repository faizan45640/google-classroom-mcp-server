[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/faizan45640-google-classroom-mcp-server-badge.png)](https://mseep.ai/app/faizan45640-google-classroom-mcp-server)

# Google Classroom MCP Server
[![smithery badge](https://smithery.ai/badge/@faizan45640/google-classroom-mcp-server)](https://smithery.ai/server/@faizan45640/google-classroom-mcp-server)

An MCP (Model Context Protocol) server that provides access to Google Classroom data through Claude and other AI assistants that support the MCP protocol.

## Setup

### Prerequisites

- Node.js (v16 or higher)
- A Google Cloud Platform project with the Google Classroom API enabled
- OAuth 2.0 client credentials for the Google Classroom API

### Installation

#### Installing via Smithery

To install Google Classroom MCP Server for Claude Desktop automatically via [Smithery](https://smithery.ai/server/@faizan45640/google-classroom-mcp-server):

```bash
npx -y @smithery/cli install @faizan45640/google-classroom-mcp-server --client claude
```

#### Installing Manually
1. Clone this repository
2. Install dependencies:

```bash
npm install
```

3. Place your Google OAuth client credentials in a file named `credentials.json` in the project root:

```json
{
  "web": {
    "client_id": "YOUR_CLIENT_ID",
    "project_id": "YOUR_PROJECT_ID",
    "auth_uri": "https://accounts.google.com/o/oauth2/auth",
    "token_uri": "https://oauth2.googleapis.com/token",
    "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
    "client_secret": "YOUR_CLIENT_SECRET",
    "redirect_uris": ["http://localhost:3000/auth/google/callback"]
  }
}
```

4. Authenticate with Google:

```bash
node index.js auth
```

This will launch a browser window to complete the OAuth flow and save your credentials to `tokens.json`.

5. Configure Claude to use this server by updating `claude_desktop_config.json` (typically in `%APPDATA%\Claude\`):

```json
{
  "mcpServers": {
    "class": {
      "command": "node",
      "args": [
        "PATH_TO_YOUR_DIRECTORY\\index.js"
      ]
    }
  }
}
```

## Usage

### Available Tools

The server provides several tools for interacting with Google Classroom:

#### 1. `courses` - List all your Google Classroom courses

```
Use the 'courses' tool to get a list of all your Google Classroom courses
```

#### 2. `course-details` - Get detailed information about a specific course

```
Use the 'course-details' tool with the courseId parameter to get details and announcements for a specific course
```

Parameters:
- `courseId`: The ID of the course (can be obtained from the `courses` tool)

#### 3. `assignments` - Get assignments for a specific course

```
Use the 'assignments' tool with the courseId parameter to get assignments and your submissions for a specific course
```

Parameters:
- `courseId`: The ID of the course (can be obtained from the `courses` tool)

### Example Prompts for Claude

1. Show me all my Google Classroom courses
2. Get details for my Math course with ID 123456789
3. Show me all assignments for my History course with ID 987654321

## Permissions

The server requests the following Google Classroom API permissions:

- `classroom.courses.readonly` - To access course information
- `classroom.announcements.readonly` - To access course announcements
- `classroom.coursework.me.readonly` - To access your coursework and assignments
- `classroom.rosters.readonly` - To access class rosters

## Troubleshooting

If you encounter permission errors, try:

1. Running the auth command again to refresh permissions:
   ```
   node index.js auth
   ```

2. Ensuring your Google account is added as a test user in the Google Cloud Console if your app is in testing mode

3. Checking the OAuth scopes in the `authenticateAndSaveCredentials` function to ensure they match your needs

## Notes

- This server is designed to be used with Claude AI or other MCP-compatible assistants
- All API requests are made using your authenticated Google account
- Token refresh is handled automatically by the server
- Sensitive credentials are stored locally in the `tokens.json` file
