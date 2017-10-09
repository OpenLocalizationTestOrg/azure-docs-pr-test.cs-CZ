<!--author=SharS last changed: 02/22/16-->

### <a name="tooconfigure-and-register-hello-device"></a><span data-ttu-id="c9637-101">tooconfigure a zaregistrovat zařízení hello</span><span class="sxs-lookup"><span data-stu-id="c9637-101">tooconfigure and register hello device</span></span>
1. <span data-ttu-id="c9637-102">Přístup k rozhraní hello prostředí Windows PowerShell na konzole sériového portu zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c9637-102">Access hello Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="c9637-103">V tématu [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](#use-putty-to-connect-to-the-device-serial-console) pokyny.</span><span class="sxs-lookup"><span data-stu-id="c9637-103">See [Use PuTTY tooconnect toohello device serial console](#use-putty-to-connect-to-the-device-serial-console) for instructions.</span></span> <span data-ttu-id="c9637-104">**Být přesně postupu hello zda toofollow nebo nebudete moct tooaccess hello konzoly.**</span><span class="sxs-lookup"><span data-stu-id="c9637-104">**Be sure toofollow hello procedure exactly or you will not be able tooaccess hello console.**</span></span>
2. <span data-ttu-id="c9637-105">V relaci hello, které se otevře stiskněte klávesu Enter jeden čas tooget příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="c9637-105">In hello session that opens up, press Enter one time tooget a command prompt.</span></span> 
3. <span data-ttu-id="c9637-106">Bude výzvami toochoose hello jazyk, které chcete tooset pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="c9637-106">You will be prompted toochoose hello language that you would like tooset for your device.</span></span> <span data-ttu-id="c9637-107">Zadejte hello jazyk a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="c9637-107">Specify hello language, and then press Enter.</span></span> 
   
    ![Konfigurace a registrace zařízení StorSimple 1](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice1-gov-include.png)
4. <span data-ttu-id="c9637-109">V nabídce hello konzoly sériového portu, který se zobrazí zvolte možnost 1 toolog na s úplným přístupem.</span><span class="sxs-lookup"><span data-stu-id="c9637-109">In hello serial console menu that is presented, choose option 1 toolog on with full access.</span></span> 
   
    ![Registrace zařízení StorSimple 2](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice2-gov-include.png)
5. <span data-ttu-id="c9637-111">Proveďte následující kroky tooconfigure hello minimální požadovaná síťová nastavení pro vaše zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="c9637-111">Perform hello following steps tooconfigure hello minimum required network settings for your device.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="c9637-112">Tyto kroky konfigurace je nutné provést v aktivním řadiči zařízení hello hello toobe.</span><span class="sxs-lookup"><span data-stu-id="c9637-112">These configuration steps need toobe performed on hello active controller of hello device.</span></span> <span data-ttu-id="c9637-113">nabídce konzoly sériového portu Hello označuje stav řadiče hello v zpráva hlavičky hello.</span><span class="sxs-lookup"><span data-stu-id="c9637-113">hello serial console menu indicates hello controller state in hello banner message.</span></span> <span data-ttu-id="c9637-114">Pokud připojíte nejsou toohello active řadiči, odpojte a pak připojte toohello aktivního řadiče.</span><span class="sxs-lookup"><span data-stu-id="c9637-114">If you are not connect toohello active controller, disconnect and then connect toohello active controller.</span></span>
   > 
   > 
   
   1. <span data-ttu-id="c9637-115">Hello příkazového řádku zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="c9637-115">At hello command prompt, type your password.</span></span> <span data-ttu-id="c9637-116">výchozí heslo zařízení Hello je **Heslo1**.</span><span class="sxs-lookup"><span data-stu-id="c9637-116">hello default device password is **Password1**.</span></span>
   2. <span data-ttu-id="c9637-117">Zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c9637-117">Type hello following command:</span></span>
      
        `Invoke-HcsSetupWizard`
   3. <span data-ttu-id="c9637-118">Průvodce instalací se zobrazí toohelp konfigurovat nastavení sítě hello hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="c9637-118">A setup wizard will appear toohelp you configure hello network settings for hello device.</span></span> <span data-ttu-id="c9637-119">Hello zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="c9637-119">Supply hello following information:</span></span> 
      
      * <span data-ttu-id="c9637-120">IP adresu síťového rozhraní DATA 0</span><span class="sxs-lookup"><span data-stu-id="c9637-120">IP address for DATA 0 network interface</span></span>
      * <span data-ttu-id="c9637-121">maska podsítě</span><span class="sxs-lookup"><span data-stu-id="c9637-121">Subnet mask</span></span>
      * <span data-ttu-id="c9637-122">brána</span><span class="sxs-lookup"><span data-stu-id="c9637-122">Gateway</span></span>
      * <span data-ttu-id="c9637-123">IP adresa primárního serveru DNS</span><span class="sxs-lookup"><span data-stu-id="c9637-123">IP address for Primary DNS server</span></span>
      * <span data-ttu-id="c9637-124">IP adresa primárního serveru NTP</span><span class="sxs-lookup"><span data-stu-id="c9637-124">IP address for Primary NTP server</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="c9637-125">Toowait může mít několik minut pro masku podsítě hello a toobe nastavení DNS použít.</span><span class="sxs-lookup"><span data-stu-id="c9637-125">You may have toowait for a few minutes for hello subnet mask and DNS settings toobe applied.</span></span> 
      > 
      > 
   4. <span data-ttu-id="c9637-126">Volitelně nakonfigurujte váš webový proxy server.</span><span class="sxs-lookup"><span data-stu-id="c9637-126">Optionally, configure your web proxy server.</span></span>
      
      > [!IMPORTANT]
      > <span data-ttu-id="c9637-127">Přestože konfigurace webového proxy serveru je volitelný, mějte na paměti, že pokud používáte webový proxy server, můžete pouze nakonfigurovat ji sem.</span><span class="sxs-lookup"><span data-stu-id="c9637-127">Although web proxy configuration is optional, be aware that if you use a web proxy, you can only configure it here.</span></span> <span data-ttu-id="c9637-128">Další informace, přejděte příliš[konfigurace webového proxy serveru pro vaše zařízení](../articles/storsimple/storsimple-configure-web-proxy.md).</span><span class="sxs-lookup"><span data-stu-id="c9637-128">For more information, go too[Configure web proxy for your device](../articles/storsimple/storsimple-configure-web-proxy.md).</span></span> 
      > 
      > 
