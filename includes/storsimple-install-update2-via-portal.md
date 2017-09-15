<!--author=alkohli last changed: 02/06/17-->

#### <a name="to-install-an-update-from-the-azure-portal"></a><span data-ttu-id="a9c49-101">Instalace aktualizace z webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a9c49-101">To install an update from the Azure portal</span></span>

1. <span data-ttu-id="a9c49-102">Na stránce služby StorSimple vyberte zařízení.</span><span class="sxs-lookup"><span data-stu-id="a9c49-102">On the StorSimple service page, select your device.</span></span> <span data-ttu-id="a9c49-103">Přejděte do **Zařízení** > **Údržba**.</span><span class="sxs-lookup"><span data-stu-id="a9c49-103">Navigate to **Devices** > **Maintenance**.</span></span>
2. <span data-ttu-id="a9c49-104">Ve spodní části stránky klikněte na **Vyhledat aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="a9c49-104">At the bottom of the page, click **Scan Updates**.</span></span> <span data-ttu-id="a9c49-105">Vytvoří se úloha vyhledávání dostupných aktualizací.</span><span class="sxs-lookup"><span data-stu-id="a9c49-105">A job is created to scan for available updates.</span></span> <span data-ttu-id="a9c49-106">Po úspěšném dokončení úlohy se zobrazí zpráva.</span><span class="sxs-lookup"><span data-stu-id="a9c49-106">You are notified when the job completes successfully.</span></span>
3. <span data-ttu-id="a9c49-107">V části **Aktualizace softwaru** na stejné stránce budou k dispozici nové aktualizace softwaru.</span><span class="sxs-lookup"><span data-stu-id="a9c49-107">In the **Software Updates** section on the same page, the new software updates are available.</span></span> <span data-ttu-id="a9c49-108">Doporučujeme, abyste si před instalací aktualizace na zařízení prošli poznámky k verzi.</span><span class="sxs-lookup"><span data-stu-id="a9c49-108">We recommend that you review the release notes before you apply an update on your device.</span></span>
4. <span data-ttu-id="a9c49-109">V dolní části stránky klikněte na **Nainstalovat aktualizace** a potom na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a9c49-109">At the bottom of the page, click **Install Updates**, and then **OK**.</span></span>
5. <span data-ttu-id="a9c49-110">V dialogovém okně **Instalace aktualizací** se ujistěte, že jste postupovali podle doporučení, potom vyberte **Rozumím výše uvedeným požadavkům a jsem připraven aktualizovat zařízení** a klikněte na tlačítko se symbolem zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="a9c49-110">In the **Install updates** dialog box, make sure that you've followed the recommendations, then select **I understand the above requirement and am ready to upgrade my device** and click the check button.</span></span>
   
    ![Potvrzovací zpráva](./media/storsimple-install-update2-via-portal/InstallUpdate12_2M.png)
