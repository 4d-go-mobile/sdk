#  ChoiceListFromCurrentEntityData

> [!CAUTION]
> Not released, just a specification

- The purpose is for input control with choice list, to show a list of data stored inside the current entity, allowing to have different choice list by entity

# How to

## Database content

For entities, an object field must contain the choice list as key value
- key is the display value
- value is the value send to server and receive in database method "On Mobile App Action"

We could fill using an object

```4d
var $entity: cs.ATable
...
$entity.aChoiceObjectField:=New Object(\
"One"; 1;\
"Two"; 2\
)
```

> 🚧 show some other 4D code to fill it (if possible)

## Input control definition

The input control manifest.json contains a `choiceList` key [as usual for choice list filled from data store](https://developer.4d.com/go-mobile/docs/project-definition/actions#dynamic-choice-lists), but contain a new key `currentEntity` ( and the value must be `true`)

`field` will be used to specify the name of the field where choice list are stored. (in the exemple `aChoiceObjectField`, see 4d code)

(`dataClass` is not mandatory, but could help to filter later to do not propose this input control for an other table)

```json
{
    "name": "myChoiceList",
    "type": [
        "text"
    ],
    "format":"push",

    "choiceList": {
        "dataSource": {
            "field": "aChoiceObjectField",
            "dataClass": "ATable",
            "currentEntity": true
        }
    }
}
```

##  Android

Inside `app/src/main/assets/inputControls.json` the input control json definition is copyed, so we could have

```json
[ ...
  {
    "type": [
      "text"
    ],
    "choiceList": {
      "dataSource": {
        "field": "aChoiceObjectField",
        "dataClass": "ATable",
        "currentEntity": true
      }
    },
    "name": "myChoiceList",
    "format": "push"
  }

...]
```

then as usual for action, inside actions.json, the corresponding action parameter contains the `source` (nothing change here)

```json
...
{
    "name": "myParameter",
    "label": "New Parameter",
    "shortLabel": "New",
    "type": "string",
    "format": "/push",
    "source": "/myChoiceList"
}
...
```

##  iOS

Inside `???ListForm.Storyboard` and `???DetailForm.Storyboard` (xml files)

Inside user defined attributes of view controller or list view, if there is actions for the table/dataclass or the entity , there is a key `actions` that contains the json of corresponding actions with input control definition injected in `choiceList` key (encoded for XML)

```xml
<userDefinedRuntimeAttribute type="string" keyPath="actions">
   <mutableString key="value">{"actions":[ ... {"name":"anAction","scope":"currentEntity", ... ,"parameters":[{"name":"newParameter","label":"New Parameter","shortLabel":"New","type":"string","choiceList":{"dataSource":{"dataClass":"ATable","field":"aChoiceObjectField","currentEntity":true}}}...]}]}</mutableString>
</userDefinedRuntimeAttribute>
```
