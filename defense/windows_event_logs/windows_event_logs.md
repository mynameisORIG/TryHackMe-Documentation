# Windows Event Manager
## task1
"Event logs record events taking place in the execution of a system to provide an audit trail that can be used to understand the activity of the system and to diagnose problems. They are essential to understand the activities of complex systems, particularly in applications with little user interaction (such as server applications)."

### SIEM Capabilites
#### Main Features
 - Threat Detection
 - Investigation
 - Time to respond
#### Additional Features
 - Basic security monitoring
 - Advanced threat detection
 - Forensics & incient response
 - Log collection
 - Normalization
 - Notifications and alerts
 - Security incident detection
 - Threat response workflow
### Room Machine
Use rdestkop script to login into machine
## task 2 - Event Viewer
The Windows Event Logs are not text files that can be viewed using a text editor. However, the raw data can be translated into XML using the Windows API. The events in these log files are stored in a proprietary binary format with a .evt or .evtx extension. The log files with the .evtx file extension typically reside in *C:\Windows\System32\winevet\Logs*

### Elements of a Windows Event Log

    - **System Logs**: Records events associated with the Operating System segments. They may include information about hardware changes, device drivers, system changes, and other activities related to the device.
    - **Security Logs**: Records events connected to logon and logoff activities on a device. The system's audit policy specifies the events. The logs are an excellent source for analysts to investigate attempted or successful unauthorized activity.
    - **Application Logs**: Records events related to applications installed on a system. The main pieces of information include application errors, events, and warnings.
    - **Directory Service Events**: Active Directory changes and activities are recorded in these logs, mainly on domain controllers.
    - **File Replication Service Events**: Records events associated with Windows Servers during the sharing of Group Policies and logon scripts to domain controllers, from where they may be accessed by the users through the client servers.
    - **DNS Event Logs** : DNS servers use these logs to record domain events and to map out
    - **Custom Logs**: Events are logged by applications that require custom data storage. This allows applications to control the log size or attach other parameters, such as ACLs, for security purposes.

#### event types

 - **Error**: An event that indicates a significant problem such as loss of data or loss of functionality. For example, if a service fails to load during startup.
 - **Warning**: An event that's not necessarily significant, but may indicate a possible future problem. For example, when disk space is low, a Warning event is logged, If an application can recover froma n event without loss of functionality or data, it can generally classify the event as a Warning event.
 - **Information**: An event that describes the successful operation of an application, driver, or service. For example, when a network driver loads successfully, it may be appropriate to log an Information event. Note that it is generally inappropriate for a desktop application to log an event each time it start.
 - **Success Audit**: An event that records an auditited security access attempt that is sucessful. For example, a user's successful attempt to log on to the system is logged as a Success Audit Event.
 - **Failure Audit**: An event that records an audited security access attempt that fails. For example, if a user tries to access a network drive and fials, the attempt is logged as a Failure Audit event.

Three ways of accessing Windows event logs:
	- Event Viewer -> GUI-based
	- Wevtuil.exe -> CLI
	- Get-WinEvent -> PowerShell CLI
###Event Viewer

Event Viewer can be launched by typing *eventvwr.msc*. It is a GUI-based application that allows you to interact quickly with and analyze logs.

Event Viewer has three panes.

    1. The pane on the left provides a hierarchical tree listing of the event log providers.
    2. The pane in the middle will display a general overview and summary of the events specific to a selected provider.
    3. The pane on the right is the actions pane.

Within **Properties**, you see the log location, log size, and when it was created, modified, and last accessed. Within the Properties window, you can also see the maximum set log size and what action to take once the criteria are met. This concept is known as log rotation. These are discussions held with corporations of various sizes. How long does it take to keep logs, and when it's permissible to overwrite them with new data.

