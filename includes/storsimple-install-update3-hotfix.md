<!--author=alkohli last changed: 09/01/16-->

#### <a name="toodownload-hotfixes"></a><span data-ttu-id="c920c-101">toodownload opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="c920c-101">toodownload hotfixes</span></span>
<span data-ttu-id="c920c-102">Proveďte následující kroky toodownload hello softwarové aktualizace z katalogu služby Microsoft Update hello hello.</span><span class="sxs-lookup"><span data-stu-id="c920c-102">Perform hello following steps toodownload hello software update from hello Microsoft Update Catalog.</span></span>

1. <span data-ttu-id="c920c-103">Spusťte Internet Explorer a přejděte příliš[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="c920c-103">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="c920c-104">Pokud používáte hello katalogu služby Microsoft Update na tomto počítači poprvé, klikněte na tlačítko **nainstalovat** při výzvami tooinstall hello rozšíření katalogu služby Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="c920c-104">If this is your first time using hello Microsoft Update Catalog on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>
    <span data-ttu-id="c920c-105">![Nainstalujte katalogu](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)</span><span class="sxs-lookup"><span data-stu-id="c920c-105">![Install catalog](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)</span></span>
3. <span data-ttu-id="c920c-106">Do vyhledávacího pole hello hello katalogu služby Microsoft Update, zadejte číslo znalostní báze Knowledge Base (KB) hello opravy hotfix hello chcete toodownload, například **3186843**a potom klikněte na **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="c920c-106">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload, for example **3186843**, and then click **Search**.</span></span>
   
    <span data-ttu-id="c920c-107">Hello opravu hotfix seznamu se zobrazí, například **kumulativní softwaru sady aktualizace 3.0 pro zařízení StorSimple řady 8000**.</span><span class="sxs-lookup"><span data-stu-id="c920c-107">hello hotfix listing appears, for example, **Cumulative Software Bundle Update 3.0 for StorSimple 8000 Series**.</span></span>
   
    ![Prohledávání katalogu](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)
4. <span data-ttu-id="c920c-109">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="c920c-109">Click **Add**.</span></span> <span data-ttu-id="c920c-110">aktualizace Hello je přidána toohello košík.</span><span class="sxs-lookup"><span data-stu-id="c920c-110">hello update is added toohello basket.</span></span>
5. <span data-ttu-id="c920c-111">Vyhledejte všechny další opravy hotfix uvedené v předchozí tabulce hello (**3186859**) a přidejte každý toohello košík.</span><span class="sxs-lookup"><span data-stu-id="c920c-111">Search for any additional hotfixes listed in hello table above (**3186859**), and add each toohello basket.</span></span>
6. <span data-ttu-id="c920c-112">Klikněte na **Zobrazit košík**.</span><span class="sxs-lookup"><span data-stu-id="c920c-112">Click **View Basket**.</span></span>
7. <span data-ttu-id="c920c-113">Klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="c920c-113">Click **Download**.</span></span> <span data-ttu-id="c920c-114">Zadejte nebo **Procházet** tooa místního umístění, kam má hello stáhne tooappear.</span><span class="sxs-lookup"><span data-stu-id="c920c-114">Specify or **Browse** tooa local location where you want hello downloads tooappear.</span></span> <span data-ttu-id="c920c-115">Hello aktualizace se stáhnou toohello zadané umístění a umístit do dílčí složky s hello stejný název jako hello aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c920c-115">hello updates are downloaded toohello specified location and placed in a sub-folder with hello same name as hello update.</span></span> <span data-ttu-id="c920c-116">Hello složce může být také síťové sdílené složky zkopírovaný tooa, který je dosažitelný z hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="c920c-116">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>

> [!NOTE]
> <span data-ttu-id="c920c-117">opravy hotfix Hello musí být dostupný z obou řadičích toodetect všechny potenciální chybové zprávy z hello sdílené řadiče.</span><span class="sxs-lookup"><span data-stu-id="c920c-117">hello hotfixes must be accessible from both controllers toodetect any potential error messages from hello peer controller.</span></span>
> 
> 

#### <a name="tooinstall-and-verify-regular-mode-hotfixes"></a><span data-ttu-id="c920c-118">tooinstall a ověřte regulární režimu opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="c920c-118">tooinstall and verify regular mode hotfixes</span></span>
<span data-ttu-id="c920c-119">Proveďte následující kroky tooinstall hello a ověřte regular režimu opravy hotfix.</span><span class="sxs-lookup"><span data-stu-id="c920c-119">Perform hello following steps tooinstall and verify regular-mode hotfixes.</span></span> <span data-ttu-id="c920c-120">Pokud jste již nainstalovali pomocí hello portálu Azure, přeskočit příliš[instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes).</span><span class="sxs-lookup"><span data-stu-id="c920c-120">If you already installed them using hello Azure Portal, skip ahead too[install and verify maintenance mode hotfixes](#to-install-and-verify-maintenance-mode-hotfixes).</span></span>

1. <span data-ttu-id="c920c-121">tooinstall hello opravy hotfix, které rozhraní Windows PowerShell hello přístupu v konzole sériového portu zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="c920c-121">tooinstall hello hotfixes, access hello Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="c920c-122">Postupujte podle hello podrobné pokyny v [použití klienta PuTTy tooconnect toohello konzoly sériového portu](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="c920c-122">Follow hello detailed instructions in [Use PuTTy tooconnect toohello serial console](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span> <span data-ttu-id="c920c-123">Na příkazovém řádku hello, stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="c920c-123">At hello command prompt, press **Enter**.</span></span>
2. <span data-ttu-id="c920c-124">Vyberte **možnost 1** toolog na toohello zařízení s úplným přístupem.</span><span class="sxs-lookup"><span data-stu-id="c920c-124">Select **Option 1** toolog on toohello device with full access.</span></span> <span data-ttu-id="c920c-125">Doporučujeme nejprve nainstalujte opravu hotfix hello na pasivní řadiči hello.</span><span class="sxs-lookup"><span data-stu-id="c920c-125">We recommend that you install hello hotfix on hello passive controller first.</span></span>
3. <span data-ttu-id="c920c-126">tooinstall hello oprav hotfix, hello příkazového řádku, zadejte:</span><span class="sxs-lookup"><span data-stu-id="c920c-126">tooinstall hello hotfix, at hello command prompt, type:</span></span>
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="c920c-127">Používejte IP místo DNS v cestě sdílené složky v hello výše příkaz.</span><span class="sxs-lookup"><span data-stu-id="c920c-127">Use IP rather than DNS in share path in hello above command.</span></span> <span data-ttu-id="c920c-128">Parametr credential Hello se používá pouze v případě, že se připojujete ověřené sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="c920c-128">hello credential parameter is used only if you are accessing an authenticated share.</span></span>
   
    <span data-ttu-id="c920c-129">Doporučujeme použít hello sdílené složky tooaccess parametr přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="c920c-129">We recommend that you use hello credential parameter tooaccess shares.</span></span> <span data-ttu-id="c920c-130">I sdílené složky, které jsou otevřené příliš "everyone" jsou obvykle otevřít toounauthenticated uživatele.</span><span class="sxs-lookup"><span data-stu-id="c920c-130">Even shares that are open too“everyone” are typically not open toounauthenticated users.</span></span>
   
    <span data-ttu-id="c920c-131">Zadejte heslo hello po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="c920c-131">Supply hello password when prompted.</span></span>
   
    <span data-ttu-id="c920c-132">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="c920c-132">A sample output is shown below.</span></span>
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \hcsmdssoftwareupdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts hello hotfix installation and could reboot one or
        both of hello controllers. If hello device is serving I/Os, these will not
        be disrupted. Are you sure you want toocontinue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
4. <span data-ttu-id="c920c-133">Typ **Y** při výzvami tooconfirm hello instalace opravy hotfix.</span><span class="sxs-lookup"><span data-stu-id="c920c-133">Type **Y** when prompted tooconfirm hello hotfix installation.</span></span>
5. <span data-ttu-id="c920c-134">Monitorovat hello aktualizace pomocí hello `Get-HcsUpdateStatus` rutiny.</span><span class="sxs-lookup"><span data-stu-id="c920c-134">Monitor hello update by using hello `Get-HcsUpdateStatus` cmdlet.</span></span> <span data-ttu-id="c920c-135">aktualizace Hello nejprve dokončí na pasivní řadiči hello.</span><span class="sxs-lookup"><span data-stu-id="c920c-135">hello update will first complete on hello passive controller.</span></span> <span data-ttu-id="c920c-136">Jakmile řadič pasivní hello je aktualizován, budou existovat převzetí služeb při selhání a aktualizace hello pak se použije na hello jiný řadič.</span><span class="sxs-lookup"><span data-stu-id="c920c-136">Once hello passive controller is updated, there will be a failover and hello update will then get applied on hello other controller.</span></span> <span data-ttu-id="c920c-137">Když jsou aktualizovány oba řadiče hello po dokončení aktualizace Hello.</span><span class="sxs-lookup"><span data-stu-id="c920c-137">hello update is complete when both hello controllers are updated.</span></span>
   
    <span data-ttu-id="c920c-138">Hello následující ukázkový výstup ukazuje hello aktualizace v průběhu.</span><span class="sxs-lookup"><span data-stu-id="c920c-138">hello following sample output shows hello update in progress.</span></span> <span data-ttu-id="c920c-139">Hello `RunInprogress` bude `True` při hello Probíhá aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c920c-139">hello `RunInprogress` will be `True` when hello update is in progress.</span></span>

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 8/29/2016 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     <span data-ttu-id="c920c-140">Následující ukázkový výstup Hello označuje, že po dokončení této aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="c920c-140">hello following sample output indicates that hello update is finished.</span></span> <span data-ttu-id="c920c-141">Hello `RunInProgress` bude `False` po dokončení aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="c920c-141">hello `RunInProgress` will be `False` when hello update has completed.</span></span>
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 8/30/2016 9:15:55 AM
    LastUpdateTimestamp : 8/30/2016 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE] 
    > <span data-ttu-id="c920c-142">V některých případech hello rutiny sestavy `False` po hello aktualizaci stále probíhá.</span><span class="sxs-lookup"><span data-stu-id="c920c-142">Occasionally, hello cmdlet reports `False` when hello update is still in progress.</span></span> <span data-ttu-id="c920c-143">tooensure, který hello opravu hotfix je dokončena, počkejte několik minut, spusťte tento příkaz znovu a ověřte, že hello `RunInProgress` je `False`.</span><span class="sxs-lookup"><span data-stu-id="c920c-143">tooensure that hello hotfix is complete, wait for a few minutes, rerun this command and verify that hello `RunInProgress` is `False`.</span></span> <span data-ttu-id="c920c-144">Pokud se jedná, byla dokončena hello opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="c920c-144">If it is, then hello hotfix has completed.</span></span>

1. <span data-ttu-id="c920c-145">Po dokončení aktualizace softwaru hello ověřte verzí softwaru systému hello.</span><span class="sxs-lookup"><span data-stu-id="c920c-145">After hello software update is complete, verify hello system software versions.</span></span> <span data-ttu-id="c920c-146">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="c920c-146">Type:</span></span>
   
    `Get-HcsSystem`
   
    <span data-ttu-id="c920c-147">Měli byste vidět hello následující verze:</span><span class="sxs-lookup"><span data-stu-id="c920c-147">You should see hello following versions:</span></span>
   
   * `HcsSoftwareVersion: 6.3.9600.17759`
   * `CisAgentVersion:  1.0.9343.0`
   * `MdsAgentVersion: 30.0.4698.16`
     
     <span data-ttu-id="c920c-148">Pokud po použití aktualizace hello neměňte čísla verzí hello, znamená to, že oprava hotfix hello se nezdařilo tooapply.</span><span class="sxs-lookup"><span data-stu-id="c920c-148">If hello version numbers do not change after applying hello update, it indicates that hello hotfix has failed tooapply.</span></span> <span data-ttu-id="c920c-149">Pokud je to váš případ a potřebujete další pomoc, kontaktujte [podporu Microsoftu](../articles/storsimple/storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="c920c-149">Should you see this, please contact [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) for further assistance.</span></span>
     
     > [!IMPORTANT]
     > <span data-ttu-id="c920c-150">Je nutné restartovat řadič active hello prostřednictvím hello `Restart-HcsController` rutiny před použitím hello zbývající aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c920c-150">You must restart hello active controller via hello `Restart-HcsController` cmdlet before applying hello remaining updates.</span></span> 
     > 
     > 
2. <span data-ttu-id="c920c-151">Opakujte kroky 3 až 5 tooinstall hello LSI ovladače a firmware opravu hotfix **KB3186859**.</span><span class="sxs-lookup"><span data-stu-id="c920c-151">Repeat steps 3-5 tooinstall hello LSI driver and firmware hotfix **KB3186859**.</span></span> <span data-ttu-id="c920c-152">Po instalaci opravy hotfix hello použít hello `Get-HcsSystem` rutiny.</span><span class="sxs-lookup"><span data-stu-id="c920c-152">After hello hotfix is installed, use hello `Get-HcsSystem` cmdlet.</span></span> <span data-ttu-id="c920c-153">Hello LSI verze musí být:</span><span class="sxs-lookup"><span data-stu-id="c920c-153">hello LSI version should be:</span></span>
   
   * `Lsisas2Version: 2.0.78.00`
3. <span data-ttu-id="c920c-154">Opakujte kroky 3 až 5 tooinstall hello Storport a aktualizace Spaceport **KB3121261**.</span><span class="sxs-lookup"><span data-stu-id="c920c-154">Repeat steps 3-5 tooinstall hello Storport and Spaceport update **KB3121261**.</span></span>
4. <span data-ttu-id="c920c-155">Při aktualizaci Update 2 nebo dřívější verzi, budete také potřebovat toodownload:</span><span class="sxs-lookup"><span data-stu-id="c920c-155">If you are updating from Update 2 or earlier version, you will also need toodownload:</span></span>
   
   * <span data-ttu-id="c920c-156">Oprava iSCSI pomocí KB3146621</span><span class="sxs-lookup"><span data-stu-id="c920c-156">iSCSI fix using KB3146621</span></span>
   * <span data-ttu-id="c920c-157">Oprava rozhraní WMI pomocí KB3103616</span><span class="sxs-lookup"><span data-stu-id="c920c-157">WMI fix using KB3103616</span></span>

#### <a name="tooinstall-and-verify-maintenance-mode-hotfixes"></a><span data-ttu-id="c920c-158">tooinstall a ověřte opravy hotfix režimu údržby</span><span class="sxs-lookup"><span data-stu-id="c920c-158">tooinstall and verify maintenance mode hotfixes</span></span>
<span data-ttu-id="c920c-159">Aktualizace firmwaru disku tooinstall KB3121899 použijte.</span><span class="sxs-lookup"><span data-stu-id="c920c-159">Use KB3121899 tooinstall disk firmware updates.</span></span> <span data-ttu-id="c920c-160">Tyto jsou rušivý aktualizace a trvat toocomplete přibližně 30 minut.</span><span class="sxs-lookup"><span data-stu-id="c920c-160">These are disruptive updates and take around 30 minutes toocomplete.</span></span> <span data-ttu-id="c920c-161">Můžete zvolit tooinstall v plánované údržby pomocí připojování konzoly sériového portu zařízení toohello.</span><span class="sxs-lookup"><span data-stu-id="c920c-161">You can choose tooinstall these in a planned maintenance window by connecting toohello device serial console.</span></span>

<span data-ttu-id="c920c-162">Všimněte si, že pokud firmware disku je již aktuální, nebudete potřebovat tooinstall tyto aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c920c-162">Note that if your disk firmware is already up-to-date, you won't need tooinstall these updates.</span></span> <span data-ttu-id="c920c-163">Spustit hello `Get-HcsUpdateAvailability` rutiny z toocheck konzoly sériového portu zařízení hello Pokud aktualizace jsou k dispozici a zda text hello aktualizace jsou rušivý (režim údržby) nebo omezovaly (regulární režim) aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c920c-163">Run hello `Get-HcsUpdateAvailability` cmdlet from hello device serial console toocheck if updates are available and whether hello updates are disruptive (maintenance mode) or non-disruptive (regular mode) updates.</span></span>

<span data-ttu-id="c920c-164">aktualizace firmwaru disku tooinstall hello, postupujte podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="c920c-164">tooinstall hello disk firmware updates, follow hello instructions below.</span></span>

1. <span data-ttu-id="c920c-165">Umístěte hello zařízení v režimu údržby hello.</span><span class="sxs-lookup"><span data-stu-id="c920c-165">Place hello device in hello Maintenance mode.</span></span> <span data-ttu-id="c920c-166">Všimněte si, že byste neměli používat vzdálenou komunikaci prostředí Windows PowerShell, při připojování zařízení tooa v režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="c920c-166">Note that you should not use Windows PowerShell remoting when connecting tooa device in Maintenance mode.</span></span> <span data-ttu-id="c920c-167">Místo toho spusťte tuto rutinu v hello zařízení řadiče při připojení prostřednictvím sériové konzoly hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="c920c-167">Instead run this cmdlet on hello device controller when connected through hello device serial console.</span></span> <span data-ttu-id="c920c-168">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="c920c-168">Type:</span></span>
   
    `Enter-HcsMaintenanceMode`
   
    <span data-ttu-id="c920c-169">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="c920c-169">A sample output is shown below.</span></span>
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update2-8100-SHG0997879L76673
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected tooController0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    <span data-ttu-id="c920c-170">Oba řadiče hello potom restartujte do režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="c920c-170">Both hello controllers then restart into Maintenance mode.</span></span>
2. <span data-ttu-id="c920c-171">tooinstall hello disku aktualizaci firmwaru, typ:</span><span class="sxs-lookup"><span data-stu-id="c920c-171">tooinstall hello disk firmware update, type:</span></span>
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="c920c-172">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="c920c-172">A sample output is shown below.</span></span>
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After hello hotfix is installed on this controller, install it on hello peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of hello controllers. By installing new updates you agree to, and accept any additional terms associated with, hello new functionality listed in hello release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want toocontinue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes toocomplete.
3. <span data-ttu-id="c920c-173">Monitorování hello instalovat průběh pomocí `Get-HcsUpdateStatus` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c920c-173">Monitor hello install progress using `Get-HcsUpdateStatus` command.</span></span> <span data-ttu-id="c920c-174">Hello po dokončení aktualizace při hello `RunInProgress` změní příliš`False`.</span><span class="sxs-lookup"><span data-stu-id="c920c-174">hello update is complete when hello `RunInProgress` changes too`False`.</span></span>
4. <span data-ttu-id="c920c-175">Po dokončení instalace hello hello řadiče, na které hello byla nainstalována oprava hotfix režimu údržby se restartuje.</span><span class="sxs-lookup"><span data-stu-id="c920c-175">After hello installation is complete, hello controller on which hello maintenance mode hotfix was installed restarts.</span></span> <span data-ttu-id="c920c-176">Přihlaste se jako možnost 1 s úplným přístupem a ověřit verzi firmwaru hello disku.</span><span class="sxs-lookup"><span data-stu-id="c920c-176">Log in as option 1 with full access and verify hello disk firmware version.</span></span> <span data-ttu-id="c920c-177">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="c920c-177">Type:</span></span>
   
   `Get-HcsFirmwareVersion`
   
   <span data-ttu-id="c920c-178">Hello očekává, že jsou verzí firmwaru disku:</span><span class="sxs-lookup"><span data-stu-id="c920c-178">hello expected disk firmware versions are:</span></span>
   
   `XMGG, XGEG, KZ50, F6C2, VR08`
   
   <span data-ttu-id="c920c-179">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="c920c-179">A sample output is shown below.</span></span>
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8100
       Name: Update2-8100-SHG0997879L76YD
       Software Version: 6.3.9600.17705
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected tooController1
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
   
    <span data-ttu-id="c920c-180">Spustit hello `Get-HcsFirmwareVersion` příkaz na hello druhý řadič tooverify který hello verze softwaru se aktualizovala.</span><span class="sxs-lookup"><span data-stu-id="c920c-180">Run hello `Get-HcsFirmwareVersion` command on hello second controller tooverify that hello software version has been updated.</span></span> <span data-ttu-id="c920c-181">Potom můžete ukončit režim údržby hello.</span><span class="sxs-lookup"><span data-stu-id="c920c-181">You can then exit hello maintenance mode.</span></span> <span data-ttu-id="c920c-182">toodo Ano, zadejte následující příkaz pro každý řadič zařízení hello:</span><span class="sxs-lookup"><span data-stu-id="c920c-182">toodo so, type hello following command for each device controller:</span></span>
   
   `Exit-HcsMaintenanceMode`
5. <span data-ttu-id="c920c-183">Hello řadiče restartovat po ukončení režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="c920c-183">hello controllers restart when you exit Maintenance mode.</span></span> <span data-ttu-id="c920c-184">Po hello disk firmware se úspěšně aktualizace a hello zařízení ukončilo režim údržby, návratový toohello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="c920c-184">After hello disk firmware updates are successfully applied and hello device has exited maintenance mode, return toohello Azure classic portal.</span></span> <span data-ttu-id="c920c-185">Všimněte si, že tohoto portálu hello nemusí zobrazit, že jste nainstalovali aktualizace režimu údržby hello 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="c920c-185">Note that hello portal might not show that you installed hello Maintenance mode updates for 24 hours.</span></span>

