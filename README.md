# AggressorScripts

Disclaimer: don't use these on anything you don't have the owner's explicit permission to test on. That's mean. Also illegal. 

## credpocalypse.cna
Monitor beacons and pick off users as they log in. Set the time interval (default 5m) and Credpocalypse will watch your beacons for new users in the running processes. If they aren't in the Credentials tab already, Credpocalypse will run logonpasswords.

NOTE: Your beacon will only be interrupted if logonpasswords is run. There's no callback, so I can't smother the output. :-/ 

Usage:
1. Aliases
```
begin_credpocalypse				- watch current beacon
end_credpocalypse [all]			- stop watching current/all beacon/s
credpocalypse_interval [time]	- 1m, 5m (default), 10m, 30m, 60m
```

2. Right click beacon(s) to get a pop up menu that lets you 
..* Add to watchlist
..* Remove from watchlist
..* Change time interval that Credpocalypse checks watchlist
..* View the watchlist 

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
