#  EditActionHasUniqueTask

> [!CAUTION]
> Not released, just a specification

- The purpose is to make any task for an action with the `preset` key equal to `edit` to be unique :
  - If offline, an edit could be in pending task after validating the action form
  - If launch a new edit, instead of creating a new task, the one in pending task will be opened
    - so data already filled will be displayed
    - and when validating, just register the new data into selected/found task
	- no new task in pending task list
  	- waiting on network connection to send the data to server (inside on mobile app action)

# Setting to activate the feature

The feature is not for all, so a conf in 

##  MobileProject

Inside `Mobile Projects/<project name>/project.4dmobileapp` add this to activate the feature

```json
	"extra": {
		"editActionHasUniqueTask": true
	}
}
```

⚠️ project must be close, and reopened

##  Android

Inside `app/src/main/assets/appInfo.json` add this to activate the feature for an already generated app

```json
	"action.edit.hasUniqueTask": true
```

##  iOS

Inside `Settings/Settings.plist` add this to activate the feature for an already generated app

```xml
	<key>action.edit.hasUniqueTask</key>
	<true/>
```

## If we want to go further

### for all type of action (doable quickly)

We could imagine have some definition by action, not only "edit" one. For that for instance a new boolean `hasUniqueTask` could be added to the action json

#### for android

Inside `app/src/main/assets/actions.json` by action you could add `hasUniqueTask` to true

#### for iOS

in storyboards definition inside `userDefinedRuntimeAttribute`, find your action and edit in the one line json, the `hasUniqueTask` to true

#### allow to set action task unique in mobile project editor (IMPORTANT: will not do it)

adding for all in mobile project editor a way to edit project to set true or false by action ie. a checkbox