6. <span data-ttu-id="c9637-129">Stiskněte kombinaci kláves Ctrl + C Průvodce instalací tooexit hello.</span><span class="sxs-lookup"><span data-stu-id="c9637-129">Press Ctrl + C tooexit hello setup wizard.</span></span>
7. <span data-ttu-id="c9637-130">Instalovat aktualizace hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c9637-130">Install hello updates as follows:</span></span>
   
   1. <span data-ttu-id="c9637-131">Použijte následující rutinu tooset IP adresy v obou řadičích hello hello:</span><span class="sxs-lookup"><span data-stu-id="c9637-131">Use hello following cmdlet tooset IPs on both hello controllers:</span></span>
      
      `Set-HcsNetInterface -InterfaceAlias Data0 -Controller0IPv4Address <Controller0 IP> -Controller1IPv4Address <Controller1 IP>`
   2. <span data-ttu-id="c9637-132">Na příkazovém řádku hello spustit `Get-HcsUpdateAvailability`.</span><span class="sxs-lookup"><span data-stu-id="c9637-132">At hello command prompt, run `Get-HcsUpdateAvailability`.</span></span> <span data-ttu-id="c9637-133">Mají být upozorněni, že jsou k dispozici aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c9637-133">You should be notified that updates are available.</span></span>
   3. <span data-ttu-id="c9637-134">Spusťte `Start-HcsUpdate`.</span><span class="sxs-lookup"><span data-stu-id="c9637-134">Run `Start-HcsUpdate`.</span></span> <span data-ttu-id="c9637-135">Tento příkaz můžete spustit v každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="c9637-135">You can run this command on any node.</span></span> <span data-ttu-id="c9637-136">Aktualizace se použijí na první řadič hello, hello řadič bude převzetí služeb při selhání a pak hello hello, aktualizace se použijí na jiný řadič.</span><span class="sxs-lookup"><span data-stu-id="c9637-136">Updates will be applied on hello first controller, hello controller will fail over, and then hello updates will be applied on hello other controller.</span></span>
      
      <span data-ttu-id="c9637-137">Můžete sledovat průběh hello hello aktualizace spuštěním `Get-HcsUpdateStatus`.</span><span class="sxs-lookup"><span data-stu-id="c9637-137">You can monitor hello progress of hello update by running `Get-HcsUpdateStatus`.</span></span>    
      
      <span data-ttu-id="c9637-138">Hello následující ukázkový výstup ukazuje hello aktualizace v průběhu.</span><span class="sxs-lookup"><span data-stu-id="c9637-138">hello following sample output shows hello update in progress.</span></span>
      
      ````
      Controller0>Get-HcsUpdateStatus
      RunInprogress       : True
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   : 
      ````
      
      <span data-ttu-id="c9637-139">Následující ukázkový výstup Hello označuje, že po dokončení této aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="c9637-139">hello following sample output indicates that hello update is finished.</span></span>
      
      ````
      Controller1>Get-HcsUpdateStatus
      
      RunInprogress       : False
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      
      ````
      
      <span data-ttu-id="c9637-140">To může trvat až hodin too11 tooapply všechny hello aktualizacemi, včetně hello aktualizace systému Windows.</span><span class="sxs-lookup"><span data-stu-id="c9637-140">It may take up too11 hours tooapply all hello updates, including hello Windows Updates.</span></span>

