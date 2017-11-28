<!--author=alkohli last changed: 01/18/2017-->


#### <a name="to-configure-and-register-the-device"></a><span data-ttu-id="d99c2-101">Konfigurace a registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="d99c2-101">To configure and register the device</span></span>

1. <span data-ttu-id="d99c2-102">V konzole sériového portu zařízení StorSimple spusťte rozhraní Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d99c2-102">Access the Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="d99c2-103">Další informace najdete v článku [Použití klienta PuTTY k připojení ke konzole sériového portu zařízení](#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="d99c2-103">See [Use PuTTY to connect to the device serial console](#use-putty-to-connect-to-the-device-serial-console) for instructions.</span></span> <span data-ttu-id="d99c2-104">**Postup proveďte přesně, jinak ke konzole nezískáte přístup.**</span><span class="sxs-lookup"><span data-stu-id="d99c2-104">**Be sure to follow the procedure exactly or you will not be able to access the console.**</span></span>

2. <span data-ttu-id="d99c2-105">Ve spuštěné relaci jedním stisknutím klávesy **Enter** zobrazte příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="d99c2-105">In the session that opens up, press **Enter** one time to get a command prompt.</span></span>

3. <span data-ttu-id="d99c2-106">Budete vyzváni k volbě jazyka, který chcete pro zařízení nastavit.</span><span class="sxs-lookup"><span data-stu-id="d99c2-106">You will be prompted to choose the language that you would like to set for your device.</span></span> <span data-ttu-id="d99c2-107">Zadejte jazyk a stiskněte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="d99c2-107">Specify the language, and then press **Enter**.</span></span>

4. <span data-ttu-id="d99c2-108">V zobrazené nabídce konzoly sériového portu se výběrem možnosti 1 **přihlaste s oprávněním k úplnému přístupu**.</span><span class="sxs-lookup"><span data-stu-id="d99c2-108">In the serial console menu that is presented, choose option 1 to **log in with full access**.</span></span>
     <span data-ttu-id="d99c2-109">Provedením kroků 5–12 konfigurujte minimální požadovaná síťová nastavení zařízení.</span><span class="sxs-lookup"><span data-stu-id="d99c2-109">Complete steps 5-12 to configure the minimum required network settings for your device.</span></span> <span data-ttu-id="d99c2-110">**Tyto kroky konfigurace je nutné provést v aktivním řadiči zařízení.**</span><span class="sxs-lookup"><span data-stu-id="d99c2-110">**These configuration steps need to be performed on the active controller of the device.**</span></span> <span data-ttu-id="d99c2-111">Nabídka konzoly sériového portu zobrazuje stav řadiče ve zprávě v záhlaví.</span><span class="sxs-lookup"><span data-stu-id="d99c2-111">The serial console menu indicates the controller state in the banner message.</span></span> <span data-ttu-id="d99c2-112">Pokud nejste připojeni k aktivnímu řadiči, odpojte se a potom se připojte k aktivnímu řadiči.</span><span class="sxs-lookup"><span data-stu-id="d99c2-112">If you are not connected to the active controller, disconnect and then connect to the active controller.</span></span>

5. <span data-ttu-id="d99c2-113">Na příkazovém řádku zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="d99c2-113">At the command prompt, type your password.</span></span> <span data-ttu-id="d99c2-114">Výchozí heslo zařízení je **Password1**.</span><span class="sxs-lookup"><span data-stu-id="d99c2-114">The default device password is **Password1**.</span></span>

6. <span data-ttu-id="d99c2-115">Zadejte následující příkaz: `Invoke-HcsSetupWizard`.</span><span class="sxs-lookup"><span data-stu-id="d99c2-115">Type the following command: `Invoke-HcsSetupWizard`.</span></span>

