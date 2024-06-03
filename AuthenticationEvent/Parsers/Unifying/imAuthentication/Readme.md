```
    _       _   _            _   _         _   _          
   /_\ _  _| |_| |_  ___ _ _| |_(_)__ __ _| |_(_)___ _ _  
  / _ \ || |  _| ' \/ -_) ' \  _| / _/ _` |  _| / _ \ ' \ 
 /_/_\_\_,_|\__|_||_\___|_||_\__|_\__\__,_|\__|_\___/_||_|
 | _ \__ _ _ _ ___ ___ _ _                                
 |  _/ _` | '_(_-</ -_) '_|                               
 |_| \__,_|_| /__/\___|_|                                                                    
```

</br>

## About

### Metadata
| Detail | Data 
| - | - |
| Version | 2.0.0 
| Type | Unifying
| Schema | Authentication
| Schema Version | 0.1.3

### Information
This is a unifying parser for the authentication schema. This parser will aggregate each source specific parser and present the normalized authentication events from each data source with a corresponding source specific parser.

### Requirements
| Dependency | Type | Version | Source | Deploy
| - | - | - | - | - |
| [vimAuthenticationEmpty](/AuthenticationEvent/Parsers/Support/vimAuthenticationEmpty/vimAuthenticationEmpty.yaml) | Support | 1.1.0 | Sentinel Blue | ![Deploy to Azure](https://aka.ms/deploytoazurebutton)
| [vimAuthenticationAADManagedIdentitySignInLogs](/AuthenticationEvent/Parsers/Source%20Specific/vimAuthenticationAADManagedIdentitySignInLogs/vimAuthenticationAADManagedIdentitySignInLogs.yaml) | Source Specific | 1.0.0 | Sentinel Blue | ![Deploy to Azure](https://aka.ms/deploytoazurebutton)
| [vimAuthenticationAADNonInteractiveUserSignInLogs](/AuthenticationEvent/Parsers/Source%20Specific/vimAuthenticationAADNonInteractiveUserSignInLogs/vimAuthenticationAADNonInteractiveUserSignInLogs.yaml) | Source Specific | 2.0.0 | Sentinel Blue | ![Deploy to Azure](https://aka.ms/deploytoazurebutton)
| [vimAuthenticationAADServicePrincipalSignInLogs](/AuthenticationEvent/Parsers/Source%20Specific/vimAuthenticationAADServicePrincipalSignInLogs/vimAuthenticationAADServicePrincipalSignInLogs.yaml) | Source Specific | 1.0.0 | Sentinel Blue | ![Deploy to Azure](https://aka.ms/deploytoazurebutton)
| [vimAuthenticationSigninLogs](/AuthenticationEvent/Parsers/Source%20Specific/vimAuthenticationSigninLogs/vimAuthenticationSigninLogs.yaml) | Source Specific | 2.0.0 | Sentinel Blue | ![Deploy to Azure](https://aka.ms/deploytoazurebutton)

### Documentation
[Asim Authentication Schema](https://learn.microsoft.com/en-us/azure/sentinel/normalization-schema-authentication)  
[Asim Common Schema](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields)

</br>

## Deployment

![Deploy to Azure](https://aka.ms/deploytoazurebutton)

</br>

## Usage
### Function Name
Use `imAuthentication()` to call this function in the log analytics workspace. 

### Parameters
| Name | Type | Filters on Field | Description
| - | - | - | - |
| starttime | Datetime | TimeGenerated | The starting of the timeline of events to capture.
| endtime | Datetime | TimeGenerated | The end time of the timeline of events to capture.
| username_has_any | Dynamic | UserPrincipalName | Filter events based on the name of the managed identity.
| targetappname_has_any | Dynamic | ResourceDisplayName | Filter on the items that were authenticated to.
| srcipaddr_has_any_prefix | Dynamic | DvcIpAddr | Filter on events with IP's matching the provided prefix.
| srchostname_has_any | Dynamic | DvcHostname | Filter on the name of the device that the authentication event was performed from. 
| eventtype_in | Dynamic | EventType | The type of authentication event. (Logon / Logoff).
| eventresultdetails_in | Dynamic | EventResultDetails | Filter based on the normalized message provided by the authentication event.
| eventresult | String | EventResult | Filter on the result of the event. (Success / Failure).
| user_type | Dynamic | UserType | Filter on the type of user that has authenticated.
| user_id | Dynamic | UserId | Filter on the GUID of the user object.
| ip | Dynamic | DvcIpAddr | Filter on IP's associated with events.
| dvc_id | Dynamic | DvcId | Filter on the unique id of a device recognized by azure.
| dvc_os | Dynamic | DvcOs | Filter on the operating system of the device used to perform the authentication.
| dvc_type | Dynamic | SrcDeviceType | Filter on the type of device used to perform the authentication.
| useragent | Dynamic | UserAgent | Filter on the user agent of the device used to perform the authentication.
| eventseverity | Dynamic | EventSeverity | Filter on the severity level associated with the event.
| eventproduct | Dynamic | EventProduct | Filter on the product that logged at the event.
| pivot_lookback | String | N/A | Sets the lookback time in the pivot queries provided in each event. Defaults to a value of 30 days. 
| disabled | bool | N/A | Used to disable this parser in calling parent parsers.

</br>

### Examples

#### Filtering on singular entities
It is possible to filter on a string without specifically creating a dynamic even though the parameter calls for a dynamic.
1. `imAuthentication(starttime=todatetime('2024-01-01'), endtime=todatetime('2024-04-01'))`
2. `imAuthentication(starttime=ago(30d), username_has_any='heather@company.com', targetappname_has_any='Exchange')`
3. `imAuthentication(starttime=ago(1d), dvc_id='62065df0-fd57-4edf-8979-888d61c479f4', pivot_lookback='180')`
4. `imAuthentication(starttime=ago(7d), user_type='member', eventseverity='high')`
5. `imAuthentication(starttime=ago(90d), username_has_any='admin', eventproduct='entra', ip='105.23.83.192')`
6. `imAuthentication(starttime=ago(90d), srchostname_has_any='DC-01')`
7. `imAuthentication(starttime=ago(90d), eventresultdetails_in='locked', eventresult='failure')`

#### Filtering on multiple entities
By using a dynamic, the parameters will filter on any of the values listed in the dynamic.
1. `imAuthentication(starttime=todatetime('2024-01-01'), endtime=todatetime('2024-04-01'), srchostname_has_any=dynamic(['DC-01', 'DC-02', 'App-Server', 'File-Server']), username_has_any=dynamic(['admin@company.com', 'martha@company.com', 'kyle@company.com', 'tameka@company.com']))`
2. `imAuthentication(starttime=ago(30d), dvc_os=dynamic(['ios', 'linux']), user_type=dynamic(['guest', 'member']))`
3. `imAuthentication(starttime=ago(1d), ip=dynamic(['182.17.1.23', '93.38.12.3', '71.4.31.100']), user_id=dynamic(['e804bf41-aa51-481d-8af0-49935c79ac5a', 'b2ba3a5b-1674-49f0-8b25-e0e90bc5fefe']))`
4. `imAuthentication(starttime=ago(90d), eventseverity=dynamic(['medium', 'high']))`
5. `imAuthentication(starttime=ago(180d), endtime=ago(90d), pivot_lookback='180', eventproduct='azure', targetappname_has_any=dynamic(['Key vault', 'Azure']), username_has_any=dynamic(['sp-storage', 'sp-privileged_identity_management']))`

</br>

## Field Mappings & Definitions
For the field mapping, see the mapping in each source specific parsers documentation.
- [Managed Identity](/AuthenticationEvent/Parsers/Source%20Specific/vimAuthenticationAADManagedIdentitySignInLogs#field-mappings--definitions)
- [Service Principal](/AuthenticationEvent/Parsers/Source%20Specific/vimAuthenticationAADServicePrincipalSignInLogs/#field-mappings--definitions)
- [Interactive Signin Logs](/AuthenticationEvent/Parsers/Source%20Specific/vimAuthenticationSigninLogs#field-mappings--definitions)
- [Non-interactive Signin Logs](/AuthenticationEvent/Parsers/Source%20Specific/vimAuthenticationAADNonInteractiveUserSignInLogs#field-mappings--definitions)

</br>