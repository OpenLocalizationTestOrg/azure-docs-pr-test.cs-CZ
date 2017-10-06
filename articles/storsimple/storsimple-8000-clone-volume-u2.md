---
title: "aaaClone svazek na řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje hello klon různé typy a využití a vysvětluje, jak můžete pomocí zálohovacího skladu tooclone svazku na zařízení řady StorSimple 8000."
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
ms.openlocfilehash: 4f7e1f62f17c7b2bd72820a00a5ab87b7e192332
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-in-azure-portal-tooclone-a-volume"></a><span data-ttu-id="edaf9-103">Použít službu StorSimple Manager zařízení hello ve službě Azure portálu tooclone svazku</span><span class="sxs-lookup"><span data-stu-id="edaf9-103">Use hello StorSimple Device Manager service in Azure portal tooclone a volume</span></span>

## <a name="overview"></a><span data-ttu-id="edaf9-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="edaf9-104">Overview</span></span>

<span data-ttu-id="edaf9-105">Tento kurz popisuje, jak můžete pomocí zálohovacího skladu tooclone svazku prostřednictvím hello **katalog zálohování** okno.</span><span class="sxs-lookup"><span data-stu-id="edaf9-105">This tutorial describes how you can use a backup set tooclone an individual volume via hello **Backup catalog** blade.</span></span> <span data-ttu-id="edaf9-106">Také vysvětluje hello rozdíl mezi *přechodný* a *trvalé* provede klonování.</span><span class="sxs-lookup"><span data-stu-id="edaf9-106">It also explains hello difference between *transient* and *permanent* clones.</span></span> <span data-ttu-id="edaf9-107">Hello pokyny v tomto kurzu platí zařízení řady StorSimple 8000 hello tooall verzi Update 3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="edaf9-107">hello guidance in this tutorial applies tooall hello StorSimple 8000 series device running Update 3 or later.</span></span>

<span data-ttu-id="edaf9-108">Hello služby StorSimple Manager zařízení **katalog zálohování** zobrazuje všechny zálohovací sklady hello, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy.</span><span class="sxs-lookup"><span data-stu-id="edaf9-108">hello StorSimple Device Manager service **Backup catalog** blade displays all hello backup sets that are created when manual or automated backups are taken.</span></span> <span data-ttu-id="edaf9-109">Pak můžete vybrat svazek v tooclone zálohovacího skladu.</span><span class="sxs-lookup"><span data-stu-id="edaf9-109">You can then select a volume in a backup set tooclone.</span></span>

 ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/bucatalog.png)

## <a name="considerations-for-cloning-a-volume"></a><span data-ttu-id="edaf9-111">Důležité informace týkající se klonování svazku</span><span class="sxs-lookup"><span data-stu-id="edaf9-111">Considerations for cloning a volume</span></span>

<span data-ttu-id="edaf9-112">Vezměte v úvahu následující informace při klonování svazku hello.</span><span class="sxs-lookup"><span data-stu-id="edaf9-112">Consider hello following information when cloning a volume.</span></span>

- <span data-ttu-id="edaf9-113">Klon se chová v hello stejný způsob jako regulární svazek.</span><span class="sxs-lookup"><span data-stu-id="edaf9-113">A clone behaves in hello same way as a regular volume.</span></span> <span data-ttu-id="edaf9-114">Všechny operace, které je možné na svazku je k dispozici pro klonování hello.</span><span class="sxs-lookup"><span data-stu-id="edaf9-114">Any operation that is possible on a volume is available for hello clone.</span></span>

- <span data-ttu-id="edaf9-115">Monitorování a výchozí zálohování jsou automaticky zakázán na klonovaný svazku.</span><span class="sxs-lookup"><span data-stu-id="edaf9-115">Monitoring and default backup are automatically disabled on a cloned volume.</span></span> <span data-ttu-id="edaf9-116">Potřebovali byste tooconfigure svazek klonovaný pro žádné zálohy.</span><span class="sxs-lookup"><span data-stu-id="edaf9-116">You would need tooconfigure a cloned volume for any backups.</span></span>

