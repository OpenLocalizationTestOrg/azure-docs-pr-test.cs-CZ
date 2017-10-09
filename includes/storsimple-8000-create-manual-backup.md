
<!--author=alkohli last changed: 01/20/2017-->

#### <a name="toocreate-a-manual-backup"></a><span data-ttu-id="77cd8-101">toocreate ruční zálohy</span><span class="sxs-lookup"><span data-stu-id="77cd8-101">toocreate a manual backup</span></span>

1. <span data-ttu-id="77cd8-102">Přejděte služby StorSimple Manager zařízení tooyour a pak klikněte na tlačítko **zařízení**.</span><span class="sxs-lookup"><span data-stu-id="77cd8-102">Go tooyour StorSimple Device Manager service and then click **Devices**.</span></span> <span data-ttu-id="77cd8-103">Hello tabulkové seznam zařízení, vyberte zařízení.</span><span class="sxs-lookup"><span data-stu-id="77cd8-103">From hello tabular listing of devices, select your device.</span></span> <span data-ttu-id="77cd8-104">Přejděte příliš**Nastavení > Správa > zásady zálohování**.</span><span class="sxs-lookup"><span data-stu-id="77cd8-104">Go too**Settings > Manage > Backup policies**.</span></span>

2. <span data-ttu-id="77cd8-105">Hello **zásady zálohování** okno obsahuje seznam všech zásad zálohování hello v tabulkovém formátu, včetně hello zásady pro hello svazek, který chcete tooback nahoru.</span><span class="sxs-lookup"><span data-stu-id="77cd8-105">hello **Backup policies** blade lists all hello backup policies in a tabular format, including hello policy for hello volume that you want tooback up.</span></span> <span data-ttu-id="77cd8-106">Vyberte zásady hello přidružené hello svazek, který chcete tooback nahoru a klikněte pravým tlačítkem na tooinvoke hello kontextové nabídky.</span><span class="sxs-lookup"><span data-stu-id="77cd8-106">Select hello policy associated with hello volume you want tooback up and right-click tooinvoke hello context menu.</span></span> <span data-ttu-id="77cd8-107">Hello rozevíracího seznamu vyberte **zálohovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="77cd8-107">From hello dropdown list, select **Back up now**.</span></span>

    ![Ruční vytvoření zálohy](./media/storsimple-8000-create-manual-backup/createmanualbu1.png)

3. <span data-ttu-id="77cd8-109">V hello **zálohovat nyní** okně hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="77cd8-109">In hello **Back up now** blade, do hello following steps:</span></span>

    1. <span data-ttu-id="77cd8-110">Zvolte odpovídající hello **snímku typ** z rozevíracího seznamu hello: **místní** snímku nebo **cloudu** snímku.</span><span class="sxs-lookup"><span data-stu-id="77cd8-110">Choose hello appropriate **Snapshot type** from hello dropdown list: **Local** snapshot or **Cloud** snapshot.</span></span> <span data-ttu-id="77cd8-111">Pokud chcete rychlejší zálohování a obnovení, vyberte místní snímek; cloudový snímek vyberte, pokud chcete větší odolnost dat.</span><span class="sxs-lookup"><span data-stu-id="77cd8-111">Select local snapshot for fast backups or restores, and cloud snapshot for data resiliency.</span></span>

        ![Ruční vytvoření zálohy](./media/storsimple-8000-create-manual-backup/createmanualbu2.png)

    2. <span data-ttu-id="77cd8-113">Klikněte na tlačítko **OK** toostart toocreate úlohu snímku.</span><span class="sxs-lookup"><span data-stu-id="77cd8-113">Click **OK** toostart a job toocreate a snapshot.</span></span> <span data-ttu-id="77cd8-114">Po úspěšném vytvoření úlohy hello se uvidíte oznámení hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="77cd8-114">You will see a notification at hello top of hello page after hello job is successfully created.</span></span>

        ![Ruční vytvoření zálohy](./media/storsimple-8000-create-manual-backup/createmanualbu4.png)

    3. <span data-ttu-id="77cd8-116">toomonitor hello úlohy, klikněte na tlačítko hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="77cd8-116">toomonitor hello job, click hello notification.</span></span> <span data-ttu-id="77cd8-117">Tím přejdete toohello **úlohy** okno, kde můžete zobrazit průběh úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="77cd8-117">This takes you toohello **Jobs** blade where you can view hello job progress.</span></span>


5. <span data-ttu-id="77cd8-118">Po dokončení úlohy zálohování hello přejděte toohello **katalog zálohování** kartě.</span><span class="sxs-lookup"><span data-stu-id="77cd8-118">After hello backup job is finished, go toohello **Backup catalog** tab.</span></span>

6. <span data-ttu-id="77cd8-119">Nastavte hello filtru výběr toohello příslušné zařízení, zásady zálohování a časové rozmezí.</span><span class="sxs-lookup"><span data-stu-id="77cd8-119">Set hello filter selections toohello appropriate device, backup policy, and time range.</span></span> <span data-ttu-id="77cd8-120">Hello zálohování by se zobrazit v seznamu hello sad záloh, který se zobrazí v katalogu hello.</span><span class="sxs-lookup"><span data-stu-id="77cd8-120">hello backup should appear in hello list of backup sets that is displayed in hello catalog.</span></span>

