```
  ___     _                   _   _          
 |_ _|_ _| |_ ___ _ _ __ _ __| |_(_)_ _____  
  | || ' \  _/ -_) '_/ _` / _|  _| \ V / -_) 
 |___|_||_\__\___|_| \__,_\__|\__|_|\_/\___| 
 / __(_)__ _ _ _ (_)_ _   | |   ___  __ _ ___
 \__ \ / _` | ' \| | ' \  | |__/ _ \/ _` (_-<
 |___/_\__, |_||_|_|_||_| |____\___/\__, /__/
 | _ \_|___/ _ ___ ___ _ _          |___/    
 |  _/ _` | '_(_-</ -_) '_|                  
 |_| \__,_|_| /__/\___|_|                    
                                            
```

</br>

## About

### Metadata
| Detail | Data 
| - | - |
| Version | 2.0.0 
| Type | Source Specific
| Schema | Authentication
| Schema Version | 0.1.3

### Information
This is a source specific parser for the [SigninLogs](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/signinlogs) table.

### Requirements
| Dependency | Type | Version | Source | Deploy |
| - | - | - | - | - |
| SigninLogs | Table | N/A | Microsoft | N/A
| [AADResultTypes](/AuthenticationEvent/Parsers/Support/AADResultTypes/AADResultTypes.yaml) | Support Parser | 1.0.0 | Sentinel Blue | ![Deploy to Azure](https://aka.ms/deploytoazurebutton)

### Documentation

[SigninLogs Table Reference](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/signinlogs)  
[Asim Authentication Schema](https://learn.microsoft.com/en-us/azure/sentinel/normalization-schema-authentication)  
[Asim Common Schema](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields)  

</br>

## Deployment
![Deploy to Azure](https://aka.ms/deploytoazurebutton)

</br>

## Usage
### Function Name
Use `vimAuthenticationSigninLogs()` to call this function in the log analytics workspace. 

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
| eventseverity | Dynamic | EventSeverity | Filter on the severity level assosciated with the event.
| eventproduct | Dynamic | EventProduct | Filter on the product that logged at the event.
| pivot_lookback | String | N/A | Sets the lookback time in the pivot queries provided in each event. Defaults to a value of 30 days. 
| disabled | bool | N/A | Used to disable this parser in calling parent parsers.

</br>

### Examples

#### Filtering on singular entities
It is possible to filter on a string without specifically creating a dynamic even though the parameter calls for a dynamic.

1. `vimAuthenticationSigninLogs(starttime=ago(7d), dvc='DC-01')`
2. `vimAuthenticationSigninLogs(starttime=ago(90d), username_has_any='john', dvc_type='mobile', eventresult='success', ip='2607:fb91:2da3:4909:e09f:93f0:5337:9cca')`
3. `vimAuthenticationSigninLogs(starttime=todatetime('2024-04-01'), endtime=todatetime('2024-04-30'), targetappname_has_any='exchange', username_has_any='kyle@contoso.org', dvc_os='mac')`

#### Filtering on multiple entities
By using a dynamic, the parameters will filter on any of the values listed in the dynamic.

1. `vimAuthenticationSigninLogs(starttime=ago(1d), username_has_any=dynamic(['tom', 'alberta']))`
2. `vimAuthenticationSigninLogs(starttime=ago(1d), ip=dynamic(['89.231.38.73', '31.11.34.253', '173.192.3.38']), srchostname_has_any=dynamic(['Bastion-3', 'Bastion-0'))`

</br>

## Field Mappings & Definitions
### Statically Defined Fields
These fields are not derived from other columns. They have been assigned static values that will remain consistent in each row returned by the parser.

| Field | Value | Class | Type | Schema | Description  
| - | - | - | - | - | - |
| EventSchema | Authentication | Mandatory | String | Common | The schema the event is normalized to. Each schema documents its schema name.
| EventSchemaVersion | 0.1.3 | Mandatory | String | Common | The version of the schema. Each schema documents its current version. 
| EventVendor | Microsoft | Mandatory | String | Common | The vendor of the product generating the event. The value should be one of the values listed in [Vendors and Products](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields#vendors-and-products).
| EventProduct | Entra ID | Mandatory | String | Common | The product generating the event. The value should be one of the values listed in [Vendors and Products](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields#vendors-and-products).
| EventCount | 1 | Mandatory | Integer | Common | The number of events described by the record.
| EventSubType | Interactive | Optional | Enumerated | Common | The sign-in type. Allowed values include: `System`, `Interactive`, `RemoteInteractive`, `Service`, `RemoteService`, `Remote`, `AssumeRole`. The value may be provided in the source record using different terms, which should be normalized to these values. The original value should be stored in the field EventOriginalSubType.
| TargetUserIdType | EntraID | Conditional | UserIdType | Authentication | The type of the user ID stored in the TargetUserId field. For more information and list of allowed values, see [UserIdType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#useridtype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#). 
| TargetUsernameType | UPN | Conditional | UsernameType | Authentication | Specifies the type of the username stored in the TargetUsername field. For more information and list of allowed values, see [UsernameType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#usernametype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#).
| TargetAppType | SaaS application | Optional | AppType | Authentication | The type of the application authorizing on behalf of the Actor. For more information, and allowed list of values, see [AppType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#apptype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#).

</br>

### Fields Defined From SigninLogs Table
These fields are derived from fields within the [SigninLogs](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/signinlogs) table.

| ASIM Field | Table Field | Class | Type | Schema | Description  
| - | - | - | - | - | - |
| EventType | ResultType / [AADResultTypes](/AuthenticationEvent/Parsers/Support/AADResultTypes/AADResultTypes.yaml) | Mandatory | String | Common | Describes the operation reported by the record. Each schema documents the list of values valid for this field. The original, source specific, value is stored in the EventOriginalType field. For Authentication records, supported values include: `Logon`, `Logoff`, `Elevate`
| EventSeverity | RiskLevelAggregated | Recommended | Enumerated | Common | The severity of the event. Valid values are: `Informational`, `Low`, `Medium`, or `High`.
| EventResult | ResultType / [AADResultTypes](/AuthenticationEvent/Parsers/Support/AADResultTypes/AADResultTypes.yaml)| Mandatory | Enumerated | Common | One of the following values: `Success`, `Partial`, `Failure`, `NA` (Not Applicable). The value might be provided in the source record by using different terms, which should be normalized to these values. Alternatively, the source might provide only the EventResultDetails field, which should be analyzed to derive the EventResult value.
| EventResultDetails | ResultType / [AADResultTypes](/AuthenticationEvent/Parsers/Support/AADResultTypes/AADResultTypes.yaml) | Recommended | Enumerated | Common | The details associated with the event result. This field is typically populated when the result is a failure. Allowed values include: `No such user or password`, `No such user`, `Incorrect password`, `Incorrect key`, `Account expired`, `Password expired`, `User locked`, `User disabled`, `Logon violates policy`, `Session expired`, `Other`. The value may be provided in the source record using different terms, which should be normalized to these values. The original value should be stored in the field EventOriginalResultDetails
| EventProductVersion | OperationVersion | Optional | String | Common | The version of the product generating the event.
| EventOriginalResultDetails | ResultType / [AADResultTypes](/AuthenticationEvent/Parsers/Support/AADResultTypes/AADResultTypes.yaml) | Optional | String | Common | The original result details provided by the source. This value is used to derive EventResultDetails, which should have only one of the values documented for each schema.
| EventUid | _ItemId | Recommended | String | Common | The unique ID of the record, as assigned by Microsoft Sentinel. This field is typically mapped to the `_ItemId` Log Analytics field. Example: `69f37748-ddcd-4331-bf0f-b137f1ea83b`
| EventOriginalType | ResultType | Optional | String | Common | The original event type or ID, if provided by the source. For example, this field is used to store the original Windows event ID. This value is used to derive EventType, which should have only one of the values documented for each schema. Example: `4624`
| EventOriginalSubType | Type | Optional | String | Common | The original event subtype or ID, if provided by the source. For example, this field is used to store the original Windows logon type. This value is used to derive EventSubType, which should have only one of the values documented for each schema.
| EventOriginalUid | Id | Optional | String | Common | A unique ID of the original record, if provided by the source. Example: `69f37748-ddcd-4331-bf0f-b137f1ea83b`
| EventOriginalSeverity | RiskLevelAggregated | Optional | String | Common | The original severity as provided by the reporting device. This value is used to derive EventSeverity.
| LogonProtocol | AuthenticationProtocol | Optional | String | Authentication | The protocol used to perform the authentication
| DvcId | DeviceDetail.deviceId | Optional | String | Common | The unique ID of the device on which the event occurred or which reported the event, depending on the schema. Example: `41502da5-21b7-48ec-81c9-baeea8d7d669`
| DvcIpAddr | IPAddress | Recommended | IP address | Common | The IP address of the device on which the event occurred or which reported the event, depending on the schema.
| DvcOs | DeviceDetail.operatingSystem | Optional | String | Common | The operating system running on the device on which the event occurred or which reported the event. 
| DvcDomain | DeviceDetail.trustType | Optional | String | Common | The domain of the device on which the event occurred or which reported the event, depending on the schema.
| DvcHostname | DeviceDetail.displayName | Optional | String | Common | The hostname of the device on which the event occurred or which reported the event, depending on the schema. Example: `ContosoDc`
| DvcZone | NetworkLocationDetails | Optional | String | Common | The network on which the event occurred or which reported the event, depending on the schema. The zone is defined by the reporting device.
| TargetUserId | UserId | Optional | UserId | Authentication | A machine-readable, alphanumeric, unique representation of the target user. For more information, and for alternative fields for additional IDs, see [The User entity](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#the-user-entity).
| TargetUserScope | UserPrincipalName | Optional | String | Authentication | The scope, such as Microsoft Entra tenant, in which TargetUserId and TargetUsername are defined. or more information and list of allowed values, see [UserScope](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#userscope) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#).
| TargetUserScopeId | HomeTenantId | Optional | String | Authentication | The scope ID, such as Microsoft Entra Directory ID, in which TargetUserId and TargetUsername are defined. for more information and list of allowed values, see [UserScopeId](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#userscopeid) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas).
| TargetUserType | UserType | Optional | UserType | Authentication | The type of the Target user. For more information, and list of allowed values, see [UserType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#usertype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#).
| SrcGeoCountry | LocationDetails.countryOrRegion | Optional | Country | Authentication | The country that the source device logged the event from. For more information, see [Logical types](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#logical-types).
| SrcGeoCity | LocationDetails.city | Optional | City | Authentication | The city in the region that the source device logged the event from. For more information, see [Logical types](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#logical-types).
| SrcGeoRegion | LocationDetails.state | Optional | Region | Authentication | The region in the country that the source device logged the event from 
| SrcGeoLongitude | LocationDetails.geoCoordinates.longitude | Optional | Longitude | Authentication | 0.1.3.| The longitude of the source device that logged the event. For more information, see [Logical types](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#logical-types).
SrcGeoLatitude | LocationDetails.geoCoordinates.latitude | Optional | Latitude | Authentication | The latitude of the source device that logged the event. For more information, see [Logical types](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#logical-types)
| SrcRiskLevel | RiskLevelDuringSignIn | Optional | Integer | Authentication | The risk level associated with the source. The value should be adjusted to a range of `0` to `100`, with `0` for benign and `100` for a high risk.
| TargetAppId | ResourceIdentity | Optional | String | Authentication | The ID of the application to which the authorization is required, often assigned by the reporting device. 
| TargetAppName | ResourceDisplayName | Optional | String | Authentication | The name of the application to which the authorization is required, including a service, a URL, or a SaaS application.
| LogonMethod | AuthenticationRequirement | Optional | String | Authentication | The method used to perform authentication.
| HttpUserAgent | UserAgent | Optional | String | Authentication | When authentication is performed over HTTP or HTTPS, this field's value is the user_agent HTTP header provided by the acting application when performing the authentication. For example: `Mozilla/ 5.0 (iPhone; CPU iPhone OS 12_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.0 Mobile/15E148 Safari/604.1`
| TargetUsername | UserPrincipalName | Optional | Username | Authenication | The target user username, including domain information when available. For more information, see The User entity.
| TargetOriginalUserType | UserType | Optional | UserType | Authentication | The user type as reported by the reporting device.
| TargetSessionId | CorrelationId | Optional | String | Authentication | The sign-in session identifier of the TargetUser on the source device.
| SrcOriginalRiskLevel | RiskLevelDuringSignIn | Optional | Integer | Authentication | The risk level associated with the source, as reported by the reporting device.
| ActingAppName | AppDisplayName | Optional | String | Authentication | The name of the application authorizing on behalf of the actor, including a process, browser, or service.
| ActingAppId | AppId | Optional | String | Authentication | The ID of the application authorizing on behalf of the actor, including a process, browser, or service.

</br>

### Aliases
These fields create another name for a field that has already been normalized.

| Alias | Field | Class | Type | Schema | Description
| - | - | - | - | - | - |
| Dvc | DvcHostname | Alias | String | Common | A unique identifier of the device on which the event occurred or which reported the event, depending on the schema. This field might alias the DvcFQDN, DvcId, DvcHostname, or DvcIpAddr fields. For cloud sources, for which there is no apparent device, use the same value as the Event Product field.
| User | TargetUsername | Alias | Username | Authentication | Alias to the TargetUsername or to the TargetUserId if TargetUsername is not defined.
| IpAddr | SrcIpAddr | Alias | String | Authentication | Alias to SrcIpAddr
| LogonTarget | TargetAppName / TargetHostname | Alias | String | Authentication | Alias to either TargetAppName, TargetUrl, or TargetHostname, whichever field best describes the authentication target.
| Dst | TargetAppName / TargetHostname | Alias | String | Authentication | A unique identifier of the authentication target. This field may alias the TargerDvcId, TargetHostname, TargetIpAddr, TargetAppId, or TargetAppName fields.
| EventMessage | EventOriginalResultDetails | Optional | String | Common | A general message or description, either included in or generated from the record.
| EventStartTime | TimeGenerated | Mandatory | Datetime | Common | The time in which the event started. If the source supports aggregation and the record represents multiple events, the time that the first event was generated. If not provided by the source record, this field aliases the [TimeGenerated](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields#timegenerated) field.
| EventEndTime | TimeGenerated | Mandatory | Datetime | Common | The time in which the event ended. If the source supports aggregation and the record represents multiple events, the time that the last event was generated. If not provided by the source record, this field aliases the [TimeGenerated](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields#timegenerated) field.
| UserType | TargetUserType | Optional | UserType | Common | The type of source user. See [UserType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#usertype) for further information.
| DvcDomainType | DvcDomain | Conditional | Enumerated | Common | The type of DvcDomain. For a list of allowed values and further information, refer to [DomainType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#domaintype). Note: This field is required if the DvcDomain field is used.
| DvcFQDN | DvcDomain | Optional | String | Common | The hostname of the device on which the event occurred or which reported the event, depending on the schema. Note: This field supports both traditional FQDN format and Windows domain\hostname format. The DvcDomainType field reflects the format used.
| DvcIdType	| DvcId | Conditional | Enumerated | Common | The type of DvcId. For a list of allowed values and further information, refer to [DvcIdType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields#dvcidtype). Note: This field is required if the DvcId field is used
| Src | SrcIpAddr | Recommended | String | Authentication | A unique identifier of the source device. This field may alias the SrcDvcId, SrcHostname, or SrcIpAddr fields.
| SrcDvcId | DvcId | Optional | String | Authentication | The ID of the source device. If multiple IDs are available, use the most important one, and store the others in the fields `SrcDvc<DvcIdType>`.
| SrcDvcIdType | DvcIdType | Conditional | DvcIdType | Authentication | The type of SrcDvcId. For a list of allowed values and further information refer to [DvcIdType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#dvcidtype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas). Note: This field is required if SrcDvcId is used.
| SrcDeviceType | DvcOs | Optional | DeviceType | Authentication | The type of the source device. For a list of allowed values and further information refer to [DeviceType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#devicetype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas).
| SrcHostname | DvcHostname / DvcIpAddr | Recommended | Hostname | Authentication | The source device hostname, excluding domain information. If no device name is available, store the relevant IP address in this field.
| SrcDomain | DvcDomain | Recommended | String | Authentication | The domain of the source device.
| SrcDomainType | DvcDomainType | Conditional | DomainType | Authentication | The type of SrcDomain. For a list of allowed values and further information refer to [DomainType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#domaintype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas).
| SrcFQDN | DvcFQDN | Optional | String | Authentication | The source device hostname, including domain information when available. Note: This field supports both traditional FQDN format and Windows domain\hostname format. The SrcDomainType field reflects the format used.
| SrcIpAddr | DvcIpAddr | Optional | IP Address | Authentication | The IP address of the source device.
| SrcDvcOs | DvcOs | Optional | String | Authentication | The OS of the source device. 

</br>

### Custom Fields
These are custom fields included in the parser which were not included in Microsoft provided schema.

#### Derived from [SigninLogs](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/signinlogs)
| ASIM Field | SigninLogs Field | Class | Type | Description  
| - | - | - | - | - |
| DvcManaged | DeviceDetail.isManaged | Optional | String | Specifies if the device associated with the event is a device managed in Azure. Value should be either `Yes` or `No`.
| DvcCompliant | DeviceDetail.isCompliant | Optional | String | Specifies if the device meets compliance requirements. | 
| DvcTrustType | DeviceDetail.trustType | Optional | String | Specifies if a managed device is registered or joined to the tenant
| MfaAuthMethod | MfaMethod.authMethod | Optional | String | Specifies the type of MFA challenge presented to the authenticating user
| MfaAuthDetail | MfaMethod.authDetail | Optional | String | Extra information as it pertains to the MFA authentication method

#### Pivots Field
The 'Pivots' field contains sub-fields broken down by entity category such as Device, User, IP Address, etc. Each entity field contains a list of sub queries that can be used to gain further information about the entity. The 'pivot_lookback' will adjust how far these queries look back in the records.

##### IP Pivots Fields
The IP field contains links to web sites that provide information about the ip address to the analyst.

| Field | Description |
| - | - |
| Abuse IP Database | Look up the IP in [Abuse IP DB](https://abuseipdb.com).
| Ip Location | Check the location of the ip with [Iplocation](https://iplocation.net).
| Spur | Determine if ip is a VPN and VPN provider via [Spur](https://spur.us).
| Threat Book | Look up IP in [Threat Book](https://threatbook.io).
| Virus Total | Check the IP in [Virus Total](https://virustotal.com).
| Azure Lookup | Check which microsoft service the IP belongs to if the IP is a microsoft owned IP with [Azure Speed](https://www.azurespeed.com/Azure/IPLookup)

##### User History Fields
The User field contains KQL queries to analyze the users authentication history.

| Field | Description |
| - | - |
| All | Query all sign-in activity for the user.
| IP | Query the users interactive sign-in history from the associated IP.
| OS | Query the users authentication history from a specific operating system.
| Device | Query the users authentication history from a device hostname
| User Agent | Query the users authentication history where a particular user agent is present
| Application | Query the users authentication hitory for a particular application

</br>

### Original SigninLogs Fields Stored in AdditionalFields
These are original fields from the SigninLogs table that have been stored in the '[AdditionalFields](https://learn.microsoft.com/en-us/azure/sentinel/normalization-common-fields#other-fields)' column.

| SigninLogs Field | Type | Description
| - | - | - |
| AuthenticationContextClassReferences | String | Contains a collection of values that represent the conditional access authentication contexts applied to the sign-in.
| AuthenticationDetails | String | The result of the authentication attempt and additional details on the authentication method.
| AuthenticationProcessingDetails | String | Additional authentication processing details, such as the agent name in case of PTA/PHS or Server/farm name in case of federated authentication.
| AuthenticationRequirementPolicies | String | Sources of authentication requirement, such as conditional access, per-user MFA, identity protection, and security defaults.
| ConditionalAccessPolicies | Dynamic | A list of conditional access policies that are triggered by the corresponding sign-in activity.
| SessionLifetimePolicies | String | Any conditional access session management policies that were applied during the sign-in event.
| DeviceDetail | Dynamic | The device information from where the sign-in occurred. Includes information such as deviceId, OS, and browser.
| LocationDetails | Dynamic | Provides the city, state, country/region and latitude and longitude from where the sign-in happened.
| NetworkLocationDetails | String | The network location details including the type of network used and its names.
| Status | Dynamic | The sign-in status. Includes the error code and description of the error (in case of a sign-in failure).
| ClientAppUsed | String | The legacy client used for sign-in activity. For example: Browser, Exchange ActiveSync, Modern clients, IMAP, MAPI, SMTP, or POP.
| ConditionalAccessStatus | String | The status of the conditional access policy triggered. Possible values: success, failure, or notApplied.
| OriginalRequestId | String | The request identifier of the first request in the authentication sequence.
| RiskDetail | String | The reason behind a specific state of a risky user, sign-in, or a risk event. Possible values: none, adminGeneratedTemporaryPassword, userPerformedSecuredPasswordChange, userPerformedSecuredPasswordReset, adminConfirmedSigninSafe, aiConfirmedSigninSafe, userPassedMFADrivenByRiskBasedPolicy, adminDismissedAllRiskForUser, or adminConfirmedSigninCompromised. The value none means that no action has been performed on the user or sign-in so far. Note: Details for this property are only available for Azure AD Premium P2 customers. All other customers are returned hidden.
| RiskEventTypes_V2 | String | The list of risk event types associated with the sign-in. Possible values: unlikelyTravel, anonymizedIPAddress, maliciousIPAddress, unfamiliarFeatures, malwareInfectedIPAddress, suspiciousIPAddress, leakedCredentials, investigationsThreatIntelligence, or generic.
| RiskState | String | The risk state of a risky user, sign-in, or a risk event. Possible values: none, confirmedSafe, remediated, dismissed, atRisk, or confirmedCompromised.
| TokenIssuerType | String | The type of identity provider. The possible values are: AzureAD, or ADFederationServices, AzureADBackupAuth, ADFederationServicesMFAAdapter, NPSExtension.
| ResourceServicePrincipalId | String | The identifier of the service principal representing the target resource in the sign-in event.
| ResourceTenantId | String | The tenant identifier of the resource referenced in the sign in
| AADTenantId | String | Undocumented
| UniqueTokenIdentifier | String | A unique base64 encoded request identifier used to track tokens issued by Azure AD as they are redeemed at resource providers.
| AutonomousSystemNumber | String | The Autonomous System Number (ASN) of the network used by the actor.
| CrossTenantAccessType | String | Describes the type of cross-tenant access used by the actor to access the resource
| UserDisplayName | string | The display name of the user.

### Fields Retained from SigninLogs
These fields have been retained from the data source and left in there original format.

| SigninLogs Field | Type | Description
| - | - | - |
| TimeGenerated | Datetime | The time the event was generated by the reporting device.
| UserId | String | The identifier of the user.

### Unused Fields
These are fields from the schemas that are not currently used in the parser.

| Field | Class | Type | Schema | Description
| - | - | - | - | - |
| EventReportUrl | Optional | String | Common | A URL provided in the event for a resource that provides more information about the event.
| EventOwner | Optional | String | Common | The owner of the event, which is usually the department or subsidiary in which it was generated.
| DvcDescription | Conditional | Enumerated | Common | A descriptive text associated with the device. For example: `Primary Domain Controller`.
| DvcMacAddr | Optional | MAC | Common | The MAC address of the device on which the event occurred or which reported the event.
| DvcOsVersion | Optional | String | Common | The version of the operating system on the device on which the event occurred or which reported the event.
| DvcAction | Recommended | String | Common | For reporting security systems, the action taken by the system, if applicable.
| DvcOriginalAction | Optional | String | Common | The original DvcAction as provided by the reporting device.
| DvcInterface | Optional | String | Common | The network interface on which data was captured. This field is typically relevant to network related activity, which is captured by an intermediate or tap device.
| DvcScopeId | Optional | String | Common | The cloud platform scope ID the device belongs to. DvcScopeId map to a subscription ID on Azure and to an account ID on AWS.
| DvcScope | Optional | String | Common | The cloud platform scope the device belongs to. DvcScope map to a subscription ID on Azure and to an account ID on AWS.
| ASimMatchingIpAddr | Recommended | String | Common | When a parser uses the `ipaddr_has_any_prefix` filtering parameters, this field is set with the one of the values `SrcIpAddr`, `DstIpAddr`, or `Both` to reflect the matching fields or fields.
| ASimMatchingHostname | Recommended | String | Common | When a parser uses the `hostname_has_any` filtering parameters, this field is set with the one of the values `SrcHostname`, `DstHostname`, or Both to reflect the matching fields or fields.
| LogonProtocol | Optional | String | Authentication | The protocol used to perform authentication.
| ActorUserId | Optional | String | Authentication | A machine-readable, alphanumeric, unique representation of the Actor. For more information, and for alternative fields for additional IDs, see [The User entity](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#the-user-entity).
| ActorScope | Optional | String | Authentication | The scope, such as Microsoft Entra tenant, in which ActorUserId and ActorUsername are defined. or more information and list of allowed values, see [UserScope](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#userscope) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas).
| ActorScopeId | Optional | String | Authentication | The scope ID, such as Microsoft Entra Directory ID, in which ActorUserId and ActorUsername are defined. for more information and list of allowed values, see [UserScopeId](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#userscopeid) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas)
| ActorUserIdType | Coniditional | UserIdType | Authentication | The type of the ID stored in the ActorUserId field. For more information and list of allowed values, see [UserIdType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#useridtype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas)
| ActorUsername | Optional | Username | Authentication | The Actor’s username, including domain information when available. For more information, see The User entity.
| ActorUsernameType | Conditional | UsernameType | Authentication | Specifies the type of the user name stored in the ActorUsername field. For more information, and list of allowed values, see [UsernameType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#usernametype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas).
| ActorUserType | Optional | UserType | Authentication | The type of the Actor. For more information, and list of allowed values, see [UserType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#usertype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas).
| ActorOriginalUserType | Optional | UserType | Authentication | The user type as reported by the reporting device.
| ActorSessionId | Optional | String | Authentication | The unique ID of the sign-in session of the Actor.
| ActingAppType | Optional | AppType | Authentication | The type of acting application. For more information, and allowed list of values, see [AppType](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas#apptype) in the [Schema Overview article](https://learn.microsoft.com/en-us/azure/sentinel/normalization-about-schemas).
| SrcDvcScopeId | Optional | String | Authentication | The cloud platform scope ID the device belongs to. SrcDvcScopeId map to a subscription ID on Azure and to an account ID on AWS.
| SrcDvcScope | Optional | String | Authentication | The cloud platform scope the device belongs to. SrcDvcScope map to a subscription ID on Azure and to an account ID on AWS.
| SrcDescription | Optional | String | Authentication | A descriptive text associated with the device. For example: `Primary Domain Controller`.
| SrcPortNumber | Optional | Integer | Authentication | The IP port from which the connection originated.
| SrcIsp | Optional | Integer | Authentication | The Internet Service Provider (ISP) used by the source device to connect to the internet.
| TargetUrl | Optional | URL | Authentication | The URL associated with the target application.
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
| ThreatRiskLevel | Optional | Integer | Authentication | The risk level associated with the identified threat. The level should be a number between 0 and 100. **Note**: The value might be provided in the source record by using a different scale, which should be normalized to this scale. The original value should be stored in ThreatRiskLevelOriginal.
| ThreatOriginalRiskLevel | Optional | String | Authentication | The risk level as reported by the reporting device.

</br>
