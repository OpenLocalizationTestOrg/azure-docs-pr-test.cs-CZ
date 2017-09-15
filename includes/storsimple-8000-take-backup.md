<!--author=alkohli last changed: 01/12/17-->

### <a name="to-take-a-backup"></a><span data-ttu-id="9d1b9-101">Provedení zálohy</span><span class="sxs-lookup"><span data-stu-id="9d1b9-101">To take a backup</span></span>

1. <span data-ttu-id="9d1b9-102">Přejděte do služby Správce zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-102">Go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="9d1b9-103">V tabulkovém výpisu zařízení vyberte a klikněte na vaše zařízení a pak na **Všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-103">From the tabular listing of devices, select and click your device and then click **All settings**.</span></span> <span data-ttu-id="9d1b9-104">V okně **Nastavení** přejděte do **Nastavení > Správa > Zásady zálohování**.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-104">In the **Settings** blade, go to **Settings > Manage > Backup policy**.</span></span>

    ![Přidání zásady zálohování](./media/storsimple-8000-take-backup/step8takebu1.png)

2. <span data-ttu-id="9d1b9-106">V okně **Zásady zálohování** klikněte na **Přidat zásadu**.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-106">In the **Backup policy** blade, click **+ Add policy**.</span></span>

    ![Přidání zásady zálohování](./media/storsimple-8000-take-backup/step8takebu2.png)

3. <span data-ttu-id="9d1b9-108">V okně **Vytvořit zásadu zálohování** zadejte název zásady zálohování, který obsahuje 3 až 150 znaků.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-108">In the **Create backup policy** blade, supply a name that contains between 3 and 150 characters for your backup policy.</span></span>

4. <span data-ttu-id="9d1b9-109">Vyberte svazky, které chcete zálohovat.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-109">Select the volumes to be backed up.</span></span> <span data-ttu-id="9d1b9-110">Pokud vyberete víc než jeden svazek, tyto svazky se seskupí, aby byla vytvořená záloha konzistentní v případě selhání.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-110">If you select more than one volume, these volumes are grouped together to create a crash-consistent backup.</span></span>

    ![Přidání zásady zálohování](./media/storsimple-8000-take-backup/step8takebu4.png)

5. <span data-ttu-id="9d1b9-112">V okně **Přidat první pravidlo**:</span><span class="sxs-lookup"><span data-stu-id="9d1b9-112">On **Add first schedule** blade:</span></span>

    1. <span data-ttu-id="9d1b9-113">Vyberte typ zálohování.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-113">Select the type of backup.</span></span> <span data-ttu-id="9d1b9-114">Pokud chcete rychlejší zálohování, vyberte **Místní** snímek.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-114">For faster restores, select **Local** snapshot.</span></span> <span data-ttu-id="9d1b9-115">Pokud vám záleží na odolnosti dat, vyberte **Cloudový** snímek.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-115">For data resiliency, select **Cloud** snapshot.</span></span>
    2. <span data-ttu-id="9d1b9-116">Zadejte četnost zálohování v minutách, hodinách, dnech nebo týdnech.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-116">Specify the backup frequency in minutes, hours, days, or weeks.</span></span>
    3. <span data-ttu-id="9d1b9-117">Vyberte dobu uchování.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-117">Select a retention time.</span></span> <span data-ttu-id="9d1b9-118">Volby uchování závisí na požadované četnosti zálohování.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-118">The retention choices depend on the backup frequency.</span></span> <span data-ttu-id="9d1b9-119">Třeba v případě denní zásady se dá doba uchování zadat v týdnech, kdežto v případě měsíční zásady v měsících.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-119">For example, for a daily policy, the retention can be specified in weeks, whereas retention for a monthly policy is in months.</span></span>
    4. <span data-ttu-id="9d1b9-120">Vyberte počáteční čas a datum zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-120">Select the starting time and date for the backup policy.</span></span>
    5. <span data-ttu-id="9d1b9-121">Kliknutím na **OK** vytvořte zásadu zálohování.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-121">Click **OK** to create the backup policy.</span></span>

        ![Přidání zásady zálohování](./media/storsimple-8000-take-backup/step8takebu5.png) 

6. <span data-ttu-id="9d1b9-123">Kliknutím na **Vytvořit** spusťte vytváření zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-123">Click **Create** to start the backup policy creation.</span></span> <span data-ttu-id="9d1b9-124">Po úspěšném vytvoření zásady zálohování se zobrazí oznámení.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-124">You are notified when the backup policy is successfully created.</span></span> <span data-ttu-id="9d1b9-125">Zároveň se aktualizuje seznam zásad zálohování.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-125">The list of backup policies is also updated.</span></span>
      
      ![Přidání zásady zálohování](./media/storsimple-8000-take-backup/step8takebu9.png)
      
      <span data-ttu-id="9d1b9-127">Teď máte zásadu zálohování, která bude podle plánu vytvářet zálohy dat svazku.</span><span class="sxs-lookup"><span data-stu-id="9d1b9-127">You now have a backup policy that creates scheduled backups of your volume data.</span></span>