7. <span data-ttu-id="d99c2-116">Zobrazí se průvodce instalací, který vám pomůže konfigurovat nastavení sítě pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="d99c2-116">A setup wizard will appear to help you configure the network settings for the device.</span></span> <span data-ttu-id="d99c2-117">Zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="d99c2-117">Supply the the following information:</span></span>
   
   * <span data-ttu-id="d99c2-118">IP adresa síťového rozhraní DATA 0</span><span class="sxs-lookup"><span data-stu-id="d99c2-118">IP address for the DATA 0 network interface</span></span>
   * <span data-ttu-id="d99c2-119">maska podsítě</span><span class="sxs-lookup"><span data-stu-id="d99c2-119">Subnet mask</span></span>
   * <span data-ttu-id="d99c2-120">brána</span><span class="sxs-lookup"><span data-stu-id="d99c2-120">Gateway</span></span>
   * <span data-ttu-id="d99c2-121">IP adresa primárního serveru DNS</span><span class="sxs-lookup"><span data-stu-id="d99c2-121">IP address for Primary DNS server</span></span>

   <span data-ttu-id="d99c2-122">Ukázkový výstup je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="d99c2-122">A sample output is presented below.</span></span>

    ```
        ---------------------------------------------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: 8100-SHX0991003G44MT
        Software Version: 6.3.9600.17759
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Active
        ---------------------------------------------------------------

        Your device needs to be registered with the Microsoft Azure StorSimple Manager service. Please run 'Invoke-HcsSetupWizard' to set up your device.

        Controller0>Invoke-HcsSetupWizard

        Which IP address family would you like to configure on interface Data0?
        [4] IPv4 [6] IPv6 [B] Both (Default is "4"): 4

        Data0 IPv4 address:10.111.111.00
        Data0 IPv4 subnet: 255.255.252.0
        Data0 IPv4 gateway: 10.111.111.11

        IPv4 primary DNS server [10.222.118.154]:10.222.222.111
    ```

    <br>
    <span data-ttu-id="d99c2-123">V předchozím ukázkovém výstupu vidíte, že systém ověřuje nastavení sítě po provedení každého kroku postupu.</span><span class="sxs-lookup"><span data-stu-id="d99c2-123">In the preceding sample output, you can see that the system is validating network settings after each step in the process.</span></span>

     > [!NOTE]
     > <span data-ttu-id="d99c2-124">Na použití nastavení masky podsítě a služby DNS může být nutné několik minut počkat.</span><span class="sxs-lookup"><span data-stu-id="d99c2-124">You may have to wait for a few minutes for the subnet mask and the DNS settings to be applied.</span></span> <span data-ttu-id="d99c2-125">Pokud se zobrazí chybová zpráva „Zkontrolujte síťové připojení rozhraní Data 0“, zkontrolujte fyzické připojení síťového rozhraní DATA 0 aktivního řadiče k síti.</span><span class="sxs-lookup"><span data-stu-id="d99c2-125">If you get a "Check the network connectivity to Data 0" error message, check the physical network connection on the DATA 0 network interface of your active controller.</span></span>

8. <span data-ttu-id="d99c2-126">Volitelně konfigurujte proxy server.</span><span class="sxs-lookup"><span data-stu-id="d99c2-126">(Optional) configure your web proxy server.</span></span> <span data-ttu-id="d99c2-127">Konfigurace proxy serveru je volitelná, ale **můžete ji provést jenom v tomto kroku**.</span><span class="sxs-lookup"><span data-stu-id="d99c2-127">Although web proxy configuration is optional, **be aware that if you use a web proxy, you can only configure it here**.</span></span> <span data-ttu-id="d99c2-128">Další informace najdete v článku [Konfigurace webového proxy serveru pro zařízení](../articles/storsimple/storsimple-8000-configure-web-proxy.md).</span><span class="sxs-lookup"><span data-stu-id="d99c2-128">For more information, go to [Configure web proxy for your device](../articles/storsimple/storsimple-8000-configure-web-proxy.md).</span></span>
9. <span data-ttu-id="d99c2-129">Konfigurujte primární server NTP pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="d99c2-129">Configure a Primary NTP server for your device.</span></span> <span data-ttu-id="d99c2-130">Servery NTP jsou požadovány, protože zařízení musí synchronizovat čas, aby ho mohli ověřit poskytovatelé cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="d99c2-130">NTP servers are required, as your device must synchronize time so that it can authenticate with your cloud service providers.</span></span> <span data-ttu-id="d99c2-131">Ujistěte se, že vaše síť umožňuje přenos dat NTP z vašeho datového centra na internet.</span><span class="sxs-lookup"><span data-stu-id="d99c2-131">Ensure that your network allows NTP traffic to pass from your datacenter to the Internet.</span></span> <span data-ttu-id="d99c2-132">Pokud to není možné, zadejte interní server NTP.</span><span class="sxs-lookup"><span data-stu-id="d99c2-132">If this is not possible, specify an internal NTP server.</span></span>

    <span data-ttu-id="d99c2-133">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="d99c2-133">A sample output is shown below.</span></span>

    ```
        Would you like to configure a web proxy?
        [Y] Yes [N] No (Default is "N"):N

        Primary NTP server [time.windows.com]:time.windows.com

    ```

