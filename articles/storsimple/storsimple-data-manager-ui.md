---
title: "Uživatelské rozhraní aplikace Microsoft Azure StorSimple Manager dat | Microsoft Docs"
description: "Popisuje způsob použití služby StorSimple Data Manager uživatelského rozhraní (soukromém náhledu)."
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
ms.openlocfilehash: 53a8599df2c647613122cd791b680e2e658586b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-using-the-storsimple-data-manager-service-ui-private-preview"></a><span data-ttu-id="ab64c-103">Spravovat pomocí služby StorSimple Data Manager uživatelského rozhraní (soukromém náhledu).</span><span class="sxs-lookup"><span data-stu-id="ab64c-103">Manage using the StorSimple Data Manager service UI (Private Preview)</span></span>

<span data-ttu-id="ab64c-104">Tento článek vysvětluje, jak můžete pomocí uživatelského rozhraní StorSimple Manager dat k transformaci dat na data uložená na řadu zařízení StorSimple 8000.</span><span class="sxs-lookup"><span data-stu-id="ab64c-104">This article explains how you can use the StorSimple Data Manager UI to perform data transformation on data residing on the StorSimple 8000 series devices.</span></span> <span data-ttu-id="ab64c-105">Transformovaná data mohou být spotřebovávána pak jinými službami Azure, například Azure Media Services, Azure HDInsight, Azure Machine Learning a Azure Search.</span><span class="sxs-lookup"><span data-stu-id="ab64c-105">The transformed data can then be consumed by other Azure services such as Azure Media Services, Azure HDInsight, Azure Machine Learning, and Azure Search.</span></span> 


## <a name="use-storsimple-data-transformation"></a><span data-ttu-id="ab64c-106">Transformace dat StorSimple použít</span><span class="sxs-lookup"><span data-stu-id="ab64c-106">Use StorSimple Data Transformation</span></span>

<span data-ttu-id="ab64c-107">Data Manager zařízení StorSimple je prostředků, ve kterém transformaci dat se dá vytvořit instance.</span><span class="sxs-lookup"><span data-stu-id="ab64c-107">The StorSimple Data Manager is the resource within which Data Transformation can be instantiated.</span></span> <span data-ttu-id="ab64c-108">Transformace dat service umožňuje přesun dat z vašeho místního zařízení StorSimple na objekty BLOB v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="ab64c-108">The Data Transformation service lets you move data from your StorSimple on-premises device to blobs in Azure storage.</span></span> <span data-ttu-id="ab64c-109">Proto v pracovním postupu budete muset zadat podrobnosti o zařízení StorSimple a data zájmu, které chcete přesunout k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ab64c-109">Hence, in workflow you need to specify the details about your StorSimple device and the data of interest that you want to move to the storage account.</span></span>

### <a name="create-a-storsimple-data-manager-service"></a><span data-ttu-id="ab64c-110">Vytvoření služby StorSimple Manager dat</span><span class="sxs-lookup"><span data-stu-id="ab64c-110">Create a StorSimple Data Manager service</span></span>

<span data-ttu-id="ab64c-111">Proveďte následující kroky k vytvoření služby StorSimple Data Manager.</span><span class="sxs-lookup"><span data-stu-id="ab64c-111">Perform the following steps to create a StorSimple Data Manager service.</span></span>

1. <span data-ttu-id="ab64c-112">Chcete-li vytvořit službu StorSimple Manager dat, přejděte na [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)</span><span class="sxs-lookup"><span data-stu-id="ab64c-112">To create a StorSimple Data Manager service, go to [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)</span></span>

2. <span data-ttu-id="ab64c-113">Klikněte  **+**  ikonu a vyhledávání pro StorSimple Data Manager.</span><span class="sxs-lookup"><span data-stu-id="ab64c-113">Click the **+** icon and search for StorSimple Data Manager.</span></span> <span data-ttu-id="ab64c-114">Klikněte na tlačítko služby StorSimple Manager dat a pak klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ab64c-114">Click your StorSimple Data Manager service and then click **Create**.</span></span>

