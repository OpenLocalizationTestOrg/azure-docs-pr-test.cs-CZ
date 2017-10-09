<!--author=alkohli last changed: 12/01/15-->


#### <a name="tooconfigure-and-register-hello-device"></a>tooconfigure a zaregistrovat zařízení hello
1. Přístup k rozhraní hello prostředí Windows PowerShell na konzole sériového portu zařízení StorSimple. V tématu [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](#use-putty-to-connect-to-the-device-serial-console) pokyny. **Být přesně postupu hello zda toofollow nebo nebudete moct tooaccess hello konzoly.**
2. V relaci hello, které se otevře stiskněte klávesu Enter jeden čas tooget příkazového řádku. 
3. Bude výzvami toochoose hello jazyk, které chcete tooset pro vaše zařízení. Zadejte hello jazyk a stiskněte klávesu Enter. 
   
    ![Konfigurace a registrace zařízení StorSimple 1](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice1-include.png)
4. V nabídce hello konzoly sériového portu, který se zobrazí zvolte možnost 1 toolog na s úplným přístupem. 
   
    ![Registrace zařízení StorSimple 2](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice2-include.png)
   
     Dokončete kroky 5-12 tooconfigure hello minimální požadovaná síťová nastavení pro vaše zařízení. **Tyto kroky konfigurace je nutné provést v aktivním řadiči zařízení hello hello toobe.** nabídce konzoly sériového portu Hello označuje stav řadiče hello v zpráva hlavičky hello. Pokud nejste připojení toohello active řadiči, odpojte a pak připojte toohello aktivního řadiče.
5. Hello příkazového řádku zadejte heslo. výchozí heslo zařízení Hello je **Heslo1**.
6. Zadejte hello následující příkaz:
   
     `Invoke-HcsSetupWizard` 
7. Průvodce instalací se zobrazí toohelp konfigurovat nastavení sítě hello hello zařízení. Hello hello zadejte následující informace: 
   
   * IP adresa pro hello síťového rozhraní DATA 0
   * maska podsítě
   * brána
   * IP adresa primárního serveru DNS
   * IP adresa primárního serveru NTP
     
     > [!NOTE]
     > Toowait může mít několik minut pro masku podsítě hello a toobe nastavení DNS hello použít. Pokud dojde "hello zařízení není připraveno." chybová zpráva, zkontrolujte hello fyzické připojení hello síťového rozhraní DATA 0 aktivního řadiče.
     > 
     > 
8. Volitelně konfigurujte proxy server. Konfigurace proxy serveru je volitelná, ale **můžete ji provést jenom v tomto kroku**. Další informace, přejděte příliš[konfigurace webového proxy serveru pro vaše zařízení](../articles/storsimple/storsimple-configure-web-proxy.md). Pokud narazíte na problémy v tomto kroku, najdete pokyny, tootroubleshooting [chyb během konfigurace webového proxy serveru](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-during-the-optional-web-proxy-settings).

     > [!NOTE]
     > Stisknutím kombinace kláves Ctrl + C na Průvodce instalací hello tooexit žádné čas. Veškerá nastavení provedená před použitím tohoto příkazu budou zachována.

1. Z bezpečnostních důvodů vyprší platnost hesla správce zařízení hello po hello první relaci a budete potřebovat toochange pro následné relace. Až k tomu budete vyzváni, zadejte heslo správce zařízení. Platné heslo správce zařízení musí být tvořeno 8 až 15 znaky. Hello heslo musí obsahovat kombinaci malá písmena, velká písmena, číslice a speciální znaky.
2. Zde je také nastavit heslo Snapshot Manager zařízení StorSimple Hello. Toto heslo se používá při ověřování zařízení u hostitele se systémem Windows, ve kterém je spuštěná služba StorSimple Snapshot Manager. Po zobrazení výzvy zadejte heslo 14 too15 znak. Hello heslo musí obsahovat kombinaci tří z následujících hello: malá písmena, velká písmena, číselné a speciální znaky. 
   
   ![Registrace zařízení StorSimple 4](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice4-include.png)
   
   Můžete resetovat heslo Snapshot Manager zařízení StorSimple hello z rozhraní služby StorSimple Manager hello. Podrobný postup je uveden příliš[změna hesel zařízení StorSimple hello pomocí hello služby StorSimple Manager](../articles/storsimple/storsimple-change-passwords.md).
   
   tootroubleshoot všechny problémy v tomto kroku naleznete pokyny, tootroubleshooting [chyby související s toopasswords](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords).
3. Hello poslední krok v Průvodci instalací hello zaregistruje zařízení s hello služby StorSimple Manager. V takovém případě budete potřebovat hello registrační klíč služby, který jste získali v kroku 2. Po zadání hello registrační klíč může být nutné toowait pro 2 – 3 minuty, než hello zařízení je zaregistrované.
   
   tootroubleshoot žádné chyby registrace zařízení najdete příliš[chyb během registrace zařízení](../articles/storsimple/storsimple-troubleshoot-deployment.md#errors-during-device-registration). Podrobné řešení potíží najdete také příliš[podrobný příklad řešení potíží](../articles/storsimple/storsimple-troubleshoot-deployment.md#step-by-step-storsimple-troubleshooting-example).
4. Po registraci zařízení hello se zobrazí šifrovací klíč dat služby. Klíč zkopírujte a uložte na bezpečném místě.
   
   > [!WARNING]
   > Tento klíč bude požadován spolu s hello služby registrace klíče tooregister další zařízení s hello služby StorSimple Manager. Odkazovat příliš[zabezpečení zařízení StorSimple](../articles/storsimple/storsimple-security.md) Další informace o tomto klíči.
   > 
   > 
   
    ![Registrace zařízení StorSimple 6](./media/storsimple-configure-and-register-device/HCS_RegisterYourDevice6-include.png)
   
    toocopy hello text z okna konzoly sériového portu hello, stačí vybrat hello text. Potom by mělo být možné toopaste v hello schránky nebo libovolného textového editoru. Nepoužívejte kombinaci kláves Ctrl + C toocopy hello služby datový šifrovací klíč. Pomocí kombinace kláves Ctrl + C způsobí, že jste tooexit hello Průvodce instalací. V důsledku toho se nezmění heslo správce zařízení hello a hello heslo Snapshot Manager zařízení StorSimple a hello zařízení bude používat výchozí hesla toohello.
5. Ukončení hello konzoly sériového portu.
6. Vraťte se toohello portál Azure classic a dokončete hello následující kroky:
   
   1. Klikněte dvakrát na vaše hello tooaccess služby StorSimple Manager **rychlý Start** stránky.
   2. Klikněte na **Zobrazit připojená zařízení**.
   3. Na hello **zařízení** ověřte, že hello zařízení úspěšně připojilo toohello služby vyhledáním hello stavu. musí být ve stavu zařízení Hello **Online**. Pokud je stav zařízení hello **Offline**, počkejte několik minut hello zařízení toocome online.
   
   ![Stránka zařízení StorSimple](./media/storsimple-configure-and-register-device/HCS_DevicesPageM-include.png) 
   
   > [!IMPORTANT]
   > Po hello zařízení je online, zařaďte hello síťové kabely, které jste odpojili v hello začátku tohoto kroku.
   > 
   > 

Po úspěšném zaregistrování hello zařízení nepřejde do stavu online, můžete spustit hello `Test-HcsmConnection -Verbose` tooensure, který hello připojení k síti je v pořádku. Hello podrobné informace o použití této rutiny najdete příliš[reference k rutině pro Test-HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx).

![Dostupné video](./media/storsimple-configure-and-register-device/Video_icon.png)**Dostupné video**

toowatch video, které ukazuje, jak tooconfigure a registrace zařízení pomocí Windows Powershellu pro StorSimple, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/initialize-the-storsimple-appliance/).

