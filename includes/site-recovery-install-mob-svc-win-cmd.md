1. Zkopírujte hello instalační program tooa místní složku (třeba C:\Temp) na serveru hello, které chcete tooprotect. Spusťte hello jako správce na příkazovém řádku následující příkazy:

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. tooinstall služby Mobility, spusťte následující příkaz hello:

  ```
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```
3. Teď je potřeba agenta hello toobe zaregistrována hello konfigurační Server.

  ```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a>Argumenty příkazového řádku Instalační program služby mobility

```
Usage :
UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
```

| Parametr|Typ|Popis|Možné hodnoty|
|-|-|-|-|
|/ Role|Povinné|Určuje, zda by měly být nainstalovány služby Mobility (MS) nebo MasterTarget(MT) by měly být nainstalovány.|MS </br> MT –|
|/InstallLocation|Nepovinné|Umístění, kde je nainstalovaná služba Mobility|Libovolné složky v počítači hello|
|/ Platform|Povinné|Určuje hello platformy, na které hello služba Mobility je získávání nainstalovaná </br> </br>- **VMware** : tuto hodnotu použijte, pokud k instalaci služby mobility na virtuálním počítači systémem *VMware vSphere hostitelích ESXi*, *hostitelů Hyper-V* a *Phsyical servery* </br> - **Azure** : tuto hodnotu použijte, pokud instalujete agenta na virtuálním počítači Azure IaaS| VMware </br> Azure|
|/ Tichou|Nepovinné|Určuje toorun hello instalačního programu v bezobslužném režimu| Není k dispozici|

>[!TIP]
> Hello instalační protokoly naleznete v části %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log

#### <a name="mobility-service-registration-command-line-arguments"></a>Argumenty příkazového řádku registrace služby mobility

```
Usage :
UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
```

  | Parametr|Typ|Popis|Možné hodnoty|
  |-|-|-|-|
  |/ CSEndPoint |Povinné|IP adresa serveru konfigurace hello| Všechny platnou IP adresu|
  |/PassphraseFilePath|Povinné|Umístění hello heslo |Všechny platné UNC nebo místní cesta|


>[!TIP]
> Hello AgentConfiguration protokoly najdete v části %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log