Each column of the pane presents a particular type of information as described below:

    - **Level** : Highlights the log recorded type based on the identified event types specified earlier. In this case, the log is labeled as Information.
    - **Date and Time**: Highlights the time at which the event was logged.
    - **Source**: The name of the software that logs the event is identified. From the above image, the source is PowerShell.
    - **Event ID** : This is a predefined numerical value that maps to a specific operation or event based on the log source. This makes Event IDs not unique, so Event ID 4103 in the above image is related to Executing Pipeline but will have an entirely different meaning in another event log.
    - **Task Category**: Highlights the Event Category. This entry will help you organize events so the Event Viewer can filter them. The event source defines this column.

The middle pane has a split view. More information is displayed in the bottom half of the middle pane for any event you click on.

This section has two tabs: **General** and **Details**.

    General is the default view, and the rendered data is displayed.
    The Details view has two options: Friendly view and XML view.

The Create Custom View and Filter Current Log are nearly identical. The only difference between the 2 is that the By log and By source radio buttons are greyed out in Filter Current Log. What is the reason for that? The filter you can make with this specific action only relates to the current log. Hence no reason for *by log* or *by source* to be enabled.

## task2 - questions and answers

1. For the questions below, use Event Viewer to analyze Microsoft-Windows-PowerShell/Operational log.
2. What is the Event ID for the first recorded event?

Filter the date and time and then search for the Event ID. The answer is **40961**

3. Filter on Event ID 4104. What was the 2nd command executed in the PowerShell session? 

whoami

4. What is the Task Category for Event ID 4104?

Execute a Remote Command

5. Analyze the Windows PowerShell log. What is the Task Category for Event ID 800?

Pipeline Execution Details


## task3 - wevtuil.exe
The wevtutil.exe tool "enables you to retrieve information about event logs and publishers. You can also use this command to install and uninstall event manifests, to run queries, and to export, archive, and clear logs."

```
wevtuil.exe <command>

Commands:

el  | enum-logs              List log names.
gl  | get-log                Get log configuration information.
sl  | set-log                Modify configuration of a log.
ep  | enum-publishers        List event publishers.
gp  | get-publisher          Get publisher configuration information.
im  | install-manifest       Install event publishers and logs from manifest.
um  | uninstall-manifest     Uninstall event publishers and logs from manifest.
qe  | query-events           Query events from a log or log file.
gli | get-log-info           Get log status information.
epl | export-log             Export a log.
al  | archive-log            Archive an exported log.
cl  | clear-log              Clear a log.
```

## task 3 questions and answers

1. How many log names are in the machine?

(The `Measure-Object` calculates the numeric properties of objects, and the characters, words, and lines in string objects, such as files of text.) [https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/measure-object?view=powershell-7.3]

```
wevtutil.exe el| Measure-Object
Count    : 1071 
```

2. What event files would be read when using the **query-events** command?

```wevtuil.exe qe /?
event log, log files, structured query
```

3. What option would you use to provide a path to a log file?

/lf:true

4. What is the **VALUE** for **/q**?

XPATH query

5. The questions below are based on this command: **wevtutil qe Application /c:3 /rd:true /f:text**

6. What is the log name?

Application

7. What is the **/rd** option for?

Event read direction

8. What is the **/c** option for?

Maximum number of events to read. 

## task 4 - Get-WinEvent
Get-WinEvent cmdlet "gets events from event logs and event tracing log files on local and remote computers." It provides information on event logs and event log providers. Additionally, you can combine numerous events from multiple sources into a single command and filter using XPath queries, structured XML queries, and hash table queries.

### Example 1: Get all logs from a  computer

obtaining all event logs locally, and the list starts with classic logs first, followed by new Windows Event logs. It is possible to have a log's **RecordCount** be zero or null.

```
WinEvent -ListLog *

LogMode   MaximumSizeInBytes RecordCount LogName
-------   ------------------ ----------- -------
Circular            15532032       14500 Application
Circular             1052672         117 Azure Information Protection
Circular             1052672        3015 CxAudioSvcLog
Circular            20971520             ForwardedEvents
Circular            20971520           0 HardwareEvents
```

