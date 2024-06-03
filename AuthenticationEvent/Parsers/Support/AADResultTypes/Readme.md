```
    _      _   ___       
   /_\    /_\ |   \      
  / _ \  / _ \| |) |     
 /_/_\_\/_/ \_\___/ _    
 | _ \___ ____  _| | |_  
 |   / -_|_-< || | |  _| 
 |_|_\___/__/\_,_|_|\__| 
 |_   _|  _ _ __  ___ ___
   | || || | '_ \/ -_|_-<
   |_| \_, | .__/\___/__/
       |__/|_|           
```

</br>

## About

### Metadata
| Detail | Data 
| - | - |
| Version | 1.0.0 
| Type | Support
| Schema | Authentication
| Schema Version | 0.1.3

### Information
This is a function that defines a table containing authentication error codes and information surrounding the code. This table will populate the fields, **EventType**, **EventResult**,  **EventResultDetails**, **EventOriginalResultDetails** & **EventSeverity** based on the **ResultType** that is used during the [lookup](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/lookup-operator) operation.

### Requirements
- None

</br>

## Deployment
![Deploy to Azure](https://aka.ms/deploytoazurebutton)

</br>

## Usage
This function is intended to be used within another function. To deploy this function within your own function, simply use the [lookup operator](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/lookup-operator) to lookup an authentication events 'ResultType' field on this table. The value from the ResultType field of the authentication tables will be corresponded to ResultType field within the table and the associated data will be returned.

### Function Name
Use `AADResultTypes` to call this parser.

### Example

```
let my_function = (
    parameter:type = "foo"
) {
    SigninLogs
    | lookup AADResultTypes on ResultType
};
my_function(parameter)
```
</br>