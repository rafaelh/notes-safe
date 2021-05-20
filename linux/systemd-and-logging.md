Commands for Systemd & log viewing

----
## Services
* systemctl list-dependencies : Show a unit’s dependencies
* systemctl list-sockets : List sockets and what activates
* systemctl list-jobs : View active systemd jobs
* systemctl list-unit-files : See unit files and their states
* systemctl list-units : Show if units are loaded/active
* systemctl get – default : List default target (like run level)
----

## Working with Services
* **systemctl start||stop||restart||reload||status something.service** - Does what it says on the tin
* **systemctl enable||disable something.service** - Enables or disables starting a service on boot
* **systemctl show service** - Show properties of a service (or other unit)
* **systemctl -H host status network** - Run any systemctl command remotely

----
## Logs
* ```journalctl``` - Show all collected log messages
* ```journalctl -u something.service``` - See the messages for the 'something' service
* ```journalctl -f``` - Follow messages as they appear
* ```journalctl -k``` - Show only kernel messages


## List failed Units
```systemctl list-units --state=failed```