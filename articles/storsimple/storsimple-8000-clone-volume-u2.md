---
title: "Klonování svazku na řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje různé klon typy a využití a vysvětluje, jak můžete pomocí zálohovacího skladu klonování svazku na zařízení řady StorSimple 8000."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/26/2017
ms.author: alkohli
ms.openlocfilehash: 70c85bcb2c26d2ad3d0515d24e028f84495634c0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-the-storsimple-device-manager-service-in-azure-portal-to-clone-a-volume"></a><span data-ttu-id="b9807-103">Použít službu StorSimple Manager zařízení na portálu Azure ke klonování svazku</span><span class="sxs-lookup"><span data-stu-id="b9807-103">Use the StorSimple Device Manager service in Azure portal to clone a volume</span></span>

## <a name="overview"></a><span data-ttu-id="b9807-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b9807-104">Overview</span></span>

<span data-ttu-id="b9807-105">Tento kurz popisuje, jak můžete pomocí zálohovacího skladu klonování svazku pomocí **katalog zálohování** okno.</span><span class="sxs-lookup"><span data-stu-id="b9807-105">This tutorial describes how you can use a backup set to clone an individual volume via the **Backup catalog** blade.</span></span> <span data-ttu-id="b9807-106">Také vysvětluje rozdíly mezi *přechodný* a *trvalé* provede klonování.</span><span class="sxs-lookup"><span data-stu-id="b9807-106">It also explains the difference between *transient* and *permanent* clones.</span></span> <span data-ttu-id="b9807-107">Pokyny v tomto kurzu platí pro všechna zařízení řady StorSimple 8000 verzi Update 3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b9807-107">The guidance in this tutorial applies to all the StorSimple 8000 series device running Update 3 or later.</span></span>

<span data-ttu-id="b9807-108">Služby StorSimple Manager zařízení **katalog zálohování** zobrazuje všechny zálohovací sklady, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy.</span><span class="sxs-lookup"><span data-stu-id="b9807-108">The StorSimple Device Manager service **Backup catalog** blade displays all the backup sets that are created when manual or automated backups are taken.</span></span> <span data-ttu-id="b9807-109">Pak můžete vybrat svazek v zálohovacího skladu klonovat.</span><span class="sxs-lookup"><span data-stu-id="b9807-109">You can then select a volume in a backup set to clone.</span></span>

 ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/bucatalog.png)

## <a name="considerations-for-cloning-a-volume"></a><span data-ttu-id="b9807-111">Důležité informace týkající se klonování svazku</span><span class="sxs-lookup"><span data-stu-id="b9807-111">Considerations for cloning a volume</span></span>

<span data-ttu-id="b9807-112">Při klonování svazku, zvažte následující informace.</span><span class="sxs-lookup"><span data-stu-id="b9807-112">Consider the following information when cloning a volume.</span></span>

- <span data-ttu-id="b9807-113">Klon se chová stejně jako regulární svazek.</span><span class="sxs-lookup"><span data-stu-id="b9807-113">A clone behaves in the same way as a regular volume.</span></span> <span data-ttu-id="b9807-114">Všechny operace, které je možné na svazku je k dispozici pro klonování.</span><span class="sxs-lookup"><span data-stu-id="b9807-114">Any operation that is possible on a volume is available for the clone.</span></span>

- <span data-ttu-id="b9807-115">Monitorování a výchozí zálohování jsou automaticky zakázán na klonovaný svazku.</span><span class="sxs-lookup"><span data-stu-id="b9807-115">Monitoring and default backup are automatically disabled on a cloned volume.</span></span> <span data-ttu-id="b9807-116">Musíte nakonfigurovat svazek klonovaný pro žádné zálohy.</span><span class="sxs-lookup"><span data-stu-id="b9807-116">You would need to configure a cloned volume for any backups.</span></span>

- <span data-ttu-id="b9807-117">Místně vázaný svazek je klonovat jako vrstvený svazek.</span><span class="sxs-lookup"><span data-stu-id="b9807-117">A locally pinned volume is cloned as a tiered volume.</span></span> <span data-ttu-id="b9807-118">Pokud potřebujete klonovaný být místně vázaný svazek, můžete klonu převést místně vázaný svazek po úspěšném dokončení operace klonování.</span><span class="sxs-lookup"><span data-stu-id="b9807-118">If you need the cloned volume to be locally pinned, you can convert the clone to a locally pinned volume after the clone operation is successfully completed.</span></span> <span data-ttu-id="b9807-119">Informace o převodu vrstvený svazek na místně vázaný svazek, přejděte na [změnit typ svazku](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).</span><span class="sxs-lookup"><span data-stu-id="b9807-119">For information about converting a tiered volume to a locally pinned volume, go to [Change the volume type](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).</span></span>

