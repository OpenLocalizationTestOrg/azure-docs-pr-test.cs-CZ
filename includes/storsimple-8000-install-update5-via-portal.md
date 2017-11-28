<!--author=alkohli last changed: 08/04/17-->

#### <a name="to-install-an-update-from-the-azure-portal"></a><span data-ttu-id="2c05e-101">Instalace aktualizace z webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2c05e-101">To install an update from the Azure portal</span></span>

1. <span data-ttu-id="2c05e-102">Na stránce služby StorSimple vyberte zařízení.</span><span class="sxs-lookup"><span data-stu-id="2c05e-102">On the StorSimple service page, select your device.</span></span>

    ![Vyberte zařízení](./media/storsimple-8000-install-update5-via-portal/update1.png)

2. <span data-ttu-id="2c05e-104">Přejděte na **nastavení zařízení** > **aktualizace zařízení**.</span><span class="sxs-lookup"><span data-stu-id="2c05e-104">Navigate to **Device settings** > **Device updates**.</span></span>

    ![Klikněte na tlačítko aktualizace zařízení](./media/storsimple-8000-install-update5-via-portal/update2.png)

2. <span data-ttu-id="2c05e-106">Zobrazí se upozornění, pokud jsou k dispozici nové aktualizace.</span><span class="sxs-lookup"><span data-stu-id="2c05e-106">A notification appears if new updates are available.</span></span> <span data-ttu-id="2c05e-107">Můžete taky v **aktualizace zařízení** okně klikněte na tlačítko **kontrolovat aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="2c05e-107">Alternatively, in the **Device updates** blade, click **Scan Updates**.</span></span> <span data-ttu-id="2c05e-108">Vytvoří se úloha vyhledávání dostupných aktualizací.</span><span class="sxs-lookup"><span data-stu-id="2c05e-108">A job is created to scan for available updates.</span></span> <span data-ttu-id="2c05e-109">Po úspěšném dokončení úlohy se zobrazí zpráva.</span><span class="sxs-lookup"><span data-stu-id="2c05e-109">You are notified when the job completes successfully.</span></span>

    ![Klikněte na tlačítko aktualizace zařízení](./media/storsimple-8000-install-update5-via-portal/update3.png)

3. <span data-ttu-id="2c05e-111">Doporučujeme, abyste si před instalací aktualizace na zařízení prošli poznámky k verzi.</span><span class="sxs-lookup"><span data-stu-id="2c05e-111">We recommend that you review the release notes before you apply an update on your device.</span></span> <span data-ttu-id="2c05e-112">Chcete-li použít aktualizace, klikněte na tlačítko **nainstalovat aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="2c05e-112">To apply updates, click **Install updates**.</span></span> <span data-ttu-id="2c05e-113">V **potvrďte pravidelné aktualizace** okno, přečtěte si požadavky pro provedení úkolu před použití aktualizací.</span><span class="sxs-lookup"><span data-stu-id="2c05e-113">In the **Confirm regular updates** blade, review the prerequisites to complete before you apply updates.</span></span> <span data-ttu-id="2c05e-114">Zaškrtněte políčko s informací, že budete chtít aktualizovat zařízení a potom klikněte na o **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="2c05e-114">Select the checkbox to indicate that you are ready to update the device and then click **Install**.</span></span>

    ![Klikněte na tlačítko aktualizace zařízení](./media/storsimple-8000-install-update5-via-portal/update4.png)

