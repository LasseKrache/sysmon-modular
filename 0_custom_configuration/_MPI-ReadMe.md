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
Wir verwenden hierfür eine Datei namens *"MPI-Filter.txt"*, in der wir die Regeln aufführen, die wir **NICHT** verwenden wollen. Bsp.: Wir nutzen am MPI kein Dropbox, weshalb wir die "Exclude-Regeln" für Dropbox nicht anwenden wollen. Anders formuliert: wenn Dropbox auf einem System läuft, dann wollen wir darüber Bescheid wissen!  

Vorgehen: Wir nutzen die bereits vorhandene Datei *"all_exclude_modules.txt"*, kopieren diese nach *"MPI-Filter"* und entfernen alle Exclude-Regeln (Zeilen) von Programmen, Verbindungen, etc., die tatsächlich ausgeschlossen werden sollen!  

Bsp.:  
* Wir verwenden im Haus "Cisco AnyConnect", deshalb entfernen wir diese Exclude-Einträge aus der MPI-Filter-Datei. 
* Wir nutzen kein Kaspersky, also bleiben diese Einträge in der Filterliste drin. Dies führt dazu, dass die eigentlich vorhandenen XML-Dateien mit dem **Aus**schluß von Kaspersky, **nicht** angewandt werden! Klar? ;-)


### Eigene Regeln
Wir haben eine Reihe spezifischer Programme, Verbindungen, etc. die reichlich Logs verursachen, wenn wir diese nicht ausschließen. Als Beispiel sei hier ACMP genannt oder auch die Verbindungen zu spezifischen Adressen (GWDG o.ä.). Um diese aus den Logs auszuschließen, verwenden wir eigene Regeln. Diese Regeln werden in von uns geschriebenen XML-Dateien in diesem Ordner (0_custom_configuration) abgespeichert. Generiert wird die Sysmon-Konfiguration ja bekanntlich über alle XML-Dateien in allen Verzeichnissen - also auch die von uns definierten.

### Log-Daten (Beispiele)
Network connection detected:
RuleName: technique_id=T1036,technique_name=Masquerading
UtcTime: 2023-01-18 12:11:09.535
ProcessGuid: {12c12966-e1c5-63c7-4c00-000000006800}
ProcessId: 2220
Image: C:\ProgramData\Microsoft\Windows Defender\Platform\4.18.2211.5-0\MsMpEng.exe
User: NT AUTHORITY\SYSTEM
Protocol: tcp
Initiated: true
SourceIsIpv6: false
SourceIp: 10.1.1.10
SourceHostname: -
SourcePort: 49763
SourcePortName: -
DestinationIsIpv6: false
DestinationIp: 20.82.19.171
DestinationHostname: -
DestinationPort: 443
DestinationPortName: -