### Example 2: Get event log providers and log names

Command here will result in the event log providers and their associated logs. The **Name** is the provider, and **LogLinks** is the log that is written to.

```
Get-WinEvent -ListProvider *

Name     : .NET Runtime
LogLinks : {Application}
Opcodes  : {}
Tasks    : {}

Name     : .NET Runtime Optimization Service
LogLinks : {Application}
Opcodes  : {}
Tasks    : {}
```

### Example 3: Log filtering

Log filtering allows you to select events from an event log. We can filter event logs using the Where-Object cmdlet as follows:

```
PS C:\Users\Administrator> Get-WinEvent -LogName Application | Where-Object { $_.ProviderName -Match 'WLMS' }

   ProviderName: WLMS

TimeCreated                     Id LevelDisplayName Message
-----------                     -- ---------------- -------
12/21/2020 4:23:47 AM          100 Information
12/18/2020 3:18:57 PM          100 Information
12/15/2020 8:50:22 AM          100 Information
12/15/2020 8:18:34 AM          100 Information
12/15/2020 7:48:34 AM          100 Information
12/14/2020 6:42:18 PM          100 Information
12/14/2020 6:12:18 PM          100 Information
12/14/2020 5:39:08 PM          100 Information
12/14/2020 5:09:08 PM          100 Information
```

When working with large event logs, per Microsoft, it's inefficient to send objects down the pipeline to a `Where-Object` command. The use of the Get-WinEvent cmdlet's **FilterHashtable** parameter is recommended to filter event logs. We can achieve the same results as above by running the following command:

```
Get-WinEvent -FilterHashtable @{
  LogName='Application' 
  ProviderName='WLMS' 
}
```

The syntax of a hash table is as follows:

Guidelines for defining a hash table are:

    Begin the hash table with an @ sign.
    Enclose the hash table in braces {}
    Enter one or more key-value pairs for the content of the hash table.
    Use an equal sign (=) to separate each key from its value.

Below is a table that displays the accepted key/value pairs for the Get-WinEvent FilterHashtable parameter.

Key name | Value data type | Accepts wildcard characters? |
LogName | `<String[]>` | yes |
ProviderName | `<String[]>` | Yes |
Path | `<String[]>` | No |
Keywords | `<Long[]` | No |
ID | `<Int32[]>` | No |
Level | `<Int32[]>` | No |
StarTime | `<DateTime>` | No |
EndTime | `<DateTime>` | No |
UserID | `<SID>` | No |
Data | `<String[]>` | No |
`<named-data>` | `<String[]>` | No |

When building a query with a hash table, Microsoft recommends making the hash table one key-value pair at a time. Event Viewer can provide quick information on what you need to build your hash table.

## task 4 questions and answers

1. Answer the following questions using the (online)[https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/Get-WinEvent?view=powershell-7.1] help documentation for **Get-WinEvent**

2. Execute the command from **Example 1** (as is). What are the names of the logs related to **OpenSSH**?

`Get-WinEvent -ListLog *`
OpenSSH/Admin, OpenSSH/Operational

3. Execute the command from **Example 8**. Instead of the string **Policy** search for **PowerShell**. What is the name of the 3rd log provider? 

`Get-WinEvent -ListProvider *Powershell*`
 Microsoft-Windows-PowerShell-DesiredStateConfiguration-FileDownloadManager

4. Execute the command from **Example 9**. Use **Microsoft-Windows-PowerShell** as the log provider. How many event ids are displayed for this event provider?

```
(Get-WinEvent -ListProvider Microsoft-Windows-Powershell).Events | Format-Table id, Description | Measure-Object
Count    : 192
```

5. How do you specify the number of events to display

`MaxEvents`

6. When using the **FilterHashtable** parameter and filtering by level, what is the value for **Informational**?

4

## task 5 - XPATH Queries

