<!--author=alkohli last changed: 02/10/17-->

#### <a name="to-download-hotfixes"></a><span data-ttu-id="db53c-101">Stažení oprav hotfix</span><span class="sxs-lookup"><span data-stu-id="db53c-101">To download hotfixes</span></span>

<span data-ttu-id="db53c-102">Provedením následujících kroků si stáhněte aktualizace softwaru z Katalogu služby Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="db53c-102">Perform the following steps to download the software update from the Microsoft Update Catalog.</span></span>

1. <span data-ttu-id="db53c-103">Spusťte Internet Explorer a přejděte na [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="db53c-103">Start Internet Explorer and navigate to [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="db53c-104">Pokud na tomto počítači používáte Katalog služby Microsoft Update poprvé, po zobrazení výzvy k instalaci doplňku Katalog služby Microsoft Update klikněte na **Nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="db53c-104">If this is your first time using the Microsoft Update Catalog on this computer, click **Install** when prompted to install the Microsoft Update Catalog add-on.</span></span>

    ![Instalace katalogu](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)

3. <span data-ttu-id="db53c-106">Do vyhledávacího pole Katalogu služby Microsoft Update zadejte číslo opravy hotfix ve znalostní bázi, kterou chcete stáhnout, například **4011839**, a potom klikněte na **Vyhledat**.</span><span class="sxs-lookup"><span data-stu-id="db53c-106">In the search box of the Microsoft Update Catalog, enter the Knowledge Base (KB) number of the hotfix you want to download, for example **4011839**, and then click **Search**.</span></span>
   
    <span data-ttu-id="db53c-107">Zobrazí se výpis opravy hotfix, například **Cumulative Software Bundle Update 4.0 for StorSimple 8000 Series** (Kumulativní aktualizace balíčku softwaru 4.0 pro StorSimple 8000 Series).</span><span class="sxs-lookup"><span data-stu-id="db53c-107">The hotfix listing appears, for example, **Cumulative Software Bundle Update 4.0 for StorSimple 8000 Series**.</span></span>
   
    ![Prohledávání katalogu](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)

4. <span data-ttu-id="db53c-109">Klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="db53c-109">Click **Download**.</span></span> <span data-ttu-id="db53c-110">Zadejte místní umístění, do kterého chcete aktualizace stáhnout, nebo do něj přejděte pomocí tlačítka **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="db53c-110">Specify or **Browse** to a local location where you want the downloads to appear.</span></span> <span data-ttu-id="db53c-111">Klikněte na tlačítko soubory ke stažení do zadaného umístění a složky.</span><span class="sxs-lookup"><span data-stu-id="db53c-111">Click the files to download to the specified location and folder.</span></span> <span data-ttu-id="db53c-112">Složku je také možné zkopírovat do sdílené síťové složky dostupné ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="db53c-112">The folder can also be copied to a network share that is reachable from the device.</span></span>
5. <span data-ttu-id="db53c-113">Vyhledejte všechny další opravy hotfix uvedené v předchozí tabulce (**4011841**) a stáhněte soubory, odpovídající týkající se určitých složek, jak je uvedeno v předchozí tabulce.</span><span class="sxs-lookup"><span data-stu-id="db53c-113">Search for any additional hotfixes listed in the table above (**4011841**), and download the corresponding files to the specific folders as listed in the preceding table.</span></span>

> [!NOTE]
> <span data-ttu-id="db53c-114">Opravy hotfix musí být dostupný z obou řadičích ke zjištění potenciálních chybové zprávy z druhé strany řadiče.</span><span class="sxs-lookup"><span data-stu-id="db53c-114">The hotfixes must be accessible from both controllers to detect any potential error messages from the peer controller.</span></span>
>
> <span data-ttu-id="db53c-115">Opravy hotfix je nutné zkopírovat do 3 oddělených složek.</span><span class="sxs-lookup"><span data-stu-id="db53c-115">The hotfixes must be copied in 3 separate folders.</span></span> <span data-ttu-id="db53c-116">Například aktualizace softwaru, SNS/MDS agenta zařízení je možné zkopírovat v _FirstOrderUpdate_ složku, všechny ostatní omezovaly aktualizace může kopírovat v _SecondOrderUpdate_ složku, a aktualizace režimu údržby zkopírovali v _ThirdOrderUpdate_ složky.</span><span class="sxs-lookup"><span data-stu-id="db53c-116">For example, the device software/Cis/MDS agent update can be copied in _FirstOrderUpdate_ folder, all the other non-disruptive updates could be copied in the _SecondOrderUpdate_ folder, and maintenance mode updates copied in _ThirdOrderUpdate_ folder.</span></span>

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a><span data-ttu-id="db53c-117">Instalace a ověření oprav hotfix běžného režimu</span><span class="sxs-lookup"><span data-stu-id="db53c-117">To install and verify regular mode hotfixes</span></span>

<span data-ttu-id="db53c-118">Provedením následujících kroků nainstalujte a ověřte opravy hotfix běžného režimu.</span><span class="sxs-lookup"><span data-stu-id="db53c-118">Perform the following steps to install and verify regular-mode hotfixes.</span></span> <span data-ttu-id="db53c-119">Pokud jste je již nainstalovali pomocí portálu Azure Classic, přeskočte k [instalaci a ověření oprav hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes).</span><span class="sxs-lookup"><span data-stu-id="db53c-119">If you already installed them using the Azure classic portal, skip ahead to [install and verify maintenance mode hotfixes](#to-install-and-verify-maintenance-mode-hotfixes).</span></span>

1. <span data-ttu-id="db53c-120">Pokud chcete nainstalovat opravy hotfix, v konzole sériového portu zařízení StorSimple spusťte rozhraní Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="db53c-120">To install the hotfixes, access the Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="db53c-121">Postupujte podle podrobných pokynů v článku [Připojení ke konzole sériového portu pomocí klienta PuTTy](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="db53c-121">Follow the detailed instructions in [Use PuTTy to connect to the serial console](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span> <span data-ttu-id="db53c-122">Na příkazovém řádku stiskněte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="db53c-122">At the command prompt, press **Enter**.</span></span>
2. <span data-ttu-id="db53c-123">Vyberte **Možnost 1** a přihlaste se k zařízení s úplným přístupem.</span><span class="sxs-lookup"><span data-stu-id="db53c-123">Select **Option 1** to log on to the device with full access.</span></span> <span data-ttu-id="db53c-124">Doporučujeme opravu hotfix nejprve nainstalovat na pasivním kontroleru.</span><span class="sxs-lookup"><span data-stu-id="db53c-124">We recommend that you install the hotfix on the passive controller first.</span></span>
3. <span data-ttu-id="db53c-125">Pokud chcete nainstalovat opravu hotfix, na příkazovém řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="db53c-125">To install the hotfix, at the command prompt, type:</span></span>
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="db53c-126">V cestě ke sdílené složce v předchozím příkazu používejte IP adresu místo DNS.</span><span class="sxs-lookup"><span data-stu-id="db53c-126">Use IP rather than DNS in share path in the above command.</span></span> <span data-ttu-id="db53c-127">Parametr Credential se používá pouze pro přístup ke sdílené složce s nutností ověření.</span><span class="sxs-lookup"><span data-stu-id="db53c-127">The credential parameter is used only if you are accessing an authenticated share.</span></span>
   
    <span data-ttu-id="db53c-128">Parametr Credential doporučujeme používat pro přístup ke sdíleným složkám.</span><span class="sxs-lookup"><span data-stu-id="db53c-128">We recommend that you use the credential parameter to access shares.</span></span> <span data-ttu-id="db53c-129">I sdílené složky otevřené všem uživatelům obvykle nejsou otevřené pro neověřené uživatele.</span><span class="sxs-lookup"><span data-stu-id="db53c-129">Even shares that are open to “everyone” are typically not open to unauthenticated users.</span></span>
   
    <span data-ttu-id="db53c-130">Po zobrazení výzvy zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="db53c-130">Supply the password when prompted.</span></span>
   
    <span data-ttu-id="db53c-131">Ukázkový výstup instalace aktualizací prvního řádu najdete níž.</span><span class="sxs-lookup"><span data-stu-id="db53c-131">A sample output for installing the first order updates is shown below.</span></span> <span data-ttu-id="db53c-132">Pro první pořadí aktualizací budete muset přejděte na konkrétní soubor.</span><span class="sxs-lookup"><span data-stu-id="db53c-132">For the first order update, you need to point to the specific file.</span></span>
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \FirstOrderUpdate\HcsSoftwareUpdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts the hotfix installation and could reboot one or
        both of the controllers. If the device is serving I/Os, these will not
        be disrupted. Are you sure you want to continue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
4. <span data-ttu-id="db53c-133">Po zobrazení výzvy k potvrzení instalace opravy hotfix zadejte **Y**.</span><span class="sxs-lookup"><span data-stu-id="db53c-133">Type **Y** when prompted to confirm the hotfix installation.</span></span>
5. <span data-ttu-id="db53c-134">Průběh aktualizace můžete sledovat pomocí rutiny `Get-HcsUpdateStatus`.</span><span class="sxs-lookup"><span data-stu-id="db53c-134">Monitor the update by using the `Get-HcsUpdateStatus` cmdlet.</span></span> <span data-ttu-id="db53c-135">Aktualizace se nejdříve dokončí na pasivním kontroleru.</span><span class="sxs-lookup"><span data-stu-id="db53c-135">The update will first complete on the passive controller.</span></span> <span data-ttu-id="db53c-136">Jakmile je pasivní kontroler aktualizovaný, proběhne převzetí služeb při selhání a potom se aktualizace nainstaluje i na druhý kontroler.</span><span class="sxs-lookup"><span data-stu-id="db53c-136">Once the passive controller is updated, there will be a failover and the update will then get applied on the other controller.</span></span> <span data-ttu-id="db53c-137">Aktualizace je dokončena, když jsou aktualizované oba kontrolery.</span><span class="sxs-lookup"><span data-stu-id="db53c-137">The update is complete when both the controllers are updated.</span></span>
   
    <span data-ttu-id="db53c-138">Následující ukázkový výstup ukazuje probíhající aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="db53c-138">The following sample output shows the update in progress.</span></span> <span data-ttu-id="db53c-139">Když aktualizace probíhá, hodnota `RunInprogress` bude `True`.</span><span class="sxs-lookup"><span data-stu-id="db53c-139">The `RunInprogress` will be `True` when the update is in progress.</span></span>

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 02/03/2017 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     <span data-ttu-id="db53c-140">Následující ukázkový výstup ukazuje dokončení aktualizace.</span><span class="sxs-lookup"><span data-stu-id="db53c-140">The following sample output indicates that the update is finished.</span></span> <span data-ttu-id="db53c-141">Když se aktualizace dokončí, hodnota `RunInProgress` bude `False`.</span><span class="sxs-lookup"><span data-stu-id="db53c-141">The `RunInProgress` will be `False` when the update has completed.</span></span>
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 02/03/2017 9:15:55 AM
    LastUpdateTimestamp : 02/03/2017 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE]
    > <span data-ttu-id="db53c-142">Rutina občas hlásí `False`, i když aktualizace stále probíhá.</span><span class="sxs-lookup"><span data-stu-id="db53c-142">Occasionally, the cmdlet reports `False` when the update is still in progress.</span></span> <span data-ttu-id="db53c-143">Pokud chcete zkontrolovat, že se oprava hotfix dokončila, počkejte několik minut, znovu spusťte tento příkaz a ověřte, že hodnota `RunInProgress` je `False`.</span><span class="sxs-lookup"><span data-stu-id="db53c-143">To ensure that the hotfix is complete, wait for a few minutes, rerun this command and verify that the `RunInProgress` is `False`.</span></span> <span data-ttu-id="db53c-144">Pokud ano, oprava hotfix byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="db53c-144">If it is, then the hotfix has completed.</span></span>

6. <span data-ttu-id="db53c-145">Po dokončení aktualizace softwaru zkontrolujte verze systémového softwaru.</span><span class="sxs-lookup"><span data-stu-id="db53c-145">After the software update is complete, verify the system software versions.</span></span> <span data-ttu-id="db53c-146">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="db53c-146">Type:</span></span>
   
    `Get-HcsSystem`
   
    <span data-ttu-id="db53c-147">Měly by se zobrazit následující verze:</span><span class="sxs-lookup"><span data-stu-id="db53c-147">You should see the following versions:</span></span>
   
   * `FriendlySoftwareVersion: StorSimple 8000 Series Update 4.0`
   *  `HcsSoftwareVersion: 6.3.9600.17820`
   
    <span data-ttu-id="db53c-148">Pokud se číslo verze po instalaci aktualizace nezměnilo, znamená to, že se instalace opravy hotfix nezdařila.</span><span class="sxs-lookup"><span data-stu-id="db53c-148">If the version number does not change after applying the update, it indicates that the hotfix has failed to apply.</span></span> <span data-ttu-id="db53c-149">Pokud je to váš případ a potřebujete další pomoc, kontaktujte [podporu Microsoftu](../articles/storsimple/storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="db53c-149">Should you see this, please contact [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) for further assistance.</span></span>
     
    > [!IMPORTANT]
    > <span data-ttu-id="db53c-150">Je nutné restartovat řadič active prostřednictvím `Restart-HcsController` rutiny před použitím další aktualizace.</span><span class="sxs-lookup"><span data-stu-id="db53c-150">You must restart the active controller via the `Restart-HcsController` cmdlet before applying the next update.</span></span>
     
7. <span data-ttu-id="db53c-151">Opakujte kroky 3 až 5 pro instalaci agenta položek konfigurace nebo MDS stáhnou do vaší _FirstOrderUpdate_ složky.</span><span class="sxs-lookup"><span data-stu-id="db53c-151">Repeat steps 3-5 to install the Cis/MDS agent downloaded to your _FirstOrderUpdate_ folder.</span></span> 
8. <span data-ttu-id="db53c-152">Opakujte kroky 3–5 a nainstalujte aktualizace druhého řádu.</span><span class="sxs-lookup"><span data-stu-id="db53c-152">Repeat steps 3-5 to install the second order updates.</span></span> <span data-ttu-id="db53c-153">**Druhý pořadí aktualizací, lze nainstalovat víc aktualizací právě spuštěním `Start-HcsHotfix cmdlet` a přejdete na složku, kde jsou umístěny druhý pořadí aktualizací. Rutina spustí všechny aktualizace, které jsou k dispozici ve složce.**</span><span class="sxs-lookup"><span data-stu-id="db53c-153">**For second order updates, multiple updates can be installed by just running the `Start-HcsHotfix cmdlet` and pointing to the folder where second order updates are located. The cmdlet will execute all the updates available in the folder.**</span></span> <span data-ttu-id="db53c-154">Pokud je aktualizace již nainstalovaná, logika aktualizace to pozná a nebude ji instalovat.</span><span class="sxs-lookup"><span data-stu-id="db53c-154">If an update is already installed, the update logic will detect that and not apply that update.</span></span> 

<span data-ttu-id="db53c-155">Jakmile budou nainstalované všechny opravy hotfix, použijte rutinu `Get-HcsSystem`.</span><span class="sxs-lookup"><span data-stu-id="db53c-155">After all the hotfixes are installed, use the `Get-HcsSystem` cmdlet.</span></span> <span data-ttu-id="db53c-156">Verze by měly být:</span><span class="sxs-lookup"><span data-stu-id="db53c-156">The versions should be:</span></span>

   * `CisAgentVersion:  1.0.9441.0`
   * `MdsAgentVersion: 35.2.2.0`
   * `Lsisas2Version: 2.0.78.00`


#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a><span data-ttu-id="db53c-157">Instalace a ověření oprav hotfix režimu údržby</span><span class="sxs-lookup"><span data-stu-id="db53c-157">To install and verify maintenance mode hotfixes</span></span>
<span data-ttu-id="db53c-158">Pomocí KB4011837 nainstalujte aktualizace firmwaru disku.</span><span class="sxs-lookup"><span data-stu-id="db53c-158">Use KB4011837 to install disk firmware updates.</span></span> <span data-ttu-id="db53c-159">Jedná se o narušující aktualizace a jejich dokončení trvá přibližně 30 minut.</span><span class="sxs-lookup"><span data-stu-id="db53c-159">These are disruptive updates and take around 30 minutes to complete.</span></span> <span data-ttu-id="db53c-160">Můžete se rozhodnout je nainstalovat během naplánovaného časového období údržby pomocí připojení ke konzole sériového portu zařízení.</span><span class="sxs-lookup"><span data-stu-id="db53c-160">You can choose to install these in a planned maintenance window by connecting to the device serial console.</span></span>

<span data-ttu-id="db53c-161">Poznámka: Pokud je váš firmware disku aktuální, není třeba instalovat tyto aktualizace.</span><span class="sxs-lookup"><span data-stu-id="db53c-161">Note that if your disk firmware is already up-to-date, you won't need to install these updates.</span></span> <span data-ttu-id="db53c-162">Z konzoly sériového portu zařízení spusťte rutinu `Get-HcsUpdateAvailability`, která zkontroluje dostupnost aktualizací a jestli se jedná o narušující (režim údržby) nebo nenarušující (běžný režim) aktualizace.</span><span class="sxs-lookup"><span data-stu-id="db53c-162">Run the `Get-HcsUpdateAvailability` cmdlet from the device serial console to check if updates are available and whether the updates are disruptive (maintenance mode) or non-disruptive (regular mode) updates.</span></span>

<span data-ttu-id="db53c-163">Pokud chcete nainstalovat aktualizace firmwaru disku, postupujte podle následujících pokynů.</span><span class="sxs-lookup"><span data-stu-id="db53c-163">To install the disk firmware updates, follow the instructions below.</span></span>

1. <span data-ttu-id="db53c-164">Umístíte zařízení do režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="db53c-164">Place the device in the maintenance mode.</span></span> <span data-ttu-id="db53c-165">**Upozorňujeme, že pro připojení k zařízení v režimu údržby byste neměli používat vzdálenou komunikaci prostředí Windows PowerShell. Místo toho se připojte prostřednictvím konzoly sériového portu zařízení a spusťte v kontroleru zařízení tuto rutinu.**</span><span class="sxs-lookup"><span data-stu-id="db53c-165">**Note that you should not use Windows PowerShell remoting when connecting to a device in maintenance mode. Instead run this cmdlet on the device controller when connected through the device serial console.**</span></span> <span data-ttu-id="db53c-166">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="db53c-166">Type:</span></span>
   
    `Enter-HcsMaintenanceMode`
   
    <span data-ttu-id="db53c-167">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="db53c-167">A sample output is shown below.</span></span>
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8600
        Name: Update4-8600-mystorsimple
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    <span data-ttu-id="db53c-168">Oba kontrolery se následně restartují do režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="db53c-168">Both the controllers then restart into maintenance mode.</span></span>
2. <span data-ttu-id="db53c-169">Pokud chcete nainstalovat aktualizaci firmwaru disku, zadejte:</span><span class="sxs-lookup"><span data-stu-id="db53c-169">To install the disk firmware update, type:</span></span>
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="db53c-170">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="db53c-170">A sample output is shown below.</span></span>
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\ThirdOrderUpdates\ -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. By installing new updates you agree to, and accept any additional terms associated with, the new functionality listed in the release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. <span data-ttu-id="db53c-171">Průběh instalace můžete sledovat pomocí příkazu `Get-HcsUpdateStatus`.</span><span class="sxs-lookup"><span data-stu-id="db53c-171">Monitor the install progress using `Get-HcsUpdateStatus` command.</span></span> <span data-ttu-id="db53c-172">Když se `RunInProgress` změní na `False`, aktualizace je dokončena.</span><span class="sxs-lookup"><span data-stu-id="db53c-172">The update is complete when the `RunInProgress` changes to `False`.</span></span>
4. <span data-ttu-id="db53c-173">Po dokončení instalace se kontroler, na který se instalovala oprava hotfix režimu údržby, restartuje.</span><span class="sxs-lookup"><span data-stu-id="db53c-173">After the installation is complete, the controller on which the maintenance mode hotfix was installed restarts.</span></span> <span data-ttu-id="db53c-174">Přihlaste se jako Možnost 1 s úplným přístup a zkontrolujte verzi firmwaru disku.</span><span class="sxs-lookup"><span data-stu-id="db53c-174">Log in as option 1 with full access and verify the disk firmware version.</span></span> <span data-ttu-id="db53c-175">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="db53c-175">Type:</span></span>
   
   `Get-HcsFirmwareVersion`
   
   <span data-ttu-id="db53c-176">Očekávané verze firmwaru disku jsou:</span><span class="sxs-lookup"><span data-stu-id="db53c-176">The expected disk firmware versions are:</span></span>
   
   `XMGJ, XGEG, KZ50, F6C2, VR08, N002, 0106`
   
   <span data-ttu-id="db53c-177">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="db53c-177">A sample output is shown below.</span></span>
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8600
       Name: Update4-8600-mystorsimple
       Software Version: 6.3.9600.17820
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected to Controller1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
           ActiveBIOS:0.45.0010
              BackupBIOS:0.45.0006
              MainCPLD:17.0.000b
              ActiveBMCRoot:2.0.001F
              BackupBMCRoot:2.0.001F
              BMCBoot:2.0.0002
              LsiFirmware:20.00.04.00
              LsiBios:07.37.00.00
              Battery1Firmware:06.2C
              Battery2Firmware:06.2C
              DomFirmware:X231600
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0x9134777A
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x19
              CanisterVPDCRC:0x142F7DC2
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:1.00|1.05
              PCM1VPDStructure:0x05
              PCM1VPDCRC:0x41BEF99C
              PCM2Firmware:1.00|1.05
              PCM2VPDStructure:0x05
              PCM2VPDCRC:0x41BEF99C

           EbodFirmware
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0xB23150F8
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x14
              CanisterVPDCRC:0xBAA55828
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:3.11
              PCM1VPDStructure:0x03
              PCM1VPDCRC:0x6B58AD13
              PCM2Firmware:3.11
              PCM2VPDStructure:0x03
              PCM2VPDCRC:0x6B58AD13

           DisksFirmware
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
   
    <span data-ttu-id="db53c-178">Na druhém kontroleru spusťte příkaz `Get-HcsFirmwareVersion` a ověřte, že došlo k aktualizaci verze softwaru.</span><span class="sxs-lookup"><span data-stu-id="db53c-178">Run the `Get-HcsFirmwareVersion` command on the second controller to verify that the software version has been updated.</span></span> <span data-ttu-id="db53c-179">Potom můžete ukončit režim údržby.</span><span class="sxs-lookup"><span data-stu-id="db53c-179">You can then exit the maintenance mode.</span></span> <span data-ttu-id="db53c-180">Uděláte to tak, že na obou kontrolerech zařízení zadáte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="db53c-180">To do so, type the following command for each device controller:</span></span>
   
   `Exit-HcsMaintenanceMode`

5. <span data-ttu-id="db53c-181">Kontrolery se restartují, jakmile ukončíte režim údržby.</span><span class="sxs-lookup"><span data-stu-id="db53c-181">The controllers restart when you exit maintenance mode.</span></span> <span data-ttu-id="db53c-182">Po úspěšné instalaci aktualizací firmwaru disku a ukončení režimu údržby na zařízení se vraťte na portál Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="db53c-182">After the disk firmware updates are successfully applied and the device has exited maintenance mode, return to the Azure classic portal.</span></span> <span data-ttu-id="db53c-183">Poznámka: Informace o instalaci aktualizací režimu údržby se na portálu může zobrazit až po 24 hodinách.</span><span class="sxs-lookup"><span data-stu-id="db53c-183">Note that the portal might not show that you installed the maintenance mode updates for 24 hours.</span></span>