8. <span data-ttu-id="c9637-141">Po všech hello aktualizace se úspěšně nainstalovala, spusťte hello následující rutiny tooconfirm, který hello software, který bylo nasazení aktualizací správně:</span><span class="sxs-lookup"><span data-stu-id="c9637-141">After all hello updates are successfully installed, run hello following cmdlet tooconfirm that hello software updates were applied correctly:</span></span>
   
     `Get-HcsSystem`
   
    <span data-ttu-id="c9637-142">Měli byste vidět hello následující verze:</span><span class="sxs-lookup"><span data-stu-id="c9637-142">You should see hello following versions:</span></span>
   
   * <span data-ttu-id="c9637-143">HcsSoftwareVersion: 6.3.9600.17491</span><span class="sxs-lookup"><span data-stu-id="c9637-143">HcsSoftwareVersion: 6.3.9600.17491</span></span>
   * <span data-ttu-id="c9637-144">CisAgentVersion: 1.0.9037.0</span><span class="sxs-lookup"><span data-stu-id="c9637-144">CisAgentVersion: 1.0.9037.0</span></span>
   * <span data-ttu-id="c9637-145">MdsAgentVersion: 26.0.4696.1433</span><span class="sxs-lookup"><span data-stu-id="c9637-145">MdsAgentVersion: 26.0.4696.1433</span></span>
9. <span data-ttu-id="c9637-146">Spuštění hello následující rutiny tooconfirm, který hello aktualizaci firmwaru použil správně:</span><span class="sxs-lookup"><span data-stu-id="c9637-146">Run hello following cmdlet tooconfirm that hello firmware update was applied correctly:</span></span>
   
    <span data-ttu-id="c9637-147">`Start-HcsFirmwareCheck`.</span><span class="sxs-lookup"><span data-stu-id="c9637-147">`Start-HcsFirmwareCheck`.</span></span>
   
     <span data-ttu-id="c9637-148">musí být ve stavu firmware Hello **UpToDate**.</span><span class="sxs-lookup"><span data-stu-id="c9637-148">hello firmware status should be **UpToDate**.</span></span>
10. <span data-ttu-id="c9637-149">Spusťte následující rutinu toopoint hello zařízení toohello Microsoft Azure Government portálu (protože je ve výchozím nastavení body toohello veřejné portál Azure classic) hello.</span><span class="sxs-lookup"><span data-stu-id="c9637-149">Run hello following cmdlet toopoint hello device toohello Microsoft Azure Government portal (because it points toohello public Azure classic portal by default).</span></span> <span data-ttu-id="c9637-150">To se restartuje oba řadiče.</span><span class="sxs-lookup"><span data-stu-id="c9637-150">This will restart both controllers.</span></span> <span data-ttu-id="c9637-151">Doporučujeme vám, že používáte dvě relace PuTTY toosimultaneously připojit tooboth řadiče, aby mohli zobrazit, když je restartování každého řadiče.</span><span class="sxs-lookup"><span data-stu-id="c9637-151">We recommend that you use two PuTTY sessions toosimultaneously connect tooboth controllers so that you can see when each controller is restarted.</span></span>
    
     `Set-CloudPlatform -AzureGovt_US`
    
    <span data-ttu-id="c9637-152">Zobrazí se potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="c9637-152">You will see a confirmation message.</span></span> <span data-ttu-id="c9637-153">Přijměte výchozí hello (**Y**).</span><span class="sxs-lookup"><span data-stu-id="c9637-153">Accept hello default (**Y**).</span></span>
11. <span data-ttu-id="c9637-154">Spusťte následující rutinu tooresume instalace hello:</span><span class="sxs-lookup"><span data-stu-id="c9637-154">Run hello following cmdlet tooresume setup:</span></span>
    
     `Invoke-HcsSetupWizard`
    
     ![Obnovení Průvodce instalací](./media/storsimple-configure-and-register-device-gov/HCS_ResumeSetup-gov-include.png)
    
    <span data-ttu-id="c9637-156">Pokud budete pokračovat v instalaci, bude průvodce hello verze hello Update 1, (který odpovídá tooversion 17469).</span><span class="sxs-lookup"><span data-stu-id="c9637-156">When you resume setup, hello wizard will be hello Update 1 version (which corresponds tooversion 17469).</span></span> 
