# AggressorScripts

Disclaimer: These scripts are to help you audit your machines or machines you're authorized to audit. Don't use these on anything you don't have the owner's explicit permission to test on. That's mean. Also illegal. 

## bueller.cna
Automate portscans to check which beacons have access to a specific host

Usage:
```
1. Select all beacon(s) you want to check access from 
2. Right click and select "Bueller? Anyone?"
3. Enter target host and port
4. Open "View" > "Script Console" to see output.
 > Note: it may take a while for all beacons to call back depending on your sleep time. 
```

## cdolla.cna
Find targets where you're local admin and list users who logged in within the last 90 days.

Like Chris King's (raikiasec) CredNinja but less fancy. And in Aggressor. (See also: https://github.com/Raikia/CredNinja)

Usage:
```
1. To scan a list of targets
 > Select all hosts in the Targets tab, right click, add note "cdolla"
 > From the beacon with the correct token: cdolla [-users]
2. To scan a single target
 > cdolla 10.10.10.10 [-users]
```

Notes:
* c is an alias for cdolla
* if -users is specified, will also print users who logged in within last 90 days
* The list of users will look like "UserName1 (31), Username2 (48)" where the numbers in parentheses are days since last login

## compromised_log.rpt
This report generates an appendix with tables of all hosts where a beacon was spawned and all users that were compromised/added to the "Credentials" tab. 

Usage:
```
1. Import by clicking Cobalt Strike > Preferences > Reporting > Select template
2. Generate by clicking Reporting > 1. Compromise Log
```

Notes on "Affected Hosts":
* Deduplicated by hostname
* AccountsUsed -> User accounts that beacons were spawned under. 
* FirstSession -> Date/time of first session/beacon opened on a host.

Notes on "Affected Users":
* Deduplicated by "username::realm"
* Domain -> will include hostname if it's a local account
* Source -> host where the credential was gathered 
* Cleartext -> if password is not NTLM hash && source == mimikatz
* Cracked -> if password is not NTLM hash && source == manual (generalized but assuming was added after cracking)
* Hash -> at least one password entry for the user was an NTLM hash

## credpocalypse.cna
Monitor beacons and pick off users as they log in. Set the time interval (default 5m) and Credpocalypse will watch your beacons for new users in the running processes. If they aren't in the Credentials tab already, Credpocalypse will run logonpasswords.

Pro Tip: Load/run with ./agscript to watch all beacons headlessly. Output will include timestamp/beacon IDs to track when logonpasswords was run. 

NOTE: Your beacon will only be interrupted if logonpasswords is run. There's no callback, so I can't smother the output. :-/ 

NOTE AGAIN: In the Watchlist and Script Console you'll see "Running on PIDs: x". X is the PID of the beacon process on the remote system (what you see on the right side of your beacon list). In the background, it's actually using the beacon IDs assigned by Cobalt. I just print the PID to make it easier for you to glance at beacons and know where Credpocalypse is running. 

Usage:
1. Aliases
```
begin_credpocalypse		- watch current beacon
end_credpocalypse [all]		- stop watching current/all beacon/s
credpocalypse_interval [time]	- 1m, 5m (default), 10m, 30m, 60m
```
2. Commands
```
begin_credpocalypse		- watch *all* beacons
end_credpocalypse [all]		- stop watching all beacons
credpocalypse_interval [time]	- 1m, 5m (default), 10m, 30m, 60m
```
3. Right click beacon(s) to get a pop up menu that lets you 
* Add to watchlist
* Remove from watchlist
* Change time interval that Credpocalypse checks watchlist
* View the watchlist 

## leave\_no\_trace.cna
Track/clean up dropped files, because littering is bad. 

Includes a tab to view all files uploaded through beacons during an engagement. 

Usage:
* View > "Leave No Trace". Click column to sort. 
```
(By default all items show as Status: ?. Click "Check for litter" to update.) 
```
* Right click items 
```
    > "Search and Destroy" tries to remove items from the chosen beacon
    > "Check for litter" 
        - Does an LS to look for the dest_file from the chosen beacon
        - Updates left column of results with status (cleaned, NOT cleaned, ?)
```

Coming soon:
* Additional options to specify directories/paths (in cases dest_file was the filename only)
* Track bcp() calls in archives too instead of just bupload() 
* Add interesting filenames/directories to compromised_log.rpt for easier reporting to blue team

FAIR WARNING:
* If you select a dest_file that has only the filename (not a full path as well) this will fail. 
* I'm sorry - it's not my fault. It's all I could pull from the archives/upload event.

## portscan_results.cna 
See and sort results from portscan module in a new tab

CREDIT:
This script uses the awesome visualization/tab code made by @001SPARTaN (for @r3dqu1nn)
As seen here: https://github.com/harleyQu1nn/AggressorScripts/blob/master/logvis.cna

Usage:
```
View > "Port Scan Results". Click column to sort.
```

## save_log.cna 
Use to export command output, so you don't have to grep beacon logs for info. 

Usage:
```
start_log
[commands]
stop_log
```

Output: cobaltstrike/saved_logs/[beacon id]_yyyyMMdd_HHmmssSSS.log
```
[2017-12-27 11:36:24 EST] BID: 12345 Tasked beacon to run: whoami
received output: 
WORKSTATION\Administrator
```

## utils.cna 
A collection of "sub" functions to do random things. Copy into your CNAs and refer to the "alias" functions at the bottom of the file for examples on how to call each utility.

Included:
```
get_env COMSPEC					- print value of env variable
get_pid explorer				- print 1st PID for given proc name
get_users					- return array of logged on users
lower C:\Users\Public\Downloads\tmp.txt 	- lc() without breaking on \'s 
parse_args -arg1 val1 -arg2 val2 -switch1	- easier than positional arguments
upper C:\Users\Public\Downloads\tmp.txt 	- uc() without breaking on \'s
```

Note: 
Raffi wrote the get_pid function (https://www.cobaltstrike.com/aggressor-script/functions.html#bppid). Included here for easy reference. 
