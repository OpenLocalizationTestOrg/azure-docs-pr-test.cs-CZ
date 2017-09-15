<!--author=alkohli last changed: 02/10/17-->

#### <a name="to-add-a-storsimple-backup-policy"></a><span data-ttu-id="cd266-101">Přidání zásady zálohování StorSimple</span><span class="sxs-lookup"><span data-stu-id="cd266-101">To add a StorSimple backup policy</span></span>

1. <span data-ttu-id="cd266-102">Přejděte ke svému zařízení StorSimple a klikněte na **Zásady zálohování**.</span><span class="sxs-lookup"><span data-stu-id="cd266-102">Go to your StorSimple device and click **Backup policy**.</span></span>

2. <span data-ttu-id="cd266-103">V okně **Zásady zálohování** klikněte na panelu příkazů na **Přidat zásadu**.</span><span class="sxs-lookup"><span data-stu-id="cd266-103">In the **Backup policy** blade, click **+ Add policy** from the command bar.</span></span>
   
    ![Přidání zásady zálohování](./media/storsimple-8000-add-backup-policy-u2/addbupol1.png)

3. <span data-ttu-id="cd266-105">V okně **Vytvořit zásadu zálohování** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cd266-105">In the **Create backup policy** blade, do the following steps:</span></span>
   
   1. <span data-ttu-id="cd266-106">Pole **Vybrat zařízení** se automaticky vyplní podle zařízení, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="cd266-106">**Select device** is automatically populated based on the device you selected.</span></span>
   
   2. <span data-ttu-id="cd266-107">Zadejte **Název zásady** zálohování, který obsahuje 3 až 150 znaků.</span><span class="sxs-lookup"><span data-stu-id="cd266-107">Specify a backup **Policy name** that contains between 3 and 150 characters.</span></span> <span data-ttu-id="cd266-108">Po vytvoření už zásadu není možné přejmenovat.</span><span class="sxs-lookup"><span data-stu-id="cd266-108">Once the policy is created, you cannot rename the policy.</span></span>
       
   3. <span data-ttu-id="cd266-109">Pokud chcete k této zásadě zálohování přiřadit svazky, vyberte **Přidat svazky** a potom v tabulkovém výpisu svazků zaškrtnutím políček přiřaďte této zásadě zálohování jeden nebo několik svazků.</span><span class="sxs-lookup"><span data-stu-id="cd266-109">To assign volumes to this backup policy, select **Add volumes** and then from the tabular listing of volumes, click the check box(es) to assign one or more volumes to this backup policy.</span></span>

       ![Přidání zásady zálohování](./media/storsimple-8000-add-backup-policy-u2/addbupol2.png)

   4. <span data-ttu-id="cd266-111">Pokud chcete pro tuto zásadu zálohování nadefinovat plán, klikněte na **První plán** a potom upravte následující parametry:</span><span class="sxs-lookup"><span data-stu-id="cd266-111">To define a schedule for this backup policy, click **First schedule** and then modify the following parameters:</span></span>

       ![Přidání zásady zálohování](./media/storsimple-8000-add-backup-policy-u2/addbupol3.png)

       1. <span data-ttu-id="cd266-113">Jako **Typ snímku** vyberte **Cloud** nebo **Místní**.</span><span class="sxs-lookup"><span data-stu-id="cd266-113">For **Snapshot type**, select **Cloud** or **Local**.</span></span>

       2. <span data-ttu-id="cd266-114">Určete frekvenci záloh (zadejte počet a vyberte z rozevíracího seznamu buď **Dny**, nebo **Týdny**).</span><span class="sxs-lookup"><span data-stu-id="cd266-114">Indicate the frequency of backups (specify a number and then choose **Days** or **Weeks** from the drop-down list.</span></span>

       3. <span data-ttu-id="cd266-115">Zadejte plán uchovávání.</span><span class="sxs-lookup"><span data-stu-id="cd266-115">Enter a retention schedule.</span></span>

       4. <span data-ttu-id="cd266-116">Zadejte čas a datum zahájení platnosti zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="cd266-116">Enter a time and date for the backup policy to begin.</span></span>

       5. <span data-ttu-id="cd266-117">Kliknutím na **OK** nadefinujte plán.</span><span class="sxs-lookup"><span data-stu-id="cd266-117">Click **OK** to define the schedule.</span></span>

   5. <span data-ttu-id="cd266-118">Kliknutím na **Vytvořit** vytvořte zásadu zálohování.</span><span class="sxs-lookup"><span data-stu-id="cd266-118">Click **Create** to create a backup policy.</span></span>

       ![Přidání zásady zálohování](./media/storsimple-8000-add-backup-policy-u2/addbupol4.png)
   
   6. <span data-ttu-id="cd266-120">Po vytvoření zásady zálohování se zobrazí oznámení.</span><span class="sxs-lookup"><span data-stu-id="cd266-120">You are notified when the backup policy is created.</span></span> <span data-ttu-id="cd266-121">Nově přidaná zásada se zobrazí v tabulkovém zobrazení v okně **Zásady zálohování**.</span><span class="sxs-lookup"><span data-stu-id="cd266-121">The newly added policy is displayed in the tabular view on the **Backup Policy** blade.</span></span>

       ![Přidání zásady zálohování](./media/storsimple-8000-add-backup-policy-u2/addbupol7.png)

