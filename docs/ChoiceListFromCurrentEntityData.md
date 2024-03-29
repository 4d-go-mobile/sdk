#  ChoiceListFromCurrentEntityData

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
"1"; "One Label";\
"2"; "Two Label"\
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

## If we want to go further

Do we manage to unselect value, or let choiceList has a value that mean nothing. I must check on android that we could set not value after setting a value, I am not sure of that.


## Demo

With two different entities
```4d
var $e : cs.EmployeeEntity

$e:=ds.Employee.new()
$e.FirstName:="John"
$e.LastName:="Doe"
$e.objectField:=New object("1"; "lorem"; "2"; "ipsum")
$e.save()

$e:=ds.Employee.new()
$e.FirstName:="Jane"
$e.LastName:="Doe"
$e.objectField:=New object("0"; "dolor"; "2"; "sit"; "7"; "amet")
$e.save()
```
 with `objectField` in input control

```json
{
    "type": [ "text" ],
    "choiceList": {
      "dataSource": {
        "field": "objectField",
        "currentEntity": true
      }
    },
    "name": "EmployeeObjectField",
    "format": "push"
 }
```

we could select different values according to data inside entities

https://github.com/4d-go-mobile/sdk/assets/129385512/d86a05d1-a5ea-4806-ba6b-2ba2b740344c