- <span data-ttu-id="edaf9-117">Místně vázaný svazek je klonovat jako vrstvený svazek.</span><span class="sxs-lookup"><span data-stu-id="edaf9-117">A locally pinned volume is cloned as a tiered volume.</span></span> <span data-ttu-id="edaf9-118">Pokud třeba hello místně vázaný toobe klonovaný svazku, můžete převést hello klon tooa místně vázaný svazek po úspěšném dokončení operace klonování hello.</span><span class="sxs-lookup"><span data-stu-id="edaf9-118">If you need hello cloned volume toobe locally pinned, you can convert hello clone tooa locally pinned volume after hello clone operation is successfully completed.</span></span> <span data-ttu-id="edaf9-119">Pro informace o převodu tooa vrstvený svazek, který je místně připnutý svazku, přejděte příliš[změnit typ svazku hello](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).</span><span class="sxs-lookup"><span data-stu-id="edaf9-119">For information about converting a tiered volume tooa locally pinned volume, go too[Change hello volume type](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).</span></span>

- <span data-ttu-id="edaf9-120">Pokud se pokusíte tooconvert, který svazek klonovaný ze vrstvené toolocally připnutý okamžitě po klonování (Pokud je stále přechodný klon), převod hello selže s touto hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="edaf9-120">If you try tooconvert a cloned volume from tiered toolocally pinned immediately after cloning (when it is still a transient clone), hello conversion fails with hello following error message:</span></span>

    `Unable toomodify hello usage type for volume {0}. This can happen if hello volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry hello modify operation.`

    <span data-ttu-id="edaf9-121">Tato chyba byl přijat pouze v případě, že se klonování na tooa jiné zařízení.</span><span class="sxs-lookup"><span data-stu-id="edaf9-121">This error is received only if you are cloning on tooa different device.</span></span> <span data-ttu-id="edaf9-122">Můžete úspěšně převést toolocally svazku hello připnutý Pokud převedete hello přechodný klon tooa trvalé klonování.</span><span class="sxs-lookup"><span data-stu-id="edaf9-122">You can successfully convert hello volume toolocally pinned if you first convert hello transient clone tooa permanent clone.</span></span> <span data-ttu-id="edaf9-123">Pořízení snímku cloudu hello přechodný klon tooconvert ho tooa trvalé klonování.</span><span class="sxs-lookup"><span data-stu-id="edaf9-123">Take a cloud snapshot of hello transient clone tooconvert it tooa permanent clone.</span></span>

## <a name="create-a-clone-of-a-volume"></a><span data-ttu-id="edaf9-124">Vytvořit klon svazku</span><span class="sxs-lookup"><span data-stu-id="edaf9-124">Create a clone of a volume</span></span>

<span data-ttu-id="edaf9-125">Můžete vytvořit klon na hello stejného zařízení, jiná zařízení nebo i o zařízení cloudu pomocí místní nebo cloudové snímku.</span><span class="sxs-lookup"><span data-stu-id="edaf9-125">You can create a clone on hello same device, another device, or even a cloud appliance by using a local or cloud snapshot.</span></span>

<span data-ttu-id="edaf9-126">Hello následující postup popisuje, jak zálohovat toocreate klonu z hello katalogu.</span><span class="sxs-lookup"><span data-stu-id="edaf9-126">hello procedure below describes how toocreate a clone from hello backup catalog.</span></span>  <span data-ttu-id="edaf9-127">Alternativní metoda tooinitiate klonování je příliš toogo**svazky**, vyberte svazek, pak klikněte pravým tlačítkem na tooinvoke hello kontextovou nabídku a vyberte **klon**.</span><span class="sxs-lookup"><span data-stu-id="edaf9-127">An alternative method tooinitiate clone is toogo too**Volumes**, select a volume, then right-click tooinvoke hello context menu and select **Clone**.</span></span>

<span data-ttu-id="edaf9-128">Proveďte následující kroky toocreate klon svazku z katalogu zálohování hello hello.</span><span class="sxs-lookup"><span data-stu-id="edaf9-128">Perform hello following steps toocreate a clone of your volume from hello backup catalog.</span></span>