3. <span data-ttu-id="ab64c-115">Pokud je vaše předplatné povolená pro vytvoření této služby, zobrazí se následující okno.</span><span class="sxs-lookup"><span data-stu-id="ab64c-115">If your subscription is enabled for creating this service, you see the following blade.</span></span>

    ![Vytvořte prostředek StorSimple Data správců](./media/storsimple-data-manager-ui/create-new-data-manager-service.png)

4. <span data-ttu-id="ab64c-117">Zadejte vstupy a klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ab64c-117">Enter the inputs and click **Create**.</span></span> <span data-ttu-id="ab64c-118">Zadané umístění musí být ten, který je umístěno účty úložiště a služby StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="ab64c-118">The specified location should be the one that houses your storage accounts and your StorSimple Manager service.</span></span> <span data-ttu-id="ab64c-119">V současné době jsou podporovány pouze oblasti západní USA a západní Evropa.</span><span class="sxs-lookup"><span data-stu-id="ab64c-119">Currently, only West US and West Europe regions are supported.</span></span> <span data-ttu-id="ab64c-120">Proto služby StorSimple Manager, service Data Manager a přidruženého účtu úložiště by měl být v předchozím podporovaných oblastí.</span><span class="sxs-lookup"><span data-stu-id="ab64c-120">Hence, your StorSimple Manager service, Data Manager service, and the associated storage account should all be in the preceding supported regions.</span></span> <span data-ttu-id="ab64c-121">Vytvořte službu trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="ab64c-121">It takes about a minute to create the service.</span></span>

### <a name="create-a-data-transformation-job-definition"></a><span data-ttu-id="ab64c-122">Vytvoření definice úlohy transformace dat</span><span class="sxs-lookup"><span data-stu-id="ab64c-122">Create a data transformation job definition</span></span>

<span data-ttu-id="ab64c-123">V rámci služby StorSimple Manager dat budete muset vytvořit definici úlohy transformace data.</span><span class="sxs-lookup"><span data-stu-id="ab64c-123">Within a StorSimple Data Manager service, you need to create a data transformation job definition.</span></span> <span data-ttu-id="ab64c-124">Definice úlohy Určuje podrobné informace o datech, která vás zajímá Přesun do účtu úložiště v nativním formátu.</span><span class="sxs-lookup"><span data-stu-id="ab64c-124">A job definition specifies details of the data that you are interested in moving into a storage account in the native format.</span></span> 

<span data-ttu-id="ab64c-125">Proveďte následující kroky k vytvoření nové definice úlohy transformace dat.</span><span class="sxs-lookup"><span data-stu-id="ab64c-125">Perform the following steps to create a new data transformation job definition.</span></span>

1.  <span data-ttu-id="ab64c-126">Přejděte ke službě, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ab64c-126">Navigate to the service that you created.</span></span> <span data-ttu-id="ab64c-127">Klikněte na tlačítko **+ úlohy definice**.</span><span class="sxs-lookup"><span data-stu-id="ab64c-127">Click **+ Job Definition**.</span></span>

    ![Klikněte na tlačítko + definice úlohy](./media/storsimple-data-manager-ui/click-add-job-definition.png)

