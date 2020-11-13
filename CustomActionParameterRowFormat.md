
# CustomActionParameterRowFormat

When opening an action form, each action parameter have a `Row` from `Eureka` framework

Row are builded according first to `format` if defined, otherwise using the `type`


## But if you want to have custom row and the SDK do not provide it functionnality? (18R6)

For that you must provide a swift code,  which implement the protocol `ActionParameterCustomFormatRowBuilder` to build a `Row` according to `format`.

There is two way to do it, implement in `AppDelegate` or using an injected [`ApplicationService`](ApplicationService.md)

### Simple example

```swift
import QMobileUI
import Eureka

extension AppDelegate: ActionParameterCustomFormatRowBuilder {
    func buildActionParameterCustomFormatRow(key: String, format: String, onRowEvent eventCallback: @escaping OnRowEventCallback) -> ActionParameterCustomFormatRowType? {
        if format == "textDate" {
            return DateRow(key).onRowEvent(eventCallback)
        }
        return nil
    }
}
```

in project JSON, in your action parameter

```json
  .., "parameters": [.., {"name":"ADate", "type": "text", "format": "textDate"}]
```