6. <span data-ttu-id="2c05e-116">Spustí se sada kontrol požadavků.</span><span class="sxs-lookup"><span data-stu-id="2c05e-116">A set of prerequisite checks starts.</span></span> <span data-ttu-id="2c05e-117">Mezi tyto kontroly patří:</span><span class="sxs-lookup"><span data-stu-id="2c05e-117">These checks include:</span></span>
   
   * <span data-ttu-id="2c05e-118">**Kontroly stavu kontrolerů** pro ověření, že oba kontrolery zařízení jsou v pořádku a online.</span><span class="sxs-lookup"><span data-stu-id="2c05e-118">**Controller health checks** to verify that both the device controllers are healthy and online.</span></span>
   * <span data-ttu-id="2c05e-119">**Kontroly stavu hardwarových komponent** pro ověření, že všechny hardwarové komponenty v zařízení StorSimple jsou v pořádku.</span><span class="sxs-lookup"><span data-stu-id="2c05e-119">**Hardware component health checks** to verify that all the hardware components on your StorSimple device are healthy.</span></span>
   * <span data-ttu-id="2c05e-120">**Kontroly rozhraní DATA 0** pro ověření, že je na zařízení povolené rozhraní DATA 0.</span><span class="sxs-lookup"><span data-stu-id="2c05e-120">**DATA 0 checks** to verify that DATA 0 is enabled on your device.</span></span> <span data-ttu-id="2c05e-121">Pokud toto rozhraní není povolené, musíte jej povolit a poté to zkusit znovu.</span><span class="sxs-lookup"><span data-stu-id="2c05e-121">If this interface is not enabled, you must enable it and then retry.</span></span>

    <span data-ttu-id="2c05e-122">Aktualizace je stažen a nainstalován pouze v případě, že jsou všechny kontroly úspěšně dokončeny.</span><span class="sxs-lookup"><span data-stu-id="2c05e-122">The update is downloaded and installed only if all the checks are successfully completed.</span></span> <span data-ttu-id="2c05e-123">Na probíhání kontrol budete upozorněni.</span><span class="sxs-lookup"><span data-stu-id="2c05e-123">You are notified when the checks are in progress.</span></span> <span data-ttu-id="2c05e-124">Pokud prechecks selže, pak vám bude poskytnuta s příčiny chyby.</span><span class="sxs-lookup"><span data-stu-id="2c05e-124">If the prechecks fail, then you will be provided with the reasons for failure.</span></span> <span data-ttu-id="2c05e-125">Vyřešit tyto problémy a poté operaci opakujte.</span><span class="sxs-lookup"><span data-stu-id="2c05e-125">Address those issues and then retry the operation.</span></span> <span data-ttu-id="2c05e-126">Pokud si s těmito problémy neporadíte sami, bude muset kontaktovat podporu Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="2c05e-126">You may need to contact Microsoft Support if you cannot address these issues by yourself.</span></span>

7. <span data-ttu-id="2c05e-127">Po úspěšném dokončení prechecks se vytvoří úloha aktualizace.</span><span class="sxs-lookup"><span data-stu-id="2c05e-127">After the prechecks are successfully completed, an update job is created.</span></span> <span data-ttu-id="2c05e-128">Po úspěšném vytvoření úlohy aktualizace se zobrazí upozornění.</span><span class="sxs-lookup"><span data-stu-id="2c05e-128">You are notified when the update job is successfully created.</span></span>
   
    ![Vytvoření úlohy aktualizace](./media/storsimple-8000-install-update5-via-portal/update6.png)
   
    <span data-ttu-id="2c05e-130">Aktualizace se potom nainstaluje na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="2c05e-130">The update is then applied on your device.</span></span>

9. <span data-ttu-id="2c05e-131">Dokončení aktualizace trvá několik hodin.</span><span class="sxs-lookup"><span data-stu-id="2c05e-131">The update takes a few hours to complete.</span></span> <span data-ttu-id="2c05e-132">Podrobnosti o úloze můžete kdykoli zobrazit výběrem úlohy aktualizace a kliknutím na **Podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="2c05e-132">Select the update job and click **Details** to view the details of the job at any time.</span></span>

    ![Vytvoření úlohy aktualizace](./media/storsimple-8000-install-update5-via-portal/update8.png)

     <span data-ttu-id="2c05e-134">Můžete také sledovat průběh úlohy aktualizace z **nastavení zařízení > úlohy**.</span><span class="sxs-lookup"><span data-stu-id="2c05e-134">You can also monitor the progress of the update job from **Device settings > Jobs**.</span></span> <span data-ttu-id="2c05e-135">Na **úlohy** okně uvidíte průběh aktualizace.</span><span class="sxs-lookup"><span data-stu-id="2c05e-135">On the **Jobs** blade, you can see the update progress.</span></span>

     ![Vytvoření úlohy aktualizace](./media/storsimple-8000-install-update5-via-portal/update7.png)

10. <span data-ttu-id="2c05e-137">Po dokončení úlohy, přejděte na **nastavení zařízení > aktualizace zařízení**.</span><span class="sxs-lookup"><span data-stu-id="2c05e-137">After the job is complete, navigate to the **Device settings > Device updates**.</span></span> <span data-ttu-id="2c05e-138">Nyní je třeba aktualizovat verze softwaru.</span><span class="sxs-lookup"><span data-stu-id="2c05e-138">The software version should now be updated.</span></span>