#### <a name="tooclone-a-volume"></a><span data-ttu-id="edaf9-129">tooclone svazku</span><span class="sxs-lookup"><span data-stu-id="edaf9-129">tooclone a volume</span></span>

1. <span data-ttu-id="edaf9-130">Přejděte služby StorSimple Manager zařízení tooyour a pak klikněte na tlačítko **katalog zálohování**.</span><span class="sxs-lookup"><span data-stu-id="edaf9-130">Go tooyour StorSimple Device Manager service and then click **Backup catalog**.</span></span>

2. <span data-ttu-id="edaf9-131">Vyberte zálohovací sklad následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="edaf9-131">Select a backup set as follows:</span></span>
   
   1. <span data-ttu-id="edaf9-132">Vyberte příslušné zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="edaf9-132">Select hello appropriate device.</span></span>
   2. <span data-ttu-id="edaf9-133">V rozevíracím seznamu hello výběru hello svazek nebo zálohování zásady zálohování hello chcete tooselect.</span><span class="sxs-lookup"><span data-stu-id="edaf9-133">In hello drop-down list, choose hello volume or backup policy for hello backup that you wish tooselect.</span></span>
   3. <span data-ttu-id="edaf9-134">Zadejte časové rozmezí hello.</span><span class="sxs-lookup"><span data-stu-id="edaf9-134">Specify hello time range.</span></span>
   4. <span data-ttu-id="edaf9-135">Klikněte na tlačítko **použít** tooexecute tento dotaz.</span><span class="sxs-lookup"><span data-stu-id="edaf9-135">Click **Apply** tooexecute this query.</span></span>

    <span data-ttu-id="edaf9-136">Hello zálohování přidružená k zásadě hello vybraný svazek nebo zálohování by se zobrazit v seznamu hello zálohovací sklady.</span><span class="sxs-lookup"><span data-stu-id="edaf9-136">hello backups associated with hello selected volume or backup policy should appear in hello list of backup sets.</span></span>
   
    ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/bucatalog.png)
     
3. <span data-ttu-id="edaf9-138">Rozbalte hello zálohovacího skladu tooview hello přidružené svazky.</span><span class="sxs-lookup"><span data-stu-id="edaf9-138">Expand hello backup set tooview hello associated volumes.</span></span> <span data-ttu-id="edaf9-139">Tyto svazky musí být převedeno do režimu offline v hostiteli hello a zařízení, než bude možné obnovit.</span><span class="sxs-lookup"><span data-stu-id="edaf9-139">These volumes must be taken offline on hello host and device before you can restore them.</span></span> <span data-ttu-id="edaf9-140">Získat přístup ke svazkům hello na hello **svazky** okno zařízení a pak hello postupujte podle kroků v [do offline režimu svazek](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline) tootake je v offline režimu.</span><span class="sxs-lookup"><span data-stu-id="edaf9-140">Access hello volumes on hello **Volumes** blade of your device, and then follow hello steps in [Take a volume offline](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline) tootake them offline.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="edaf9-141">Ujistěte se, že jste provedli hello svazky do offline režimu na hostiteli hello nejprve, před provedením hello svazky do režimu offline v zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="edaf9-141">Make sure that you have taken hello volumes offline on hello host first, before you take hello volumes offline on hello device.</span></span> <span data-ttu-id="edaf9-142">Pokud neprovedete offline hello svazky na hostiteli hello, může potenciálně vést k poškození toodata.</span><span class="sxs-lookup"><span data-stu-id="edaf9-142">If you do not take hello volumes offline on hello host, it could potentially lead toodata corruption.</span></span>
   
4. <span data-ttu-id="edaf9-143">Přejděte zpět toohello **katalog zálohování** a vyberte svazek v zálohovacím skladu.</span><span class="sxs-lookup"><span data-stu-id="edaf9-143">Navigate back toohello **Backup catalog** and select a volume in a backup set.</span></span> <span data-ttu-id="edaf9-144">Klikněte pravým tlačítkem a pak z hello kontextové nabídky, vyberte **klon**.</span><span class="sxs-lookup"><span data-stu-id="edaf9-144">Right-click and then from hello context menu, select **Clone**.</span></span>

   ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/clonevol3b.png) 

