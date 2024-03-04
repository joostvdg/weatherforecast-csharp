# C# Sample Accelerator

A sample accelerator for C#.

This sample is the Weather Forecast RESTful API application made available from Microsoft.

The starting source for this sample was created using:
```
$ dotnet new webapi --framework net6.0 --language C#
```

## Test

12

## Nuget from Nexus

* https://help.sonatype.com/repomanager2/.net-package-repositories-with-nuget
* https://learn.microsoft.com/en-us/nuget/install-nuget-client-tools
* https://learn.microsoft.com/en-us/aspnet/core/tutorials/getting-started-with-swashbuckle?view=aspnetcore-7.0
* https://www.nuget.org/packages/Swashbuckle.AspNetCore
* https://awasu.com/weblog/nexus3/nuget/

```sh
https://nexus.services.h2o-2-13047.h2o.vmware.com/repository/nuget-group/
```

```sh
dotnet nuget add source  https://nexus.services.h2o-2-13047.h2o.vmware.com/repository/nuget.org-proxy/index.json --name Proxy
```

```sh
dotnet nuget disable source nuget.org
dotnet nuget list source
```

```sh
dotnet add package Swashbuckle.AspNetCore --version 6.5.0
```

```sh
❯ dotnet nuget list source
Registered Sources:
  1.  nuget.org [Disabled]
      https://api.nuget.org/v3/index.json
  2.  Proxy [Enabled]
      https://nexus.services.h2o-2-13047.h2o.vmware.com/repository/nuget-group/index.json
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" protocolVersion="3" />
    <add key="Proxy" value="https://nexus.services.h2o-2-13047.h2o.vmware.com/repository/nuget.org-proxy/index.json" />
  </packageSources>
  <disabledPackageSources>
    <add key="nuget.org" value="true" />
  </disabledPackageSources>
</configuration>
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="Proxy" value="https://nexus.services.h2o-2-13047.h2o.vmware.com/repository/nuget.org-proxy/index.json" />
  </packageSources>
</configuration>
```


```yaml
apiVersion: v1
kind: Secret
metadata:
  name: nuget-config
  namespace: teal
type: service.binding/nugetconfig
stringData:
  type: nugetconfig
  provider: sample
  nuget.config: |
    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <packageSources>
        <add key="Proxy" value="https://nexus.services.h2o-2-13047.h2o.vmware.com/repository/nuget.org-proxy/index.json" />
      </packageSources>
    </configuration>
```

## Running the app locally

To run the sample application:

```
$ dotnet run
```

## Deploying to Kubernetes as a TAP workload with Tanzu CLI

> NOTE: The provided `config/workload.yaml` file uses the Git URL for this sample. When you want to modify the source, you must push the code to your own Git repository and then update the `spec.source.git` information in the `config/workload.yaml` file.

If you make modifications to the source, push these changes to your own Git repository.

When you are done developing your app, you can simply deploy it using:

```
tanzu apps workload apply -f config/workload.yaml
```

If you would like deploy the code from your local working directory you can use the following command:

```
tanzu apps workload create weatherforecast-csharp -f config/workload.yaml \
  --local-path . \
  --source-image <REPOSITORY-PREFIX>/weatherforecast-csharp-source \
  --type web
```

## Accessing the app deployed to your cluster

Determine the URL to use for the accessing the app by running:

```
tanzu apps workload get weatherforecast-csharp
```

To access the deployed app use the URL shown under "Workload Knative Services" and append the endpoint `/weatherforecast` to that URL.

This depends on the TAP installation having DNS configured for the Knative ingress.

## Pipeline With TAP

* Build & Test C# in Tekton Pipeline/Task
* Generate Git Tag
* Build image with Buildpacks using said image

## Database via ResourceClaim on existing DB via Secret

* 

## Application Configuration Service

* Run Application in two TAP environments
* Retrieve config for specific environment
* 

## Blue - Green Deployment

* Run in two TAP environments
* Able to fail over
* shared database?
* ?

## Test Containers via VM

* https://apps-cloudmgmt.techzone.vmware.com/resource/running-testcontainers-tanzu-application-platform#sec38999-sub10
