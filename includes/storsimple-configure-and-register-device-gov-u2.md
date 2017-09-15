<!--author=SharS last changed: 02/22/2016-->

### <a name="to-configure-and-register-the-device"></a><span data-ttu-id="f64cf-101">Konfigurace a registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="f64cf-101">To configure and register the device</span></span>
1. <span data-ttu-id="f64cf-102">V konzole sériového portu zařízení StorSimple spusťte rozhraní Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f64cf-102">Access the Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="f64cf-103">Další informace najdete v článku [Použití klienta PuTTY k připojení ke konzole sériového portu zařízení](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="f64cf-103">See [Use PuTTY to connect to the device serial console](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#use-putty-to-connect-to-the-device-serial-console) for instructions.</span></span> <span data-ttu-id="f64cf-104">**Postup proveďte přesně, jinak ke konzole nezískáte přístup.**</span><span class="sxs-lookup"><span data-stu-id="f64cf-104">**Be sure to follow the procedure exactly or you will not be able to access the console.**</span></span>
2. <span data-ttu-id="f64cf-105">Ve spuštěné relaci jedním stisknutím klávesy Enter zobrazte příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="f64cf-105">In the session that opens up, press Enter one time to get a command prompt.</span></span>
3. <span data-ttu-id="f64cf-106">Budete vyzváni k volbě jazyka, který chcete pro zařízení nastavit.</span><span class="sxs-lookup"><span data-stu-id="f64cf-106">You will be prompted to choose the language that you would like to set for your device.</span></span> <span data-ttu-id="f64cf-107">Zadejte jazyk a stiskněte Enter.</span><span class="sxs-lookup"><span data-stu-id="f64cf-107">Specify the language, and then press Enter.</span></span>
   
    ![Konfigurace a registrace zařízení StorSimple 1](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice1-gov-include.png)
4. <span data-ttu-id="f64cf-109">V zobrazené nabídce konzoly sériového portu se výběrem možnosti 1 přihlaste s oprávněním k úplnému přístupu.</span><span class="sxs-lookup"><span data-stu-id="f64cf-109">In the serial console menu that is presented, choose option 1 to log on with full access.</span></span>
   
    ![Registrace zařízení StorSimple 2](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice2-gov-include.png)
