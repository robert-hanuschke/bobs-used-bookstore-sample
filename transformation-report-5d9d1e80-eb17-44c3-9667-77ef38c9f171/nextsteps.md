# Next Steps

## Issues resolved
- Transformed Bookstore.Domain.csproj to net8.0
- Transformed Bookstore.Data.csproj to net8.0
- Transformed Bookstore.Web.csproj to net8.0
- Transformed Bookstore.Cdk.csproj to net8.0
- Transformed Bookstore.Domain.Tests.csproj to net8.0

## Validation and Testing

Based on the transformation results, your solution appears to have been successfully migrated to cross-platform .NET with no build errors reported across all five projects. To ensure the transformation is complete and functional, follow these validation steps:

### 1. Verify Build Success

```bash
# Clean and rebuild the entire solution
dotnet clean
dotnet build --configuration Release
```

Confirm that all projects compile without warnings or errors.

### 2. Review Target Framework

Check that all projects are targeting the appropriate .NET version:

```bash
# Review each .csproj file
grep -r "<TargetFramework>" --include="*.csproj"
```

Ensure consistency across projects (e.g., `net6.0`, `net7.0`, or `net8.0`).

### 3. Execute Unit Tests

Run the test project to validate functionality:

```bash
# Run all tests
dotnet test app/Bookstore.Domain.Tests/Bookstore.Domain.Tests.csproj --configuration Release

# Generate detailed test results
dotnet test --logger "console;verbosity=detailed"
```

Review test results for any failures or skipped tests that may indicate compatibility issues.

### 4. Verify Package References

Check that all NuGet packages are compatible with the target framework:

```bash
# List outdated packages
dotnet list package --outdated

# Check for deprecated packages
dotnet list package --deprecated
```

Update any packages that have newer cross-platform compatible versions.

### 5. Validate Data Layer Functionality

For `Bookstore.Data`, verify database connectivity and Entity Framework Core compatibility:

- Test database migrations if using EF Core
- Verify connection strings are correctly configured for cross-platform environments
- Test CRUD operations against your data store

### 6. Review CDK Infrastructure Code

For `Bookstore.Cdk`, ensure AWS CDK constructs are compatible:

```bash
# Synthesize CloudFormation template
cd app/Bookstore.Cdk
cdk synth
```

Verify that the CDK app generates valid CloudFormation templates.

### 7. Test Web Application Locally

Run the web application to ensure it functions correctly:

```bash
# Run the web project
dotnet run --project app/Bookstore.Web/Bookstore.Web.csproj
```

Test the following:
- Application starts without errors
- All endpoints respond correctly
- Static files are served properly
- Configuration is loaded correctly

### 8. Check for Runtime Compatibility Issues

Review code for patterns that may have cross-platform issues:

- File path separators (use `Path.Combine` instead of hardcoded slashes)
- Case-sensitive file system references
- Windows-specific APIs or libraries
- Environment-specific configuration

### 9. Validate Configuration Files

Ensure configuration files are properly migrated:

- `appsettings.json` and environment-specific variants
- `launchSettings.json` for development settings
- Any custom configuration providers

### 10. Performance and Integration Testing

Conduct broader testing:

- Load test the web application under expected traffic
- Test integration points with external services
- Verify logging and monitoring functionality
- Test error handling and exception scenarios

## Deployment Preparation

### 1. Create Publish Profiles

Generate deployment artifacts:

```bash
# Publish for Linux
dotnet publish app/Bookstore.Web/Bookstore.Web.csproj -c Release -r linux-x64 --self-contained false

# Publish for Windows (if needed)
dotnet publish app/Bookstore.Web/Bookstore.Web.csproj -c Release -r win-x64 --self-contained false
```

### 2. Update Deployment Scripts

Review and update any deployment automation:

- Replace .NET Framework-specific deployment commands with `dotnet` CLI commands
- Update server prerequisites to include the appropriate .NET runtime
- Verify deployment target environments have the correct .NET version installed

### 3. Deploy CDK Infrastructure

If using AWS CDK for infrastructure:

```bash
cd app/Bookstore.Cdk
cdk deploy
```

Verify that all AWS resources are created successfully.

### 4. Deploy Application

Deploy the web application to your target environment:

- Copy published artifacts to the target server
- Configure the web server (Kestrel, IIS, Nginx, Apache)
- Update environment variables and configuration
- Start the application and monitor logs

### 5. Post-Deployment Validation

After deployment:

- Verify the application is accessible
- Test critical user workflows
- Monitor application logs for errors
- Check performance metrics
- Validate database connectivity in production

## Documentation Updates

Update project documentation to reflect the migration:

- Update README with new build and run instructions using `dotnet` CLI
- Document any breaking changes or configuration updates
- Update development environment setup guides
- Revise deployment procedures

## Monitoring

Establish monitoring for the migrated application:

- Set up application performance monitoring
- Configure error tracking and alerting
- Monitor resource utilization on the new platform
- Track key business metrics to ensure functionality parity