# Not Documented Action Response

⚠️ this following api could change, it could help to implement some behaviour in fully online app, but need server side code


## openURL

In "on mobile app action"

```4d
Case of
: ($action.name="action"0)
  $0.openURL:="https://www.google.com"

```

### VCard example

```4d
Case of
: ($action.name="exportVcard")
  $contact:=$1.context.entity.primaryKey
  $0.openURL:="https://myserver/4DAction/vCardExport/?id="+String(contact)

```

Then implement the vcard export in your web endpoint, for instance 4DAction `vCardExport` method

You could use [PIM](https://github.com/mesopelagique/PIM) to create a vcard from your data and according to contact primary key, and send it to mobile app which open the url
