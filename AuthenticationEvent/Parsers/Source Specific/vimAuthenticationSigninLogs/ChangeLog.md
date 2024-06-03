# Version 1.0.0; February 21st 2024

- Switched source from Microsoft to Sentinel Blue. This parser is based on the Microsoft provided parser, vimAuthenticationSigninLogs version 0.1.0.

- Resolved data type errors produced by the SignInLogs table having the same columns of a different data type than the other authentication based tables. This would result in the columns having their data types appended to the column. Added the following code at line 39.

    ```
    | extend
        ConditionalAccessPolicies = tostring(ConditionalAccessPolicies)
        , LocationDetails         = tostring(LocationDetails)
        , DeviceDetail            = tostring(DeviceDetail)
        , MfaDetail               = tostring(MfaDetail)
        , Status                  = tostring(Status)
    ```

</br>

# Version 1.1.0; April 10th 2024

- Aligned the schema to version 0.1.3

- Added the following columns to the parser
    - EventMessage
    - EventResultDetails
    - EventUid
    - EventSeverity
    - DvcCompliant
    - DvcManaged
    - DvcTrustType
    - Dvc
    - DvcHostname
    - DvcIpAddr
    - DvcDomain
    - DvcId
    - DvcIdType
    - ActingAppId
    - ActingAppName
    - ActingAppType
    - SrcGeoRegion
    - SrcDvcIdType
    - SrcDomain
    - SrcDomainType
    - SrcHostname
    - SrcFQDN
    - SrcIpAddr
    - TargetUserScope
    - TargetUserOriginalType
    - TargetUserId
    - TargetUserScopeId
    - SrcOriginalRiskLevel
    - Src
    - IpAddr
    - UserScope
    - UserScopeId
    - UserIdType
    - UserAADTenant
    - Username
    - UserUPN
    - SimpleUsername
    - Domain
    - Hostname
    - DomainType
    - FQDN
    - DvcOs
    - IpPivots
    - SrcDeviceType

> [!NOTE]
> The 'IpPivots' column contains links to websites that will provide insight about the IP address. Spur, Abuse IP DB, Virus Total, Threat Book & IP Location were included in this.

- Added the following filtering parameters
    - dvc
    - dvc_id
    - dvc_type
    - os
    - ip
    - useragent
    - app

- Changed 'targetusername_has' parameter to 'user'

</br>

# Version 2.0.0; May 8th 2024
- Rewrote parser from scratch for open sourcing. Parser was aligned with version 0.1.3 of the Authentication schema.
- See [Readme](/AuthenticationEvent/Parsers/Source%20Specific/vimAuthenticationSigninLogs/Readme.md) file for further information on this update. This file was created based on version 2.0.0 development.