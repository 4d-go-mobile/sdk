 # call By Code ActionRequest

Experimental code to launch action request v19 if done by a custom graphique interface

```swift

import QMobileAPI
import QMobileUI
import Combine
         
...

class ...
  ...

  func sampleCode () { 
        let actionName = "myAction"
        let parametersValues: ActionParameters? = ["aKey": 5, "AnOtherKey": "value"]
        
        let contextParameters: ActionParameters? = self.actionContext()?.actionContextParameters() // if in detail or list form
        // ... contains for instance [ActionParametersKey.table: "maTable"]

        // you could create a Combine publisher to do somestuff in your graphic interface when action result is done before lauching default action result management (synchro, statusText, ...)
        let waitUI: AnyPublisher<Void, Never> = Empty(completeImmediately: true).eraseToAnyPublisher()


        // Do the action request:

        let action = Action(name: actionName) // no parameter to not allow edit
        let request = ActionRequest(action: action, actionParameters: parametersValues, contextParameters: contextParameters, id: ActionRequest.generateID(action) /* put empty id if you do not want offline*/)
        ActionManager.instance.executeActionRequest(request, waitPresenter: waitUI) { result in
            switch result {
            case .success(let actionResult):
                print("\(actionResult)")
            case .failure(let error):
                print("\(error)")
            }
        }
  }
```  
        