- <span data-ttu-id="b9807-120">Pokud se pokusíte převést svazek klonovaný ze zřízeny vrstvené místně vázaný okamžitě po klonování (Pokud je stále přechodný klon), převod selže s touto se následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="b9807-120">If you try to convert a cloned volume from tiered to locally pinned immediately after cloning (when it is still a transient clone), the conversion fails with the following error message:</span></span>

    `Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.`

    <span data-ttu-id="b9807-121">Tato chyba byl přijat pouze v případě, že se klonování na jiném zařízení.</span><span class="sxs-lookup"><span data-stu-id="b9807-121">This error is received only if you are cloning on to a different device.</span></span> <span data-ttu-id="b9807-122">Pokud převedete přechodný klonu nejdřív na trvalé klon místně vázaný svazek můžete úspěšně převést.</span><span class="sxs-lookup"><span data-stu-id="b9807-122">You can successfully convert the volume to locally pinned if you first convert the transient clone to a permanent clone.</span></span> <span data-ttu-id="b9807-123">Pořízení snímku cloudu přechodný klon ho převést na trvalé klonu.</span><span class="sxs-lookup"><span data-stu-id="b9807-123">Take a cloud snapshot of the transient clone to convert it to a permanent clone.</span></span>

## <a name="create-a-clone-of-a-volume"></a><span data-ttu-id="b9807-124">Vytvořit klon svazku</span><span class="sxs-lookup"><span data-stu-id="b9807-124">Create a clone of a volume</span></span>

<span data-ttu-id="b9807-125">Můžete vytvořit klon na stejné zařízení, jiná zařízení nebo i o zařízení cloudu pomocí místní nebo cloudový snímek.</span><span class="sxs-lookup"><span data-stu-id="b9807-125">You can create a clone on the same device, another device, or even a cloud appliance by using a local or cloud snapshot.</span></span>

<span data-ttu-id="b9807-126">Následující postup popisuje vytvoření klonu z katalogu služby zálohování.</span><span class="sxs-lookup"><span data-stu-id="b9807-126">The procedure below describes how to create a clone from the backup catalog.</span></span>  <span data-ttu-id="b9807-127">Alternativní metoda pro zahájení klonování je přejít na **svazky**, vyberte svazek, a pak klikněte pravým tlačítkem myši a vyvolání v místní nabídce vyberte **klon**.</span><span class="sxs-lookup"><span data-stu-id="b9807-127">An alternative method to initiate clone is to go to **Volumes**, select a volume, then right-click to invoke the context menu and select **Clone**.</span></span>

<span data-ttu-id="b9807-128">Proveďte následující kroky k vytvoření klonu svazku z katalogu služby zálohování.</span><span class="sxs-lookup"><span data-stu-id="b9807-128">Perform the following steps to create a clone of your volume from the backup catalog.</span></span>

#### <a name="to-clone-a-volume"></a><span data-ttu-id="b9807-129">Klonování svazku</span><span class="sxs-lookup"><span data-stu-id="b9807-129">To clone a volume</span></span>

1. <span data-ttu-id="b9807-130">Přejděte do služby StorSimple Manager zařízení a pak klikněte na tlačítko **katalog zálohování**.</span><span class="sxs-lookup"><span data-stu-id="b9807-130">Go to your StorSimple Device Manager service and then click **Backup catalog**.</span></span>

2. <span data-ttu-id="b9807-131">Vyberte zálohovací sklad následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b9807-131">Select a backup set as follows:</span></span>
   
   1. <span data-ttu-id="b9807-132">Vyberte příslušné zařízení.</span><span class="sxs-lookup"><span data-stu-id="b9807-132">Select the appropriate device.</span></span>
   2. <span data-ttu-id="b9807-133">V rozevíracím seznamu vyberte svazek nebo zálohování zásady pro zálohu, kterou chcete vybrat.</span><span class="sxs-lookup"><span data-stu-id="b9807-133">In the drop-down list, choose the volume or backup policy for the backup that you wish to select.</span></span>
   3. <span data-ttu-id="b9807-134">Zadejte časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="b9807-134">Specify the time range.</span></span>
   4. <span data-ttu-id="b9807-135">Klikněte na tlačítko **použít** ke spuštění tohoto dotazu.</span><span class="sxs-lookup"><span data-stu-id="b9807-135">Click **Apply** to execute this query.</span></span>

    <span data-ttu-id="b9807-136">Zálohování přidružené k vybranému svazku nebo zásady zálohování by se měla objevit v seznamu sad záloh.</span><span class="sxs-lookup"><span data-stu-id="b9807-136">The backups associated with the selected volume or backup policy should appear in the list of backup sets.</span></span>
   
    ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/bucatalog.png)
     
