# Version 1.0.0; January 24th 2024
Initial release for imAuthentication version 1.0.0

vimAuthenticationMicrosoftWindowsEvent parser was converted to vimAuthenticationWinlogbeatv7 for compatibility with logstash.
Reference to vimAuthenticationMicrosoftWindowsEvent has been removed from this parser.

</br>

# Version 1.1.0; April 10th 2024

- Added the following filtering parameters for compatibility with vimAuthenticationAADNonInteractiveUserSignInLogs-v1.0.0 & vimAuthenticationSignInLogs-v1.1.0 parsers.
    - dvc
    - dvc_id
    - dvc_type
    - os
    - ip
    - useragent
    - app

# Version 2.0.0; May 22nd, 2024
- imAuthentication parser has been overhauled. See [readme](/AuthenticationEvent/Parsers/Unifying/imAuthentication/Readme.md) for further information.
- Change summary:
    - Aligned parsers with authentication schema version 0.1.3
    - Added new custom columns & other columns not previously used
    - Centralized authentication event codes
    - Documented schema
    - Overhauled parameters
