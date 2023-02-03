**MPI-Dokumentation**

Diese Datei enthält Infos zur Konfiguration und Nutzung von SYSMON(-Modular) am MPI.

# SYSMON (Allgemeines)

# SYSMON-MODULAR
Sysmon-Modular wird via Github von Olaf Hartong entwickelt, mehr dazu hier: https://github.com/olafhartong/sysmon-modular

Ein Video erklärt auch die prinzipielle Vorgehensweise: https://www.youtube.com/watch?v=Cx_zrM8Hu7Y

## Befehle

  * Laden des Moduls in den Arbeitsspeicher via Powershell:
    ```
    . .\Merge-SysmonXml.ps1
    ```  
    
  * Erzeugen einer Liste mit allen Regeln (in allen Unterverzeichnissen)
    ```
    Merge-AllSysmonXML -AsString -BasePath .\ | Out-File MySysmonConfig.xml
    ```

  * Erzeugen einer **angepassten** Liste mit allen Regeln (außer den in der "Filterliste" angegebenen, s.u.):
    ```
    Merge-AllSysmonXML -AsString -BasePath /Users/cg/Documents/Projekte/sysmon-modular/ -ExcludeList /Users/cg/Documents/Projekte/sysmon-modular/0_custom_configuration\MPI-Filter.txt | Out-File MySysmonConfig.xml
    ```  
    (Hierbei MÜSSEN die Pfade komplett angegeben werden!)



## MPI Konfiguration

Hier einige Infos zu den Anpassungen am MPI. 

### Filter
Wir verwenden hierfür eine Datei namens *"MPI-Filter.txt"*, in der wir die Exclude-Regeln aufführen, die wir **NICHT** verwenden wollen. Bsp.: Wir nutzen am MPI kein Dropbox, weshalb wir die "Exclude-Regeln" für Dropbox nicht anwenden wollen. Anders formuliert: wenn Dropbox auf einem System läuft, dann wollen wir darüber Bescheid wissen!  

Vorgehen: Wir nutzen die bereits vorhandene Datei *"all_exclude_modules.txt"*, kopieren diese nach *"MPI-Filter"* und entfernen alle Exclude-Regeln (Zeilen) von Programmen, Verbindungen, etc., die tatsächlich ausgeschlossen werden sollen!  

Bsp.:  
* Wir verwenden im Haus "Cisco AnyConnect", deshalb entfernen wir diese Exclude-Einträge aus der MPI-Filter-Datei. MACHEN WIR DOCH NICHT, weil auf den überwachten Servern diese Software nicht läuft!
* Wir nutzen kein Kaspersky, also bleiben diese Einträge in der Filterliste drin. Dies führt dazu, dass die eigentlich vorhandenen XML-Dateien mit dem **Aus**schluß von Kaspersky, **nicht** angewandt werden! Klar? ;-)


### Eigene Regeln
Wir haben eine Reihe spezifischer Programme, Verbindungen, etc. die reichlich Logs verursachen, wenn wir diese nicht ausschließen. Als Beispiel sei hier ACMP genannt oder auch die Verbindungen zu spezifischen Adressen (GWDG o.ä.). Um diese aus den Logs auszuschließen, verwenden wir eigene Regeln. Diese Regeln werden in von uns geschriebenen XML-Dateien in diesem Ordner (0_custom_configuration) abgespeichert. Generiert wird die Sysmon-Konfiguration ja bekanntlich über alle XML-Dateien in allen Verzeichnissen - also auch die von uns definierten.