5. <span data-ttu-id="f64cf-111">Proveďte následující postup pro konfiguraci nastavení sítě minimální požadované pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="f64cf-111">Perform the following steps to configure the minimum required network settings for your device.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="f64cf-112">Tyto kroky konfigurace je nutné provést v aktivním řadiči zařízení.</span><span class="sxs-lookup"><span data-stu-id="f64cf-112">These configuration steps need to be performed on the active controller of the device.</span></span> <span data-ttu-id="f64cf-113">Nabídka konzoly sériového portu zobrazuje stav řadiče ve zprávě v záhlaví.</span><span class="sxs-lookup"><span data-stu-id="f64cf-113">The serial console menu indicates the controller state in the banner message.</span></span> <span data-ttu-id="f64cf-114">Pokud se připojíte k aktivnímu řadiči, odpojte a připojte se k aktivnímu řadiči.</span><span class="sxs-lookup"><span data-stu-id="f64cf-114">If you are not connect to the active controller, disconnect and then connect to the active controller.</span></span>
   > 
   > 
   
   1. <span data-ttu-id="f64cf-115">Na příkazovém řádku zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="f64cf-115">At the command prompt, type your password.</span></span> <span data-ttu-id="f64cf-116">Výchozí heslo zařízení je **Password1**.</span><span class="sxs-lookup"><span data-stu-id="f64cf-116">The default device password is **Password1**.</span></span>
   2. <span data-ttu-id="f64cf-117">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f64cf-117">Type the following command:</span></span>
      
        `Invoke-HcsSetupWizard`
   3. <span data-ttu-id="f64cf-118">Zobrazí se průvodce instalací, který vám pomůže konfigurovat nastavení sítě pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="f64cf-118">A setup wizard will appear to help you configure the network settings for the device.</span></span> <span data-ttu-id="f64cf-119">Zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="f64cf-119">Supply the following information:</span></span>
      
      * <span data-ttu-id="f64cf-120">IP adresu síťového rozhraní DATA 0</span><span class="sxs-lookup"><span data-stu-id="f64cf-120">IP address for DATA 0 network interface</span></span>
      * <span data-ttu-id="f64cf-121">maska podsítě</span><span class="sxs-lookup"><span data-stu-id="f64cf-121">Subnet mask</span></span>
      * <span data-ttu-id="f64cf-122">brána</span><span class="sxs-lookup"><span data-stu-id="f64cf-122">Gateway</span></span>
      * <span data-ttu-id="f64cf-123">IP adresa primárního serveru DNS</span><span class="sxs-lookup"><span data-stu-id="f64cf-123">IP address for Primary DNS server</span></span>
      * <span data-ttu-id="f64cf-124">IP adresa primárního serveru NTP</span><span class="sxs-lookup"><span data-stu-id="f64cf-124">IP address for Primary NTP server</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="f64cf-125">Možná budete muset Počkejte několik minut pro masku podsítě a nastavení DNS, které se má použít.</span><span class="sxs-lookup"><span data-stu-id="f64cf-125">You may have to wait for a few minutes for the subnet mask and DNS settings to be applied.</span></span>
      > 
      > 
   4. <span data-ttu-id="f64cf-126">Volitelně nakonfigurujte váš webový proxy server.</span><span class="sxs-lookup"><span data-stu-id="f64cf-126">Optionally, configure your web proxy server.</span></span>
      
      > [!IMPORTANT]
      > <span data-ttu-id="f64cf-127">Přestože konfigurace webového proxy serveru je volitelný, mějte na paměti, že pokud používáte webový proxy server, můžete pouze nakonfigurovat ji sem.</span><span class="sxs-lookup"><span data-stu-id="f64cf-127">Although web proxy configuration is optional, be aware that if you use a web proxy, you can only configure it here.</span></span> <span data-ttu-id="f64cf-128">Další informace najdete v článku [Konfigurace webového proxy serveru pro zařízení](../articles/storsimple/storsimple-configure-web-proxy.md).</span><span class="sxs-lookup"><span data-stu-id="f64cf-128">For more information, go to [Configure web proxy for your device](../articles/storsimple/storsimple-configure-web-proxy.md).</span></span>
      > 
      > 
6. <span data-ttu-id="f64cf-129">Stiskněte kombinaci kláves Ctrl + C ukončíte Průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="f64cf-129">Press Ctrl + C to exit the setup wizard.</span></span>
7. <span data-ttu-id="f64cf-130">Nainstalujte aktualizace následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f64cf-130">Install the updates as follows:</span></span>
   
   1. <span data-ttu-id="f64cf-131">Chcete-li nastavit IP adresy na obou řadičích použijte následující rutinu:</span><span class="sxs-lookup"><span data-stu-id="f64cf-131">Use the following cmdlet to set IPs on both the controllers:</span></span>
      
      `Set-HcsNetInterface -InterfaceAlias Data0 -Controller0IPv4Address <Controller0 IP> -Controller1IPv4Address <Controller1 IP>`
   2. <span data-ttu-id="f64cf-132">Na příkazovém řádku spusťte `Get-HcsUpdateAvailability`.</span><span class="sxs-lookup"><span data-stu-id="f64cf-132">At the command prompt, run `Get-HcsUpdateAvailability`.</span></span> <span data-ttu-id="f64cf-133">Mají být upozorněni, že jsou k dispozici aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f64cf-133">You should be notified that updates are available.</span></span>
   3. <span data-ttu-id="f64cf-134">Spusťte `Start-HcsUpdate`.</span><span class="sxs-lookup"><span data-stu-id="f64cf-134">Run `Start-HcsUpdate`.</span></span> <span data-ttu-id="f64cf-135">Tento příkaz můžete spustit v každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="f64cf-135">You can run this command on any node.</span></span> <span data-ttu-id="f64cf-136">Aktualizace se použijí na prvním řadiči, řadičem bude převzetí služeb při selhání a pak aktualizace se použijí na jiný řadič.</span><span class="sxs-lookup"><span data-stu-id="f64cf-136">Updates will be applied on the first controller, the controller will fail over, and then the updates will be applied on the other controller.</span></span>
      
      <span data-ttu-id="f64cf-137">Průběh aktualizace můžete sledovat spuštěním `Get-HcsUpdateStatus`.</span><span class="sxs-lookup"><span data-stu-id="f64cf-137">You can monitor the progress of the update by running `Get-HcsUpdateStatus`.</span></span>    
      
      <span data-ttu-id="f64cf-138">Následující ukázkový výstup ukazuje probíhající aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="f64cf-138">The following sample output shows the update in progress.</span></span>
      
      ````
      Controller0>Get-HcsUpdateStatus
      RunInprogress       : True
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      ````
      
      <span data-ttu-id="f64cf-139">Následující ukázkový výstup ukazuje dokončení aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f64cf-139">The following sample output indicates that the update is finished.</span></span>
      
      ```
      Controller1>Get-HcsUpdateStatus
      
      RunInprogress       : False
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      ```
      
      <span data-ttu-id="f64cf-140">To může trvat až 11 hodin použít všechny aktualizace, včetně aktualizací Windows.</span><span class="sxs-lookup"><span data-stu-id="f64cf-140">It may take up to 11 hours to apply all the updates, including the Windows Updates.</span></span>