12. <span data-ttu-id="c9637-157">Přijměte hello nastavení sítě.</span><span class="sxs-lookup"><span data-stu-id="c9637-157">Accept hello network settings.</span></span> <span data-ttu-id="c9637-158">Zobrazí se zpráva ověření a po přijetí jednotlivých nastavení.</span><span class="sxs-lookup"><span data-stu-id="c9637-158">You will see a validation message after you accept each setting.</span></span>
13. <span data-ttu-id="c9637-159">Z bezpečnostních důvodů vyprší platnost hesla správce zařízení hello po hello první relaci a budete potřebovat toochange it teď.</span><span class="sxs-lookup"><span data-stu-id="c9637-159">For security reasons, hello device administrator password expires after hello first session, and you will need toochange it now.</span></span> <span data-ttu-id="c9637-160">Až k tomu budete vyzváni, zadejte heslo správce zařízení.</span><span class="sxs-lookup"><span data-stu-id="c9637-160">When prompted, provide a device administrator password.</span></span> <span data-ttu-id="c9637-161">Platné heslo správce zařízení musí být tvořeno 8 až 15 znaky.</span><span class="sxs-lookup"><span data-stu-id="c9637-161">A valid device administrator password must be between 8 and 15 characters.</span></span> <span data-ttu-id="c9637-162">Hello heslo musí obsahovat tři z následujících hello: malá písmena, velká písmena, číselné a speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="c9637-162">hello password must contain three of hello following: lowercase, uppercase, numeric, and special characters.</span></span>
    
    <br/>![Registrace zařízení StorSimple 5](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice5_gov-include.png)
