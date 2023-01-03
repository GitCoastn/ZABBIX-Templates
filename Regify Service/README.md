# Template App Regify App
## Essential things to say:
* Zabbix Version: 6.2
* Target Software: [Regify Server](https://www.regify.com/index.php?PageID=portfolio-regimail) - We used only regimail but the template should work for all.
* Prerequisites: Access via SSH and HTTPS
* General things: If you came here directy, please read the [general information](../readme.md)
* No longer actively supported - As we do not provide the service anymore, I'm afraid I can't add updates.
* Closed appliance: As you can see, it's a really small template but there aren't much more perimeters to check.
## Template properties
### Macros

| Macro | Description |
| --- | --- |
| {$MEMORY.UTIL.MAX} | This macro is used as a threshold in memory utilization trigger. |
| {$REGIFY.SSHPASSWORD} | Password of the SSH user you created. |
| {$REGIFY.SSHUSER} | SSH user you created. |
| {$REGIFY.URL} | Base URL (e.g. regify.cloud4you.biz) without protocol and leading/trailing slashes - also known as FQDN |
| {$VFS.FS.PUSED.MAX.CRIT} | Disk usage in percent for critical trigger. |
| {$VFS.FS.PUSED.MAX.WARN} | Disk usage in percent for warning trigger. |

### Items
* **CPU load average of the last 5 minutes** - Receiving CPU load from the SSH command "cpu avgld"
* **Current MariaDB status** - Receiving MariaDB status from the SSH command "mdb stat"
* **Current Monitoring Message** - Receiving general service status from the HTTPS call of "https://{$REGIFY.URL}/monitoring.php"
* **Free appliance memory in percent** - Receiving free memory from the SSH command "free mem"
* **SSH Users active** - Receiving active SSH users from the SSH command "usr act"
* **Used disk space on / in percent** - Receiving disk usage from the SSH command "usd spc"

### Triggers
* [Average] MariaDB status abnormal
* [High] Status not "OK", Service not running
* [Warning] Status not "OK", but Service is running
* [Average] High memory utilization for over 5min
* [Information] Users connected via SSH
* [Average] Disk space is critically low
* [Warning] Disk space is low
