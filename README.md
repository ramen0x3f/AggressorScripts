# AggressorScripts

Disclaimer: These scripts are to help you audit your machines or machines you're authorized to audit. Don't use these on anything you don't have the owner's explicit permission to test on. That's mean. Also illegal. 

## credpocalypse.cna
Monitor beacons and pick off users as they log in. Set the time interval (default 5m) and Credpocalypse will watch your beacons for new users in the running processes. If they aren't in the Credentials tab already, Credpocalypse will run logonpasswords.

NOTE: Your beacon will only be interrupted if logonpasswords is run. There's no callback, so I can't smother the output. :-/ 

NOTE AGAIN: In the Watchlist and Script Console you'll see "Running on PIDs: x". X is the PID of the beacon process on the remote system (what you see on the right side of your beacon list). In the background, it's actually using the beacon IDs assigned by Cobalt. I just print the PID to make it easier for you to glance at beacons and know where Credpocalypse is running. 

Usage:
1. Aliases
```
begin_credpocalypse		- watch current beacon
end_credpocalypse [all]		- stop watching current/all beacon/s
credpocalypse_interval [time]	- 1m, 5m (default), 10m, 30m, 60m
```

2. Right click beacon(s) to get a pop up menu that lets you 
* Add to watchlist
* Remove from watchlist
* Change time interval that Credpocalypse checks watchlist
* View the watchlist 

## save_log.cna 
Use to export command output, so you don't have to grep beacon logs for info.

Usage:
```
start_log
[commands]
stop_log
```

Output:
	cobaltstrike/saved_logs/[beacon id]_yyyyMMdd_HHmmssSSS.log
