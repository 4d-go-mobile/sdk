# Logger

A logger is an object to log/trace a message.

A message is accociated to a level to allow filtering of important or not information.

## Level

* 0 for verbose
* 1 for debug
* 2 for info (default)
* 3 warning
* 4 error
* 5 severe

### How to change it?

log.level = a number    in Settings.plist for first start

or in runtime with debug interface activated by going to app external settings and returning to the app

## In code

We use framework https://github.com/DaveWoodCom/XCGLogger

```swift
logger.info("my info message")
logger.warning("a warning message")
// etc...
```

## Stored log

In Settings.plist there is property log.writeToFile to defined log name. default debug.log

### auto rotating

if a log is bigger than maxSize (default 1_048_576 ) , the file is splitted and suffixed by date

maxSize could be customized with log.maxFileSize

If there is too much log, we remove it. log.maxLogFiles (default: 10 )

Desactivate autorotage log.autorotate = false

## log to apple log

log.appleSystem = YES in Settings.plist

## Formatting 

By default when using Xcode there is emoticon

### Configure

log.formatter = emoticon in Settings.plist

### emoticon

```swift
 prefixes[.verbose] = "🗯"
 prefixes[.debug] = "🔹"
 prefixes[.info] = "ℹ️"
 prefixes[.warning] = "⚠️"
 prefixes[.error] = "‼️"
 prefixes[.severe] = "💣"
                
```

### ansi

no prefix

### circle

```swift
                prefixes[.verbose] = "🔘"
                prefixes[.debug] = "🔵"
                prefixes[.info] = "⚪"
                prefixes[.warning] = "☢️"
                prefixes[.error] = "🔴"
                prefixes[.severe] = "⚫"
```

## message customizing in settings.plist

log.showThreadName  
log.showLevel 
log.showFileNames
log.showLineNumbers 
log.showFunctionName
log.showDate 
