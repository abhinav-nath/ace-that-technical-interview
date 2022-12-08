# Logging

## Log levels

Following are the log levels available in Log4j 2:

Most Entries <------------------------------> Least Entries

```
ALL < TRACE < DEBUG < INFO < WARN < ERROR < FATAL < OFF
```

| Log Level | intLevel            | Description                                                                                               | Examples                                                                                                                                          |
|-----------|---------------------|-----------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| ALL       | `Integer.MAX_VALUE` | Has the lowest possible rank and is intended to turn on all logging.                                      |                                                                                                                                                   |
| TRACE     | 600                 | Designates finer-grained informational events than the `DEBUG`.                                           | Local variable values. System level calls (OS)                                                                                                    |
| DEBUG     | 500                 | Designates fine-grained informational events that are most useful to debug an application.                | Method/API calls /stack invocations, parameter values. Data retrieval logic (SQL statements).                                                     |
| INFO      | 400                 | Designates informational messages that highlight the progress of the application at coarse-grained level. | Service Start/Stop/Restart. Service Invocations with payload.                                                                                     |
| WARN      | 300                 | Designates potentially harmful situations.                                                                | Service timeout, Retry<br/>Circuit breaker logic invoked.<br/>i.e. unable to complete a calculation. Defaulting to recovery behavior/assumptions. |
| ERROR     | 200                 | Designates error events that might still allow the application to continue running.                       | Failed Connections.<br/>Unrecoverable Error in a service, forcing a restart.                                                                      |
| FATAL     | 100                 | Designates very severe error events that will presumably lead the application to abort.                   | Resource exhausted, OOM errors.                                                                                                                   |
| OFF       | 0                   | The highest possible rank and is intended to turn off logging.                                            |                                                                                                                                                   |

### `intLevel`

An integer that determines where the custom level exists in relation to the standard levels built-in to Log4J 2.