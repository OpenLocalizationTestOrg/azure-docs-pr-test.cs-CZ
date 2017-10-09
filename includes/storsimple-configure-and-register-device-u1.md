<!--author=alkohli last changed: 02/22/2016-->


### <a name="tooconfigure-and-register-hello-device"></a>tooconfigure a zaregistrovat zařízení hello
1. Přístup k rozhraní hello prostředí Windows PowerShell na konzole sériového portu zařízení StorSimple. V tématu [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](#use-putty-to-connect-to-the-device-serial-console) pokyny. **Být přesně postupu hello zda toofollow nebo nebudete moct tooaccess hello konzoly.**
2. V relaci hello, které se otevře stiskněte klávesu Enter jeden čas tooget příkazového řádku. 
3. Bude výzvami toochoose hello jazyk, které chcete tooset pro vaše zařízení. Zadejte hello jazyk a stiskněte klávesu Enter. 
   
    ![Konfigurace a registrace zařízení StorSimple 1](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice1-U1-include.png)
4. V nabídce hello konzoly sériového portu, který se zobrazí zvolte možnost 1 toolog na s úplným přístupem. 
   
    ![Registrace zařízení StorSimple 2](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice2_U1-include.png)
   
     Dokončete kroky 5-12 tooconfigure hello minimální požadovaná síťová nastavení pro vaše zařízení. **Tyto kroky konfigurace je nutné provést v aktivním řadiči zařízení hello hello toobe.** nabídce konzoly sériového portu Hello označuje stav řadiče hello v zpráva hlavičky hello. Pokud nejste připojení toohello active řadiči, odpojte a pak připojte toohello aktivního řadiče.
5. Hello příkazového řádku zadejte heslo. výchozí heslo zařízení Hello je **Heslo1**.
6. Typ hello následující příkaz: `Invoke-HcsSetupWizard`. 
7. Průvodce instalací se zobrazí toohelp konfigurovat nastavení sítě hello hello zařízení. Hello hello zadejte následující informace: 
   
   * IP adresa pro hello síťového rozhraní DATA 0
   * maska podsítě
   * brána
   * IP adresa primárního serveru DNS
     
        Všimněte si, že systém hello ověřuje nastavení sítě za každý krok v procesu hello.
     
     > [!NOTE]
     > Toowait může mít několik minut pro masku podsítě hello a toobe nastavení DNS hello použít. Pokud se zobrazí chybová zpráva "Zkontrolujte hello síťové připojení tooData 0", zkontrolujte hello fyzické připojení hello síťového rozhraní DATA 0 aktivního řadiče.
     > 
     > 
8. Volitelně konfigurujte proxy server. Konfigurace proxy serveru je volitelná, ale **můžete ji provést jenom v tomto kroku**. Další informace, přejděte příliš[konfigurace webového proxy serveru pro vaše zařízení](../articles/storsimple/storsimple-configure-web-proxy.md).
9. Konfigurujte primární server NTP pro zařízení. Servery NTP jsou požadovány, protože zařízení musí synchronizovat čas, aby ho mohli ověřit poskytovatelé cloudových služeb. Ujistěte se, že vaše síť umožňuje toopass provoz NTP z vašeho datového centra toohello Internetu. Pokud to není možné, zadejte interní server NTP. 
10. Z bezpečnostních důvodů vyprší platnost hesla správce zařízení hello po hello první relaci a budete potřebovat toochange it teď. Až k tomu budete vyzváni, zadejte heslo správce zařízení. Platné heslo správce zařízení musí být tvořeno 8 až 15 znaky. Hello heslo musí obsahovat tři z následujících hello: malá písmena, velká písmena, číselné a speciální znaky.
    
    <br/>![Registrace zařízení StorSimple 5](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice5_U1-include.png)
11. Hello poslední krok v Průvodci instalací hello zaregistruje zařízení s hello služby StorSimple Manager. V takovém případě budete potřebovat hello registrační klíč služby, který jste získali v kroku 2. Po zadání hello registrační klíč může být nutné toowait pro 2 – 3 minuty, než hello zařízení je zaregistrované.
    
    > [!NOTE]
    > Stisknutím kombinace kláves Ctrl + C na Průvodce instalací hello tooexit žádné čas. Pokud jste zadali všechna nastavení sítě hello (IP adresu pro Data 0, masku podsítě a bránu), vaše záznamy zůstanou zachovány.
    > 
    > 
    
    ![Registrace zařízení StorSimple 6](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice6_U1-include.png)
12. Po registraci zařízení hello se zobrazí šifrovací klíč dat služby. Klíč zkopírujte a uložte na bezpečném místě. **Tento klíč bude požadován spolu s hello služby registrace klíče tooregister další zařízení s hello služby StorSimple Manager.** Odkazovat příliš[zabezpečení zařízení StorSimple](../articles/storsimple/storsimple-security.md) Další informace o tomto klíči.
    
    ![Registrace zařízení StorSimple 7](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice7_U1-include.png)    
    
    > [!NOTE]
    > toocopy hello text z okna konzoly sériového portu hello, stačí vybrat hello text. Potom by mělo být možné toopaste v hello schránky nebo libovolného textového editoru. Nepoužívejte kombinaci kláves Ctrl + C toocopy hello služby datový šifrovací klíč. Pomocí kombinace kláves Ctrl + C způsobí, že jste tooexit hello Průvodce instalací. V důsledku toho se nezmění heslo správce zařízení hello a hello zařízení bude používat výchozí heslo toohello.
    > 
    > 
13. Ukončení hello konzoly sériového portu.
14. Vraťte se toohello portál Azure classic a dokončete hello následující kroky:
    
    1. Klikněte dvakrát na vaše hello tooaccess služby StorSimple Manager **rychlý Start** stránky.
    2. Klikněte na **Zobrazit připojená zařízení**.
    3. Na hello **zařízení** ověřte, že hello zařízení úspěšně připojilo toohello služby vyhledáním hello stavu. musí být ve stavu zařízení Hello **Online**.
       
        ![Stránka zařízení StorSimple](./media/storsimple-configure-and-register-device-u1/HCS_DevicesPageM_U1-include.png) 
       
        Pokud je stav zařízení hello **Offline**, počkejte několik minut hello zařízení toocome online. 
       
        Pokud zařízení hello je stále v režimu offline po několik minut, pak musíte toomake se, že vaší brány firewall sítě byla nakonfigurována, jak je popsáno v [sítě požadavky pro zařízení StorSimple](../articles/storsimple/storsimple-system-requirements.md). 
       
        Ověřte, zda je port 9354 otevřený pro odchozí komunikaci jako hello sběrnice Toto je používáno pro komunikaci StorSimple Manager Service zařízení.