### Log-Daten (Beispiele)
{
  "winlogbeat_event_created": "2023-01-25T13:28:42.504Z",
  "winlogbeat_agent_id": "914fb52e-4afc-4f50-9765-004e04fe4a2e",
  "winlogbeat_winlog_opcode": "Info",
  "winlogbeat_ecs_version": "1.12.0",
  "gl2_remote_ip": "10.1.20.6",
  "gl2_remote_port": 49969,
  "winlogbeat_event_code": "4624",
  "source": "VM2-W10",
  "gl2_source_input": "63bbe574831dab3b261b1f18",
  "winlogbeat_winlog_event_data_LmPackageName": "-",
  "winlogbeat_winlog_event_data_VirtualAccount": "%%1843",
  "winlogbeat_user_id": "S-1-5-21-3888206595-2008269422-3803541270-1214",
  "winlogbeat_winlog_event_data_ImpersonationLevel": "%%1833",
  "winlogbeat_host_os_platform": "windows",
  "winlogbeat_winlog_event_data_AuthenticationPackageName": "Kerberos",
  "gl2_source_node": "88a114ee-fd74-4933-b3f2-4da4f465308a",
  "winlogbeat_winlog_event_data_SubjectDomainName": "-",
  "winlogbeat_user_name": "a_cg",
  "winlogbeat_source_port": 60666,
  "winlogbeat_event_category": [
    "authentication"
  ],
  "winlogbeat_winlog_activity_id": "{2ec7f185-30ab-0000-c2f1-c72eab30d901}",
  "gl2_accounted_message_size": 5575,
  "winlogbeat_process_pid": 0,
  "winlogbeat_event_action": "logged-in",
  "winlogbeat_host_mac": [
    "00:50:56:97:6a:38"
  ],
  "streams": [
    "63bc19ad831dab3b261bd630"
  ],
  "gl2_message_id": "01GQMHVKM1E16WRPB7B385EJZ3",
  "winlogbeat_host_os_type": "windows",
  "winlogbeat_winlog_event_data_SubjectLogonId": "0x0",
  "winlogbeat_winlog_logon_type": "Network",
  "winlogbeat_@timestamp": "2023-01-25T13:28:40.961Z",
  "winlogbeat_winlog_event_data_TargetDomainName": "MPICC.DE",
  "winlogbeat_process_executable": "-",
  "winlogbeat_agent_version": "7.17.8",
  "winlogbeat_winlog_event_data_RestrictedAdminMode": "-",
  "winlogbeat_winlog_event_data_TargetUserSid": "S-1-5-21-3888206595-2008269422-3803541270-1214",
  "winlogbeat_winlog_event_data_TransmittedServices": "-",
  "winlogbeat_winlog_event_data_LogonType": "3",
  "winlogbeat_agent_ephemeral_id": "c2d952c3-372e-4685-b9eb-47cb8700d537",
  "winlogbeat_winlog_version": 2,
  "winlogbeat_@metadata_version": "7.17.8",
  "winlogbeat_winlog_record_id": 365835,
  "_id": "2fd3cf14-9cb4-11ed-bafa-0242ac160004",
  "winlogbeat_agent_hostname": "VM2-W10",
  "winlogbeat_winlog_logon_id": "0xb13731",
  "winlogbeat_host_os_build": "19045.2006",
  "winlogbeat_log_level": "information",
  "winlogbeat_host_os_kernel": "10.0.19041.2006 (WinBuild.160101.0800)",
  "winlogbeat_@metadata_type": "_doc",
  "winlogbeat_process_name": "-",
  "winlogbeat_event_type": [
    "start"
  ],
  "winlogbeat_user_domain": "MPICC.DE",
  "winlogbeat_host_os_name": "Windows 10 Enterprise",
  "winlogbeat_winlog_event_data_ElevatedToken": "%%1842",
  "winlogbeat_@metadata_beat": "winlogbeat",
  "winlogbeat_event_module": "security",
  "winlogbeat_host_ip": [
    "fe80::25c8:78c8:fbb9:ae8f",
    "10.1.20.6"
  ],
  "winlogbeat_winlog_event_data_SubjectUserSid": "S-1-0-0",
  "winlogbeat_related_ip": "10.1.1.11",
  "winlogbeat_event_provider": "Microsoft-Windows-Security-Auditing",
  "winlogbeat_host_id": "85d2a666-2a57-4f4c-a8f4-ddebb0b33580",
  "winlogbeat_winlog_event_data_TargetLinkedLogonId": "0x0",
  "beats_type": "winlogbeat",
  "winlogbeat_winlog_event_data_TargetOutboundDomainName": "-",
  "winlogbeat_winlog_event_data_KeyLength": "0",
  "winlogbeat_agent_name": "VM2-W10",
  "winlogbeat_winlog_event_id": "4624",
  "winlogbeat_event_outcome": "success",
  "winlogbeat_winlog_event_data_LogonProcessName": "Kerberos",
  "winlogbeat_host_architecture": "x86_64",
  "winlogbeat_winlog_event_data_SubjectUserName": "-",
  "timestamp": "2023-01-25T13:28:40.961Z",
  "winlogbeat_winlog_task": "Logon",
  "winlogbeat_source_ip": "10.1.1.11",
  "winlogbeat_host_name": "VM2-W10.mpicc.de",
  "winlogbeat_related_user": "a_cg",
  "winlogbeat_winlog_channel": "Security",
  "winlogbeat_winlog_computer_name": "VM2-W10.mpicc.de",
  "winlogbeat_host_hostname": "VM2-W10",
  "winlogbeat_event_kind": "event",
  "winlogbeat_host_os_family": "windows",
  "winlogbeat_winlog_event_data_TargetUserName": "a_cg",
  "winlogbeat_winlog_process_thread_id": 800,
  "winlogbeat_winlog_event_data_TargetLogonId": "0xb13731",
  "winlogbeat_winlog_api": "wineventlog",
  "message": "An account was successfully logged on.\n\nSubject:\n\tSecurity ID:\t\tS-1-0-0\n\tAccount Name:\t\t-\n\tAccount Domain:\t\t-\n\tLogon ID:\t\t0x0\n\nLogon Information:\n\tLogon Type:\t\t3\n\tRestricted Admin Mode:\t-\n\tVirtual Account:\t\tNo\n\tElevated Token:\t\tYes\n\nImpersonation Level:\t\tImpersonation\n\nNew Logon:\n\tSecurity ID:\t\tS-1-5-21-3888206595-2008269422-3803541270-1214\n\tAccount Name:\t\ta_cg\n\tAccount Domain:\t\tMPICC.DE\n\tLogon ID:\t\t0xB13731\n\tLinked Logon ID:\t\t0x0\n\tNetwork Account Name:\t-\n\tNetwork Account Domain:\t-\n\tLogon GUID:\t\t{df07ec0f-27b9-fea4-e47d-28f05d130563}\n\nProcess Information:\n\tProcess ID:\t\t0x0\n\tProcess Name:\t\t-\n\nNetwork Information:\n\tWorkstation Name:\t-\n\tSource Network Address:\t10.1.1.11\n\tSource Port:\t\t60666\n\nDetailed Authentication Information:\n\tLogon Process:\t\tKerberos\n\tAuthentication Package:\tKerberos\n\tTransited Services:\t-\n\tPackage Name (NTLM only):\t-\n\tKey Length:\t\t0\n\nThis event is generated when a logon session is created. It is generated on the computer that was accessed.\n\nThe subject fields indicate the account on the local system which requested the logon. This is most commonly a service such as the Server service, or a local process such as Winlogon.exe or Services.exe.\n\nThe logon type field indicates the kind of logon that occurred. The most common types are 2 (interactive) and 3 (network).\n\nThe New Logon fields indicate the account for whom the new logon was created, i.e. the account that was logged on.\n\nThe network fields indicate where a remote logon request originated. Workstation name is not always available and may be left blank in some cases.\n\nThe impersonation level field indicates the extent to which a process in the logon session can impersonate.\n\nThe authentication information fields provide detailed information about this specific logon request.\n\t- Logon GUID is a unique identifier that can be used to correlate this event with a KDC event.\n\t- Transited services indicate which intermediate services have participated in this logon request.\n\t- Package name indicates which sub-protocol was used among the NTLM protocols.\n\t- Key length indicates the length of the generated session key. This will be 0 if no session key was requested.",
  "winlogbeat_winlog_provider_guid": "{54849625-5478-4994-a5ba-3e3b0328c30d}",
  "winlogbeat_agent_type": "winlogbeat",
  "winlogbeat_winlog_event_data_TargetOutboundUserName": "-",
  "winlogbeat_host_os_version": "10.0",
  "winlogbeat_winlog_provider_name": "Microsoft-Windows-Security-Auditing",
  "winlogbeat_source_domain": "-",
  "winlogbeat_winlog_process_pid": 696,
  "winlogbeat_winlog_event_data_LogonGuid": "{df07ec0f-27b9-fea4-e47d-28f05d130563}",
  "winlogbeat_winlog_keywords": [
    "Audit Success"
  ]
}