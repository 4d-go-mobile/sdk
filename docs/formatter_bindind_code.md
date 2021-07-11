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

#### adding right button

```swift
    fileprivate static let myButtonTag = 789 // fast way to retrieve object, another way is to look at subview type and content

    @objc dynamic public var myFormatWithButton: String? {
        get {
            return self.text // or if possible make reverse format
        }
        set {
            // add button if not exists
            if self.viewWithTag(UILabel.myButtonTag) == nil {
                let button = UIButton(primaryAction: UIAction(title: "", image: UIImage(systemName: "phone"), identifier: UIAction.Identifier("myNewFormat"),  state: .on, handler: { action in
                    
                    // do something here on touch
                }))
                //imageView.tintColor = .background
                button.tag = UILabel.myButtonTag
                self.addSubview(button)
                button.translatesAutoresizingMaskIntoConstraints = false
                self.centerYAnchor.constraint(equalTo: button.centerYAnchor).isActive = true
                self.rightAnchor.constraint(equalTo:button.rightAnchor, constant: 4).isActive = true
            }
            self.text = newValue
            // enable or not the button according to content for instance
            (self.viewWithTag(789) as? UIButton)?.isEnabled = !(self.text?.isEmpty ?? true)
        }
    }
    
 ```

#### adding an image and a tag gesture

```swift
    @objc dynamic public var myNewFormat: String? {
        get {
            return self.text // or if possible make reverse format
        }
        set {
            let gesture = UITapGestureRecognizer(target: self, action: #selector(myNewFormatAction(_:)))
            // add button if not exists
            if self.viewWithTag(759) == nil {
                let imageView = UIImageView(image: UIImage(systemName: "phone"))
                imageView.addGestureRecognizer(gesture)
                imageView.tag = 759
                imageView.isUserInteractionEnabled = false
                self.addSubview(imageView)
                imageView.translatesAutoresizingMaskIntoConstraints = false
                self.centerYAnchor.constraint(equalTo: imageView.centerYAnchor).isActive = true
                self.rightAnchor.constraint(equalTo:imageView.rightAnchor, constant: 4).isActive = true
            }
            self.text = newValue
        
            if let imageView = (self.viewWithTag(789) as? UIImageView) {
                let isEmpty = (self.text?.isEmpty ?? true)
                imageView.isUserInteractionEnabled = !isEmpty
                // could change optionnaly color or image tint according to text value
                imageView.tintColor = isEmpty ? .systemGray5: .background
            }
        }
    }
    @objc func myNewFormatAction(_ sender: UITapGestureRecognizer) {
        print("do something")
    }



