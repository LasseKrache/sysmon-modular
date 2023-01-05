**MPI-Dokumentation**

Diese Datei enthält Infos zur Konfiguration und Nutzung von SYSMON(-Modular) am MPI.

# SYSMON (Allgemeines)

# SYSMON-MODULAR

  * Initiale Ausführung in Powershell via   
    ```
    . .\Merge-SysmonXml.ps1
    ```  
    Nachdem das Modul dann in den Arbeitsspeicher geladen wurde, geht es weiter.

  * Erzeugen einer Liste mit allen Regeln (in allen Unterverzeichnissen)
    ```
    Merge-AllSysmonXML -AsString -BasePath .\ | Out-File MySysmonConfig.xml
    ```

  * Erzeugen einer angepassten Liste mit allen Regeln (außer den in der "Filterliste" angegebenen, s.u.)
    ```
    Merge-AllSysmonXML -AsString -BasePath /Users/cg/Documents/Projekte/sysmon-modular/ -ExcludeList /Users/cg/Documents/Projekte/sysmon-modular/0_custom_configuration\MPI-Filter.txt | Out-File MySysmonConfig.xml
    ```  
    Hierbei MÜSSEN die Pfade komplett angegeben werden!

# MPI Konfiguration
Hier einige Infos zu den Anpassungen am MPI. 
## Filter
Wir verwenden für eine Datei namens *"MPI-Filter.txt"*, in der wir die Regeln aufführen, die wir **NICHT** verwenden wollen. Bsp.: Wir nutzen am MPI kein Dropbox, weshalb wie die "Exclude-Regeln" für Dropbox nicht anwenden wollen. Anders formuliert: wenn Dropbox auf einem System läuft, dann wollen wir darüber Bescheid wissen!  
Vorgehen: Wir nutzen die bereits vorhandene Datei *"all_exclude_modules.txt"*, kopieren diese nach *"MPI-Filter"* und entfernen alle Regeln (Zeilen), die protokolliert werden sollen!  
Bsp.:  
* Wir verwenden im Haus "Cisco AnyConnect", deshalb entfernen wir diese Einträge aus der MPI-Filter-Datei. 
* Wir nutzen kein Kaspersky, also bleiben diese Einträge drin. Dies führt dazu, dass die eigentlich vorhandenen XML-Dateien mit dem AUSschluß von Kaspersky, **nicht** angewandt werden! Klar? ;-)


## Eigene Regeln
Wir haben eine Reihe spezifischer Programme, Verbindungen, etc. die reichlich Logs verursachen, wenn wir diese nicht ausschließen. Als Beispiel sei hier ACMP genannt oder auch die Verbindungen zu spezifischen Adressen (GWDG o.ä.). Um diese aus den Logs auszuschließen, verwenden wir eigene Regeln. Diese Regeln werden in von uns geschriebenen XML-Dateien in diesem Ordner (0_custom_configuration) abgespeichert.