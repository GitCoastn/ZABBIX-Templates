# Template App MailStore SPE by HTTP
## Essential things to say:
* Zabbix Version: 6.2
* Target Software: [MailStore SPE Edition](https://www.mailstore.com/de/produkte/mailstore-spe/)
* Prerequisites: Access to API
* General things: If you came here directy, please read the [general information](../readme.md)
## Template properties
### Macros

| Macro | Description |
| --- | --- |
| {$MAILSTORE.FREQUENCY} | Frequency of checks |
| {$MAILSTORE.DIFFTIMESPAN} | The timespan in which a change of the amount of archived mails is expected. |
| {$MAILSTORE.USER} | Adminuser for Mailstore |
| {$MAILSTORE.PASSWORD} | Password of Adminuser for Mailstore |

### Items
* **MailStore: GetServiceStatus** - Receiving Json from the API-Call "GetServiceStatus"
  * *MailStore: GetServiceStatus*/**Mailstore: Amount of Informations** - (Calculated) Counts the amount of informational messages
  * *MailStore: GetServiceStatus*/**Mailstore: First information** - (Calculated) Text of the first informational message (null if empty)
  * *MailStore: GetServiceStatus*/**Mailstore: Amount of Warnings** - (Calculated) Counts the amount of warning messages
  * *MailStore: GetServiceStatus*/**Mailstore: First warning** - (Calculated) Text of the first warning message (null if empty)
  * *MailStore: GetServiceStatus*/**Mailstore: Amount of Errors** - (Calculated) Counts the amount of error messages
  * *MailStore: GetServiceStatus*/**Mailstore: First error** - (Calculated) Text of the first error message (null if empty)
  * *MailStore: GetServiceStatus*/**MailStore: Get version** - (Calculated) Server version
  * *MailStore: GetServiceStatus*/**MailStore: Get webclient version** - (Calculated) Webclient version
  * *MailStore: GetServiceStatus*/**Mailstore: General problem indicator** - (Calculated) When you get no result, the server has an issue.
* **MailStore: GetInstances** - Receiving Json from the API-Call "GetInstances"
  * *MailStore: GetInstances*/**Discover: Instances** - (Calculated) Discovery based on GetInstances-Json
    * *MailStore: GetInstances/Discover: Instances*/**Instance {#ID} StartStop Error** - (Calculated) When there is one, StartStop-Error will be saved here
    * *MailStore: GetInstances/Discover: Instances*/**Instance {#ID} Status** - (Calculated) Receives the running status of the instance
    * *MailStore: GetInstances/Discover: Instances*/**Instance {#ID}: Get Stores** :microscope: - Instance based discovery of stores with GetStores-Json
      * *MailStore: GetInstances/Discover: Instances/Instance {#ID}: Get Stores*/**Instance {#ID} archive size** - (Calculated) Size of the mailarchive
      * *MailStore: GetInstances/Discover: Instances/Instance {#ID}: Get Stores*/**Instance {#ID} Mail Counter** - (Calculated) Amount of mails in the mailarchive
      * *MailStore: GetInstances/Discover: Instances/Instance {#ID}: Get Stores*/**Instance {#ID} problem indicator** - (Calculated) When you get no stores, the instance has an issue (process frozen etc).
### Triggers
* [Average] Mailstore: There are active errors!
* [Warning] Mailstore: There are active warnings!
* [Information] Mailstore: There are active informations!
* [Average] Mailstore: Received empty service status, there's maybe a problem
* [Information] Mailstore: Installed version changed
* [Information] Mailstore: Installed webclient version changed
  * **Discovered:** [Warning] Mailstore: Instance "{#ID}" did not archive mails in the last {$MAILSTORE.DIFFTIMESPAN:"{#ID}"} - May check?
  * **Discovered:** [Average] Mailstore: Instance "{#ID}" has a Start/Stop-Error
  * **Discovered:** [Average] Mailstore: Instance "{#ID}" is not running
  * **Discovered:** [Average] Mailstore: No store data gatherable from instance "{#ID}" - probably frozen
