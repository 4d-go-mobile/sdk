# ActionParameterRules

An action parameter could be associated to a list of input rules. Some are set by project editor like minimum or maximul values, like is mandatory or not, etc..

You could find an action parameter definition in project.4dmobileapp file, for instance for a a parameter you could have two rules here: first is mandatory and must contains one letter only (defined by regex)

```json
			"parameters": [
				{
					"fieldNumber": 2,
					"name": "Name",
					"label": "Name",
					"shortLabel": "Name",
					"type": "string",
					"rules": [
						"mandatory",
						{
							"regex": "[a-z]"
						}
					]
				},
```


## min rules

```json
"rules": [
						{
							"min": 5
						}
]
```

## max rules

```json
"rules": [
						{
							"max": 5
						}
]
```

## mandatory

```json
"rules": [
		"mandatory"
]
```

## regex

```json
"rules": [
		{
							"regex": "[a-z]*"
		}
]
```

### regex with specific error message


```json
"rules": [
		{
							"regex": "[a-z]*$Must contains only lowercase letters"
		}
]
```

## is multiple of 

```json
"rules": [
		{
				"isMultipleOf": 10
		}
]
```

### for time, formatter = duration

example for 15 minutes, ie 15*60

```json
"rules": [
		{
				"isMultipleOf": 900
		}
]
```
