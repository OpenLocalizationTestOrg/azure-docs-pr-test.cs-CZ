<!--author=alkohli last changed: 08/04/17-->

#### <a name="tooinstall-an-update-from-hello-azure-portal"></a><span data-ttu-id="5a682-101">tooinstall aktualizace z hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5a682-101">tooinstall an update from hello Azure portal</span></span>

1. <span data-ttu-id="5a682-102">Na stránce služby StorSimple hello vyberte zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a682-102">On hello StorSimple service page, select your device.</span></span>

    ![Vyberte zařízení](./media/storsimple-8000-install-update5-via-portal/update1.png)

2. <span data-ttu-id="5a682-104">Přejděte příliš**nastavení zařízení** > **aktualizace zařízení**.</span><span class="sxs-lookup"><span data-stu-id="5a682-104">Navigate too**Device settings** > **Device updates**.</span></span>

    ![Klikněte na tlačítko aktualizace zařízení](./media/storsimple-8000-install-update5-via-portal/update2.png)

2. <span data-ttu-id="5a682-106">Zobrazí se upozornění, pokud jsou k dispozici nové aktualizace.</span><span class="sxs-lookup"><span data-stu-id="5a682-106">A notification appears if new updates are available.</span></span> <span data-ttu-id="5a682-107">Můžete taky v hello **aktualizace zařízení** okně klikněte na tlačítko **kontrolovat aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="5a682-107">Alternatively, in hello **Device updates** blade, click **Scan Updates**.</span></span> <span data-ttu-id="5a682-108">Úloha je vytvořena tooscan dostupné aktualizace.</span><span class="sxs-lookup"><span data-stu-id="5a682-108">A job is created tooscan for available updates.</span></span> <span data-ttu-id="5a682-109">Budete informováni hello úloha úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="5a682-109">You are notified when hello job completes successfully.</span></span>

    ![Klikněte na tlačítko aktualizace zařízení](./media/storsimple-8000-install-update5-via-portal/update3.png)

3. <span data-ttu-id="5a682-111">Doporučujeme, abyste zkontrolovali hello poznámky k verzi před instalací aktualizace na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a682-111">We recommend that you review hello release notes before you apply an update on your device.</span></span> <span data-ttu-id="5a682-112">Klikněte na tlačítko tooapply aktualizace **nainstalovat aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="5a682-112">tooapply updates, click **Install updates**.</span></span> <span data-ttu-id="5a682-113">V hello **potvrďte pravidelné aktualizace** okno, zkontrolujte hello požadavky toocomplete před instalací aktualizace.</span><span class="sxs-lookup"><span data-stu-id="5a682-113">In hello **Confirm regular updates** blade, review hello prerequisites toocomplete before you apply updates.</span></span> <span data-ttu-id="5a682-114">Vyberte zaškrtávací políčko tooindicate hello jsou připravené tooupdate hello zařízení a potom klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="5a682-114">Select hello checkbox tooindicate that you are ready tooupdate hello device and then click **Install**.</span></span>

    ![Klikněte na tlačítko aktualizace zařízení](./media/storsimple-8000-install-update5-via-portal/update4.png)

