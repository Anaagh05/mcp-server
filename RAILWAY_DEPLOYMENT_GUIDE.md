# Railway Deployment Guide

This document summarizes the steps we took to deploy the MCP server to Railway. 

## 1. Prerequisites
- `credentials.json` downloaded from Google Cloud Console.
- `token.json` generated locally by running `python auth.py` and completing the Google OAuth flow.
- Both of these files are listed in `.gitignore` and are **not** committed to version control.

## 2. Deployment Setup

The repository contains a `railway.toml` file which automatically configures Railway to build the app using Nixpacks and start it with Uvicorn.

To deploy via the Railway CLI, the following commands were used:

```bash
# 1. Login to Railway
railway login

# 2. Initialize the project
railway init -n saksham-mcp-server

# 3. Deploy the code
railway up
```

## 3. Environment Variables Configuration

Since Railway containers are ephemeral and we don't commit secrets, the contents of your secret JSON files were injected directly into Railway as environment variables.

```bash
# Skip manual tool execution approvals in the cloud
railway variables set AUTO_APPROVE=true

# Inject credentials
Get-Content -Raw credentials.json | railway variables set GOOGLE_CREDENTIALS_JSON --stdin
Get-Content -Raw token.json | railway variables set GOOGLE_TOKEN_JSON --stdin
```

## 4. Live Server

The server is currently live and accessible at:
👉 `https://saksham-mcp-server-production-1edb.up.railway.app`

### Testing the Endpoints:

**Health Check:**
```bash
curl https://saksham-mcp-server-production-1edb.up.railway.app/
```

**List Tools:**
```bash
curl https://saksham-mcp-server-production-1edb.up.railway.app/tools
```
