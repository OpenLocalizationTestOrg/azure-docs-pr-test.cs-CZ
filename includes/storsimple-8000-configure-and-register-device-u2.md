<!--author=alkohli last changed: 01/18/2017-->


#### <a name="tooconfigure-and-register-hello-device"></a><span data-ttu-id="d7624-101">tooconfigure a zaregistrovat zařízení hello</span><span class="sxs-lookup"><span data-stu-id="d7624-101">tooconfigure and register hello device</span></span>

1. <span data-ttu-id="d7624-102">Přístup k rozhraní hello prostředí Windows PowerShell na konzole sériového portu zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="d7624-102">Access hello Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="d7624-103">V tématu [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](#use-putty-to-connect-to-the-device-serial-console) pokyny.</span><span class="sxs-lookup"><span data-stu-id="d7624-103">See [Use PuTTY tooconnect toohello device serial console](#use-putty-to-connect-to-the-device-serial-console) for instructions.</span></span> <span data-ttu-id="d7624-104">**Být přesně postupu hello zda toofollow nebo nebudete moct tooaccess hello konzoly.**</span><span class="sxs-lookup"><span data-stu-id="d7624-104">**Be sure toofollow hello procedure exactly or you will not be able tooaccess hello console.**</span></span>

2. <span data-ttu-id="d7624-105">V relaci hello, které se otevře, stiskněte klávesu **Enter** jeden čas tooget příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d7624-105">In hello session that opens up, press **Enter** one time tooget a command prompt.</span></span>

3. <span data-ttu-id="d7624-106">Bude výzvami toochoose hello jazyk, které chcete tooset pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="d7624-106">You will be prompted toochoose hello language that you would like tooset for your device.</span></span> <span data-ttu-id="d7624-107">Zadejte jazyk hello a potom stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="d7624-107">Specify hello language, and then press **Enter**.</span></span>

4. <span data-ttu-id="d7624-108">V nabídce hello konzoly sériového portu, který se zobrazí, zvolte možnost 1 příliš**přihlásit úplný přístup**.</span><span class="sxs-lookup"><span data-stu-id="d7624-108">In hello serial console menu that is presented, choose option 1 too**log in with full access**.</span></span>
     <span data-ttu-id="d7624-109">Dokončete kroky 5-12 tooconfigure hello minimální požadovaná síťová nastavení pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="d7624-109">Complete steps 5-12 tooconfigure hello minimum required network settings for your device.</span></span> <span data-ttu-id="d7624-110">**Tyto kroky konfigurace je nutné provést v aktivním řadiči zařízení hello hello toobe.**</span><span class="sxs-lookup"><span data-stu-id="d7624-110">**These configuration steps need toobe performed on hello active controller of hello device.**</span></span> <span data-ttu-id="d7624-111">nabídce konzoly sériového portu Hello označuje stav řadiče hello v zpráva hlavičky hello.</span><span class="sxs-lookup"><span data-stu-id="d7624-111">hello serial console menu indicates hello controller state in hello banner message.</span></span> <span data-ttu-id="d7624-112">Pokud nejste připojení toohello active řadiči, odpojte a pak připojte toohello aktivního řadiče.</span><span class="sxs-lookup"><span data-stu-id="d7624-112">If you are not connected toohello active controller, disconnect and then connect toohello active controller.</span></span>

5. <span data-ttu-id="d7624-113">Hello příkazového řádku zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="d7624-113">At hello command prompt, type your password.</span></span> <span data-ttu-id="d7624-114">výchozí heslo zařízení Hello je **Heslo1**.</span><span class="sxs-lookup"><span data-stu-id="d7624-114">hello default device password is **Password1**.</span></span>

6. <span data-ttu-id="d7624-115">Typ hello následující příkaz: `Invoke-HcsSetupWizard`.</span><span class="sxs-lookup"><span data-stu-id="d7624-115">Type hello following command: `Invoke-HcsSetupWizard`.</span></span>

7. <span data-ttu-id="d7624-116">Průvodce instalací se zobrazí toohelp konfigurovat nastavení sítě hello hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="d7624-116">A setup wizard will appear toohelp you configure hello network settings for hello device.</span></span> <span data-ttu-id="d7624-117">Hello hello zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="d7624-117">Supply hello hello following information:</span></span>
   
   * <span data-ttu-id="d7624-118">IP adresa pro hello síťového rozhraní DATA 0</span><span class="sxs-lookup"><span data-stu-id="d7624-118">IP address for hello DATA 0 network interface</span></span>
   * <span data-ttu-id="d7624-119">maska podsítě</span><span class="sxs-lookup"><span data-stu-id="d7624-119">Subnet mask</span></span>
   * <span data-ttu-id="d7624-120">brána</span><span class="sxs-lookup"><span data-stu-id="d7624-120">Gateway</span></span>
   * <span data-ttu-id="d7624-121">IP adresa primárního serveru DNS</span><span class="sxs-lookup"><span data-stu-id="d7624-121">IP address for Primary DNS server</span></span>

   <span data-ttu-id="d7624-122">Ukázkový výstup je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="d7624-122">A sample output is presented below.</span></span>

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
    <span data-ttu-id="d7624-123">V předchozích ukázkový výstup hello uvidíte, že hello systému ověřuje nastavení sítě za každý krok v procesu hello.</span><span class="sxs-lookup"><span data-stu-id="d7624-123">In hello preceding sample output, you can see that hello system is validating network settings after each step in hello process.</span></span>

     > [!NOTE]
     > <span data-ttu-id="d7624-124">Toowait může mít několik minut pro masku podsítě hello a toobe nastavení DNS hello použít.</span><span class="sxs-lookup"><span data-stu-id="d7624-124">You may have toowait for a few minutes for hello subnet mask and hello DNS settings toobe applied.</span></span> <span data-ttu-id="d7624-125">Pokud se zobrazí chybová zpráva "Zkontrolujte hello síťové připojení tooData 0", zkontrolujte hello fyzické připojení hello síťového rozhraní DATA 0 aktivního řadiče.</span><span class="sxs-lookup"><span data-stu-id="d7624-125">If you get a "Check hello network connectivity tooData 0" error message, check hello physical network connection on hello DATA 0 network interface of your active controller.</span></span>

8. <span data-ttu-id="d7624-126">Volitelně konfigurujte proxy server.</span><span class="sxs-lookup"><span data-stu-id="d7624-126">(Optional) configure your web proxy server.</span></span> <span data-ttu-id="d7624-127">Konfigurace proxy serveru je volitelná, ale **můžete ji provést jenom v tomto kroku**.</span><span class="sxs-lookup"><span data-stu-id="d7624-127">Although web proxy configuration is optional, **be aware that if you use a web proxy, you can only configure it here**.</span></span> <span data-ttu-id="d7624-128">Další informace, přejděte příliš[konfigurace webového proxy serveru pro vaše zařízení](../articles/storsimple/storsimple-8000-configure-web-proxy.md).</span><span class="sxs-lookup"><span data-stu-id="d7624-128">For more information, go too[Configure web proxy for your device](../articles/storsimple/storsimple-8000-configure-web-proxy.md).</span></span>
9. <span data-ttu-id="d7624-129">Konfigurujte primární server NTP pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="d7624-129">Configure a Primary NTP server for your device.</span></span> <span data-ttu-id="d7624-130">Servery NTP jsou požadovány, protože zařízení musí synchronizovat čas, aby ho mohli ověřit poskytovatelé cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="d7624-130">NTP servers are required, as your device must synchronize time so that it can authenticate with your cloud service providers.</span></span> <span data-ttu-id="d7624-131">Ujistěte se, že vaše síť umožňuje toopass provoz NTP z vašeho datového centra toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="d7624-131">Ensure that your network allows NTP traffic toopass from your datacenter toohello Internet.</span></span> <span data-ttu-id="d7624-132">Pokud to není možné, zadejte interní server NTP.</span><span class="sxs-lookup"><span data-stu-id="d7624-132">If this is not possible, specify an internal NTP server.</span></span>

    <span data-ttu-id="d7624-133">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="d7624-133">A sample output is shown below.</span></span>

    ```
        Would you like tooconfigure a web proxy?
        [Y] Yes [N] No (Default is "N"):N

        Primary NTP server [time.windows.com]:time.windows.com

    ```

10. <span data-ttu-id="d7624-134">Z bezpečnostních důvodů vyprší platnost hesla správce zařízení hello po hello první relaci a budete potřebovat toochange it teď.</span><span class="sxs-lookup"><span data-stu-id="d7624-134">For security reasons, hello device administrator password expires after hello first session, and you will need toochange it now.</span></span> <span data-ttu-id="d7624-135">Až k tomu budete vyzváni, zadejte heslo správce zařízení.</span><span class="sxs-lookup"><span data-stu-id="d7624-135">When prompted, provide a device administrator password.</span></span> <span data-ttu-id="d7624-136">Platné heslo správce zařízení musí být tvořeno 8 až 15 znaky.</span><span class="sxs-lookup"><span data-stu-id="d7624-136">A valid device administrator password must be between 8 and 15 characters.</span></span> <span data-ttu-id="d7624-137">Hello heslo musí obsahovat tři z následujících hello: malá písmena, velká písmena, číselné a speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="d7624-137">hello password must contain three of hello following: lowercase, uppercase, numeric, and special characters.</span></span>

    ```
        hello device administrator password must be between 8 and 15 characters. hello password must contain a combination of uppercase letters, lowercase letters, numbers and special characters.
        Administrator Password:********
        Confirm Administrator Password:********
    ```
11. <span data-ttu-id="d7624-138">Hello poslední krok v Průvodci instalací hello zaregistruje zařízení s hello služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="d7624-138">hello final step in hello setup wizard registers your device with hello StorSimple Device Manager service.</span></span> <span data-ttu-id="d7624-139">V takovém případě budete potřebovat hello registrační klíč služby, který jste získali v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="d7624-139">For this, you will need hello service registration key that you obtained in step 2.</span></span> <span data-ttu-id="d7624-140">Po zadání hello registrační klíč může být nutné toowait pro 2 – 3 minuty, než hello zařízení je zaregistrované.</span><span class="sxs-lookup"><span data-stu-id="d7624-140">After you supply hello registration key, you may need toowait for 2-3 minutes before hello device is registered.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="d7624-141">Stisknutím kombinace kláves Ctrl + C na Průvodce instalací hello tooexit žádné čas.</span><span class="sxs-lookup"><span data-stu-id="d7624-141">You can press Ctrl + C at any time tooexit hello setup wizard.</span></span> <span data-ttu-id="d7624-142">Pokud jste zadali všechna nastavení sítě hello (IP adresu pro Data 0, masku podsítě a bránu), vaše záznamy zůstanou zachovány.</span><span class="sxs-lookup"><span data-stu-id="d7624-142">If you have entered all hello network settings (IP address for Data 0, Subnet mask, and Gateway), your entries will be retained.</span></span>
    
    <span data-ttu-id="d7624-143">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="d7624-143">A sample output is shown below.</span></span>

    ```
        hello service registration key is available in hello StorSimple Manager service.
        Enter service registration key:**************************************
        Device registration is in progress. Please wait.

    ```

12. <span data-ttu-id="d7624-144">Po registraci zařízení hello se zobrazí šifrovací klíč dat služby.</span><span class="sxs-lookup"><span data-stu-id="d7624-144">After hello device is registered, a Service Data Encryption key will appear.</span></span> <span data-ttu-id="d7624-145">Klíč zkopírujte a uložte na bezpečném místě.</span><span class="sxs-lookup"><span data-stu-id="d7624-145">Copy this key and save it in a safe location.</span></span> <span data-ttu-id="d7624-146">**Tento klíč bude požadován spolu s hello služby registrace klíče tooregister další zařízení s hello služby StorSimple Manager zařízení.**</span><span class="sxs-lookup"><span data-stu-id="d7624-146">**This key will be required with hello service registration key tooregister additional devices with hello StorSimple Device Manager service.**</span></span> <span data-ttu-id="d7624-147">Odkazovat příliš[zabezpečení zařízení StorSimple](../articles/storsimple/storsimple-security.md) Další informace o tomto klíči.</span><span class="sxs-lookup"><span data-stu-id="d7624-147">Refer too[StorSimple security](../articles/storsimple/storsimple-security.md) for more information about this key.</span></span>
    
    ![Registrace zařízení StorSimple 7](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup1.png)
    
    > [!NOTE]
    > <span data-ttu-id="d7624-149">toocopy hello text z okna konzoly sériového portu hello, stačí vybrat hello text.</span><span class="sxs-lookup"><span data-stu-id="d7624-149">toocopy hello text from hello serial console window, simply select hello text.</span></span> <span data-ttu-id="d7624-150">Potom by mělo být možné toopaste v hello schránky nebo libovolného textového editoru.</span><span class="sxs-lookup"><span data-stu-id="d7624-150">You should then be able toopaste it in hello clipboard or any text editor.</span></span> <span data-ttu-id="d7624-151">Nepoužívejte kombinaci kláves Ctrl + C toocopy hello služby datový šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="d7624-151">DO NOT use Ctrl + C toocopy hello service data encryption key.</span></span> <span data-ttu-id="d7624-152">Pomocí kombinace kláves Ctrl + C způsobí, že jste tooexit hello Průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="d7624-152">Using Ctrl + C will cause you tooexit hello setup wizard.</span></span> <span data-ttu-id="d7624-153">V důsledku toho se nezmění heslo správce zařízení hello a hello zařízení bude používat výchozí heslo toohello.</span><span class="sxs-lookup"><span data-stu-id="d7624-153">As a result, hello device administrator password will not be changed and hello device will revert toohello default password.</span></span>
    
13. <span data-ttu-id="d7624-154">Ukončení hello konzoly sériového portu.</span><span class="sxs-lookup"><span data-stu-id="d7624-154">Exit hello serial console.</span></span>
14. <span data-ttu-id="d7624-155">Vraťte se toohello portál Azure a dokončete hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d7624-155">Return toohello Azure portal, and complete hello following steps:</span></span>
    
    1. <span data-ttu-id="d7624-156">Přejděte služby StorSimple Manager zařízení tooyour.</span><span class="sxs-lookup"><span data-stu-id="d7624-156">Go tooyour StorSimple Device Manager service.</span></span>
    2. <span data-ttu-id="d7624-157">Klikněte na **Zařízení**.</span><span class="sxs-lookup"><span data-stu-id="d7624-157">Click **Devices**.</span></span>
    3. <span data-ttu-id="d7624-158">V tabulkovém výpis zařízení hello ověřte, zda že Hello zařízení úspěšně připojilo toohello služby vyhledáním hello stavu.</span><span class="sxs-lookup"><span data-stu-id="d7624-158">In hello tabular listing of devices, verify that hello device has successfully connected toohello service by looking up hello status.</span></span> <span data-ttu-id="d7624-159">musí být ve stavu zařízení Hello **připraveni tooset až**.</span><span class="sxs-lookup"><span data-stu-id="d7624-159">hello device status should be **Ready tooset up**.</span></span>
       
        ![Stránka zařízení StorSimple](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup2.png)
       
        <span data-ttu-id="d7624-161">Toowait může potřebovat pro během několika minut pro toochange stav zařízení hello příliš**připraveni tooset až**.</span><span class="sxs-lookup"><span data-stu-id="d7624-161">You may need toowait for a couple of minutes for hello device status toochange too**Ready tooset up**.</span></span>
       
        <span data-ttu-id="d7624-162">Pokud hello zařízení nezobrazí v tomto seznamu, pak musíte toomake se, že vaší brány firewall sítě byla nakonfigurována, jak je popsáno v [sítě požadavky pro zařízení StorSimple](../articles/storsimple/storsimple-8000-system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="d7624-162">If hello device does not show up in this list, then you need toomake sure that your firewall network was configured as described in [networking requirements for your StorSimple device](../articles/storsimple/storsimple-8000-system-requirements.md).</span></span> <span data-ttu-id="d7624-163">Ověřte, zda je port 9354 otevřený pro odchozí komunikaci jako hello sběrnice Toto je používáno pro komunikaci zařízení služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="d7624-163">Verify that port 9354 is open for outbound communication as this is used by hello service bus for StorSimple Device Manager service-to-device communication.</span></span>

