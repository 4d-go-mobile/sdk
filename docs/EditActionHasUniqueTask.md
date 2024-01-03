#  EditActionHasUniqueTask

> [!CAUTION]
> Not released, just a specification

- The purpose is to make any task for an action with the preset `edit` to be unique :
  - If offline, an edit could be in pending task
  - If launch a new edit, instead of creating a new task, the one in pending task will be opened
    - so data already filled will be displayed
    - and when validating, just register the new data (no new task in pending task list) + waiting to network to come back to send the data to server

# Setting to activate the feature

##  MobileProject

> 🚧 how to inject inside mobile app for new projet? (have a boolean inside mobileapp file, and "4d mobile app" + android project generator must do the following edit

##  Android

Inside `app/src/main/assets/appInfo.json`

```json
	"action.edit.hasUniqueTask": true
```

##  iOS

Inside `Settings/Settings.plist`

```xml
	<key>action.edit.hasUniqueTask</key>
	<true/>
```

## If we want to go further

### for all type of action (doable quickly)

> :bulb: We could imagine have some definition by action, not only "edit" one. For that for instance a new boolean `hasUniqueTask` could be in the future added to the action json (or if we have time)

#### for iOS

in storyboards definition inside `userDefinedRuntimeAttribute`

#### for android

Inside `app/src/main/assets/actions.json` by action

#### allow to set action task unique in mobile project editor (will not do it)

adding for all in mobile project editor a way to edit project to set true or false by action
