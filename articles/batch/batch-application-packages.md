---
title: "aaaInstall balíčky aplikací na výpočetní uzly - Azure Batch | Microsoft Docs"
description: "Použití hello aplikace balíčky funkcí Azure Batch tooeasily spravovat více aplikací a verze pro instalaci na Batch výpočetních uzlů."
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
ms.openlocfilehash: 683be7b7f1bd5db7835332016f6dccb72f45c3b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-applications-toocompute-nodes-with-batch-application-packages"></a><span data-ttu-id="74aa9-103">Nasaďte uzly toocompute aplikací pomocí balíčků aplikací Batch</span><span class="sxs-lookup"><span data-stu-id="74aa9-103">Deploy applications toocompute nodes with Batch application packages</span></span>

<span data-ttu-id="74aa9-104">Funkce balíčky aplikace Hello služby Azure Batch poskytuje snadnou správu úloh aplikací a jejich nasazení toohello výpočetní uzly ve fondu.</span><span class="sxs-lookup"><span data-stu-id="74aa9-104">hello application packages feature of Azure Batch provides easy management of task applications and their deployment toohello compute nodes in your pool.</span></span> <span data-ttu-id="74aa9-105">Pomocí balíčků aplikací můžete odeslat a spravovat více verzí hello aplikací, které vaše úkoly spouštět, včetně jejich podpůrné soubory.</span><span class="sxs-lookup"><span data-stu-id="74aa9-105">With application packages, you can upload and manage multiple versions of hello applications your tasks run, including their supporting files.</span></span> <span data-ttu-id="74aa9-106">Pak můžete automaticky nasadit jednu nebo více z těchto aplikací toohello výpočetní uzly ve fondu.</span><span class="sxs-lookup"><span data-stu-id="74aa9-106">You can then automatically deploy one or more of these applications toohello compute nodes in your pool.</span></span>

<span data-ttu-id="74aa9-107">V tomto článku se dozvíte, jak tooupload a správě balíčků aplikací v nástroji hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="74aa9-107">In this article, you will learn how tooupload and manage application packages in hello Azure portal.</span></span> <span data-ttu-id="74aa9-108">Potom se dozvíte, jak tooinstall je ve fondu na výpočetní uzly s hello [Batch .NET] [ api_net] knihovny.</span><span class="sxs-lookup"><span data-stu-id="74aa9-108">You will then learn how tooinstall them on a pool's compute nodes with hello [Batch .NET][api_net] library.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="74aa9-109">Balíčky aplikací jsou podporované ve všech fondech služby Batch vytvořených po 5. červenci 2017.</span><span class="sxs-lookup"><span data-stu-id="74aa9-109">Application packages are supported on all Batch pools created after 5 July 2017.</span></span> <span data-ttu-id="74aa9-110">Že jsou podporované v fondy Batch vytvořit až 10. března 2016 5 2017 července pouze v případě, že hello fondu byla vytvořena pomocí konfigurace cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="74aa9-110">They are supported on Batch pools created between 10 March 2016 and 5 July 2017 only if hello pool was created using a Cloud Service configuration.</span></span> <span data-ttu-id="74aa9-111">Fondy batch vytvořen předchozí too10. března 2016 nepodporují balíčky aplikací.</span><span class="sxs-lookup"><span data-stu-id="74aa9-111">Batch pools created prior too10 March 2016 do not support application packages.</span></span>
>
> <span data-ttu-id="74aa9-112">Hello rozhraní API pro vytváření a správě balíčků aplikací jsou součástí hello [rozhraní Batch Management .NET] [[api_net_mgmt]] knihovny.</span><span class="sxs-lookup"><span data-stu-id="74aa9-112">hello APIs for creating and managing application packages are part of hello [Batch Management .NET][[api_net_mgmt]] library.</span></span> <span data-ttu-id="74aa9-113">Hello rozhraní API pro instalaci balíčků aplikací na výpočetním uzlu jsou součástí hello [Batch .NET] [ api_net] knihovny.</span><span class="sxs-lookup"><span data-stu-id="74aa9-113">hello APIs for installing application packages on a compute node are part of hello [Batch .NET][api_net] library.</span></span>  
>
> <span data-ttu-id="74aa9-114">Funkce balíčky aplikace Hello zde popsané nahrazuje funkce aplikace Batch hello k dispozici v předchozích verzích služby hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-114">hello application packages feature described here supersedes hello Batch Apps feature available in previous versions of hello service.</span></span>
> 
> 

