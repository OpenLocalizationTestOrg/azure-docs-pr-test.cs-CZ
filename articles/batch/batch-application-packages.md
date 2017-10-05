---
title: "Instalovat balíčky aplikací na výpočetní uzly - Azure Batch | Microsoft Docs"
description: "Použijte funkci balíčků aplikací Azure Batch snadno spravovat více aplikací a verzí pro instalaci na Batch výpočetních uzlů."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 3b6044b7-5f65-4a27-9d43-71e1863d16cf
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: afcc04c80ec15872a22de5d5969a7ef6a583562f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-applications-to-compute-nodes-with-batch-application-packages"></a><span data-ttu-id="68db7-103">Nasazení aplikací na výpočetní uzly pomocí balíčků aplikací Batch</span><span class="sxs-lookup"><span data-stu-id="68db7-103">Deploy applications to compute nodes with Batch application packages</span></span>

<span data-ttu-id="68db7-104">Funkci balíčků aplikací Azure Batch poskytuje snadnou správu úloh aplikací a jejich nasazení na výpočetní uzly ve fondu.</span><span class="sxs-lookup"><span data-stu-id="68db7-104">The application packages feature of Azure Batch provides easy management of task applications and their deployment to the compute nodes in your pool.</span></span> <span data-ttu-id="68db7-105">Pomocí balíčků aplikací můžete odeslat a spravovat více verzí aplikací, které vaše úkoly spouštět, včetně jejich podpůrné soubory.</span><span class="sxs-lookup"><span data-stu-id="68db7-105">With application packages, you can upload and manage multiple versions of the applications your tasks run, including their supporting files.</span></span> <span data-ttu-id="68db7-106">Můžete pak automaticky nasadit jednu nebo více těchto aplikací na výpočetní uzly ve fondu.</span><span class="sxs-lookup"><span data-stu-id="68db7-106">You can then automatically deploy one or more of these applications to the compute nodes in your pool.</span></span>

<span data-ttu-id="68db7-107">V tomto článku se dozvíte, jak odesílat a spravovat balíčky aplikací na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="68db7-107">In this article, you will learn how to upload and manage application packages in the Azure portal.</span></span> <span data-ttu-id="68db7-108">Potom se dozvíte, jak k instalaci na výpočetní uzly fondu s [Batch .NET] [ api_net] knihovny.</span><span class="sxs-lookup"><span data-stu-id="68db7-108">You will then learn how to install them on a pool's compute nodes with the [Batch .NET][api_net] library.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="68db7-109">Balíčky aplikací jsou podporované ve všech fondech služby Batch vytvořených po 5. červenci 2017.</span><span class="sxs-lookup"><span data-stu-id="68db7-109">Application packages are supported on all Batch pools created after 5 July 2017.</span></span> <span data-ttu-id="68db7-110">Ve fondech služby Batch vytvořených mezi 10. březnem 2016 a 5. červencem 2017 jsou podporované, pouze pokud byl fond vytvořen pomocí konfigurace cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="68db7-110">They are supported on Batch pools created between 10 March 2016 and 5 July 2017 only if the pool was created using a Cloud Service configuration.</span></span> <span data-ttu-id="68db7-111">Fondy služby Batch vytvořené před 10. březnem 2016 nepodporují balíčky aplikací.</span><span class="sxs-lookup"><span data-stu-id="68db7-111">Batch pools created prior to 10 March 2016 do not support application packages.</span></span>
>
> <span data-ttu-id="68db7-112">Rozhraní API pro vytváření a správě balíčků aplikací jsou součástí [rozhraní Batch Management .NET] [[api_net_mgmt]] knihovny.</span><span class="sxs-lookup"><span data-stu-id="68db7-112">The APIs for creating and managing application packages are part of the [Batch Management .NET][[api_net_mgmt]] library.</span></span> <span data-ttu-id="68db7-113">Rozhraní API pro instalaci balíčků aplikací na výpočetním uzlu jsou součástí [Batch .NET] [ api_net] knihovny.</span><span class="sxs-lookup"><span data-stu-id="68db7-113">The APIs for installing application packages on a compute node are part of the [Batch .NET][api_net] library.</span></span>  
>
> <span data-ttu-id="68db7-114">Funkci balíčků aplikací v rámci popsaného nahrazuje funkci aplikace Batch k dispozici v předchozích verzích služby.</span><span class="sxs-lookup"><span data-stu-id="68db7-114">The application packages feature described here supersedes the Batch Apps feature available in previous versions of the service.</span></span>
> 
> 

