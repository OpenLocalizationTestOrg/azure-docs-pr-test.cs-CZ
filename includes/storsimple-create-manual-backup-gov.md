<!--author=SharS last changed: 9/17/15-->

#### <a name="to-create-a-manual-backup"></a><span data-ttu-id="536e3-101">Ruční vytvoření zálohy</span><span class="sxs-lookup"><span data-stu-id="536e3-101">To create a manual backup</span></span>
1. <span data-ttu-id="536e3-102">Na stránce **Zařízení** přejděte na kartu **Zásady zálohování**.</span><span class="sxs-lookup"><span data-stu-id="536e3-102">On the **Devices** page, go to the **Backup Policies** tab.</span></span> <span data-ttu-id="536e3-103">Tato karta obsahuje všechny zásady zálohování v tabelárním formátu, včetně zásad svazku, který chcete zálohovat.</span><span class="sxs-lookup"><span data-stu-id="536e3-103">This tab lists all the backup policies in a tabular format, including the policy for the volume that you want to back up.</span></span>
2. <span data-ttu-id="536e3-104">Vyberte zásady kliknutím na libovolné místo v příslušném řádku, s výjimkou prvního sloupce.</span><span class="sxs-lookup"><span data-stu-id="536e3-104">Select the policy by clicking anywhere in the corresponding row except for the first column.</span></span> <span data-ttu-id="536e3-105">Ve spodní části stránky klikněte na **Take backup** (Zálohovat).</span><span class="sxs-lookup"><span data-stu-id="536e3-105">At the bottom of the page, click **Take backup**.</span></span> <span data-ttu-id="536e3-106">Tlačítko se rozbalí a zobrazí se možnosti zálohování: místní snímek nebo cloudový snímek.</span><span class="sxs-lookup"><span data-stu-id="536e3-106">The button will expand to show the backup options: local snapshot and cloud snapshot.</span></span> 
3. <span data-ttu-id="536e3-107">Po výběru některé z těchto možností se zobrazí výzva k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="536e3-107">When you choose either of these options, you will be prompted for confirmation.</span></span> <span data-ttu-id="536e3-108">Klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="536e3-108">Click **Yes**.</span></span> 
   
    ![Vytvoření ruční backup1](./media/storsimple-create-manual-backup-gov/HCS_CreateManualBackup1-gov-include.png)
   
    <span data-ttu-id="536e3-110">Tím se spustí úloha pro vytvoření snímku.</span><span class="sxs-lookup"><span data-stu-id="536e3-110">This will start a job to create a snapshot.</span></span> <span data-ttu-id="536e3-111">Po úspěšném vytvoření úlohy se na konci stránky zobrazí oznámení.</span><span class="sxs-lookup"><span data-stu-id="536e3-111">You will see a notification at the bottom of the page after the job is successfully created.</span></span>
4. <span data-ttu-id="536e3-112">Pokud chcete úlohu sledovat, v oznamovací oblasti (ve spodní části stránky) klikněte na **View Job** (Zobrazit úlohu).</span><span class="sxs-lookup"><span data-stu-id="536e3-112">To monitor the job, click **View Job** in the notification area (at the bottom of the page).</span></span> 
   
    ![Vytvoření ruční backup2](./media/storsimple-create-manual-backup-gov/HCS_CreateManualBackup2-gov-include.png)
5. <span data-ttu-id="536e3-114">Po dokončení úlohy zálohování přejděte na kartu **Backup catalog** (Katalog zálohování).</span><span class="sxs-lookup"><span data-stu-id="536e3-114">After the backup job is finished, go to the **Backup catalog** tab.</span></span>
6. <span data-ttu-id="536e3-115">Nastavte filtry na příslušné zařízení, zásady zálohování a časové rozmezí.</span><span class="sxs-lookup"><span data-stu-id="536e3-115">Set the filter selections to the appropriate device, backup policy, and time range.</span></span> <span data-ttu-id="536e3-116">Klikněte na ikonu zaškrtnutí</span><span class="sxs-lookup"><span data-stu-id="536e3-116">Click the check icon</span></span> ![ikona zaškrtnutí](./media/storsimple-create-manual-backup/HCS_CheckIcon-include.png) <span data-ttu-id="536e3-118">po nastavení filtrů.</span><span class="sxs-lookup"><span data-stu-id="536e3-118">after setting the filters.</span></span>
   
   <span data-ttu-id="536e3-119">Záloha by se měla zobrazit v seznamu sad záloh uvedených v katalogu.</span><span class="sxs-lookup"><span data-stu-id="536e3-119">The backup should appear in the list of backup sets that is displayed in the catalog.</span></span>
