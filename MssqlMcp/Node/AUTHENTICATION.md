# MSSQL MCP Server Authentication Examples

This document provides examples of how to configure the MSSQL MCP Server with different authentication methods.

## Quick Start with Environment Variables

For local development, you can use a `.env` file:

1. Copy the example file: `cp .env.example .env`
2. Edit `.env` with your database configuration
3. The server will automatically load these variables

## Azure Active Directory Authentication (Default)

This is the default authentication method. The server will prompt for browser-based authentication to Azure AD.

### Claude Desktop Configuration
```json
{
  "mcpServers": {
    "mssql-aad": {
      "command": "node",
      "args": ["/path/to/your/dist/index.js"],
      "env": {
        "SERVER_NAME": "your-server.database.windows.net",
        "DATABASE_NAME": "your-database-name",
        "READONLY": "false"
      }
    }
  }
}
```

### VS Code Agent Configuration
```json
{
  "mcp": {
    "servers": {
      "mssql-aad": {
        "type": "stdio",
        "command": "node",
        "args": ["/path/to/your/dist/index.js"],
        "env": {
          "SERVER_NAME": "your-server.database.windows.net",
          "DATABASE_NAME": "your-database-name",
          "READONLY": "false"
        }
      }
    }
  }
}
```

## SQL Server Authentication (Username/Password)

Use this method when you have SQL Server authentication credentials.

### Claude Desktop Configuration
```json
{
  "mcpServers": {
    "mssql-sql-auth": {
      "command": "node",
      "args": ["/path/to/your/dist/index.js"],
      "env": {
        "SERVER_NAME": "your-server.database.windows.net",
        "DATABASE_NAME": "your-database-name",
        "USE_USERNAME_PASSWORD": "true",
        "SQL_USERNAME": "your-sql-username",
        "SQL_PASSWORD": "your-sql-password",
        "READONLY": "false"
      }
    }
  }
}
```

### VS Code Agent Configuration
```json
{
  "mcp": {
    "servers": {
      "mssql-sql-auth": {
        "type": "stdio",
        "command": "node",
        "args": ["/path/to/your/dist/index.js"],
        "env": {
          "SERVER_NAME": "your-server.database.windows.net",
          "DATABASE_NAME": "your-database-name",
          "USE_USERNAME_PASSWORD": "true",
          "SQL_USERNAME": "your-sql-username",
          "SQL_PASSWORD": "your-sql-password",
          "READONLY": "false"
        }
      }
    }
  }
}
```

## Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| SERVER_NAME | Yes | Your Azure SQL server name (e.g., myserver.database.windows.net) |
| DATABASE_NAME | Yes | Your database name |
| USE_USERNAME_PASSWORD | No | Set to "true" to use SQL Server authentication |
| SQL_USERNAME | Conditional* | SQL Server username (required if USE_USERNAME_PASSWORD is true) |
| SQL_PASSWORD | Conditional* | SQL Server password (required if USE_USERNAME_PASSWORD is true) |
| READONLY | No | Set to "true" for read-only operations (default: false) |
| CONNECTION_TIMEOUT | No | Connection timeout in seconds (default: 30) |
| TRUST_SERVER_CERTIFICATE | No | Set to "true" to trust self-signed certificates (default: false) |

*Required when USE_USERNAME_PASSWORD is set to "true"

## Security Considerations

1. **Azure Active Directory**: More secure as it uses modern OAuth2 authentication with token-based access
2. **SQL Server Authentication**: Simpler setup but requires managing username/password credentials
3. **Environment Variables**: Store sensitive credentials securely and avoid committing them to source control
4. **Read-Only Mode**: Use READONLY="true" in production environments when only read access is needed
