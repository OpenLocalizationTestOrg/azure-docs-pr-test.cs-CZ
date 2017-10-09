---
title: "aaaTutorial - použití hello Azure Batch Klientská knihovna pro .NET | Microsoft Docs"
description: "Informace hello základními koncepty Azure Batch a vytvoření jednoduché řešení pomocí rozhraní .NET."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 76cb9807-cbc1-405a-8136-d1e53e66e82b
ms.service: batch
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-compute
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 06062b3886a8081bd9a831824a981503ef55f9b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-building-solutions-with-hello-batch-client-library-for-net"></a><span data-ttu-id="65c94-103">Začínáme sestavování řešení s použitím hello Batch Klientská knihovna pro .NET</span><span class="sxs-lookup"><span data-stu-id="65c94-103">Get started building solutions with hello Batch client library for .NET</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="65c94-104">.NET</span><span class="sxs-lookup"><span data-stu-id="65c94-104">.NET</span></span>](batch-dotnet-get-started.md)
> * [<span data-ttu-id="65c94-105">Python</span><span class="sxs-lookup"><span data-stu-id="65c94-105">Python</span></span>](batch-python-tutorial.md)
> * [<span data-ttu-id="65c94-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="65c94-106">Node.js</span></span>](batch-nodejs-get-started.md)
>
>

<span data-ttu-id="65c94-107">Další hello Základy [Azure Batch] [ azure_batch] a hello [Batch .NET] [ net_api] knihovny v tomto článku probereme C# ukázkové aplikace krok podle krok.</span><span class="sxs-lookup"><span data-stu-id="65c94-107">Learn hello basics of [Azure Batch][azure_batch] and hello [Batch .NET][net_api] library in this article as we discuss a C# sample application step by step.</span></span> <span data-ttu-id="65c94-108">Podíváme na tom, jak ukázková aplikace hello využívá tooprocess služby Batch hello paralelní úlohy v cloudu hello a jak komunikuje s [Azure Storage](../storage/common/storage-introduction.md) pro přípravě a načítání souborů.</span><span class="sxs-lookup"><span data-stu-id="65c94-108">We look at how hello sample application leverages hello Batch service tooprocess a parallel workload in hello cloud, and how it interacts with [Azure Storage](../storage/common/storage-introduction.md) for file staging and retrieval.</span></span> <span data-ttu-id="65c94-109">Budete další běžné pracovním postupem aplikací Batch a získáte základní přehled o hello hlavním součástem služby Batch například úlohách, úkolech, fondech a výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="65c94-109">You'll learn a common Batch application workflow and gain a base understanding of hello major components of Batch such as jobs, tasks, pools, and compute nodes.</span></span>

<span data-ttu-id="65c94-110">![Pracovní postup řešení Batch (Basic)][11]</span><span class="sxs-lookup"><span data-stu-id="65c94-110">![Batch solution workflow (basic)][11]</span></span><br/>

## <a name="prerequisites"></a><span data-ttu-id="65c94-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="65c94-111">Prerequisites</span></span>
<span data-ttu-id="65c94-112">Tento článek předpokládá, že máte praktické znalosti jazyka C# a sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="65c94-112">This article assumes that you have a working knowledge of C# and Visual Studio.</span></span> <span data-ttu-id="65c94-113">Předpokládá také, že jste možnost toosatisfy hello účet vytvoření požadavky, které jsou uvedeny níže Azure a hello Batch a služby úložiště.</span><span class="sxs-lookup"><span data-stu-id="65c94-113">It also assumes that you're able toosatisfy hello account creation requirements that are specified below for Azure and hello Batch and Storage services.</span></span>

### <a name="accounts"></a><span data-ttu-id="65c94-114">Účty</span><span class="sxs-lookup"><span data-stu-id="65c94-114">Accounts</span></span>
* <span data-ttu-id="65c94-115">**Účet Azure**: Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet Azure][azure_free_account].</span><span class="sxs-lookup"><span data-stu-id="65c94-115">**Azure account**: If you don't already have an Azure subscription, [create a free Azure account][azure_free_account].</span></span>
* <span data-ttu-id="65c94-116">**Účet Batch**: Po pořízení předplatného Azure si [vytvořte účet Azure Batch](batch-account-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="65c94-116">**Batch account**: Once you have an Azure subscription, [create an Azure Batch account](batch-account-create-portal.md).</span></span>
* <span data-ttu-id="65c94-117">**Účet Storage**: Viz část [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) v článku [Informace o účtech Azure Storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="65c94-117">**Storage account**: See [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="65c94-118">Batch aktuálně podporuje *pouze* hello **pro obecné účely** typ účtu úložiště, jak je popsáno v kroku #5 [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) v [o Azure účty úložiště](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="65c94-118">Batch currently supports *only* hello **general-purpose** storage account type, as described in step #5 [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>
>
>

### <a name="visual-studio"></a><span data-ttu-id="65c94-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65c94-119">Visual Studio</span></span>
<span data-ttu-id="65c94-120">Musíte mít **Visual Studio 2015 nebo novější** toobuild hello ukázkový projekt.</span><span class="sxs-lookup"><span data-stu-id="65c94-120">You must have **Visual Studio 2015 or newer** toobuild hello sample project.</span></span> <span data-ttu-id="65c94-121">Bezplatné a zkušební verze sady Visual Studio můžete najít v hello [přehled produktů Visual Studio][visual_studio].</span><span class="sxs-lookup"><span data-stu-id="65c94-121">You can find free and trial versions of Visual Studio in hello [overview of Visual Studio products][visual_studio].</span></span>

### <a name="dotnettutorial-code-sample"></a><span data-ttu-id="65c94-122">Ukázka kódu *DotNetTutorial*</span><span class="sxs-lookup"><span data-stu-id="65c94-122">*DotNetTutorial* code sample</span></span>
<span data-ttu-id="65c94-123">Hello [DotNetTutorial] [ github_dotnettutorial] ukázka je jednou z mnoha ukázek kódu služby Batch najít v hello hello [azure-batch-samples] [ github_samples] úložiště v GitHub.</span><span class="sxs-lookup"><span data-stu-id="65c94-123">hello [DotNetTutorial][github_dotnettutorial] sample is one of hello many Batch code samples found in hello [azure-batch-samples][github_samples] repository on GitHub.</span></span> <span data-ttu-id="65c94-124">Všechny ukázky hello si můžete stáhnout kliknutím **klonovat nebo stáhnout > stáhnout ZIP** na domovskou stránku hello úložiště nebo kliknutím hello [azure-batch-samples-master.zip] [ github_samples_zip]přímý odkaz ke stažení.</span><span class="sxs-lookup"><span data-stu-id="65c94-124">You can download all hello samples by clicking  **Clone or download > Download ZIP** on hello repository home page, or by clicking hello [azure-batch-samples-master.zip][github_samples_zip] direct download link.</span></span> <span data-ttu-id="65c94-125">Po extrahování obsahu souboru ZIP hello hello, najdete v následující složce hello hello řešení:</span><span class="sxs-lookup"><span data-stu-id="65c94-125">Once you've extracted hello contents of hello ZIP file, you can find hello solution in hello following folder:</span></span>

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a><span data-ttu-id="65c94-126">Průzkumník Azure Batch (volitelné)</span><span class="sxs-lookup"><span data-stu-id="65c94-126">Azure Batch Explorer (optional)</span></span>
<span data-ttu-id="65c94-127">Hello [Průzkumník Azure Batch] [ github_batchexplorer] je bezplatný program, který je součástí hello [azure-batch-samples] [ github_samples] úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="65c94-127">hello [Azure Batch Explorer][github_batchexplorer] is a free utility that is included in hello [azure-batch-samples][github_samples] repository on GitHub.</span></span> <span data-ttu-id="65c94-128">Při není požadováno toocomplete tohoto kurzu, může být užitečné při vývoji a ladění řešení Batch.</span><span class="sxs-lookup"><span data-stu-id="65c94-128">While not required toocomplete this tutorial, it can be useful while developing and debugging your Batch solutions.</span></span>

## <a name="dotnettutorial-sample-project-overview"></a><span data-ttu-id="65c94-129">Přehled ukázkového projektu DotNetTutorial</span><span class="sxs-lookup"><span data-stu-id="65c94-129">DotNetTutorial sample project overview</span></span>
<span data-ttu-id="65c94-130">Hello *DotNetTutorial* ukázka kódu je řešení sady Visual Studio, která se skládá ze dvou projektů: **DotNetTutorial** a **TaskApplication**.</span><span class="sxs-lookup"><span data-stu-id="65c94-130">hello *DotNetTutorial* code sample is a Visual Studio solution that consists of two projects: **DotNetTutorial** and **TaskApplication**.</span></span>

* <span data-ttu-id="65c94-131">**DotNetTutorial** je hello klientskou aplikaci, která interaguje s tooexecute služby Batch a Storage hello paralelní úlohy na výpočetních uzlech (virtuálních počítačů).</span><span class="sxs-lookup"><span data-stu-id="65c94-131">**DotNetTutorial** is hello client application that interacts with hello Batch and Storage services tooexecute a parallel workload on compute nodes (virtual machines).</span></span> <span data-ttu-id="65c94-132">DotNetTutorial se spouští na místní pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="65c94-132">DotNetTutorial runs on your local workstation.</span></span>
* <span data-ttu-id="65c94-133">**TaskApplication** je hello program, který běží na výpočetních uzlech v Azure tooperform hello samotnou práci.</span><span class="sxs-lookup"><span data-stu-id="65c94-133">**TaskApplication** is hello program that runs on compute nodes in Azure tooperform hello actual work.</span></span> <span data-ttu-id="65c94-134">V ukázce hello `TaskApplication.exe` hello analyzuje text v souboru staženém ze služby Azure Storage (vstupní soubor hello).</span><span class="sxs-lookup"><span data-stu-id="65c94-134">In hello sample, `TaskApplication.exe` parses hello text in a file downloaded from Azure Storage (hello input file).</span></span> <span data-ttu-id="65c94-135">Pak se vytvoří textový soubor (hello výstupního souboru), obsahuje seznam hello první tři slova, která se zobrazují ve vstupním souboru hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-135">Then it produces a text file (hello output file) that contains a list of hello top three words that appear in hello input file.</span></span> <span data-ttu-id="65c94-136">Po vytvoření výstupního souboru hello, TaskApplication odešle soubor tooAzure hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="65c94-136">After it creates hello output file, TaskApplication uploads hello file tooAzure Storage.</span></span> <span data-ttu-id="65c94-137">Díky tomu je k dispozici toohello klientská aplikace ke stažení.</span><span class="sxs-lookup"><span data-stu-id="65c94-137">This makes it available toohello client application for download.</span></span> <span data-ttu-id="65c94-138">TaskApplication běží paralelně v několika výpočetních uzlech v hello služby Batch.</span><span class="sxs-lookup"><span data-stu-id="65c94-138">TaskApplication runs in parallel on multiple compute nodes in hello Batch service.</span></span>

<span data-ttu-id="65c94-139">Hello následující diagram znázorňuje primární operace hello, které provádí klientská aplikace hello, *DotNetTutorial*a hello aplikaci, kterou spouští úkoly hello *TaskApplication*.</span><span class="sxs-lookup"><span data-stu-id="65c94-139">hello following diagram illustrates hello primary operations that are performed by hello client application, *DotNetTutorial*, and hello application that is executed by hello tasks, *TaskApplication*.</span></span> <span data-ttu-id="65c94-140">Tento základní pracovní postup je typický pro mnoho výpočetních řešení, která jsou vytvořená pomocí služby Batch.</span><span class="sxs-lookup"><span data-stu-id="65c94-140">This basic workflow is typical of many compute solutions that are created with Batch.</span></span> <span data-ttu-id="65c94-141">Když nepředvádí všechny funkce, které jsou k dispozici v hello služby Batch, téměř každý scénář Batch zahrnuje části tohoto pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="65c94-141">While it does not demonstrate every feature available in hello Batch service, nearly every Batch scenario includes portions of this workflow.</span></span>

<span data-ttu-id="65c94-142">![Ukázkový pracovní postup služby Batch][8]</span><span class="sxs-lookup"><span data-stu-id="65c94-142">![Batch example workflow][8]</span></span><br/>

[<span data-ttu-id="65c94-143">**Krok 1.**</span><span class="sxs-lookup"><span data-stu-id="65c94-143">**Step 1.**</span></span>](#step-1-create-storage-containers) <span data-ttu-id="65c94-144">Ve službě Azure Blob Storage vytvořte **kontejnery** .</span><span class="sxs-lookup"><span data-stu-id="65c94-144">Create **containers** in Azure Blob Storage.</span></span><br/><span data-ttu-id="65c94-145">
[**Krok 2.**](#step-2-upload-task-application-and-data-files)</span><span class="sxs-lookup"><span data-stu-id="65c94-145">
[**Step 2.**](#step-2-upload-task-application-and-data-files)</span></span> <span data-ttu-id="65c94-146">Odesílání úloh aplikace soubory a vstupní soubory toocontainers.</span><span class="sxs-lookup"><span data-stu-id="65c94-146">Upload task application files and input files toocontainers.</span></span><br/><span data-ttu-id="65c94-147">
[**Krok 3.**](#step-3-create-batch-pool)</span><span class="sxs-lookup"><span data-stu-id="65c94-147">
[**Step 3.**](#step-3-create-batch-pool)</span></span> <span data-ttu-id="65c94-148">Vytvořte **fond** Batch.</span><span class="sxs-lookup"><span data-stu-id="65c94-148">Create a Batch **pool**.</span></span><br/>
  <span data-ttu-id="65c94-149">&nbsp;&nbsp;&nbsp;&nbsp;**3a.**</span><span class="sxs-lookup"><span data-stu-id="65c94-149">&nbsp;&nbsp;&nbsp;&nbsp;**3a.**</span></span> <span data-ttu-id="65c94-150">Hello fondu **StartTask** stahování hello úkolů (TaskApplication) binární soubory toonodes připojí hello fondu.</span><span class="sxs-lookup"><span data-stu-id="65c94-150">hello pool **StartTask** downloads hello task binary files (TaskApplication) toonodes as they join hello pool.</span></span><br/><span data-ttu-id="65c94-151">
[**Krok 4.**](#step-4-create-batch-job)</span><span class="sxs-lookup"><span data-stu-id="65c94-151">
[**Step 4.**](#step-4-create-batch-job)</span></span> <span data-ttu-id="65c94-152">Vytvořte **úlohu** Batch.</span><span class="sxs-lookup"><span data-stu-id="65c94-152">Create a Batch **job**.</span></span><br/><span data-ttu-id="65c94-153">
[**Krok 5.**](#step-5-add-tasks-to-job)</span><span class="sxs-lookup"><span data-stu-id="65c94-153">
[**Step 5.**](#step-5-add-tasks-to-job)</span></span> <span data-ttu-id="65c94-154">Přidat **úlohy** toohello úlohy.</span><span class="sxs-lookup"><span data-stu-id="65c94-154">Add **tasks** toohello job.</span></span><br/>
  <span data-ttu-id="65c94-155">&nbsp;&nbsp;&nbsp;&nbsp;**5a.**</span><span class="sxs-lookup"><span data-stu-id="65c94-155">&nbsp;&nbsp;&nbsp;&nbsp;**5a.**</span></span> <span data-ttu-id="65c94-156">Hello úkoly jsou naplánované tooexecute na uzlech.</span><span class="sxs-lookup"><span data-stu-id="65c94-156">hello tasks are scheduled tooexecute on nodes.</span></span><br/>
    <span data-ttu-id="65c94-157">&nbsp;&nbsp;&nbsp;&nbsp;**5b.**</span><span class="sxs-lookup"><span data-stu-id="65c94-157">&nbsp;&nbsp;&nbsp;&nbsp;**5b.**</span></span> <span data-ttu-id="65c94-158">Každý úkol stáhne svoje vstupní data ze služby Azure Storage a potom zahájí spuštění.</span><span class="sxs-lookup"><span data-stu-id="65c94-158">Each task downloads its input data from Azure Storage, then begins execution.</span></span><br/><span data-ttu-id="65c94-159">
[**Krok 6.**](#step-6-monitor-tasks)</span><span class="sxs-lookup"><span data-stu-id="65c94-159">
[**Step 6.**](#step-6-monitor-tasks)</span></span> <span data-ttu-id="65c94-160">Sledujte úkoly.</span><span class="sxs-lookup"><span data-stu-id="65c94-160">Monitor tasks.</span></span><br/>
  <span data-ttu-id="65c94-161">&nbsp;&nbsp;&nbsp;&nbsp;**6a.**</span><span class="sxs-lookup"><span data-stu-id="65c94-161">&nbsp;&nbsp;&nbsp;&nbsp;**6a.**</span></span> <span data-ttu-id="65c94-162">Jako úkoly při dokončení odesílají svoje výstupní data tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="65c94-162">As tasks are completed, they upload their output data tooAzure Storage.</span></span><br/><span data-ttu-id="65c94-163">
[**Krok 7.**](#step-7-download-task-output)</span><span class="sxs-lookup"><span data-stu-id="65c94-163">
[**Step 7.**](#step-7-download-task-output)</span></span> <span data-ttu-id="65c94-164">Stáhněte si výstup úkolu ze služby Storage.</span><span class="sxs-lookup"><span data-stu-id="65c94-164">Download task output from Storage.</span></span>

<span data-ttu-id="65c94-165">Jak je uvedeno, Ne každé řešení Batch provede právě tyto kroky a může obsahovat i mnohem víc, ale hello *DotNetTutorial* ukázkovou aplikaci předvádí běžné procesy, které jsou v řešení Batch se nenašly.</span><span class="sxs-lookup"><span data-stu-id="65c94-165">As mentioned, not every Batch solution performs these exact steps, and may include many more, but hello *DotNetTutorial* sample application demonstrates common processes found in a Batch solution.</span></span>

## <a name="build-hello-dotnettutorial-sample-project"></a><span data-ttu-id="65c94-166">Sestavení hello *DotNetTutorial* ukázkový projekt</span><span class="sxs-lookup"><span data-stu-id="65c94-166">Build hello *DotNetTutorial* sample project</span></span>
<span data-ttu-id="65c94-167">Předtím, než můžete úspěšně spustit ukázkový text hello, musíte zadat přihlašovací údaje účtu Batch a úložiště v hello *DotNetTutorial* projektu `Program.cs` souboru.</span><span class="sxs-lookup"><span data-stu-id="65c94-167">Before you can successfully run hello sample, you must specify both Batch and Storage account credentials in hello *DotNetTutorial* project's `Program.cs` file.</span></span> <span data-ttu-id="65c94-168">Pokud jste tak ještě neučinili, otevřete hello řešení v sadě Visual Studio dvojitým kliknutím na soubor hello `DotNetTutorial.sln` soubor řešení.</span><span class="sxs-lookup"><span data-stu-id="65c94-168">If you have not done so already, open hello solution in Visual Studio by double-clicking hello `DotNetTutorial.sln` solution file.</span></span> <span data-ttu-id="65c94-169">Nebo ho otevřete v sadě Visual Studio pomocí hello **soubor > Otevřít > projekt nebo řešení** nabídky.</span><span class="sxs-lookup"><span data-stu-id="65c94-169">Or open it from within Visual Studio by using hello **File > Open > Project/Solution** menu.</span></span>

<span data-ttu-id="65c94-170">Otevřete `Program.cs` v rámci hello *DotNetTutorial* projektu.</span><span class="sxs-lookup"><span data-stu-id="65c94-170">Open `Program.cs` within hello *DotNetTutorial* project.</span></span> <span data-ttu-id="65c94-171">Pak zadejte svoje přihlašovací údaje jako zadanou horní hello hello souboru:</span><span class="sxs-lookup"><span data-stu-id="65c94-171">Then add your credentials as specified near hello top of hello file:</span></span>

```csharp
// Update hello Batch and Storage account credential strings below with hello values
// unique tooyour accounts. These are used when constructing connection strings
// for hello Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [!IMPORTANT]
> <span data-ttu-id="65c94-172">Jak je uvedeno nahoře, je nutné zadat aktuálně hello přihlašovací údaje pro **pro obecné účely** účet úložiště ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="65c94-172">As mentioned above, you must currently specify hello credentials for a **general-purpose** storage account in Azure Storage.</span></span> <span data-ttu-id="65c94-173">Aplikace Batch používat úložiště objektů blob v rámci hello **pro obecné účely** účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="65c94-173">Your Batch applications use blob storage within hello **general-purpose** storage account.</span></span> <span data-ttu-id="65c94-174">Nezadávejte hello pověření pro účet úložiště, který byl vytvořen tak, že vyberete hello *úložiště objektů Blob* typ účtu.</span><span class="sxs-lookup"><span data-stu-id="65c94-174">Do not specify hello credentials for a Storage account that was created by selecting hello *Blob storage* account type.</span></span>
>
>

<span data-ttu-id="65c94-175">Přihlašovací údaje účtu Batch a Storage v rámci hello okně účtu každé služby můžete najít v hello [portál Azure][azure_portal]:</span><span class="sxs-lookup"><span data-stu-id="65c94-175">You can find your Batch and Storage account credentials within hello account blade of each service in hello [Azure portal][azure_portal]:</span></span>

<span data-ttu-id="65c94-176">![Přihlašovací údaje hello portálu služby batch][9]
![přihlašovací údaje Storage na portálu hello][10]</span><span class="sxs-lookup"><span data-stu-id="65c94-176">![Batch credentials in hello portal][9]
![Storage credentials in hello portal][10]</span></span><br/>

<span data-ttu-id="65c94-177">Teď, když aktualizujete hello projektu pomocí svých přihlašovacích údajů, klikněte pravým tlačítkem v Průzkumníku řešení hello a klikněte na tlačítko **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="65c94-177">Now that you've updated hello project with your credentials, right-click hello solution in Solution Explorer and click **Build Solution**.</span></span> <span data-ttu-id="65c94-178">Pokud se zobrazí výzva, potvrďte hello obnovení všech balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="65c94-178">Confirm hello restoration of any NuGet packages, if you're prompted.</span></span>

> [!TIP]
> <span data-ttu-id="65c94-179">Pokud hello balíčky NuGet automaticky neobnoví, nebo pokud se zobrazí chyby o balíčcích hello toorestore selhání, ujistěte se, že máte hello [Správce balíčků NuGet] [ nuget_packagemgr] nainstalována.</span><span class="sxs-lookup"><span data-stu-id="65c94-179">If hello NuGet packages are not automatically restored, or if you see errors about a failure toorestore hello packages, ensure that you have hello [NuGet Package Manager][nuget_packagemgr] installed.</span></span> <span data-ttu-id="65c94-180">Potom povolte stažení chybějících balíčků hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-180">Then enable hello download of missing packages.</span></span> <span data-ttu-id="65c94-181">V tématu [povolení obnovy balíčků během sestavení] [ nuget_restore] tooenable stahování balíčku.</span><span class="sxs-lookup"><span data-stu-id="65c94-181">See [Enabling Package Restore During Build][nuget_restore] tooenable package download.</span></span>
>
>

<span data-ttu-id="65c94-182">V následujících částech hello jsme rozdělit hello ukázkové aplikace hello kroky, které se provádí tooprocess úloh ve hello služby Batch a popisují tyto kroky si podrobně.</span><span class="sxs-lookup"><span data-stu-id="65c94-182">In hello following sections, we break down hello sample application into hello steps that it performs tooprocess a workload in hello Batch service, and discuss those steps in detail.</span></span> <span data-ttu-id="65c94-183">Doporučujeme vám toorefer toohello otevřete řešení v sadě Visual Studio při procházení hello zbývající části tohoto článku, vzhledem k tomu, že každý jednotlivý řádek kódu v ukázce hello popsané.</span><span class="sxs-lookup"><span data-stu-id="65c94-183">We encourage you toorefer toohello open solution in Visual Studio while you work your way through hello rest of this article, since not every line of code in hello sample is discussed.</span></span>

<span data-ttu-id="65c94-184">Přejděte toohello horní části hello `MainAsync` metoda v hello *DotNetTutorial* projektu `Program.cs` souboru toostart s krokem 1.</span><span class="sxs-lookup"><span data-stu-id="65c94-184">Navigate toohello top of hello `MainAsync` method in hello *DotNetTutorial* project's `Program.cs` file toostart with Step 1.</span></span> <span data-ttu-id="65c94-185">Každý krok níže, pak přibližně způsobem hello rozšiřování metoda volá `MainAsync`.</span><span class="sxs-lookup"><span data-stu-id="65c94-185">Each step below then roughly follows hello progression of method calls in `MainAsync`.</span></span>

## <a name="step-1-create-storage-containers"></a><span data-ttu-id="65c94-186">Krok 1: Vytvoření kontejnerů služby Storage</span><span class="sxs-lookup"><span data-stu-id="65c94-186">Step 1: Create Storage containers</span></span>
<span data-ttu-id="65c94-187">![Vytvoření kontejnerů ve službě Azure Storage][1]
</span><span class="sxs-lookup"><span data-stu-id="65c94-187">![Create containers in Azure Storage][1]
</span></span><br/>

<span data-ttu-id="65c94-188">Batch obsahuje vestavěnou podporu pro komunikaci se službou Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="65c94-188">Batch includes built-in support for interacting with Azure Storage.</span></span> <span data-ttu-id="65c94-189">Kontejnery v účtu úložiště bude poskytovat hello soubory potřebné hello úlohy, které běží v účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="65c94-189">Containers in your Storage account will provide hello files needed by hello tasks that run in your Batch account.</span></span> <span data-ttu-id="65c94-190">Hello kontejnery také poskytují místě toostore hello výstupní data, která hello úkoly vytvářejí.</span><span class="sxs-lookup"><span data-stu-id="65c94-190">hello containers also provide a place toostore hello output data that hello tasks produce.</span></span> <span data-ttu-id="65c94-191">Hello nejprve thing hello *DotNetTutorial* klientská aplikace se vytvoří tři kontejnery ve [Azure Blob Storage](../storage/common/storage-introduction.md):</span><span class="sxs-lookup"><span data-stu-id="65c94-191">hello first thing hello *DotNetTutorial* client application does is create three containers in [Azure Blob Storage](../storage/common/storage-introduction.md):</span></span>

* <span data-ttu-id="65c94-192">**aplikace**: Tento kontejner bude ukládat aplikace hello spuštěná hello úlohy, jakož i veškeré její závislé, například knihovny DLL.</span><span class="sxs-lookup"><span data-stu-id="65c94-192">**application**: This container will store hello application run by hello tasks, as well as any of its dependencies, such as DLLs.</span></span>
* <span data-ttu-id="65c94-193">**vstupní**: budou úkoly stahovat z hello hello datové soubory tooprocess *vstupní* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="65c94-193">**input**: Tasks will download hello data files tooprocess from hello *input* container.</span></span>
* <span data-ttu-id="65c94-194">**výstup**: když úkoly dokončí zpracování vstupního souboru, odešlou výsledky toohello hello *výstup* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="65c94-194">**output**: When tasks complete input file processing, they will upload hello results toohello *output* container.</span></span>

<span data-ttu-id="65c94-195">V pořadí toointeract s úložiště účtu a vytvoření kontejnerů používáme hello [Klientská knihovna pro úložiště Azure pro .NET][net_api_storage].</span><span class="sxs-lookup"><span data-stu-id="65c94-195">In order toointeract with a Storage account and create containers, we use hello [Azure Storage Client Library for .NET][net_api_storage].</span></span> <span data-ttu-id="65c94-196">Vytvoříme účet toohello odkaz s [CloudStorageAccount][net_cloudstorageaccount]a z té vytvoříme [CloudBlobClient][net_cloudblobclient]:</span><span class="sxs-lookup"><span data-stu-id="65c94-196">We create a reference toohello account with [CloudStorageAccount][net_cloudstorageaccount], and from that create a [CloudBlobClient][net_cloudblobclient]:</span></span>

```csharp
// Construct hello Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve hello storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create hello blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

<span data-ttu-id="65c94-197">Používáme hello `blobClient` odkazovat v hello aplikaci a předáváme jako parametr metody tooseveral.</span><span class="sxs-lookup"><span data-stu-id="65c94-197">We use hello `blobClient` reference throughout hello application and pass it as a parameter tooseveral methods.</span></span> <span data-ttu-id="65c94-198">Příkladem je v bloku kódu hello, který následuje hello výše, kde voláme `CreateContainerIfNotExistAsync` tooactually vytvoření kontejnerů hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-198">An example of this is in hello code block that immediately follows hello above, where we call `CreateContainerIfNotExistAsync` tooactually create hello containers.</span></span>

```csharp
// Use hello blob client toocreate hello containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

<span data-ttu-id="65c94-199">Po vytvoření kontejnerů hello hello aplikaci teď můžete nahrát hello soubory, které se použijí tak, že úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-199">Once hello containers have been created, hello application can now upload hello files that will be used by hello tasks.</span></span>

> [!TIP]
> <span data-ttu-id="65c94-200">[Jak toouse úložiště Blob z .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) nabízí pěkný přehled o práci s Azure Storage kontejnery a objekty BLOB ve službě.</span><span class="sxs-lookup"><span data-stu-id="65c94-200">[How toouse Blob Storage from .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) provides a good overview of working with Azure Storage containers and blobs.</span></span> <span data-ttu-id="65c94-201">Musí být v hello horní části seznamu čtení jako začátek práce s Batch.</span><span class="sxs-lookup"><span data-stu-id="65c94-201">It should be near hello top of your reading list as you start working with Batch.</span></span>
>
>

## <a name="step-2-upload-task-application-and-data-files"></a><span data-ttu-id="65c94-202">Krok 2: Nahrání aplikačních a datových souborů úkolů</span><span class="sxs-lookup"><span data-stu-id="65c94-202">Step 2: Upload task application and data files</span></span>
<span data-ttu-id="65c94-203">![Soubory toocontainers odeslání úloh aplikace a vstupních (data)][2]
</span><span class="sxs-lookup"><span data-stu-id="65c94-203">![Upload task application and input (data) files toocontainers][2]
</span></span><br/>

<span data-ttu-id="65c94-204">V souboru hello operace nahrávání *DotNetTutorial* nejdřív definuje kolekce **aplikace** a **vstupní** cesty k souborům, které jsou v místním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-204">In hello file upload operation, *DotNetTutorial* first defines collections of **application** and **input** file paths as they exist on hello local machine.</span></span> <span data-ttu-id="65c94-205">Pak odešle tyto soubory toohello kontejnery, které jste vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-205">Then it uploads these files toohello containers that you created in hello previous step.</span></span>

```csharp
// Paths toohello executable and its dependencies that will be executed by hello tasks
List<string> applicationFilePaths = new List<string>
{
    // hello DotNetTutorial project includes a project reference tooTaskApplication,
    // allowing us toodetermine hello path of hello task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// hello collection of data files that are toobe processed by hello tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload hello application and its dependencies tooAzure Storage. This is the
// application that will process hello data files, and will be executed by each
// of hello tasks on hello compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload hello data files. This is hello data that will be processed by each of
// hello tasks that are executed on hello compute nodes within hello pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

<span data-ttu-id="65c94-206">Existují dvě metody v `Program.cs` které se účastní procesu nahrávání hello:</span><span class="sxs-lookup"><span data-stu-id="65c94-206">There are two methods in `Program.cs` that are involved in hello upload process:</span></span>

* <span data-ttu-id="65c94-207">`UploadFilesToContainerAsync`: Tato metoda vrátí kolekci [ResourceFile] [ net_resourcefile] (viz následující popis) a interně volá `UploadFileToContainerAsync` tooupload každý soubor, který je předán hello *filePaths* parametr.</span><span class="sxs-lookup"><span data-stu-id="65c94-207">`UploadFilesToContainerAsync`: This method returns a collection of [ResourceFile][net_resourcefile] objects (discussed below) and internally calls `UploadFileToContainerAsync` tooupload each file that is passed in hello *filePaths* parameter.</span></span>
* <span data-ttu-id="65c94-208">`UploadFileToContainerAsync`: Toto je hello metoda, která ve skutečnosti provádí nahrávání souborů hello a vytvoří hello [ResourceFile] [ net_resourcefile] objekty.</span><span class="sxs-lookup"><span data-stu-id="65c94-208">`UploadFileToContainerAsync`: This is hello method that actually performs hello file upload and creates hello [ResourceFile][net_resourcefile] objects.</span></span> <span data-ttu-id="65c94-209">Po nahrání souboru hello, získá sdílený přístupový podpis (SAS) pro soubor hello a vrátí objekt ResourceFile, který ho zastupuje.</span><span class="sxs-lookup"><span data-stu-id="65c94-209">After uploading hello file, it obtains a shared access signature (SAS) for hello file and returns a ResourceFile object that represents it.</span></span> <span data-ttu-id="65c94-210">Sdílené přístupové podpisy jsou také popsány níže.</span><span class="sxs-lookup"><span data-stu-id="65c94-210">Shared access signatures are also discussed below.</span></span>

```csharp
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} toocontainer [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath);

        // Set hello expiry time and permissions for hello blob shared access signature.
        // In this case, no start time is specified, so hello shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct hello SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a><span data-ttu-id="65c94-211">ResourceFiles</span><span class="sxs-lookup"><span data-stu-id="65c94-211">ResourceFiles</span></span>
<span data-ttu-id="65c94-212">A [ResourceFile] [ net_resourcefile] poskytuje úlohy v dávce s soubor tooa hello adresy URL ve službě Azure Storage, který je stažený tooa výpočetního uzlu před spuštěním této úlohy.</span><span class="sxs-lookup"><span data-stu-id="65c94-212">A [ResourceFile][net_resourcefile] provides tasks in Batch with hello URL tooa file in Azure Storage that is downloaded tooa compute node before that task is run.</span></span> <span data-ttu-id="65c94-213">Hello [ResourceFile.BlobSource] [ net_resourcefile_blobsource] vlastnost určuje úplnou adresu URL hello hello souboru, protože existuje ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="65c94-213">hello [ResourceFile.BlobSource][net_resourcefile_blobsource] property specifies hello full URL of hello file as it exists in Azure Storage.</span></span> <span data-ttu-id="65c94-214">Adresa URL Hello mohou obsahovat také sdílený přístupový podpis (SAS), který poskytuje soubor toohello zabezpečený přístup.</span><span class="sxs-lookup"><span data-stu-id="65c94-214">hello URL may also include a shared access signature (SAS) that provides secure access toohello file.</span></span> <span data-ttu-id="65c94-215">Většina typů úkolů v rámci v Batch .NET obsahuje vlastnost *ResourceFiles* včetně:</span><span class="sxs-lookup"><span data-stu-id="65c94-215">Most tasks types within Batch .NET include a *ResourceFiles* property, including:</span></span>

* <span data-ttu-id="65c94-216">[CloudTask][net_task]</span><span class="sxs-lookup"><span data-stu-id="65c94-216">[CloudTask][net_task]</span></span>
* <span data-ttu-id="65c94-217">[StartTask][net_pool_starttask]</span><span class="sxs-lookup"><span data-stu-id="65c94-217">[StartTask][net_pool_starttask]</span></span>
* <span data-ttu-id="65c94-218">[JobPreparationTask][net_jobpreptask]</span><span class="sxs-lookup"><span data-stu-id="65c94-218">[JobPreparationTask][net_jobpreptask]</span></span>
* <span data-ttu-id="65c94-219">[JobReleaseTask][net_jobreltask]</span><span class="sxs-lookup"><span data-stu-id="65c94-219">[JobReleaseTask][net_jobreltask]</span></span>

<span data-ttu-id="65c94-220">Hello ukázková aplikace DotNetTutorial nepoužívá hello JobPreparationTask nebo JobReleaseTask typy úloh, ale si můžete přečíst více o nich [spustit přípravy a dokončení úlohy na Azure Batch výpočetních uzlech](batch-job-prep-release.md).</span><span class="sxs-lookup"><span data-stu-id="65c94-220">hello DotNetTutorial sample application does not use hello JobPreparationTask or JobReleaseTask task types, but you can read more about them in [Run job preparation and completion tasks on Azure Batch compute nodes](batch-job-prep-release.md).</span></span>

### <a name="shared-access-signature-sas"></a><span data-ttu-id="65c94-221">Sdílený přístupový podpis (SAS)</span><span class="sxs-lookup"><span data-stu-id="65c94-221">Shared access signature (SAS)</span></span>
<span data-ttu-id="65c94-222">Sdílené přístupové podpisy jsou řetězce, které – když jsou součástí adresy URL – poskytnout zabezpečený přístup toocontainers a objekty BLOB ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="65c94-222">Shared access signatures are strings which—when included as part of a URL—provide secure access toocontainers and blobs in Azure Storage.</span></span> <span data-ttu-id="65c94-223">Hello aplikace DotNetTutorial používá objektu blob i a kontejner sdíleného přístupu podpis adresy URL a ukazuje, jak tooobtain tyto sdílený přístup k řetězce podpisu z hello služby úložiště.</span><span class="sxs-lookup"><span data-stu-id="65c94-223">hello DotNetTutorial application uses both blob and container shared access signature URLs, and demonstrates how tooobtain these shared access signature strings from hello Storage service.</span></span>

* <span data-ttu-id="65c94-224">**Sdílené přístupové podpisy objektů blob**: StartTask fondu hello v aplikaci DotNetTutorial používá objekt blob sdílené přístupové podpisy při stahování aplikačních binárních souborů hello a vstupních datových souborů ze služby Storage (viz krok 3 níže).</span><span class="sxs-lookup"><span data-stu-id="65c94-224">**Blob shared access signatures**: hello pool's StartTask in DotNetTutorial uses blob shared access signatures when it downloads hello application binaries and input data files from Storage (see Step #3 below).</span></span> <span data-ttu-id="65c94-225">Hello `UploadFileToContainerAsync` metoda v DotNetTutorial `Program.cs` obsahuje hello kód, který získá sdílený přístupový podpis jednotlivých objektů blob.</span><span class="sxs-lookup"><span data-stu-id="65c94-225">hello `UploadFileToContainerAsync` method in DotNetTutorial's `Program.cs` contains hello code that obtains each blob's shared access signature.</span></span> <span data-ttu-id="65c94-226">Dělá to tak, že volá [CloudBlob.GetSharedAccessSignature][net_sas_blob].</span><span class="sxs-lookup"><span data-stu-id="65c94-226">It does so by calling [CloudBlob.GetSharedAccessSignature][net_sas_blob].</span></span>
* <span data-ttu-id="65c94-227">**Sdílené přístupové podpisy kontejnerů**: když každý úkol dokončí svojí práci ve výpočetním uzlu hello, odešle svůj výstupní soubor toohello *výstup* kontejneru ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="65c94-227">**Container shared access signatures**: As each task finishes its work on hello compute node, it uploads its output file toohello *output* container in Azure Storage.</span></span> <span data-ttu-id="65c94-228">toodo Ano, používá TaskApplication sdílený přístupový podpis kontejneru, který poskytuje přístup pro zápis toohello kontejneru jako součást hello cesty při nahrávání souboru hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-228">toodo so, TaskApplication uses a container shared access signature that provides write access toohello container as part of hello path when it uploads hello file.</span></span> <span data-ttu-id="65c94-229">Získání sdíleného přístupového podpisu kontejneru hello se provádí podobným způsobem jako při získání objektu blob hello sdílený přístupový podpis.</span><span class="sxs-lookup"><span data-stu-id="65c94-229">Obtaining hello container shared access signature is done in a similar fashion as when obtaining hello blob shared access signature.</span></span> <span data-ttu-id="65c94-230">V aplikaci DotNetTutorial zjistíte, že hello `GetContainerSasUrl` volání metod helper [CloudBlobContainer.GetSharedAccessSignature] [ net_sas_container] toodo tak.</span><span class="sxs-lookup"><span data-stu-id="65c94-230">In DotNetTutorial, you will find that hello `GetContainerSasUrl` helper method calls [CloudBlobContainer.GetSharedAccessSignature][net_sas_container] toodo so.</span></span> <span data-ttu-id="65c94-231">Další informace o tom, jak TaskApplication používá hello kontejneru sdíleného přístupového podpisu v "krok 6: sledování úkolů."</span><span class="sxs-lookup"><span data-stu-id="65c94-231">You'll read more about how TaskApplication uses hello container shared access signature in "Step 6: Monitor Tasks."</span></span>

> [!TIP]
> <span data-ttu-id="65c94-232">Podívejte se na dvě části řady hello na sdílených přístupových podpisů [část 1: pochopení hello sdílené model podpis (SAS) přístupu](../storage/common/storage-dotnet-shared-access-signature-part-1.md) a [část 2: vytvoření a používání sdíleného přístupového podpisu (SAS) pomocí úložiště objektů Blob](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), toolearn více o zajišťování bezpečného přístupu toodata ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="65c94-232">Check out hello two-part series on shared access signatures, [Part 1: Understanding hello shared access signature (SAS) model](../storage/common/storage-dotnet-shared-access-signature-part-1.md) and [Part 2: Create and use a shared access signature (SAS) with Blob storage](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), toolearn more about providing secure access toodata in your Storage account.</span></span>
>
>

## <a name="step-3-create-batch-pool"></a><span data-ttu-id="65c94-233">Krok 3: Vytvoření fondu služby Batch</span><span class="sxs-lookup"><span data-stu-id="65c94-233">Step 3: Create Batch pool</span></span>
<span data-ttu-id="65c94-234">![Vytvoření fondu Batch][3]
</span><span class="sxs-lookup"><span data-stu-id="65c94-234">![Create a Batch pool][3]
</span></span><br/>

<span data-ttu-id="65c94-235">**Fond** Batch je kolekce výpočetních uzlů (virtuálních počítačů), na kterých služba Batch provádí úkoly z úlohy.</span><span class="sxs-lookup"><span data-stu-id="65c94-235">A Batch **pool** is a collection of compute nodes (virtual machines) on which Batch executes a job's tasks.</span></span>

<span data-ttu-id="65c94-236">Po nahrání hello aplikace a data souborů toohello účet úložiště se rozhraní API úložiště Azure, *DotNetTutorial* zahájí provádění služba Batch toohello volání rozhraní API poskytované knihovny Batch .NET hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-236">After uploading hello application and data files toohello Storage account with Azure Storage APIs, *DotNetTutorial* begins making calls toohello Batch service with APIs provided by hello Batch .NET library.</span></span> <span data-ttu-id="65c94-237">Hello kód nejprve vytvoří [BatchClient][net_batchclient]:</span><span class="sxs-lookup"><span data-stu-id="65c94-237">hello code first creates a [BatchClient][net_batchclient]:</span></span>

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

<span data-ttu-id="65c94-238">V dalším kroku hello ukázka vytvoří fond výpočetních uzlů v účtu Batch hello pomocí volání příliš`CreatePoolIfNotExistsAsync`.</span><span class="sxs-lookup"><span data-stu-id="65c94-238">Next, hello sample creates a pool of compute nodes in hello Batch account with a call too`CreatePoolIfNotExistsAsync`.</span></span> <span data-ttu-id="65c94-239">`CreatePoolIfNotExistsAsync`hello používá [BatchClient.PoolOperations.CreatePool] [ net_pool_create] metoda toocreate nový fond v hello služby Batch:</span><span class="sxs-lookup"><span data-stu-id="65c94-239">`CreatePoolIfNotExistsAsync` uses hello [BatchClient.PoolOperations.CreatePool][net_pool_create] method toocreate a new pool in hello Batch service:</span></span>

```csharp
private static async Task CreatePoolIfNotExistAsync(BatchClient batchClient, string poolId, IList<ResourceFile> resourceFiles)
{
    CloudPool pool = null;
    try
    {
        Console.WriteLine("Creating pool [{0}]...", poolId);

        // Create hello unbound pool. Until we call CloudPool.Commit() or CommitAsync(), no pool is actually created in the
        // Batch service. This CloudPool instance is therefore considered "unbound," and we can modify its properties.
        pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicatedComputeNodes: 3,                                             // 3 compute nodes
            virtualMachineSize: "small",                                                // single-core, 1.75 GB memory, 225 GB disk
            cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));   // Windows Server 2012 R2

        // Create and assign hello StartTask that will be executed when compute nodes join hello pool.
        // In this case, we copy hello StartTask's resource files (that will be automatically downloaded
        // toohello node by hello StartTask) into hello shared directory that all tasks will have access to.
        pool.StartTask = new StartTask
        {
            // Specify a command line for hello StartTask that copies hello task application files toothe
            // node's shared directory. Every compute node in a Batch pool is configured with a number
            // of pre-defined environment variables that can be referenced by commands or applications
            // run by tasks.

            // Since a successful execution of robocopy can return a non-zero exit code (e.g. 1 when one or
            // more files were successfully copied) we need toomanually exit with a 0 for Batch toorecognize
            // StartTask execution success.
            CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
            ResourceFiles = resourceFiles,
            WaitForSuccess = true
        };

        await pool.CommitAsync();
    }
    catch (BatchException be)
    {
        // Swallow hello specific error code PoolExists since that is expected if hello pool already exists
        if (be.RequestInformation?.BatchError != null && be.RequestInformation.BatchError.Code == BatchErrorCodeStrings.PoolExists)
        {
            Console.WriteLine("hello pool {0} already existed when we tried toocreate it", poolId);
        }
        else
        {
            throw; // Any other exception is unexpected
        }
    }
}
```

<span data-ttu-id="65c94-240">Při vytváření fondu s [CreatePool][net_pool_create], můžete zadat několik parametrů, třeba hello počet výpočetních uzlů, hello [velikost uzlů hello](../cloud-services/cloud-services-sizes-specs.md), a hello operační uzlů systém.</span><span class="sxs-lookup"><span data-stu-id="65c94-240">When you create a pool with [CreatePool][net_pool_create], you specify several parameters such as hello number of compute nodes, hello [size of hello nodes](../cloud-services/cloud-services-sizes-specs.md), and hello nodes' operating system.</span></span> <span data-ttu-id="65c94-241">V *DotNetTutorial*, používáme [CloudServiceConfiguration] [ net_cloudserviceconfiguration] toospecify Windows Server 2012 R2 z [cloudové služby](../cloud-services/cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="65c94-241">In *DotNetTutorial*, we use [CloudServiceConfiguration][net_cloudserviceconfiguration] toospecify Windows Server 2012 R2 from [Cloud Services](../cloud-services/cloud-services-guestos-update-matrix.md).</span></span> 

<span data-ttu-id="65c94-242">Můžete také vytvářet fondy výpočetních uzlů, které jsou virtuální počítače (VM) Azure zadáním hello [VirtualMachineConfiguration] [ net_virtualmachineconfiguration] fondu.</span><span class="sxs-lookup"><span data-stu-id="65c94-242">You can also create pools of compute nodes that are Azure Virtual Machines (VMs) by specifying hello [VirtualMachineConfiguration][net_virtualmachineconfiguration] for your pool.</span></span> <span data-ttu-id="65c94-243">Fond výpočetních uzlů virtuálních počítačů můžete vytvořit z [image systému Linux](batch-linux-nodes.md) nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="65c94-243">You can create a pool of VM compute nodes from either Windows or [Linux images](batch-linux-nodes.md).</span></span> <span data-ttu-id="65c94-244">Hello zdroj pro vaše Image virtuálních počítačů může být buď:</span><span class="sxs-lookup"><span data-stu-id="65c94-244">hello source for your VM images can be either:</span></span>

- <span data-ttu-id="65c94-245">Hello [Marketplace virtuálních počítačů Azure][vm_marketplace], který poskytuje bitové kopie systému Windows a Linux, které jsou připravené k použití.</span><span class="sxs-lookup"><span data-stu-id="65c94-245">hello [Azure Virtual Machines Marketplace][vm_marketplace], which provides both Windows and Linux images that are ready-to-use.</span></span> 
- <span data-ttu-id="65c94-246">Vlastní image, kterou připravíte a poskytnete.</span><span class="sxs-lookup"><span data-stu-id="65c94-246">A custom image that you prepare and provide.</span></span> <span data-ttu-id="65c94-247">Další informace o vlastních imagích najdete v tématu [Vývoj rozsáhlých paralelních výpočetních řešení pomocí služby Batch](batch-api-basics.md#pool).</span><span class="sxs-lookup"><span data-stu-id="65c94-247">For more details about custom images, see [Develop large-scale parallel compute solutions with Batch](batch-api-basics.md#pool).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="65c94-248">Za výpočetní prostředky ve službě Batch vám budou účtované poplatky.</span><span class="sxs-lookup"><span data-stu-id="65c94-248">You are charged for compute resources in Batch.</span></span> <span data-ttu-id="65c94-249">můžete snížit náklady toominimize `targetDedicatedComputeNodes` too1 před spuštěním ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-249">toominimize costs, you can lower `targetDedicatedComputeNodes` too1 before you run hello sample.</span></span>
>
>

<span data-ttu-id="65c94-250">Spolu s těmito fyzickými vlastnostmi uzlu můžete určit také [StartTask] [ net_pool_starttask] hello fondu.</span><span class="sxs-lookup"><span data-stu-id="65c94-250">Along with these physical node properties, you may also specify a [StartTask][net_pool_starttask] for hello pool.</span></span> <span data-ttu-id="65c94-251">Hello StartTask se spustí na každém uzlu jako takový uzel připojí hello fondu a pokaždé, když je uzel restartován.</span><span class="sxs-lookup"><span data-stu-id="65c94-251">hello StartTask executes on each node as that node joins hello pool, and each time a node is restarted.</span></span> <span data-ttu-id="65c94-252">Hello StartTask je zvláště užitečná pro instalaci aplikací na výpočetní uzly předchozí toohello provedení úlohy.</span><span class="sxs-lookup"><span data-stu-id="65c94-252">hello StartTask is especially useful for installing applications on compute nodes prior toohello execution of tasks.</span></span> <span data-ttu-id="65c94-253">Například pokud vaše úlohy zpracování dat pomocí skriptů Python, můžete použít StartTask tooinstall Pythonu na výpočetní uzly hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-253">For example, if your tasks process data by using Python scripts, you could use a StartTask tooinstall Python on hello compute nodes.</span></span>

<span data-ttu-id="65c94-254">V této ukázkové aplikaci hello StartTask zkopíruje hello soubory, které stáhne ze služby Storage (které jsou určené pomocí hello [StartTask][net_starttask].[ ResourceFiles] [ net_starttask_resourcefiles] vlastnost) z hello StartTask pracovní adresář toohello sdíleného adresáře, *všechny* úkoly spuštěné na uzlu hello přístup.</span><span class="sxs-lookup"><span data-stu-id="65c94-254">In this sample application, hello StartTask copies hello files that it downloads from Storage (which are specified by using hello [StartTask][net_starttask].[ResourceFiles][net_starttask_resourcefiles] property) from hello StartTask working directory toohello shared directory that *all* tasks running on hello node can access.</span></span> <span data-ttu-id="65c94-255">V podstatě zkopíruje `TaskApplication.exe` a jeho závislosti toohello sdíleného adresáře v každém uzlu jako hello uzel připojí hello fondu, tak, aby všechny úlohy, které běží na uzlu hello k němu přístup.</span><span class="sxs-lookup"><span data-stu-id="65c94-255">Essentially, this copies `TaskApplication.exe` and its dependencies toohello shared directory on each node as hello node joins hello pool, so that any tasks that run on hello node can access it.</span></span>

> [!TIP]
> <span data-ttu-id="65c94-256">Hello **balíčky aplikací** funkce služby Azure Batch poskytuje jiný způsob tooget aplikaci na hello výpočetních uzlů ve fondu.</span><span class="sxs-lookup"><span data-stu-id="65c94-256">hello **application packages** feature of Azure Batch provides another way tooget your application onto hello compute nodes in a pool.</span></span> <span data-ttu-id="65c94-257">V tématu [nasazení uzly toocompute aplikací pomocí balíčků aplikací Batch](batch-application-packages.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="65c94-257">See [Deploy applications toocompute nodes with Batch application packages](batch-application-packages.md) for details.</span></span>
>
>

<span data-ttu-id="65c94-258">Také je zajímavé ve fragmentu kódu hello výše hello použití dvou proměnných prostředí v hello *CommandLine* vlastnost hello StartTask: `%AZ_BATCH_TASK_WORKING_DIR%` a `%AZ_BATCH_NODE_SHARED_DIR%`.</span><span class="sxs-lookup"><span data-stu-id="65c94-258">Also notable in hello code snippet above is hello use of two environment variables in hello *CommandLine* property of hello StartTask: `%AZ_BATCH_TASK_WORKING_DIR%` and `%AZ_BATCH_NODE_SHARED_DIR%`.</span></span> <span data-ttu-id="65c94-259">Každý výpočetní uzel v rámci fondu Batch je automaticky nakonfigurovaný pomocí řady proměnných prostředí, které jsou specifické tooBatch.</span><span class="sxs-lookup"><span data-stu-id="65c94-259">Each compute node within a Batch pool is automatically configured with several environment variables that are specific tooBatch.</span></span> <span data-ttu-id="65c94-260">Jakýkoli proces spuštěný úkolem má přístup k proměnným prostředí toothese.</span><span class="sxs-lookup"><span data-stu-id="65c94-260">Any process that is executed by a task has access toothese environment variables.</span></span>

> [!TIP]
> <span data-ttu-id="65c94-261">toofind Další informace o hello proměnné prostředí, které jsou dostupné na výpočetní uzlech ve fondu Batch a informace o pracovních adresářích úkolu najdete v části hello [nastavení prostředí pro úkoly](batch-api-basics.md#environment-settings-for-tasks) a [souborů a adresářů ](batch-api-basics.md#files-and-directories) části hello [přehled funkcí Batch pro vývojáře](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="65c94-261">toofind out more about hello environment variables that are available on compute nodes in a Batch pool, and information on task working directories, see hello [Environment settings for tasks](batch-api-basics.md#environment-settings-for-tasks) and [Files and directories](batch-api-basics.md#files-and-directories) sections in hello [Batch feature overview for developers](batch-api-basics.md).</span></span>
>
>

## <a name="step-4-create-batch-job"></a><span data-ttu-id="65c94-262">Krok 4: Vytvoření úlohy Batch</span><span class="sxs-lookup"><span data-stu-id="65c94-262">Step 4: Create Batch job</span></span>
<span data-ttu-id="65c94-263">![Vytvoření úlohy Batch][4]</span><span class="sxs-lookup"><span data-stu-id="65c94-263">![Create Batch job][4]</span></span><br/>

<span data-ttu-id="65c94-264">**Úloha** Batch je kolekcí úkolů a je přidružená k fondu výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="65c94-264">A Batch **job** is a collection of tasks, and is associated with a pool of compute nodes.</span></span> <span data-ttu-id="65c94-265">Hello úlohy pro úlohu provést hello přidružené fondu výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="65c94-265">hello tasks in a job execute on hello associated pool's compute nodes.</span></span>

<span data-ttu-id="65c94-266">Úlohu můžete použít nejen k uspořádání a sledování úkolů v souvisejících úlohách, ale také pro nastavení určitých omezení – například maximálního runtime hello hello úlohy (a rozšíření, její úkoly) a také priority úloh ve vztahu tooother úloh v hello Batch účet.</span><span class="sxs-lookup"><span data-stu-id="65c94-266">You can use a job not only for organizing and tracking tasks in related workloads, but also for imposing certain constraints--such as hello maximum runtime for hello job (and by extension, its tasks) as well as job priority in relation tooother jobs in hello Batch account.</span></span> <span data-ttu-id="65c94-267">V tomto příkladu však hello úlohy je přidruženo pouze hello fondu, který byl vytvořen v kroku #3.</span><span class="sxs-lookup"><span data-stu-id="65c94-267">In this example, however, hello job is associated only with hello pool that was created in step #3.</span></span> <span data-ttu-id="65c94-268">Žádné další vlastnosti se nekonfigurují.</span><span class="sxs-lookup"><span data-stu-id="65c94-268">No additional properties are configured.</span></span>

<span data-ttu-id="65c94-269">Všechny úlohy Batch jsou přidružené ke konkrétnímu fondu.</span><span class="sxs-lookup"><span data-stu-id="65c94-269">All Batch jobs are associated with a specific pool.</span></span> <span data-ttu-id="65c94-270">Toto přidružení označuje uzly, které hello úlohy se spustí na.</span><span class="sxs-lookup"><span data-stu-id="65c94-270">This association indicates which nodes hello job's tasks will execute on.</span></span> <span data-ttu-id="65c94-271">Toto určíte pomocí hello [CloudJob.PoolInformation] [ net_job_poolinfo] vlastnost, jak je znázorněno v následující fragment kódu hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-271">You specify this by using hello [CloudJob.PoolInformation][net_job_poolinfo] property, as shown in hello code snippet below.</span></span>

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

<span data-ttu-id="65c94-272">Teď, když vytvořil úlohu úlohy se přidají pracovní tooperform hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-272">Now that a job has been created, tasks are added tooperform hello work.</span></span>

## <a name="step-5-add-tasks-toojob"></a><span data-ttu-id="65c94-273">Krok 5: Přidejte toojob úlohy</span><span class="sxs-lookup"><span data-stu-id="65c94-273">Step 5: Add tasks toojob</span></span>
<span data-ttu-id="65c94-274">![Přidat toojob úlohy][5]</span><span class="sxs-lookup"><span data-stu-id="65c94-274">![Add tasks toojob][5]</span></span><br/><span data-ttu-id="65c94-275">
*(1) úkoly jsou přidány toohello úlohy, hello (2) úkoly jsou naplánované toorun na uzlech a hello (3) úkoly stahují hello data souborů tooprocess*</span><span class="sxs-lookup"><span data-stu-id="65c94-275">
*(1) Tasks are added toohello job, (2) hello tasks are scheduled toorun on nodes, and (3) hello tasks download hello data files tooprocess*</span></span>

<span data-ttu-id="65c94-276">Batch **úlohy** jsou hello jednotlivé jednotky práce, které jsou spouštěny na hello výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="65c94-276">Batch **tasks** are hello individual units of work that execute on hello compute nodes.</span></span> <span data-ttu-id="65c94-277">Úkol má příkazový řádek a spouští hello skripty nebo spustitelné soubory, které zadáte v takovém příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="65c94-277">A task has a command line and runs hello scripts or executables that you specify in that command line.</span></span>

<span data-ttu-id="65c94-278">tooactually práci, musí úkoly nejprve přidat úloha tooa.</span><span class="sxs-lookup"><span data-stu-id="65c94-278">tooactually perform work, tasks must be added tooa job.</span></span> <span data-ttu-id="65c94-279">Každý [CloudTask] [ net_task] se konfiguruje pomocí vlastnosti příkazového řádku a [ResourceFiles] [ net_task_resourcefiles] (stejně jako u StartTask fondu hello), Hello úkol stáhne toohello uzlu předtím, než se jeho příkazový řádek automaticky spustí.</span><span class="sxs-lookup"><span data-stu-id="65c94-279">Each [CloudTask][net_task] is configured by using a command-line property and [ResourceFiles][net_task_resourcefiles] (as with hello pool's StartTask) that hello task downloads toohello node before its command line is automatically executed.</span></span> <span data-ttu-id="65c94-280">V hello *DotNetTutorial* ukázkový projekt, každý úkol zpracovává jenom jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="65c94-280">In hello *DotNetTutorial* sample project, each task processes only one file.</span></span> <span data-ttu-id="65c94-281">Proto jeho kolekce ResourceFiles obsahuje jen jeden prvek.</span><span class="sxs-lookup"><span data-stu-id="65c94-281">Thus, its ResourceFiles collection contains a single element.</span></span>

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks toojob [{1}]...", inputFiles.Count, jobId);

    // Create a collection toohold hello tasks that we'll be adding toohello job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of hello tasks. Because we copied hello task application toothe
    // node's shared directory with hello pool's StartTask, we can access it via
    // hello shared directory on hello node that hello task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add hello tasks as a collection, as opposed tooissuing a separate AddTask call
    // for each. Bulk task submission helps tooensure efficient underlying API calls
    // toohello Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [!IMPORTANT]
> <span data-ttu-id="65c94-282">Když přistoupí k proměnným prostředí, jako například `%AZ_BATCH_NODE_SHARED_DIR%` nebo spuštění aplikace nebyla nalezena v uzlu hello `PATH`, musí příkazové řádky úkolu předponu `cmd /c`.</span><span class="sxs-lookup"><span data-stu-id="65c94-282">When they access environment variables such as `%AZ_BATCH_NODE_SHARED_DIR%` or execute an application not found in hello node's `PATH`, task command lines must be prefixed with `cmd /c`.</span></span> <span data-ttu-id="65c94-283">Tato akce explicitně provést hello překladač příkazů a dostane pokyn tooterminate po provedení příkazu.</span><span class="sxs-lookup"><span data-stu-id="65c94-283">This will explicitly execute hello command interpreter and instruct it tooterminate after carrying out your command.</span></span> <span data-ttu-id="65c94-284">Tento požadavek není nutný, pokud vaše úkoly spouští aplikace v uzlu hello `PATH` (například *robocopy.exe* nebo *powershell.exe*) a používají se žádné proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="65c94-284">This requirement is unnecessary if your tasks execute an application in hello node's `PATH` (such as *robocopy.exe* or *powershell.exe*) and no environment variables are used.</span></span>
>
>

<span data-ttu-id="65c94-285">V rámci hello `foreach` ve smyčce ve fragmentu kódu hello výše, můžete vidět, že hello příkazového řádku pro úlohu hello je vytvořený tak, aby tři argumenty příkazového řádku se předávají příliš*TaskApplication.exe*:</span><span class="sxs-lookup"><span data-stu-id="65c94-285">Within hello `foreach` loop in hello code snippet above, you can see that hello command line for hello task is constructed such that three command-line arguments are passed too*TaskApplication.exe*:</span></span>

1. <span data-ttu-id="65c94-286">Hello **první argument** hello cesta souboru tooprocess hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-286">hello **first argument** is hello path of hello file tooprocess.</span></span> <span data-ttu-id="65c94-287">Toto je hello místní cestu toohello soubor, protože existuje na uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-287">This is hello local path toohello file as it exists on hello node.</span></span> <span data-ttu-id="65c94-288">Když hello objekt ResourceFile v `UploadFileToContainerAsync` vytvořen vyšší, název souboru hello byl použit pro tuto vlastnost (jako parametr konstruktor ResourceFile toohello).</span><span class="sxs-lookup"><span data-stu-id="65c94-288">When hello ResourceFile object in `UploadFileToContainerAsync` was first created above, hello file name was used for this property (as a parameter toohello ResourceFile constructor).</span></span> <span data-ttu-id="65c94-289">To znamená, že hello soubor najdete v hello stejný adresář jako *TaskApplication.exe*.</span><span class="sxs-lookup"><span data-stu-id="65c94-289">This indicates that hello file can be found in hello same directory as *TaskApplication.exe*.</span></span>
2. <span data-ttu-id="65c94-290">Hello **druhý argument** Určuje, že hello horní *N* slova zasílány toohello výstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="65c94-290">hello **second argument** specifies that hello top *N* words should be written toohello output file.</span></span> <span data-ttu-id="65c94-291">V ukázce hello to je pevně zakódováno, aby hello nejčastějších tří slov jsou zapsány toohello výstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="65c94-291">In hello sample, this is hard-coded so that hello top three words are written toohello output file.</span></span>
3. <span data-ttu-id="65c94-292">Hello **třetí argument** je hello sdílený přístupový podpis (SAS), který poskytuje přístup pro zápis toohello **výstup** kontejneru ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="65c94-292">hello **third argument** is hello shared access signature (SAS) that provides write access toohello **output** container in Azure Storage.</span></span> <span data-ttu-id="65c94-293">*TaskApplication.exe* používá tento sdílený přístup k adrese URL podpisem při nahrávání hello výstupní soubor tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="65c94-293">*TaskApplication.exe* uses this shared access signature URL when it uploads hello output file tooAzure Storage.</span></span> <span data-ttu-id="65c94-294">Hello kódu pro tento lze najít v hello `UploadFileToContainer` metoda v projektu TaskApplication hello `Program.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="65c94-294">You can find hello code for this in hello `UploadFileToContainer` method in hello TaskApplication project's `Program.cs` file:</span></span>

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference toohello container using hello SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload hello file (as a new blob) toohello container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when hello Batch service
                // sets hello CloudTask.ExecutionInformation.ExitCode for hello task that
                // executed this application, it properly indicates that there was a
                // problem with hello task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a><span data-ttu-id="65c94-295">Krok 6: Sledování úkolů</span><span class="sxs-lookup"><span data-stu-id="65c94-295">Step 6: Monitor tasks</span></span>
<span data-ttu-id="65c94-296">![Sledujte úkoly.][6]</span><span class="sxs-lookup"><span data-stu-id="65c94-296">![Monitor tasks][6]</span></span><br/><span data-ttu-id="65c94-297">
*Hello klienta aplikace (1) monitorování hello stav dokončení a úspěšnosti úkolů a (2) hello úlohy odeslání výsledek data tooAzure úložiště*</span><span class="sxs-lookup"><span data-stu-id="65c94-297">
*hello client application (1) monitors hello tasks for completion and success status, and (2) hello tasks upload result data tooAzure Storage*</span></span>

<span data-ttu-id="65c94-298">Pokud úkoly přidáte tooa úlohy, jsou automaticky zařazeny do fronty a naplánovaných pro spuštění na výpočetních uzlech ve fondu hello spojené s úlohou hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-298">When tasks are added tooa job, they are automatically queued and scheduled for execution on compute nodes within hello pool associated with hello job.</span></span> <span data-ttu-id="65c94-299">Na základě hello nastavení, které zadáte, služba Batch zpracovává všechny služby Řízení front úloh, plánování, opakování a dalších úloh správy povinností za vás.</span><span class="sxs-lookup"><span data-stu-id="65c94-299">Based on hello settings you specify, Batch handles all task queuing, scheduling, retrying, and other task administration duties for you.</span></span>

<span data-ttu-id="65c94-300">Toomonitoring provádění úkolů existuje mnoho přístupů.</span><span class="sxs-lookup"><span data-stu-id="65c94-300">There are many approaches toomonitoring task execution.</span></span> <span data-ttu-id="65c94-301">DotNetTutorial ukazuje jednoduchý příklad, který hlásí jenom dokončení a stavy úspěchu/neúspěchu úkolu.</span><span class="sxs-lookup"><span data-stu-id="65c94-301">DotNetTutorial shows a simple example that reports only on completion and task failure or success states.</span></span> <span data-ttu-id="65c94-302">V rámci hello `MonitorTasks` metoda v DotNetTutorial `Program.cs`, existují tři koncepty Batch .NET, které místě prodiskutovat.</span><span class="sxs-lookup"><span data-stu-id="65c94-302">Within hello `MonitorTasks` method in DotNetTutorial's `Program.cs`, there are three Batch .NET concepts that warrant discussion.</span></span> <span data-ttu-id="65c94-303">Jsou uvedené níže v pořadí podle jejich výskytu:</span><span class="sxs-lookup"><span data-stu-id="65c94-303">They are listed below in their order of appearance:</span></span>

1. <span data-ttu-id="65c94-304">**ODATADetailLevel**: Zadání [ODATADetailLevel][net_odatadetaillevel] v operaci vypsání seznamu (například získání seznamu úkolů úlohy) je důležité pro zajištění výkonu aplikace Batch.</span><span class="sxs-lookup"><span data-stu-id="65c94-304">**ODATADetailLevel**: Specifying [ODATADetailLevel][net_odatadetaillevel] in list operations (such as obtaining a list of a job's tasks) is essential in ensuring Batch application performance.</span></span> <span data-ttu-id="65c94-305">Přidat [efektivní dotazování služby Azure Batch hello](batch-efficient-list-queries.md) tooyour čtení seznamu, pokud máte v úmyslu provádět jakékoli řazení sledování stavu v aplikacích Batch.</span><span class="sxs-lookup"><span data-stu-id="65c94-305">Add [Query hello Azure Batch service efficiently](batch-efficient-list-queries.md) tooyour reading list if you plan on doing any sort of status monitoring within your Batch applications.</span></span>
2. <span data-ttu-id="65c94-306">**TaskStateMonitor**: [TaskStateMonitor][net_taskstatemonitor] poskytuje aplikacím Batch .NET pomocné nástroje ke sledování stavů úkolů.</span><span class="sxs-lookup"><span data-stu-id="65c94-306">**TaskStateMonitor**: [TaskStateMonitor][net_taskstatemonitor] provides Batch .NET applications with helper utilities for monitoring task states.</span></span> <span data-ttu-id="65c94-307">V `MonitorTasks`, *DotNetTutorial* čeká na všechny úlohy tooreach [TaskState.Completed] [ net_taskstate] v časovém limitu.</span><span class="sxs-lookup"><span data-stu-id="65c94-307">In `MonitorTasks`, *DotNetTutorial* waits for all tasks tooreach [TaskState.Completed][net_taskstate] within a time limit.</span></span> <span data-ttu-id="65c94-308">Potom ukončí úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-308">Then it terminates hello job.</span></span>
3. <span data-ttu-id="65c94-309">**TerminateJobAsync**: ukončení úlohy pomocí [JobOperations.TerminateJobAsync] [ net_joboperations_terminatejob] (nebo hello blokování JobOperations.TerminateJob) označí tuto úlohu jako dokončenou.</span><span class="sxs-lookup"><span data-stu-id="65c94-309">**TerminateJobAsync**: Terminating a job with [JobOperations.TerminateJobAsync][net_joboperations_terminatejob] (or hello blocking JobOperations.TerminateJob) marks that job as completed.</span></span> <span data-ttu-id="65c94-310">Je nezbytné toodo, pokud vaše řešení Batch používá [JobReleaseTask][net_jobreltask].</span><span class="sxs-lookup"><span data-stu-id="65c94-310">It is essential toodo so if your Batch solution uses a [JobReleaseTask][net_jobreltask].</span></span> <span data-ttu-id="65c94-311">Jedná se o zvláštní typ úkolu, který je popsaný v článku [Úkoly přípravy a dokončení úlohy](batch-job-prep-release.md).</span><span class="sxs-lookup"><span data-stu-id="65c94-311">This is a special type of task, which is described in [Job preparation and completion tasks](batch-job-prep-release.md).</span></span>

<span data-ttu-id="65c94-312">Hello `MonitorTasks` metoda z *DotNetTutorial*na `Program.cs` se zobrazí níže:</span><span class="sxs-lookup"><span data-stu-id="65c94-312">hello `MonitorTasks` method from *DotNetTutorial*'s `Program.cs` appears below:</span></span>

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed tooreach hello Completed state within hello timeout period.";

    // Obtain hello collection of tasks currently managed by hello job. Note that we use
    // a detail level too specify that only hello "id" property of each task should be
    // populated. Using a detail level for all list operations helps toolower
    // response time from hello Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor toomonitor hello state of our tasks. In this case, we
    // will wait for all tasks tooreach hello Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached hello "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property tooensure that it did not encounter a failure
    // or return a non-zero exit code.

    // Update hello detail level toopopulate only hello task id and executionInfo
    // properties. We refresh hello tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate hello task's properties with hello latest info from hello Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.Result == TaskExecutionResult.Failure)
        {
            // A task with failure information set indicates there was a problem with hello task. It is important toonote that
            // hello task's state can be "Completed," yet still have encountered a failure.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a failure: {1}", task.Id, task.ExecutionInformation.FailureInformation.Message);
            if (task.ExecutionInformation.ExitCode != 0)
            {
                // A non-zero exit code may indicate that hello application executed by hello task encountered an error
                // during execution. As not every application returns non-zero on failure by default (e.g. robocopy),
                // your implementation of error checking may differ from this example.

                Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
            }
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within hello specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a><span data-ttu-id="65c94-313">Krok 7: Stažení výstupu úkolu</span><span class="sxs-lookup"><span data-stu-id="65c94-313">Step 7: Download task output</span></span>
<span data-ttu-id="65c94-314">![Stažení výstupu úkolu ze služby Storage][7]</span><span class="sxs-lookup"><span data-stu-id="65c94-314">![Download task output from Storage][7]</span></span><br/>

<span data-ttu-id="65c94-315">Teď, když hello dokončení úlohy hello výstup z úlohy hello si můžete stáhnout ze služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="65c94-315">Now that hello job is completed, hello output from hello tasks can be downloaded from Azure Storage.</span></span> <span data-ttu-id="65c94-316">To lze provést pomocí volání příliš`DownloadBlobsFromContainerAsync` v *DotNetTutorial*na `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="65c94-316">This is done with a call too`DownloadBlobsFromContainerAsync` in *DotNetTutorial*'s `Program.cs`:</span></span>

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference tooa previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all hello block blobs in hello specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference toohello current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents tooa file in hello specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded too{0}", directoryPath);
}
```

> [!NOTE]
> <span data-ttu-id="65c94-317">Hello volání příliš`DownloadBlobsFromContainerAsync` v hello *DotNetTutorial* aplikace určuje, zda text hello soubory by měly být stažené tooyour `%TEMP%` složky.</span><span class="sxs-lookup"><span data-stu-id="65c94-317">hello call too`DownloadBlobsFromContainerAsync` in hello *DotNetTutorial* application specifies that hello files should be downloaded tooyour `%TEMP%` folder.</span></span> <span data-ttu-id="65c94-318">Myslíte, že volné toomodify to výstupní umístění.</span><span class="sxs-lookup"><span data-stu-id="65c94-318">Feel free toomodify this output location.</span></span>
>
>

## <a name="step-8-delete-containers"></a><span data-ttu-id="65c94-319">Krok 8: Odstranění kontejnerů</span><span class="sxs-lookup"><span data-stu-id="65c94-319">Step 8: Delete containers</span></span>
<span data-ttu-id="65c94-320">Vzhledem k tomu, že musíte platit za data uložená ve službě Azure Storage, je vždy tooremove vhodné objekty BLOB, které již nejsou potřebné pro úlohy Batch.</span><span class="sxs-lookup"><span data-stu-id="65c94-320">Because you are charged for data that resides in Azure Storage, it's always a good idea tooremove blobs that are no longer needed for your Batch jobs.</span></span> <span data-ttu-id="65c94-321">V DotNetTutorial `Program.cs`, to provádí pomocí tří volání toohello Pomocná metoda `DeleteContainerAsync`:</span><span class="sxs-lookup"><span data-stu-id="65c94-321">In DotNetTutorial's `Program.cs`, this is done with three calls toohello helper method `DeleteContainerAsync`:</span></span>

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

<span data-ttu-id="65c94-322">Hello metoda sama jenom získá odkaz na kontejner toohello a potom zavolá [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:</span><span class="sxs-lookup"><span data-stu-id="65c94-322">hello method itself merely obtains a reference toohello container, and then calls [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:</span></span>

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-hello-job-and-hello-pool"></a><span data-ttu-id="65c94-323">Krok 9: Odstranění hello úlohy a fondu hello</span><span class="sxs-lookup"><span data-stu-id="65c94-323">Step 9: Delete hello job and hello pool</span></span>
<span data-ttu-id="65c94-324">V posledním kroku hello jste výzvami toodelete hello úlohy a hello fondu, které pocházejí z aplikace DotNetTutorial hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-324">In hello final step, you're prompted toodelete hello job and hello pool that were created by hello DotNetTutorial application.</span></span> <span data-ttu-id="65c94-325">I když se vám neúčtují poplatky za úlohy a úlohy samotné, *účtují* se vám poplatky za výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="65c94-325">Although you're not charged for jobs and tasks themselves, you *are* charged for compute nodes.</span></span> <span data-ttu-id="65c94-326">Proto doporučujeme, abyste uzly přidělovali, jen když je to potřeba.</span><span class="sxs-lookup"><span data-stu-id="65c94-326">Thus, we recommend that you allocate nodes only as needed.</span></span> <span data-ttu-id="65c94-327">Odstraňování nepoužívaných fondů by mělo být součástí vašeho standardního procesu údržby.</span><span class="sxs-lookup"><span data-stu-id="65c94-327">Deleting unused pools can be part of your maintenance process.</span></span>

<span data-ttu-id="65c94-328">Hello BatchClient na [JobOperations] [ net_joboperations] a [PoolOperations] [ net_pooloperations] mají odpovídající metody odstranění, které se volají, pokud Hello uživatel potvrdí odstranění:</span><span class="sxs-lookup"><span data-stu-id="65c94-328">hello BatchClient's [JobOperations][net_joboperations] and [PoolOperations][net_pooloperations] both have corresponding deletion methods, which are called if hello user confirms deletion:</span></span>

```csharp
// Clean up hello resources we've created in hello Batch account if hello user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [!IMPORTANT]
> <span data-ttu-id="65c94-329">Pamatujte, že se vám účtují poplatky za výpočetní prostředky, takže odstranění nepoužívaných fondů vám ušetří náklady.</span><span class="sxs-lookup"><span data-stu-id="65c94-329">Keep in mind that you are charged for compute resources—deleting unused pools will minimize cost.</span></span> <span data-ttu-id="65c94-330">Mějte také, že odstraněním fondu odstraníte všechny výpočetní uzly v tomto fondu, a že všechna data na uzlech hello neopravitelné po odstranění fondu hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-330">Also, be aware that deleting a pool deletes all compute nodes within that pool, and that any data on hello nodes will be unrecoverable after hello pool is deleted.</span></span>
>
>

## <a name="run-hello-dotnettutorial-sample"></a><span data-ttu-id="65c94-331">Spustit hello *DotNetTutorial* vzorku</span><span class="sxs-lookup"><span data-stu-id="65c94-331">Run hello *DotNetTutorial* sample</span></span>
<span data-ttu-id="65c94-332">Když spustíte hello ukázkovou aplikaci, bude výstup konzoly hello podobné toohello následující.</span><span class="sxs-lookup"><span data-stu-id="65c94-332">When you run hello sample application, hello console output will be similar toohello following.</span></span> <span data-ttu-id="65c94-333">Během provádění, dojde k pozastavení při `Awaiting task completion, timeout in 00:30:00...` při se spustí výpočetní uzly fondu hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-333">During execution, you will experience a pause at `Awaiting task completion, timeout in 00:30:00...` while hello pool's compute nodes are started.</span></span> <span data-ttu-id="65c94-334">Použití hello [portál Azure] [ azure_portal] toomonitor fondu, výpočetních uzlů, úlohy a úkolů během a po spuštění.</span><span class="sxs-lookup"><span data-stu-id="65c94-334">Use hello [Azure portal][azure_portal] toomonitor your pool, compute nodes, job, and tasks during and after execution.</span></span> <span data-ttu-id="65c94-335">Použití hello [portál Azure] [ azure_portal] nebo hello [Azure Storage Explorer] [ storage_explorers] tooview hello prostředků služby Storage (kontejnerů a objektů BLOB) které jsou Vytvoření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="65c94-335">Use hello [Azure portal][azure_portal] or hello [Azure Storage Explorer][storage_explorers] tooview hello Storage resources (containers and blobs) that are created by hello application.</span></span>

<span data-ttu-id="65c94-336">Typická doba provádění je **přibližně 5 minut** při spuštění aplikace hello ve výchozí konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="65c94-336">Typical execution time is **approximately 5 minutes** when you run hello application in its default configuration.</span></span>

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe toocontainer [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll toocontainer [application]...
Uploading file ..\..\taskdata1.txt toocontainer [input]...
Uploading file ..\..\taskdata2.txt toocontainer [input]...
Uploading file ..\..\taskdata3.txt toocontainer [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks toojob [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within hello specified timeout period.
Downloading all files from container [output]...
All files downloaded tooC:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER tooexit...
```

## <a name="next-steps"></a><span data-ttu-id="65c94-337">Další kroky</span><span class="sxs-lookup"><span data-stu-id="65c94-337">Next steps</span></span>
<span data-ttu-id="65c94-338">Působí volné toomake změny příliš*DotNetTutorial* a *TaskApplication* výpočetní tooexperiment jiné scénáře.</span><span class="sxs-lookup"><span data-stu-id="65c94-338">Feel free toomake changes too*DotNetTutorial* and *TaskApplication* tooexperiment with different compute scenarios.</span></span> <span data-ttu-id="65c94-339">Zkuste například přidat prodlevu provádění příliš*TaskApplication*, například stejně jako u [Thread.Sleep][net_thread_sleep], toosimulate dlouho běžící úlohy a monitorovat je hello portálu.</span><span class="sxs-lookup"><span data-stu-id="65c94-339">For example, try adding an execution delay too*TaskApplication*, such as with [Thread.Sleep][net_thread_sleep], toosimulate long-running tasks and monitor them in hello portal.</span></span> <span data-ttu-id="65c94-340">Zkuste přidat další úkoly nebo upravte hello počet výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="65c94-340">Try adding more tasks or adjusting hello number of compute nodes.</span></span> <span data-ttu-id="65c94-341">Přidejte logiku toocheck pro a povolit hello použití existující doba provádění toospeed fondu (*pomocný parametr*: Podívejte se na `ArticleHelpers.cs` v hello [Microsoft.Azure.Batch.Samples.Common] [ github_samples_common] projektu v [azure-batch-samples][github_samples]).</span><span class="sxs-lookup"><span data-stu-id="65c94-341">Add logic toocheck for and allow hello use of an existing pool toospeed execution time (*hint*: check out `ArticleHelpers.cs` in hello [Microsoft.Azure.Batch.Samples.Common][github_samples_common] project in [azure-batch-samples][github_samples]).</span></span>

<span data-ttu-id="65c94-342">Teď, když jste obeznámeni s hello základní pracovní postup řešení Batch, je čas toodig v toohello další funkce hello služby Batch.</span><span class="sxs-lookup"><span data-stu-id="65c94-342">Now that you're familiar with hello basic workflow of a Batch solution, it's time toodig in toohello additional features of hello Batch service.</span></span>

* <span data-ttu-id="65c94-343">Zkontrolujte hello [funkcí přehled Azure Batch](batch-api-basics.md) článek, který doporučujeme, pokud jste novou službu toohello.</span><span class="sxs-lookup"><span data-stu-id="65c94-343">Review hello [Overview of Azure Batch features](batch-api-basics.md) article, which we recommend if you're new toohello service.</span></span>
* <span data-ttu-id="65c94-344">Spuštění na hello další články vývoj Batch pod **vývoje** v hello [Batch studijní][batch_learning_path].</span><span class="sxs-lookup"><span data-stu-id="65c94-344">Start on hello other Batch development articles under **Development in-depth** in hello [Batch learning path][batch_learning_path].</span></span>
* <span data-ttu-id="65c94-345">Podívejte se na různé implementace zpracování úlohy hello "nejčastějších N slov" pomocí Batch v hello [TopNWords] [ github_topnwords] ukázka.</span><span class="sxs-lookup"><span data-stu-id="65c94-345">Check out a different implementation of processing hello "top N words" workload by using Batch in hello [TopNWords][github_topnwords] sample.</span></span>
* <span data-ttu-id="65c94-346">Zkontrolujte hello Batch .NET [poznámky k verzi](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/Batch/DataPlane/changelog.md#azurebatch-release-notes) hello nejnovější změny v hello knihovně.</span><span class="sxs-lookup"><span data-stu-id="65c94-346">Review hello Batch .NET [release notes](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/Batch/DataPlane/changelog.md#azurebatch-release-notes) for hello latest changes in hello library.</span></span>

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/vs/
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "Vytvoření kontejnerů ve službě Azure Storage"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Soubory toocontainers odeslání úloh aplikace a vstupních (data)"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Vytvoření fondu Batch"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Vytvoření úlohy Batch"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "Přidat toojob úlohy"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Sledování úkolů"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Stažení výstupu úkolu ze služby Storage"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "Pracovní postup řešení Batch (úplný diagram)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "Přihlašovací údaje Batch na portálu"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "Přihlašovací údaje služby Storage na portálu"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "Pracovní postup řešení Batch (minimální diagram)"
