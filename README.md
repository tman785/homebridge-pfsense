Initial Commit of creating a homekit switch to toggle rules in pfsense

The script will toggle the disabled flag for any rule with a description beginning with a specified term.  Script will also provide a state

Usage:
./toggle_rule enable
./toggle_rule disable
./toggle_rule state

Prerequisites
1.  Pfsense needs to have a working instance of FauxAPI
https://github.com/ndejong/pfsense_fauxapi
 
2.  Need to have a working instance of homebridge running

3.  Install the cmdswitch2 plugin
https://www.npmjs.com/package/homebridge-cmdswitch2



Installation Directions
1.  Create a folder to house the scripts

2.  Download the toggle_rule script into the folder

3.  Download the FauxAPI python library
https://github.com/ndejong/pfsense_fauxapi_client_python

4.  Place the following files at the root of the folder (very important)
 -  \_\_version__.py
 - PfsenseFauxapi.py

5.  chmod +x toggle_rule

6.  In your config.json file:

                {
                "platform":"cmdSwitch2",
                "name":"CMD Switch",
                "switches":[{
                        "name":"Kids Internet",
                        "on_cmd":"/home/folder/toggle_rule enable",
                        "off_cmd":"/home/folder/toggle_rule disable",
                        "state_cmd":"/home/folder/toggle_rule state 2>&1 /dev/null; script_output=$? ; echo $script_output | grep 1"
                        }]
                }

The state_cmd needs to be written exactly (with exception to the folder path).  Basically, the state_cmd only looks to see if the switch is on.  Toggle_rule will report an exit value of 1 if that's the case.  I had to perform some extra steps to report back a 1 for the plugin

7. Modify toggle_rule:

        api_hostname = "pfsense_host"
        api_key = "fauxapi_key"
        api_secret = "fauxpai_secret"
        rule_prefix = "TEST"