8. <span data-ttu-id="f64cf-141">Spusťte následující rutinu tak, aby odkazoval zařízení na portálu Microsoft Azure Government (protože je ve výchozím nastavení odkazuje na veřejnou portál Azure classic).</span><span class="sxs-lookup"><span data-stu-id="f64cf-141">Run the following cmdlet to point the device to the Microsoft Azure Government portal (because it points to the public Azure classic portal by default).</span></span> <span data-ttu-id="f64cf-142">To se restartuje oba řadiče.</span><span class="sxs-lookup"><span data-stu-id="f64cf-142">This will restart both controllers.</span></span> <span data-ttu-id="f64cf-143">Doporučujeme vám, že používáte dvě relace PuTTY lze najednou připojit pro oba řadiče, aby mohli zobrazit, když je restartování každého řadiče.</span><span class="sxs-lookup"><span data-stu-id="f64cf-143">We recommend that you use two PuTTY sessions to simultaneously connect to both controllers so that you can see when each controller is restarted.</span></span>
   
    `Set-CloudPlatform -AzureGovt_US`
   
   <span data-ttu-id="f64cf-144">Zobrazí se potvrzovací zpráva.</span><span class="sxs-lookup"><span data-stu-id="f64cf-144">You will see a confirmation message.</span></span> <span data-ttu-id="f64cf-145">Přijměte výchozí nastavení (**Y**).</span><span class="sxs-lookup"><span data-stu-id="f64cf-145">Accept the default (**Y**).</span></span>
9. <span data-ttu-id="f64cf-146">Spusťte následující rutiny můžete pokračovat v instalaci:</span><span class="sxs-lookup"><span data-stu-id="f64cf-146">Run the following cmdlet to resume setup:</span></span>
   
    `Invoke-HcsSetupWizard`
   
    ![Obnovení Průvodce instalací](./media/storsimple-configure-and-register-device-gov-u2/HCS_ResumeSetup-gov-include.png)
   
   <span data-ttu-id="f64cf-148">Pokud budete pokračovat v instalaci, bude průvodce verzi Update 2.</span><span class="sxs-lookup"><span data-stu-id="f64cf-148">When you resume setup, the wizard will be the Update 2 version.</span></span>
