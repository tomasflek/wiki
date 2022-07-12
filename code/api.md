# ASP.NET Web API

## HTTP Methods
| Method | Description |
|------|:------------|
| [GET](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET) | Reqest for specific data. |
| [POST](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST) | Sends data to a server. |
| [DELETE](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/DELETE) | Removes data from a server. |


For more mothods and details see [mozila page](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods).

## HTTP Response status codes

**Successful response**

| Code | Type | Description |
|----------|:-------------:|:-------------|
| 200 | OK | The request succeeded. |
| 201 | Created | The request led to the create of a resource. |
| 204 | No Content | |

**Client error responses**
| Code | Type | Description |
|----------|:-------------:|:-------------|
| 400 | Bad request | |
| 401 | Unauthorized | |
| 404 | Not found | In API this meands that a endpoind is valid, but the resource does not exist. |
| 408 | Request timeout | |
| 429 | Too many requests | |

**Server error responses**
| Code | Type | Description |
|----------|:-------------:|:-------------|
| 500 | Internal server error | |
| 503 | Service unavailable | |

For more detailed description on other codes see [mozila pages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#information_responses).

## Rules to create API endpoint
- it is a good practice to create Dto based on operation, e.g. `PointOfInterestsDto.cs` for read, `PointOfInterestsForCreateDto.cs` for create operation, `PointOfInterestsForUpdateDto.cs` for update operation.
- if a create operation is called, it is a good practice to reurn created object on JSON response - call `return CreatedAtRoute` method

**Validations**
- use [build in](https://docs.microsoft.com/en-us/aspnet/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) validation
- [FluentValidation](https://fluentvalidation.net)

**ActionResults**
- `return NotFound();`
- `return NoContent();`
- `return StatusCode(500, "A problem happened ...");`

**Logging providers**
- [Logging providers](https://docs.microsoft.com/en-us/dotnet/core/extensions/logging-providers)
- [Third-party logging providers](https://docs.microsoft.com/en-us/dotnet/core/extensions/logging-providers#third-party-logging-providers)

Example of `Serilog` usage instead of default logging provider:

``` csharp
Log.Logger = new LoggerConfiguration()
        .MinimumLevel.Debug()
        .WriteTo.Console()
        .WriteTo.File("logs/info.txt", rollingInterval: RollingInterval.Day)
        .CreateLogger();

var builder = WebApplication.CreateBuilder(args);
builder.Logging.ClearProviders();
builder.Logging.AddConsole();

builder.Host.UseSerilog();
```
> Make sure that `Serilog.AspNetCore`, `Serilog.Sinks.Console` and `Serilog.Sinks.File` nuget packages are installed.

**Tips**
- to receive XML (if XML is requested instead of JSON) use `builder.Services.AddControllers().AddXmlDataContractSerializerFormatters();`