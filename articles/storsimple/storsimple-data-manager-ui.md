---
title: "aaaMicrosoft Azure StorSimple Data Manager uživatelského rozhraní | Microsoft Docs"
description: "Popisuje, jak toouse služby StorSimple Data Manager uživatelského rozhraní (soukromém náhledu)."
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: b0ee12b3e495400b54e48eb1a98c68b1af2e5f7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-using-hello-storsimple-data-manager-service-ui-private-preview"></a><span data-ttu-id="6403b-103">Spravovat pomocí služby StorSimple Manager dat hello uživatelského rozhraní (soukromém náhledu).</span><span class="sxs-lookup"><span data-stu-id="6403b-103">Manage using hello StorSimple Data Manager service UI (Private Preview)</span></span>

<span data-ttu-id="6403b-104">Tento článek vysvětluje, jak můžete použít hello transformaci dat tooperform uživatelského rozhraní správce StorSimple dat na data uložená na řadu zařízení StorSimple 8000 hello.</span><span class="sxs-lookup"><span data-stu-id="6403b-104">This article explains how you can use hello StorSimple Data Manager UI tooperform data transformation on data residing on hello StorSimple 8000 series devices.</span></span> <span data-ttu-id="6403b-105">Hello Transformovaná data můžete pak být spotřebovávána jinými službami Azure, například Azure Media Services, Azure HDInsight, Azure Machine Learning a Azure Search.</span><span class="sxs-lookup"><span data-stu-id="6403b-105">hello transformed data can then be consumed by other Azure services such as Azure Media Services, Azure HDInsight, Azure Machine Learning, and Azure Search.</span></span> 


## <a name="use-storsimple-data-transformation"></a><span data-ttu-id="6403b-106">Transformace dat StorSimple použít</span><span class="sxs-lookup"><span data-stu-id="6403b-106">Use StorSimple Data Transformation</span></span>

<span data-ttu-id="6403b-107">Hello Data Manager zařízení StorSimple je hello prostředků v rámci kterého transformaci dat se dá vytvořit instance.</span><span class="sxs-lookup"><span data-stu-id="6403b-107">hello StorSimple Data Manager is hello resource within which Data Transformation can be instantiated.</span></span> <span data-ttu-id="6403b-108">Hello transformaci dat služby umožňuje přesun dat z vašeho zařízení StorSimple místní zařízení tooblobs v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="6403b-108">hello Data Transformation service lets you move data from your StorSimple on-premises device tooblobs in Azure storage.</span></span> <span data-ttu-id="6403b-109">Proto v pracovním postupu potřebujete toospecify hello podrobnosti o StorSimple zařízení a hello dat, které chcete účet úložiště toohello toomove zájmu.</span><span class="sxs-lookup"><span data-stu-id="6403b-109">Hence, in workflow you need toospecify hello details about your StorSimple device and hello data of interest that you want toomove toohello storage account.</span></span>

### <a name="create-a-storsimple-data-manager-service"></a><span data-ttu-id="6403b-110">Vytvoření služby StorSimple Manager dat</span><span class="sxs-lookup"><span data-stu-id="6403b-110">Create a StorSimple Data Manager service</span></span>

<span data-ttu-id="6403b-111">Proveďte následující kroky toocreate služby StorSimple Manager dat hello.</span><span class="sxs-lookup"><span data-stu-id="6403b-111">Perform hello following steps toocreate a StorSimple Data Manager service.</span></span>

1. <span data-ttu-id="6403b-112">toocreate služby StorSimple Manager dat, přejděte příliš[https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)</span><span class="sxs-lookup"><span data-stu-id="6403b-112">toocreate a StorSimple Data Manager service, go too[https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)</span></span>

2. <span data-ttu-id="6403b-113">Klikněte na tlačítko hello  **+**  ikonu a vyhledávání pro StorSimple Data Manager.</span><span class="sxs-lookup"><span data-stu-id="6403b-113">Click hello **+** icon and search for StorSimple Data Manager.</span></span> <span data-ttu-id="6403b-114">Klikněte na tlačítko služby StorSimple Manager dat a pak klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6403b-114">Click your StorSimple Data Manager service and then click **Create**.</span></span>

