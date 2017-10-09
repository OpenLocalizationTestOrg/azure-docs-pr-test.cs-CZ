<!--author=alkohli last changed: 01/12/17-->

### <a name="tootake-a-backup"></a><span data-ttu-id="8c466-101">tootake zálohu</span><span class="sxs-lookup"><span data-stu-id="8c466-101">tootake a backup</span></span>

1. <span data-ttu-id="8c466-102">Přejděte služby StorSimple Manager zařízení tooyour.</span><span class="sxs-lookup"><span data-stu-id="8c466-102">Go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="8c466-103">Hello tabulkové seznam všech zařízení, vyberte a klikněte na zařízení a potom klikněte na **všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="8c466-103">From hello tabular listing of devices, select and click your device and then click **All settings**.</span></span> <span data-ttu-id="8c466-104">V hello **nastavení** okně přejděte příliš**Nastavení > Správa > Zálohování zásad**.</span><span class="sxs-lookup"><span data-stu-id="8c466-104">In hello **Settings** blade, go too**Settings > Manage > Backup policy**.</span></span>

    ![Přidání zásady zálohování](./media/storsimple-8000-take-backup/step8takebu1.png)

2. <span data-ttu-id="8c466-106">V hello **zálohování zásad** okně klikněte na tlačítko **+ přidat zásadu**.</span><span class="sxs-lookup"><span data-stu-id="8c466-106">In hello **Backup policy** blade, click **+ Add policy**.</span></span>

    ![Přidání zásady zálohování](./media/storsimple-8000-take-backup/step8takebu2.png)

3. <span data-ttu-id="8c466-108">V hello **vytvořit zásady zálohování** okno, zadejte název, který obsahuje od 3 do 150 znaků pro zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="8c466-108">In hello **Create backup policy** blade, supply a name that contains between 3 and 150 characters for your backup policy.</span></span>

4. <span data-ttu-id="8c466-109">Vyberte toobe hello svazky zálohovat.</span><span class="sxs-lookup"><span data-stu-id="8c466-109">Select hello volumes toobe backed up.</span></span> <span data-ttu-id="8c466-110">Pokud vyberete více než jeden svazek, tyto svazky jsou seskupené společně toocreate zálohu konzistentní při selhání.</span><span class="sxs-lookup"><span data-stu-id="8c466-110">If you select more than one volume, these volumes are grouped together toocreate a crash-consistent backup.</span></span>

    ![Přidání zásady zálohování](./media/storsimple-8000-take-backup/step8takebu4.png)

5. <span data-ttu-id="8c466-112">V okně **Přidat první pravidlo**:</span><span class="sxs-lookup"><span data-stu-id="8c466-112">On **Add first schedule** blade:</span></span>

    1. <span data-ttu-id="8c466-113">Vyberte typ hello zálohy.</span><span class="sxs-lookup"><span data-stu-id="8c466-113">Select hello type of backup.</span></span> <span data-ttu-id="8c466-114">Pokud chcete rychlejší zálohování, vyberte **Místní** snímek.</span><span class="sxs-lookup"><span data-stu-id="8c466-114">For faster restores, select **Local** snapshot.</span></span> <span data-ttu-id="8c466-115">Pokud vám záleží na odolnosti dat, vyberte **Cloudový** snímek.</span><span class="sxs-lookup"><span data-stu-id="8c466-115">For data resiliency, select **Cloud** snapshot.</span></span>
    2. <span data-ttu-id="8c466-116">Zadejte četnost zálohování hello v minuty, hodiny, dny nebo týdny.</span><span class="sxs-lookup"><span data-stu-id="8c466-116">Specify hello backup frequency in minutes, hours, days, or weeks.</span></span>
    3. <span data-ttu-id="8c466-117">Vyberte dobu uchování.</span><span class="sxs-lookup"><span data-stu-id="8c466-117">Select a retention time.</span></span> <span data-ttu-id="8c466-118">Hello volby uchování závisí na četnosti zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="8c466-118">hello retention choices depend on hello backup frequency.</span></span> <span data-ttu-id="8c466-119">Například pro denní zásady uchovávání hello lze zadat v týdnech, kdežto uchování pro měsíční zásady v měsících.</span><span class="sxs-lookup"><span data-stu-id="8c466-119">For example, for a daily policy, hello retention can be specified in weeks, whereas retention for a monthly policy is in months.</span></span>
    4. <span data-ttu-id="8c466-120">Vyberte počáteční čas a datum zásady zálohování hello hello.</span><span class="sxs-lookup"><span data-stu-id="8c466-120">Select hello starting time and date for hello backup policy.</span></span>
    5. <span data-ttu-id="8c466-121">Klikněte na tlačítko **OK** zásady zálohování toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="8c466-121">Click **OK** toocreate hello backup policy.</span></span>

        ![Přidání zásady zálohování](./media/storsimple-8000-take-backup/step8takebu5.png) 

6. <span data-ttu-id="8c466-123">Klikněte na tlačítko **vytvořit** vytvoření zásady zálohování toostart hello.</span><span class="sxs-lookup"><span data-stu-id="8c466-123">Click **Create** toostart hello backup policy creation.</span></span> <span data-ttu-id="8c466-124">Budete upozorněni, když zásady zálohování hello je úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="8c466-124">You are notified when hello backup policy is successfully created.</span></span> <span data-ttu-id="8c466-125">je také aktualizovat Hello seznam zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="8c466-125">hello list of backup policies is also updated.</span></span>
      
      ![Přidání zásady zálohování](./media/storsimple-8000-take-backup/step8takebu9.png)
      
      <span data-ttu-id="8c466-127">Teď máte zásadu zálohování, která bude podle plánu vytvářet zálohy dat svazku.</span><span class="sxs-lookup"><span data-stu-id="8c466-127">You now have a backup policy that creates scheduled backups of your volume data.</span></span>




