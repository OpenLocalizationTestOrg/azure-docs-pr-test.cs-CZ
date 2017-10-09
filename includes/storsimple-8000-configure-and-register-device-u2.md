<!--author=alkohli last changed: 01/18/2017-->


#### <a name="tooconfigure-and-register-hello-device"></a>tooconfigure a zaregistrovat zařízení hello

1. Přístup k rozhraní hello prostředí Windows PowerShell na konzole sériového portu zařízení StorSimple. V tématu [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](#use-putty-to-connect-to-the-device-serial-console) pokyny. **Být přesně postupu hello zda toofollow nebo nebudete moct tooaccess hello konzoly.**

2. V relaci hello, které se otevře, stiskněte klávesu **Enter** jeden čas tooget příkazového řádku.

3. Bude výzvami toochoose hello jazyk, které chcete tooset pro vaše zařízení. Zadejte jazyk hello a potom stiskněte klávesu **Enter**.

4. V nabídce hello konzoly sériového portu, který se zobrazí, zvolte možnost 1 příliš**přihlásit úplný přístup**.
     Dokončete kroky 5-12 tooconfigure hello minimální požadovaná síťová nastavení pro vaše zařízení. **Tyto kroky konfigurace je nutné provést v aktivním řadiči zařízení hello hello toobe.** nabídce konzoly sériového portu Hello označuje stav řadiče hello v zpráva hlavičky hello. Pokud nejste připojení toohello active řadiči, odpojte a pak připojte toohello aktivního řadiče.

5. Hello příkazového řádku zadejte heslo. výchozí heslo zařízení Hello je **Heslo1**.

6. Typ hello následující příkaz: `Invoke-HcsSetupWizard`.

7. Průvodce instalací se zobrazí toohelp konfigurovat nastavení sítě hello hello zařízení. Hello hello zadejte následující informace:
   
   * IP adresa pro hello síťového rozhraní DATA 0
   * maska podsítě
   * brána
   * IP adresa primárního serveru DNS

   Ukázkový výstup je uveden níže.

    ```
        ---------------------------------------------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: 8100-SHX0991003G44MT
        Software Version: 6.3.9600.17759
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected tooController0 - Active
        ---------------------------------------------------------------

        Your device needs toobe registered with hello Microsoft Azure StorSimple Manager service. Please run 'Invoke-HcsSetupWizard' tooset up your device.

        Controller0>Invoke-HcsSetupWizard

        Which IP address family would you like tooconfigure on interface Data0?
        [4] IPv4 [6] IPv6 [B] Both (Default is "4"): 4

        Data0 IPv4 address:10.111.111.00
        Data0 IPv4 subnet: 255.255.252.0
        Data0 IPv4 gateway: 10.111.111.11

        IPv4 primary DNS server [10.222.118.154]:10.222.222.111
    ```

    <br>
    V předchozích ukázkový výstup hello uvidíte, že hello systému ověřuje nastavení sítě za každý krok v procesu hello.

     > [!NOTE]
     > Toowait může mít několik minut pro masku podsítě hello a toobe nastavení DNS hello použít. Pokud se zobrazí chybová zpráva "Zkontrolujte hello síťové připojení tooData 0", zkontrolujte hello fyzické připojení hello síťového rozhraní DATA 0 aktivního řadiče.

8. Volitelně konfigurujte proxy server. Konfigurace proxy serveru je volitelná, ale **můžete ji provést jenom v tomto kroku**. Další informace, přejděte příliš[konfigurace webového proxy serveru pro vaše zařízení](../articles/storsimple/storsimple-8000-configure-web-proxy.md).
9. Konfigurujte primární server NTP pro zařízení. Servery NTP jsou požadovány, protože zařízení musí synchronizovat čas, aby ho mohli ověřit poskytovatelé cloudových služeb. Ujistěte se, že vaše síť umožňuje toopass provoz NTP z vašeho datového centra toohello Internetu. Pokud to není možné, zadejte interní server NTP.

    Ukázkový výstup najdete níž.

    ```
        Would you like tooconfigure a web proxy?
        [Y] Yes [N] No (Default is "N"):N

        Primary NTP server [time.windows.com]:time.windows.com

    ```

10. Z bezpečnostních důvodů vyprší platnost hesla správce zařízení hello po hello první relaci a budete potřebovat toochange it teď. Až k tomu budete vyzváni, zadejte heslo správce zařízení. Platné heslo správce zařízení musí být tvořeno 8 až 15 znaky. Hello heslo musí obsahovat tři z následujících hello: malá písmena, velká písmena, číselné a speciální znaky.

    ```
        hello device administrator password must be between 8 and 15 characters. hello password must contain a combination of uppercase letters, lowercase letters, numbers and special characters.
        Administrator Password:********
        Confirm Administrator Password:********
    ```
11. Hello poslední krok v Průvodci instalací hello zaregistruje zařízení s hello služby StorSimple Manager zařízení. V takovém případě budete potřebovat hello registrační klíč služby, který jste získali v kroku 2. Po zadání hello registrační klíč může být nutné toowait pro 2 – 3 minuty, než hello zařízení je zaregistrované.
    
    > [!NOTE]
    > Stisknutím kombinace kláves Ctrl + C na Průvodce instalací hello tooexit žádné čas. Pokud jste zadali všechna nastavení sítě hello (IP adresu pro Data 0, masku podsítě a bránu), vaše záznamy zůstanou zachovány.
    
    Ukázkový výstup najdete níž.

    ```
        hello service registration key is available in hello StorSimple Manager service.
        Enter service registration key:**************************************
        Device registration is in progress. Please wait.

    ```

12. Po registraci zařízení hello se zobrazí šifrovací klíč dat služby. Klíč zkopírujte a uložte na bezpečném místě. **Tento klíč bude požadován spolu s hello služby registrace klíče tooregister další zařízení s hello služby StorSimple Manager zařízení.** Odkazovat příliš[zabezpečení zařízení StorSimple](../articles/storsimple/storsimple-security.md) Další informace o tomto klíči.
    
    ![Registrace zařízení StorSimple 7](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup1.png)
    
    > [!NOTE]
    > toocopy hello text z okna konzoly sériového portu hello, stačí vybrat hello text. Potom by mělo být možné toopaste v hello schránky nebo libovolného textového editoru. Nepoužívejte kombinaci kláves Ctrl + C toocopy hello služby datový šifrovací klíč. Pomocí kombinace kláves Ctrl + C způsobí, že jste tooexit hello Průvodce instalací. V důsledku toho se nezmění heslo správce zařízení hello a hello zařízení bude používat výchozí heslo toohello.
    
13. Ukončení hello konzoly sériového portu.
14. Vraťte se toohello portál Azure a dokončete hello následující kroky:
    
    1. Přejděte služby StorSimple Manager zařízení tooyour.
    2. Klikněte na **Zařízení**.
    3. V tabulkovém výpis zařízení hello ověřte, zda že Hello zařízení úspěšně připojilo toohello služby vyhledáním hello stavu. musí být ve stavu zařízení Hello **připraveni tooset až**.
       
        ![Stránka zařízení StorSimple](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup2.png)
       
        Toowait může potřebovat pro během několika minut pro toochange stav zařízení hello příliš**připraveni tooset až**.
       
        Pokud hello zařízení nezobrazí v tomto seznamu, pak musíte toomake se, že vaší brány firewall sítě byla nakonfigurována, jak je popsáno v [sítě požadavky pro zařízení StorSimple](../articles/storsimple/storsimple-8000-system-requirements.md). Ověřte, zda je port 9354 otevřený pro odchozí komunikaci jako hello sběrnice Toto je používáno pro komunikaci zařízení služby StorSimple Manager zařízení.

