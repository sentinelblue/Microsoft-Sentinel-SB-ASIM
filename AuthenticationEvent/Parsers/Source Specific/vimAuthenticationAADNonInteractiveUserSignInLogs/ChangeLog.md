# Version 1.0.0; April 10th 2024

- Changed 'LogonTarget' value from 'ResourceIdentity' to 'TargetAppName'. Microsoft had differing values in their AAD account sign-in parsers which was breaking things.

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

- Fixed other issues related to invalid data types or values that were introduced by Microsoft.

<br>

# Version 2.0.0; May 8th 2024
- Rewrote parser from scratch for open sourcing. Parser was aligned with version 0.1.3 of the Authentication schema.
- See [Readme](/AuthenticationEvent/Parsers/Source%20Specific/vimAuthenticationAADNonInteractiveUserSignInLogs/Readme.md) file for further information on this update. This file was created based on version 2.0.0 development.