3. <span data-ttu-id="edaf9-146">V hello **klon** okně hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="edaf9-146">In hello **Clone** blade, do hello following steps:</span></span>
   
    1. <span data-ttu-id="edaf9-147">Určete cílové zařízení.</span><span class="sxs-lookup"><span data-stu-id="edaf9-147">Identify a target device.</span></span> <span data-ttu-id="edaf9-148">Toto je hello umístění, kde bude vytvořen hello klonování.</span><span class="sxs-lookup"><span data-stu-id="edaf9-148">This is hello location where hello clone will be created.</span></span> <span data-ttu-id="edaf9-149">Můžete vybrat hello stejné zařízení nebo zadejte jiné zařízení.</span><span class="sxs-lookup"><span data-stu-id="edaf9-149">You can choose hello same device or specify another device.</span></span>

      > [!NOTE]
      > <span data-ttu-id="edaf9-150">Ujistěte se, že požadovaná pro klon hello kapacita hello je nižší než hello kapacity, které jsou k dispozici na cílovém zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="edaf9-150">Make sure that hello capacity required for hello clone is lower than hello capacity available on hello target device.</span></span>
       
    2. <span data-ttu-id="edaf9-151">Zadejte název jedinečné svazku pro vaše klon.</span><span class="sxs-lookup"><span data-stu-id="edaf9-151">Specify a unique volume name for your clone.</span></span> <span data-ttu-id="edaf9-152">Hello název musí obsahovat 3 až 127 znaků.</span><span class="sxs-lookup"><span data-stu-id="edaf9-152">hello name must contain between 3 and 127 characters.</span></span>
      
        > [!NOTE]
        > <span data-ttu-id="edaf9-153">Hello **klonování svazku jako** pole bude **nastavování** i v případě, že se klonování místně vázaný svazek.</span><span class="sxs-lookup"><span data-stu-id="edaf9-153">hello **Clone Volume As** field will be **Tiered** even if you are cloning a locally pinned volume.</span></span> <span data-ttu-id="edaf9-154">Nelze změnit toto nastavení; ale pokud klonovaný svazku toobe místně vázaný také potřebovat hello, můžete převést hello klon tooa místně vázaný svazek po úspěšně vytvořit klon hello.</span><span class="sxs-lookup"><span data-stu-id="edaf9-154">You cannot change this setting; however, if you need hello cloned volume toobe locally pinned as well, you can convert hello clone tooa locally pinned volume after you successfully create hello clone.</span></span> <span data-ttu-id="edaf9-155">Pro informace o převodu tooa vrstvený svazek, který je místně připnutý svazku, přejděte příliš[změnit typ svazku hello](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).</span><span class="sxs-lookup"><span data-stu-id="edaf9-155">For information about converting a tiered volume tooa locally pinned volume, go too[Change hello volume type](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).</span></span>
          
    3. <span data-ttu-id="edaf9-156">V části **připojení hostitele**, zadejte záznam řízení přístupu (ACR) pro klon hello.</span><span class="sxs-lookup"><span data-stu-id="edaf9-156">Under **Connected hosts**, specify an access control record (ACR) for hello clone.</span></span> <span data-ttu-id="edaf9-157">Můžete přidat nové ACR nebo vyberte možnost ze seznamu existující hello.</span><span class="sxs-lookup"><span data-stu-id="edaf9-157">You can add a new ACR or choose from hello existing list.</span></span> <span data-ttu-id="edaf9-158">Hello ACR určí, které hostitele může přístup Tenhle klon.</span><span class="sxs-lookup"><span data-stu-id="edaf9-158">hello ACR will determine which hosts can access this clone.</span></span>
      
        ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/clonevol3a.png) 

    4. <span data-ttu-id="edaf9-160">Klikněte na tlačítko **klon** toocomplete hello operaci.</span><span class="sxs-lookup"><span data-stu-id="edaf9-160">Click **Clone** toocomplete hello operation.</span></span>

