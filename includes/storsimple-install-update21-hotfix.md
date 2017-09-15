<!--author=alkohli last changed: 05/19/16-->

#### <a name="to-download-hotfixes"></a><span data-ttu-id="01491-101">Stažení oprav hotfix</span><span class="sxs-lookup"><span data-stu-id="01491-101">To download hotfixes</span></span>
<span data-ttu-id="01491-102">Provedením následujících kroků si stáhněte aktualizace softwaru z Katalogu služby Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="01491-102">Perform the following steps to download the software update from the Microsoft Update Catalog.</span></span>

1. <span data-ttu-id="01491-103">Spusťte Internet Explorer a přejděte na [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="01491-103">Start Internet Explorer and navigate to [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="01491-104">Pokud na tomto počítači používáte Katalog služby Microsoft Update poprvé, po zobrazení výzvy k instalaci doplňku Katalog služby Microsoft Update klikněte na **Nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="01491-104">If this is your first time using the Microsoft Update Catalog on this computer, click **Install** when prompted to install the Microsoft Update Catalog add-on.</span></span>
    <span data-ttu-id="01491-105">![Nainstalujte katalogu](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)</span><span class="sxs-lookup"><span data-stu-id="01491-105">![Install catalog](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)</span></span>
3. <span data-ttu-id="01491-106">Do vyhledávacího pole katalogu služby Microsoft Update, zadejte číslo znalostní báze Knowledge Base (KB) opravy hotfix, které chcete stáhnout, například **3179904**a potom klikněte na **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="01491-106">In the search box of the Microsoft Update Catalog, enter the Knowledge Base (KB) number of the hotfix you want to download, for example **3179904**, and then click **Search**.</span></span>
   
    <span data-ttu-id="01491-107">Zobrazí se seznam oprav hotfix, například **kumulativní 2.2 aktualizace sady softwaru pro zařízení StorSimple řady 8000**.</span><span class="sxs-lookup"><span data-stu-id="01491-107">The hotfix listing appears, for example, **Cumulative Software Bundle Update 2.2 for StorSimple 8000 Series**.</span></span>
   
    ![Prohledávání katalogu](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)
4. <span data-ttu-id="01491-109">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="01491-109">Click **Add**.</span></span> <span data-ttu-id="01491-110">Aktualizace se přidá do košíku.</span><span class="sxs-lookup"><span data-stu-id="01491-110">The update is added to the basket.</span></span>
5. <span data-ttu-id="01491-111">Vyhledejte všechny další opravy hotfix uvedené v předchozí tabulce (**3103616**, **3146621**) a přidejte všechny do košíku.</span><span class="sxs-lookup"><span data-stu-id="01491-111">Search for any additional hotfixes listed in the table above (**3103616**, **3146621**), and add each to the basket.</span></span>
6. <span data-ttu-id="01491-112">Klikněte na **Zobrazit košík**.</span><span class="sxs-lookup"><span data-stu-id="01491-112">Click **View Basket**.</span></span>
7. <span data-ttu-id="01491-113">Klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="01491-113">Click **Download**.</span></span> <span data-ttu-id="01491-114">Zadejte místní umístění, do kterého chcete aktualizace stáhnout, nebo do něj přejděte pomocí tlačítka **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="01491-114">Specify or **Browse** to a local location where you want the downloads to appear.</span></span> <span data-ttu-id="01491-115">Aktualizace se stáhnou do zadaného umístění a umístit do podsložky se stejným názvem jako aktualizace.</span><span class="sxs-lookup"><span data-stu-id="01491-115">The updates are downloaded to the specified location and placed in a sub-folder with the same name as the update.</span></span> <span data-ttu-id="01491-116">Složku je také možné zkopírovat do sdílené síťové složky dostupné ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="01491-116">The folder can also be copied to a network share that is reachable from the device.</span></span>

> [!NOTE]
> <span data-ttu-id="01491-117">Opravy hotfix musí být dostupný z obou řadičích ke zjištění potenciálních chybové zprávy z druhé strany řadiče.</span><span class="sxs-lookup"><span data-stu-id="01491-117">The hotfixes must be accessible from both controllers to detect any potential error messages from the peer controller.</span></span>
> 
> 

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a><span data-ttu-id="01491-118">Instalace a ověření oprav hotfix běžného režimu</span><span class="sxs-lookup"><span data-stu-id="01491-118">To install and verify regular mode hotfixes</span></span>
<span data-ttu-id="01491-119">Provedením následujících kroků nainstalujte a ověřte opravy hotfix běžného režimu.</span><span class="sxs-lookup"><span data-stu-id="01491-119">Perform the following steps to install and verify regular-mode hotfixes.</span></span> <span data-ttu-id="01491-120">Pokud jste již nainstalovali pomocí portálu Azure, přeskočit na [instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes).</span><span class="sxs-lookup"><span data-stu-id="01491-120">If you already installed them using the Azure Portal, skip ahead to [install and verify maintenance mode hotfixes](#to-install-and-verify-maintenance-mode-hotfixes).</span></span>

1. <span data-ttu-id="01491-121">Pokud chcete nainstalovat opravy hotfix, v konzole sériového portu zařízení StorSimple spusťte rozhraní Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="01491-121">To install the hotfixes, access the Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="01491-122">Postupujte podle podrobných pokynů v článku [Připojení ke konzole sériového portu pomocí klienta PuTTy](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="01491-122">Follow the detailed instructions in [Use PuTTy to connect to the serial console](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span> <span data-ttu-id="01491-123">Na příkazovém řádku stiskněte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="01491-123">At the command prompt, press **Enter**.</span></span>
2. <span data-ttu-id="01491-124">Vyberte **Možnost 1** a přihlaste se k zařízení s úplným přístupem.</span><span class="sxs-lookup"><span data-stu-id="01491-124">Select **Option 1** to log on to the device with full access.</span></span> <span data-ttu-id="01491-125">Doporučujeme opravu hotfix nejprve nainstalovat na pasivním kontroleru.</span><span class="sxs-lookup"><span data-stu-id="01491-125">We recommend that you install the hotfix on the passive controller first.</span></span>
3. <span data-ttu-id="01491-126">Pokud chcete nainstalovat opravu hotfix, na příkazovém řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="01491-126">To install the hotfix, at the command prompt, type:</span></span>
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="01491-127">V cestě ke sdílené složce v předchozím příkazu používejte IP adresu místo DNS.</span><span class="sxs-lookup"><span data-stu-id="01491-127">Use IP rather than DNS in share path in the above command.</span></span> <span data-ttu-id="01491-128">Parametr Credential se používá pouze pro přístup ke sdílené složce s nutností ověření.</span><span class="sxs-lookup"><span data-stu-id="01491-128">The credential parameter is used only if you are accessing an authenticated share.</span></span>
   
    <span data-ttu-id="01491-129">Parametr Credential doporučujeme používat pro přístup ke sdíleným složkám.</span><span class="sxs-lookup"><span data-stu-id="01491-129">We recommend that you use the credential parameter to access shares.</span></span> <span data-ttu-id="01491-130">I sdílené složky otevřené všem uživatelům obvykle nejsou otevřené pro neověřené uživatele.</span><span class="sxs-lookup"><span data-stu-id="01491-130">Even shares that are open to “everyone” are typically not open to unauthenticated users.</span></span>
   
    <span data-ttu-id="01491-131">Po zobrazení výzvy zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="01491-131">Supply the password when prompted.</span></span>
   
    <span data-ttu-id="01491-132">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="01491-132">A sample output is shown below.</span></span>
   
    ```
    Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
    \hcsmdssoftwareupdate.exe -Credential contoso\John

    Confirm

    This operation starts the hotfix installation and could reboot one or
    both of the controllers. If the device is serving I/Os, these will not
    be disrupted. Are you sure you want to continue?
    [Y] Yes [N] No [?] Help (default is "Y"): Y
    ```

4. <span data-ttu-id="01491-133">Po zobrazení výzvy k potvrzení instalace opravy hotfix zadejte **Y**.</span><span class="sxs-lookup"><span data-stu-id="01491-133">Type **Y** when prompted to confirm the hotfix installation.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="01491-134">Pokud instalace aktualizace 2.2, instalovat pouze binární soubor, kterými 'všechna hcsmdssoftwareudpate'.</span><span class="sxs-lookup"><span data-stu-id="01491-134">If installing Update 2.2, only install the binary file prefaced with 'all-hcsmdssoftwareudpate'.</span></span> <span data-ttu-id="01491-135">Neinstalujte Cis a aktualizace agenta MDS uvedena všechna cismdsagentupdatebundle.</span><span class="sxs-lookup"><span data-stu-id="01491-135">Do not install the Cis and the MDS agent update prefaced with all-cismdsagentupdatebundle.</span></span> <span data-ttu-id="01491-136">Uděláte to tak bude dojít k chybě.</span><span class="sxs-lookup"><span data-stu-id="01491-136">Failure to do so will result in an error.</span></span> 

5. <span data-ttu-id="01491-137">Průběh aktualizace můžete sledovat pomocí rutiny `Get-HcsUpdateStatus`.</span><span class="sxs-lookup"><span data-stu-id="01491-137">Monitor the update by using the `Get-HcsUpdateStatus` cmdlet.</span></span> <span data-ttu-id="01491-138">Aktualizace se nejdříve dokončí na pasivním kontroleru.</span><span class="sxs-lookup"><span data-stu-id="01491-138">The update will first complete on the passive controller.</span></span> <span data-ttu-id="01491-139">Jakmile je pasivní kontroler aktualizovaný, proběhne převzetí služeb při selhání a potom se aktualizace nainstaluje i na druhý kontroler.</span><span class="sxs-lookup"><span data-stu-id="01491-139">Once the passive controller is updated, there will be a failover and the update will then get applied on the other controller.</span></span> <span data-ttu-id="01491-140">Aktualizace je dokončena, když jsou aktualizované oba kontrolery.</span><span class="sxs-lookup"><span data-stu-id="01491-140">The update is complete when both the controllers are updated.</span></span>
   
    <span data-ttu-id="01491-141">Následující ukázkový výstup ukazuje probíhající aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="01491-141">The following sample output shows the update in progress.</span></span> <span data-ttu-id="01491-142">Když aktualizace probíhá, hodnota `RunInprogress` bude `True`.</span><span class="sxs-lookup"><span data-stu-id="01491-142">The `RunInprogress` will be `True` when the update is in progress.</span></span>
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 5/5/2016 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     <span data-ttu-id="01491-143">Následující ukázkový výstup ukazuje dokončení aktualizace.</span><span class="sxs-lookup"><span data-stu-id="01491-143">The following sample output indicates that the update is finished.</span></span> <span data-ttu-id="01491-144">Když se aktualizace dokončí, hodnota `RunInProgress` bude `False`.</span><span class="sxs-lookup"><span data-stu-id="01491-144">The `RunInProgress` will be `False` when the update has completed.</span></span>
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 5/17/2016 9:15:55 AM
    LastUpdateTimestamp : 5/17/2016 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE]
    > <span data-ttu-id="01491-145">Rutina občas hlásí `False`, i když aktualizace stále probíhá.</span><span class="sxs-lookup"><span data-stu-id="01491-145">Occasionally, the cmdlet reports `False` when the update is still in progress.</span></span> <span data-ttu-id="01491-146">Pokud chcete zkontrolovat, že se oprava hotfix dokončila, počkejte několik minut, znovu spusťte tento příkaz a ověřte, že hodnota `RunInProgress` je `False`.</span><span class="sxs-lookup"><span data-stu-id="01491-146">To ensure that the hotfix is complete, wait for a few minutes, rerun this command and verify that the `RunInProgress` is `False`.</span></span> <span data-ttu-id="01491-147">Pokud ano, oprava hotfix byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="01491-147">If it is, then the hotfix has completed.</span></span>

1. <span data-ttu-id="01491-148">Po dokončení aktualizace softwaru zkontrolujte verze systémového softwaru.</span><span class="sxs-lookup"><span data-stu-id="01491-148">After the software update is complete, verify the system software versions.</span></span> <span data-ttu-id="01491-149">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="01491-149">Type:</span></span>
   
    `Get-HcsSystem`
   
    <span data-ttu-id="01491-150">Měly by se zobrazit následující verze:</span><span class="sxs-lookup"><span data-stu-id="01491-150">You should see the following versions:</span></span>
   
   * `HcsSoftwareVersion: 6.3.9600.17708`
   * `CisAgentVersion: 1.0.9299.0`
   * `MdsAgentVersion: 30.0.4698.16` 
     
     <span data-ttu-id="01491-151">Pokud číslo verze se nezmění po instalaci aktualizace, znamená to, že se nepodařilo použít opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="01491-151">If the version numbers do not change after applying the update, it indicates that the hotfix has failed to apply.</span></span> <span data-ttu-id="01491-152">Pokud je to váš případ a potřebujete další pomoc, kontaktujte [podporu Microsoftu](../articles/storsimple/storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="01491-152">Should you see this, please contact [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) for further assistance.</span></span>
     
     > [!IMPORTANT]
     > <span data-ttu-id="01491-153">Před instalací zbývajících aktualizací je nutné restartovat aktivní kontroler pomocí rutiny `Restart-HcsController`.</span><span class="sxs-lookup"><span data-stu-id="01491-153">You must restart the active controller via the `Restart-HcsController` cmdlet before applying the remaining updates.</span></span> 
     > 
     > 
2. <span data-ttu-id="01491-154">Opakujte kroky 3 až 5 pro instalaci zbývající oprav hotfix regular režimu.</span><span class="sxs-lookup"><span data-stu-id="01491-154">Repeat steps 3-5 to install the remaining regular-mode hotfixes.</span></span>
   
   * <span data-ttu-id="01491-155">Aktualizace iSCSI KB3146621</span><span class="sxs-lookup"><span data-stu-id="01491-155">The iSCSI update KB3146621</span></span>
   * <span data-ttu-id="01491-156">Aktualizace služby WMI KB3103616</span><span class="sxs-lookup"><span data-stu-id="01491-156">The WMI update KB3103616</span></span>
3. <span data-ttu-id="01491-157">Tento krok přeskočte, pokud jsou aktualizace z Update 2.</span><span class="sxs-lookup"><span data-stu-id="01491-157">Skip this step if you are updating from Update 2.</span></span> <span data-ttu-id="01491-158">Při aktualizaci z verze před Update 2, bude také muset stáhnout:</span><span class="sxs-lookup"><span data-stu-id="01491-158">If you are updating from a version prior to Update 2, you will also need to download:</span></span>

    - <span data-ttu-id="01491-159">Ovladač LSI KB3121900</span><span class="sxs-lookup"><span data-stu-id="01491-159">The LSI driver KB3121900</span></span>

    - <span data-ttu-id="01491-160">Aktualizace Spaceport KB3090322</span><span class="sxs-lookup"><span data-stu-id="01491-160">The Spaceport update KB3090322</span></span>

    - <span data-ttu-id="01491-161">Aktualizace Storport KB3080728</span><span class="sxs-lookup"><span data-stu-id="01491-161">The Storport update KB3080728</span></span>

#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a><span data-ttu-id="01491-162">Instalace a ověření oprav hotfix režimu údržby</span><span class="sxs-lookup"><span data-stu-id="01491-162">To install and verify maintenance mode hotfixes</span></span>
<span data-ttu-id="01491-163">Použití KB3121899 k instalaci aktualizace firmwaru disku.</span><span class="sxs-lookup"><span data-stu-id="01491-163">Use KB3121899 to install disk firmware updates.</span></span> <span data-ttu-id="01491-164">Jedná se o narušující aktualizace a jejich dokončení trvá přibližně 30 minut.</span><span class="sxs-lookup"><span data-stu-id="01491-164">These are disruptive updates and take around 30 minutes to complete.</span></span> <span data-ttu-id="01491-165">Můžete se rozhodnout je nainstalovat během naplánovaného časového období údržby pomocí připojení ke konzole sériového portu zařízení.</span><span class="sxs-lookup"><span data-stu-id="01491-165">You can choose to install these in a planned maintenance window by connecting to the device serial console.</span></span>

<span data-ttu-id="01491-166">Poznámka: Pokud je váš firmware disku aktuální, není třeba instalovat tyto aktualizace.</span><span class="sxs-lookup"><span data-stu-id="01491-166">Note that if your disk firmware is already up-to-date, you won't need to install these updates.</span></span> <span data-ttu-id="01491-167">Z konzoly sériového portu zařízení spusťte rutinu `Get-HcsUpdateAvailability`, která zkontroluje dostupnost aktualizací a jestli se jedná o narušující (režim údržby) nebo nenarušující (běžný režim) aktualizace.</span><span class="sxs-lookup"><span data-stu-id="01491-167">Run the `Get-HcsUpdateAvailability` cmdlet from the device serial console to check if updates are available and whether the updates are disruptive (maintenance mode) or non-disruptive (regular mode) updates.</span></span>

<span data-ttu-id="01491-168">Pokud chcete nainstalovat aktualizace firmwaru disku, postupujte podle následujících pokynů.</span><span class="sxs-lookup"><span data-stu-id="01491-168">To install the disk firmware updates, follow the instructions below.</span></span>

1. <span data-ttu-id="01491-169">Umístíte zařízení do režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="01491-169">Place the device in the Maintenance mode.</span></span> <span data-ttu-id="01491-170">Všimněte si, že byste neměli používat vzdálenou komunikaci prostředí Windows PowerShell, při připojení k zařízení v režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="01491-170">Note that you should not use Windows PowerShell remoting when connecting to a device in Maintenance mode.</span></span> <span data-ttu-id="01491-171">Místo toho spusťte tuto rutinu v řadiče zařízení při připojení prostřednictvím konzole sériového portu zařízení.</span><span class="sxs-lookup"><span data-stu-id="01491-171">Instead run this cmdlet on the device controller when connected through the device serial console.</span></span> <span data-ttu-id="01491-172">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="01491-172">Type:</span></span>
   
    `Enter-HcsMaintenanceMode`
   
    <span data-ttu-id="01491-173">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="01491-173">A sample output is shown below.</span></span>
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update2-8100-SHG0997879L76673
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    <span data-ttu-id="01491-174">Oběma řadičům potom restartujte do režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="01491-174">Both the controllers then restart into Maintenance mode.</span></span>
2. <span data-ttu-id="01491-175">Pokud chcete nainstalovat aktualizaci firmwaru disku, zadejte:</span><span class="sxs-lookup"><span data-stu-id="01491-175">To install the disk firmware update, type:</span></span>
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="01491-176">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="01491-176">A sample output is shown below.</span></span>
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. By installing new updates you agree to, and accept any additional terms associated with, the new functionality listed in the release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. <span data-ttu-id="01491-177">Průběh instalace můžete sledovat pomocí příkazu `Get-HcsUpdateStatus`.</span><span class="sxs-lookup"><span data-stu-id="01491-177">Monitor the install progress using `Get-HcsUpdateStatus` command.</span></span> <span data-ttu-id="01491-178">Když se `RunInProgress` změní na `False`, aktualizace je dokončena.</span><span class="sxs-lookup"><span data-stu-id="01491-178">The update is complete when the `RunInProgress` changes to `False`.</span></span>
4. <span data-ttu-id="01491-179">Po dokončení instalace se kontroler, na který se instalovala oprava hotfix režimu údržby, restartuje.</span><span class="sxs-lookup"><span data-stu-id="01491-179">After the installation is complete, the controller on which the maintenance mode hotfix was installed restarts.</span></span> <span data-ttu-id="01491-180">Přihlaste se jako Možnost 1 s úplným přístup a zkontrolujte verzi firmwaru disku.</span><span class="sxs-lookup"><span data-stu-id="01491-180">Log in as option 1 with full access and verify the disk firmware version.</span></span> <span data-ttu-id="01491-181">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="01491-181">Type:</span></span>
   
   `Get-HcsFirmwareVersion`
   
   <span data-ttu-id="01491-182">Očekávané verze firmwaru disku jsou:</span><span class="sxs-lookup"><span data-stu-id="01491-182">The expected disk firmware versions are:</span></span>
   
   `XMGG, XGEG, KZ50, F6C2, VR08`
   
   <span data-ttu-id="01491-183">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="01491-183">A sample output is shown below.</span></span>
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8100
       Name: Update2-8100-SHG0997879L76YD
       Software Version: 6.3.9600.17705
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected to Controller1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
         ActiveBIOS:0.45.0006
         BackupBIOS:0.45.0008
         MainCPLD:17.0.0005
         ActiveBMCRoot:2.0.000E
         BackupBMCRoot:2.0.000E
         BMCBoot:2.0.0001
         LsiFirmware:19.00.00.00
         LsiBios:07.37.00.00
         Battery1Firmware:06.29
         Battery2Firmware:06.29
         DomFirmware:X231600
         CanisterFirmware:3.5.0.32
         CanisterBootloader:5.03
         CanisterConfigCRC:0xD1B030A4
         CanisterVPDStructure:0x06
         CanisterGEMCPLD:0x17
         CanisterVPDCRC:0xEE3504B4
         MidplaneVPDStructure:0x0C
         MidplaneVPDCRC:0xA6BD4F64
         MidplaneCPLD:0x10
         PCM1Firmware:1.00|1.05
         PCM1VPDStructure:0x05
         PCM1VPDCRC:0x41BEF99C
         PCM2Firmware:1.00|1.05
         PCM2VPDStructure:0x05
         PCM2VPDCRC:0x41BEF99C
   
         DisksFirmware
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
   
    <span data-ttu-id="01491-184">Na druhém kontroleru spusťte příkaz `Get-HcsFirmwareVersion` a ověřte, že došlo k aktualizaci verze softwaru.</span><span class="sxs-lookup"><span data-stu-id="01491-184">Run the `Get-HcsFirmwareVersion` command on the second controller to verify that the software version has been updated.</span></span> <span data-ttu-id="01491-185">Potom můžete ukončit režim údržby.</span><span class="sxs-lookup"><span data-stu-id="01491-185">You can then exit the maintenance mode.</span></span> <span data-ttu-id="01491-186">Uděláte to tak, že na obou kontrolerech zařízení zadáte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="01491-186">To do so, type the following command for each device controller:</span></span>
   
   `Exit-HcsMaintenanceMode`
5. <span data-ttu-id="01491-187">V řadičích restartovat po ukončení režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="01491-187">The controllers restart when you exit Maintenance mode.</span></span> <span data-ttu-id="01491-188">Po úspěšné instalaci aktualizací firmwaru disku a ukončení režimu údržby na zařízení se vraťte na portál Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="01491-188">After the disk firmware updates are successfully applied and the device has exited maintenance mode, return to the Azure classic portal.</span></span> <span data-ttu-id="01491-189">Všimněte si, že nemusí zobrazit na portálu, že jste nainstalovali aktualizace režimu údržby po dobu 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="01491-189">Note that the portal might not show that you installed the Maintenance mode updates for 24 hours.</span></span>