6. <span data-ttu-id="a9c49-112">Spustí se sada kontrol požadavků.</span><span class="sxs-lookup"><span data-stu-id="a9c49-112">A set of prerequisite checks starts.</span></span> <span data-ttu-id="a9c49-113">Mezi tyto kontroly patří:</span><span class="sxs-lookup"><span data-stu-id="a9c49-113">These checks include:</span></span>
   
   * <span data-ttu-id="a9c49-114">**Kontroly stavu kontrolerů** pro ověření, že oba kontrolery zařízení jsou v pořádku a online.</span><span class="sxs-lookup"><span data-stu-id="a9c49-114">**Controller health checks** to verify that both the device controllers are healthy and online.</span></span>
   * <span data-ttu-id="a9c49-115">**Kontroly stavu hardwarových komponent** pro ověření, že všechny hardwarové komponenty v zařízení StorSimple jsou v pořádku.</span><span class="sxs-lookup"><span data-stu-id="a9c49-115">**Hardware component health checks** to verify that all the hardware components on your StorSimple device are healthy.</span></span>
   * <span data-ttu-id="a9c49-116">**Kontroly rozhraní DATA 0** pro ověření, že je na zařízení povolené rozhraní DATA 0.</span><span class="sxs-lookup"><span data-stu-id="a9c49-116">**DATA 0 checks** to verify that DATA 0 is enabled on your device.</span></span> <span data-ttu-id="a9c49-117">Pokud toto rozhraní není povolené, musíte jej povolit a poté to zkusit znovu.</span><span class="sxs-lookup"><span data-stu-id="a9c49-117">If this interface is not enabled, you must enable it and then retry.</span></span>
   * <span data-ttu-id="a9c49-118">**Kontroly rozhraní DATA 2 a DATA 3** pro ověření, že nejsou povolená síťová rozhraní DATA 2 a DATA 3.</span><span class="sxs-lookup"><span data-stu-id="a9c49-118">**DATA 2 and DATA 3 checks** to verify that DATA 2 and DATA 3 network interfaces are not enabled.</span></span> <span data-ttu-id="a9c49-119">Pokud jsou tato rozhraní povolená, musíte je zakázat a potom se pokusit o aktualizaci zařízení.</span><span class="sxs-lookup"><span data-stu-id="a9c49-119">If these interfaces are enabled, then you must disable these and then try to update your device.</span></span> <span data-ttu-id="a9c49-120">Tato kontrola se provádí, pouze pokud provádíte aktualizaci ze zařízení, na kterém běží software verze GA.</span><span class="sxs-lookup"><span data-stu-id="a9c49-120">This check is performed only if you are updating from a device running GA software.</span></span> <span data-ttu-id="a9c49-121">U zařízení s verzemi 0.1, 0.2 nebo 0.3 tato kontrola není nutná.</span><span class="sxs-lookup"><span data-stu-id="a9c49-121">Devices running versions 0.1, 0.2, or 0.3 will not need this check.</span></span>
   * <span data-ttu-id="a9c49-122">**Kontrola brány** na zařízeních s verzí starší než Update 1.</span><span class="sxs-lookup"><span data-stu-id="a9c49-122">**Gateway check** on any device running a version prior to Update 1.</span></span> <span data-ttu-id="a9c49-123">Tato kontrola se provádí na všech zařízeních se softwarem starší verze než Update 1, ale selže na zařízeních s bránou nakonfigurovanou pro jiné síťové rozhraní než DATA 0.</span><span class="sxs-lookup"><span data-stu-id="a9c49-123">This check is performed on all the device running pre-update 1 software but fails on the devices that have a gateway configured for a network interface other than DATA 0.</span></span>
     
     <span data-ttu-id="a9c49-124">Aktualizace se nainstaluje, pokud se úspěšně dokončí všechny kontroly.</span><span class="sxs-lookup"><span data-stu-id="a9c49-124">The update is applied if all checks are successfully completed.</span></span> <span data-ttu-id="a9c49-125">Na probíhání kontrol budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="a9c49-125">You are notified when the checks are in progress.</span></span>
     
     ![Upozornění na předběžnou kontrolu](./media/storsimple-install-update2-via-portal/InstallUpdate12_3M.png)
     
     <span data-ttu-id="a9c49-127">V následujícím příkladu je ukázka selhání kontrol.</span><span class="sxs-lookup"><span data-stu-id="a9c49-127">The following is an example in which the checks failed.</span></span> <span data-ttu-id="a9c49-128">Je třeba ověřit, že oba kontrolery zařízení jsou v pořádku a online.</span><span class="sxs-lookup"><span data-stu-id="a9c49-128">You must verify that both the device controllers are healthy and online.</span></span> <span data-ttu-id="a9c49-129">Také je třeba zkontrolovat stav hardwarových komponent.</span><span class="sxs-lookup"><span data-stu-id="a9c49-129">You also need to check the health of the hardware components.</span></span> <span data-ttu-id="a9c49-130">V tomto příkladu vyžadují pozornost komponenty Controller 0 a Controller 1.</span><span class="sxs-lookup"><span data-stu-id="a9c49-130">In this example, Controller 0 and Controller 1 components need attention.</span></span> <span data-ttu-id="a9c49-131">Pokud si s těmito problémy neporadíte sami, bude muset kontaktovat podporu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="a9c49-131">You may need to contact Microsoft Support if you cannot address these issues by yourself.</span></span>
     
       ![Selhání kontrol](./media/storsimple-install-update2-via-portal/HCS_PreUpgradeChecksFailed-include.png)
7. <span data-ttu-id="a9c49-133">Po úspěšném dokončení kontrol se vytvoří úloha aktualizace.</span><span class="sxs-lookup"><span data-stu-id="a9c49-133">After the checks are successfully completed, an update job is created.</span></span> <span data-ttu-id="a9c49-134">Po úspěšném vytvoření úlohy aktualizace se zobrazí upozornění.</span><span class="sxs-lookup"><span data-stu-id="a9c49-134">You are notified when the update job is successfully created.</span></span>
   
    ![Vytvoření úlohy aktualizace](./media/storsimple-install-update2-via-portal/InstallUpdate12_44M.png)
   
    <span data-ttu-id="a9c49-136">Aktualizace se potom nainstaluje na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="a9c49-136">The update is then applied on your device.</span></span>
    
8. <span data-ttu-id="a9c49-137">Pokud chcete sledovat průběh úlohy aktualizace, klikněte na **Zobrazit úlohu**.</span><span class="sxs-lookup"><span data-stu-id="a9c49-137">To monitor the progress of the update job, click **View Job**.</span></span> <span data-ttu-id="a9c49-138">Na stránce **Úlohy** se zobrazí průběh aktualizace.</span><span class="sxs-lookup"><span data-stu-id="a9c49-138">On the **Jobs** page, you can see the update progress.</span></span>
9. <span data-ttu-id="a9c49-139">Dokončení aktualizace trvá několik hodin.</span><span class="sxs-lookup"><span data-stu-id="a9c49-139">The update takes a few hours to complete.</span></span> <span data-ttu-id="a9c49-140">Podrobnosti o úloze můžete kdykoli zobrazit výběrem úlohy aktualizace a kliknutím na **Podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="a9c49-140">Select the update job and click **Details** to view the details of the job at any time.</span></span>
10. <span data-ttu-id="a9c49-141">Po dokončení úlohy přejděte na stránku **Údržba** a přejděte dolů do části **Aktualizace softwaru**.</span><span class="sxs-lookup"><span data-stu-id="a9c49-141">After the job is complete, navigate to the **Maintenance** page and scroll down to **Software Updates**.</span></span>
