<!--author=alkohli last changed: 11/07/16 -->

#### <a name="to-install-updates-via-the-azure-portal"></a><span data-ttu-id="9ce75-101">Instalace aktualizací prostřednictvím webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9ce75-101">To install updates via the Azure portal</span></span>

1. <span data-ttu-id="9ce75-102">Přejděte do Správce zařízení StorSimple a vyberte **Zařízení**.</span><span class="sxs-lookup"><span data-stu-id="9ce75-102">Go to your StorSimple Device Manager and select **Devices**.</span></span> <span data-ttu-id="9ce75-103">Ze seznamu zařízení připojených k vaší službě vyberte a klikněte na zařízení, které chcete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="9ce75-103">From the list of devices connected to your service, select and click the device you want to update.</span></span> 

    ![aktualizace zařízení](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate1m.png) 

2. <span data-ttu-id="9ce75-105">V okně **Nastavení** klikněte na **Aktualizace zařízení**.</span><span class="sxs-lookup"><span data-stu-id="9ce75-105">In the **Settings** blade, click **Device updates**.</span></span> 

    ![aktualizace zařízení](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate2m.png)  

3. <span data-ttu-id="9ce75-107">Pokud jsou k dispozici aktualizace softwaru, zobrazí se zpráva.</span><span class="sxs-lookup"><span data-stu-id="9ce75-107">You see a message if the software updates are available.</span></span> <span data-ttu-id="9ce75-108">Aktualizace můžete zkontrolovat také kliknutím na **Vyhledat**.</span><span class="sxs-lookup"><span data-stu-id="9ce75-108">To check for updates, you can also click **Scan**.</span></span>

    ![aktualizace zařízení](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate3m.png)

    <span data-ttu-id="9ce75-110">Po spuštění a úspěšném dokončení vyhledávání se zobrazí zpráva.</span><span class="sxs-lookup"><span data-stu-id="9ce75-110">You will be notified when the scan starts and completes successfully.</span></span>

    ![aktualizace zařízení](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate5m.png)

4. <span data-ttu-id="9ce75-112">Jakmile budou aktualizace prohledané, klikněte na **Stáhnout aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="9ce75-112">Once the updates are scanned, click **Download updates**.</span></span> 

    ![aktualizace zařízení](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate6m.png)

5. <span data-ttu-id="9ce75-114">V okně **Nové aktualizace** si prohlédněte informaci o nutnosti potvrdit instalaci po stažení aktualizací.</span><span class="sxs-lookup"><span data-stu-id="9ce75-114">In the **New updates** blade, review the information that after the updates are downloaded, you need to confirm the installation.</span></span> <span data-ttu-id="9ce75-115">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="9ce75-115">Click **OK**.</span></span>

    ![aktualizace zařízení](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate7m.png)

6. <span data-ttu-id="9ce75-117">Po spuštění a úspěšném dokončení nahrávání se zobrazí zpráva.</span><span class="sxs-lookup"><span data-stu-id="9ce75-117">You are notified when the upload starts and completes successfully.</span></span>

     ![aktualizace zařízení](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate8m.png)

5. <span data-ttu-id="9ce75-119">V okně **Aktualizace zařízení** klikněte na **Nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="9ce75-119">In the **Device updates** blade, click **Install**.</span></span>

     ![aktualizace zařízení](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate11m.png)   

6. <span data-ttu-id="9ce75-121">V okně **Nové aktualizace** se zobrazí upozornění na narušení běhu zařízení touto aktualizací.</span><span class="sxs-lookup"><span data-stu-id="9ce75-121">In the **New updates** blade, you are warned that the update is disruptive.</span></span> <span data-ttu-id="9ce75-122">Protože je virtuální pole zařízením s jedním uzlem, zařízení se po aktualizaci restartuje.</span><span class="sxs-lookup"><span data-stu-id="9ce75-122">As virtual array is a single node device, the device restarts after it is updated.</span></span> <span data-ttu-id="9ce75-123">To naruší běh všech probíhajících V/V.</span><span class="sxs-lookup"><span data-stu-id="9ce75-123">This disrupts any IO in progress.</span></span> <span data-ttu-id="9ce75-124">Kliknutím na **OK** nainstalujte aktualizace.</span><span class="sxs-lookup"><span data-stu-id="9ce75-124">Click **OK** to install the updates.</span></span> 

    ![aktualizace zařízení](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate12m.png) 

7. <span data-ttu-id="9ce75-126">Po spuštění úlohy instalace se zobrazí zpráva.</span><span class="sxs-lookup"><span data-stu-id="9ce75-126">You are notified when the install job starts.</span></span> 

    ![aktualizace zařízení](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate13m.png)

8.  <span data-ttu-id="9ce75-128">Po úspěšném dokončení úlohy instalace můžete kliknutím na odkaz **Zobrazit úlohu** v okně **Aktualizace zařízení** monitorovat instalaci.</span><span class="sxs-lookup"><span data-stu-id="9ce75-128">After the install job completes successfully, click **View Job** link in the **Device updates** blade to monitor the installation.</span></span> 

    ![aktualizace zařízení](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate15m.png)

    <span data-ttu-id="9ce75-130">Tím přejdete do okna **Instalace aktualizací**.</span><span class="sxs-lookup"><span data-stu-id="9ce75-130">This takes you to the **Install Updates** blade.</span></span> <span data-ttu-id="9ce75-131">Tady si můžete prohlédnout podrobné informace o úloze.</span><span class="sxs-lookup"><span data-stu-id="9ce75-131">You can view detailed information about the job here.</span></span>

    ![aktualizace zařízení](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate16m.png)

9. <span data-ttu-id="9ce75-133">Po úspěšné instalaci aktualizace zobrazí zprávu, která tímto účelem se v okně aktualizace zařízení.</span><span class="sxs-lookup"><span data-stu-id="9ce75-133">After the updates are successfully installed, you see a message to this effect in the Device updates blade.</span></span> <span data-ttu-id="9ce75-134">Verze softwaru také změní na **10.0.10288.0**.</span><span class="sxs-lookup"><span data-stu-id="9ce75-134">The software version also changes to **10.0.10288.0**.</span></span> 

    ![aktualizace zařízení](../includes/media/storsimple-virtual-array-install-update-via-portal/azupdate17m.png)