## <a name="application-package-requirements"></a><span data-ttu-id="74aa9-115">Požadavky na balíček aplikace</span><span class="sxs-lookup"><span data-stu-id="74aa9-115">Application package requirements</span></span>
<span data-ttu-id="74aa9-116">balíčky aplikací toouse, budete potřebovat příliš[propojení účtu Azure Storage](#link-a-storage-account) tooyour účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="74aa9-116">toouse application packages, you need too[link an Azure Storage account](#link-a-storage-account) tooyour Batch account.</span></span>

<span data-ttu-id="74aa9-117">Tato funkce byla zavedena v [Batch REST API] [ api_rest] verze 2015-12-01.2.2 a odpovídající hello [Batch .NET] [ api_net] knihovní verze 3.1.0.</span><span class="sxs-lookup"><span data-stu-id="74aa9-117">This feature was introduced in [Batch REST API][api_rest] version 2015-12-01.2.2 and hello corresponding [Batch .NET][api_net] library version 3.1.0.</span></span> <span data-ttu-id="74aa9-118">Doporučujeme vám, že vždy používáte nejnovější verzi rozhraní API hello při práci se službou Batch.</span><span class="sxs-lookup"><span data-stu-id="74aa9-118">We recommend that you always use hello latest API version when working with Batch.</span></span>

> [!NOTE]
> <span data-ttu-id="74aa9-119">Balíčky aplikací jsou podporované ve všech fondech služby Batch vytvořených po 5. červenci 2017.</span><span class="sxs-lookup"><span data-stu-id="74aa9-119">Application packages are supported on all Batch pools created after 5 July 2017.</span></span> <span data-ttu-id="74aa9-120">Že jsou podporované v fondy Batch vytvořit až 10. března 2016 5 2017 července pouze v případě, že hello fondu byla vytvořena pomocí konfigurace cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="74aa9-120">They are supported on Batch pools created between 10 March 2016 and 5 July 2017 only if hello pool was created using a Cloud Service configuration.</span></span> <span data-ttu-id="74aa9-121">Fondy batch vytvořen předchozí too10. března 2016 nepodporují balíčky aplikací.</span><span class="sxs-lookup"><span data-stu-id="74aa9-121">Batch pools created prior too10 March 2016 do not support application packages.</span></span>
>
>

## <a name="about-applications-and-application-packages"></a><span data-ttu-id="74aa9-122">O aplikacích a balíčky aplikací</span><span class="sxs-lookup"><span data-stu-id="74aa9-122">About applications and application packages</span></span>
<span data-ttu-id="74aa9-123">V rámci Azure Batch *aplikace* odkazuje tooa sadu verzí binární soubory, které se dají automaticky stažené toohello výpočetních uzlů ve fondu.</span><span class="sxs-lookup"><span data-stu-id="74aa9-123">Within Azure Batch, an *application* refers tooa set of versioned binaries that can be automatically downloaded toohello compute nodes in your pool.</span></span> <span data-ttu-id="74aa9-124">*Balíčku aplikace* odkazuje tooa *konkrétní sadu* těchto binárních souborů a představuje danou *verze* aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-124">An *application package* refers tooa *specific set* of those binaries and represents a given *version* of hello application.</span></span>

<span data-ttu-id="74aa9-125">![Vysokoúrovňový diagram aplikace a balíčky aplikací][1]</span><span class="sxs-lookup"><span data-stu-id="74aa9-125">![High-level diagram of applications and application packages][1]</span></span>

### <a name="applications"></a><span data-ttu-id="74aa9-126">Aplikace</span><span class="sxs-lookup"><span data-stu-id="74aa9-126">Applications</span></span>
<span data-ttu-id="74aa9-127">Aplikace ve službě Batch obsahuje jeden nebo více aplikací, balíčků a určuje možnosti konfigurace pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-127">An application in Batch contains one or more application packages and specifies configuration options for hello application.</span></span> <span data-ttu-id="74aa9-128">Aplikace můžete například zadat hello výchozí aplikace balíčku verze tooinstall na výpočetní uzly a zda může být jeho balíčky aktualizovat ani odstranit.</span><span class="sxs-lookup"><span data-stu-id="74aa9-128">For example, an application can specify hello default application package version tooinstall on compute nodes and whether its packages can be updated or deleted.</span></span>

### <a name="application-packages"></a><span data-ttu-id="74aa9-129">Balíčky aplikací</span><span class="sxs-lookup"><span data-stu-id="74aa9-129">Application packages</span></span>
<span data-ttu-id="74aa9-130">Balíček aplikace je soubor .zip, který obsahuje binární soubory aplikace hello a podpůrné soubory, které jsou požadovány pro vaše aplikace hello toorun úlohy.</span><span class="sxs-lookup"><span data-stu-id="74aa9-130">An application package is a .zip file that contains hello application binaries and supporting files that are required for your tasks toorun hello application.</span></span> <span data-ttu-id="74aa9-131">Každý balíček aplikace představuje určitou verzi aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-131">Each application package represents a specific version of hello application.</span></span>

<span data-ttu-id="74aa9-132">Můžete zadat balíčky aplikací na úrovních hello fondu a úloh.</span><span class="sxs-lookup"><span data-stu-id="74aa9-132">You can specify application packages at hello pool and task levels.</span></span> <span data-ttu-id="74aa9-133">Nejméně jeden z těchto balíčků a (volitelně) verzi můžete zadat při vytváření fondu nebo úloh.</span><span class="sxs-lookup"><span data-stu-id="74aa9-133">You can specify one or more of these packages and (optionally) a version when you create a pool or task.</span></span>

* <span data-ttu-id="74aa9-134">**Fond aplikací balíčky** nasazených příliš*každých* uzlu ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-134">**Pool application packages** are deployed too*every* node in hello pool.</span></span> <span data-ttu-id="74aa9-135">Jestliže se uzel připojí fondu, a, pokud je restartovat nebo obnovit z Image se aplikace nasadí.</span><span class="sxs-lookup"><span data-stu-id="74aa9-135">Applications are deployed when a node joins a pool, and when it is rebooted or reimaged.</span></span>
  
    <span data-ttu-id="74aa9-136">Balíčky fondu aplikací jsou vhodné, když všechny uzly v rámci fondu spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="74aa9-136">Pool application packages are appropriate when all nodes in a pool execute a job's tasks.</span></span> <span data-ttu-id="74aa9-137">Jeden nebo více balíčků aplikací můžete určit, když vytvoříte fond, a můžete přidat nebo aktualizovat existující fond balíčky.</span><span class="sxs-lookup"><span data-stu-id="74aa9-137">You can specify one or more application packages when you create a pool, and you can add or update an existing pool's packages.</span></span> <span data-ttu-id="74aa9-138">Pokud aktualizujete balíčky existující fond aplikací, je nutné restartovat nový balíček jeho uzly tooinstall hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-138">If you update an existing pool's application packages, you must restart its nodes tooinstall hello new package.</span></span>
* <span data-ttu-id="74aa9-139">**Úloha balíčky aplikací** nasazených jen tooa výpočetním uzlu naplánované toorun úlohy, právě před spuštěním příkazového řádku úkolu hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-139">**Task application packages** are deployed only tooa compute node scheduled toorun a task, just before running hello task's command line.</span></span> <span data-ttu-id="74aa9-140">Pokud hello zadaný balíček aplikace a verze, který již je na uzlu hello, není znovu nasazena a slouží hello existující balíček.</span><span class="sxs-lookup"><span data-stu-id="74aa9-140">If hello specified application package and version is already on hello node, it is not redeployed and hello existing package is used.</span></span>
  
    <span data-ttu-id="74aa9-141">Balíčky aplikací úloh jsou užitečné v sdílený fond prostředí, kde různé úlohy se spouštějí na jeden fond, a hello fondu se neodstraní po dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="74aa9-141">Task application packages are useful in shared-pool environments, where different jobs are run on one pool, and hello pool is not deleted when a job is completed.</span></span> <span data-ttu-id="74aa9-142">Pokud vaše úlohy má méně úloh než uzly ve fondu hello, balíčky aplikací úloh Minimalizovat přenos dat vzhledem k tomu, že je vaše aplikace nasazené toohello pouze uzly, které využívají úlohy.</span><span class="sxs-lookup"><span data-stu-id="74aa9-142">If your job has fewer tasks than nodes in hello pool, task application packages can minimize data transfer since your application is deployed only toohello nodes that run tasks.</span></span>
  
    <span data-ttu-id="74aa9-143">Další scénáře, které můžete využít balíčky aplikací úloh jsou úlohy, které spouštějí rozsáhlé aplikace, ale pouze několik úkolů.</span><span class="sxs-lookup"><span data-stu-id="74aa9-143">Other scenarios that can benefit from task application packages are jobs that run a large application, but for only a few tasks.</span></span> <span data-ttu-id="74aa9-144">Předběžné zpracování fáze nebo sloučení úlohy, kde je aplikace hello předzpracováním nebo sloučená těžký, může například těžit z pomocí balíčků aplikací úloh.</span><span class="sxs-lookup"><span data-stu-id="74aa9-144">For example, a pre-processing stage or a merge task, where hello pre-processing or merge application is heavyweight, may benefit from using task application packages.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="74aa9-145">Existují omezení počtu hello aplikace a balíčky aplikací v rámci účtu Batch a na velikost balíčku maximální aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-145">There are restrictions on hello number of applications and application packages within a Batch account and on hello maximum application package size.</span></span> <span data-ttu-id="74aa9-146">V tématu [kvóty a omezení pro hello služby Azure Batch](batch-quota-limit.md) podrobnosti o těchto omezeních.</span><span class="sxs-lookup"><span data-stu-id="74aa9-146">See [Quotas and limits for hello Azure Batch service](batch-quota-limit.md) for details about these limits.</span></span>
> 
> 

### <a name="benefits-of-application-packages"></a><span data-ttu-id="74aa9-147">Výhody balíčky aplikací</span><span class="sxs-lookup"><span data-stu-id="74aa9-147">Benefits of application packages</span></span>
<span data-ttu-id="74aa9-148">Balíčky aplikací můžete zjednodušit kód hello v řešení Batch a nižší hello režijní požadované toomanage hello aplikace, které běží vaše úkoly.</span><span class="sxs-lookup"><span data-stu-id="74aa9-148">Application packages can simplify hello code in your Batch solution and lower hello overhead required toomanage hello applications that your tasks run.</span></span>

<span data-ttu-id="74aa9-149">Pomocí balíčků aplikací váš fond spouštěcí úkol nemá toospecify dlouhý seznam tooinstall soubory jednotlivých prostředků na uzlech hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-149">With application packages, your pool's start task doesn't have toospecify a long list of individual resource files tooinstall on hello nodes.</span></span> <span data-ttu-id="74aa9-150">Nemáte toomanually spravovat více verzí soubory aplikace v Azure Storage nebo na uzly.</span><span class="sxs-lookup"><span data-stu-id="74aa9-150">You don't have toomanually manage multiple versions of your application files in Azure Storage, or on your nodes.</span></span> <span data-ttu-id="74aa9-151">A nepotřebujete tooworry o generování [adresy URL SAS](../storage/common/storage-dotnet-shared-access-signature-part-1.md) tooprovide přístup k souborům toohello ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="74aa9-151">And, you don't need tooworry about generating [SAS URLs](../storage/common/storage-dotnet-shared-access-signature-part-1.md) tooprovide access toohello files in your Storage account.</span></span> <span data-ttu-id="74aa9-152">Batch funguje hello pozadí pomocí balíčků aplikací Azure Storage toostore a nasadit je toocompute uzlů.</span><span class="sxs-lookup"><span data-stu-id="74aa9-152">Batch works in hello background with Azure Storage toostore application packages and deploy them toocompute nodes.</span></span>

> [!NOTE] 
> <span data-ttu-id="74aa9-153">Hello celková velikost spouštěcí úkol musí být menší než nebo rovna too32768 znaků, včetně zdrojových souborů a proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="74aa9-153">hello total size of a start task must be less than or equal too32768 characters, including resource files and environment variables.</span></span> <span data-ttu-id="74aa9-154">Pokud spouštěcí úkol překračuje tento limit, pak pomocí balíčků aplikací je jinou možnost.</span><span class="sxs-lookup"><span data-stu-id="74aa9-154">If your start task exceeds this limit, then using application packages is another option.</span></span> <span data-ttu-id="74aa9-155">Můžete také vytvořit komprimované archivu obsahující soubory prostředků, nahrajte ho jako objekt blob tooAzure úložiště a pak ho rozbalte z hello příkazový řádek spouštěcího úkolu.</span><span class="sxs-lookup"><span data-stu-id="74aa9-155">You can also create a zipped archive containing your resource files, upload it as a blob tooAzure Storage, and then unzip it from hello command line of your start task.</span></span> 
>
>

## <a name="upload-and-manage-applications"></a><span data-ttu-id="74aa9-156">Odesílat a spravovat aplikace</span><span class="sxs-lookup"><span data-stu-id="74aa9-156">Upload and manage applications</span></span>
<span data-ttu-id="74aa9-157">Můžete použít hello [portál Azure] [ portal] nebo hello [rozhraní Batch Management .NET](batch-management-dotnet.md) balíčky aplikací hello knihovně toomanage v účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="74aa9-157">You can use hello [Azure portal][portal] or hello [Batch Management .NET](batch-management-dotnet.md) library toomanage hello application packages in your Batch account.</span></span> <span data-ttu-id="74aa9-158">Hello v další části několik, nejprve ukazuje, jak toolink účtu úložiště, pak popisují přidáním aplikací a balíčků a jejich s správě hello portálu.</span><span class="sxs-lookup"><span data-stu-id="74aa9-158">In hello next few sections, we first show how toolink a Storage account, then discuss adding applications and packages and managing them with hello portal.</span></span>

### <a name="link-a-storage-account"></a><span data-ttu-id="74aa9-159">Odkaz účet úložiště</span><span class="sxs-lookup"><span data-stu-id="74aa9-159">Link a Storage account</span></span>
<span data-ttu-id="74aa9-160">toouse balíčky aplikací, je nutné nejprve propojit tooyour účet Azure Storage účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="74aa9-160">toouse application packages, you must first link an Azure Storage account tooyour Batch account.</span></span> <span data-ttu-id="74aa9-161">Pokud ještě nemáte nakonfigurovaný účet úložiště, hello portál Azure zobrazí upozornění hello poprvé, klikněte na tlačítko hello **aplikace** dlaždici v hello **účet Batch** okno.</span><span class="sxs-lookup"><span data-stu-id="74aa9-161">If you have not yet configured a Storage account, hello Azure portal displays a warning hello first time you click hello **Applications** tile in hello **Batch account** blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="74aa9-162">Batch aktuálně podporuje *pouze* hello **pro obecné účely** typ účtu úložiště, jak je popsáno v kroku 5, [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account)v [o Azure účty úložiště](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="74aa9-162">Batch currently supports *only* hello **General-purpose** storage account type as described in step 5, [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account), in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="74aa9-163">Při propojení tooyour účtu Azure Storage účtu Batch, propojte *pouze* **pro obecné účely** účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="74aa9-163">When you link an Azure Storage account tooyour Batch account, link *only* a **General-purpose** storage account.</span></span>
> 
> 

<span data-ttu-id="74aa9-164">![Upozornění: nakonfigurován žádný účet úložiště, na portálu Azure][9]</span><span class="sxs-lookup"><span data-stu-id="74aa9-164">!['No storage account configured' warning in Azure portal][9]</span></span>

<span data-ttu-id="74aa9-165">Hello hello používá služba Batch související toostore účet úložiště balíčky aplikací.</span><span class="sxs-lookup"><span data-stu-id="74aa9-165">hello Batch service uses hello associated Storage account toostore your application packages.</span></span> <span data-ttu-id="74aa9-166">Po propojili jste hello dva účty Batch můžete automaticky nasadit balíčky hello uložené v hello propojené úložiště účet tooyour výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="74aa9-166">After you've linked hello two accounts, Batch can automatically deploy hello packages stored in hello linked Storage account tooyour compute nodes.</span></span> <span data-ttu-id="74aa9-167">Klikněte na tlačítko toolink tooyour účet úložiště účtu Batch, **nastavení účtu úložiště** na hello **upozornění** okna a potom klikněte na **účet úložiště** na hello **Účet úložiště** okno.</span><span class="sxs-lookup"><span data-stu-id="74aa9-167">toolink a Storage account tooyour Batch account, click **Storage account settings** on hello **Warning** blade, and then click **Storage Account** on hello **Storage Account** blade.</span></span>

<span data-ttu-id="74aa9-168">![Zvolte okně účtu úložiště na portálu Azure][10]</span><span class="sxs-lookup"><span data-stu-id="74aa9-168">![Choose storage account blade in Azure portal][10]</span></span>

<span data-ttu-id="74aa9-169">Doporučujeme vytvořit účet úložiště *konkrétně* pro použití s vaším účtem Batch a vyberte ho sem.</span><span class="sxs-lookup"><span data-stu-id="74aa9-169">We recommend that you create a Storage account *specifically* for use with your Batch account, and select it here.</span></span> <span data-ttu-id="74aa9-170">Podrobnosti o toocreate účtu úložiště, najdete v části "Vytvoření účtu úložiště" v [účty Azure Storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="74aa9-170">For details about how toocreate a storage account, see "Create a Storage account" in [About Azure Storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="74aa9-171">Po vytvoření účtu úložiště, pak můžete propojit se účtu Batch tooyour pomocí hello **účet úložiště** okno.</span><span class="sxs-lookup"><span data-stu-id="74aa9-171">After you've created a Storage account, you can then link it tooyour Batch account by using hello **Storage Account** blade.</span></span>

> [!WARNING]
> <span data-ttu-id="74aa9-172">Hello služba Batch používá Azure Storage toostore balíčky aplikací jako objekty BLOB bloku.</span><span class="sxs-lookup"><span data-stu-id="74aa9-172">hello Batch service uses Azure Storage toostore your application packages as block blobs.</span></span> <span data-ttu-id="74aa9-173">Jste [účtován jako normální] [ storage_pricing] pro data objektů blob bloku hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-173">You are [charged as normal][storage_pricing] for hello block blob data.</span></span> <span data-ttu-id="74aa9-174">Zda tooconsider hello velikost a počet balíčky aplikací a pravidelně odstraňuje zastaralá balíčky toominimize náklady.</span><span class="sxs-lookup"><span data-stu-id="74aa9-174">Be sure tooconsider hello size and number of your application packages, and periodically remove deprecated packages toominimize costs.</span></span>
> 
> 

### <a name="view-current-applications"></a><span data-ttu-id="74aa9-175">Zobrazit aktuální aplikace</span><span class="sxs-lookup"><span data-stu-id="74aa9-175">View current applications</span></span>
<span data-ttu-id="74aa9-176">tooview hello aplikace v účtu Batch, klikněte na tlačítko hello **aplikace** položka nabídky v levé nabídce hello při zobrazení hello **účet Batch** okno.</span><span class="sxs-lookup"><span data-stu-id="74aa9-176">tooview hello applications in your Batch account, click hello **Applications** menu item in hello left menu while viewing hello **Batch account** blade.</span></span>

<span data-ttu-id="74aa9-177">![Dlaždice aplikace][2]</span><span class="sxs-lookup"><span data-stu-id="74aa9-177">![Applications tile][2]</span></span>

<span data-ttu-id="74aa9-178">Výběrem této možnosti nabídky otevře hello **aplikace** okno:</span><span class="sxs-lookup"><span data-stu-id="74aa9-178">Selecting this menu option opens hello **Applications** blade:</span></span>

<span data-ttu-id="74aa9-179">![Seznam aplikací][3]</span><span class="sxs-lookup"><span data-stu-id="74aa9-179">![List applications][3]</span></span>

<span data-ttu-id="74aa9-180">Hello **aplikace** zobrazí okno hello ID každé aplikace ve vašem účtu a hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="74aa9-180">hello **Applications** blade displays hello ID of each application in your account and hello following properties:</span></span>

* <span data-ttu-id="74aa9-181">**Balíčky**: hello číslo verze přidružené k této aplikaci.</span><span class="sxs-lookup"><span data-stu-id="74aa9-181">**Packages**: hello number of versions associated with this application.</span></span>
* <span data-ttu-id="74aa9-182">**Výchozí verze**: verze aplikace hello nainstalovat, pokud neuvedete na verzi, když zadáte hello aplikací pro fond.</span><span class="sxs-lookup"><span data-stu-id="74aa9-182">**Default version**: hello application version installed if you do not indicate a version when you specify hello application for a pool.</span></span> <span data-ttu-id="74aa9-183">Toto nastavení je volitelné.</span><span class="sxs-lookup"><span data-stu-id="74aa9-183">This setting is optional.</span></span>
* <span data-ttu-id="74aa9-184">**Povolit aktualizace**: hello hodnotu, která určuje, zda balíček aktualizace, odstranění a přidání jsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="74aa9-184">**Allow updates**: hello value that specifies whether package updates, deletions, and additions are allowed.</span></span> <span data-ttu-id="74aa9-185">Pokud je toto nastaveno příliš**ne**, jsou pro aplikaci hello zakázány balíček aktualizace a odstranění.</span><span class="sxs-lookup"><span data-stu-id="74aa9-185">If this is set too**No**, package updates and deletions are disabled for hello application.</span></span> <span data-ttu-id="74aa9-186">Můžete přidat pouze nové verze balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="74aa9-186">Only new application package versions can be added.</span></span> <span data-ttu-id="74aa9-187">Výchozí hodnota Hello je **Ano**.</span><span class="sxs-lookup"><span data-stu-id="74aa9-187">hello default is **Yes**.</span></span>

### <a name="view-application-details"></a><span data-ttu-id="74aa9-188">Zobrazení podrobností o aplikaci</span><span class="sxs-lookup"><span data-stu-id="74aa9-188">View application details</span></span>
<span data-ttu-id="74aa9-189">okno hello tooopen, která zahrnuje hello podrobnosti pro aplikace, vyberte hello aplikaci v hello **aplikace** okno.</span><span class="sxs-lookup"><span data-stu-id="74aa9-189">tooopen hello blade that includes hello details for an application, select hello application in hello **Applications** blade.</span></span>

<span data-ttu-id="74aa9-190">![Podrobnosti o aplikaci][4]</span><span class="sxs-lookup"><span data-stu-id="74aa9-190">![Application details][4]</span></span>

<span data-ttu-id="74aa9-191">V okně podrobností aplikace hello můžete nakonfigurovat následující nastavení pro vaše aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-191">In hello application details blade, you can configure hello following settings for your application.</span></span>

* <span data-ttu-id="74aa9-192">**Povolit aktualizace**: Určete, zda jeho balíčky aplikací můžete aktualizovat nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="74aa9-192">**Allow updates**: Specify whether its application packages can be updated or deleted.</span></span> <span data-ttu-id="74aa9-193">Později v tomto článku najdete v části "Aktualizace nebo odstranění balíčku aplikace".</span><span class="sxs-lookup"><span data-stu-id="74aa9-193">See "Update or Delete an application package" later in this article.</span></span>
* <span data-ttu-id="74aa9-194">**Výchozí verze**: určit výchozí aplikace balíčku toodeploy toocompute uzlů.</span><span class="sxs-lookup"><span data-stu-id="74aa9-194">**Default version**: Specify a default application package toodeploy toocompute nodes.</span></span>
* <span data-ttu-id="74aa9-195">**Zobrazovaný název**: Zadejte popisný název, který dávku řešení můžete použít při zobrazuje informace o aplikaci hello, například v hello uživatelského rozhraní služby, které poskytujete tooyour zákazníkům prostřednictvím Batch.</span><span class="sxs-lookup"><span data-stu-id="74aa9-195">**Display name**: Specify a friendly name that your Batch solution can use when it displays information about hello application, for example, in hello UI of a service that you provide tooyour customers through Batch.</span></span>

### <a name="add-a-new-application"></a><span data-ttu-id="74aa9-196">Přidejte novou aplikaci</span><span class="sxs-lookup"><span data-stu-id="74aa9-196">Add a new application</span></span>
<span data-ttu-id="74aa9-197">toocreate novou aplikaci, přidejte balíček aplikace a zadejte ID aplikace nové, jedinečné.</span><span class="sxs-lookup"><span data-stu-id="74aa9-197">toocreate a new application, add an application package and specify a new, unique application ID.</span></span> <span data-ttu-id="74aa9-198">Hello první balíček aplikace, které přidáte s novým ID aplikace hello také vytvoří novou aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-198">hello first application package that you add with hello new application ID also creates hello new application.</span></span>

<span data-ttu-id="74aa9-199">Klikněte na tlačítko **přidat** na hello **aplikace** okno tooopen hello **novou aplikaci** okno.</span><span class="sxs-lookup"><span data-stu-id="74aa9-199">Click **Add** on hello **Applications** blade tooopen hello **New application** blade.</span></span>

<span data-ttu-id="74aa9-200">![Nové okno aplikace na portálu Azure][5]</span><span class="sxs-lookup"><span data-stu-id="74aa9-200">![New application blade in Azure portal][5]</span></span>

<span data-ttu-id="74aa9-201">Hello **novou aplikaci** okno poskytuje následující hello polí toospecify hello nastavení nové aplikace a balíček aplikace.</span><span class="sxs-lookup"><span data-stu-id="74aa9-201">hello **New application** blade provides hello following fields toospecify hello settings of your new application and application package.</span></span>

<span data-ttu-id="74aa9-202">**Id aplikace**</span><span class="sxs-lookup"><span data-stu-id="74aa9-202">**Application id**</span></span>

<span data-ttu-id="74aa9-203">Toto pole určuje hello ID novou aplikaci, která je subjektu toohello standardní ID dávky Azure ověřovacích pravidel.</span><span class="sxs-lookup"><span data-stu-id="74aa9-203">This field specifies hello ID of your new application, which is subject toohello standard Azure Batch ID validation rules.</span></span> <span data-ttu-id="74aa9-204">Hello pravidla pro zajištění ID aplikací jsou následující:</span><span class="sxs-lookup"><span data-stu-id="74aa9-204">hello rules for providing an application ID are as follows:</span></span>

* <span data-ttu-id="74aa9-205">Na uzlech Windows hello ID může obsahovat libovolnou kombinaci alfanumerických znaků, pomlčky a podtržítka.</span><span class="sxs-lookup"><span data-stu-id="74aa9-205">On Windows nodes, hello ID can contain any combination of alphanumeric characters, hyphens, and underscores.</span></span> <span data-ttu-id="74aa9-206">Na uzlech Linux jsou povoleny pouze alfanumerické znaky a podtržítka.</span><span class="sxs-lookup"><span data-stu-id="74aa9-206">On Linux nodes, only alphanumeric characters and underscores are permitted.</span></span>
* <span data-ttu-id="74aa9-207">Nesmí obsahovat víc než 64 znaků.</span><span class="sxs-lookup"><span data-stu-id="74aa9-207">Cannot contain more than 64 characters.</span></span>
* <span data-ttu-id="74aa9-208">Musí být jedinečný v rámci hello účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="74aa9-208">Must be unique within hello Batch account.</span></span>
* <span data-ttu-id="74aa9-209">Je zachována a velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="74aa9-209">Is case-preserving and case-insensitive.</span></span>

<span data-ttu-id="74aa9-210">**Verze**</span><span class="sxs-lookup"><span data-stu-id="74aa9-210">**Version**</span></span>

<span data-ttu-id="74aa9-211">Toto pole určuje hello verzi balíčku aplikace hello, který ukládáte.</span><span class="sxs-lookup"><span data-stu-id="74aa9-211">This field specifies hello version of hello application package you are uploading.</span></span> <span data-ttu-id="74aa9-212">Verze řetězce jsou subjektu toohello následující pravidla ověřování:</span><span class="sxs-lookup"><span data-stu-id="74aa9-212">Version strings are subject toohello following validation rules:</span></span>

* <span data-ttu-id="74aa9-213">Na uzlech Windows hello řetězec verze může obsahovat libovolnou kombinaci alfanumerických znaků, pomlčky, podtržítka a tečky.</span><span class="sxs-lookup"><span data-stu-id="74aa9-213">On Windows nodes, hello version string can contain any combination of alphanumeric characters, hyphens, underscores, and periods.</span></span> <span data-ttu-id="74aa9-214">Na uzlech Linux řetězec verze hello může obsahovat pouze alfanumerické znaky a podtržítka.</span><span class="sxs-lookup"><span data-stu-id="74aa9-214">On Linux nodes, hello version string can contain only alphanumeric characters and underscores.</span></span>
* <span data-ttu-id="74aa9-215">Nesmí obsahovat víc než 64 znaků.</span><span class="sxs-lookup"><span data-stu-id="74aa9-215">Cannot contain more than 64 characters.</span></span>
* <span data-ttu-id="74aa9-216">Musí být jedinečný v rámci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-216">Must be unique within hello application.</span></span>
* <span data-ttu-id="74aa9-217">Jsou zachována a velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="74aa9-217">Are case-preserving and case-insensitive.</span></span>

<span data-ttu-id="74aa9-218">**Balíček aplikace**</span><span class="sxs-lookup"><span data-stu-id="74aa9-218">**Application package**</span></span>

<span data-ttu-id="74aa9-219">Toto pole určuje hello soubor .zip, který obsahuje binární soubory aplikace hello a podpůrné soubory, které jsou požadované tooexecute hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="74aa9-219">This field specifies hello .zip file that contains hello application binaries and supporting files that are required tooexecute hello application.</span></span> <span data-ttu-id="74aa9-220">Klikněte na tlačítko hello **vyberte soubor** pole nebo hello tooand toobrowse ikonu složky, vyberte soubor .zip, který obsahuje soubory aplikace.</span><span class="sxs-lookup"><span data-stu-id="74aa9-220">Click hello **Select a file** box or hello folder icon toobrowse tooand select a .zip file that contains your application's files.</span></span>

<span data-ttu-id="74aa9-221">Jakmile vyberete soubor, klikněte na tlačítko **OK** toobegin hello nahrávání tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="74aa9-221">After you've selected a file, click **OK** toobegin hello upload tooAzure Storage.</span></span> <span data-ttu-id="74aa9-222">Po dokončení operace nahrávání hello hello portál zobrazí oznámení a zavře okno hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-222">When hello upload operation is complete, hello portal displays a notification and closes hello blade.</span></span> <span data-ttu-id="74aa9-223">V závislosti na velikosti hello hello souboru se odesílání a hello rychlost síťového připojení může tato operace chvíli trvat.</span><span class="sxs-lookup"><span data-stu-id="74aa9-223">Depending on hello size of hello file that you are uploading and hello speed of your network connection, this operation may take some time.</span></span>

> [!WARNING]
> <span data-ttu-id="74aa9-224">Nezavírejte stránku hello **novou aplikaci** okno před dokončením operace nahrávání hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-224">Do not close hello **New application** blade before hello upload operation is complete.</span></span> <span data-ttu-id="74aa9-225">Díky tomu se zastaví proces odesílání hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-225">Doing so will stop hello upload process.</span></span>
> 
> 

### <a name="add-a-new-application-package"></a><span data-ttu-id="74aa9-226">Přidat nový balíček aplikace</span><span class="sxs-lookup"><span data-stu-id="74aa9-226">Add a new application package</span></span>
<span data-ttu-id="74aa9-227">tooadd novou verzi balíčku aplikace pro existující aplikace, vyberte aplikaci v hello **aplikace** okně klikněte na tlačítko **balíčky**, pak klikněte na tlačítko **přidat** tooopen Hello **přidat balíček** okno.</span><span class="sxs-lookup"><span data-stu-id="74aa9-227">tooadd a new application package version for an existing application, select an application in hello **Applications** blade, click **Packages**, then click **Add** tooopen hello **Add package** blade.</span></span>

<span data-ttu-id="74aa9-228">![Přidejte balíček okna aplikací na portálu Azure][8]</span><span class="sxs-lookup"><span data-stu-id="74aa9-228">![Add application package blade in Azure portal][8]</span></span>

<span data-ttu-id="74aa9-229">Jak vidíte, hello pole shodují s těmi hello **novou aplikaci** okno, ale hello **id aplikace** pole je zakázána.</span><span class="sxs-lookup"><span data-stu-id="74aa9-229">As you can see, hello fields match those of hello **New application** blade, but hello **Application id** box is disabled.</span></span> <span data-ttu-id="74aa9-230">Stejně jako u nové aplikace hello zadejte hello **verze** pro nový balíček, procházet tooyour **balíčku aplikace** .zip souboru a pak klikněte na **OK** tooupload hello balíček.</span><span class="sxs-lookup"><span data-stu-id="74aa9-230">As you did for hello new application, specify hello **Version** for your new package, browse tooyour **Application package** .zip file, then click **OK** tooupload hello package.</span></span>

### <a name="update-or-delete-an-application-package"></a><span data-ttu-id="74aa9-231">Aktualizace nebo odstranění balíčku aplikace</span><span class="sxs-lookup"><span data-stu-id="74aa9-231">Update or delete an application package</span></span>
<span data-ttu-id="74aa9-232">tooupdate nebo odstranit, klikněte na existující balíček aplikace, otevřete hello okno Podrobnosti pro aplikace hello **balíčky** tooopen hello **balíčky** okně klikněte na tlačítko hello **třemi tečkami**v řádku hello balíčku aplikace hello má toomodify a vyberte hello akce, které chcete tooperform.</span><span class="sxs-lookup"><span data-stu-id="74aa9-232">tooupdate or delete an existing application package, open hello details blade for hello application, click **Packages** tooopen hello **Packages** blade, click hello **ellipsis** in hello row of hello application package that you want toomodify, and select hello action that you want tooperform.</span></span>

<span data-ttu-id="74aa9-233">![Aktualizace nebo odstranění balíčku na portálu Azure][7]</span><span class="sxs-lookup"><span data-stu-id="74aa9-233">![Update or delete package in Azure portal][7]</span></span>

<span data-ttu-id="74aa9-234">**Aktualizace**</span><span class="sxs-lookup"><span data-stu-id="74aa9-234">**Update**</span></span>

<span data-ttu-id="74aa9-235">Když kliknete na tlačítko **aktualizace**, hello *balíček aktualizace* zobrazí se okno.</span><span class="sxs-lookup"><span data-stu-id="74aa9-235">When you click **Update**, hello *Update package* blade is displayed.</span></span> <span data-ttu-id="74aa9-236">Toto okno je podobné toohello *nový balíček aplikace* okno, ale pouze pole výběr balíčku hello je povoleno, což vám toospecify nové tooupload souboru ZIP.</span><span class="sxs-lookup"><span data-stu-id="74aa9-236">This blade is similar toohello *New application package* blade, however only hello package selection field is enabled, allowing you toospecify a new ZIP file tooupload.</span></span>

<span data-ttu-id="74aa9-237">![Okno balíček aktualizace na portálu Azure][11]</span><span class="sxs-lookup"><span data-stu-id="74aa9-237">![Update package blade in Azure portal][11]</span></span>

<span data-ttu-id="74aa9-238">**Odstranění**</span><span class="sxs-lookup"><span data-stu-id="74aa9-238">**Delete**</span></span>

<span data-ttu-id="74aa9-239">Když kliknete na tlačítko **odstranit**, se zobrazí výzva tooconfirm hello odstranění verze balíčku hello a Batch odstraní hello balíček z úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="74aa9-239">When you click **Delete**, you are asked tooconfirm hello deletion of hello package version, and Batch deletes hello package from Azure Storage.</span></span> <span data-ttu-id="74aa9-240">Pokud odstraníte hello výchozí verze aplikace, hello **výchozí verze** je odebrat nastavení pro aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-240">If you delete hello default version of an application, hello **Default version** setting is removed for hello application.</span></span>

<span data-ttu-id="74aa9-241">![Odstranit aplikaci][12]</span><span class="sxs-lookup"><span data-stu-id="74aa9-241">![Delete application ][12]</span></span>

## <a name="install-applications-on-compute-nodes"></a><span data-ttu-id="74aa9-242">Instalace aplikací na výpočetní uzly</span><span class="sxs-lookup"><span data-stu-id="74aa9-242">Install applications on compute nodes</span></span>
<span data-ttu-id="74aa9-243">Teď, když jste se naučili, jak balíčky toomanage aplikace s hello portálu Azure, můžete probereme jak toodeploy je toocompute uzlů a jejich spuštění s úkoly služby Batch.</span><span class="sxs-lookup"><span data-stu-id="74aa9-243">Now that you've learned how toomanage application packages with hello Azure portal, we can discuss how toodeploy them toocompute nodes and run them with Batch tasks.</span></span>

### <a name="install-pool-application-packages"></a><span data-ttu-id="74aa9-244">Instalovat balíčky fondu aplikací</span><span class="sxs-lookup"><span data-stu-id="74aa9-244">Install pool application packages</span></span>
<span data-ttu-id="74aa9-245">tooinstall balíček aplikace na všech výpočetních uzlů ve fondu, zadejte balíček aplikace jeden nebo více *odkazy* hello fondu.</span><span class="sxs-lookup"><span data-stu-id="74aa9-245">tooinstall an application package on all compute nodes in a pool, specify one or more application package *references* for hello pool.</span></span> <span data-ttu-id="74aa9-246">Hello balíčky aplikací, které zadáte pro fond jsou nainstalované na každém výpočetním uzlu, pokud takový uzel připojí hello fondu a pokud je uzel hello restartovat nebo obnovit z Image.</span><span class="sxs-lookup"><span data-stu-id="74aa9-246">hello application packages that you specify for a pool are installed on each compute node when that node joins hello pool, and when hello node is rebooted or reimaged.</span></span>

<span data-ttu-id="74aa9-247">V Batch .NET, zadejte jednu nebo více [CloudPool][net_cloudpool].[ ApplicationPackageReferences] [ net_cloudpool_pkgref] při vytvoření nového fondu, nebo pro existující fond.</span><span class="sxs-lookup"><span data-stu-id="74aa9-247">In Batch .NET, specify one or more [CloudPool][net_cloudpool].[ApplicationPackageReferences][net_cloudpool_pkgref] when you create a new pool, or for an existing pool.</span></span> <span data-ttu-id="74aa9-248">Hello [ApplicationPackageReference] [ net_pkgref] třída určuje ID aplikace a verze tooinstall fondu výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="74aa9-248">hello [ApplicationPackageReference][net_pkgref] class specifies an application ID and version tooinstall on a pool's compute nodes.</span></span>

```csharp
// Create hello unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicatedComputeNodes: 1,
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify hello application and version tooinstall on hello compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit hello pool so that it's created in hello Batch service. As hello nodes join
// hello pool, hello specified application package is installed on each.
await myCloudPool.CommitAsync();
```

> [!IMPORTANT]
> <span data-ttu-id="74aa9-249">Pokud z nějakého důvodu selže nasazení balíček aplikace, značky služby Batch hello hello uzlu [nepoužitelná][net_nodestate], a žádné úlohy jsou naplánovány pro spuštění v tomto uzlu.</span><span class="sxs-lookup"><span data-stu-id="74aa9-249">If an application package deployment fails for any reason, hello Batch service marks hello node [unusable][net_nodestate], and no tasks are scheduled for execution on that node.</span></span> <span data-ttu-id="74aa9-250">V takovém případě byste měli **restartujte** hello uzlu tooreinitiate hello balíčku nasazení.</span><span class="sxs-lookup"><span data-stu-id="74aa9-250">In this case, you should **restart** hello node tooreinitiate hello package deployment.</span></span> <span data-ttu-id="74aa9-251">Restartování uzlu hello taky umožňuje znovu plánování úkolů na uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-251">Restarting hello node also enables task scheduling again on hello node.</span></span>
> 
> 

### <a name="install-task-application-packages"></a><span data-ttu-id="74aa9-252">Instalovat balíčky aplikace úkolů</span><span class="sxs-lookup"><span data-stu-id="74aa9-252">Install task application packages</span></span>
<span data-ttu-id="74aa9-253">Podobně jako tooa fondu, zadejte balíček aplikace *odkazy* pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="74aa9-253">Similar tooa pool, you specify application package *references* for a task.</span></span> <span data-ttu-id="74aa9-254">Když je úkol naplánované toorun na uzlu, balíček hello stažena a extrahována těsně před hello úloh příkazový řádek se spustí.</span><span class="sxs-lookup"><span data-stu-id="74aa9-254">When a task is scheduled toorun on a node, hello package is downloaded and extracted just before hello task's command line is executed.</span></span> <span data-ttu-id="74aa9-255">Pokud zadaného balíčku a verze je již nainstalován v uzlu hello, nebude stažen hello balíček a se používá existující balíček hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-255">If a specified package and version is already installed on hello node, hello package is not downloaded and hello existing package is used.</span></span>

<span data-ttu-id="74aa9-256">tooinstall balíček aplikace úlohy, konfigurovat úlohy hello [CloudTask][net_cloudtask].[ ApplicationPackageReferences] [ net_cloudtask_pkgref] vlastnost:</span><span class="sxs-lookup"><span data-stu-id="74aa9-256">tooinstall a task application package, configure hello task's [CloudTask][net_cloudtask].[ApplicationPackageReferences][net_cloudtask_pkgref] property:</span></span>

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

## <a name="execute-hello-installed-applications"></a><span data-ttu-id="74aa9-257">Spouštění aplikací hello nainstalován</span><span class="sxs-lookup"><span data-stu-id="74aa9-257">Execute hello installed applications</span></span>
<span data-ttu-id="74aa9-258">Hello balíčky, které jste určili pro fond nebo úloh jsou stažené a rozbalené tooa s názvem adresáře v rámci hello `AZ_BATCH_ROOT_DIR` hello uzlu.</span><span class="sxs-lookup"><span data-stu-id="74aa9-258">hello packages that you've specified for a pool or task are downloaded and extracted tooa named directory within hello `AZ_BATCH_ROOT_DIR` of hello node.</span></span> <span data-ttu-id="74aa9-259">Batch vytvoří také proměnná prostředí, který obsahuje toohello hello cestu s názvem adresáře.</span><span class="sxs-lookup"><span data-stu-id="74aa9-259">Batch also creates an environment variable that contains hello path toohello named directory.</span></span> <span data-ttu-id="74aa9-260">Vaše příkazové řádky úkolu pomocí této proměnné prostředí při odkazování na aplikace hello na uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-260">Your task command lines use this environment variable when referencing hello application on hello node.</span></span> 

<span data-ttu-id="74aa9-261">V uzlů Windows hello proměnné se v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="74aa9-261">On Windows nodes, hello variable is in hello following format:</span></span>

```
Windows:
AZ_BATCH_APP_PACKAGE_APPLICATIONID#version
```

<span data-ttu-id="74aa9-262">Na uzlech Linux formátu hello se mírně liší.</span><span class="sxs-lookup"><span data-stu-id="74aa9-262">On Linux nodes, hello format is slightly different.</span></span> <span data-ttu-id="74aa9-263">Tečky (.) a pomlčky (-) a tyto znaky (#) jsou plochou toounderscores v proměnné prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-263">Periods (.), hypens (-) and number signs (#) are flattened toounderscores in hello environment variable.</span></span> <span data-ttu-id="74aa9-264">Například:</span><span class="sxs-lookup"><span data-stu-id="74aa9-264">For example:</span></span>

```
Linux:
AZ_BATCH_APP_PACKAGE_APPLICATIONID_version
```

<span data-ttu-id="74aa9-265">`APPLICATIONID`a `version` jsou hodnoty, které odpovídají toohello aplikace a verze balíčku jste určili pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="74aa9-265">`APPLICATIONID` and `version` are values that correspond toohello application and package version you've specified for deployment.</span></span> <span data-ttu-id="74aa9-266">Například pokud jste zadali, že verze 2.7 aplikace *digestoru* by měly být nainstalovány na systému Windows se vaše příkazové řádky úkolu využije tento tooaccess proměnné prostředí jeho soubory:</span><span class="sxs-lookup"><span data-stu-id="74aa9-266">For example, if you specified that version 2.7 of application *blender* should be installed on Windows nodes, your task command lines would use this environment variable tooaccess its files:</span></span>

```
Windows:
AZ_BATCH_APP_PACKAGE_BLENDER#2.7
```

<span data-ttu-id="74aa9-267">Na Linuxových uzlů zadejte proměnnou prostředí hello v tomto formátu:</span><span class="sxs-lookup"><span data-stu-id="74aa9-267">On Linux nodes, specify hello environment variable in this format:</span></span>

```
Linux:
AZ_BATCH_APP_PACKAGE_BLENDER_2_7
``` 

<span data-ttu-id="74aa9-268">Při nahrávání balíčku aplikace, můžete zadat, výchozí verze toodeploy tooyour výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="74aa9-268">When you upload an application package, you can specify a default version toodeploy tooyour compute nodes.</span></span> <span data-ttu-id="74aa9-269">Pokud jste zadali výchozí verze pro aplikaci, můžete vynechat hello verze příponu když odkazujete aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-269">If you have specified a default version for an application, you can omit hello version suffix when you reference hello application.</span></span> <span data-ttu-id="74aa9-270">Verze aplikace hello výchozí můžete zadat v hello portál Azure, v okně aplikace hello, jak je znázorněno v [odesílat a spravovat aplikace](#upload-and-manage-applications).</span><span class="sxs-lookup"><span data-stu-id="74aa9-270">You can specify hello default application version in hello Azure portal, on hello Applications blade, as shown in [Upload and manage applications](#upload-and-manage-applications).</span></span>

<span data-ttu-id="74aa9-271">Například pokud nastavíte jako hello výchozí verze pro aplikace "2.7" *digestoru*a vaše úkoly odkazovat hello následující proměnné prostředí, pak uzlů Windows, budou spuštěny verze 2.7:</span><span class="sxs-lookup"><span data-stu-id="74aa9-271">For example, if you set "2.7" as hello default version for application *blender*, and your tasks reference hello following environment variable, then your Windows nodes will execute version 2.7:</span></span>

`AZ_BATCH_APP_PACKAGE_BLENDER`

<span data-ttu-id="74aa9-272">Hello následující fragment kódu ukazuje příklad úloh příkazový řádek, který spouští hello výchozí verzi hello *digestoru* aplikace:</span><span class="sxs-lookup"><span data-stu-id="74aa9-272">hello following code snippet shows an example task command line that launches hello default version of hello *blender* application:</span></span>

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [!TIP]
> <span data-ttu-id="74aa9-273">V tématu [nastavení prostředí pro úkoly](batch-api-basics.md#environment-settings-for-tasks) v hello [přehled funkcí Batch](batch-api-basics.md) Další informace o nastavení prostředí výpočetního uzlu.</span><span class="sxs-lookup"><span data-stu-id="74aa9-273">See [Environment settings for tasks](batch-api-basics.md#environment-settings-for-tasks) in hello [Batch feature overview](batch-api-basics.md) for more information about compute node environment settings.</span></span>
> 
> 

## <a name="update-a-pools-application-packages"></a><span data-ttu-id="74aa9-274">Aktualizace balíčků aplikací fondu</span><span class="sxs-lookup"><span data-stu-id="74aa9-274">Update a pool's application packages</span></span>
<span data-ttu-id="74aa9-275">Pokud se balíček aplikace již byla nakonfigurována existujícího fondu, můžete zadat nový balíček pro fond hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-275">If an existing pool has already been configured with an application package, you can specify a new package for hello pool.</span></span> <span data-ttu-id="74aa9-276">Pokud zadáte nový odkaz na balíček pro fond, platí následující hello:</span><span class="sxs-lookup"><span data-stu-id="74aa9-276">If you specify a new package reference for a pool, hello following apply:</span></span>

* <span data-ttu-id="74aa9-277">Hello služba Batch nainstaluje hello nově zadaný balíček na všechny nové uzly, které připojí hello fondu a na všechny existující uzlu, který je restartovat nebo obnovit z Image.</span><span class="sxs-lookup"><span data-stu-id="74aa9-277">hello Batch service installs hello newly specified package on all new nodes that join hello pool and on any existing node that is rebooted or reimaged.</span></span>
* <span data-ttu-id="74aa9-278">Výpočetní uzly, které jsou už ve fondu hello při aktualizaci hello balíček odkazuje neinstalujte automaticky hello nový balíček aplikace.</span><span class="sxs-lookup"><span data-stu-id="74aa9-278">Compute nodes that are already in hello pool when you update hello package references do not automatically install hello new application package.</span></span> <span data-ttu-id="74aa9-279">Tyto výpočetní uzly, je nutné restartovat nebo přeinstalovanou tooreceive hello nový balíček.</span><span class="sxs-lookup"><span data-stu-id="74aa9-279">These compute nodes must be rebooted or reimaged tooreceive hello new package.</span></span>
* <span data-ttu-id="74aa9-280">Po nasazení nového balíčku projeví hello vytvoření proměnné prostředí balíček odkazuje nové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-280">When a new package is deployed, hello created environment variables reflect hello new application package references.</span></span>

<span data-ttu-id="74aa9-281">V tomto příkladu má existující fond hello 2.7 verzi hello *digestoru* aplikace konfigurovaná jako jeden z jeho [CloudPool][net_cloudpool].[ ApplicationPackageReferences][net_cloudpool_pkgref].</span><span class="sxs-lookup"><span data-stu-id="74aa9-281">In this example, hello existing pool has version 2.7 of hello *blender* application configured as one of its [CloudPool][net_cloudpool].[ApplicationPackageReferences][net_cloudpool_pkgref].</span></span> <span data-ttu-id="74aa9-282">uzly fondu hello tooupdate s verzí 2.76b, zadejte nový [ApplicationPackageReference] [ net_pkgref] s novou verzí hello a potvrzení změn hello.</span><span class="sxs-lookup"><span data-stu-id="74aa9-282">tooupdate hello pool's nodes with version 2.76b, specify a new [ApplicationPackageReference][net_pkgref] with hello new version, and commit hello change.</span></span>

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

<span data-ttu-id="74aa9-283">Teď, když hello novou verzi byl nakonfigurován, hello služba Batch nainstaluje verzi 2.76b tooany *nové* uzlu, který připojí hello fondu.</span><span class="sxs-lookup"><span data-stu-id="74aa9-283">Now that hello new version has been configured, hello Batch service installs version 2.76b tooany *new* node that joins hello pool.</span></span> <span data-ttu-id="74aa9-284">tooinstall 2.76b na hello uzlech, jež jsou *již* ve fondu hello restartovat nebo obnovit z Image je.</span><span class="sxs-lookup"><span data-stu-id="74aa9-284">tooinstall 2.76b on hello nodes that are *already* in hello pool, reboot or reimage them.</span></span> <span data-ttu-id="74aa9-285">Všimněte si, že restartovaný uzly zachovat hello soubory z předchozích nasazení balíčku.</span><span class="sxs-lookup"><span data-stu-id="74aa9-285">Note that rebooted nodes retain hello files from previous package deployments.</span></span>

## <a name="list-hello-applications-in-a-batch-account"></a><span data-ttu-id="74aa9-286">Seznam hello aplikace v účtu Batch</span><span class="sxs-lookup"><span data-stu-id="74aa9-286">List hello applications in a Batch account</span></span>
<span data-ttu-id="74aa9-287">Můžete vytvořit seznam hello aplikace a jejich balíčky v účtu Batch pomocí hello [ApplicationOperations][net_appops].[ ListApplicationSummaries] [ net_appops_listappsummaries] metoda.</span><span class="sxs-lookup"><span data-stu-id="74aa9-287">You can list hello applications and their packages in a Batch account by using hello [ApplicationOperations][net_appops].[ListApplicationSummaries][net_appops_listappsummaries] method.</span></span>

```csharp
// List hello applications and their application packages in hello Batch account.
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

## <a name="wrap-up"></a><span data-ttu-id="74aa9-288">Zabalení</span><span class="sxs-lookup"><span data-stu-id="74aa9-288">Wrap up</span></span>
<span data-ttu-id="74aa9-289">Pomocí balíčků aplikací můžete pomoct vašim zákazníkům vyberte hello aplikací pro svou práci a zadejte hello přesnou verzi toouse při zpracování úlohy se služby podporují službu Batch.</span><span class="sxs-lookup"><span data-stu-id="74aa9-289">With application packages, you can help your customers select hello applications for their jobs and specify hello exact version toouse when processing jobs with your Batch-enabled service.</span></span> <span data-ttu-id="74aa9-290">Také můžete zadat hello možnost pro vaše zákazníky tooupload a sledovat své vlastní aplikace ve službě.</span><span class="sxs-lookup"><span data-stu-id="74aa9-290">You might also provide hello ability for your customers tooupload and track their own applications in your service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="74aa9-291">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74aa9-291">Next steps</span></span>
* <span data-ttu-id="74aa9-292">Hello [Batch REST API] [ api_rest] taky poskytuje podporu toowork pomocí balíčků aplikací.</span><span class="sxs-lookup"><span data-stu-id="74aa9-292">hello [Batch REST API][api_rest] also provides support toowork with application packages.</span></span> <span data-ttu-id="74aa9-293">Například v tématu hello [applicationPackageReferences] [ rest_add_pool_with_packages] element v [přidat účet fondu tooan] [ rest_add_pool] informace o toospecify balíčky tooinstall pomocí hello REST API.</span><span class="sxs-lookup"><span data-stu-id="74aa9-293">For example, see hello [applicationPackageReferences][rest_add_pool_with_packages] element in [Add a pool tooan account][rest_add_pool] for information about how toospecify packages tooinstall by using hello REST API.</span></span> <span data-ttu-id="74aa9-294">V tématu [aplikace] [ rest_applications] podrobnosti o tom, jak informace o aplikaci tooobtain pomocí hello Batch REST API.</span><span class="sxs-lookup"><span data-stu-id="74aa9-294">See [Applications][rest_applications] for details about how tooobtain application information by using hello Batch REST API.</span></span>
* <span data-ttu-id="74aa9-295">Zjistěte, jak tooprogrammatically [spravovat účty Azure Batch a kvóty pomocí rozhraní Batch Management .NET](batch-management-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="74aa9-295">Learn how tooprogrammatically [manage Azure Batch accounts and quotas with Batch Management .NET](batch-management-dotnet.md).</span></span> <span data-ttu-id="74aa9-296">Hello [rozhraní Batch Management .NET][api_net_mgmt] knihovny můžete povolit funkce vytváření a odstraňování účtu Batch aplikace nebo služby.</span><span class="sxs-lookup"><span data-stu-id="74aa9-296">hello [Batch Management .NET][api_net_mgmt] library can enable account creation and deletion features for your Batch application or service.</span></span>

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
