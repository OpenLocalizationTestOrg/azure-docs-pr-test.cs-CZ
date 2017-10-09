Hello kroky toounregister procesový server se liší v závislosti na jeho stav připojení s hello konfigurační Server.

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a>Zrušení registrace procesového serveru, který je v připojeném stavu

1. Vzdálené do hello procesový server jako správce.
2. Spusťte hello **ovládací panely** a otevřete **programy > odinstalovat program**
3. Odinstalovat program podle názvu hello **Microsoft Azure Site obnovení nebo proces konfigurace serveru**
4. Po dokončení kroku 3 můžete odinstalovat **závislosti pro Microsoft Azure Site Recovery Configuration/Process Server**.

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a>Zrušení registrace procesového serveru, který je v odpojeném stavu

> [!WARNING]
> Pokud neexistuje žádný způsob, jak toorevive hello virtuální počítač na které hello byl nainstalovaný procesového serveru je třeba použít hello pomocí následujících kroků.

1. Přihlaste se na konfiguračním serveru tooyour jako správce.
2. Otevřete příkazový řádek pro správu a Procházet adresář toohello `%ProgramData%\ASR\home\svsystems\bin`.
3. Nyní spusťte příkaz hello.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. To bude vyprázdnit hello podrobnosti hello procesového serveru ze systému hello.