3. <span data-ttu-id="6403b-115">Pokud je vaše předplatné povolená pro vytvoření této služby, uvidíte hello následující okno.</span><span class="sxs-lookup"><span data-stu-id="6403b-115">If your subscription is enabled for creating this service, you see hello following blade.</span></span>

    ![Vytvořte prostředek StorSimple Data správců](./media/storsimple-data-manager-ui/create-new-data-manager-service.png)

4. <span data-ttu-id="6403b-117">Zadejte vstupy hello a klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6403b-117">Enter hello inputs and click **Create**.</span></span> <span data-ttu-id="6403b-118">Hello zadané umístění musí být hello jeden zaštiťující účty úložiště a služby StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="6403b-118">hello specified location should be hello one that houses your storage accounts and your StorSimple Manager service.</span></span> <span data-ttu-id="6403b-119">V současné době jsou podporovány pouze oblasti západní USA a západní Evropa.</span><span class="sxs-lookup"><span data-stu-id="6403b-119">Currently, only West US and West Europe regions are supported.</span></span> <span data-ttu-id="6403b-120">Proto vaší služby StorSimple Manager, služba Data Manager a hello přidružené účtu úložiště musí být v oblastech hello předchozí podporována.</span><span class="sxs-lookup"><span data-stu-id="6403b-120">Hence, your StorSimple Manager service, Data Manager service, and hello associated storage account should all be in hello preceding supported regions.</span></span> <span data-ttu-id="6403b-121">Trvá o minutu toocreate hello služby.</span><span class="sxs-lookup"><span data-stu-id="6403b-121">It takes about a minute toocreate hello service.</span></span>

### <a name="create-a-data-transformation-job-definition"></a><span data-ttu-id="6403b-122">Vytvoření definice úlohy transformace dat</span><span class="sxs-lookup"><span data-stu-id="6403b-122">Create a data transformation job definition</span></span>

<span data-ttu-id="6403b-123">V rámci služby StorSimple Manager dat je nutné toocreate definice úlohy transformace data.</span><span class="sxs-lookup"><span data-stu-id="6403b-123">Within a StorSimple Data Manager service, you need toocreate a data transformation job definition.</span></span> <span data-ttu-id="6403b-124">Definice úlohy Určuje podrobnosti hello data, která vás zajímá Přesun do účtu úložiště v nativním formátu hello.</span><span class="sxs-lookup"><span data-stu-id="6403b-124">A job definition specifies details of hello data that you are interested in moving into a storage account in hello native format.</span></span> 

<span data-ttu-id="6403b-125">Proveďte následující kroky toocreate novou definici úlohy transformace dat hello.</span><span class="sxs-lookup"><span data-stu-id="6403b-125">Perform hello following steps toocreate a new data transformation job definition.</span></span>

1.  <span data-ttu-id="6403b-126">Přejděte toohello službu, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="6403b-126">Navigate toohello service that you created.</span></span> <span data-ttu-id="6403b-127">Klikněte na tlačítko **+ úlohy definice**.</span><span class="sxs-lookup"><span data-stu-id="6403b-127">Click **+ Job Definition**.</span></span>

    ![Klikněte na tlačítko + definice úlohy](./media/storsimple-data-manager-ui/click-add-job-definition.png)