2. <span data-ttu-id="ab64c-129">Otevře nové okno Definice úlohy.</span><span class="sxs-lookup"><span data-stu-id="ab64c-129">The new job definition blade opens up.</span></span> <span data-ttu-id="ab64c-130">Pojmenujte svou definici úlohy a klikněte na tlačítko **zdroj**.</span><span class="sxs-lookup"><span data-stu-id="ab64c-130">Give your job definition a name and click **Source**.</span></span> <span data-ttu-id="ab64c-131">V **zdroj dat konfigurace** okno, zadejte podrobnosti o zařízení StorSimple a požadovaná data.</span><span class="sxs-lookup"><span data-stu-id="ab64c-131">In the **Configure data source** blade, specify the details of your StorSimple device and the data of interest.</span></span>

    ![Vytvořit definici úlohy](./media/storsimple-data-manager-ui//create-new-job-deifnition.png)

3. <span data-ttu-id="ab64c-133">Vzhledem k tomu, že toto je nová služba Data Manager, jsou nakonfigurovány žádné datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="ab64c-133">Since this is a new Data Manager service, no data repositories are configured.</span></span> <span data-ttu-id="ab64c-134">Chcete-li přidat StorSimple Manager jako úložiště dat, klikněte na tlačítko **přidat nový** rozevírací úložiště dat a pak klikněte na **úložiště dat přidat**.</span><span class="sxs-lookup"><span data-stu-id="ab64c-134">To add your StorSimple Manager as a data repository, click **Add new** in the data repository dropdown and then click **Add Data Repository**.</span></span>

4. <span data-ttu-id="ab64c-135">Zvolte **řady StorSimple 8000 Manager** jako úložiště zadejte a zadejte vlastnosti vaší **StorSimple Manager**.</span><span class="sxs-lookup"><span data-stu-id="ab64c-135">Choose **StorSimple 8000 series Manager** as the repository type and enter the properties of your **StorSimple Manager**.</span></span> <span data-ttu-id="ab64c-136">Pro **Id prostředku** pole, je nutné zadat číslo před **:** v registračního klíče služby StorSimple manager.</span><span class="sxs-lookup"><span data-stu-id="ab64c-136">For the **Resource Id** field, you need to enter the number before the **:** in the registration key of your StorSimple manager.</span></span>

    ![Vytvoření zdroje dat](./media/storsimple-data-manager-ui/create-new-data-source.png)

5.  <span data-ttu-id="ab64c-138">Klikněte na tlačítko **OK** po dokončení.</span><span class="sxs-lookup"><span data-stu-id="ab64c-138">Click **OK** when done.</span></span> <span data-ttu-id="ab64c-139">To umožňuje ušetřit úložiště dat a tato StorSimple Manager lze opětovně použít v jiné definice úlohy bez opětovného zadávání tyto parametry.</span><span class="sxs-lookup"><span data-stu-id="ab64c-139">This saves your data repository and this StorSimple Manager can be reused in other job definitions without entering these parameters again.</span></span> <span data-ttu-id="ab64c-140">Jak dlouho trvá několik sekund, po kliknutí na tlačítko **OK** pro StorSimple Manager objeví v rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="ab64c-140">It takes a few seconds after you click **OK** for the StorSimple Manager to show up in the dropdown.</span></span>

6.  <span data-ttu-id="ab64c-141">V **zdroj dat konfigurace** okno, zadejte název zařízení a název svazku, která obsahuje vaše data týkající se.</span><span class="sxs-lookup"><span data-stu-id="ab64c-141">In the **Configure data source** blade, enter the device name and the volume name that has your data of interest.</span></span>

7.  <span data-ttu-id="ab64c-142">V **filtru** pododdílu, zadejte kořenový adresář, který obsahuje vaše data týkající se (v tomto poli by měla začínat znakem `\`).</span><span class="sxs-lookup"><span data-stu-id="ab64c-142">In the **Filter** subsection, enter the root directory that contains your data of interest (this field should start with a `\`).</span></span> <span data-ttu-id="ab64c-143">Můžete také přidat všechny souboru filtry.</span><span class="sxs-lookup"><span data-stu-id="ab64c-143">You can also add any file filters here.</span></span>

8.  <span data-ttu-id="ab64c-144">Služba transformaci dat funguje na data, která vložena do Azure pomocí snímků.</span><span class="sxs-lookup"><span data-stu-id="ab64c-144">The data transformation service works on the data that is pushed up to the Azure via snapshots.</span></span> <span data-ttu-id="ab64c-145">Při spuštění této úlohy je možné provést zálohu při každém spuštění této úlohy (pro práci na nejnovější data), nebo chcete použít poslední existující zálohy v cloudu (Pokud pracujete na některé Archivovaná data).</span><span class="sxs-lookup"><span data-stu-id="ab64c-145">When running this job, you can choose to take a backup every time this job is run (to work on latest data) or to use the last existing backup in the cloud (if you are working on some archived data).</span></span>

    ![Podrobnosti nové zdroje dat](./media/storsimple-data-manager-ui/new-data-source-details.png)

9. <span data-ttu-id="ab64c-147">V dalším kroku nastavení Target je potřeba nakonfigurovat ji tak.</span><span class="sxs-lookup"><span data-stu-id="ab64c-147">Next, the Target settings need to be configured.</span></span> <span data-ttu-id="ab64c-148">Existují 2 typy podporované cíle – účty Azure Storage a účty služby Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="ab64c-148">There are 2 types of supported targets – Azure Storage accounts and Azure Media Services accounts.</span></span> <span data-ttu-id="ab64c-149">Vyberte účty úložiště pro soubory umístit do objektů BLOB v daném účtu.</span><span class="sxs-lookup"><span data-stu-id="ab64c-149">Choose storage accounts to put files into blobs in that account.</span></span> <span data-ttu-id="ab64c-150">Zvolte media services účet ukládat soubory do prostředky v daném účtu.</span><span class="sxs-lookup"><span data-stu-id="ab64c-150">Choose media services account to put files into assets in that account.</span></span> <span data-ttu-id="ab64c-151">Znovu musíme přidejte úložiště.</span><span class="sxs-lookup"><span data-stu-id="ab64c-151">Again, we need to add a repository.</span></span> <span data-ttu-id="ab64c-152">V rozevírací nabídce, a vyberte **přidat nový** a potom **nakonfigurovat nastavení**.</span><span class="sxs-lookup"><span data-stu-id="ab64c-152">In the dropdown, select **Add new** and then **Configure settings**.</span></span>

    ![Vytvoření jímku dat](./media/storsimple-data-manager-ui/create-new-data-sink.png)

10. <span data-ttu-id="ab64c-154">Tady můžete vybrat typ úložiště, které chcete přidat a ostatní parametry související s úložištěm.</span><span class="sxs-lookup"><span data-stu-id="ab64c-154">Here, you can select the type of repository you want to add and the other parameters associated with the repository.</span></span> <span data-ttu-id="ab64c-155">V obou případech se vytvoří frontu úložiště spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="ab64c-155">In both cases, a storage queue is created when the job runs.</span></span> <span data-ttu-id="ab64c-156">Tato fronta je naplňována zprávami o transformovaných objektech blob, jakmile jsou připravené.</span><span class="sxs-lookup"><span data-stu-id="ab64c-156">This queue is populated with messages about transformed blobs as they are ready.</span></span> <span data-ttu-id="ab64c-157">Název této fronty je stejný jako název definice úlohy.</span><span class="sxs-lookup"><span data-stu-id="ab64c-157">The name of this queue is the same as the name of the job definition.</span></span> <span data-ttu-id="ab64c-158">Pokud vyberete **Media Services** jako typ úložiště, pak můžete také zadat přihlašovací údaje účtu úložiště kde je vytvářena fronta.</span><span class="sxs-lookup"><span data-stu-id="ab64c-158">If you select **Media Services** as the repo type, then you can also enter storage account credentials where the queue is created.</span></span>

    ![Nová data jímky podrobnosti](./media/storsimple-data-manager-ui/new-data-sink-details.png)

11. <span data-ttu-id="ab64c-160">Po přidání úložiště dat (která má několik sekund), bude najít v rozevírací nabídce v úložišti **název cílového účtu**.</span><span class="sxs-lookup"><span data-stu-id="ab64c-160">After adding the data repository (which takes a few seconds), you will find the repo in the dropdown in the **Target account name**.</span></span>  <span data-ttu-id="ab64c-161">Vyberte cíl, který potřebujete.</span><span class="sxs-lookup"><span data-stu-id="ab64c-161">Choose the target that you need.</span></span>

12. <span data-ttu-id="ab64c-162">Klikněte na tlačítko **OK** k vytvoření definice úlohy.</span><span class="sxs-lookup"><span data-stu-id="ab64c-162">Click **OK** to create the job definition.</span></span> <span data-ttu-id="ab64c-163">Vaše definice úlohy je teď nastavený.</span><span class="sxs-lookup"><span data-stu-id="ab64c-163">Your job definition is now set up.</span></span> <span data-ttu-id="ab64c-164">Můžete použít tuto definici úlohy několikrát prostřednictvím uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ab64c-164">You can use this job definition multiple times via the UI.</span></span>

    ![Přidat nové definice úlohy](./media/storsimple-data-manager-ui/add-new-job-definition.png)

### <a name="run-the-job-definition"></a><span data-ttu-id="ab64c-166">Spustit definici úlohy</span><span class="sxs-lookup"><span data-stu-id="ab64c-166">Run the job definition</span></span>

<span data-ttu-id="ab64c-167">Kdykoli budete potřebovat pro přesun dat z StorSimple k účtu úložiště, který jste zadali v definici úlohy, musíte ji volat.</span><span class="sxs-lookup"><span data-stu-id="ab64c-167">Whenever you need to move data from StorSimple to the storage account that you have specified in the job definition, you will need to invoke it.</span></span> <span data-ttu-id="ab64c-168">Není určitou volnost v každém vyvolání úlohy Změna parametrů.</span><span class="sxs-lookup"><span data-stu-id="ab64c-168">There is some flexibility in changing the parameters every time you invoke the job.</span></span> <span data-ttu-id="ab64c-169">Kroky jsou následující:</span><span class="sxs-lookup"><span data-stu-id="ab64c-169">The steps are as follows:</span></span>

1. <span data-ttu-id="ab64c-170">Vybrat služby StorSimple Manager dat, přejděte na **monitorování**.</span><span class="sxs-lookup"><span data-stu-id="ab64c-170">Select your StorSimple Data Manager service and go to **Monitoring**.</span></span> <span data-ttu-id="ab64c-171">Klikněte na tlačítko **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="ab64c-171">Click **Run Now**.</span></span>

    ![Definice úlohy aktivační události](./media/storsimple-data-manager-ui/run-now.png)

2. <span data-ttu-id="ab64c-173">Zvolte definici úlohy, který chcete spustit.</span><span class="sxs-lookup"><span data-stu-id="ab64c-173">Choose the job definition that you want to run.</span></span> <span data-ttu-id="ab64c-174">Klikněte na tlačítko **spustit nastavení** změnit nastavení, které můžete chtít změnit pro tuto úlohu spustit.</span><span class="sxs-lookup"><span data-stu-id="ab64c-174">Click **Run settings** to modify any settings that you might want to change for this job run.</span></span>

    ![Nastavení úloh spustit](./media/storsimple-data-manager-ui/run-settings.png)

3. <span data-ttu-id="ab64c-176">Klikněte na tlačítko **OK** a pak klikněte na **spustit** spustit úlohu.</span><span class="sxs-lookup"><span data-stu-id="ab64c-176">Click **OK** and then click **Run** to launch your job.</span></span> <span data-ttu-id="ab64c-177">Ke sledování této úlohy, přejděte na **úlohy** stránky vaše Data Manager zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="ab64c-177">To monitor this job, go to the **Jobs** page in your StorSimple Data Manager.</span></span>

    ![Seznam úloh a stav](./media/storsimple-data-manager-ui/jobs-list-and-status.png)

4. <span data-ttu-id="ab64c-179">Kromě monitorování v **úlohy** okno, můžete také poslouchat na fronty úložiště, kde se zpráva přidá pokaždé, když se soubor přesune ze zařízení StorSimple účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="ab64c-179">In addition to monitoring in the **Jobs** blade, you can also listen on the storage queue where a message is added every time a file is moved from StorSimple to the storage account.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ab64c-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ab64c-180">Next steps</span></span>

<span data-ttu-id="ab64c-181">[Spuštění úlohy StorSimple Manager dat pomocí .NET SDK](storsimple-data-manager-dotnet-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="ab64c-181">[Use .NET SDK to launch StorSimple Data Manager jobs](storsimple-data-manager-dotnet-jobs.md).</span></span>