3. <span data-ttu-id="b9807-138">Rozbalte zálohovací sklad zobrazíte přidružené svazky.</span><span class="sxs-lookup"><span data-stu-id="b9807-138">Expand the backup set to view the associated volumes.</span></span> <span data-ttu-id="b9807-139">Tyto svazky musí být převedeno do režimu offline v hostiteli a zařízení, než bude možné obnovit.</span><span class="sxs-lookup"><span data-stu-id="b9807-139">These volumes must be taken offline on the host and device before you can restore them.</span></span> <span data-ttu-id="b9807-140">Přístup ke svazkům na **svazky** okno vašeho zařízení a pak postupujte podle kroků v [do offline režimu svazek](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline) je uvedení do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="b9807-140">Access the volumes on the **Volumes** blade of your device, and then follow the steps in [Take a volume offline](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline) to take them offline.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="b9807-141">Ujistěte se, že jste provedli svazky do offline režimu na hostiteli nejprve, před provedením svazky do režimu offline v zařízení.</span><span class="sxs-lookup"><span data-stu-id="b9807-141">Make sure that you have taken the volumes offline on the host first, before you take the volumes offline on the device.</span></span> <span data-ttu-id="b9807-142">Pokud neprovedete offline svazky na hostiteli, může potenciálně vést k poškození dat.</span><span class="sxs-lookup"><span data-stu-id="b9807-142">If you do not take the volumes offline on the host, it could potentially lead to data corruption.</span></span>
   
4. <span data-ttu-id="b9807-143">Přejděte zpět **katalog zálohování** a vyberte svazek v zálohovacím skladu.</span><span class="sxs-lookup"><span data-stu-id="b9807-143">Navigate back to the **Backup catalog** and select a volume in a backup set.</span></span> <span data-ttu-id="b9807-144">Klikněte pravým tlačítkem myši a potom v místní nabídce vyberte **klon**.</span><span class="sxs-lookup"><span data-stu-id="b9807-144">Right-click and then from the context menu, select **Clone**.</span></span>

   ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/clonevol3b.png) 

3. <span data-ttu-id="b9807-146">V **klon** okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b9807-146">In the **Clone** blade, do the following steps:</span></span>
   
    1. <span data-ttu-id="b9807-147">Určete cílové zařízení.</span><span class="sxs-lookup"><span data-stu-id="b9807-147">Identify a target device.</span></span> <span data-ttu-id="b9807-148">Toto je umístění, kde bude vytvořen klonu.</span><span class="sxs-lookup"><span data-stu-id="b9807-148">This is the location where the clone will be created.</span></span> <span data-ttu-id="b9807-149">Můžete vybrat stejné zařízení nebo zadejte jiné zařízení.</span><span class="sxs-lookup"><span data-stu-id="b9807-149">You can choose the same device or specify another device.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b9807-150">Ujistěte se, že kapacitu, vyžaduje se pro klonování je nižší než kapacity, které jsou k dispozici na cílovém zařízení.</span><span class="sxs-lookup"><span data-stu-id="b9807-150">Make sure that the capacity required for the clone is lower than the capacity available on the target device.</span></span>
       
    2. <span data-ttu-id="b9807-151">Zadejte název jedinečné svazku pro vaše klon.</span><span class="sxs-lookup"><span data-stu-id="b9807-151">Specify a unique volume name for your clone.</span></span> <span data-ttu-id="b9807-152">Název musí obsahovat 3 až 127 znaků.</span><span class="sxs-lookup"><span data-stu-id="b9807-152">The name must contain between 3 and 127 characters.</span></span>
      
        > [!NOTE]
        > <span data-ttu-id="b9807-153">**Klonování svazku jako** pole bude **nastavování** i v případě, že se klonování místně vázaný svazek.</span><span class="sxs-lookup"><span data-stu-id="b9807-153">The **Clone Volume As** field will be **Tiered** even if you are cloning a locally pinned volume.</span></span> <span data-ttu-id="b9807-154">Nelze změnit toto nastavení; ale pokud budete potřebovat klonovaný svazku, který má být místně vázaný i, můžete převést klonu pro místně vázaný svazek po úspěšném vytvoření klonu.</span><span class="sxs-lookup"><span data-stu-id="b9807-154">You cannot change this setting; however, if you need the cloned volume to be locally pinned as well, you can convert the clone to a locally pinned volume after you successfully create the clone.</span></span> <span data-ttu-id="b9807-155">Informace o převodu vrstvený svazek na místně vázaný svazek, přejděte na [změnit typ svazku](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).</span><span class="sxs-lookup"><span data-stu-id="b9807-155">For information about converting a tiered volume to a locally pinned volume, go to [Change the volume type](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).</span></span>
          
    3. <span data-ttu-id="b9807-156">V části **připojení hostitele**, zadejte záznam řízení přístupu (ACR) pro klonování.</span><span class="sxs-lookup"><span data-stu-id="b9807-156">Under **Connected hosts**, specify an access control record (ACR) for the clone.</span></span> <span data-ttu-id="b9807-157">Můžete přidat nové ACR nebo zvolte ze seznamu existující.</span><span class="sxs-lookup"><span data-stu-id="b9807-157">You can add a new ACR or choose from the existing list.</span></span> <span data-ttu-id="b9807-158">ACR určí, které hostitele může přístup Tenhle klon.</span><span class="sxs-lookup"><span data-stu-id="b9807-158">The ACR will determine which hosts can access this clone.</span></span>
      
        ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/clonevol3a.png) 

    4. <span data-ttu-id="b9807-160">Klikněte na tlačítko **klon** k dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="b9807-160">Click **Clone** to complete the operation.</span></span>

