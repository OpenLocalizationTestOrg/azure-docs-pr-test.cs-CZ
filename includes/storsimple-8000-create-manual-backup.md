
<!--author=alkohli last changed: 01/20/2017-->

#### <a name="to-create-a-manual-backup"></a><span data-ttu-id="15f30-101">Ruční vytvoření zálohy</span><span class="sxs-lookup"><span data-stu-id="15f30-101">To create a manual backup</span></span>

1. <span data-ttu-id="15f30-102">Přejděte do služby Správce zařízení StorSimple a klikněte na **Zařízení**.</span><span class="sxs-lookup"><span data-stu-id="15f30-102">Go to your StorSimple Device Manager service and then click **Devices**.</span></span> <span data-ttu-id="15f30-103">V tabulkovém výpisu zařízení vyberte své zařízení.</span><span class="sxs-lookup"><span data-stu-id="15f30-103">From the tabular listing of devices, select your device.</span></span> <span data-ttu-id="15f30-104">Přejděte do **Nastavení > Správa > Zásady zálohování**.</span><span class="sxs-lookup"><span data-stu-id="15f30-104">Go to **Settings > Manage > Backup policies**.</span></span>

2. <span data-ttu-id="15f30-105">Okno **Zásady zálohování** obsahuje výpis všech zásad zálohování v tabulkovém formátu, včetně zásad svazku, který chcete zálohovat.</span><span class="sxs-lookup"><span data-stu-id="15f30-105">The **Backup policies** blade lists all the backup policies in a tabular format, including the policy for the volume that you want to back up.</span></span> <span data-ttu-id="15f30-106">Vyberte zásadu přidruženou ke svazku, který chcete zálohovat, a kliknutím pravým tlačítkem myši vyvolejte místní nabídku.</span><span class="sxs-lookup"><span data-stu-id="15f30-106">Select the policy associated with the volume you want to back up and right-click to invoke the context menu.</span></span> <span data-ttu-id="15f30-107">Z rozevíracího seznamu vyberte **Zálohovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="15f30-107">From the dropdown list, select **Back up now**.</span></span>

    ![Ruční vytvoření zálohy](./media/storsimple-8000-create-manual-backup/createmanualbu1.png)

3. <span data-ttu-id="15f30-109">V okně **Zálohovat nyní** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="15f30-109">In the **Back up now** blade, do the following steps:</span></span>

    1. <span data-ttu-id="15f30-110">Z rozevíracího seznamu zvolte vhodný **Typ snímku**: **Místní** snímek nebo **Cloudový** snímek.</span><span class="sxs-lookup"><span data-stu-id="15f30-110">Choose the appropriate **Snapshot type** from the dropdown list: **Local** snapshot or **Cloud** snapshot.</span></span> <span data-ttu-id="15f30-111">Pokud chcete rychlejší zálohování a obnovení, vyberte místní snímek; cloudový snímek vyberte, pokud chcete větší odolnost dat.</span><span class="sxs-lookup"><span data-stu-id="15f30-111">Select local snapshot for fast backups or restores, and cloud snapshot for data resiliency.</span></span>

        ![Ruční vytvoření zálohy](./media/storsimple-8000-create-manual-backup/createmanualbu2.png)

    2. <span data-ttu-id="15f30-113">Kliknutím na **OK** spusťte úlohu pro vytvoření snímku.</span><span class="sxs-lookup"><span data-stu-id="15f30-113">Click **OK** to start a job to create a snapshot.</span></span> <span data-ttu-id="15f30-114">Po úspěšném vytvoření úlohy se v horní části stránky zobrazí oznámení.</span><span class="sxs-lookup"><span data-stu-id="15f30-114">You will see a notification at the top of the page after the job is successfully created.</span></span>

        ![Ruční vytvoření zálohy](./media/storsimple-8000-create-manual-backup/createmanualbu4.png)

    3. <span data-ttu-id="15f30-116">Pokud chcete úlohu monitorovat, klikněte na oznámení.</span><span class="sxs-lookup"><span data-stu-id="15f30-116">To monitor the job, click the notification.</span></span> <span data-ttu-id="15f30-117">Tím přejdete do okna **Úlohy**, kde můžete zobrazit průběh úlohy.</span><span class="sxs-lookup"><span data-stu-id="15f30-117">This takes you to the **Jobs** blade where you can view the job progress.</span></span>


5. <span data-ttu-id="15f30-118">Po dokončení úlohy zálohování přejděte na kartu **Backup catalog** (Katalog zálohování).</span><span class="sxs-lookup"><span data-stu-id="15f30-118">After the backup job is finished, go to the **Backup catalog** tab.</span></span>

6. <span data-ttu-id="15f30-119">Nastavte filtry na příslušné zařízení, zásady zálohování a časové rozmezí.</span><span class="sxs-lookup"><span data-stu-id="15f30-119">Set the filter selections to the appropriate device, backup policy, and time range.</span></span> <span data-ttu-id="15f30-120">Záloha by se měla zobrazit v seznamu sad záloh uvedených v katalogu.</span><span class="sxs-lookup"><span data-stu-id="15f30-120">The backup should appear in the list of backup sets that is displayed in the catalog.</span></span>