4. <span data-ttu-id="edaf9-161">Úloha klonování se zahájí a zobrazí se upozornění při klonování hello je úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="edaf9-161">A clone job is initiated and you are notified when hello clone is successfully created.</span></span> <span data-ttu-id="edaf9-162">Klikněte na úlohu oznámení hello nebo přejděte příliš**úlohy** okno toomonitor hello klon úlohy.</span><span class="sxs-lookup"><span data-stu-id="edaf9-162">Click hello job notification or go too**Jobs** blade toomonitor hello clone job.</span></span>

    ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/clonevol5.png)

7. <span data-ttu-id="edaf9-164">Po dokončení úlohy klon hello přejděte tooyour zařízení a pak klikněte na tlačítko **svazky**.</span><span class="sxs-lookup"><span data-stu-id="edaf9-164">After hello clone job is complete, go tooyour device and then click **Volumes**.</span></span> <span data-ttu-id="edaf9-165">V seznamu hello svazků, měli byste vidět hello klon, který byl právě vytvořen v hello stejné kontejneru svazků, který má hello zdrojový svazek.</span><span class="sxs-lookup"><span data-stu-id="edaf9-165">In hello list of volumes, you should see hello clone that was just created in hello same volume container that has hello source volume.</span></span>

    ![Zálohovací sklad seznamu](./media/storsimple-8000-clone-volume-u2/clonevol6.png)

