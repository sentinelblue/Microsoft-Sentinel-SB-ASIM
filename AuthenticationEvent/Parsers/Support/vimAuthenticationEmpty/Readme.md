```
  ___            _                                        
 | __|_ __  _ __| |_ _  _                                 
 | _|| '  \| '_ \  _| || |                                
 |___|_|_|_| .__/\__|\_, |                                
    _      |_|  _    |__/    _   _         _   _          
   /_\ _  _| |_| |_  ___ _ _| |_(_)__ __ _| |_(_)___ _ _  
  / _ \ || |  _| ' \/ -_) ' \  _| / _/ _` |  _| / _ \ ' \ 
 /_/_\_\_,_|\__|_||_\___|_||_\__|_\__\__,_|\__|_\___/_||_|
 | _ \__ _ _ _ ___ ___ _ _                                
 |  _/ _` | '_(_-</ -_) '_|                               
 |_| \__,_|_| /__/\___|_|                                 
                                                         
```

## About

### Metadata
| Detail | Data 
| - | - |
| Version | 1.1.0 
| Type | Support
| Schema | Authentication
| Schema Version | 0.1.3

### Information
This parser contains a data table that contains each column in the ASIM authentication schema. The parser is used to initialize schema in unifying parsers.

### Requirements
N/A

### Documentation
[Asim Authentication Schema](https://learn.microsoft.com/en-us/azure/sentinel/normalization-schema-authentication)  
[Asim Common Schema](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields)

## Deployment

![Deploy to Azure](https://aka.ms/deploytoazurebutton)

## Usage
To use this parser, simply call it within the kql terminal.

### Function Name
Use `vimAuthenticationEmpty` to call this parser.

### Examples
```
vimAuthenticationEmpty
| getschema
```

```
union 
  vimAuthenticationEmpty,
  vimAuthenticationSigninLogs
  vimAuthenticationAADNonInteractiveSignInLogs
```