2. <span data-ttu-id="6403b-129">Otevře okno Definice úlohy nové Hello.</span><span class="sxs-lookup"><span data-stu-id="6403b-129">hello new job definition blade opens up.</span></span> <span data-ttu-id="6403b-130">Pojmenujte svou definici úlohy a klikněte na tlačítko **zdroj**.</span><span class="sxs-lookup"><span data-stu-id="6403b-130">Give your job definition a name and click **Source**.</span></span> <span data-ttu-id="6403b-131">V hello **zdroj dat konfigurace** okno, zadejte hello podrobnosti o zařízení StorSimple a hello požadovaná data.</span><span class="sxs-lookup"><span data-stu-id="6403b-131">In hello **Configure data source** blade, specify hello details of your StorSimple device and hello data of interest.</span></span>

    ![Vytvořit definici úlohy](./media/storsimple-data-manager-ui//create-new-job-deifnition.png)

3. <span data-ttu-id="6403b-133">Vzhledem k tomu, že toto je nová služba Data Manager, jsou nakonfigurovány žádné datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="6403b-133">Since this is a new Data Manager service, no data repositories are configured.</span></span> <span data-ttu-id="6403b-134">Klikněte na tlačítko tooadd vaše StorSimple Manager jako úložiště dat, **přidat nový** v hello rozevírací úložiště dat a pak klikněte na **úložiště dat přidat**.</span><span class="sxs-lookup"><span data-stu-id="6403b-134">tooadd your StorSimple Manager as a data repository, click **Add new** in hello data repository dropdown and then click **Add Data Repository**.</span></span>

4. <span data-ttu-id="6403b-135">Zvolte **řady StorSimple 8000 Manager** jako úložiště hello zadejte a zadejte vlastnosti hello vaše **StorSimple Manager**.</span><span class="sxs-lookup"><span data-stu-id="6403b-135">Choose **StorSimple 8000 series Manager** as hello repository type and enter hello properties of your **StorSimple Manager**.</span></span> <span data-ttu-id="6403b-136">Pro hello **Id prostředku** pole, je třeba číslo hello tooenter před hello **:** v hello registračního klíče služby StorSimple manager.</span><span class="sxs-lookup"><span data-stu-id="6403b-136">For hello **Resource Id** field, you need tooenter hello number before hello **:** in hello registration key of your StorSimple manager.</span></span>

    ![Vytvoření zdroje dat](./media/storsimple-data-manager-ui/create-new-data-source.png)

5.  <span data-ttu-id="6403b-138">Klikněte na tlačítko **OK** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="6403b-138">Click **OK** when done.</span></span> <span data-ttu-id="6403b-139">To umožňuje ušetřit úložiště dat a tato StorSimple Manager lze opětovně použít v jiné definice úlohy bez opětovného zadávání tyto parametry.</span><span class="sxs-lookup"><span data-stu-id="6403b-139">This saves your data repository and this StorSimple Manager can be reused in other job definitions without entering these parameters again.</span></span> <span data-ttu-id="6403b-140">Jak dlouho trvá několik sekund, po kliknutí na tlačítko **OK** pro hello tooshow StorSimple Manager nahoru v rozevírací nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="6403b-140">It takes a few seconds after you click **OK** for hello StorSimple Manager tooshow up in hello dropdown.</span></span>

6.  <span data-ttu-id="6403b-141">V hello **zdroj dat konfigurace** okno, zadejte název zařízení hello a hello název svazku, která obsahuje vaše data týkající se.</span><span class="sxs-lookup"><span data-stu-id="6403b-141">In hello **Configure data source** blade, enter hello device name and hello volume name that has your data of interest.</span></span>

7.  <span data-ttu-id="6403b-142">V hello **filtru** pododdílu, zadejte hello kořenový adresář, který obsahuje vaše data týkající se (v tomto poli by měla začínat znakem `\`).</span><span class="sxs-lookup"><span data-stu-id="6403b-142">In hello **Filter** subsection, enter hello root directory that contains your data of interest (this field should start with a `\`).</span></span> <span data-ttu-id="6403b-143">Můžete také přidat všechny souboru filtry.</span><span class="sxs-lookup"><span data-stu-id="6403b-143">You can also add any file filters here.</span></span>

8.  <span data-ttu-id="6403b-144">Služba transformaci dat Hello funguje na hello data, která se instaluje se toohello Azure prostřednictvím snímky.</span><span class="sxs-lookup"><span data-stu-id="6403b-144">hello data transformation service works on hello data that is pushed up toohello Azure via snapshots.</span></span> <span data-ttu-id="6403b-145">Při spuštění této úlohy můžete zvolit tootake zálohu pokaždé, když tato úloha se spouští (toowork na nejnovější data) nebo toouse hello poslední existující zálohy v cloudu hello (Pokud pracujete na některé Archivovaná data).</span><span class="sxs-lookup"><span data-stu-id="6403b-145">When running this job, you can choose tootake a backup every time this job is run (toowork on latest data) or toouse hello last existing backup in hello cloud (if you are working on some archived data).</span></span>

    ![Podrobnosti nové zdroje dat](./media/storsimple-data-manager-ui/new-data-source-details.png)

9. <span data-ttu-id="6403b-147">V dalším kroku nastavení cílového hello potřebovat toobe nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="6403b-147">Next, hello Target settings need toobe configured.</span></span> <span data-ttu-id="6403b-148">Existují 2 typy podporované cíle – účty Azure Storage a účty služby Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="6403b-148">There are 2 types of supported targets – Azure Storage accounts and Azure Media Services accounts.</span></span> <span data-ttu-id="6403b-149">Vyberte účty úložiště tooput soubory do objektů BLOB v daném účtu.</span><span class="sxs-lookup"><span data-stu-id="6403b-149">Choose storage accounts tooput files into blobs in that account.</span></span> <span data-ttu-id="6403b-150">Výběr účtu media services tooput souborů do prostředky v daném účtu.</span><span class="sxs-lookup"><span data-stu-id="6403b-150">Choose media services account tooput files into assets in that account.</span></span> <span data-ttu-id="6403b-151">Znovu potřebujeme tooadd úložiště.</span><span class="sxs-lookup"><span data-stu-id="6403b-151">Again, we need tooadd a repository.</span></span> <span data-ttu-id="6403b-152">V rozevírací nabídce hello, vyberte **přidat nový** a potom **nakonfigurovat nastavení**.</span><span class="sxs-lookup"><span data-stu-id="6403b-152">In hello dropdown, select **Add new** and then **Configure settings**.</span></span>

    ![Vytvoření jímku dat](./media/storsimple-data-manager-ui/create-new-data-sink.png)

10. <span data-ttu-id="6403b-154">Tady můžete vybrat hello typ úložiště, které chcete tooadd a hello další parametry související s úložištěm hello.</span><span class="sxs-lookup"><span data-stu-id="6403b-154">Here, you can select hello type of repository you want tooadd and hello other parameters associated with hello repository.</span></span> <span data-ttu-id="6403b-155">V obou případech se vytvoří frontu úložiště při spuštění úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="6403b-155">In both cases, a storage queue is created when hello job runs.</span></span> <span data-ttu-id="6403b-156">Tato fronta je naplňována zprávami o transformovaných objektech blob, jakmile jsou připravené.</span><span class="sxs-lookup"><span data-stu-id="6403b-156">This queue is populated with messages about transformed blobs as they are ready.</span></span> <span data-ttu-id="6403b-157">Hello název této fronty je hello stejný jako název hello hello definice úlohy.</span><span class="sxs-lookup"><span data-stu-id="6403b-157">hello name of this queue is hello same as hello name of hello job definition.</span></span> <span data-ttu-id="6403b-158">Pokud vyberete **Media Services** jako hello typ úložiště, pak můžete také zadat přihlašovací údaje účtu úložiště kde je vytvářena fronta hello.</span><span class="sxs-lookup"><span data-stu-id="6403b-158">If you select **Media Services** as hello repo type, then you can also enter storage account credentials where hello queue is created.</span></span>

    ![Nová data jímky podrobnosti](./media/storsimple-data-manager-ui/new-data-sink-details.png)

11. <span data-ttu-id="6403b-160">Po přidání hello úložiště dat, (která má několik sekund), zjistí hello úložišti v rozevírací nabídce hello v hello **název cílového účtu**.</span><span class="sxs-lookup"><span data-stu-id="6403b-160">After adding hello data repository (which takes a few seconds), you will find hello repo in hello dropdown in hello **Target account name**.</span></span>  <span data-ttu-id="6403b-161">Zvolte hello cíl, který potřebujete.</span><span class="sxs-lookup"><span data-stu-id="6403b-161">Choose hello target that you need.</span></span>

12. <span data-ttu-id="6403b-162">Klikněte na tlačítko **OK** definice úlohy toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="6403b-162">Click **OK** toocreate hello job definition.</span></span> <span data-ttu-id="6403b-163">Vaše definice úlohy je teď nastavený.</span><span class="sxs-lookup"><span data-stu-id="6403b-163">Your job definition is now set up.</span></span> <span data-ttu-id="6403b-164">Můžete použít tuto definici úlohy několikrát prostřednictvím hello uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6403b-164">You can use this job definition multiple times via hello UI.</span></span>

    ![Přidat nové definice úlohy](./media/storsimple-data-manager-ui/add-new-job-definition.png)

### <a name="run-hello-job-definition"></a><span data-ttu-id="6403b-166">Spustit definice úlohy hello</span><span class="sxs-lookup"><span data-stu-id="6403b-166">Run hello job definition</span></span>

<span data-ttu-id="6403b-167">Kdykoli budete potřebovat toomove data z účtu úložiště toohello StorSimple, které jste zadali v definici úlohy hello, budete potřebovat tooinvoke ho.</span><span class="sxs-lookup"><span data-stu-id="6403b-167">Whenever you need toomove data from StorSimple toohello storage account that you have specified in hello job definition, you will need tooinvoke it.</span></span> <span data-ttu-id="6403b-168">V každém vyvolání úlohy hello Změna parametrů hello je určitou volnost.</span><span class="sxs-lookup"><span data-stu-id="6403b-168">There is some flexibility in changing hello parameters every time you invoke hello job.</span></span> <span data-ttu-id="6403b-169">Hello kroky jsou následující:</span><span class="sxs-lookup"><span data-stu-id="6403b-169">hello steps are as follows:</span></span>

1. <span data-ttu-id="6403b-170">Vyberte služby StorSimple Manager dat a pak použijte příliš**monitorování**.</span><span class="sxs-lookup"><span data-stu-id="6403b-170">Select your StorSimple Data Manager service and go too**Monitoring**.</span></span> <span data-ttu-id="6403b-171">Klikněte na tlačítko **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="6403b-171">Click **Run Now**.</span></span>

    ![Definice úlohy aktivační události](./media/storsimple-data-manager-ui/run-now.png)

2. <span data-ttu-id="6403b-173">Zvolte hello definice úlohy, které chcete toorun.</span><span class="sxs-lookup"><span data-stu-id="6403b-173">Choose hello job definition that you want toorun.</span></span> <span data-ttu-id="6403b-174">Klikněte na tlačítko **spustit nastavení** toomodify všechna nastavení, které můžete chtít toochange pro tuto úlohu spustit.</span><span class="sxs-lookup"><span data-stu-id="6403b-174">Click **Run settings** toomodify any settings that you might want toochange for this job run.</span></span>

    ![Nastavení úloh spustit](./media/storsimple-data-manager-ui/run-settings.png)

3. <span data-ttu-id="6403b-176">Klikněte na tlačítko **OK** a pak klikněte na **spustit** toolaunch úlohu.</span><span class="sxs-lookup"><span data-stu-id="6403b-176">Click **OK** and then click **Run** toolaunch your job.</span></span> <span data-ttu-id="6403b-177">toomonitor této úlohy, přejděte toohello **úlohy** stránky vaše Data Manager zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="6403b-177">toomonitor this job, go toohello **Jobs** page in your StorSimple Data Manager.</span></span>

    ![Seznam úloh a stav](./media/storsimple-data-manager-ui/jobs-list-and-status.png)

4. <span data-ttu-id="6403b-179">V přidání toomonitoring v hello **úlohy** okno, můžete také poslouchat na fronty hello úložiště, kde se zpráva přidá pokaždé, když je přesunut do souboru z účtu úložiště toohello StorSimple.</span><span class="sxs-lookup"><span data-stu-id="6403b-179">In addition toomonitoring in hello **Jobs** blade, you can also listen on hello storage queue where a message is added every time a file is moved from StorSimple toohello storage account.</span></span>


## <a name="next-steps"></a><span data-ttu-id="6403b-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6403b-180">Next steps</span></span>

<span data-ttu-id="6403b-181">[Pomocí sady .NET SDK toolaunch StorSimple Manager dat úloh](storsimple-data-manager-dotnet-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="6403b-181">[Use .NET SDK toolaunch StorSimple Data Manager jobs](storsimple-data-manager-dotnet-jobs.md).</span></span>
