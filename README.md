# F5 SSL Orchestrator Office 365 URL Update Script
A small Python (2.x) utility to download and maintain the dynamic set of Office 365 URLs as data groups and custom URL categories on the F5 BIG-IP, for use with SSL Orchestrator.

### To install 
- Download the script onto the F5 BIG-IP:

  `curl -k https://raw.githubusercontent.com/kevingstewart/sslo_o365_update/main/sslo_o365_update.py -o sslo_o365_update.py`

- Run the script with the --install option and a time interval (the time interval controls periodic updates, in seconds - 3600sec = 1hr).

  `python sslo_o365_update.py --install 3600`

### To modify the configuration
- tmsh edit the configuration json file

  `tmsh`
  
  `edit sys file ifile o365_config.json`

### To uninstall
- Run the script with the --uninstall option. This will remove the iCall script and handler, and configuration json file.

  `python sslo_o365_update.py --uninstall`

### Configuration environment
The installed script creates a working directory (/shared/o365), a configuration (iFile) json file, an iCall script, and iCall script periodic handler.
The configuration json file controls the various settings of the script:


***Endpoint** - Microsoft Web Service Customer endpoints (ENABLE ONLY ONE ENDPOINT). These are the set of URLs defined by customer endpoints as described here: https://docs.microsoft.com/en-us/office365/enterprise/urls-and-ip-address-ranges. Valid values are [Worldwide, USGovDoD, USGovGCCHigh, China, Germany].*

    endpoint": "Worldwide"


***O365 "SeviceArea"** - O365 endpoints to consume, as described here: https://docs.microsoft.com/en-us/office365/enterprise/urls-and-ip-address-ranges.*

    service_areas
        common: true|false                -> Microsoft 365 Common and Office Online"
        exchange: true|false              -> Exchange Online  
        sharepoint: true|false            -> SharePoint Online and OneDrive for Business
        skype: true|false                 -> Skype for Business Online and Microsoft Teams


***Outputs** - O365 Record objects to create*    

    outputs
        url_categories: true|false        -> Create URL categories
        url_datagroups: true|false        -> Create URL data groups
        ip4_datagroups: true|false        -> Create IPv4 data groups
        ip6_datagroups: true|false        -> Create IPv6 data groups


***O365 Categories** - create a single URL data set, and/or separate data sets for O365 Optimize/Default/Allow categories*

    o365_categories                  
        all: true|false                   -> Create a single date set containing all URLs (all categories)
        optimize: true|false              -> Create a data set containing O365 Optimize category URLs (note that the optimized URLs are in Exchange and SharePoint service areas)
        default: true|false               -> Create a data set containing O365 Allow category URLs
        allow: true|false                 -> Create a data set containing O365 Default category URLs


***Required O365 Endpoints to import** - O365 required endpoints or all endpoints. WARNING: "import all" includes non-O365 URLs that one may not want to bypass (ex. www.youtube.com).*

    only_required: true|false       -> false=import all URLs, true=Office 365 required only URLs


***Excluded URLs** - (URL pattern matching is supported). Provide URLs in list format - ex. ["m.facebook.com", ".itunes.apple.com", "bit.ly"].*

    excluded_urls: []


***Included URLs** - (URL must be exact match to URL as it exists in JSON record - pattern matching not supported). Provide URLs in list format - ex. ["m.facebook.com", ".itunes.apple.com", "bit.ly"].*    

    included_urls: [] 
   
   
***Excluded IPs** - (IP must be exact match to IP as it exists in JSON record - IP/CIDR mask cannot be modified). Provide IPs in list format - ex. ["191.234.140.0/22", "2620:1ec:a92::152/128"].*

    excluded_ips: [] 

***System-level configuration settings***

    system:
        force_refresh: true|false        -> Action if O365 endpoint list is not updated (a "fetch now" function)
        log_level: 1                     -> 0=none, 1=normal, 2=verbose
        ha_config: 0                     -> 0=stand alone, 1=HA paired
        device_group: "device-group-1".  -> Name of Sync-Failover Device Group.  Required for HA paired BIG-IP.
   
 

