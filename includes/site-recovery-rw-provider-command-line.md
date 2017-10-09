UnifiedSetup.exe [/ ServerMode < CS/PS >] [/ InstallDrive <DriveLetter>] [/ MySQLCredsFilePath <MySQL credentials file path>] [/ VaultCredsFilePath <Vault credentials file path>] [/ EnvType < VMWare nebo NonVMWare >] [/ PSIP < IP adresa toobe používaných pro přenos dat] [/CSIP <IP address of CS toobe registered with>] [/ PassphraseFilePath <Passphrase file path>]

Parametry:

* /ServerMode: Povinné. Určuje, zda by měly být nainstalovány oba servery konfigurace a proces hello nebo pouze hello procesový server. Vstupní hodnoty: CS, PS.
* InstallLocation: Povinné. Hello složka, ve které hello součásti jsou nainstalovány.
* /MySQLCredsFilePath: Povinné. Cesta k souboru Hello v které hello MySQL jsou uložené přihlašovací údaje serveru. Hello soubor by měl být v tomto formátu:
* [MySQLCredentials]
* MySQLRootPassword = "<Password>"
* MySQLUserPassword = "<Password>"
* /VaultCredsFilePath: Povinné. Hello umístění souboru s přihlašovacími údaji hello
* /EnvType: Povinné. Hello typ instalace. Hodnoty: VMware, NonVMware.
* /PSIP a /CSIP: Povinné. IP adresa Hello hello proces a konfigurace serveru.
* /PassphraseFilePath: Povinné. Hello umístění souboru hello přístupové heslo.
* /BypassProxy: Volitelné. Určuje, že konfigurační server hello připojí tooAzure bez serveru proxy.
* /ProxySettingsFilePath: Volitelné. Nastavení proxy serveru (hello výchozí proxy server vyžaduje ověřování, nebo vlastní proxy server). Hello soubor by měl být v tomto formátu:
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "<IP adresa>"
* ProxyPort = "<Port>"
* ProxyUserName="<User Name>"
* ProxyPassword="<Password>"
* DataTransferSecurePort: Volitelné. číslo portu Hello data replikace.
* SkipSpaceCheck: Volitelné. Přeskočí kontrolu místa v mezipaměti.
* AcceptThirdpartyEULA: Povinné. Přijme smlouva EULA – hello třetích stran.
* ShowThirdpartyEULA: Povinné. Zobrazí smlouvy EULA třetích stran. Pokud je zadán jako vstup, všechny ostatní parametry budou ignorovány.
