---
title: Usage
---

## Running the Proxy

For a quick setup and test environment, the proxy only requires a single environment variable to be set; `DVC_LB_PROXY_SDK_KEY`. 
This key is used to authenticate to the CDN and Events API, as well 
You can alternatively pass in a full configured file instead of the environment variables via the `-c` flag.

Once the environment variable has been set - start the proxy binary.

```bash
devcycle-local-bucketing-proxy
```

At this point the proxy is live and ready to accept requests from any SDK that is supported. The default configuration 
is to start a TCP server on `localhost:8080`.

# SDK Configuration

Currently only Server SDKs are supported. The default configuration of the proxy will start run at `localhost:8080` 
in HTTP TCP mode.
Not all SDKs are configured the same way. Please see the SDK documentation for specific configuration instructions.

Sample configurations for each SDK verified to work with the proxy are below.

## PHP SDK Configuration

### HTTP Socket Configuration
```php
use DevCycle\DevCycleConfiguration;

$config = DevCycleConfiguration::getDefaultConfiguration()
    ->setApiKey("Authorization", getenv("DEVCYCLE_SERVER_SDK_KEY"))
    ->setHost("http://localhost:8080");
```

### Unix Socket Configuration

```php
use DevCycle\DevCycleConfiguration;

$config = DevCycleConfiguration::getDefaultConfiguration()
    ->setApiKey("Authorization", getenv("DEVCYCLE_SERVER_SDK_KEY"))
    ->setHost("http:/v1")
    ->setUDSPath("/path/to/socket/file.sock");
```


## Python SDK Configuration

```python
from devcycle_python_sdk import DevCycleLocalOptions

options = DevCycleLocalOptions(config_cdn_uri = "http://localhost:8080/", events_api_uri = "http://localhost:8080/")
```

## C# SDK Configuration

### Local Bucketing

```csharp
using DevCycle.SDK.Server.Common.Model.Local;
var options = new DevCycleLocalOptions
                { CdnUri = "http://localhost:8080/", EventsApiUri = "http://localhost:8080/" };
```

### Cloud Bucketing

```csharp
using DevCycle.SDK.Server.Common.API;
var restOptions = new DevCycleRestClientOptions { BaseUrl = new Uri("http://localhost:8080/") };
```