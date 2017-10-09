1. Zkopírujte hello instalační program tooa místní složky (například TMP) na serveru hello, které chcete tooprotect. V terminálu spusťte následující příkazy hello:
  ```
  cd /tmp
  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. tooinstall služby Mobility, spusťte následující příkaz hello:

  ```
  sudo ./install -d <Install Location> -r MS -v VmWare -q
  ```
3. Po dokončení instalace musí hello služba Mobility tooget registrované toohello konfigurační server. Spusťte následující příkaz tooregister hello služba Mobility se konfigurační server hello.

  ```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```

#### <a name="mobility-service-installer-command-line"></a>Instalátoru služby mobility příkazového řádku

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|Parametr|Typ|Popis|Možné hodnoty|
|-|-|-|-|
|-r |Povinné|Určuje, zda by měly být nainstalovány služby Mobility (MS) nebo MasterTarget(MT) by měly být nainstalovány.|MS </br> MT –|
|-d |Nepovinné|Umístění, kde bude nainstalován služba Mobility|/usr/local/ASR|
|-v|Povinné|Určuje hello platformy, na které hello služba Mobility je získávání nainstalovaná </br> </br>- **VMware** : tuto hodnotu použijte, pokud k instalaci služby mobility na virtuálním počítači systémem *VMware vSphere hostitelích ESXi*, *hostitelů Hyper-V* a *Phsyical servery* </br> - **Azure** : tuto hodnotu použijte, pokud instalujete agenta na virtuálním počítači Azure IaaS| VMware </br> Azure|
|-q|Nepovinné|Určuje toorun instalačního programu v bezobslužném režimu| Není k dispozici|


#### <a name="mobility-service-configuration-command-line"></a>Konfigurace služby mobility příkazového řádku

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|Parametr|Typ|Popis|Možné hodnoty|
|-|-|-|-|
|-i |Povinné|IP hello konfigurační Server|Libovolná platná IP adresa|
|-P |Povinné|Uložení přístupové heslo pro připojení k hello celého souboru cestě hello souboru|Všechny platné složce.|
