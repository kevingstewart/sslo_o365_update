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
The installed script creates a working directory (/shared/o365), an configuration (iFile) json file, an iCall script, and iCall script periodic handler.
The configuration json file controls the various settings of the script:
