<!--author=alkohli last changed: 08/21/17-->

#### <a name="toodownload-hotfixes"></a><span data-ttu-id="511c5-101">toodownload opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="511c5-101">toodownload hotfixes</span></span>

<span data-ttu-id="511c5-102">Proveďte následující kroky toodownload hello softwarové aktualizace z katalogu služby Microsoft Update hello hello.</span><span class="sxs-lookup"><span data-stu-id="511c5-102">Perform hello following steps toodownload hello software update from hello Microsoft Update Catalog.</span></span>

1. <span data-ttu-id="511c5-103">Spusťte Internet Explorer a přejděte příliš[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="511c5-103">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="511c5-104">Pokud používáte hello katalogu služby Microsoft Update na tomto počítači poprvé, klikněte na tlačítko **nainstalovat** při výzvami tooinstall hello rozšíření katalogu služby Microsoft Update.</span><span class="sxs-lookup"><span data-stu-id="511c5-104">If this is your first time using hello Microsoft Update Catalog on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>

    ![Instalace katalogu](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)

3. <span data-ttu-id="511c5-106">Do vyhledávacího pole hello hello katalogu služby Microsoft Update, zadejte číslo znalostní báze Knowledge Base (KB) hello opravy hotfix hello chcete toodownload, například **4037264**a potom klikněte na **vyhledávání**.</span><span class="sxs-lookup"><span data-stu-id="511c5-106">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload, for example **4037264**, and then click **Search**.</span></span>
   
    <span data-ttu-id="511c5-107">Hello opravu hotfix seznamu se zobrazí, například **kumulativní softwaru sady aktualizací 5.0 pro zařízení StorSimple řady 8000**.</span><span class="sxs-lookup"><span data-stu-id="511c5-107">hello hotfix listing appears, for example, **Cumulative Software Bundle Update 5.0 for StorSimple 8000 Series**.</span></span>
   
    ![Prohledávání katalogu](./media/storsimple-install-update5-hotfix/update-catalog-search.png)

4. <span data-ttu-id="511c5-109">Klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="511c5-109">Click **Download**.</span></span> <span data-ttu-id="511c5-110">Zadejte nebo **Procházet** tooa místního umístění, kam má hello stáhne tooappear.</span><span class="sxs-lookup"><span data-stu-id="511c5-110">Specify or **Browse** tooa local location where you want hello downloads tooappear.</span></span> <span data-ttu-id="511c5-111">Klikněte na tlačítko hello soubory toodownload toohello zadané umístění a složky.</span><span class="sxs-lookup"><span data-stu-id="511c5-111">Click hello files toodownload toohello specified location and folder.</span></span> <span data-ttu-id="511c5-112">Hello složce může být také síťové sdílené složky zkopírovaný tooa, který je dosažitelný z hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="511c5-112">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>
5. <span data-ttu-id="511c5-113">Vyhledejte všechny další opravy hotfix uvedené v předchozí tabulce hello (**4037266**), a stažení hello odpovídající soubory toohello určitých složek, jak je uvedeno v předcházející tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="511c5-113">Search for any additional hotfixes listed in hello table above (**4037266**), and download hello corresponding files toohello specific folders as listed in hello preceding table.</span></span>

> [!NOTE]
> <span data-ttu-id="511c5-114">opravy hotfix Hello musí být dostupný z obou řadičích toodetect všechny potenciální chybové zprávy z hello sdílené řadiče.</span><span class="sxs-lookup"><span data-stu-id="511c5-114">hello hotfixes must be accessible from both controllers toodetect any potential error messages from hello peer controller.</span></span>
>
> <span data-ttu-id="511c5-115">Hello opravy hotfix je nutné zkopírovat v 3 samostatné složky.</span><span class="sxs-lookup"><span data-stu-id="511c5-115">hello hotfixes must be copied in 3 separate folders.</span></span> <span data-ttu-id="511c5-116">Například aktualizace softwaru, SNS/MDS agenta hello zařízení je možné zkopírovat v _FirstOrderUpdate_ složky, všechny hello jiné omezovaly aktualizace může být zkopírovali v hello _SecondOrderUpdate_ složku, a aktualizace režimu údržby zkopírovali v _ThirdOrderUpdate_ složky.</span><span class="sxs-lookup"><span data-stu-id="511c5-116">For example, hello device software/Cis/MDS agent update can be copied in _FirstOrderUpdate_ folder, all hello other non-disruptive updates could be copied in hello _SecondOrderUpdate_ folder, and maintenance mode updates copied in _ThirdOrderUpdate_ folder.</span></span>

#### <a name="tooinstall-and-verify-regular-mode-hotfixes"></a><span data-ttu-id="511c5-117">tooinstall a ověřte regulární režimu opravy hotfix</span><span class="sxs-lookup"><span data-stu-id="511c5-117">tooinstall and verify regular mode hotfixes</span></span>

<span data-ttu-id="511c5-118">Proveďte následující kroky tooinstall hello a ověřte regular režimu opravy hotfix.</span><span class="sxs-lookup"><span data-stu-id="511c5-118">Perform hello following steps tooinstall and verify regular-mode hotfixes.</span></span> <span data-ttu-id="511c5-119">Pokud jste již nainstalovali pomocí hello portál Azure, přeskočit příliš[instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes).</span><span class="sxs-lookup"><span data-stu-id="511c5-119">If you already installed them using hello Azure portal, skip ahead too[install and verify maintenance mode hotfixes](#to-install-and-verify-maintenance-mode-hotfixes).</span></span>

1. <span data-ttu-id="511c5-120">tooinstall hello opravy hotfix, které rozhraní Windows PowerShell hello přístupu v konzole sériového portu zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="511c5-120">tooinstall hello hotfixes, access hello Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="511c5-121">Postupujte podle hello podrobné pokyny v [použití klienta PuTTy tooconnect toohello konzoly sériového portu](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="511c5-121">Follow hello detailed instructions in [Use PuTTy tooconnect toohello serial console](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span></span> <span data-ttu-id="511c5-122">Na příkazovém řádku hello, stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="511c5-122">At hello command prompt, press **Enter**.</span></span>
2. <span data-ttu-id="511c5-123">Vyberte **možnost 1** toolog na toohello zařízení s úplným přístupem.</span><span class="sxs-lookup"><span data-stu-id="511c5-123">Select **Option 1** toolog on toohello device with full access.</span></span> <span data-ttu-id="511c5-124">Doporučujeme nejprve nainstalujte opravu hotfix hello na pasivní řadiči hello.</span><span class="sxs-lookup"><span data-stu-id="511c5-124">We recommend that you install hello hotfix on hello passive controller first.</span></span>
3. <span data-ttu-id="511c5-125">tooinstall hello oprav hotfix, hello příkazového řádku, zadejte:</span><span class="sxs-lookup"><span data-stu-id="511c5-125">tooinstall hello hotfix, at hello command prompt, type:</span></span>
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="511c5-126">Používejte IP místo DNS v cestě sdílené složky v hello výše příkaz.</span><span class="sxs-lookup"><span data-stu-id="511c5-126">Use IP rather than DNS in share path in hello above command.</span></span> <span data-ttu-id="511c5-127">Parametr credential Hello se používá pouze v případě, že se připojujete ověřené sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="511c5-127">hello credential parameter is used only if you are accessing an authenticated share.</span></span>
   
    <span data-ttu-id="511c5-128">Doporučujeme použít hello sdílené složky tooaccess parametr přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="511c5-128">We recommend that you use hello credential parameter tooaccess shares.</span></span> <span data-ttu-id="511c5-129">I sdílené složky, které jsou otevřené příliš "everyone" jsou obvykle otevřít toounauthenticated uživatele.</span><span class="sxs-lookup"><span data-stu-id="511c5-129">Even shares that are open too“everyone” are typically not open toounauthenticated users.</span></span>
   
4. <span data-ttu-id="511c5-130">Zadejte heslo hello po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="511c5-130">Supply hello password when prompted.</span></span> <span data-ttu-id="511c5-131">Ukázkový výstup pro instalaci první pořadí aktualizací hello je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="511c5-131">A sample output for installing hello first order updates is shown below.</span></span> <span data-ttu-id="511c5-132">Hello první pořadí aktualizací musíte toopoint toohello konkrétní soubor.</span><span class="sxs-lookup"><span data-stu-id="511c5-132">For hello first order update, you need toopoint toohello specific file.</span></span>

    >[!NOTE] 
    > <span data-ttu-id="511c5-133">Měli byste instalaci hello _HcsSoftwareUpdate.exe_ první.</span><span class="sxs-lookup"><span data-stu-id="511c5-133">You should install hello _HcsSoftwareUpdate.exe_ first.</span></span> <span data-ttu-id="511c5-134">Po dokončení této instalace poté nainstalujte _CisMdsAgentUpdate.exe_.</span><span class="sxs-lookup"><span data-stu-id="511c5-134">After this install has completed, then install _CisMdsAgentUpdate.exe_.</span></span>
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \FirstOrderUpdate\HcsSoftwareUpdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts hello hotfix installation and could reboot one or
        both of hello controllers. If hello device is serving I/Os, these will not
        be disrupted. Are you sure you want toocontinue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
5. <span data-ttu-id="511c5-135">Typ **Y** při výzvami tooconfirm hello instalace opravy hotfix.</span><span class="sxs-lookup"><span data-stu-id="511c5-135">Type **Y** when prompted tooconfirm hello hotfix installation.</span></span>
6. <span data-ttu-id="511c5-136">Monitorovat hello aktualizace pomocí hello `Get-HcsUpdateStatus` rutiny.</span><span class="sxs-lookup"><span data-stu-id="511c5-136">Monitor hello update by using hello `Get-HcsUpdateStatus` cmdlet.</span></span> <span data-ttu-id="511c5-137">aktualizace Hello nejprve dokončí na pasivní řadiči hello.</span><span class="sxs-lookup"><span data-stu-id="511c5-137">hello update will first complete on hello passive controller.</span></span> <span data-ttu-id="511c5-138">Jakmile řadič pasivní hello je aktualizován, budou existovat převzetí služeb při selhání a aktualizace hello pak se použije na hello jiný řadič.</span><span class="sxs-lookup"><span data-stu-id="511c5-138">Once hello passive controller is updated, there will be a failover and hello update will then get applied on hello other controller.</span></span> <span data-ttu-id="511c5-139">Když jsou aktualizovány oba řadiče hello po dokončení aktualizace Hello.</span><span class="sxs-lookup"><span data-stu-id="511c5-139">hello update is complete when both hello controllers are updated.</span></span>
   
    <span data-ttu-id="511c5-140">Hello následující ukázkový výstup ukazuje hello aktualizace v průběhu.</span><span class="sxs-lookup"><span data-stu-id="511c5-140">hello following sample output shows hello update in progress.</span></span> <span data-ttu-id="511c5-141">Hello `RunInprogress` je `True` při hello Probíhá aktualizace.</span><span class="sxs-lookup"><span data-stu-id="511c5-141">hello `RunInprogress` is `True` when hello update is in progress.</span></span>

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 07/28/2017 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     <span data-ttu-id="511c5-142">Následující ukázkový výstup Hello označuje, že po dokončení této aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="511c5-142">hello following sample output indicates that hello update is finished.</span></span> <span data-ttu-id="511c5-143">Hello `RunInProgress` je `False` po dokončení aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="511c5-143">hello `RunInProgress` is `False` when hello update is complete.</span></span>
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 07/28/2017 9:15:55 AM
    LastUpdateTimestamp : 07/28/2017 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE]
    > <span data-ttu-id="511c5-144">V některých případech hello rutiny sestavy `False` po hello aktualizaci stále probíhá.</span><span class="sxs-lookup"><span data-stu-id="511c5-144">Occasionally, hello cmdlet reports `False` when hello update is still in progress.</span></span> <span data-ttu-id="511c5-145">tooensure, který hello opravu hotfix je dokončena, počkejte několik minut, spusťte tento příkaz znovu a ověřte, že hello `RunInProgress` je `False`.</span><span class="sxs-lookup"><span data-stu-id="511c5-145">tooensure that hello hotfix is complete, wait for a few minutes, rerun this command and verify that hello `RunInProgress` is `False`.</span></span> <span data-ttu-id="511c5-146">Pokud se jedná, byla dokončena hello opravu hotfix.</span><span class="sxs-lookup"><span data-stu-id="511c5-146">If it is, then hello hotfix has completed.</span></span>

7. <span data-ttu-id="511c5-147">Po dokončení aktualizace softwaru hello ověřte verzí softwaru systému hello.</span><span class="sxs-lookup"><span data-stu-id="511c5-147">After hello software update is complete, verify hello system software versions.</span></span> <span data-ttu-id="511c5-148">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="511c5-148">Type:</span></span>
   
    `Get-HcsSystem`
   
    <span data-ttu-id="511c5-149">Měli byste vidět hello následující verze:</span><span class="sxs-lookup"><span data-stu-id="511c5-149">You should see hello following versions:</span></span>
   
   * `FriendlySoftwareVersion: StorSimple 8000 Series Update 5.0`
   *  `HcsSoftwareVersion: 6.3.9600.17845`
   
    <span data-ttu-id="511c5-150">Pokud hello číslo verze se nezmění po použití aktualizace hello, znamená to, že oprava hotfix hello se nezdařilo tooapply.</span><span class="sxs-lookup"><span data-stu-id="511c5-150">If hello version number does not change after applying hello update, it indicates that hello hotfix has failed tooapply.</span></span> <span data-ttu-id="511c5-151">Pokud je to váš případ a potřebujete další pomoc, kontaktujte [podporu Microsoftu](../articles/storsimple/storsimple-8000-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="511c5-151">Should you see this, please contact [Microsoft Support](../articles/storsimple/storsimple-8000-contact-microsoft-support.md) for further assistance.</span></span>
     
    > [!IMPORTANT]
    > <span data-ttu-id="511c5-152">Je nutné restartovat řadič active hello prostřednictvím hello `Restart-HcsController` rutiny před použitím hello další aktualizace.</span><span class="sxs-lookup"><span data-stu-id="511c5-152">You must restart hello active controller via hello `Restart-HcsController` cmdlet before applying hello next update.</span></span>
     
8. <span data-ttu-id="511c5-153">Opakujte kroky 3 až 6 tooinstall hello _CisMDSAgentupdate.exe_ agent stáhne tooyour _FirstOrderUpdate_ složky.</span><span class="sxs-lookup"><span data-stu-id="511c5-153">Repeat steps 3-6 tooinstall hello _CisMDSAgentupdate.exe_ agent downloaded tooyour _FirstOrderUpdate_ folder.</span></span>
8. <span data-ttu-id="511c5-154">Zopakujte kroky 3 až 6 tooinstall hello druhý pořadí aktualizací.</span><span class="sxs-lookup"><span data-stu-id="511c5-154">Repeat steps 3-6 tooinstall hello second order updates.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="511c5-155">Druhý pořadí aktualizací, lze nainstalovat víc aktualizací právě spuštěním hello `Start-HcsHotfix cmdlet` a přejdete toohello složku, kde jsou umístěny druhý pořadí aktualizací.</span><span class="sxs-lookup"><span data-stu-id="511c5-155">For second order updates, multiple updates can be installed by just running hello `Start-HcsHotfix cmdlet` and pointing toohello folder where second order updates are located.</span></span> <span data-ttu-id="511c5-156">rutiny Hello provede všechny hello aktualizace k dispozici ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="511c5-156">hello cmdlet will execute all hello updates available in hello folder.</span></span> <span data-ttu-id="511c5-157">Pokud už je nainstalovaná aktualizace, logiku aktualizace hello rozpozná a tuto aktualizaci nelze použít.</span><span class="sxs-lookup"><span data-stu-id="511c5-157">If an update is already installed, hello update logic will detect that and not apply that update.</span></span>

    <span data-ttu-id="511c5-158">Po instalaci všech hello opravy hotfix, použijte hello `Get-HcsSystem` rutiny.</span><span class="sxs-lookup"><span data-stu-id="511c5-158">After all hello hotfixes are installed, use hello `Get-HcsSystem` cmdlet.</span></span> <span data-ttu-id="511c5-159">verze Hello by měla být:</span><span class="sxs-lookup"><span data-stu-id="511c5-159">hello versions should be:</span></span>
    
    * `CisAgentVersion:  1.0.9724.0`
    * `MdsAgentVersion: 35.2.2.0`
    * `Lsisas2Version: 2.0.78.00`


#### <a name="tooinstall-and-verify-maintenance-mode-hotfixes"></a><span data-ttu-id="511c5-160">tooinstall a ověřte opravy hotfix režimu údržby</span><span class="sxs-lookup"><span data-stu-id="511c5-160">tooinstall and verify maintenance mode hotfixes</span></span>

<span data-ttu-id="511c5-161">Aktualizace firmwaru disku tooinstall KB4037263 použijte.</span><span class="sxs-lookup"><span data-stu-id="511c5-161">Use KB4037263 tooinstall disk firmware updates.</span></span> <span data-ttu-id="511c5-162">Tyto jsou rušivý aktualizace a trvat toocomplete přibližně 30 minut.</span><span class="sxs-lookup"><span data-stu-id="511c5-162">These are disruptive updates and take around 30 minutes toocomplete.</span></span> <span data-ttu-id="511c5-163">Můžete zvolit tooinstall v plánované údržby pomocí připojování konzoly sériového portu zařízení toohello.</span><span class="sxs-lookup"><span data-stu-id="511c5-163">You can choose tooinstall these in a planned maintenance window by connecting toohello device serial console.</span></span>

> [!NOTE] 
> <span data-ttu-id="511c5-164">Pokud už je aktuální firmware disku, nebude nutné tooinstall tyto aktualizace.</span><span class="sxs-lookup"><span data-stu-id="511c5-164">If your disk firmware is already up-to-date, you won't need tooinstall these updates.</span></span> <span data-ttu-id="511c5-165">Spustit hello `Get-HcsUpdateAvailability` rutiny z toocheck konzoly sériového portu zařízení hello Pokud aktualizace jsou k dispozici a zda text hello aktualizace jsou rušivý (režim údržby) nebo omezovaly (regulární režim) aktualizace.</span><span class="sxs-lookup"><span data-stu-id="511c5-165">Run hello `Get-HcsUpdateAvailability` cmdlet from hello device serial console toocheck if updates are available and whether hello updates are disruptive (maintenance mode) or non-disruptive (regular mode) updates.</span></span>

<span data-ttu-id="511c5-166">aktualizace firmwaru disku tooinstall hello, postupujte podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="511c5-166">tooinstall hello disk firmware updates, follow hello instructions below.</span></span>

1. <span data-ttu-id="511c5-167">Umístěte hello zařízení v režimu údržby hello.</span><span class="sxs-lookup"><span data-stu-id="511c5-167">Place hello device in hello maintenance mode.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="511c5-168">Při připojování zařízení tooa v režimu údržby, nepoužívejte vzdálenou komunikaci prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="511c5-168">Do not use Windows PowerShell remoting when connecting tooa device in maintenance mode.</span></span> <span data-ttu-id="511c5-169">Místo toho spusťte tuto rutinu v hello zařízení řadiče při připojení prostřednictvím sériové konzoly hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="511c5-169">Instead run this cmdlet on hello device controller when connected through hello device serial console.</span></span>

    <span data-ttu-id="511c5-170">tooplace hello řadiče v režimu údržby, zadejte:</span><span class="sxs-lookup"><span data-stu-id="511c5-170">tooplace hello controller in maintenance mode, type:</span></span>
   
    `Enter-HcsMaintenanceMode`
   
    <span data-ttu-id="511c5-171">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="511c5-171">A sample output is shown below.</span></span>
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8600
        Name: Update4-8600-mystorsimple
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected tooController0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    <span data-ttu-id="511c5-172">Oba řadiče hello potom restartujte do režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="511c5-172">Both hello controllers then restart into maintenance mode.</span></span>
2. <span data-ttu-id="511c5-173">tooinstall hello disku aktualizaci firmwaru, typ:</span><span class="sxs-lookup"><span data-stu-id="511c5-173">tooinstall hello disk firmware update, type:</span></span>
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="511c5-174">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="511c5-174">A sample output is shown below.</span></span>
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\ThirdOrderUpdates\ -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After hello hotfix is installed on this controller, install it on hello peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of hello controllers. By installing new updates you agree to, and accept any additional terms associated with, hello new functionality listed in hello release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want toocontinue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes toocomplete.
3. <span data-ttu-id="511c5-175">Monitorování hello instalovat průběh pomocí `Get-HcsUpdateStatus` příkaz.</span><span class="sxs-lookup"><span data-stu-id="511c5-175">Monitor hello install progress using `Get-HcsUpdateStatus` command.</span></span> <span data-ttu-id="511c5-176">Hello po dokončení aktualizace při hello `RunInProgress` změní příliš`False`.</span><span class="sxs-lookup"><span data-stu-id="511c5-176">hello update is complete when hello `RunInProgress` changes too`False`.</span></span>
4. <span data-ttu-id="511c5-177">Po dokončení instalace hello hello řadiče, na které hello byla nainstalována oprava hotfix režimu údržby se restartuje.</span><span class="sxs-lookup"><span data-stu-id="511c5-177">After hello installation is complete, hello controller on which hello maintenance mode hotfix was installed restarts.</span></span> <span data-ttu-id="511c5-178">Přihlaste se jako možnost 1 s úplným přístupem a ověřit verzi firmwaru hello disku.</span><span class="sxs-lookup"><span data-stu-id="511c5-178">Log in as option 1 with full access and verify hello disk firmware version.</span></span> <span data-ttu-id="511c5-179">Zadejte:</span><span class="sxs-lookup"><span data-stu-id="511c5-179">Type:</span></span>
   
   `Get-HcsFirmwareVersion`
   
   <span data-ttu-id="511c5-180">Hello očekává, že jsou verzí firmwaru disku:</span><span class="sxs-lookup"><span data-stu-id="511c5-180">hello expected disk firmware versions are:</span></span>
   
   `XMGJ, XGEG, KZ50, F6C2, VR08, N003, 0107`
   
   <span data-ttu-id="511c5-181">Ukázkový výstup najdete níž.</span><span class="sxs-lookup"><span data-stu-id="511c5-181">A sample output is shown below.</span></span>
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8600
       Name: Update4-8600-mystorsimple
       Software Version: 6.3.9600.17845
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected tooController1
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
   
    <span data-ttu-id="511c5-182">Spustit hello `Get-HcsFirmwareVersion` příkaz na hello druhý řadič tooverify který hello verze softwaru se aktualizovala.</span><span class="sxs-lookup"><span data-stu-id="511c5-182">Run hello `Get-HcsFirmwareVersion` command on hello second controller tooverify that hello software version has been updated.</span></span> <span data-ttu-id="511c5-183">Potom můžete ukončit režim údržby hello.</span><span class="sxs-lookup"><span data-stu-id="511c5-183">You can then exit hello maintenance mode.</span></span> <span data-ttu-id="511c5-184">toodo Ano, zadejte následující příkaz pro každý řadič zařízení hello:</span><span class="sxs-lookup"><span data-stu-id="511c5-184">toodo so, type hello following command for each device controller:</span></span>
   
   `Exit-HcsMaintenanceMode`

5. <span data-ttu-id="511c5-185">Hello řadiče restartovat po ukončení režimu údržby.</span><span class="sxs-lookup"><span data-stu-id="511c5-185">hello controllers restart when you exit maintenance mode.</span></span> <span data-ttu-id="511c5-186">Po hello disk firmware se úspěšně aktualizace a hello zařízení ukončilo režim údržby, návratový toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="511c5-186">After hello disk firmware updates are successfully applied and hello device has exited maintenance mode, return toohello Azure portal.</span></span> <span data-ttu-id="511c5-187">Všimněte si, že tohoto portálu hello nemusí zobrazit, že jste nainstalovali aktualizace režimu údržby hello 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="511c5-187">Note that hello portal might not show that you installed hello maintenance mode updates for 24 hours.</span></span>
