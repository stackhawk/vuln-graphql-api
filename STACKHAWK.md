# StackHawk Security Testing Integration

This repository is integrated with [StackHawk](https://stackhawk.com/) for automated security testing of the vulnerable GraphQL API. StackHawk performs dynamic application security testing (DAST) to identify vulnerabilities in running applications.

## Overview

StackHawk scans this vulnerable GraphQL API to demonstrate security testing capabilities and identify common GraphQL vulnerabilities including:

- **Injection vulnerabilities** (SQL injection, NoSQL injection)
- **Authentication and authorization flaws**
- **Information disclosure**
- **GraphQL-specific vulnerabilities** (query depth attacks, introspection enabled)
- **Cross-Site Scripting (XSS)**
- **Insecure direct object references**

## Configuration Files

### `stackhawk.yml`
Main configuration file for local development and comprehensive security testing:
- **Application ID**: `a919a264-de99-4087-b3b4-5939104caf9f`
- **Environment**: Development
- **Host**: `http://localhost:3000`
- **GraphQL Configuration**: Enabled with introspection and deep query testing
- **Failure Threshold**: High (only fails on high-severity vulnerabilities)

### `stackhawk-github.yml`
GitHub Actions-specific configuration optimized for CI/CD:
- **Environment**: GitHub
- **Host**: `http://vuln-graphql:3000` (Docker network)
- **Optimized scan duration** for faster CI/CD pipeline execution

## GitHub Actions Integration

The repository includes a GitHub Actions workflow (`.github/workflows/hawkscan.yml`) that:

1. **Triggers on**:
   - Push events to `main` and `master` branches
   - Pull requests to `main` and `master` branches

2. **Workflow steps**:
   - Checks out the code
   - Builds and starts the vulnerable GraphQL API using Docker Compose
   - Runs StackHawk security scan using the official StackHawk GitHub Action
   - Uploads results to GitHub Security tab (if enabled)
   - Provides scan results and dashboard links

## Prerequisites

### Required Secrets
Add the following secret to your GitHub repository:

- `HAWK_API_KEY`: Your StackHawk API key (get it from [StackHawk Dashboard](https://app.stackhawk.com/))

### Setting up the API Key
1. Go to [StackHawk Dashboard](https://app.stackhawk.com/)
2. Navigate to **Settings** → **API Keys**
3. Create or copy your API key
4. In your GitHub repository, go to **Settings** → **Secrets and variables** → **Actions**
5. Add a new secret named `HAWK_API_KEY` with your API key value

## Local Testing

To run StackHawk scans locally:

### Prerequisites
- Docker and Docker Compose installed
- StackHawk CLI installed (`npm install -g @stackhawk/cli`)
- StackHawk account and API key

### Running Local Scans

1. **Start the application**:
   ```bash
   SERVER_PORT=3000 docker-compose up -d
   ```

2. **Run StackHawk scan**:
   ```bash
   export HAWK_API_KEY="your-api-key-here"
   hawk scan stackhawk.yml
   ```

3. **Stop the application**:
   ```bash
   docker-compose down
   ```

## Viewing Results

### StackHawk Dashboard
- Visit [StackHawk Dashboard](https://app.stackhawk.com/)
- Navigate to your application to view detailed scan results
- Review vulnerabilities, remediation guidance, and historical trends

### GitHub Security Tab
- Go to your repository's **Security** tab
- View **Code scanning alerts** for StackHawk findings (if enabled)
- Track vulnerability status and remediation progress

## GraphQL-Specific Testing

This configuration includes comprehensive GraphQL security testing:

### Enabled Features
- **Schema Introspection**: Tests if GraphQL introspection is exposed
- **Query Depth Analysis**: Tests for query depth limit vulnerabilities
- **Batch Query Testing**: Tests for batch query vulnerabilities
- **Authentication Testing**: Tests GraphQL authentication mechanisms
- **Authorization Testing**: Tests field-level and operation-level authorization

### GraphQL Vulnerabilities Detected
- **Introspection enabled** (information disclosure)
- **No query depth limiting** (DoS vulnerabilities)
- **No query complexity analysis** (resource exhaustion)
- **Authentication bypass** in GraphQL mutations
- **Authorization flaws** in GraphQL field resolution
- **Injection vulnerabilities** in GraphQL resolvers

## Customization

### Adjusting Scan Coverage
Edit `stackhawk.yml` to modify:
- **Scan duration**: Adjust `spider.maxDuration`
- **GraphQL depth**: Modify `graphqlConf.maxDepth`
- **Request delays**: Tune `introspection.requestDelay`
- **Failure threshold**: Change `hawk.failureThreshold`

### Adding Authentication
If the API requires authentication, configure:
```yaml
authentication:
  sessionManagement:
    sessionTokenPath:
      - "$.token"
      - "$.data.token"
```

### Environment-Specific Configuration
Create additional configuration files for different environments:
- `stackhawk-staging.yml`
- `stackhawk-production.yml`

## Best Practices

1. **Run scans regularly**: Set up automated scans on code changes
2. **Review all findings**: Even in a vulnerable application, track new vulnerabilities
3. **Set appropriate thresholds**: Balance security with development velocity
4. **Monitor trends**: Use StackHawk dashboard to track vulnerability trends over time
5. **Integrate with development workflow**: Use scan results to guide security improvements

## Troubleshooting

### Common Issues

**Scan fails to start**:
- Verify API key is correctly set
- Check if application is accessible at configured host
- Ensure Docker containers are running

**No vulnerabilities found**:
- Verify GraphQL endpoint is accessible at `/graphql`
- Check if application started correctly
- Review StackHawk logs for connection issues

**High scan duration**:
- Reduce `maxDepth` in GraphQL configuration
- Decrease `spider.maxDuration`
- Limit scan scope with URL filters

### Getting Help
- [StackHawk Documentation](https://docs.stackhawk.com/)
- [StackHawk Support](https://docs.stackhawk.com/contact/)
- [GitHub Issues](https://github.com/stackhawk/vuln-graphql-api/issues)

## Security Note

⚠️ **This is a vulnerable application for testing purposes only.**

Do not deploy this application in production environments. It contains intentional security vulnerabilities for demonstration and testing of security scanning tools.