6. <span data-ttu-id="5a682-116">Spustí se sada kontrol požadavků.</span><span class="sxs-lookup"><span data-stu-id="5a682-116">A set of prerequisite checks starts.</span></span> <span data-ttu-id="5a682-117">Mezi tyto kontroly patří:</span><span class="sxs-lookup"><span data-stu-id="5a682-117">These checks include:</span></span>
   
   * <span data-ttu-id="5a682-118">**Kontroly stavu řadiče** tooverify obě text hello řadiče zařízení jsou v pořádku a online.</span><span class="sxs-lookup"><span data-stu-id="5a682-118">**Controller health checks** tooverify that both hello device controllers are healthy and online.</span></span>
   * <span data-ttu-id="5a682-119">**Kontroly stavu součásti hardwaru** tooverify, že všechny hardwarové součásti v zařízení StorSimple hello jsou v pořádku.</span><span class="sxs-lookup"><span data-stu-id="5a682-119">**Hardware component health checks** tooverify that all hello hardware components on your StorSimple device are healthy.</span></span>
   * <span data-ttu-id="5a682-120">**Ověří DATA 0** tooverify, že DATA 0 je povolena na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a682-120">**DATA 0 checks** tooverify that DATA 0 is enabled on your device.</span></span> <span data-ttu-id="5a682-121">Pokud toto rozhraní není povolené, musíte jej povolit a poté to zkusit znovu.</span><span class="sxs-lookup"><span data-stu-id="5a682-121">If this interface is not enabled, you must enable it and then retry.</span></span>

    <span data-ttu-id="5a682-122">aktualizace Hello je stažen a nainstalován pouze v případě, že jsou všechny kontroly hello úspěšně dokončil.</span><span class="sxs-lookup"><span data-stu-id="5a682-122">hello update is downloaded and installed only if all hello checks are successfully completed.</span></span> <span data-ttu-id="5a682-123">Budete upozorněni, když byly v průběhu kontroly hello.</span><span class="sxs-lookup"><span data-stu-id="5a682-123">You are notified when hello checks are in progress.</span></span> <span data-ttu-id="5a682-124">Pokud hello prechecks selže, pak vám bude poskytnuta s hello příčiny chyby.</span><span class="sxs-lookup"><span data-stu-id="5a682-124">If hello prechecks fail, then you will be provided with hello reasons for failure.</span></span> <span data-ttu-id="5a682-125">Vyřešit tyto problémy a poté opakujte operaci hello.</span><span class="sxs-lookup"><span data-stu-id="5a682-125">Address those issues and then retry hello operation.</span></span> <span data-ttu-id="5a682-126">Pokud nelze vyřešit tyto problémy tak, že sami může být nutné toocontact Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="5a682-126">You may need toocontact Microsoft Support if you cannot address these issues by yourself.</span></span>

7. <span data-ttu-id="5a682-127">Po úspěšném dokončení hello prechecks se vytvoří úloha aktualizace.</span><span class="sxs-lookup"><span data-stu-id="5a682-127">After hello prechecks are successfully completed, an update job is created.</span></span> <span data-ttu-id="5a682-128">Budete informováni hello aktualizace úlohy je úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="5a682-128">You are notified when hello update job is successfully created.</span></span>
   
    ![Vytvoření úlohy aktualizace](./media/storsimple-8000-install-update5-via-portal/update6.png)
   
    <span data-ttu-id="5a682-130">aktualizace Hello se potom použije ve vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="5a682-130">hello update is then applied on your device.</span></span>

9. <span data-ttu-id="5a682-131">aktualizace Hello trvat několik hodin toocomplete.</span><span class="sxs-lookup"><span data-stu-id="5a682-131">hello update takes a few hours toocomplete.</span></span> <span data-ttu-id="5a682-132">Vyberte hello aktualizace úlohu a klikněte na tlačítko **podrobnosti** tooview hello podrobnosti úlohy hello kdykoli.</span><span class="sxs-lookup"><span data-stu-id="5a682-132">Select hello update job and click **Details** tooview hello details of hello job at any time.</span></span>

    ![Vytvoření úlohy aktualizace](./media/storsimple-8000-install-update5-via-portal/update8.png)

     <span data-ttu-id="5a682-134">Také můžete monitorovat průběh hello hello aktualizace úlohy z **nastavení zařízení > úlohy**.</span><span class="sxs-lookup"><span data-stu-id="5a682-134">You can also monitor hello progress of hello update job from **Device settings > Jobs**.</span></span> <span data-ttu-id="5a682-135">Na hello **úlohy** okně uvidíte hello aktualizovat průběh.</span><span class="sxs-lookup"><span data-stu-id="5a682-135">On hello **Jobs** blade, you can see hello update progress.</span></span>

     ![Vytvoření úlohy aktualizace](./media/storsimple-8000-install-update5-via-portal/update7.png)

10. <span data-ttu-id="5a682-137">Po dokončení úlohy hello přejděte toohello **nastavení zařízení > aktualizace zařízení**.</span><span class="sxs-lookup"><span data-stu-id="5a682-137">After hello job is complete, navigate toohello **Device settings > Device updates**.</span></span> <span data-ttu-id="5a682-138">verzi softwaru Hello nyní aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="5a682-138">hello software version should now be updated.</span></span>

