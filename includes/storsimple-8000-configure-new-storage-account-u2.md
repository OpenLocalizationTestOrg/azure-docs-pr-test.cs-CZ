<!--author=alkohli last changed: 01/20/17-->


#### <a name="to-add-a-storage-account-credential-in-the-same-azure-subscription-as-the-storsimple-device-manager-service"></a><span data-ttu-id="f8e61-101">Přidání přihlašovacích údajů účtu úložiště ve stejném předplatném Azure, jako je služba Správce zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="f8e61-101">To add a storage account credential in the same Azure subscription as the StorSimple Device Manager service</span></span>

1. <span data-ttu-id="f8e61-102">Přejděte do služby Správce zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="f8e61-102">Go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="f8e61-103">V části **Konfigurace** klikněte na **Přihlašovací údaje účtu úložiště**.</span><span class="sxs-lookup"><span data-stu-id="f8e61-103">In the **Configuration** section, click **Storage account credentials**.</span></span>

    ![Přihlašovací údaje účtu úložiště](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct1.png)

2. <span data-ttu-id="f8e61-105">V okně **Přihlašovací údaje účtu úložiště** klikněte na **+ Přidat**.</span><span class="sxs-lookup"><span data-stu-id="f8e61-105">On the **Storage account credentials** blade, click **+ Add**.</span></span>

    ![Přidání přihlašovacích údajů účtu úložiště](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct2.png)

3. <span data-ttu-id="f8e61-107">V okně **Přidat přihlašovací údaje účtu úložiště** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f8e61-107">In the **Add a storage account credential** blade, do the following steps:</span></span>

    1. <span data-ttu-id="f8e61-108">Protože přidáváte přihlašovací údaje účtu úložiště ve stejném předplatném Azure, jako je vaše služba, zkontrolujte, že je vybrána možnost **Aktuální**.</span><span class="sxs-lookup"><span data-stu-id="f8e61-108">As you are adding a storage account credential in the same Azure subscription as your service, ensure that **Current** is selected.</span></span>

    2. <span data-ttu-id="f8e61-109">V rozevíracím seznamu **Účet úložiště** vyberte existující účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="f8e61-109">From the **storage account** dropdown list, select an existing storage account.</span></span>

    3. <span data-ttu-id="f8e61-110">V závislosti na vybraném účtu úložiště se zobrazí **umístění** (bude zobrazeno šedě a tady ho nelze změnit).</span><span class="sxs-lookup"><span data-stu-id="f8e61-110">Based on the storage account selected, the **location** will be displayed (grayed out and cannot be changed here).</span></span>

    4. <span data-ttu-id="f8e61-111">Výběrem možnosti **Povolit režim SSL** vytvořte zabezpečený kanál pro síťovou komunikaci mezi vaším zařízením a cloudem.</span><span class="sxs-lookup"><span data-stu-id="f8e61-111">Select **Enable SSL Mode** to create a secure channel for network communication between your device and the cloud.</span></span> <span data-ttu-id="f8e61-112">Výběr možnosti **Povolit protokol SSL** zrušte jenom v případě, že pracujete v privátním cloudu.</span><span class="sxs-lookup"><span data-stu-id="f8e61-112">Disable **Enable SSL** only if you are operating within a private cloud.</span></span>

        ![Okno přidání přihlašovacích údajů účtu úložiště](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct3.png)

    5. <span data-ttu-id="f8e61-114">Kliknutím na **Přidat** spusťte vytváření úlohy pro přihlašovací údaje účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f8e61-114">Click **Add** to start the job creation for the storage account credential.</span></span> <span data-ttu-id="f8e61-115">Po úspěšném vytvoření přihlašovacích údajů účtu úložiště se zobrazí oznámení.</span><span class="sxs-lookup"><span data-stu-id="f8e61-115">You will be notified after the storage account credential is successfully created.</span></span>

        ![Oznámení o úspěšném vytvoření přihlašovacích údajů účtu úložiště](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct5.png)

<span data-ttu-id="f8e61-117">Nově vytvořené přihlašovací údaje účtu úložiště se zobrazí v seznamu **Přihlašovací údaje účtu úložiště**.</span><span class="sxs-lookup"><span data-stu-id="f8e61-117">The newly created storage account credential will be displayed under the list of **Storage account credentials**.</span></span>

![Seznam přihlašovacích údajů účtu úložiště](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct6.png)

