<!--author=SharS last changed: 06/22/2016-->

### <a name="tooconfigure-and-register-hello-device"></a>tooconfigure a zaregistrovat zařízení hello
1. Přístup k rozhraní hello prostředí Windows PowerShell na konzole sériového portu zařízení StorSimple. V tématu [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](../articles/storsimple/storsimple-8000-deployment-walkthrough-gov-u2.md#use-putty-to-connect-to-the-device-serial-console) pokyny. **Být přesně postupu hello zda toofollow nebo nebudete moct tooaccess hello konzoly.**
2. V relaci hello, které se otevře, stiskněte klávesu **Enter** jeden čas tooget příkazového řádku.
3. Bude výzvami toochoose hello jazyk, které chcete tooset pro vaše zařízení. Zadejte jazyk hello a potom stiskněte klávesu **Enter**.
   
    ![Konfigurace a registrace zařízení StorSimple 1](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice1-gov-include.png)
4. V nabídce hello konzoly sériového portu, který se zobrazí zvolte možnost 1 toolog na s úplným přístupem.
   
    ![Registrace zařízení StorSimple 2](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice2-gov-include.png)
5. Proveďte následující kroky tooconfigure hello minimální požadovaná síťová nastavení pro vaše zařízení hello.
   
   > [!IMPORTANT]
   > Tyto kroky konfigurace je nutné provést v aktivním řadiči zařízení hello hello toobe. nabídce konzoly sériového portu Hello označuje stav řadiče hello v zpráva hlavičky hello. Pokud připojíte nejsou toohello active řadiči, odpojte a pak připojte toohello aktivního řadiče.
   
   1. Hello příkazového řádku zadejte heslo. výchozí heslo zařízení Hello je **Heslo1**.
   2. Zadejte hello následující příkaz:
      
        `Invoke-HcsSetupWizard`
   3. Průvodce instalací se zobrazí toohelp konfigurovat nastavení sítě hello hello zařízení. Hello zadejte následující informace:
      
      * IP adresu síťového rozhraní DATA 0
      * maska podsítě
      * brána
      * IP adresa primárního serveru DNS
      * IP adresa primárního serveru NTP
      
      > [!NOTE]
      > Toowait může mít několik minut pro masku podsítě hello a toobe nastavení DNS použít.
    
   4. Volitelně nakonfigurujte váš webový proxy server.
      
      > [!IMPORTANT]
      > Přestože konfigurace webového proxy serveru je volitelný, mějte na paměti, že pokud používáte webový proxy server, můžete pouze nakonfigurovat ji sem. Další informace, přejděte příliš[konfigurace webového proxy serveru pro vaše zařízení](../articles/storsimple/storsimple-configure-web-proxy.md).
     
6. Stiskněte kombinaci kláves Ctrl + C Průvodce instalací tooexit hello.
8. Spusťte následující rutinu toopoint hello zařízení toohello Microsoft Azure Government portálu (protože je ve výchozím nastavení body toohello veřejné portál Azure classic) hello. To se restartuje oba řadiče. Doporučujeme vám, že používáte dvě relace PuTTY toosimultaneously připojit tooboth řadiče, aby mohli zobrazit, když je restartování každého řadiče.
   
    `Set-CloudPlatform -AzureGovt_US`
   
   Zobrazí se potvrzovací zpráva. Přijměte výchozí hello (**Y**).
9. Spusťte následující rutinu tooresume instalace hello:
   
    `Invoke-HcsSetupWizard`
   
    ![Obnovení Průvodce instalací](./media/storsimple-configure-and-register-device-gov-u2/HCS_ResumeSetup-gov-include.png)
   
10. Přijměte hello nastavení sítě. Zobrazí se zpráva ověření a po přijetí jednotlivých nastavení.
11. Z bezpečnostních důvodů vyprší platnost hesla správce zařízení hello po hello první relaci a budete potřebovat toochange it teď. Až k tomu budete vyzváni, zadejte heslo správce zařízení. Platné heslo správce zařízení musí být tvořeno 8 až 15 znaky. Hello heslo musí obsahovat tři z následujících hello: malá písmena, velká písmena, číselné a speciální znaky.
    
    <br/>![Registrace zařízení StorSimple 5](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice5_gov-include.png)
12. Hello poslední krok v Průvodci instalací hello zaregistruje zařízení s hello služby StorSimple Manager zařízení. V takovém případě bude nutné hello registrační klíč služby, který jste získali v [krok 2: registrační klíč služby hello Get](../articles/storsimple/storsimple-8000-deployment-walkthrough-gov-u2.md#step-2-get-the-service-registration-key). Po zadání hello registrační klíč může být nutné toowait pro 2 – 3 minuty, než hello zařízení je zaregistrované.
    
    > [!NOTE]
    > Stisknutím kombinace kláves Ctrl + C na Průvodce instalací hello tooexit žádné čas. Pokud jste zadali všechna nastavení sítě hello (IP adresu pro Data 0, masku podsítě a bránu), vaše záznamy zůstanou zachovány.
    
    ![Probíhá registrace zařízení StorSimple](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegistrationProgress-gov-include.png)
13. Po registraci zařízení hello se zobrazí šifrovací klíč dat služby. Klíč zkopírujte a uložte na bezpečném místě. **Tento klíč je požadován s hello služby registrace klíče tooregister další zařízení s hello služby StorSimple Manager zařízení.** Odkazovat příliš[zabezpečení zařízení StorSimple](../articles/storsimple/storsimple-8000-security.md) Další informace o tomto klíči.
    
    ![Registrace zařízení StorSimple 7](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice7_gov-include.png)
    > [!IMPORTANT]
    > toocopy hello text z okna konzoly sériového portu hello, stačí vybrat hello text. Potom by mělo být možné toopaste v hello schránky nebo libovolného textového editoru.
    > 
    > Nepoužívejte **kombinaci kláves Ctrl + C** šifrovacího klíče dat služby toocopy hello. Pomocí **kombinaci kláves Ctrl + C** způsobí Průvodce instalací tooexit hello. V důsledku toho se nezmění heslo správce zařízení hello a hello zařízení bude používat výchozí heslo toohello.
    
14. Ukončení hello konzoly sériového portu.
15. Vraťte se toohello portálu Azure Government a dokončete hello následující kroky:
    
    1. Přejděte služby StorSimple Manager zařízení tooyour.
    2. Klikněte na **Zařízení**. Ze seznamu hello zařízení Identifikujte hello zařízení, že jste ddeploying. Ověřte, že toto zařízení hello se úspěšně připojila toohello služby vyhledáním hello stavu. musí být ve stavu zařízení Hello **Online**.
            
        Pokud je stav zařízení hello **Offline**, počkejte několik minut hello zařízení toocome online.
       
        Pokud zařízení hello je stále v režimu offline po několik minut, pak musíte toomake se, že vaší brány firewall sítě byla nakonfigurována, jak je popsáno v [sítě požadavky pro zařízení StorSimple](../articles/storsimple/storsimple-8000-system-requirements.md).
       
        Ověřte, zda je port 9354 otevřený pro odchozí komunikaci jako hello sběrnice Toto je používáno pro komunikaci služby na device Manager zařízení StorSimple.