<span data-ttu-id="edaf9-167">Klon, který je vytvořen tímto způsobem je přechodný klonu.</span><span class="sxs-lookup"><span data-stu-id="edaf9-167">A clone that is created this way is a transient clone.</span></span> <span data-ttu-id="edaf9-168">Další informace o typech klonování najdete v tématu [přechodný oproti trvalé klony](#transient-vs-permanent-clones).</span><span class="sxs-lookup"><span data-stu-id="edaf9-168">For more information about clone types, see [Transient vs. permanent clones](#transient-vs-permanent-clones).</span></span>


## <a name="transient-vs-permanent-clones"></a><span data-ttu-id="edaf9-169">Pouze dočasné a trvalé klony</span><span class="sxs-lookup"><span data-stu-id="edaf9-169">Transient vs. permanent clones</span></span>
<span data-ttu-id="edaf9-170">Přechodný klony vytvoří jenom v případě, že klonování tooanother zařízení.</span><span class="sxs-lookup"><span data-stu-id="edaf9-170">Transient clones are created only when you clone tooanother device.</span></span> <span data-ttu-id="edaf9-171">Může klonovat konkrétní svazku z jiného zařízení zálohovacího skladu tooa spravuje hello Správce zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="edaf9-171">You can clone a specific volume from a backup set tooa different device managed by hello StorSimple Device Manager.</span></span> <span data-ttu-id="edaf9-172">klon přechodný Hello obsahuje odkazy na toohello data v původním svazku hello a používá tento tooread dat a zápisu místně na cílovém zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="edaf9-172">hello transient clone has references toohello data in hello original volume and uses that data tooread and write locally on hello target device.</span></span>

<span data-ttu-id="edaf9-173">Po provedení cloudový snímek přechodný klonování je výsledný klon hello *trvalé* klonování.</span><span class="sxs-lookup"><span data-stu-id="edaf9-173">After you take a cloud snapshot of a transient clone, hello resulting clone is a *permanent* clone.</span></span> <span data-ttu-id="edaf9-174">Během tohoto procesu kopii dat hello se vytvoří v cloudu hello a hello toocopy času, které tato data je určena velikostí hello hello dat a hello Azure latenci (Toto je kopie Azure do Azure).</span><span class="sxs-lookup"><span data-stu-id="edaf9-174">During this process, a copy of hello data is created on hello cloud and hello time toocopy this data is determined by hello size of hello data and hello Azure latencies (this is an Azure-to-Azure copy).</span></span> <span data-ttu-id="edaf9-175">Tento proces může trvat tooweeks dnů.</span><span class="sxs-lookup"><span data-stu-id="edaf9-175">This process can take days tooweeks.</span></span> <span data-ttu-id="edaf9-176">klon přechodný Hello stane trvalé klonování a nemá žádné odkazy toohello původní svazek data, která byla naklonována ze.</span><span class="sxs-lookup"><span data-stu-id="edaf9-176">hello transient clone becomes a permanent clone and doesn’t have any references toohello original volume data that it was cloned from.</span></span>

## <a name="scenarios-for-transient-and-permanent-clones"></a><span data-ttu-id="edaf9-177">Scénáře pro přechodný a trvalé klony</span><span class="sxs-lookup"><span data-stu-id="edaf9-177">Scenarios for transient and permanent clones</span></span>
<span data-ttu-id="edaf9-178">Hello následující oddíly popisují příklad situacích, ve kterých může být použit klony přechodný a trvalé.</span><span class="sxs-lookup"><span data-stu-id="edaf9-178">hello following sections describe example situations in which transient and permanent clones can be used.</span></span>

### <a name="item-level-recovery-with-a-transient-clone"></a><span data-ttu-id="edaf9-179">Obnovení na úrovni položek s přechodný klon</span><span class="sxs-lookup"><span data-stu-id="edaf9-179">Item-level recovery with a transient clone</span></span>
<span data-ttu-id="edaf9-180">Je nutné toorecover jeden rok starý soubor s Microsoft PowerPoint.</span><span class="sxs-lookup"><span data-stu-id="edaf9-180">You need toorecover a one-year-old Microsoft PowerPoint presentation file.</span></span> <span data-ttu-id="edaf9-181">Váš správce IT identifikuje hello konkrétní zálohování z této doby a pak filtry hello svazku.</span><span class="sxs-lookup"><span data-stu-id="edaf9-181">Your IT administrator identifies hello specific backup from that time, and then filters hello volume.</span></span> <span data-ttu-id="edaf9-182">Správce Hello pak provede klonování svazku hello, vyhledá hello soubor, který hledáte a poskytuje ji tooyou.</span><span class="sxs-lookup"><span data-stu-id="edaf9-182">hello administrator then clones hello volume, locates hello file that you are looking for, and provides it tooyou.</span></span> <span data-ttu-id="edaf9-183">V tomto scénáři se používá přechodný klonu.</span><span class="sxs-lookup"><span data-stu-id="edaf9-183">In this scenario, a transient clone is used.</span></span>

### <a name="testing-in-hello-production-environment-with-a-permanent-clone"></a><span data-ttu-id="edaf9-184">Testování hello produkčního prostředí s trvalé klon</span><span class="sxs-lookup"><span data-stu-id="edaf9-184">Testing in hello production environment with a permanent clone</span></span>
<span data-ttu-id="edaf9-185">Je nutné tooverify chyby testování v produkčním prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="edaf9-185">You need tooverify a testing bug in hello production environment.</span></span> <span data-ttu-id="edaf9-186">Vytvořit klon svazku hello hello produkčního prostředí a pak proveďte cloudový snímek tento toocreate klonování svazku nezávislé klonovaný.</span><span class="sxs-lookup"><span data-stu-id="edaf9-186">You create a clone of hello volume in hello production environment and then take a cloud snapshot of this clone toocreate an independent cloned volume.</span></span> <span data-ttu-id="edaf9-187">V tomto scénáři se používá trvalá klonu.</span><span class="sxs-lookup"><span data-stu-id="edaf9-187">In this scenario, a permanent clone is used.</span></span>

## <a name="next-steps"></a><span data-ttu-id="edaf9-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="edaf9-188">Next steps</span></span>
* <span data-ttu-id="edaf9-189">Zjistěte, jak příliš[obnovit svazek StorSimple ze zálohovacího skladu](storsimple-8000-restore-from-backup-set-u2.md).</span><span class="sxs-lookup"><span data-stu-id="edaf9-189">Learn how too[restore a StorSimple volume from a backup set](storsimple-8000-restore-from-backup-set-u2.md).</span></span>
* <span data-ttu-id="edaf9-190">Zjistěte, jak příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="edaf9-190">Learn how too[use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