10. <span data-ttu-id="f64cf-149">Přijměte nastavení sítě.</span><span class="sxs-lookup"><span data-stu-id="f64cf-149">Accept the network settings.</span></span> <span data-ttu-id="f64cf-150">Zobrazí se zpráva ověření a po přijetí jednotlivých nastavení.</span><span class="sxs-lookup"><span data-stu-id="f64cf-150">You will see a validation message after you accept each setting.</span></span>
11. <span data-ttu-id="f64cf-151">Platnost hesla správce zařízení z bezpečnostních důvodů vyprší po první relaci a je nutné heslo nyní změnit.</span><span class="sxs-lookup"><span data-stu-id="f64cf-151">For security reasons, the device administrator password expires after the first session, and you will need to change it now.</span></span> <span data-ttu-id="f64cf-152">Až k tomu budete vyzváni, zadejte heslo správce zařízení.</span><span class="sxs-lookup"><span data-stu-id="f64cf-152">When prompted, provide a device administrator password.</span></span> <span data-ttu-id="f64cf-153">Platné heslo správce zařízení musí být tvořeno 8 až 15 znaky.</span><span class="sxs-lookup"><span data-stu-id="f64cf-153">A valid device administrator password must be between 8 and 15 characters.</span></span> <span data-ttu-id="f64cf-154">Heslo musí obsahovat kombinaci tří z následujících čtyř typů znaků: malá písmena, velká písmena, číslice a speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="f64cf-154">The password must contain three of the following: lowercase, uppercase, numeric, and special characters.</span></span>
    
    <br/>![Registrace zařízení StorSimple 5](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice5_gov-include.png)
