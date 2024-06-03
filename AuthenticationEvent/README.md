# Authentication Event Parser

```text
    _       _   _            _   _         _   _          
   /_\ _  _| |_| |_  ___ _ _| |_(_)__ __ _| |_(_)___ _ _  
  / _ \ || |  _| ' \/ -_) ' \  _| / _/ _` |  _| / _ \ ' \ 
 /_/_\_\_,_|\__|_||_\___|_||_\__|_\__\__,_|\__|_\___/_||_|
 | __|_ _____ _ _| |_                                     
 | _|\ V / -_) ' \  _|                                    
 |___|\_/\___|_||_\__|                                    
 | _ \__ _ _ _ ___ ___ _ _                                
 |  _/ _` | '_(_-</ -_) '_|                               
 |_| \__,_|_| /__/\___|_|                                 
                                                         
```

## About

| Detail | Data
| - | - |
| Schema | Authentication
| Schema Version | 0.1.3

### Information

This is an [Azure Security Information Model](https://learn.microsoft.com/en-us/azure/sentinel/normalization) based Authentication Event Parser for [Microsoft Sentinel](https://azure.microsoft.com/en-us/products/microsoft-sentinel). This implementation retains the functionality of the [parsers provided by Microsoft](https://github.com/Azure/Azure-Sentinel/tree/master/Parsers/ASimAuthentication/Parsers) while increasing the amount of normalized data & the utility of the parser.

### Data Sources

These are the current data sources that this parser normalizes.

| Data Source | Provider | Description
| - | - | - |
| [AADServicePrincipalSignInLogs](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/aadserviceprincipalsigninlogs) | Microsoft | Authentication events related to Service Prinicpals.
| [AADManagedIdentitySignInLogs](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/aadmanagedidentitysigninlogs) | Microsoft| Authentication events related to Managed Identity.
| [SigninLogs](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/signinlogs) | Microsoft| Entra ID interactive user authentication events.
| [AADNonInteractiveUserSignInLogs](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/aadnoninteractiveusersigninlogs) | Microsoft | Entra ID non-interactive user events.

### Requirements

- A Microsoft Sentinel Instance
- See [imAuthentication](/AuthenticationEvent/Parsers/Unifying/imAuthentication#Requirements) for requirements related to the parsers code.

### Documentation

[Asim Authentication Schema](https://learn.microsoft.com/en-us/azure/sentinel/normalization-schema-authentication)  
[Asim Common Schema](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields)

</br>

## Deployment

![Deploy to Azure](https://aka.ms/deploytoazurebutton)

</br>

## Highlights

The items listed below highlight the improvements that have been made to the parser over what is currently offered by Microsoft. These items are implemented in each sub-parser, meaning any parser within this repository will use the features listed below.

### Greater amount of parameters for filtering events

The original parameters provided by Microsoft while adding additional parameters to the parser for increased precision when analyzing events.

| New Parameter | Description |
| - | - |
| user_type | Filter on the type of user that has authenticated.
| user_id | Filter on the GUID of the user object.
| ip | Filter on IP's associated with events.
| dvc_id | Filter on the unique id of a device recognized by azure.
| dvc_os | Filter on the operating system of the device used to perform the authentication.
| dvc_type | Filter on the type of device used to perform the authentication.
| useragent | Filter on the user agent of the device used to perform the authentication.
| eventseverity | Filter on the severity level associated with the event.
| eventproduct | Filter on the product that logged at the event.
| pivot_lookback | Sets the lookback time in the pivot queries provided in each event. Defaults to a value of 30 days.

</br>

### Increased data normalization

More data from the source has been normalized into content that is useful to the analyst viewing the data. Highlights would include information about the Device & MFA Method.

</br>

f
</br>

### Pivot Queries

The **Pivots** field is a field introduced by Sentinel Blue. This field contains sub queries that can be used by the analyst to gather further information about an entity such as IP reputation or User Account authentication history, allowing an analyst to gather data quickly and efficiently.

</br>

![Pivot Queries](/AuthenticationEvent/Images/pivots.png)

</br>

### Centralized error codes with translations for 'Other' descriptions

All of the Entra ID related error codes have been centralized into a lookup table, [AADResultTypes](/AuthenticationEvent/Parsers/Support/AADResultTypes/AADResultTypes.yaml). This table provides information about each authentication result code. For result codes that have a description of 'Other', the meaning of the code has been deciphered and replaced with a description explaining the code.

</br>

![Descriptions](/AuthenticationEvent/Images/eventmessage.png)

</br>

### Co-compatibility with Microsoft alert rules

This parser is intended to be compatible with alert rules provided by Microsoft that are using the imAuthentication parser on schema version 0.1.3.

</br>

### Time saved with ordered ouptut

Through the use of [project-reorder](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/project-reorder-operator), the ordering of the fields has been adjusted to quickly put eyes on key pieces of data such as the user account, ip, device & logon target. The output is also ordered based on the time generated. With one line of code, the parser function can be called and meaning full data presented. This cuts out the work of having to manually order your data or sift through unordered data with each new query.

</br>

![project-reorder](/AuthenticationEvent/Images/reorder.png)

</br>

### Preserve source specific data with Additional Details

The [additional details](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields#other-fields) field is a special field in the [common schema](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields) that allows for unnormalized source specific data to be retained. This field acts like a storage unit where unnormalized data about the event is stored without disrupting the visibility of the normalized data.  

</br>

### Event severity dictated by risk level

Within the Interactive and Non-interactive parsers for Entra ID, the **EventSeverity** column has been normalized based on the aggregated level of risk assigned to the event by Microsofts machine learning / artificial intelligence model. In combination with the **eventseverity** parameter, this allows analysts to quickly locate events deemed risky -> `imAuthentication(starttime=ago(180d), eventseverity='high')`.

</br>

## Usage

### Function Name

To access this parser within a kql editor, use `imAuthentication()`.

The imAuthentication parser & all source specific parsers have the same parameters. See [imAuthentication](/AuthenticationEvent/Parsers/Unifying/imAuthentication#Usage) for further information about using the parser.
