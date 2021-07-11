# Formatter with code

> iOS only

## Binding

## Default behaviour

UI for fields are by default only `UILabel`, a swift graphical component to display text by filling `text` or `attributedText` field.

By default we bind on `text`, and we could find in storyboard generated when selecting the label this `User Defined Runtime Attributes`

| KeyPath | Type | Value | 
| -| - | - | 
| `bindTo.record.<database field name>` | String | text |

## Custom format binding

To introduce a new format we will bind to a new `UILabel` attribute by creating an extension.

For the example we will use the name `myNewFormat` and all files will be added to  `Resources/mobile/formatter`

in a `Sources/Formatters/UILabel+myNewFormat.swift`

```swift
import UIKit

extension UILabel {

    @objc dynamic public var myNewFormat: String? {
        get {
            return self.text // or if possible make reverse format
        }
        set {
            if let value = newValue {
                 self.text = value
                 // 1/ here instead you could format the string to wanted value according to passed value
                 // 2/ you could add children view or gestures to make interaction with the field
            } else {
                 self.text = ""
                 // 1: set any value if database value null/nil
                 // 2: you could remove added subview or hide them, or remove gestures
            }
        }
    }
}
```

and to make storyboard bind on your property like that

| KeyPath | Type | Value | 
| - | - | - | 
| `bindTo.record.<database field name>` | String | myNewFormat |

the formatter define a `manifest.json` for the format

```json
{
	"name": "myNewFormat",
	"binding": "myNewFormat",
	"type" : "text"
}
```

If you choose `text` as `type`, it will be visible on format menu for 4D alpha/text field 

For other type of data, the swift extension must replace `: String?` by associated type
- date: : `Date?`
- number: `NSNumber?`

### some examples

- [formatter-Url](https://github.com/4d-go-mobile/formatter-Url)
- [formatter-Mail](https://github.com/4d-go-mobile/formatter-Mail)
- [formatter-Phone](https://github.com/4d-go-mobile/formatter-Phone)