12. <span data-ttu-id="f64cf-156">V posledním kroku instalace zařízení průvodce registruje zařízení ve službě StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="f64cf-156">The final step in the setup wizard registers your device with the StorSimple Manager service.</span></span> <span data-ttu-id="f64cf-157">V takovém případě budete potřebovat registrační klíč služby, který jste získali v [krok 2: získání registračního klíče služby](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#step-2-get-the-service-registration-key).</span><span class="sxs-lookup"><span data-stu-id="f64cf-157">For this, you will need the service registration key that you obtained in [Step 2: Get the service registration key](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#step-2-get-the-service-registration-key).</span></span> <span data-ttu-id="f64cf-158">Po zadání registračního klíče může být nutné 2–3 minuty počkat, než se zařízení zaregistruje.</span><span class="sxs-lookup"><span data-stu-id="f64cf-158">After you supply the registration key, you may need to wait for 2-3 minutes before the device is registered.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="f64cf-159">Průvodce lze kdykoliv ukončit stisknutím kombinace kláves Ctrl+C.</span><span class="sxs-lookup"><span data-stu-id="f64cf-159">You can press Ctrl + C at any time to exit the setup wizard.</span></span> <span data-ttu-id="f64cf-160">Pokud jste zadali všechna nastavení sítě (IP adresu pro rozhraní DATA 0, masku podsítě a bránu), vaše záznamy zůstanou zachovány.</span><span class="sxs-lookup"><span data-stu-id="f64cf-160">If you have entered all the network settings (IP address for Data 0, Subnet mask, and Gateway), your entries will be retained.</span></span>
    > 
    > 
    
    ![Probíhá registrace zařízení StorSimple](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegistrationProgress-gov-include.png)
13. <span data-ttu-id="f64cf-162">Po provedení registrace zařízení se zobrazí šifrovací klíč dat služby.</span><span class="sxs-lookup"><span data-stu-id="f64cf-162">After the device is registered, a Service Data Encryption key will appear.</span></span> <span data-ttu-id="f64cf-163">Klíč zkopírujte a uložte na bezpečném místě.</span><span class="sxs-lookup"><span data-stu-id="f64cf-163">Copy this key and save it in a safe location.</span></span> <span data-ttu-id="f64cf-164">**Tento klíč bude požadován spolu s registračním klíčem služby k registraci dalších zařízení ve službě StorSimple Manager.**</span><span class="sxs-lookup"><span data-stu-id="f64cf-164">**This key will be required with the service registration key to register additional devices with the StorSimple Manager service.**</span></span> <span data-ttu-id="f64cf-165">Další informace o tomto klíči najdete v článku [Zabezpečení zařízení StorSimple](../articles/storsimple/storsimple-security.md).</span><span class="sxs-lookup"><span data-stu-id="f64cf-165">Refer to [StorSimple security](../articles/storsimple/storsimple-security.md) for more information about this key.</span></span>
    
    ![Registrace zařízení StorSimple 7](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice7_gov-include.png)    
    
    > [!IMPORTANT]
    > <span data-ttu-id="f64cf-167">Pokud chcete zkopírovat text z okna konzoly sériového portu, jednoduše ho vyberte.</span><span class="sxs-lookup"><span data-stu-id="f64cf-167">To copy the text from the serial console window, simply select the text.</span></span> <span data-ttu-id="f64cf-168">Potom by mělo být možné text vložit do schránky nebo libovolného textového editoru.</span><span class="sxs-lookup"><span data-stu-id="f64cf-168">You should then be able to paste it in the clipboard or any text editor.</span></span>
    > 
    > <span data-ttu-id="f64cf-169">Ke zkopírování šifrovacího klíče dat služby NEPOUŽÍVEJTE kombinaci kláves Ctrl+C.</span><span class="sxs-lookup"><span data-stu-id="f64cf-169">DO NOT use Ctrl + C to copy the service data encryption key.</span></span> <span data-ttu-id="f64cf-170">Použití kombinace kláves Ctrl+C způsobí ukončení průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="f64cf-170">Using Ctrl + C will cause you to exit the setup wizard.</span></span> <span data-ttu-id="f64cf-171">Heslo správce zařízení nebude změněno a zařízení bude používat výchozí heslo.</span><span class="sxs-lookup"><span data-stu-id="f64cf-171">As a result, the device administrator password will not be changed and the device will revert to the default password.</span></span>
    > 
    > 
14. <span data-ttu-id="f64cf-172">Ukončete konzolu sériového portu.</span><span class="sxs-lookup"><span data-stu-id="f64cf-172">Exit the serial console.</span></span>
15. <span data-ttu-id="f64cf-173">Vraťte se k portálu Azure Government a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f64cf-173">Return to the Azure Government Portal, and complete the following steps:</span></span>
    
    1. <span data-ttu-id="f64cf-174">Dvojitým kliknutím na službu StorSimple Manager zobrazte stránku **Rychlý start**.</span><span class="sxs-lookup"><span data-stu-id="f64cf-174">Double-click your StorSimple Manager service to access the **Quick Start** page.</span></span>
    2. <span data-ttu-id="f64cf-175">Klikněte na **Zobrazit připojená zařízení**.</span><span class="sxs-lookup"><span data-stu-id="f64cf-175">Click **View connected devices**.</span></span>
    3. <span data-ttu-id="f64cf-176">Na stránce **Zařízení** kontrolou stavu ověřte, že se zařízení úspěšně připojilo ke službě.</span><span class="sxs-lookup"><span data-stu-id="f64cf-176">On the **Devices** page, verify that the device has successfully connected to the service by looking up the status.</span></span> <span data-ttu-id="f64cf-177">Zařízení musí být ve stavu **Online**.</span><span class="sxs-lookup"><span data-stu-id="f64cf-177">The device status should be **Online**.</span></span>
       
        ![Stránka zařízení StorSimple](./media/storsimple-configure-and-register-device-gov-u2/HCS_DeviceOnline-gov-include.png)
       
        <span data-ttu-id="f64cf-179">Pokud je zařízení ve stavu **Offline**, počkejte několik minut, než zařízení přejde do stavu Online.</span><span class="sxs-lookup"><span data-stu-id="f64cf-179">If the device status is **Offline**, wait for a couple of minutes for the device to come online.</span></span>
       
        <span data-ttu-id="f64cf-180">Pokud je zařízení i po uplynutí několika minut stále v režimu Offline, je nutné zkontrolovat, jestli je brána firewall nastavená způsobem popsaným v článku o [požadavcích zařízení StorSimple na síťe](../articles/storsimple/storsimple-system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="f64cf-180">If the device is still offline after a few minutes, then you need to make sure that your firewall network was configured as described in [networking requirements for your StorSimple device](../articles/storsimple/storsimple-system-requirements.md).</span></span>
       
        <span data-ttu-id="f64cf-181">Zkontrolujte, jestli je port 9354 otevřený pro odchozí komunikaci, protože ho používá sběrnice služby pro komunikaci služby StorSimple Manager se zařízením.</span><span class="sxs-lookup"><span data-stu-id="f64cf-181">Verify that port 9354 is open for outbound communication as this is used by the service bus for StorSimple Manager Service-to-device communication.</span></span>
