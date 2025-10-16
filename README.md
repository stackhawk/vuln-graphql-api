# This repo has been archived and now lives at https://github.com/kaakaww/vuln-graphql-api
## vuln-graphql-api

This fork of [vulnerable-graphql-api](https://github.com/CarveSystems/vulnerable-graphql-api) simplifies 
and cleans up the Docker build for quick deployment and testing with docker-compose. 

## Security Testing with StackHawk

🔒 **This repository is integrated with [StackHawk](https://stackhawk.com/) for automated security testing.**

StackHawk performs dynamic application security testing (DAST) to identify GraphQL vulnerabilities including injection flaws, authentication bypasses, and GraphQL-specific security issues.

📋 **[View complete StackHawk integration guide →](STACKHAWK.md)**

### Quick Start Security Testing
```bash
# Set your StackHawk API key
export HAWK_API_KEY="your-api-key"

# Start the vulnerable API
SERVER_PORT=3000 docker-compose up -d

# Run security scan
hawk scan stackhawk.yml

# View results at https://app.stackhawk.com/
```

## Docker Build

 - Make sure _docker-compose_ is present on the system.
 - Set `SERVER_PORT` in the environment and run `docker-compose up` 
 
```bash
SERVER_PORT=3000
docker-compose up
```