10. <span data-ttu-id="d99c2-134">Platnost hesla správce zařízení z bezpečnostních důvodů vyprší po první relaci a je nutné heslo nyní změnit.</span><span class="sxs-lookup"><span data-stu-id="d99c2-134">For security reasons, the device administrator password expires after the first session, and you will need to change it now.</span></span> <span data-ttu-id="d99c2-135">Až k tomu budete vyzváni, zadejte heslo správce zařízení.</span><span class="sxs-lookup"><span data-stu-id="d99c2-135">When prompted, provide a device administrator password.</span></span> <span data-ttu-id="d99c2-136">Platné heslo správce zařízení musí být tvořeno 8 až 15 znaky.</span><span class="sxs-lookup"><span data-stu-id="d99c2-136">A valid device administrator password must be between 8 and 15 characters.</span></span> <span data-ttu-id="d99c2-137">Heslo musí obsahovat kombinaci tří z následujících čtyř typů znaků: malá písmena, velká písmena, číslice a speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="d99c2-137">The password must contain three of the following: lowercase, uppercase, numeric, and special characters.</span></span>

    ```
        The device administrator password must be between 8 and 15 characters. The password must contain a combination of uppercase letters, lowercase letters, numbers and special characters.
        Administrator Password:********
        Confirm Administrator Password:********
    ```
11. <span data-ttu-id="d99c2-138">V posledním kroku průvodce instalací zařízení zaregistruje zařízení ve službě Správce zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="d99c2-138">The final step in the setup wizard registers your device with the StorSimple Device Manager service.</span></span> <span data-ttu-id="d99c2-139">K provedení tohoto kroku budete potřebovat registrační klíč služby, který jste získali v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="d99c2-139">For this, you will need the service registration key that you obtained in step 2.</span></span> <span data-ttu-id="d99c2-140">Po zadání registračního klíče může být nutné 2–3 minuty počkat, než se zařízení zaregistruje.</span><span class="sxs-lookup"><span data-stu-id="d99c2-140">After you supply the registration key, you may need to wait for 2-3 minutes before the device is registered.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="d99c2-141">Průvodce lze kdykoliv ukončit stisknutím kombinace kláves Ctrl+C.</span><span class="sxs-lookup"><span data-stu-id="d99c2-141">You can press Ctrl + C at any time to exit the setup wizard.</span></span> <span data-ttu-id="d99c2-142">Pokud jste zadali všechna nastavení sítě (IP adresu pro rozhraní DATA 0, masku podsítě a bránu), vaše záznamy zůstanou zachovány.</span><span class="sxs-lookup"><span data-stu-id="d99c2-142">If you have entered all the network settings (IP address for Data 0, Subnet mask, and Gateway), your entries will be retained.</span></span>
    
    <span data-ttu-id="d99c2-143">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="d99c2-143">A sample output is shown below.</span></span>

    ```
        The service registration key is available in the StorSimple Manager service.
        Enter service registration key:**************************************
        Device registration is in progress. Please wait.

    ```