## <a name="application-package-requirements"></a><span data-ttu-id="68db7-115">Požadavky na balíček aplikace</span><span class="sxs-lookup"><span data-stu-id="68db7-115">Application package requirements</span></span>
<span data-ttu-id="68db7-116">Chcete-li použít balíčky aplikací, je potřeba [propojení účtu Azure Storage](#link-a-storage-account) k účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="68db7-116">To use application packages, you need to [link an Azure Storage account](#link-a-storage-account) to your Batch account.</span></span>

<span data-ttu-id="68db7-117">Tato funkce byla zavedena v [Batch REST API] [ api_rest] verze 2015-12-01.2.2 a odpovídající [Batch .NET] [ api_net] knihovní verze 3.1.0.</span><span class="sxs-lookup"><span data-stu-id="68db7-117">This feature was introduced in [Batch REST API][api_rest] version 2015-12-01.2.2 and the corresponding [Batch .NET][api_net] library version 3.1.0.</span></span> <span data-ttu-id="68db7-118">Doporučujeme vám, že vždy používáte nejnovější verzi rozhraní API při práci se službou Batch.</span><span class="sxs-lookup"><span data-stu-id="68db7-118">We recommend that you always use the latest API version when working with Batch.</span></span>

> [!NOTE]
> <span data-ttu-id="68db7-119">Balíčky aplikací jsou podporované ve všech fondech služby Batch vytvořených po 5. červenci 2017.</span><span class="sxs-lookup"><span data-stu-id="68db7-119">Application packages are supported on all Batch pools created after 5 July 2017.</span></span> <span data-ttu-id="68db7-120">Ve fondech služby Batch vytvořených mezi 10. březnem 2016 a 5. červencem 2017 jsou podporované, pouze pokud byl fond vytvořen pomocí konfigurace cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="68db7-120">They are supported on Batch pools created between 10 March 2016 and 5 July 2017 only if the pool was created using a Cloud Service configuration.</span></span> <span data-ttu-id="68db7-121">Fondy služby Batch vytvořené před 10. březnem 2016 nepodporují balíčky aplikací.</span><span class="sxs-lookup"><span data-stu-id="68db7-121">Batch pools created prior to 10 March 2016 do not support application packages.</span></span>
>
>

## <a name="about-applications-and-application-packages"></a><span data-ttu-id="68db7-122">O aplikacích a balíčky aplikací</span><span class="sxs-lookup"><span data-stu-id="68db7-122">About applications and application packages</span></span>
<span data-ttu-id="68db7-123">V rámci Azure Batch *aplikace* odkazuje na sadu verzí binární soubory, které lze automaticky staženy na výpočetní uzly ve fondu.</span><span class="sxs-lookup"><span data-stu-id="68db7-123">Within Azure Batch, an *application* refers to a set of versioned binaries that can be automatically downloaded to the compute nodes in your pool.</span></span> <span data-ttu-id="68db7-124">*Balíčku aplikace* odkazuje *konkrétní sadu* těchto binárních souborů a představuje daný *verze* aplikace.</span><span class="sxs-lookup"><span data-stu-id="68db7-124">An *application package* refers to a *specific set* of those binaries and represents a given *version* of the application.</span></span>

<span data-ttu-id="68db7-125">![Vysokoúrovňový diagram aplikace a balíčky aplikací][1]</span><span class="sxs-lookup"><span data-stu-id="68db7-125">![High-level diagram of applications and application packages][1]</span></span>

### <a name="applications"></a><span data-ttu-id="68db7-126">Aplikace</span><span class="sxs-lookup"><span data-stu-id="68db7-126">Applications</span></span>
<span data-ttu-id="68db7-127">Aplikace ve službě Batch obsahuje jeden nebo více aplikací, balíčků a určuje možnosti konfigurace pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="68db7-127">An application in Batch contains one or more application packages and specifies configuration options for the application.</span></span> <span data-ttu-id="68db7-128">Aplikace můžete například zadat výchozí verze balíčku aplikace nainstalovat na výpočetní uzly a zda může být jeho balíčky aktualizovat ani odstranit.</span><span class="sxs-lookup"><span data-stu-id="68db7-128">For example, an application can specify the default application package version to install on compute nodes and whether its packages can be updated or deleted.</span></span>

### <a name="application-packages"></a><span data-ttu-id="68db7-129">Balíčky aplikací</span><span class="sxs-lookup"><span data-stu-id="68db7-129">Application packages</span></span>
<span data-ttu-id="68db7-130">Balíček aplikace je soubor .zip, který obsahuje binární soubory aplikace a podpůrné soubory, které jsou požadovány pro vaše úkoly a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="68db7-130">An application package is a .zip file that contains the application binaries and supporting files that are required for your tasks to run the application.</span></span> <span data-ttu-id="68db7-131">Každý balíček aplikace představuje určitou verzi aplikace.</span><span class="sxs-lookup"><span data-stu-id="68db7-131">Each application package represents a specific version of the application.</span></span>

<span data-ttu-id="68db7-132">Můžete zadat balíčky aplikací na úrovni fondu a úloh.</span><span class="sxs-lookup"><span data-stu-id="68db7-132">You can specify application packages at the pool and task levels.</span></span> <span data-ttu-id="68db7-133">Nejméně jeden z těchto balíčků a (volitelně) verzi můžete zadat při vytváření fondu nebo úloh.</span><span class="sxs-lookup"><span data-stu-id="68db7-133">You can specify one or more of these packages and (optionally) a version when you create a pool or task.</span></span>

* <span data-ttu-id="68db7-134">**Fond aplikací balíčky** nasazených *každých* uzlu ve fondu.</span><span class="sxs-lookup"><span data-stu-id="68db7-134">**Pool application packages** are deployed to *every* node in the pool.</span></span> <span data-ttu-id="68db7-135">Jestliže se uzel připojí fondu, a, pokud je restartovat nebo obnovit z Image se aplikace nasadí.</span><span class="sxs-lookup"><span data-stu-id="68db7-135">Applications are deployed when a node joins a pool, and when it is rebooted or reimaged.</span></span>
  
    <span data-ttu-id="68db7-136">Balíčky fondu aplikací jsou vhodné, když všechny uzly v rámci fondu spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="68db7-136">Pool application packages are appropriate when all nodes in a pool execute a job's tasks.</span></span> <span data-ttu-id="68db7-137">Jeden nebo více balíčků aplikací můžete určit, když vytvoříte fond, a můžete přidat nebo aktualizovat existující fond balíčky.</span><span class="sxs-lookup"><span data-stu-id="68db7-137">You can specify one or more application packages when you create a pool, and you can add or update an existing pool's packages.</span></span> <span data-ttu-id="68db7-138">Pokud aktualizujete balíčky existující fond aplikací, je nutné restartovat jeho uzly k instalaci nového balíčku.</span><span class="sxs-lookup"><span data-stu-id="68db7-138">If you update an existing pool's application packages, you must restart its nodes to install the new package.</span></span>
* <span data-ttu-id="68db7-139">**Úloha balíčky aplikací** se nasadit jenom na výpočetním uzlu naplánované spuštění úlohy, právě před spuštěním příkazového řádku úkolu.</span><span class="sxs-lookup"><span data-stu-id="68db7-139">**Task application packages** are deployed only to a compute node scheduled to run a task, just before running the task's command line.</span></span> <span data-ttu-id="68db7-140">Pokud zadaný balíček aplikace a verze je již v uzlu, není znovu nasazena a používá existující balíček.</span><span class="sxs-lookup"><span data-stu-id="68db7-140">If the specified application package and version is already on the node, it is not redeployed and the existing package is used.</span></span>
  
    <span data-ttu-id="68db7-141">Balíčky aplikací úloh jsou užitečné v sdílený fond prostředí, kde různé úlohy se spouštějí na jeden fond, a fondu se neodstraní po dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="68db7-141">Task application packages are useful in shared-pool environments, where different jobs are run on one pool, and the pool is not deleted when a job is completed.</span></span> <span data-ttu-id="68db7-142">Pokud má vaše úloha méně úkolů, než je uzlů ve fondu, balíčky aplikací úkolů můžou omezit přenosy dat, protože se aplikace může nasadit jen na uzly, které úkoly budou skutečně provádět.</span><span class="sxs-lookup"><span data-stu-id="68db7-142">If your job has fewer tasks than nodes in the pool, task application packages can minimize data transfer since your application is deployed only to the nodes that run tasks.</span></span>
  
    <span data-ttu-id="68db7-143">Další scénáře, které můžete využít balíčky aplikací úloh jsou úlohy, které spouštějí rozsáhlé aplikace, ale pouze několik úkolů.</span><span class="sxs-lookup"><span data-stu-id="68db7-143">Other scenarios that can benefit from task application packages are jobs that run a large application, but for only a few tasks.</span></span> <span data-ttu-id="68db7-144">Předběžné zpracování fáze nebo sloučení úlohy, kde je aplikace předběžné zpracování nebo sloučená těžký, může například těžit z pomocí balíčků aplikací úloh.</span><span class="sxs-lookup"><span data-stu-id="68db7-144">For example, a pre-processing stage or a merge task, where the pre-processing or merge application is heavyweight, may benefit from using task application packages.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="68db7-145">Existují omezení počtu aplikace a balíčky aplikací v rámci účtu Batch a na velikosti balíčku maximální aplikace.</span><span class="sxs-lookup"><span data-stu-id="68db7-145">There are restrictions on the number of applications and application packages within a Batch account and on the maximum application package size.</span></span> <span data-ttu-id="68db7-146">V tématu [kvóty a omezení pro službu Azure Batch](batch-quota-limit.md) podrobnosti o těchto omezeních.</span><span class="sxs-lookup"><span data-stu-id="68db7-146">See [Quotas and limits for the Azure Batch service](batch-quota-limit.md) for details about these limits.</span></span>
> 
> 

### <a name="benefits-of-application-packages"></a><span data-ttu-id="68db7-147">Výhody balíčky aplikací</span><span class="sxs-lookup"><span data-stu-id="68db7-147">Benefits of application packages</span></span>
<span data-ttu-id="68db7-148">Balíčky aplikací, můžete zjednodušit kód v řešení pro Batch a snížení režie potřebná ke správě aplikací, které vaše úkoly spouštět.</span><span class="sxs-lookup"><span data-stu-id="68db7-148">Application packages can simplify the code in your Batch solution and lower the overhead required to manage the applications that your tasks run.</span></span>

<span data-ttu-id="68db7-149">Pomocí balíčků aplikací váš fond spouštěcí úkol nemá k určení dlouhý seznam jednotlivé zdrojové soubory k instalaci na uzlech.</span><span class="sxs-lookup"><span data-stu-id="68db7-149">With application packages, your pool's start task doesn't have to specify a long list of individual resource files to install on the nodes.</span></span> <span data-ttu-id="68db7-150">Nemusíte ručně spravovat více verzí soubory aplikace v Azure Storage nebo na uzly.</span><span class="sxs-lookup"><span data-stu-id="68db7-150">You don't have to manually manage multiple versions of your application files in Azure Storage, or on your nodes.</span></span> <span data-ttu-id="68db7-151">A nemusíte si dělat starosti o generování [adresy URL SAS](../storage/common/storage-dotnet-shared-access-signature-part-1.md) k poskytování přístupu k souborům ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="68db7-151">And, you don't need to worry about generating [SAS URLs](../storage/common/storage-dotnet-shared-access-signature-part-1.md) to provide access to the files in your Storage account.</span></span> <span data-ttu-id="68db7-152">Služba batch pracuje na pozadí s Azure Storage a ukládat balíčky aplikací, je nasadit na výpočetních uzlech.</span><span class="sxs-lookup"><span data-stu-id="68db7-152">Batch works in the background with Azure Storage to store application packages and deploy them to compute nodes.</span></span>

> [!NOTE] 
> <span data-ttu-id="68db7-153">Celková velikost spouštěcího úkolu nesmí přesahovat 32768 znaků, včetně souborů prostředků a proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="68db7-153">The total size of a start task must be less than or equal to 32768 characters, including resource files and environment variables.</span></span> <span data-ttu-id="68db7-154">Pokud spouštěcí úkol překračuje tento limit, pak pomocí balíčků aplikací je jinou možnost.</span><span class="sxs-lookup"><span data-stu-id="68db7-154">If your start task exceeds this limit, then using application packages is another option.</span></span> <span data-ttu-id="68db7-155">Můžete také vytvořit komprimované archivu obsahující soubory prostředků, nahrajte ho jako objekt blob do služby Azure Storage a potom rozbalte ho z příkazového řádku úkolu spuštění.</span><span class="sxs-lookup"><span data-stu-id="68db7-155">You can also create a zipped archive containing your resource files, upload it as a blob to Azure Storage, and then unzip it from the command line of your start task.</span></span> 
>
>

## <a name="upload-and-manage-applications"></a><span data-ttu-id="68db7-156">Odesílat a spravovat aplikace</span><span class="sxs-lookup"><span data-stu-id="68db7-156">Upload and manage applications</span></span>
<span data-ttu-id="68db7-157">Můžete použít [portál Azure] [ portal] nebo [rozhraní Batch Management .NET](batch-management-dotnet.md) knihovny ke správě balíčků aplikací v účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="68db7-157">You can use the [Azure portal][portal] or the [Batch Management .NET](batch-management-dotnet.md) library to manage the application packages in your Batch account.</span></span> <span data-ttu-id="68db7-158">V následujících částech několik nejprve ukážeme, jak propojit účet úložiště a pak zabývat přidáním aplikací a balíčků a jejich pomocí portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="68db7-158">In the next few sections, we first show how to link a Storage account, then discuss adding applications and packages and managing them with the portal.</span></span>

### <a name="link-a-storage-account"></a><span data-ttu-id="68db7-159">Odkaz účet úložiště</span><span class="sxs-lookup"><span data-stu-id="68db7-159">Link a Storage account</span></span>
<span data-ttu-id="68db7-160">Pokud chcete používat balíčky aplikací, je nutné nejprve propojit účtu Azure Storage k účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="68db7-160">To use application packages, you must first link an Azure Storage account to your Batch account.</span></span> <span data-ttu-id="68db7-161">Pokud ještě nemáte nakonfigurovaný účet úložiště, portálu Azure zobrazí upozornění poprvé kliknete **aplikace** dlaždice v nástroji **účet Batch** okno.</span><span class="sxs-lookup"><span data-stu-id="68db7-161">If you have not yet configured a Storage account, the Azure portal displays a warning the first time you click the **Applications** tile in the **Batch account** blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="68db7-162">Batch aktuálně podporuje *pouze* **pro obecné účely** typ účtu úložiště, jak je popsáno v kroku 5, [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account)v [účty Azure storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="68db7-162">Batch currently supports *only* the **General-purpose** storage account type as described in step 5, [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account), in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="68db7-163">Při propojení účtu Azure Storage k účtu Batch, propojte *pouze* **pro obecné účely** účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="68db7-163">When you link an Azure Storage account to your Batch account, link *only* a **General-purpose** storage account.</span></span>
> 
> 

<span data-ttu-id="68db7-164">![Upozornění: nakonfigurován žádný účet úložiště, na portálu Azure][9]</span><span class="sxs-lookup"><span data-stu-id="68db7-164">!['No storage account configured' warning in Azure portal][9]</span></span>

<span data-ttu-id="68db7-165">Služba Batch používá přidružený účet úložiště pro uložení balíčků vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="68db7-165">The Batch service uses the associated Storage account to store your application packages.</span></span> <span data-ttu-id="68db7-166">Po připojení dva účty Batch můžete automaticky nasadit balíčky uložených v propojeném účtu úložiště pro výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="68db7-166">After you've linked the two accounts, Batch can automatically deploy the packages stored in the linked Storage account to your compute nodes.</span></span> <span data-ttu-id="68db7-167">Odkaz účet úložiště k účtu Batch, klikněte na tlačítko **nastavení účtu úložiště** na **upozornění** okna a pak klikněte na tlačítko **účet úložiště** na **účet úložiště** okno.</span><span class="sxs-lookup"><span data-stu-id="68db7-167">To link a Storage account to your Batch account, click **Storage account settings** on the **Warning** blade, and then click **Storage Account** on the **Storage Account** blade.</span></span>

<span data-ttu-id="68db7-168">![Zvolte okně účtu úložiště na portálu Azure][10]</span><span class="sxs-lookup"><span data-stu-id="68db7-168">![Choose storage account blade in Azure portal][10]</span></span>

<span data-ttu-id="68db7-169">Doporučujeme vytvořit účet úložiště *konkrétně* pro použití s vaším účtem Batch a vyberte ho sem.</span><span class="sxs-lookup"><span data-stu-id="68db7-169">We recommend that you create a Storage account *specifically* for use with your Batch account, and select it here.</span></span> <span data-ttu-id="68db7-170">Podrobnosti o tom, jak vytvořit účet úložiště, najdete v části "Vytvoření účtu úložiště" v [účty Azure Storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="68db7-170">For details about how to create a storage account, see "Create a Storage account" in [About Azure Storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="68db7-171">Po vytvoření účtu úložiště, můžete pak propojit se vašeho účtu Batch pomocí **účet úložiště** okno.</span><span class="sxs-lookup"><span data-stu-id="68db7-171">After you've created a Storage account, you can then link it to your Batch account by using the **Storage Account** blade.</span></span>

> [!WARNING]
> <span data-ttu-id="68db7-172">Služba Batch používá Azure Storage k ukládání balíčky aplikací jako objekty BLOB bloku.</span><span class="sxs-lookup"><span data-stu-id="68db7-172">The Batch service uses Azure Storage to store your application packages as block blobs.</span></span> <span data-ttu-id="68db7-173">Jste [účtován jako normální] [ storage_pricing] pro data objektů blob bloku.</span><span class="sxs-lookup"><span data-stu-id="68db7-173">You are [charged as normal][storage_pricing] for the block blob data.</span></span> <span data-ttu-id="68db7-174">Je nutné vzít v úvahu velikost a počet balíčky aplikací a pravidelně odstraňuje zastaralá balíčky, chcete-li minimalizovat náklady.</span><span class="sxs-lookup"><span data-stu-id="68db7-174">Be sure to consider the size and number of your application packages, and periodically remove deprecated packages to minimize costs.</span></span>
> 
> 

### <a name="view-current-applications"></a><span data-ttu-id="68db7-175">Zobrazit aktuální aplikace</span><span class="sxs-lookup"><span data-stu-id="68db7-175">View current applications</span></span>
<span data-ttu-id="68db7-176">Chcete-li zobrazit aplikace v účtu Batch, klikněte na tlačítko **aplikace** položky nabídky v levé nabídce při prohlížení **účet Batch** okno.</span><span class="sxs-lookup"><span data-stu-id="68db7-176">To view the applications in your Batch account, click the **Applications** menu item in the left menu while viewing the **Batch account** blade.</span></span>

<span data-ttu-id="68db7-177">![Dlaždice aplikace][2]</span><span class="sxs-lookup"><span data-stu-id="68db7-177">![Applications tile][2]</span></span>

<span data-ttu-id="68db7-178">Výběrem této možnosti nabídky otevře **aplikace** okno:</span><span class="sxs-lookup"><span data-stu-id="68db7-178">Selecting this menu option opens the **Applications** blade:</span></span>

<span data-ttu-id="68db7-179">![Seznam aplikací][3]</span><span class="sxs-lookup"><span data-stu-id="68db7-179">![List applications][3]</span></span>

<span data-ttu-id="68db7-180">**Aplikace** ID každé aplikace zobrazuje v účtu a následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="68db7-180">The **Applications** blade displays the ID of each application in your account and the following properties:</span></span>

* <span data-ttu-id="68db7-181">**Balíčky**: číslo verze přidružené k této aplikaci.</span><span class="sxs-lookup"><span data-stu-id="68db7-181">**Packages**: The number of versions associated with this application.</span></span>
* <span data-ttu-id="68db7-182">**Výchozí verze**: verze aplikace nainstalovat, pokud neuvedete na verzi, když zadáte aplikací pro fond.</span><span class="sxs-lookup"><span data-stu-id="68db7-182">**Default version**: The application version installed if you do not indicate a version when you specify the application for a pool.</span></span> <span data-ttu-id="68db7-183">Toto nastavení je volitelné.</span><span class="sxs-lookup"><span data-stu-id="68db7-183">This setting is optional.</span></span>
* <span data-ttu-id="68db7-184">**Povolit aktualizace**: hodnota, která určuje, zda balíček aktualizace, odstranění a přidání jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="68db7-184">**Allow updates**: The value that specifies whether package updates, deletions, and additions are allowed.</span></span> <span data-ttu-id="68db7-185">Pokud je nastavena v **ne**, balíček aktualizace a odstranění jsou zakázány pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="68db7-185">If this is set to **No**, package updates and deletions are disabled for the application.</span></span> <span data-ttu-id="68db7-186">Můžete přidat pouze nové verze balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="68db7-186">Only new application package versions can be added.</span></span> <span data-ttu-id="68db7-187">Výchozí hodnota je **Ano**.</span><span class="sxs-lookup"><span data-stu-id="68db7-187">The default is **Yes**.</span></span>

### <a name="view-application-details"></a><span data-ttu-id="68db7-188">Zobrazení podrobností o aplikaci</span><span class="sxs-lookup"><span data-stu-id="68db7-188">View application details</span></span>
<span data-ttu-id="68db7-189">Otevřete okno, které zahrnuje podrobné informace pro aplikaci, vyberte aplikaci v **aplikace** okno.</span><span class="sxs-lookup"><span data-stu-id="68db7-189">To open the blade that includes the details for an application, select the application in the **Applications** blade.</span></span>

<span data-ttu-id="68db7-190">![Podrobnosti o aplikaci][4]</span><span class="sxs-lookup"><span data-stu-id="68db7-190">![Application details][4]</span></span>

<span data-ttu-id="68db7-191">V okně podrobností aplikace můžete nakonfigurovat následující nastavení pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="68db7-191">In the application details blade, you can configure the following settings for your application.</span></span>

* <span data-ttu-id="68db7-192">**Povolit aktualizace**: Určete, zda jeho balíčky aplikací můžete aktualizovat nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="68db7-192">**Allow updates**: Specify whether its application packages can be updated or deleted.</span></span> <span data-ttu-id="68db7-193">Později v tomto článku najdete v části "Aktualizace nebo odstranění balíčku aplikace".</span><span class="sxs-lookup"><span data-stu-id="68db7-193">See "Update or Delete an application package" later in this article.</span></span>
* <span data-ttu-id="68db7-194">**Výchozí verze**: Zadejte výchozí balíček aplikace pro nasazení na výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="68db7-194">**Default version**: Specify a default application package to deploy to compute nodes.</span></span>
* <span data-ttu-id="68db7-195">**Zobrazovaný název**: Zadejte popisný název, který vaše řešení Batch můžete použít při zobrazuje informace o aplikaci, například v uživatelském rozhraní služby, která vám umožní vašim zákazníkům prostřednictvím Batch poskytovat.</span><span class="sxs-lookup"><span data-stu-id="68db7-195">**Display name**: Specify a friendly name that your Batch solution can use when it displays information about the application, for example, in the UI of a service that you provide to your customers through Batch.</span></span>

### <a name="add-a-new-application"></a><span data-ttu-id="68db7-196">Přidejte novou aplikaci</span><span class="sxs-lookup"><span data-stu-id="68db7-196">Add a new application</span></span>
<span data-ttu-id="68db7-197">Chcete-li vytvořit novou aplikaci, přidejte balíček aplikace a zadejte ID aplikace nové, jedinečné.</span><span class="sxs-lookup"><span data-stu-id="68db7-197">To create a new application, add an application package and specify a new, unique application ID.</span></span> <span data-ttu-id="68db7-198">První balíček aplikace, který přidáte s novým ID aplikace také vytvoří novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="68db7-198">The first application package that you add with the new application ID also creates the new application.</span></span>

<span data-ttu-id="68db7-199">Klikněte na tlačítko **přidat** na **aplikace** otevřete **novou aplikaci** okno.</span><span class="sxs-lookup"><span data-stu-id="68db7-199">Click **Add** on the **Applications** blade to open the **New application** blade.</span></span>

<span data-ttu-id="68db7-200">![Nové okno aplikace na portálu Azure][5]</span><span class="sxs-lookup"><span data-stu-id="68db7-200">![New application blade in Azure portal][5]</span></span>

<span data-ttu-id="68db7-201">**Novou aplikaci** okno obsahuje následující pole k zadání nastavení pro novou aplikaci a balíček aplikace.</span><span class="sxs-lookup"><span data-stu-id="68db7-201">The **New application** blade provides the following fields to specify the settings of your new application and application package.</span></span>

<span data-ttu-id="68db7-202">**Id aplikace**</span><span class="sxs-lookup"><span data-stu-id="68db7-202">**Application id**</span></span>

<span data-ttu-id="68db7-203">Toto pole určuje ID novou aplikaci, která je předmětem standardní pravidla ověřování ID dávky Azure.</span><span class="sxs-lookup"><span data-stu-id="68db7-203">This field specifies the ID of your new application, which is subject to the standard Azure Batch ID validation rules.</span></span> <span data-ttu-id="68db7-204">Pravidla pro zajištění ID aplikací jsou následující:</span><span class="sxs-lookup"><span data-stu-id="68db7-204">The rules for providing an application ID are as follows:</span></span>

* <span data-ttu-id="68db7-205">Na Windows se ID může obsahovat libovolnou kombinaci alfanumerických znaků, pomlčky a podtržítka.</span><span class="sxs-lookup"><span data-stu-id="68db7-205">On Windows nodes, the ID can contain any combination of alphanumeric characters, hyphens, and underscores.</span></span> <span data-ttu-id="68db7-206">Na uzlech Linux jsou povoleny pouze alfanumerické znaky a podtržítka.</span><span class="sxs-lookup"><span data-stu-id="68db7-206">On Linux nodes, only alphanumeric characters and underscores are permitted.</span></span>
* <span data-ttu-id="68db7-207">Nesmí obsahovat víc než 64 znaků.</span><span class="sxs-lookup"><span data-stu-id="68db7-207">Cannot contain more than 64 characters.</span></span>
* <span data-ttu-id="68db7-208">Musí být jedinečný v rámci účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="68db7-208">Must be unique within the Batch account.</span></span>
* <span data-ttu-id="68db7-209">Je zachována a velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="68db7-209">Is case-preserving and case-insensitive.</span></span>

<span data-ttu-id="68db7-210">**Verze**</span><span class="sxs-lookup"><span data-stu-id="68db7-210">**Version**</span></span>

<span data-ttu-id="68db7-211">Toto pole určuje verzi balíčku aplikace, které odesíláte.</span><span class="sxs-lookup"><span data-stu-id="68db7-211">This field specifies the version of the application package you are uploading.</span></span> <span data-ttu-id="68db7-212">Řetězce verzi se vztahují následující pravidla ověřování:</span><span class="sxs-lookup"><span data-stu-id="68db7-212">Version strings are subject to the following validation rules:</span></span>

* <span data-ttu-id="68db7-213">Na uzlech Windows řetězec verze může obsahovat libovolnou kombinaci alfanumerických znaků, pomlčky, podtržítka a tečky.</span><span class="sxs-lookup"><span data-stu-id="68db7-213">On Windows nodes, the version string can contain any combination of alphanumeric characters, hyphens, underscores, and periods.</span></span> <span data-ttu-id="68db7-214">Na uzlech Linux řetězec verze může obsahovat pouze alfanumerické znaky a podtržítka.</span><span class="sxs-lookup"><span data-stu-id="68db7-214">On Linux nodes, the version string can contain only alphanumeric characters and underscores.</span></span>
* <span data-ttu-id="68db7-215">Nesmí obsahovat víc než 64 znaků.</span><span class="sxs-lookup"><span data-stu-id="68db7-215">Cannot contain more than 64 characters.</span></span>
* <span data-ttu-id="68db7-216">Musí být jedinečný v rámci aplikace.</span><span class="sxs-lookup"><span data-stu-id="68db7-216">Must be unique within the application.</span></span>
* <span data-ttu-id="68db7-217">Jsou zachována a velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="68db7-217">Are case-preserving and case-insensitive.</span></span>

<span data-ttu-id="68db7-218">**Balíček aplikace**</span><span class="sxs-lookup"><span data-stu-id="68db7-218">**Application package**</span></span>

<span data-ttu-id="68db7-219">Toto pole určuje soubor .zip, který obsahuje binární soubory aplikace a podpůrné soubory, které jsou nutné ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="68db7-219">This field specifies the .zip file that contains the application binaries and supporting files that are required to execute the application.</span></span> <span data-ttu-id="68db7-220">Klikněte **vyberte soubor** pole nebo na ikonu složky a vyhledejte a vyberte soubor .zip, který obsahuje soubory aplikace.</span><span class="sxs-lookup"><span data-stu-id="68db7-220">Click the **Select a file** box or the folder icon to browse to and select a .zip file that contains your application's files.</span></span>

<span data-ttu-id="68db7-221">Jakmile vyberete soubor, klikněte na tlačítko **OK** zahájíte nahrávání do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="68db7-221">After you've selected a file, click **OK** to begin the upload to Azure Storage.</span></span> <span data-ttu-id="68db7-222">Po dokončení operace odesílání na portálu zobrazí oznámení a zavře okno.</span><span class="sxs-lookup"><span data-stu-id="68db7-222">When the upload operation is complete, the portal displays a notification and closes the blade.</span></span> <span data-ttu-id="68db7-223">V závislosti na rychlosti síťového připojení a velikost souboru, který odesíláte může tato operace chvíli trvat.</span><span class="sxs-lookup"><span data-stu-id="68db7-223">Depending on the size of the file that you are uploading and the speed of your network connection, this operation may take some time.</span></span>

> [!WARNING]
> <span data-ttu-id="68db7-224">Nezavírejte okno **novou aplikaci** okno před dokončením operace odesílání.</span><span class="sxs-lookup"><span data-stu-id="68db7-224">Do not close the **New application** blade before the upload operation is complete.</span></span> <span data-ttu-id="68db7-225">Díky tomu bude zastavení procesu nahrávání.</span><span class="sxs-lookup"><span data-stu-id="68db7-225">Doing so will stop the upload process.</span></span>
> 
> 

### <a name="add-a-new-application-package"></a><span data-ttu-id="68db7-226">Přidat nový balíček aplikace</span><span class="sxs-lookup"><span data-stu-id="68db7-226">Add a new application package</span></span>
<span data-ttu-id="68db7-227">Přidat novou verzi balíčku aplikace pro existující aplikace, vyberte aplikaci v **aplikace** okně klikněte na tlačítko **balíčky**, pak klikněte na tlačítko **přidat** otevřete **přidat balíček** okno.</span><span class="sxs-lookup"><span data-stu-id="68db7-227">To add a new application package version for an existing application, select an application in the **Applications** blade, click **Packages**, then click **Add** to open the **Add package** blade.</span></span>

<span data-ttu-id="68db7-228">![Přidejte balíček okna aplikací na portálu Azure][8]</span><span class="sxs-lookup"><span data-stu-id="68db7-228">![Add application package blade in Azure portal][8]</span></span>

<span data-ttu-id="68db7-229">Jak vidíte, se pole shodují těch, které **novou aplikaci** okno, ale **id aplikace** pole je zakázána.</span><span class="sxs-lookup"><span data-stu-id="68db7-229">As you can see, the fields match those of the **New application** blade, but the **Application id** box is disabled.</span></span> <span data-ttu-id="68db7-230">Stejně jako u nové aplikace, zadejte **verze** pro nový balíček, přejděte do vaší **balíčku aplikace** .zip souboru a pak klikněte na **OK** pro nahrání balíčku.</span><span class="sxs-lookup"><span data-stu-id="68db7-230">As you did for the new application, specify the **Version** for your new package, browse to your **Application package** .zip file, then click **OK** to upload the package.</span></span>

### <a name="update-or-delete-an-application-package"></a><span data-ttu-id="68db7-231">Aktualizace nebo odstranění balíčku aplikace</span><span class="sxs-lookup"><span data-stu-id="68db7-231">Update or delete an application package</span></span>
<span data-ttu-id="68db7-232">Aktualizovat nebo odstranit existující balíček aplikace, otevřete okno Podrobnosti pro aplikace, klikněte na tlačítko **balíčky** otevřete **balíčky** okně klikněte na tlačítko **třemi tečkami** v řádku balíček aplikace, který chcete upravit a vyberte akci, kterou chcete provést.</span><span class="sxs-lookup"><span data-stu-id="68db7-232">To update or delete an existing application package, open the details blade for the application, click **Packages** to open the **Packages** blade, click the **ellipsis** in the row of the application package that you want to modify, and select the action that you want to perform.</span></span>

<span data-ttu-id="68db7-233">![Aktualizace nebo odstranění balíčku na portálu Azure][7]</span><span class="sxs-lookup"><span data-stu-id="68db7-233">![Update or delete package in Azure portal][7]</span></span>

<span data-ttu-id="68db7-234">**Aktualizace**</span><span class="sxs-lookup"><span data-stu-id="68db7-234">**Update**</span></span>

<span data-ttu-id="68db7-235">Když kliknete na tlačítko **aktualizace**, *balíček aktualizace* zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="68db7-235">When you click **Update**, the *Update package* blade is displayed.</span></span> <span data-ttu-id="68db7-236">Toto okno je podobná *nový balíček aplikace* okno, ale pouze pole výběr balíčku je povoleno, umožní vám to zadat nový soubor ZIP k odeslání.</span><span class="sxs-lookup"><span data-stu-id="68db7-236">This blade is similar to the *New application package* blade, however only the package selection field is enabled, allowing you to specify a new ZIP file to upload.</span></span>

<span data-ttu-id="68db7-237">![Okno balíček aktualizace na portálu Azure][11]</span><span class="sxs-lookup"><span data-stu-id="68db7-237">![Update package blade in Azure portal][11]</span></span>

<span data-ttu-id="68db7-238">**Odstranění**</span><span class="sxs-lookup"><span data-stu-id="68db7-238">**Delete**</span></span>

<span data-ttu-id="68db7-239">Když kliknete na tlačítko **odstranit**, se zobrazí výzva k potvrzení odstranění verze balíčku a Batch odstraní balíček z úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="68db7-239">When you click **Delete**, you are asked to confirm the deletion of the package version, and Batch deletes the package from Azure Storage.</span></span> <span data-ttu-id="68db7-240">Pokud odstraníte výchozí verze aplikace, **výchozí verze** nastavení se odebere pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="68db7-240">If you delete the default version of an application, the **Default version** setting is removed for the application.</span></span>

<span data-ttu-id="68db7-241">![Odstranit aplikaci][12]</span><span class="sxs-lookup"><span data-stu-id="68db7-241">![Delete application ][12]</span></span>

## <a name="install-applications-on-compute-nodes"></a><span data-ttu-id="68db7-242">Instalace aplikací na výpočetní uzly</span><span class="sxs-lookup"><span data-stu-id="68db7-242">Install applications on compute nodes</span></span>
<span data-ttu-id="68db7-243">Teď, když jste se naučili jak spravovat balíčky aplikací pomocí portálu Azure, můžete pojednává o je nasadit na výpočetní uzly a jejich spuštění s úkoly služby Batch.</span><span class="sxs-lookup"><span data-stu-id="68db7-243">Now that you've learned how to manage application packages with the Azure portal, we can discuss how to deploy them to compute nodes and run them with Batch tasks.</span></span>

### <a name="install-pool-application-packages"></a><span data-ttu-id="68db7-244">Instalovat balíčky fondu aplikací</span><span class="sxs-lookup"><span data-stu-id="68db7-244">Install pool application packages</span></span>
<span data-ttu-id="68db7-245">Balíček aplikace nainstalovat na všechny výpočetní uzly ve fondu, zadejte balíček aplikace jeden nebo více *odkazy* pro fond.</span><span class="sxs-lookup"><span data-stu-id="68db7-245">To install an application package on all compute nodes in a pool, specify one or more application package *references* for the pool.</span></span> <span data-ttu-id="68db7-246">Balíčky aplikací, které zadáte pro fond jsou nainstalované na každém výpočetním uzlu, pokud tento uzel připojí k fondu a pokud je uzel restartován nebo obnovit z Image.</span><span class="sxs-lookup"><span data-stu-id="68db7-246">The application packages that you specify for a pool are installed on each compute node when that node joins the pool, and when the node is rebooted or reimaged.</span></span>

<span data-ttu-id="68db7-247">V Batch .NET, zadejte jednu nebo více [CloudPool][net_cloudpool].[ ApplicationPackageReferences] [ net_cloudpool_pkgref] při vytvoření nového fondu, nebo pro existující fond.</span><span class="sxs-lookup"><span data-stu-id="68db7-247">In Batch .NET, specify one or more [CloudPool][net_cloudpool].[ApplicationPackageReferences][net_cloudpool_pkgref] when you create a new pool, or for an existing pool.</span></span> <span data-ttu-id="68db7-248">[ApplicationPackageReference] [ net_pkgref] třída určuje ID aplikace a verzi, instalace na vytvoření fondu výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="68db7-248">The [ApplicationPackageReference][net_pkgref] class specifies an application ID and version to install on a pool's compute nodes.</span></span>

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicatedComputeNodes: 1,
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package is installed on each.
await myCloudPool.CommitAsync();
```

> [!IMPORTANT]
> <span data-ttu-id="68db7-249">Pokud z nějakého důvodu selže nasazení balíček aplikace, služba Batch označí uzlu [nepoužitelná][net_nodestate], a žádné úlohy jsou naplánovány pro spuštění v tomto uzlu.</span><span class="sxs-lookup"><span data-stu-id="68db7-249">If an application package deployment fails for any reason, the Batch service marks the node [unusable][net_nodestate], and no tasks are scheduled for execution on that node.</span></span> <span data-ttu-id="68db7-250">V takovém případě byste měli **restartujte** uzel k nasazení balíčku provést znovu.</span><span class="sxs-lookup"><span data-stu-id="68db7-250">In this case, you should **restart** the node to reinitiate the package deployment.</span></span> <span data-ttu-id="68db7-251">Restartování uzlu taky umožňuje znovu plánování úkolů na uzlu.</span><span class="sxs-lookup"><span data-stu-id="68db7-251">Restarting the node also enables task scheduling again on the node.</span></span>
> 
> 

### <a name="install-task-application-packages"></a><span data-ttu-id="68db7-252">Instalovat balíčky aplikace úkolů</span><span class="sxs-lookup"><span data-stu-id="68db7-252">Install task application packages</span></span>
<span data-ttu-id="68db7-253">Podobně jako u fondu, zadejte balíček aplikace *odkazy* pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="68db7-253">Similar to a pool, you specify application package *references* for a task.</span></span> <span data-ttu-id="68db7-254">Když je naplánován ke spuštění na uzel, balíčku stažena a extrahována těsně před příkazový řádek úkolu je spustí.</span><span class="sxs-lookup"><span data-stu-id="68db7-254">When a task is scheduled to run on a node, the package is downloaded and extracted just before the task's command line is executed.</span></span> <span data-ttu-id="68db7-255">Pokud zadaného balíčku a verze je už nainstalovaný na uzlu, není-li stáhnout balíček a se používá existující balíček.</span><span class="sxs-lookup"><span data-stu-id="68db7-255">If a specified package and version is already installed on the node, the package is not downloaded and the existing package is used.</span></span>

<span data-ttu-id="68db7-256">Chcete-li nainstalovat balíček aplikace úkolů, nakonfigurujte úkolu [CloudTask][net_cloudtask].[ ApplicationPackageReferences] [ net_cloudtask_pkgref] vlastnost:</span><span class="sxs-lookup"><span data-stu-id="68db7-256">To install a task application package, configure the task's [CloudTask][net_cloudtask].[ApplicationPackageReferences][net_cloudtask_pkgref] property:</span></span>

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-the-installed-applications"></a><span data-ttu-id="68db7-257">Spuštění nainstalovaných aplikací</span><span class="sxs-lookup"><span data-stu-id="68db7-257">Execute the installed applications</span></span>
<span data-ttu-id="68db7-258">Balíčky, které jste určili pro fond nebo úloh jsou stažené a rozbalené na adresář s názvem v rámci `AZ_BATCH_ROOT_DIR` uzlu.</span><span class="sxs-lookup"><span data-stu-id="68db7-258">The packages that you've specified for a pool or task are downloaded and extracted to a named directory within the `AZ_BATCH_ROOT_DIR` of the node.</span></span> <span data-ttu-id="68db7-259">Batch vytvoří také proměnná prostředí, který obsahuje cestu k adresáři s názvem.</span><span class="sxs-lookup"><span data-stu-id="68db7-259">Batch also creates an environment variable that contains the path to the named directory.</span></span> <span data-ttu-id="68db7-260">Vaše příkazové řádky úkolu pomocí této proměnné prostředí při odkazování na aplikace na uzlu.</span><span class="sxs-lookup"><span data-stu-id="68db7-260">Your task command lines use this environment variable when referencing the application on the node.</span></span> 

<span data-ttu-id="68db7-261">V uzlech systému Windows je proměnná v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="68db7-261">On Windows nodes, the variable is in the following format:</span></span>

```
Windows:
AZ_BATCH_APP_PACKAGE_APPLICATIONID#version
```

<span data-ttu-id="68db7-262">Na Linuxových uzlů formát se mírně liší.</span><span class="sxs-lookup"><span data-stu-id="68db7-262">On Linux nodes, the format is slightly different.</span></span> <span data-ttu-id="68db7-263">Tečky (.) a pomlčky (-) a tyto znaky (#), se sloučí na podtržené v proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="68db7-263">Periods (.), hypens (-) and number signs (#) are flattened to underscores in the environment variable.</span></span> <span data-ttu-id="68db7-264">Například:</span><span class="sxs-lookup"><span data-stu-id="68db7-264">For example:</span></span>

```
Linux:
AZ_BATCH_APP_PACKAGE_APPLICATIONID_version
```

<span data-ttu-id="68db7-265">`APPLICATIONID`a `version` jsou hodnoty, které odpovídají verzi aplikací a balíčků, jste určili pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="68db7-265">`APPLICATIONID` and `version` are values that correspond to the application and package version you've specified for deployment.</span></span> <span data-ttu-id="68db7-266">Například pokud jste zadali, že verze 2.7 aplikace *digestoru* by měly být nainstalovány na uzlech Windows by vaše příkazové řádky úkolu pomocí této proměnné prostředí pro přístup k jeho soubory:</span><span class="sxs-lookup"><span data-stu-id="68db7-266">For example, if you specified that version 2.7 of application *blender* should be installed on Windows nodes, your task command lines would use this environment variable to access its files:</span></span>

```
Windows:
AZ_BATCH_APP_PACKAGE_BLENDER#2.7
```

<span data-ttu-id="68db7-267">Na Linuxových uzlů zadejte proměnné prostředí v tomto formátu:</span><span class="sxs-lookup"><span data-stu-id="68db7-267">On Linux nodes, specify the environment variable in this format:</span></span>

```
Linux:
AZ_BATCH_APP_PACKAGE_BLENDER_2_7
``` 

<span data-ttu-id="68db7-268">Při nahrávání balíčku aplikace, můžete zadat výchozí verze pro nasazení na výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="68db7-268">When you upload an application package, you can specify a default version to deploy to your compute nodes.</span></span> <span data-ttu-id="68db7-269">Pokud jste zadali výchozí verze pro aplikaci, můžete vynechat přípony verze při odkazu na aplikaci.</span><span class="sxs-lookup"><span data-stu-id="68db7-269">If you have specified a default version for an application, you can omit the version suffix when you reference the application.</span></span> <span data-ttu-id="68db7-270">V portálu Azure, v okně aplikace můžete určit výchozí verze aplikace, jak je znázorněno v [odesílat a spravovat aplikace](#upload-and-manage-applications).</span><span class="sxs-lookup"><span data-stu-id="68db7-270">You can specify the default application version in the Azure portal, on the Applications blade, as shown in [Upload and manage applications](#upload-and-manage-applications).</span></span>

<span data-ttu-id="68db7-271">Například pokud nastavíte jako výchozí verze pro aplikace "2.7" *digestoru*a vaše úkoly odkazování následující proměnné prostředí, pak uzlů Windows, budou spuštěny verze 2.7:</span><span class="sxs-lookup"><span data-stu-id="68db7-271">For example, if you set "2.7" as the default version for application *blender*, and your tasks reference the following environment variable, then your Windows nodes will execute version 2.7:</span></span>

`AZ_BATCH_APP_PACKAGE_BLENDER`

<span data-ttu-id="68db7-272">Následující fragment kódu ukazuje příklad úloh příkazový řádek, který spustí výchozí verzi *digestoru* aplikace:</span><span class="sxs-lookup"><span data-stu-id="68db7-272">The following code snippet shows an example task command line that launches the default version of the *blender* application:</span></span>

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [!TIP]
> <span data-ttu-id="68db7-273">V tématu [nastavení prostředí pro úkoly](batch-api-basics.md#environment-settings-for-tasks) v [přehled funkcí Batch](batch-api-basics.md) Další informace o nastavení prostředí výpočetního uzlu.</span><span class="sxs-lookup"><span data-stu-id="68db7-273">See [Environment settings for tasks](batch-api-basics.md#environment-settings-for-tasks) in the [Batch feature overview](batch-api-basics.md) for more information about compute node environment settings.</span></span>
> 
> 

## <a name="update-a-pools-application-packages"></a><span data-ttu-id="68db7-274">Aktualizace balíčků aplikací fondu</span><span class="sxs-lookup"><span data-stu-id="68db7-274">Update a pool's application packages</span></span>
<span data-ttu-id="68db7-275">Pokud se balíček aplikace již byla nakonfigurována existujícího fondu, můžete zadat nový balíček pro fond.</span><span class="sxs-lookup"><span data-stu-id="68db7-275">If an existing pool has already been configured with an application package, you can specify a new package for the pool.</span></span> <span data-ttu-id="68db7-276">Pokud zadáte nový odkaz na balíček pro fond následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="68db7-276">If you specify a new package reference for a pool, the following apply:</span></span>

* <span data-ttu-id="68db7-277">Služba Batch nainstaluje nově zadaný balíček na všechny nové uzly, které se připojí k fondu a na všechny existující uzlu, který je restartovat nebo obnovit z Image.</span><span class="sxs-lookup"><span data-stu-id="68db7-277">The Batch service installs the newly specified package on all new nodes that join the pool and on any existing node that is rebooted or reimaged.</span></span>
* <span data-ttu-id="68db7-278">Výpočetní uzly, které jsou již ve fondu, když aktualizujete balíček odkazuje automaticky neinstaluje nového balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="68db7-278">Compute nodes that are already in the pool when you update the package references do not automatically install the new application package.</span></span> <span data-ttu-id="68db7-279">Tyto výpočetní uzly musí být restartovat nebo obnovit z Image pro příjem nového balíčku.</span><span class="sxs-lookup"><span data-stu-id="68db7-279">These compute nodes must be rebooted or reimaged to receive the new package.</span></span>
* <span data-ttu-id="68db7-280">Po nasazení nového balíčku projeví proměnné vytvořené prostředí balíček odkazuje nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="68db7-280">When a new package is deployed, the created environment variables reflect the new application package references.</span></span>

<span data-ttu-id="68db7-281">V tomto příkladu má existující fond verzi 2.7 *digestoru* aplikace konfigurovaná jako jeden z jeho [CloudPool][net_cloudpool].[ ApplicationPackageReferences][net_cloudpool_pkgref].</span><span class="sxs-lookup"><span data-stu-id="68db7-281">In this example, the existing pool has version 2.7 of the *blender* application configured as one of its [CloudPool][net_cloudpool].[ApplicationPackageReferences][net_cloudpool_pkgref].</span></span> <span data-ttu-id="68db7-282">Pokud chcete aktualizovat uzly fondu s verzí 2.76b, zadejte nový [ApplicationPackageReference] [ net_pkgref] s novou verzí a potvrzení změn.</span><span class="sxs-lookup"><span data-stu-id="68db7-282">To update the pool's nodes with version 2.76b, specify a new [ApplicationPackageReference][net_pkgref] with the new version, and commit the change.</span></span>

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

<span data-ttu-id="68db7-283">Teď, když byl nakonfigurován na novou verzi, služba Batch nainstaluje do jakéhokoli verze 2.76b *nové* uzlu, který se připojí k fondu.</span><span class="sxs-lookup"><span data-stu-id="68db7-283">Now that the new version has been configured, the Batch service installs version 2.76b to any *new* node that joins the pool.</span></span> <span data-ttu-id="68db7-284">Chcete-li nainstalovat 2.76b na uzly, které jsou *již* ve fondu, restartovat nebo obnovit z Image je.</span><span class="sxs-lookup"><span data-stu-id="68db7-284">To install 2.76b on the nodes that are *already* in the pool, reboot or reimage them.</span></span> <span data-ttu-id="68db7-285">Všimněte si, že restartovaný uzly zachovat soubory z předchozích nasazení balíčku.</span><span class="sxs-lookup"><span data-stu-id="68db7-285">Note that rebooted nodes retain the files from previous package deployments.</span></span>

## <a name="list-the-applications-in-a-batch-account"></a><span data-ttu-id="68db7-286">Zobrazí seznam aplikací v účtu Batch</span><span class="sxs-lookup"><span data-stu-id="68db7-286">List the applications in a Batch account</span></span>
<span data-ttu-id="68db7-287">Můžete vytvořit seznam aplikací a jejich balíčků v účtu Batch pomocí [ApplicationOperations][net_appops].[ ListApplicationSummaries] [ net_appops_listappsummaries] metoda.</span><span class="sxs-lookup"><span data-stu-id="68db7-287">You can list the applications and their packages in a Batch account by using the [ApplicationOperations][net_appops].[ListApplicationSummaries][net_appops_listappsummaries] method.</span></span>

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a><span data-ttu-id="68db7-288">Zabalení</span><span class="sxs-lookup"><span data-stu-id="68db7-288">Wrap up</span></span>
<span data-ttu-id="68db7-289">Pomocí balíčků aplikací můžete pomoct vašim zákazníkům vyberte aplikace, pro svou práci a určete přesnou verze se má použít při zpracování úlohy se služby podporují službu Batch.</span><span class="sxs-lookup"><span data-stu-id="68db7-289">With application packages, you can help your customers select the applications for their jobs and specify the exact version to use when processing jobs with your Batch-enabled service.</span></span> <span data-ttu-id="68db7-290">Můžete zadat také možnost pro vaše zákazníky k odesílání a sledování vlastních aplikací ve službě.</span><span class="sxs-lookup"><span data-stu-id="68db7-290">You might also provide the ability for your customers to upload and track their own applications in your service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="68db7-291">Další kroky</span><span class="sxs-lookup"><span data-stu-id="68db7-291">Next steps</span></span>
* <span data-ttu-id="68db7-292">[Batch REST API] [ api_rest] taky poskytuje podporu pro práci s balíčky aplikací.</span><span class="sxs-lookup"><span data-stu-id="68db7-292">The [Batch REST API][api_rest] also provides support to work with application packages.</span></span> <span data-ttu-id="68db7-293">Například najdete v článku [applicationPackageReferences] [ rest_add_pool_with_packages] element v [přidat fond na účet] [ rest_add_pool] informace o tom, jak zadat balíčků pro instalaci pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="68db7-293">For example, see the [applicationPackageReferences][rest_add_pool_with_packages] element in [Add a pool to an account][rest_add_pool] for information about how to specify packages to install by using the REST API.</span></span> <span data-ttu-id="68db7-294">V tématu [aplikace] [ rest_applications] podrobnosti o tom, jak získat informace o aplikaci pomocí rozhraní REST API služby Batch.</span><span class="sxs-lookup"><span data-stu-id="68db7-294">See [Applications][rest_applications] for details about how to obtain application information by using the Batch REST API.</span></span>
* <span data-ttu-id="68db7-295">Zjistěte, jak programově [spravovat účty Azure Batch a kvóty pomocí rozhraní Batch Management .NET](batch-management-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="68db7-295">Learn how to programmatically [manage Azure Batch accounts and quotas with Batch Management .NET](batch-management-dotnet.md).</span></span> <span data-ttu-id="68db7-296">[Rozhraní Batch Management .NET][api_net_mgmt] knihovny můžete povolit funkce vytváření a odstraňování účtu Batch aplikace nebo služby.</span><span class="sxs-lookup"><span data-stu-id="68db7-296">The [Batch Management .NET][api_net_mgmt] library can enable account creation and deletion features for your Batch application or service.</span></span>

[api_net]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/client?view=azure-dotnet
[api_net_mgmt]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/management?view=azure-dotnet
[api_rest]: https://docs.microsoft.com/en-us/rest/api/batchservice/
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Vysokoúrovňový diagram balíčky aplikací"
[2]: ./media/batch-application-packages/app_pkg_02.png "Dlaždice aplikace na portálu Azure"
[3]: ./media/batch-application-packages/app_pkg_03.png "Okno aplikace na portálu Azure"
[4]: ./media/batch-application-packages/app_pkg_04.png "Okno podrobností aplikace v portálu Azure"
[5]: ./media/batch-application-packages/app_pkg_05.png "Nové okno aplikace na portálu Azure"
[7]: ./media/batch-application-packages/app_pkg_07.png "Aktualizace nebo odstranění balíčků rozevírací seznam v portálu Azure"
[8]: ./media/batch-application-packages/app_pkg_08.png "Okno nový balíček aplikace na portálu Azure"
[9]: ./media/batch-application-packages/app_pkg_09.png "Žádná propojené výstraha účtu úložiště"
[10]: ./media/batch-application-packages/app_pkg_10.png "Zvolte okně účtu úložiště na portálu Azure"
[11]: ./media/batch-application-packages/app_pkg_11.png "Okno balíček aktualizace na portálu Azure"
[12]: ./media/batch-application-packages/app_pkg_12.png "Odstranit balíček dialogové okno potvrzení na portálu Azure"
