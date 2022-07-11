# API

## HTTP Methods
| Method | Description |
|------|:------------:|
| [GET](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET) | Reqest for specific data. |
| [POST](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST) | Sends data to a server. |


For more mothods and details see [mozila page](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods).

## HTTP Response status codes

**Successful response**

| Code | Type | Description |
|----------|:-------------:|:-------------:|
| 200 | OK | The request succeded. |

**Client error responses**
| Code | Type | Description |
|----------|:-------------:|:-------------:|
| 400 | Bad request | |
| 401 | Unauthorized | |
| 404 | Not found | In API this meands that a endpoind is valid, but the resource does not exist. |
| 408 | Request timeout | |
| 429 | Too many requests | |

**Server error responses**
| Code | Type | Description |
|----------|:-------------:|:-------------:|
| 500 | Internal server error | |
| 503 | Service unavailable | |

For more detailed description on other codes see [mozila pages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#information_responses).

## Rules to create API endpoint
- 