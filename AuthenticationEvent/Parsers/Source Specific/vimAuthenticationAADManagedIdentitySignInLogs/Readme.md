```
  __  __                            _ 
 |  \/  |__ _ _ _  __ _ __ _ ___ __| |
 | |\/| / _` | ' \/ _` / _` / -_) _` |
 |_|  |_\__,_|_||_\__,_\__, \___\__,_|
  ___    _         _   |___/          
 |_ _|__| |___ _ _| |_(_) |_ _  _     
  | |/ _` / -_) ' \  _| |  _| || |    
 |___\__,_\___|_||_\__|_|\__|\_, |    
 | _ \__ _ _ _ ___ ___ _ _   |__/     
 |  _/ _` | '_(_-</ -_) '_|           
 |_| \__,_|_| /__/\___|_|                                                
```


</br>

## About

### Metadata
| Detail | Data 
| - | - |
| Version | 1.0.0 
| Type | Source Specific
| Schema | Authentication
| Schema Version | 0.1.3

### Information
This is a source specific parser for the [AADManagedIdentitySignInLogs](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/aadmanagedidentitysigninlogs) table.

### Requirements
| Dependency | Type | Version | Source | Deploy
| - | - | - | - | - |
| AADManagedIdentitySignInLogs | Table | N/A | Microsoft
| [AADResultTypes](/AuthenticationEvent/Parsers/Support/AADResultTypes/AADResultTypes.yaml) | Support Parser | 1.0.0 | Sentinel Blue | ![Deploy to Azure](https://aka.ms/deploytoazurebutton)

### Documentation
[AADManagedIdentitySignInLogs Table Reference](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/aadmanagedidentitysigninlogs)  
[Asim Authentication Schema](https://learn.microsoft.com/en-us/azure/sentinel/normalization-schema-authentication)  
[Asim Common Schema](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields)

</br>

## Deployment
![Deploy to Azure](https://aka.ms/deploytoazurebutton)

</br>

## Usage
### Function Name
Use `vimAuthenticationAADManagedIdentitySignInLogs()` to call this function in the log analytics workspace. 

### Parameters

| Name | Type | Filters on Field | Description
| - | - | - | - |
| starttime | Datetime | TimeGenerated | The starting of the timeline of events to capture.
| endtime | Datetime | TimeGenerated | The end time of the timeline of events to capture.
| username_has_any | Dynamic | ServicePrincipalName | Filter events based on the name of the managed identity.
| targetappname_has_any | Dynamic | ResourceDisplayName | Filter on the items that were authenticated to.
| srcipaddr_has_any_prefix | Dynamic | IPAddress | Filter on events with IP's matching the provided prefix.
| eventtype_in | Dynamic | EventType | The type of authentication event. (Logon / Logoff).
| eventresultdetails_in | Dynamic | EventResultDetails | Filter based on the normalized message provided by the authentication event.
| eventresult | String | EventResult | Filter on the result of the event. (Success / Failure).
| user_type | Dynamic | TargetUserType | Filter on the type of user that has authenticated.
| user_id | Dynamic | ServicePrincipalId | Filter on the GUID of the user object.
| ip | Dynamic | IPAddress | Filter on IP's associated with events.
| eventseverity | Dynamic | EventSeverity | Filter on the severity level assosciated with the event.
| eventproduct | Dynamic | EventProduct | Filter on the product that logged at the event.
| pivot_lookback | String | N/A | Sets the lookback time in the pivot queries provided in each event. Defaults to a value of 30 days. 
| disabled | bool | N/A | Used to disable this parser in calling parent parsers.
| srchostname_has_any | Dynamic | N/A | Parameter not available in this parser.
| dvc_id | Dynamic | N/A | Parameter not available in this parser.
| dvc_os | Dynamic | N/A | Parameter not available in this parser.
| dvc_type | Dynamic | N/A | Parameter not available in this parser.
| useragent | Dynamic | N/A | Parameter not available in this parser.

</br>

### Examples

#### Filtering on singular entities
It is possible to filter on a string without specifically creating a dynamic even though the parameter calls for a dynamic.

1. `vimAuthenticationAADManagedIdentitySignInLogs(starttime=ago(7d), eventresult='Failure', pivot_lookback='90')`
2. `vimAuthenticationAADManagedIdentitySignInLogs(starttime=ago(90d), username_has_any='MI-StorageAccount', eventresult='success', ip='2607:fb91:2da3:4909:e09f:93f0:5337:9cca')`
3. `vimAuthenticationAADManagedIdentitySignInLogs(starttime=todatetime('2024-04-01'), endtime=todatetime('2024-04-30'), targetappname_has_any='key vault', user_id='c06e0b26-059f-4644-a696-b1fa8ee2a307')`

#### Filtering on multiple entities
By using a dynamic, the parameters will filter on any of the values listed in the dynamic.

1. `vimAuthenticationAADManagedIdentitySignInLogs(starttime=ago(1d), username_has_any=dynamic(['SP-Account01', 'SP-Account-02']))`
2. `vimAuthenticationAADManagedIdentitySignInLogs(starttime=ago(1d), ip=dynamic(['89.231.38.73', '31.11.34.253', '173.192.3.38']), targetappname_has_any=dynamic(['Log Analytics API', 'Azure Key Value']))`

</br>

## Field Mappings & Definitions
This section describes how each field within the schema is normalized from the Managed Identity signin logs source. 

</br>

### Statically Defined Fields
These fields are not derived from other columns. They have been assigned static values that will remain consistent in each row returned by the parser.

| Field | Value | Class | Type | Schema | Description  
| - | - | - | - | - | - |
| EventProduct | Entra ID | Mandatory | String | Common | The product generating the event. The value should be one of the values listed in [Vendors and Products](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields#vendors-and-products).
| EventSchema | Authentication | Mandatory | String | Common | The schema the event is normalized to. Each schema documents its schema name.
| EventSchemaVersion | 0.1.3 | Mandatory | String | Common | The version of the schema. Each schema documents its current version. 
| EventVendor | Microsoft | Mandatory | String | Common | The vendor of the product generating the event. The value should be one of the values listed in [Vendors and Products](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields#vendors-and-products).
| EventCount | 1 | Mandatory | Integer | Common | The number of events described by the record.
| TargetAppType | Resource | Optional | AppType | Authentication | The type of the application authorizing on behalf of the Actor. For more information, and allowed list of values, see [AppType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#apptype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas).
| TargetUserType | Application | Optional | UserType | Authentication | The type of the Target user. For more information, and list of allowed values, see [UserType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#usertype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#).
| LogonMethod | Managed Identity | Optional | String | Authentication | The method used to perform authentication.
| TargetUsernameType | Simple | Conditional | UsernameType | Authentication | Specifies the type of the username stored in the TargetUsername field. For more information and list of allowed values, see [UsernameType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#usernametype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#).
| TargetUserIdType | EntraID | Conditional | UserIdType | Authentication | The type of the user ID stored in the TargetUserId field. For more information and list of allowed values, see [UserIdType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#useridtype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#). 
| EventSubType | Service | Optional | Enumerated | Common | The sign-in type. Allowed values include: `System`, `Interactive`, `RemoteInteractive`, `Service`, `RemoteService`, `Remote`, `AssumeRole`. The value may be provided in the source record using different terms, which should be normalized to these values. The original value should be stored in the field EventOriginalSubType.

</br>

### Fields Defined From AADManagedIdentitySignInLogs Table
These fields are derived from fields within the [AADManagedIdentitySignInLogs](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/aadmanagedidentitysigninlogs) table.

| ASIM Field | Table Field | Class | Type | Schema | Description  
| - | - | - | - | - | - |
| EventType | ResultType/[AADResultTypes](/AuthenticationEvent/Parsers/Support/AADResultTypes/AADResultTypes.yaml) | Mandatory | String | Common | Describes the operation reported by the record. Each schema documents the list of values valid for this field. The original, source specific, value is stored in the EventOriginalType field. For Authentication records, supported values include: `Logon`, `Logoff`, `Elevate`
| EventOriginalSubType | Type | Optional | String | Common | The original event subtype or ID, if provided by the source. For example, this field is used to store the original Windows logon type. This value is used to derive EventSubType, which should have only one of the values documented for each schema.
| EventResult | ResultType/[AADResultTypes](/AuthenticationEvent/Parsers/Support/AADResultTypes/AADResultTypes.yaml) | Mandatory | Enumerated | Common | One of the following values: `Success`, `Partial`, `Failure`, `NA` (Not Applicable). The value might be provided in the source record by using different terms, which should be normalized to these values. Alternatively, the source might provide only the EventResultDetails field, which should be analyzed to derive the EventResult value.
| EventSeverity | ResultType/[AADResultTypes](/AuthenticationEvent/Parsers/Support/AADResultTypes/AADResultTypes.yaml) | Recommended | Enumerated | Common | The severity of the event. Valid values are: `Informational`, `Low`, `Medium`, or `High`.
| EventResultDetails | ResultType/[AADResultTypes](/AuthenticationEvent/Parsers/Support/AADResultTypes/AADResultTypes.yaml) | Recommended | Enumerated | Common | The details associated with the event result. This field is typically populated when the result is a failure. Allowed values include: `No such user or password`, `No such user`, `Incorrect password`, `Incorrect key`, `Account expired`, `Password expired`, `User locked`, `User disabled`, `Logon violates policy`, `Session expired`, `Other`. The value may be provided in the source record using different terms, which should be normalized to these values. The original value should be stored in the field EventOriginalResultDetails
| EventOriginalType | ResultType | Optional | String | Common | The original event type or ID, if provided by the source. For example, this field is used to store the original Windows event ID. This value is used to derive EventType, which should have only one of the values documented for each schema. Example: `4624`
| EventOriginalResultDetails | ResultType/[AADResultTypes](/AuthenticationEvent/Parsers/Support/AADResultTypes/AADResultTypes.yaml) | Optional | String | Common | The original result details provided by the source. This value is used to derive EventResultDetails, which should have only one of the values documented for each schema.
| ActingAppId | AppId | Optional | String | Authentication | The ID of the application authorizing on behalf of the actor, including a process, browser, or service.
| TargetAppId | ResourceIdentity | Optional | String | Authentication | The ID of the application to which the authorization is required, often assigned by the reporting device. 
| TargetAppName | ResourceDisplayName | Optional | String | Authentication | The name of the application to which the authorization is required, including a service, a URL, or a SaaS application.
| TargetUsername | ServicePrincipalName | Optional | Username | Authenication | The target user username, including domain information when available. For more information, see The User entity.
| TargetUserId | ServicePrincipalId | Optional | UserId | Authentication | A machine-readable, alphanumeric, unique representation of the target user. For more information, and for alternative fields for additional IDs, see [The User entity](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#the-user-entity).
| EventOriginalUid | Id | Optional | String | Common | A unique ID of the original record, if provided by the source. Example: `69f37748-ddcd-4331-bf0f-b137f1ea83b`
| TargetSessionId | CorrelationId | Optional | String | Authentication | The sign-in session identifier of the TargetUser on the source device.
| SrcIpAddr | IPAddress | Optional | IP Address | Authentication | The IP address of the source device.
| EventUid | _ItemId | Recommended | String | Common | The unique ID of the record, as assigned by Microsoft Sentinel. This field is typically mapped to the `_ItemId` Log Analytics field. Example: `69f37748-ddcd-4331-bf0f-b137f1ea83b`
| EventProductVersion | OperationVersion | Optional | String | Common | The version of the product generating the event.

</br>

</br>

### Aliases
These fields create another name for a field that has already been normalized. 

| Alias | Field | Class | Type | Schema | Description
| - | - | - | - | - | - |
| EventStartTime | TimeGenerated | Mandatory | Datetime | Common | The time in which the event started. If the source supports aggregation and the record represents multiple events, the time that the first event was generated. If not provided by the source record, this field aliases the [TimeGenerated](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields#timegenerated) field.
| EventEndTime | TimeGenerated | Mandatory | Datetime | Common | The time in which the event ended. If the source supports aggregation and the record represents multiple events, the time that the last event was generated. If not provided by the source record, this field aliases the [TimeGenerated](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields#timegenerated) field.
| EventMessage | EventOriginalResultDetails | Optional | String | Common | A general message or description, either included in or generated from the record.
| DvcIpAddr | SrcIpAddr | Recommended | IP address | Common | The IP address of the device on which the event occurred or which reported the event, depending on the schema.
| DvcHostname | Dvc | Optional | String | Common | The hostname of the device on which the event occurred or which reported the event, depending on the schema. Example: `ContosoDc`
| SrcHostname | Dvc | Recommended | Hostname | Authentication | The source device hostname, excluding domain information. If no device name is available, store the relevant IP address in this field.
| User | TargetUsername | Alias | Username | Authentication | Alias to the TargetUsername or to the TargetUserId if TargetUsername is not defined.
| LogonTarget | TargetAppName | Alias | String | Authentication | Alias to either TargetAppName, TargetUrl, or TargetHostname, whichever field best describes the authentication target.
| Dst | TargetAppName | Alias | String | Authentication | A unique identifier of the authentication target. This field may alias the TargerDvcId, TargetHostname, TargetIpAddr, TargetAppId, or TargetAppName fields.
| Src | SrcIpAddr | Recommended | String | Authentication | A unique identifier of the source device. This field may alias the SrcDvcId, SrcHostname, or SrcIpAddr fields.
| IpAddr | SrcIpAddr | Alias | String | Authentication | Alias to SrcIpAddr
| Dvc | EventVendor / EventProduct | Alias | String | Common | A unique identifier of the device on which the event occurred or which reported the event, depending on the schema. This field might alias the DvcFQDN, DvcId, DvcHostname, or DvcIpAddr fields. For cloud sources, for which there is no apparent device, use the same value as the Event Product field.
| UserId | TargetUserId | Optional | String | Authentication | A machine-readable, alphanumeric, unique representation of the user.
| UserType | TargetUserType | Optional | UserType | Authentication | The type of source user. See [UserType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#usertype) for further information.

### Original AADManagedIdentitySignInLogs Fields Stored in AdditionalFields
These are original fields from the AADManagedIdentitySignInLogs table that have been stored in the '[AdditionalFields](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields#other-fields)' column.
| Field | Type | Description
| - | - | - |
| UniqueTokenIdentifier | String | A unique base64 encoded request identifier used to track tokens issued by Azure AD as they are redeemed at resource providers.
| ConditionalAccessPolicies | Dynamic | A list of conditional access policies that are triggered by the corresponding sign-in activity.
| ConditionalAccessStatus | String | The status of the conditional access policy triggered. Possible values: success, failure, or notApplied.
| AuthenticationProcessingDetails | String | Additional authentication processing details, such as the agent name in case of PTA/PHS or Server/farm name in case of federated authentication.
| LocationDetails | Dynamic | Provides the city, state, country/region and latitude and longitude from where the sign-in happened.

</br>

### Custom Fields
#### Pivots Field
The 'Pivots' field contains sub-fields broken down by entity category such as Device, User, IP Address, etc. Each entity field contains a list of sub queries that can be used to gain further information about the entity. The 'pivot_lookback' will adjust how far these queries look back in the records.

| Field | Description |
| - | - |
| User History | Contains pivot queries related to the user entity

##### User History Fields
The User field contains KQL queries to analyze the users authentication history.

| Field | Description |
| - | - |
| All | Query all sign-in activity for the user.
| Application | Query users authentication activity for the target application.

</br>

### Unused Fields
These are fields from the schemas that are not currently used in the parser.

| Field | Class | Type | Schema | Description
| - | - | - | - | - |
| EventOriginalSeverity | Optional | String | Common | The original severity as provided by the reporting device. This value is used to derive EventSeverity.
| EventReportUrl | Optional | String | Common | A URL provided in the event for a resource that provides more information about the event.
| EventOwner | Optional | String | Common | The owner of the event, which is usually the department or subsidiary in which it was generated.
| DvcDomain | Recommended | String | Common | The domain of the device on which the event occurred or which reported the event, depending on the schema.
| DvcDomainType | Conditional | Enumerated | Common | The type of DvcDomain. For a list of allowed values and further information, refer to [DomainType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#domaintype). Note: This field is required if the DvcDomain field is used.
| DvcFQDN | Optional | String | Common | The hostname of the device on which the event occurred or which reported the event, depending on the schema. Note: This field supports both traditional FQDN format and Windows domain\hostname format. The DvcDomainType field reflects the format used.
| DvcDescription | Conditional | Enumerated | Common | A descriptive text associated with the device. For example: `Primary Domain Controller`.
| DvcId | Optional | String | Common | The unique ID of the device on which the event occurred or which reported the event, depending on the schema. Example: `41502da5-21b7-48ec-81c9-baeea8d7d669`
| DvcIdType | Conditional | Enumerated | Common | The type of DvcId. For a list of allowed values and further information, refer to [DvcIdType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields#dvcidtype). Note: This field is required if the DvcId field is used
| DvcMacAddr | Optional | MAC | Common | The MAC address of the device on which the event occurred or which reported the event.
| DvcZone | Optional | String | Common | The network on which the event occurred or which reported the event, depending on the schema. The zone is defined by the reporting device.
| DvcOs | Optional | String | Common | The operating system running on the device on which the event occurred or which reported the event.
| DvcOsVersion | Optional | String | Common | The version of the operating system on the device on which the event occurred or which reported the event.
| DvcAction | Recommended | String | Common | For reporting security systems, the action taken by the system, if applicable.
| DvcOriginalAction | Optional | String | Common | The original DvcAction as provided by the reporting device.
| DvcInterface | Optional | String | Common | The network interface on which data was captured. This field is typically relevant to network related activity, which is captured by an intermediate or tap device.
| DvcScopeId | Optional | String | Common | The cloud platform scope ID the device belongs to. DvcScopeId map to a subscription ID on Azure and to an account ID on AWS.
| DvcScope | Optional | String | Common | The cloud platform scope the device belongs to. DvcScope map to a subscription ID on Azure and to an account ID on AWS.
| AdditionalFields | Optional | Dynamic | Common | If your source provides additional information worth preserving, either keep it with the original field names or create the dynamic AdditionalFields field, and add to it the extra information as key/value pairs.
| ASimMatchingIpAddr | Recommended | String | Common | When a parser uses the `ipaddr_has_any_prefix` filtering parameters, this field is set with the one of the values `SrcIpAddr`, `DstIpAddr`, or `Both` to reflect the matching fields or fields.
| ASimMatchingHostname | Recommended | String | Common | When a parser uses the `hostname_has_any` filtering parameters, this field is set with the one of the values `SrcHostname`, `DstHostname`, or Both to reflect the matching fields or fields.
| LogonProtocol | Optional | String | Authentication | The protocol used to perform the authentication
| ActorUserId | Optional | String | Authentication | A machine-readable, alphanumeric, unique representation of the Actor. For more information, and for alternative fields for additional IDs, see [The User entity](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#the-user-entity).
| ActorScope | Optional | String | Authentication | The scope, such as Microsoft Entra tenant, in which ActorUserId and ActorUsername are defined. or more information and list of allowed values, see [UserScope](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#userscope) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas).
| ActorScopeId | Optional | String | Authentication | The scope ID, such as Microsoft Entra Directory ID, in which ActorUserId and ActorUsername are defined. for more information and list of allowed values, see [UserScopeId](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#userscopeid) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas)
| ActorUserIdType | Coniditional | UserIdType | Authentication | The type of the ID stored in the ActorUserId field. For more information and list of allowed values, see [UserIdType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#useridtype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas)
| ActorUsername | Optional | Username | Authentication | The Actor’s username, including domain information when available. For more information, see The User entity.
| ActorUsernameType | Conditional | UsernameType | Authentication | Specifies the type of the user name stored in the ActorUsername field. For more information, and list of allowed values, see [UsernameType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#usernametype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas).
| ActorUserType | Optional | UserType | Authentication | The type of the Actor. For more information, and list of allowed values, see [UserType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#usertype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas).
| ActorOriginalUserType | Optional | UserType | Authentication | The user type as reported by the reporting device.
| ActorSessionId | Optional | String | Authentication | The unique ID of the sign-in session of the Actor.
| ActingAppName | Optional | String | Authentication | The name of the application authorizing on behalf of the actor, including a process, browser, or service.
| ActingAppType | Optional | AppType | Authentication | The type of acting application. For more information, and allowed list of values, see [AppType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#apptype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas).
| HttpUserAgent | Optional | String | Authentication | When authentication is performed over HTTP or HTTPS, this field's value is the user_agent HTTP header provided by the acting application when performing the authentication. For example: `Mozilla/ 5.0 (iPhone; CPU iPhone OS 12_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.0 Mobile/15E148 Safari/604.1`
| TargetUserId | Optional | UserId | Authentication | A machine-readable, alphanumeric, unique representation of the target user. For more information, and for alternative fields for additional IDs, see [The User entity](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#the-user-entity).
| TargetUserScope | Optional | String | Authentication | The scope, such as Microsoft Entra tenant, in which [TargetUserId](https://learn.microsoft.com/en-us/azure/sentinel/normalization-schema-authentication#targetuserid) and [TargetUsername](https://learn.microsoft.com/en-us/azure/sentinel/normalization-schema-authentication#targetusername) are defined. or more information and list of allowed values, see [UserScope](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#userscope) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas).
| TargetUserScopeId | Optional | String | Authentication | The scope ID, such as Microsoft Entra Directory ID, in which TargetUserId and TargetUsername are defined. for more information and list of allowed values, see [UserScopeId](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#userscopeid) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas).
| TargetOriginalUserType | Optional | UserType | Authentication | The user type as reported by the reporting device.
| SrcDvcId | Optional | String | Authentication | The ID of the source device. If multiple IDs are available, use the most important one, and store the others in the fields `SrcDvc<DvcIdType>`.
| SrcDvcScopeId | Optional | String | Authentication | The cloud platform scope ID the device belongs to. SrcDvcScopeId map to a subscription ID on Azure and to an account ID on AWS.
| SrcDvcScope | Optional | String | Authentication | The cloud platform scope the device belongs to. SrcDvcScope map to a subscription ID on Azure and to an account ID on AWS.
| SrcDvcIdType | Conditional | DvcIdType | Authentication | The type of SrcDvcId. For a list of allowed values and further information refer to [DvcIdType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#dvcidtype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas). Note: This field is required if SrcDvcId is used.
| SrcDeviceType | Optional | DeviceType | Authentication | The type of the source device. For a list of allowed values and further information refer to [DeviceType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#devicetype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas).
| SrcDomain | Recommended | String | Authentication | The domain of the source device.
| SrcDomainType | Conditional | DomainType | Authentication | The type of SrcDomain. For a list of allowed values and further information refer to [DomainType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#domaintype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas).
| SrcFQDN | Optional | String | Authentication | The source device hostname, including domain information when available. Note: This field supports both traditional FQDN format and Windows domain\hostname format. The SrcDomainType field reflects the format used.
| SrcDescription | Optional | String | Authentication | A descriptive text associated with the device. For example: `Primary Domain Controller`.
| SrcPortNumber | Optional | Integer | Authentication | The IP port from which the connection originated.
| SrcDvcOs | Optional | String | Authentication | The OS of the source device. 
| SrcIsp | Optional | Integer | Authentication | The Internet Service Provider (ISP) used by the source device to connect to the internet.
| SrcGeoCountry | Optional | Country | Authentication | The country that the source device logged the event from. For more information, see [Logical types](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#logical-types).
| SrcGeoCity | Optional | City | Authentication | The city in the region that the source device logged the event from. For more information, see [Logical types](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#logical-types).
| SrcGeoRegion | Optional | Region | Authentication | The region in the country that the source device logged the event from 
| SrcGeoLongitude | Optional | Longitude | Authentication | 0.1.3.| The longitude of the source device that logged the event. For more information, see [Logical types](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#logical-types).
SrcGeoLatitude | Optional | Latitude | Authentication | The latitude of the source device that logged the event. For more information, see [Logical types](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#logical-types)
| SrcRiskLevel | Optional | Integer | Authentication | The risk level associated with the source. The value should be adjusted to a range of `0` to `100`, with `0` for benign and `100` for a high risk.
| SrcOriginalRiskLevel | Optional | Integer | Authentication | The risk level associated with the source, as reported by the reporting device.
| TargetUrl | Optional | URL | Authentication | The URL associated with the target application.
| TargetHostname | Recommended | Hostname | Authentication | The target device hostname, excluding domain information.
| TargetDomain | Recommended | String | Authentication | The domain of the target device.
| TargetDomainType | Conditional | Enumerated | Authentication | The type of TargetDomain. For a list of allowed values and further information refer to [DomainType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#domaintype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas). Required if TargetDomain is used.
| TargetFQDN | Optional | String | Authentication | The target device hostname, including domain information when available. Note: This field supports both traditional FQDN format and Windows domain\hostname format. The TargetDomainType reflects the format used.
| TargetDescription | Optional | String | Authentication | A descriptive text associated with the device. For example: `Primary Domain Controller`.
| TargetDvcId | Optional | String | Authentication | The ID of the target device. If multiple IDs are available, use the most important one, and store the others in the fields `TargetDvc<DvcIdType>`.
| TargetDvcScopeId | Optional | String | Authentication | The cloud platform scope ID the device belongs to. TargetDvcScopeId map to a subscription ID on Azure and to an account ID on AWS.
| TargerDvcScope | Optional | String | Authentication | The cloud platform scope the device belongs to. TargetDvcScope map to a subscription ID on Azure and to an account ID on AWS.
| TargetDvcIdType | Conditional | Enumerated | Authentication | The type of TargetDvcId. For a list of allowed values and further information refer to [DvcIdType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#dvcidtype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas). Required if TargetDeviceId is used.
| TargetDeviceType | Optional | Enumerated | Authentication | The type of the target device. For a list of allowed values and further information refer to [DeviceType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#devicetype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas).
| TargetIpAddr | Optional | IP Address | Authentication | The IP address of the target device.
| TargetDvcOs | Optional | String | Authentication | The OS of the target device.
| TargetPortNumber | Optional | Integer | Authentication | The port of the target device.
| TargetGeoCountry | Optional | Country | Authentication | The country associated with the target IP address.
| TargetGeoRegion | Optional | Region | Authentication | The region associated with the target IP address.
| TargetGeoCity	| Optional | City | Authentication | The city associated with the target IP address.
| TargetGeoLatitude | Optional | Latitude | Authentication | The latitude of the geographical coordinate associated with the target IP address.
| TargetGeoLongitude | Optional | Longitude | Authentication | The longitude of the geographical coordinate associated with the target IP address.
| TargetRiskLevel | Optional | Integer | Authentication | The risk level associated with the target. The value should be adjusted to a range of 0 to 100, with 0 for benign and 100 for a high risk.
| TargetOriginalRiskLevel | Optional | Integer | Authentication | The risk level associated with the target, as reported by the reporting device.
| RuleName | Optional | String | Authentication | The name or ID of the rule by associated with the inspection results.
| RuleNumber | Optional | Integer | Authentication | The number of the rule associated with the inspection results.
| Rule | Alias | String | Authentication | Either the value of RuleName or the value of RuleNumber. If the value of RuleNumber is used, the type should be converted to string.
| ThreatId | Optional | String | Authentication | The ID of the threat or malware identified in the audit activity.
| ThreatName | Optional | String | Authentication | The name of the threat or malware identified in the audit activity.
| ThreatCategory | Optional | String | Authentication | The category of the threat or malware identified in audit file activity.
| ThreatConfidence | Optional | Integer | Authentication |he confidence level of the threat identified, normalized to a value between 0 and a 100.
| ThreatOriginalConfidence | Optional | String | Authentication |The original confidence level of the threat identified, as reported by the reporting device.
| ThreatIsActive | Optional	| Boolean | Authentication |True if the threat identified is considered an active threat.
| ThreatFirstReportedTime | Optional | datetime | Authentication |The first time the IP address or domain were identified as a threat.
| ThreatLastReportedTime | Optional	| datetime | Authentication |The last time the IP address or domain were identified as a threat.
| ThreatIpAddr | Optional | IP Address | Authentication |An IP address for which a threat was identified. The field ThreatField contains the name of the field ThreatIpAddr represents.
| ThreatField | Optional | Enumerated | Authentication |The field for which a threat was identified. The value is either SrcIpAddr or TargetIpAddr.
| TargetOriginalRiskLevel | Optional | Integer | Authentication | The risk level associated with the target, as reported by the reporting device.

</br>