14. <span data-ttu-id="c9637-164">Hello poslední krok v Průvodci instalací hello zaregistruje zařízení s hello služby StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="c9637-164">hello final step in hello setup wizard registers your device with hello StorSimple Manager service.</span></span> <span data-ttu-id="c9637-165">V takovém případě bude nutné hello registrační klíč služby, který jste získali v [krok 2: registrační klíč služby hello Get](#step-2-get-the-service-registration-key).</span><span class="sxs-lookup"><span data-stu-id="c9637-165">For this, you will need hello service registration key that you obtained in [Step 2: Get hello service registration key](#step-2-get-the-service-registration-key).</span></span> <span data-ttu-id="c9637-166">Po zadání hello registrační klíč může být nutné toowait pro 2 – 3 minuty, než hello zařízení je zaregistrované.</span><span class="sxs-lookup"><span data-stu-id="c9637-166">After you supply hello registration key, you may need toowait for 2-3 minutes before hello device is registered.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="c9637-167">Stisknutím kombinace kláves Ctrl + C na Průvodce instalací hello tooexit žádné čas.</span><span class="sxs-lookup"><span data-stu-id="c9637-167">You can press Ctrl + C at any time tooexit hello setup wizard.</span></span> <span data-ttu-id="c9637-168">Pokud jste zadali všechna nastavení sítě hello (IP adresu pro Data 0, masku podsítě a bránu), vaše záznamy zůstanou zachovány.</span><span class="sxs-lookup"><span data-stu-id="c9637-168">If you have entered all hello network settings (IP address for Data 0, Subnet mask, and Gateway), your entries will be retained.</span></span>
    > 
    > 
    
    ![Probíhá registrace zařízení StorSimple](./media/storsimple-configure-and-register-device-gov/HCS_RegistrationProgress-gov-include.png)
15. <span data-ttu-id="c9637-170">Po registraci zařízení hello se zobrazí šifrovací klíč dat služby.</span><span class="sxs-lookup"><span data-stu-id="c9637-170">After hello device is registered, a Service Data Encryption key will appear.</span></span> <span data-ttu-id="c9637-171">Klíč zkopírujte a uložte na bezpečném místě.</span><span class="sxs-lookup"><span data-stu-id="c9637-171">Copy this key and save it in a safe location.</span></span> <span data-ttu-id="c9637-172">**Tento klíč bude požadován spolu s hello služby registrace klíče tooregister další zařízení s hello služby StorSimple Manager.**</span><span class="sxs-lookup"><span data-stu-id="c9637-172">**This key will be required with hello service registration key tooregister additional devices with hello StorSimple Manager service.**</span></span> <span data-ttu-id="c9637-173">Odkazovat příliš[zabezpečení zařízení StorSimple](../articles/storsimple/storsimple-security.md) Další informace o tomto klíči.</span><span class="sxs-lookup"><span data-stu-id="c9637-173">Refer too[StorSimple security](../articles/storsimple/storsimple-security.md) for more information about this key.</span></span>
    
    ![Registrace zařízení StorSimple 7](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice7_gov-include.png)    
    
    > [!IMPORTANT]
    > <span data-ttu-id="c9637-175">toocopy hello text z okna konzoly sériového portu hello, stačí vybrat hello text.</span><span class="sxs-lookup"><span data-stu-id="c9637-175">toocopy hello text from hello serial console window, simply select hello text.</span></span> <span data-ttu-id="c9637-176">Potom by mělo být možné toopaste v hello schránky nebo libovolného textového editoru.</span><span class="sxs-lookup"><span data-stu-id="c9637-176">You should then be able toopaste it in hello clipboard or any text editor.</span></span> 
    > 
    > <span data-ttu-id="c9637-177">Nepoužívejte kombinaci kláves Ctrl + C toocopy hello služby datový šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="c9637-177">DO NOT use Ctrl + C toocopy hello service data encryption key.</span></span> <span data-ttu-id="c9637-178">Pomocí kombinace kláves Ctrl + C způsobí, že jste tooexit hello Průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="c9637-178">Using Ctrl + C will cause you tooexit hello setup wizard.</span></span> <span data-ttu-id="c9637-179">V důsledku toho se nezmění heslo správce zařízení hello a hello zařízení bude používat výchozí heslo toohello.</span><span class="sxs-lookup"><span data-stu-id="c9637-179">As a result, hello device administrator password will not be changed and hello device will revert toohello default password.</span></span>
    > 
    > 
16. <span data-ttu-id="c9637-180">Ukončení hello konzoly sériového portu.</span><span class="sxs-lookup"><span data-stu-id="c9637-180">Exit hello serial console.</span></span>
17. <span data-ttu-id="c9637-181">Vraťte se toohello portálu Azure Government a dokončete hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c9637-181">Return toohello Azure Government Portal, and complete hello following steps:</span></span>
    
    1. <span data-ttu-id="c9637-182">Klikněte dvakrát na vaše hello tooaccess služby StorSimple Manager **rychlý Start** stránky.</span><span class="sxs-lookup"><span data-stu-id="c9637-182">Double-click your StorSimple Manager service tooaccess hello **Quick Start** page.</span></span>
    2. <span data-ttu-id="c9637-183">Klikněte na **Zobrazit připojená zařízení**.</span><span class="sxs-lookup"><span data-stu-id="c9637-183">Click **View connected devices**.</span></span>
    3. <span data-ttu-id="c9637-184">Na hello **zařízení** ověřte, že hello zařízení úspěšně připojilo toohello služby vyhledáním hello stavu.</span><span class="sxs-lookup"><span data-stu-id="c9637-184">On hello **Devices** page, verify that hello device has successfully connected toohello service by looking up hello status.</span></span> <span data-ttu-id="c9637-185">musí být ve stavu zařízení Hello **Online**.</span><span class="sxs-lookup"><span data-stu-id="c9637-185">hello device status should be **Online**.</span></span>
       
        ![Stránka zařízení StorSimple](./media/storsimple-configure-and-register-device-gov/HCS_DeviceOnline-gov-include.png) 
       
        <span data-ttu-id="c9637-187">Pokud je stav zařízení hello **Offline**, počkejte několik minut hello zařízení toocome online.</span><span class="sxs-lookup"><span data-stu-id="c9637-187">If hello device status is **Offline**, wait for a couple of minutes for hello device toocome online.</span></span> 
       
        <span data-ttu-id="c9637-188">Pokud zařízení hello je stále v režimu offline po několik minut, pak musíte toomake se, že vaší brány firewall sítě byla nakonfigurována, jak je popsáno v [sítě požadavky pro zařízení StorSimple](../articles/storsimple/storsimple-system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="c9637-188">If hello device is still offline after a few minutes, then you need toomake sure that your firewall network was configured as described in [networking requirements for your StorSimple device](../articles/storsimple/storsimple-system-requirements.md).</span></span> 
       
        <span data-ttu-id="c9637-189">Ověřte, zda je port 9354 otevřený pro odchozí komunikaci jako hello sběrnice Toto je používáno pro komunikaci zařízení služby StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="c9637-189">Verify that port 9354 is open for outbound communication as this is used by hello service bus for StorSimple Manager service-to-device communication.</span></span>

