|Název parametru| Typ | Popis| Možné hodnoty|
|-|-|-|-|
| /ServerMode|Povinné|Určuje, zda by měly být nainstalovány oba servery konfigurace a proces hello, nebo jenom procesový server hello|CS<br>PS|
|/InstallLocation|Povinné|Hello složku, ve které hello součásti| Libovolné složky v počítači hello|
|/MySQLCredsFilePath|Povinné|Cesta k souboru Hello v které hello MySQL jsou uložené přihlašovací údaje serveru|Hello soubor by měl být ve formátu hello níže uvedené|
|/VaultCredsFilePath|Povinné|Hello cestu souboru s přihlašovacími údaji hello|Platná cesta k souboru|
|/EnvType|Povinné|Typ prostředí, které chcete tooprotect |VMware<br>NonVMware|
|/PSIP|Povinné|IP adresa toobe hello síťový adaptér používá pro přenos dat replikace| Libovolná platná IP adresa|
|/CSIP|Povinné|IP adresa Hello hello síťové karty, na které hello konfigurační server naslouchá na| Libovolná platná IP adresa|
|/PassphraseFilePath|Povinné|Úplná cesta toolocation Hello hello přístupové heslo souboru|Platná cesta k souboru|
|/BypassProxy|Nepovinné|Určuje, že konfigurační server hello připojí tooAzure bez serveru proxy|toodo získat tuto hodnotu z Venu|
|/ProxySettingsFilePath|Nepovinné|Nastavení proxy serveru (hello výchozí proxy server vyžaduje ověřování, nebo vlastní proxy server)|Hello soubor by měl být ve formátu hello níže uvedené|
|DataTransferSecurePort|Nepovinné|Číslo portu na toobe PSIP hello používá pro replikaci dat| Platné číslo portu (výchozí hodnota je 9433)|
|/SkipSpaceCheck|Nepovinné|Přeskočí kontrolu místa na disku mezipaměti.| |
|/AcceptThirdpartyEULA|Povinné|Příznak značí přijetí smlouvy EULA třetích stran| |
|/ShowThirdpartyEULA|Nepovinné|Zobrazí smlouvy EULA třetích stran. Pokud je zadán jako vstup, všechny ostatní parametry budou ignorovány| |