4. <span data-ttu-id="b9807-161">Úloha klonování se zahájí a zobrazí se upozornění při klonování se úspěšně vytvořil.</span><span class="sxs-lookup"><span data-stu-id="b9807-161">A clone job is initiated and you are notified when the clone is successfully created.</span></span> <span data-ttu-id="b9807-162">Kliknutím na úlohu oznámení nebo přejděte na **úlohy** okně můžete sledovat úlohu klonování.</span><span class="sxs-lookup"><span data-stu-id="b9807-162">Click the job notification or go to **Jobs** blade to monitor the clone job.</span></span>

    ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/clonevol5.png)

7. <span data-ttu-id="b9807-164">Po dokončení úlohy klon přejít na zařízení a potom klikněte na **svazky**.</span><span class="sxs-lookup"><span data-stu-id="b9807-164">After the clone job is complete, go to your device and then click **Volumes**.</span></span> <span data-ttu-id="b9807-165">V seznamu svazků měli byste vidět klon, který byl právě vytvořen ve stejném kontejneru svazku, který má zdrojový svazek.</span><span class="sxs-lookup"><span data-stu-id="b9807-165">In the list of volumes, you should see the clone that was just created in the same volume container that has the source volume.</span></span>

    ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/clonevol6.png)

<span data-ttu-id="b9807-167">Klon, který je vytvořen tímto způsobem je přechodný klonu.</span><span class="sxs-lookup"><span data-stu-id="b9807-167">A clone that is created this way is a transient clone.</span></span> <span data-ttu-id="b9807-168">Další informace o typech klonování najdete v tématu [přechodný oproti trvalé klony](#transient-vs-permanent-clones).</span><span class="sxs-lookup"><span data-stu-id="b9807-168">For more information about clone types, see [Transient vs. permanent clones](#transient-vs-permanent-clones).</span></span>


## <a name="transient-vs-permanent-clones"></a><span data-ttu-id="b9807-169">Pouze dočasné a trvalé klony</span><span class="sxs-lookup"><span data-stu-id="b9807-169">Transient vs. permanent clones</span></span>
<span data-ttu-id="b9807-170">Přechodný klony vytvoří jenom v případě, že se klonování na jiné zařízení.</span><span class="sxs-lookup"><span data-stu-id="b9807-170">Transient clones are created only when you clone to another device.</span></span> <span data-ttu-id="b9807-171">Může klonovat svazek konkrétní ze zálohovacího skladu na jiném zařízení spravovaných pomocí Správce zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="b9807-171">You can clone a specific volume from a backup set to a different device managed by the StorSimple Device Manager.</span></span> <span data-ttu-id="b9807-172">Přechodný klonu má odkazů na data v původním svazku a používá tato data ke čtení a zápisu místně na cílovém zařízení.</span><span class="sxs-lookup"><span data-stu-id="b9807-172">The transient clone has references to the data in the original volume and uses that data to read and write locally on the target device.</span></span>

<span data-ttu-id="b9807-173">Po provedení cloudový snímek přechodný klonování je výsledný klonování *trvalé* klonování.</span><span class="sxs-lookup"><span data-stu-id="b9807-173">After you take a cloud snapshot of a transient clone, the resulting clone is a *permanent* clone.</span></span> <span data-ttu-id="b9807-174">Během tohoto procesu se vytvoří kopie dat v cloudu a čas zkopírovat tato data je určena velikostí dat a Azure latenci (Toto je kopie Azure do Azure).</span><span class="sxs-lookup"><span data-stu-id="b9807-174">During this process, a copy of the data is created on the cloud and the time to copy this data is determined by the size of the data and the Azure latencies (this is an Azure-to-Azure copy).</span></span> <span data-ttu-id="b9807-175">Tento proces může trvat dnů, týdnů.</span><span class="sxs-lookup"><span data-stu-id="b9807-175">This process can take days to weeks.</span></span> <span data-ttu-id="b9807-176">Přechodný klon stane trvalé klonování a nemá všechny odkazy na původní data svazku, která byla naklonována ze.</span><span class="sxs-lookup"><span data-stu-id="b9807-176">The transient clone becomes a permanent clone and doesn’t have any references to the original volume data that it was cloned from.</span></span>

## <a name="scenarios-for-transient-and-permanent-clones"></a><span data-ttu-id="b9807-177">Scénáře pro přechodný a trvalé klony</span><span class="sxs-lookup"><span data-stu-id="b9807-177">Scenarios for transient and permanent clones</span></span>
<span data-ttu-id="b9807-178">Následující části popisují příklad situacích, ve kterých může být použit klony přechodný a trvalé.</span><span class="sxs-lookup"><span data-stu-id="b9807-178">The following sections describe example situations in which transient and permanent clones can be used.</span></span>

### <a name="item-level-recovery-with-a-transient-clone"></a><span data-ttu-id="b9807-179">Obnovení na úrovni položek s přechodný klon</span><span class="sxs-lookup"><span data-stu-id="b9807-179">Item-level recovery with a transient clone</span></span>
<span data-ttu-id="b9807-180">Budete muset obnovit soubor s jedním. roku starý Microsoft PowerPoint.</span><span class="sxs-lookup"><span data-stu-id="b9807-180">You need to recover a one-year-old Microsoft PowerPoint presentation file.</span></span> <span data-ttu-id="b9807-181">Váš správce IT identifikuje konkrétní zálohování z této doby a pak filtry svazku.</span><span class="sxs-lookup"><span data-stu-id="b9807-181">Your IT administrator identifies the specific backup from that time, and then filters the volume.</span></span> <span data-ttu-id="b9807-182">Správce pak provede klonování svazku, vyhledá soubor, který hledáte a poskytuje vám.</span><span class="sxs-lookup"><span data-stu-id="b9807-182">The administrator then clones the volume, locates the file that you are looking for, and provides it to you.</span></span> <span data-ttu-id="b9807-183">V tomto scénáři se používá přechodný klonu.</span><span class="sxs-lookup"><span data-stu-id="b9807-183">In this scenario, a transient clone is used.</span></span>

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a><span data-ttu-id="b9807-184">Testování v produkčním prostředí s trvalé klon</span><span class="sxs-lookup"><span data-stu-id="b9807-184">Testing in the production environment with a permanent clone</span></span>
<span data-ttu-id="b9807-185">Je třeba ověřit chyby testování v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b9807-185">You need to verify a testing bug in the production environment.</span></span> <span data-ttu-id="b9807-186">Vytvořit klon svazku v provozním prostředí a pak proveďte cloudový snímek Tenhle klon. k vytvoření klonovaného svazek nezávislé.</span><span class="sxs-lookup"><span data-stu-id="b9807-186">You create a clone of the volume in the production environment and then take a cloud snapshot of this clone to create an independent cloned volume.</span></span> <span data-ttu-id="b9807-187">V tomto scénáři se používá trvalá klonu.</span><span class="sxs-lookup"><span data-stu-id="b9807-187">In this scenario, a permanent clone is used.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9807-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b9807-188">Next steps</span></span>
* <span data-ttu-id="b9807-189">Zjistěte, jak [obnovit svazek StorSimple ze zálohovacího skladu](storsimple-8000-restore-from-backup-set-u2.md).</span><span class="sxs-lookup"><span data-stu-id="b9807-189">Learn how to [restore a StorSimple volume from a backup set](storsimple-8000-restore-from-backup-set-u2.md).</span></span>
* <span data-ttu-id="b9807-190">Zjistěte, jak [použít službu StorSimple Manager zařízení ke správě zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="b9807-190">Learn how to [use the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

