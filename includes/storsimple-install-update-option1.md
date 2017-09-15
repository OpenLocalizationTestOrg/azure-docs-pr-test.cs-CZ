<!--author=SharS last changed: 03/17/2016-->

#### <a name="to-download-hotfixes"></a><span data-ttu-id="b8c93-101">Stažení oprav hotfix</span><span class="sxs-lookup"><span data-stu-id="b8c93-101">To download hotfixes</span></span>
<span data-ttu-id="b8c93-102">Proveďte následující kroky stáhnout aktualizace softwaru.</span><span class="sxs-lookup"><span data-stu-id="b8c93-102">Perform the following steps to download the software update.</span></span>

1. <span data-ttu-id="b8c93-103">Spusťte Internet Explorer a přejděte na [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b8c93-103">Start Internet Explorer and navigate to [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="b8c93-104">Pokud na tomto počítači používáte Katalog služby Microsoft Update poprvé, po zobrazení výzvy k instalaci doplňku Katalog služby Microsoft Update klikněte na **Nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="b8c93-104">If this is your first time using the Microsoft Update Catalog on this computer, click **Install** when prompted to install the Microsoft Update Catalog add-on.</span></span>
    <span data-ttu-id="b8c93-105">![Nainstalujte katalogu](./media/storsimple-install-update-option-1/HCS_InstallCatalog-include.png)</span><span class="sxs-lookup"><span data-stu-id="b8c93-105">![Install catalog](./media/storsimple-install-update-option-1/HCS_InstallCatalog-include.png)</span></span>
3. <span data-ttu-id="b8c93-106">Do vyhledávacího pole katalogu služby Microsoft Update, zadejte číslo znalostní báze Knowledge Base (KB) opravy hotfix, které chcete stáhnout, například **3063418**a potom klikněte na **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="b8c93-106">In the search box of the Microsoft Update Catalog, enter the Knowledge Base (KB) number of the hotfix you want to download, for example **3063418**, and then click **Search**.</span></span>
4. <span data-ttu-id="b8c93-107">Zobrazí se **StorSimple aktualizace 1.2 zařízení aktualizace** sady.</span><span class="sxs-lookup"><span data-stu-id="b8c93-107">You will see the **StorSimple Update 1.2 Appliance Update** bundle.</span></span> <span data-ttu-id="b8c93-108">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="b8c93-108">Click **Add**.</span></span> <span data-ttu-id="b8c93-109">Aktualizace bude přidána do košíku.</span><span class="sxs-lookup"><span data-stu-id="b8c93-109">The update will be added to the basket.</span></span>
5. <span data-ttu-id="b8c93-110">Vyhledejte všechny další opravy hotfix uvedené v předchozí tabulce (**3043005** a **3063416**) a přidejte všechny košíku.</span><span class="sxs-lookup"><span data-stu-id="b8c93-110">Search for any additional hotfixes listed in the table above (**3043005** and **3063416**), and add each the basket.</span></span>
6. <span data-ttu-id="b8c93-111">Klikněte na **Zobrazit košík**.</span><span class="sxs-lookup"><span data-stu-id="b8c93-111">Click **View Basket**.</span></span>
   
    ![Zobrazit košík](./media/storsimple-install-update-option-1/HCS_InstallBasket-include.png)
7. <span data-ttu-id="b8c93-113">Klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="b8c93-113">Click **Download**.</span></span> <span data-ttu-id="b8c93-114">Zadejte místní umístění, do kterého chcete aktualizace stáhnout, nebo do něj přejděte pomocí tlačítka **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="b8c93-114">Specify or **Browse** to a local location where you want the downloads to appear.</span></span> <span data-ttu-id="b8c93-115">Aktualizace se stáhnou do zadaného umístění do podsložky se stejným názvem, jako má aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b8c93-115">The updates are downloaded to the specified location and placed in a subfolder with the same name as the update.</span></span> <span data-ttu-id="b8c93-116">Složku je také možné zkopírovat do sdílené síťové složky dostupné ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="b8c93-116">The folder can also be copied to a network share that is reachable from the device.</span></span>

> [!NOTE]
> <span data-ttu-id="b8c93-117">Opravy hotfix musí být dostupný z obou řadičích ke zjištění potenciálních chybové zprávy z druhé strany řadiče.</span><span class="sxs-lookup"><span data-stu-id="b8c93-117">The hotfixes must be accessible from both controllers to detect any potential error messages from the peer controller.</span></span>
> 
> 

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a><span data-ttu-id="b8c93-118">Instalace a ověření oprav hotfix běžného režimu</span><span class="sxs-lookup"><span data-stu-id="b8c93-118">To install and verify regular mode hotfixes</span></span>
<span data-ttu-id="b8c93-119">Proveďte následující kroky k instalaci a ověření opravy hotfix regular režimu.</span><span class="sxs-lookup"><span data-stu-id="b8c93-119">Perform the following steps to install and verify the regular-mode hotfixes.</span></span> <span data-ttu-id="b8c93-120">Pokud jste již nainstalovali pomocí portálu Azure, přeskočit na [instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes).</span><span class="sxs-lookup"><span data-stu-id="b8c93-120">If you already installed them using the Azure Portal, skip ahead to [install and verify maintenance mode hotfixes](#to-install-and-verify-maintenance-mode-hotfixes).</span></span>

1. <span data-ttu-id="b8c93-121">K instalaci aktualizace softwaru, přístup k rozhraní Windows PowerShell na konzole sériového portu zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="b8c93-121">To install the software update, access the Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="b8c93-122">Postupujte podle podrobných pokynů v článku [Připojení ke konzole sériového portu pomocí klienta PuTTy](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="b8c93-122">Follow the detailed instructions in [Use PuTTy to connect to the serial console](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span> <span data-ttu-id="b8c93-123">Na příkazovém řádku stiskněte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="b8c93-123">At the command prompt, press **Enter**.</span></span>
2. <span data-ttu-id="b8c93-124">Vyberte **Možnost 1** a přihlaste se k zařízení s úplným přístupem.</span><span class="sxs-lookup"><span data-stu-id="b8c93-124">Select **Option 1** to log on to the device with full access.</span></span>
3. <span data-ttu-id="b8c93-125">Chcete-li instalaci balíčku aktualizace, na příkazovém řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="b8c93-125">To install the update package, at the command prompt, type:</span></span>
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="b8c93-126">V cestě ke sdílené složce v předchozím příkazu používejte IP adresu místo DNS.</span><span class="sxs-lookup"><span data-stu-id="b8c93-126">Use IP rather than DNS in share path in the above command.</span></span> <span data-ttu-id="b8c93-127">Parametr Credential se používá pouze pro přístup ke sdílené složce s nutností ověření.</span><span class="sxs-lookup"><span data-stu-id="b8c93-127">The credential parameter is used only if you are accessing an authenticated share.</span></span>
   
    <span data-ttu-id="b8c93-128">Parametr Credential doporučujeme používat pro přístup ke sdíleným složkám.</span><span class="sxs-lookup"><span data-stu-id="b8c93-128">We recommend that you use the credential parameter to access shares.</span></span> <span data-ttu-id="b8c93-129">I sdílené složky otevřené všem uživatelům obvykle nejsou otevřené pro neověřené uživatele.</span><span class="sxs-lookup"><span data-stu-id="b8c93-129">Even shares that are open to “everyone” are typically not open to unauthenticated users.</span></span>
   
    <span data-ttu-id="b8c93-130">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="b8c93-130">A sample output is shown below.</span></span>
   
    ```
    Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
    \hcsmdssoftwareupdate.exe -Credential contoso\John

    Confirm

    This operation starts the hotfix installation and could reboot one or
    both of the controllers. If the device is serving I/Os, these will not
    be disrupted. Are you sure you want to continue?
    [Y] Yes [N] No [?] Help (default is "Y"): Y
    ```

4. <span data-ttu-id="b8c93-131">Po zobrazení výzvy k potvrzení instalace opravy hotfix zadejte **Y**.</span><span class="sxs-lookup"><span data-stu-id="b8c93-131">Type **Y** when prompted to confirm the hotfix installation.</span></span>
5. <span data-ttu-id="b8c93-132">Průběh aktualizace můžete sledovat pomocí rutiny `Get-HcsUpdateStatus`.</span><span class="sxs-lookup"><span data-stu-id="b8c93-132">Monitor the update by using the `Get-HcsUpdateStatus` cmdlet.</span></span>
   
    <span data-ttu-id="b8c93-133">Následující ukázkový výstup ukazuje probíhající aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="b8c93-133">The following sample output shows the update in progress.</span></span> <span data-ttu-id="b8c93-134">Když aktualizace probíhá, hodnota `RunInprogress` bude `True`.</span><span class="sxs-lookup"><span data-stu-id="b8c93-134">The `RunInprogress` will be `True` when the update is in progress.</span></span>
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp : 9/02/2015 10:36:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
     <span data-ttu-id="b8c93-135">Následující ukázkový výstup ukazuje dokončení aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b8c93-135">The following sample output indicates that the update is finished.</span></span> <span data-ttu-id="b8c93-136">Když se aktualizace dokončí, hodnota `RunInProgress` bude `False`.</span><span class="sxs-lookup"><span data-stu-id="b8c93-136">The `RunInProgress` will be `False` when the update has completed.</span></span>

    ```
    Controller1>Get-HcsUpdateStatus

    RunInprogress       : False
    LastHotfixTimestamp : 9/02/2015 10:56:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
   > [!NOTE]
   > <span data-ttu-id="b8c93-137">Rutina občas hlásí `False`, i když aktualizace stále probíhá.</span><span class="sxs-lookup"><span data-stu-id="b8c93-137">Occasionally, the cmdlet reports `False` when the update is still in progress.</span></span> <span data-ttu-id="b8c93-138">Pokud chcete zkontrolovat, že se oprava hotfix dokončila, počkejte několik minut, znovu spusťte tento příkaz a ověřte, že hodnota `RunInProgress` je `False`.</span><span class="sxs-lookup"><span data-stu-id="b8c93-138">To ensure that the hotfix is complete, wait for a few minutes, rerun this command and verify that the `RunInProgress` is `False`.</span></span> <span data-ttu-id="b8c93-139">Pokud ano, oprava hotfix byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="b8c93-139">If it is, then the hotfix has completed.</span></span>
   > 
   > 
6. <span data-ttu-id="b8c93-140">Po dokončení aktualizace softwaru zkontrolujte verze systémového softwaru.</span><span class="sxs-lookup"><span data-stu-id="b8c93-140">After the software update is complete, verify the system software versions.</span></span> <span data-ttu-id="b8c93-141">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b8c93-141">Type the following command:</span></span>
   
    `Get-HcsSystem`
   
    <span data-ttu-id="b8c93-142">Měly by se zobrazit následující verze:</span><span class="sxs-lookup"><span data-stu-id="b8c93-142">You should see the following versions:</span></span>
   
   * <span data-ttu-id="b8c93-143">HcsSoftwareVersion: 6.3.9600.17584</span><span class="sxs-lookup"><span data-stu-id="b8c93-143">HcsSoftwareVersion: 6.3.9600.17584</span></span>
   * <span data-ttu-id="b8c93-144">CisAgentVersion: 1.0.9049.0</span><span class="sxs-lookup"><span data-stu-id="b8c93-144">CisAgentVersion: 1.0.9049.0</span></span>
   * <span data-ttu-id="b8c93-145">MdsAgentVersion: 26.0.4696.1433</span><span class="sxs-lookup"><span data-stu-id="b8c93-145">MdsAgentVersion: 26.0.4696.1433</span></span>
     
     <span data-ttu-id="b8c93-146">Pokud číslo verze se nezmění po instalaci aktualizace, znamená to, že se nepodařilo použít opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="b8c93-146">If the version numbers do not change after applying the update, it indicates that the hotfix has failed to apply.</span></span> <span data-ttu-id="b8c93-147">Pokud je to váš případ a potřebujete další pomoc, kontaktujte [podporu Microsoftu](../articles/storsimple/storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="b8c93-147">Should you see this, please contact [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) for further assistance.</span></span>
7. <span data-ttu-id="b8c93-148">Zopakujte kroky 3 až 5 pro instalaci oprav hotfix zbývající regular režimu (KB3043005).</span><span class="sxs-lookup"><span data-stu-id="b8c93-148">Repeat steps 3-5 to install the remaining regular-mode hotfix (KB3043005).</span></span>

#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a><span data-ttu-id="b8c93-149">Instalace a ověření oprav hotfix režimu údržby</span><span class="sxs-lookup"><span data-stu-id="b8c93-149">To install and verify maintenance mode hotfixes</span></span>
<span data-ttu-id="b8c93-150">Použití KB3063416 k instalaci aktualizace firmwaru disku.</span><span class="sxs-lookup"><span data-stu-id="b8c93-150">Use KB3063416 to install disk firmware updates.</span></span> <span data-ttu-id="b8c93-151">Tyto jsou rušivý aktualizace a trvat zhruba 30 – 45 minut.</span><span class="sxs-lookup"><span data-stu-id="b8c93-151">These are disruptive updates and take around 30-45 minutes to complete.</span></span> <span data-ttu-id="b8c93-152">Můžete se rozhodnout je nainstalovat během naplánovaného časového období údržby pomocí připojení ke konzole sériového portu zařízení.</span><span class="sxs-lookup"><span data-stu-id="b8c93-152">You can choose to install these in a planned maintenance window by connecting to the device serial console.</span></span>

<span data-ttu-id="b8c93-153">Pokud chcete nainstalovat aktualizace firmwaru disku, postupujte podle následujících pokynů.</span><span class="sxs-lookup"><span data-stu-id="b8c93-153">To install the disk firmware updates, follow the instructions below.</span></span>

1. <span data-ttu-id="b8c93-154">Umístíte zařízení do režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="b8c93-154">Place the device in Maintenance mode.</span></span> <span data-ttu-id="b8c93-155">Všimněte si, že byste neměli používat vzdálenou komunikaci prostředí Windows PowerShell, při připojení k zařízení v režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="b8c93-155">Note that you should not use Windows PowerShell remoting when connecting to a device in Maintenance mode.</span></span> <span data-ttu-id="b8c93-156">Musíte se tato rutina spustit na řadiči zařízení při připojení prostřednictvím konzole sériového portu zařízení.</span><span class="sxs-lookup"><span data-stu-id="b8c93-156">You will need to run this cmdlet on the device controller when connected through the device serial console.</span></span> <span data-ttu-id="b8c93-157">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="b8c93-157">Type:</span></span>
   
    `Enter-HcsMaintenanceMode`
   
    <span data-ttu-id="b8c93-158">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="b8c93-158">A sample output is shown below.</span></span>
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update1-8100-SHG0997879L76YD
        Software Version: 6.3.9600.17584
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    <span data-ttu-id="b8c93-159">Oběma řadičům potom restartujte do režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="b8c93-159">Both the controllers then restart into Maintenance mode.</span></span>
2. <span data-ttu-id="b8c93-160">Pokud chcete nainstalovat aktualizaci firmwaru disku, zadejte:</span><span class="sxs-lookup"><span data-stu-id="b8c93-160">To install the disk firmware update, type:</span></span>
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="b8c93-161">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="b8c93-161">A sample output is shown below.</span></span>
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. <span data-ttu-id="b8c93-162">Průběh instalace můžete sledovat pomocí příkazu `Get-HcsUpdateStatus`.</span><span class="sxs-lookup"><span data-stu-id="b8c93-162">Monitor the install progress using `Get-HcsUpdateStatus` command.</span></span> <span data-ttu-id="b8c93-163">Když se `RunInProgress` změní na `False`, aktualizace je dokončena.</span><span class="sxs-lookup"><span data-stu-id="b8c93-163">The update is complete when the `RunInProgress` changes to `False`.</span></span>
4. <span data-ttu-id="b8c93-164">Po dokončení instalace bude nutné restartovat řadiče, na kterém byla nainstalována oprava hotfix režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="b8c93-164">After the installation is complete, the controller on which the maintenance mode hotfix was installed will be rebooted.</span></span> <span data-ttu-id="b8c93-165">Přihlaste se jako Možnost 1 s úplným přístup a zkontrolujte verzi firmwaru disku.</span><span class="sxs-lookup"><span data-stu-id="b8c93-165">Log in as option 1 with full access and verify the disk firmware version.</span></span> <span data-ttu-id="b8c93-166">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="b8c93-166">Type:</span></span>
   
   `Get-HcsFirmwareVersion`
   
   <span data-ttu-id="b8c93-167">Očekávané verze firmwaru disku jsou:</span><span class="sxs-lookup"><span data-stu-id="b8c93-167">The expected disk firmware versions are:</span></span>
   
   `XMGG, XGEE, KZ50, F6C2, VR08`
   
   <span data-ttu-id="b8c93-168">Na druhém kontroleru spusťte příkaz `Get-HcsFirmwareVersion` a ověřte, že došlo k aktualizaci verze softwaru.</span><span class="sxs-lookup"><span data-stu-id="b8c93-168">Run the `Get-HcsFirmwareVersion` command on the second controller to verify that the software version has been updated.</span></span> <span data-ttu-id="b8c93-169">Potom můžete ukončit režim údržby.</span><span class="sxs-lookup"><span data-stu-id="b8c93-169">You can then exit the maintenance mode.</span></span> <span data-ttu-id="b8c93-170">Zadejte následující příkaz pro každý řadič zařízení:</span><span class="sxs-lookup"><span data-stu-id="b8c93-170">Type the following command for each device controller:</span></span>
   
   `Exit-HcsMaintenanceMode`
5. <span data-ttu-id="b8c93-171">V řadičích restartovat po ukončení režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="b8c93-171">The controllers restart when you exit Maintenance mode.</span></span> <span data-ttu-id="b8c93-172">Po úspěšné instalaci aktualizací firmwaru disku a ukončení režimu údržby na zařízení se vraťte na portál Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="b8c93-172">After the disk firmware updates are successfully applied and the device has exited maintenance mode, return to the Azure classic portal.</span></span> <span data-ttu-id="b8c93-173">Všimněte si, že nemusí zobrazit na portálu, že jste nainstalovali aktualizace režimu údržby po dobu 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="b8c93-173">Note that the portal might not show that you installed the Maintenance mode updates for 24 hours.</span></span>