12. <span data-ttu-id="d99c2-144">Po provedení registrace zařízení se zobrazí šifrovací klíč dat služby.</span><span class="sxs-lookup"><span data-stu-id="d99c2-144">After the device is registered, a Service Data Encryption key will appear.</span></span> <span data-ttu-id="d99c2-145">Klíč zkopírujte a uložte na bezpečném místě.</span><span class="sxs-lookup"><span data-stu-id="d99c2-145">Copy this key and save it in a safe location.</span></span> <span data-ttu-id="d99c2-146">**Tento klíč bude požadován spolu s registračním klíčem služby k registraci dalších zařízení ve službě Správce zařízení StorSimple.**</span><span class="sxs-lookup"><span data-stu-id="d99c2-146">**This key will be required with the service registration key to register additional devices with the StorSimple Device Manager service.**</span></span> <span data-ttu-id="d99c2-147">Další informace o tomto klíči najdete v článku [Zabezpečení zařízení StorSimple](../articles/storsimple/storsimple-security.md).</span><span class="sxs-lookup"><span data-stu-id="d99c2-147">Refer to [StorSimple security](../articles/storsimple/storsimple-security.md) for more information about this key.</span></span>
    
    ![Registrace zařízení StorSimple 7](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup1.png)
    
    > [!NOTE]
    > <span data-ttu-id="d99c2-149">Pokud chcete zkopírovat text z okna konzoly sériového portu, jednoduše ho vyberte.</span><span class="sxs-lookup"><span data-stu-id="d99c2-149">To copy the text from the serial console window, simply select the text.</span></span> <span data-ttu-id="d99c2-150">Potom by mělo být možné text vložit do schránky nebo libovolného textového editoru.</span><span class="sxs-lookup"><span data-stu-id="d99c2-150">You should then be able to paste it in the clipboard or any text editor.</span></span> <span data-ttu-id="d99c2-151">Ke zkopírování šifrovacího klíče dat služby NEPOUŽÍVEJTE kombinaci kláves Ctrl+C.</span><span class="sxs-lookup"><span data-stu-id="d99c2-151">DO NOT use Ctrl + C to copy the service data encryption key.</span></span> <span data-ttu-id="d99c2-152">Použití kombinace kláves Ctrl+C způsobí ukončení průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="d99c2-152">Using Ctrl + C will cause you to exit the setup wizard.</span></span> <span data-ttu-id="d99c2-153">Heslo správce zařízení nebude změněno a zařízení bude používat výchozí heslo.</span><span class="sxs-lookup"><span data-stu-id="d99c2-153">As a result, the device administrator password will not be changed and the device will revert to the default password.</span></span>
    
13. <span data-ttu-id="d99c2-154">Ukončete konzolu sériového portu.</span><span class="sxs-lookup"><span data-stu-id="d99c2-154">Exit the serial console.</span></span>
14. <span data-ttu-id="d99c2-155">Přejděte zpět na web Azure Portal a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d99c2-155">Return to the Azure portal, and complete the following steps:</span></span>
    
    1. <span data-ttu-id="d99c2-156">Přejděte do služby Správce zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="d99c2-156">Go to your StorSimple Device Manager service.</span></span>
    2. <span data-ttu-id="d99c2-157">Klikněte na **Zařízení**.</span><span class="sxs-lookup"><span data-stu-id="d99c2-157">Click **Devices**.</span></span>
    3. <span data-ttu-id="d99c2-158">V tabulkovém výpisu zařízení vyhledejte stav zařízení, abyste ověřili, že se úspěšně připojilo ke službě.</span><span class="sxs-lookup"><span data-stu-id="d99c2-158">In the tabular listing of devices, verify that the device has successfully connected to the service by looking up the status.</span></span> <span data-ttu-id="d99c2-159">Stav zařízení musí být **Připraveno k nastavení**.</span><span class="sxs-lookup"><span data-stu-id="d99c2-159">The device status should be **Ready to set up**.</span></span>
       
        ![Stránka zařízení StorSimple](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup2.png)
       
        <span data-ttu-id="d99c2-161">Možná budete muset několik minut počkat, než se stav zařízení změní na **Připraveno k nastavení**.</span><span class="sxs-lookup"><span data-stu-id="d99c2-161">You may need to wait for a couple of minutes for the device status to change to **Ready to set up**.</span></span>
       
        <span data-ttu-id="d99c2-162">Pokud se zařízení v seznamu nezobrazuje, je nutné zkontrolovat, jestli je brána firewall nastavená způsobem popsaným v článku o [požadavcích zařízení StorSimple na sítě](../articles/storsimple/storsimple-8000-system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="d99c2-162">If the device does not show up in this list, then you need to make sure that your firewall network was configured as described in [networking requirements for your StorSimple device](../articles/storsimple/storsimple-8000-system-requirements.md).</span></span> <span data-ttu-id="d99c2-163">Zkontrolujte, jestli je port 9354 otevřený pro odchozí komunikaci, protože ho používá sběrnice služby pro komunikaci služby Správce zařízení StorSimple se zařízením.</span><span class="sxs-lookup"><span data-stu-id="d99c2-163">Verify that port 9354 is open for outbound communication as this is used by the service bus for StorSimple Device Manager service-to-device communication.</span></span>

