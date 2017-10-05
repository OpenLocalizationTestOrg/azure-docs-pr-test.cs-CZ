---
title: "Kurz – použití klientské knihovny Azure Batch pro .NET | Dokumentace Microsoftu"
description: "Informace o základních konceptech služby Azure Batch a vytvoření jednoduchého řešení pomocí rozhraní .NET"
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
ms.openlocfilehash: cf8fdca51a6a4ad1b7cd4fe6980543199f6b36e0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-building-solutions-with-the-batch-client-library-for-net"></a><span data-ttu-id="5c153-103">Začínáme sestavovat řešení pomocí klientské knihovny služby Batch pro .NET</span><span class="sxs-lookup"><span data-stu-id="5c153-103">Get started building solutions with the Batch client library for .NET</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5c153-104">.NET</span><span class="sxs-lookup"><span data-stu-id="5c153-104">.NET</span></span>](batch-dotnet-get-started.md)
> * [<span data-ttu-id="5c153-105">Python</span><span class="sxs-lookup"><span data-stu-id="5c153-105">Python</span></span>](batch-python-tutorial.md)
> * [<span data-ttu-id="5c153-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="5c153-106">Node.js</span></span>](batch-nodejs-get-started.md)
>
>

<span data-ttu-id="5c153-107">V tomto článku se seznámíte se základy [Azure Batch][azure_batch] a s knihovnou [Batch .NET][net_api] a společně si krok za krokem probereme ukázkovou aplikaci v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="5c153-107">Learn the basics of [Azure Batch][azure_batch] and the [Batch .NET][net_api] library in this article as we discuss a C# sample application step by step.</span></span> <span data-ttu-id="5c153-108">Podíváme se, jak tato ukázková aplikace využívá službu Batch ke zpracování paralelní úlohy v cloudu, a jak komunikuje se službou [Azure Storage](../storage/common/storage-introduction.md) při přípravě a načítání souborů.</span><span class="sxs-lookup"><span data-stu-id="5c153-108">We look at how the sample application leverages the Batch service to process a parallel workload in the cloud, and how it interacts with [Azure Storage](../storage/common/storage-introduction.md) for file staging and retrieval.</span></span> <span data-ttu-id="5c153-109">Seznámíte se s běžným pracovním postupem aplikací Batch a získáte základní přehled o součástech služby Batch, například o úlohách, úkolech, fondech a výpočetních uzlech.</span><span class="sxs-lookup"><span data-stu-id="5c153-109">You'll learn a common Batch application workflow and gain a base understanding of the major components of Batch such as jobs, tasks, pools, and compute nodes.</span></span>

<span data-ttu-id="5c153-110">![Pracovní postup řešení Batch (Basic)][11]</span><span class="sxs-lookup"><span data-stu-id="5c153-110">![Batch solution workflow (basic)][11]</span></span><br/>

## <a name="prerequisites"></a><span data-ttu-id="5c153-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5c153-111">Prerequisites</span></span>
<span data-ttu-id="5c153-112">Tento článek předpokládá, že máte praktické znalosti jazyka C# a sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5c153-112">This article assumes that you have a working knowledge of C# and Visual Studio.</span></span> <span data-ttu-id="5c153-113">Předpokládá také, že dokážete splnit požadavky na vytvoření účtů Azure, služby Batch a služby Storage, které jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="5c153-113">It also assumes that you're able to satisfy the account creation requirements that are specified below for Azure and the Batch and Storage services.</span></span>

### <a name="accounts"></a><span data-ttu-id="5c153-114">Účty</span><span class="sxs-lookup"><span data-stu-id="5c153-114">Accounts</span></span>
* <span data-ttu-id="5c153-115">**Účet Azure**: Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet Azure][azure_free_account].</span><span class="sxs-lookup"><span data-stu-id="5c153-115">**Azure account**: If you don't already have an Azure subscription, [create a free Azure account][azure_free_account].</span></span>
* <span data-ttu-id="5c153-116">**Účet Batch**: Po pořízení předplatného Azure si [vytvořte účet Azure Batch](batch-account-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5c153-116">**Batch account**: Once you have an Azure subscription, [create an Azure Batch account](batch-account-create-portal.md).</span></span>
* <span data-ttu-id="5c153-117">**Účet Storage**: Viz část [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) v článku [Informace o účtech Azure Storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="5c153-117">**Storage account**: See [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5c153-118">Služba Batch aktuálně podporuje *jenom* typ účtu úložiště **pro obecné účely**, jak je popsáno v kroku č. 5 [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) v článku [Informace o účtech úložiště Azure](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="5c153-118">Batch currently supports *only* the **general-purpose** storage account type, as described in step #5 [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>
>
>

### <a name="visual-studio"></a><span data-ttu-id="5c153-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c153-119">Visual Studio</span></span>
<span data-ttu-id="5c153-120">K vytvoření ukázkového projektu potřebujete sadu **Visual Studio 2015 nebo novější**.</span><span class="sxs-lookup"><span data-stu-id="5c153-120">You must have **Visual Studio 2015 or newer** to build the sample project.</span></span> <span data-ttu-id="5c153-121">V [přehledu produktů Visual Studio][visual_studio] najdete bezplatné a zkušební verze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5c153-121">You can find free and trial versions of Visual Studio in the [overview of Visual Studio products][visual_studio].</span></span>

### <a name="dotnettutorial-code-sample"></a><span data-ttu-id="5c153-122">Ukázka kódu *DotNetTutorial*</span><span class="sxs-lookup"><span data-stu-id="5c153-122">*DotNetTutorial* code sample</span></span>
<span data-ttu-id="5c153-123">Ukázka [DotNetTutorial][github_dotnettutorial] je jednou z mnoha ukázek kódu Batch, které najdete v úložišti na GitHubu [azure-batch-samples][github_samples].</span><span class="sxs-lookup"><span data-stu-id="5c153-123">The [DotNetTutorial][github_dotnettutorial] sample is one of the many Batch code samples found in the [azure-batch-samples][github_samples] repository on GitHub.</span></span> <span data-ttu-id="5c153-124">Všechny ukázky můžete stáhnout kliknutím na **Klonovat nebo stáhnout > Stáhnout ZIP** na domovské stránce úložiště, nebo kliknutím na přímý odkaz ke stažení [azure-batch-samples-master.zip][github_samples_zip].</span><span class="sxs-lookup"><span data-stu-id="5c153-124">You can download all the samples by clicking  **Clone or download > Download ZIP** on the repository home page, or by clicking the [azure-batch-samples-master.zip][github_samples_zip] direct download link.</span></span> <span data-ttu-id="5c153-125">Po extrahování obsahu souboru ZIP najdete řešení v následující složce:</span><span class="sxs-lookup"><span data-stu-id="5c153-125">Once you've extracted the contents of the ZIP file, you can find the solution in the following folder:</span></span>

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a><span data-ttu-id="5c153-126">Průzkumník Azure Batch (volitelné)</span><span class="sxs-lookup"><span data-stu-id="5c153-126">Azure Batch Explorer (optional)</span></span>
<span data-ttu-id="5c153-127">[Azure Batch Explorer][github_batchexplorer] je bezplatný nástroj, který najdete v úložišti na GitHubu [azure-batch-samples][github_samples].</span><span class="sxs-lookup"><span data-stu-id="5c153-127">The [Azure Batch Explorer][github_batchexplorer] is a free utility that is included in the [azure-batch-samples][github_samples] repository on GitHub.</span></span> <span data-ttu-id="5c153-128">Není sice k dokončení kurzu nutný, ale může být užitečný při vývoji a ladění vašich řešení Batch.</span><span class="sxs-lookup"><span data-stu-id="5c153-128">While not required to complete this tutorial, it can be useful while developing and debugging your Batch solutions.</span></span>

## <a name="dotnettutorial-sample-project-overview"></a><span data-ttu-id="5c153-129">Přehled ukázkového projektu DotNetTutorial</span><span class="sxs-lookup"><span data-stu-id="5c153-129">DotNetTutorial sample project overview</span></span>
<span data-ttu-id="5c153-130">Ukázka kódu *DotNetTutorial* je řešení sady Visual Studio, které se skládá ze dvou projektů: **DotNetTutorial** a **TaskApplication**.</span><span class="sxs-lookup"><span data-stu-id="5c153-130">The *DotNetTutorial* code sample is a Visual Studio solution that consists of two projects: **DotNetTutorial** and **TaskApplication**.</span></span>

* <span data-ttu-id="5c153-131">**DotNetTutorial** je klientská aplikace, která komunikuje se službou Batch a se službou Azure Storage při spouštění paralelní úlohy na výpočetních uzlech (virtuálních počítačích).</span><span class="sxs-lookup"><span data-stu-id="5c153-131">**DotNetTutorial** is the client application that interacts with the Batch and Storage services to execute a parallel workload on compute nodes (virtual machines).</span></span> <span data-ttu-id="5c153-132">DotNetTutorial se spouští na místní pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="5c153-132">DotNetTutorial runs on your local workstation.</span></span>
* <span data-ttu-id="5c153-133">**TaskApplication** je program, který běží na výpočetních uzlech v Azure a provádí samotnou práci.</span><span class="sxs-lookup"><span data-stu-id="5c153-133">**TaskApplication** is the program that runs on compute nodes in Azure to perform the actual work.</span></span> <span data-ttu-id="5c153-134">V tomto příkladu `TaskApplication.exe` analyzuje text v souboru staženém ze služby Azure Storage (vstupní soubor).</span><span class="sxs-lookup"><span data-stu-id="5c153-134">In the sample, `TaskApplication.exe` parses the text in a file downloaded from Azure Storage (the input file).</span></span> <span data-ttu-id="5c153-135">Potom vytvoří textový soubor (výstupní soubor), který obsahuje seznam nejčastějších tří slov, která se zobrazují ve vstupním souboru.</span><span class="sxs-lookup"><span data-stu-id="5c153-135">Then it produces a text file (the output file) that contains a list of the top three words that appear in the input file.</span></span> <span data-ttu-id="5c153-136">Po vytvoření výstupního souboru TaskApplication odešle soubor do služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5c153-136">After it creates the output file, TaskApplication uploads the file to Azure Storage.</span></span> <span data-ttu-id="5c153-137">Klientská aplikace ho tak bude mít k dispozici ke stažení.</span><span class="sxs-lookup"><span data-stu-id="5c153-137">This makes it available to the client application for download.</span></span> <span data-ttu-id="5c153-138">TaskApplication běží paralelně v několika výpočetních uzlech v rámci služby Batch.</span><span class="sxs-lookup"><span data-stu-id="5c153-138">TaskApplication runs in parallel on multiple compute nodes in the Batch service.</span></span>

<span data-ttu-id="5c153-139">Následující diagram znázorňuje primární operace, které provádí klientská aplikace *DotNetTutorial* a aplikace *TaskApplication*, kterou spouští úkoly.</span><span class="sxs-lookup"><span data-stu-id="5c153-139">The following diagram illustrates the primary operations that are performed by the client application, *DotNetTutorial*, and the application that is executed by the tasks, *TaskApplication*.</span></span> <span data-ttu-id="5c153-140">Tento základní pracovní postup je typický pro mnoho výpočetních řešení, která jsou vytvořená pomocí služby Batch.</span><span class="sxs-lookup"><span data-stu-id="5c153-140">This basic workflow is typical of many compute solutions that are created with Batch.</span></span> <span data-ttu-id="5c153-141">I když nepředvádí všechny funkce, které jsou ve službě Batch dostupné, téměř každý scénář Batch bude obsahovat části tohoto pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="5c153-141">While it does not demonstrate every feature available in the Batch service, nearly every Batch scenario includes portions of this workflow.</span></span>

<span data-ttu-id="5c153-142">![Ukázkový pracovní postup služby Batch][8]</span><span class="sxs-lookup"><span data-stu-id="5c153-142">![Batch example workflow][8]</span></span><br/>

[<span data-ttu-id="5c153-143">**Krok 1.**</span><span class="sxs-lookup"><span data-stu-id="5c153-143">**Step 1.**</span></span>](#step-1-create-storage-containers) <span data-ttu-id="5c153-144">Ve službě Azure Blob Storage vytvořte **kontejnery** .</span><span class="sxs-lookup"><span data-stu-id="5c153-144">Create **containers** in Azure Blob Storage.</span></span><br/><span data-ttu-id="5c153-145">
[**Krok 2.**](#step-2-upload-task-application-and-data-files)</span><span class="sxs-lookup"><span data-stu-id="5c153-145">
[**Step 2.**](#step-2-upload-task-application-and-data-files)</span></span> <span data-ttu-id="5c153-146">Odešlete do kontejneru aplikační soubory a vstupní soubory úkolu.</span><span class="sxs-lookup"><span data-stu-id="5c153-146">Upload task application files and input files to containers.</span></span><br/><span data-ttu-id="5c153-147">
[**Krok 3.**](#step-3-create-batch-pool)</span><span class="sxs-lookup"><span data-stu-id="5c153-147">
[**Step 3.**](#step-3-create-batch-pool)</span></span> <span data-ttu-id="5c153-148">Vytvořte **fond** Batch.</span><span class="sxs-lookup"><span data-stu-id="5c153-148">Create a Batch **pool**.</span></span><br/>
  <span data-ttu-id="5c153-149">&nbsp;&nbsp;&nbsp;&nbsp;**3a.**</span><span class="sxs-lookup"><span data-stu-id="5c153-149">&nbsp;&nbsp;&nbsp;&nbsp;**3a.**</span></span> <span data-ttu-id="5c153-150">Když se uzly připojí k fondu, fond **StartTask** stáhne binární soubory úkolů (TaskApplication).</span><span class="sxs-lookup"><span data-stu-id="5c153-150">The pool **StartTask** downloads the task binary files (TaskApplication) to nodes as they join the pool.</span></span><br/><span data-ttu-id="5c153-151">
[**Krok 4.**](#step-4-create-batch-job)</span><span class="sxs-lookup"><span data-stu-id="5c153-151">
[**Step 4.**](#step-4-create-batch-job)</span></span> <span data-ttu-id="5c153-152">Vytvořte **úlohu** Batch.</span><span class="sxs-lookup"><span data-stu-id="5c153-152">Create a Batch **job**.</span></span><br/><span data-ttu-id="5c153-153">
[**Krok 5.**](#step-5-add-tasks-to-job)</span><span class="sxs-lookup"><span data-stu-id="5c153-153">
[**Step 5.**](#step-5-add-tasks-to-job)</span></span> <span data-ttu-id="5c153-154">Přidejte do úlohy **úkoly**.</span><span class="sxs-lookup"><span data-stu-id="5c153-154">Add **tasks** to the job.</span></span><br/>
  <span data-ttu-id="5c153-155">&nbsp;&nbsp;&nbsp;&nbsp;**5a.**</span><span class="sxs-lookup"><span data-stu-id="5c153-155">&nbsp;&nbsp;&nbsp;&nbsp;**5a.**</span></span> <span data-ttu-id="5c153-156">Úkoly jsou naplánované, aby se spustily na uzlech.</span><span class="sxs-lookup"><span data-stu-id="5c153-156">The tasks are scheduled to execute on nodes.</span></span><br/>
    <span data-ttu-id="5c153-157">&nbsp;&nbsp;&nbsp;&nbsp;**5b.**</span><span class="sxs-lookup"><span data-stu-id="5c153-157">&nbsp;&nbsp;&nbsp;&nbsp;**5b.**</span></span> <span data-ttu-id="5c153-158">Každý úkol stáhne svoje vstupní data ze služby Azure Storage a potom zahájí spuštění.</span><span class="sxs-lookup"><span data-stu-id="5c153-158">Each task downloads its input data from Azure Storage, then begins execution.</span></span><br/><span data-ttu-id="5c153-159">
[**Krok 6.**](#step-6-monitor-tasks)</span><span class="sxs-lookup"><span data-stu-id="5c153-159">
[**Step 6.**](#step-6-monitor-tasks)</span></span> <span data-ttu-id="5c153-160">Sledujte úkoly.</span><span class="sxs-lookup"><span data-stu-id="5c153-160">Monitor tasks.</span></span><br/>
  <span data-ttu-id="5c153-161">&nbsp;&nbsp;&nbsp;&nbsp;**6a.**</span><span class="sxs-lookup"><span data-stu-id="5c153-161">&nbsp;&nbsp;&nbsp;&nbsp;**6a.**</span></span> <span data-ttu-id="5c153-162">Úkoly při dokončení odesílají svoje výstupní data do služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5c153-162">As tasks are completed, they upload their output data to Azure Storage.</span></span><br/><span data-ttu-id="5c153-163">
[**Krok 7.**](#step-7-download-task-output)</span><span class="sxs-lookup"><span data-stu-id="5c153-163">
[**Step 7.**](#step-7-download-task-output)</span></span> <span data-ttu-id="5c153-164">Stáhněte si výstup úkolu ze služby Storage.</span><span class="sxs-lookup"><span data-stu-id="5c153-164">Download task output from Storage.</span></span>

<span data-ttu-id="5c153-165">Jak jsme už zmínili, ne každé řešení Batch provede právě tyto kroky a může jich obsahovat i mnohem víc, ale ukázková aplikace *DotNetTutorial* předvádí běžné procesy, které probíhají v řešení Batch.</span><span class="sxs-lookup"><span data-stu-id="5c153-165">As mentioned, not every Batch solution performs these exact steps, and may include many more, but the *DotNetTutorial* sample application demonstrates common processes found in a Batch solution.</span></span>

## <a name="build-the-dotnettutorial-sample-project"></a><span data-ttu-id="5c153-166">Vytvoření ukázkového projektu *DotNetTutorial*</span><span class="sxs-lookup"><span data-stu-id="5c153-166">Build the *DotNetTutorial* sample project</span></span>
<span data-ttu-id="5c153-167">Předtím, než ukázku úspěšně spustíte, musíte zadat přihlašovací údaje k účtu Batch i k účtu Storage do souboru `Program.cs` v projektu *DotNetTutorial*.</span><span class="sxs-lookup"><span data-stu-id="5c153-167">Before you can successfully run the sample, you must specify both Batch and Storage account credentials in the *DotNetTutorial* project's `Program.cs` file.</span></span> <span data-ttu-id="5c153-168">Pokud jste to ještě neudělali, otevřete řešení v sadě Visual Studio dvojím kliknutím na soubor řešení `DotNetTutorial.sln`.</span><span class="sxs-lookup"><span data-stu-id="5c153-168">If you have not done so already, open the solution in Visual Studio by double-clicking the `DotNetTutorial.sln` solution file.</span></span> <span data-ttu-id="5c153-169">Nebo ho otevřete v sadě Visual Studio pomocí nabídky **Soubor > Otevřít > Projekt nebo řešení**.</span><span class="sxs-lookup"><span data-stu-id="5c153-169">Or open it from within Visual Studio by using the **File > Open > Project/Solution** menu.</span></span>

<span data-ttu-id="5c153-170">V projektu *DotNetTutorial* otevřete soubor `Program.cs`.</span><span class="sxs-lookup"><span data-stu-id="5c153-170">Open `Program.cs` within the *DotNetTutorial* project.</span></span> <span data-ttu-id="5c153-171">Potom podle pokynů na začátku souboru zadejte svoje přihlašovací údaje:</span><span class="sxs-lookup"><span data-stu-id="5c153-171">Then add your credentials as specified near the top of the file:</span></span>

```csharp
// Update the Batch and Storage account credential strings below with the values
// unique to your accounts. These are used when constructing connection strings
// for the Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [!IMPORTANT]
> <span data-ttu-id="5c153-172">Jak je uvedeno výše, aktuálně musíte ve službě Azure Storage zadat přihlašovací údaje účtu úložiště, který je pro **obecné účely**.</span><span class="sxs-lookup"><span data-stu-id="5c153-172">As mentioned above, you must currently specify the credentials for a **general-purpose** storage account in Azure Storage.</span></span> <span data-ttu-id="5c153-173">Vaše aplikace Batch používají úložiště objektů blob v rámci účtu úložiště pro **obecné účely**.</span><span class="sxs-lookup"><span data-stu-id="5c153-173">Your Batch applications use blob storage within the **general-purpose** storage account.</span></span> <span data-ttu-id="5c153-174">Nezadávejte přihlašovací údaje k účtu služby Storage, který jste vytvořili výběrem účtu typu *Blob Storage*.</span><span class="sxs-lookup"><span data-stu-id="5c153-174">Do not specify the credentials for a Storage account that was created by selecting the *Blob storage* account type.</span></span>
>
>

<span data-ttu-id="5c153-175">Přihlašovací údaje k účtu Batch a k účtu služby Storage najdete v okně účtu každé služby na webu [Azure Portal][azure_portal]:</span><span class="sxs-lookup"><span data-stu-id="5c153-175">You can find your Batch and Storage account credentials within the account blade of each service in the [Azure portal][azure_portal]:</span></span>

<span data-ttu-id="5c153-176">![Přihlašovací údaje služby Batch na portálu][9]
![Přihlašovací údaje služby Storage na portálu][10]</span><span class="sxs-lookup"><span data-stu-id="5c153-176">![Batch credentials in the portal][9]
![Storage credentials in the portal][10]</span></span><br/>

<span data-ttu-id="5c153-177">Po aktualizaci projektu pomocí svých přihlašovacích údajů klikněte pravým tlačítkem v Průzkumníku řešení a potom klikněte na **Sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="5c153-177">Now that you've updated the project with your credentials, right-click the solution in Solution Explorer and click **Build Solution**.</span></span> <span data-ttu-id="5c153-178">Pokud se zobrazí výzva, potvrďte obnovení všech balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="5c153-178">Confirm the restoration of any NuGet packages, if you're prompted.</span></span>

> [!TIP]
> <span data-ttu-id="5c153-179">Pokud se balíčky NuGet automaticky neobnoví, nebo když se zobrazí chyby týkající se neúspěšného obnovení balíčků, zkontrolujte, jestli máte nainstalovaného [Správce balíčků NuGet][nuget_packagemgr].</span><span class="sxs-lookup"><span data-stu-id="5c153-179">If the NuGet packages are not automatically restored, or if you see errors about a failure to restore the packages, ensure that you have the [NuGet Package Manager][nuget_packagemgr] installed.</span></span> <span data-ttu-id="5c153-180">Potom povolte stažení chybějících balíčků.</span><span class="sxs-lookup"><span data-stu-id="5c153-180">Then enable the download of missing packages.</span></span> <span data-ttu-id="5c153-181">V článku [Povolení obnovy balíčků během sestavení][nuget_restore] najdete další informace o povolení stahování balíčků.</span><span class="sxs-lookup"><span data-stu-id="5c153-181">See [Enabling Package Restore During Build][nuget_restore] to enable package download.</span></span>
>
>

<span data-ttu-id="5c153-182">V následujících částech si ukázkovou aplikaci rozdělíme do kroků, které aplikace provádí při zpracování úloh ve službě Batch, a jednotlivé kroky si podrobně probereme.</span><span class="sxs-lookup"><span data-stu-id="5c153-182">In the following sections, we break down the sample application into the steps that it performs to process a workload in the Batch service, and discuss those steps in detail.</span></span> <span data-ttu-id="5c153-183">Doporučujeme, abyste při procházení zbývající části tohoto článku nahlíželi do řešení otevřeného v sadě Visual Studio, protože tady nezvládneme probrat každý jednotlivý řádek kódu.</span><span class="sxs-lookup"><span data-stu-id="5c153-183">We encourage you to refer to the open solution in Visual Studio while you work your way through the rest of this article, since not every line of code in the sample is discussed.</span></span>

<span data-ttu-id="5c153-184">V projektu *DotNetTutorial* v souboru `Program.cs` přejděte do horní části metody `MainAsync` a začněte s krokem 1.</span><span class="sxs-lookup"><span data-stu-id="5c153-184">Navigate to the top of the `MainAsync` method in the *DotNetTutorial* project's `Program.cs` file to start with Step 1.</span></span> <span data-ttu-id="5c153-185">Každý níže uvedený krok zhruba následuje průběh volání metod v `MainAsync`.</span><span class="sxs-lookup"><span data-stu-id="5c153-185">Each step below then roughly follows the progression of method calls in `MainAsync`.</span></span>

## <a name="step-1-create-storage-containers"></a><span data-ttu-id="5c153-186">Krok 1: Vytvoření kontejnerů služby Storage</span><span class="sxs-lookup"><span data-stu-id="5c153-186">Step 1: Create Storage containers</span></span>
<span data-ttu-id="5c153-187">![Vytvoření kontejnerů ve službě Azure Storage][1]
</span><span class="sxs-lookup"><span data-stu-id="5c153-187">![Create containers in Azure Storage][1]
</span></span><br/>

<span data-ttu-id="5c153-188">Batch obsahuje vestavěnou podporu pro komunikaci se službou Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5c153-188">Batch includes built-in support for interacting with Azure Storage.</span></span> <span data-ttu-id="5c153-189">Kontejnery v účtu Storage poskytnou soubory, které potřebují úkoly spuštěné v účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="5c153-189">Containers in your Storage account will provide the files needed by the tasks that run in your Batch account.</span></span> <span data-ttu-id="5c153-190">Kontejnery také poskytují místo pro ukládání výstupních dat, která úkoly vytvářejí.</span><span class="sxs-lookup"><span data-stu-id="5c153-190">The containers also provide a place to store the output data that the tasks produce.</span></span> <span data-ttu-id="5c153-191">Klientská aplikace *DotNetTutorial* nejdřív vytvoří tři kontejnery ve službě [Azure Blob Storage](../storage/common/storage-introduction.md):</span><span class="sxs-lookup"><span data-stu-id="5c153-191">The first thing the *DotNetTutorial* client application does is create three containers in [Azure Blob Storage](../storage/common/storage-introduction.md):</span></span>

* <span data-ttu-id="5c153-192">**application**: Do tohoto kontejneru se bude ukládat aplikace spuštěná úkoly a také veškeré její závislé položky, například knihovny DLL.</span><span class="sxs-lookup"><span data-stu-id="5c153-192">**application**: This container will store the application run by the tasks, as well as any of its dependencies, such as DLLs.</span></span>
* <span data-ttu-id="5c153-193">**input**: Datové soubory ke zpracování budou úkoly stahovat z kontejneru *input*.</span><span class="sxs-lookup"><span data-stu-id="5c153-193">**input**: Tasks will download the data files to process from the *input* container.</span></span>
* <span data-ttu-id="5c153-194">**output**: Když úkoly dokončí zpracování vstupního souboru, odešlou výsledky do kontejneru *output*.</span><span class="sxs-lookup"><span data-stu-id="5c153-194">**output**: When tasks complete input file processing, they will upload the results to the *output* container.</span></span>

<span data-ttu-id="5c153-195">Za účelem práce s účtem služby Storage a vytvoření kontejnerů použijeme [klientskou knihovnu služby Azure Storage pro .NET][net_api_storage].</span><span class="sxs-lookup"><span data-stu-id="5c153-195">In order to interact with a Storage account and create containers, we use the [Azure Storage Client Library for .NET][net_api_storage].</span></span> <span data-ttu-id="5c153-196">Referenci na účet vytvoříme pomocí [CloudStorageAccount][net_cloudstorageaccount] a z té vytvoříme [CloudBlobClient][net_cloudblobclient]:</span><span class="sxs-lookup"><span data-stu-id="5c153-196">We create a reference to the account with [CloudStorageAccount][net_cloudstorageaccount], and from that create a [CloudBlobClient][net_cloudblobclient]:</span></span>

```csharp
// Construct the Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve the storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create the blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

<span data-ttu-id="5c153-197">Referenci `blobClient` používáme v celé aplikaci a předáváme jako parametr do několika metod.</span><span class="sxs-lookup"><span data-stu-id="5c153-197">We use the `blobClient` reference throughout the application and pass it as a parameter to several methods.</span></span> <span data-ttu-id="5c153-198">Příklad toho je v bloku kódu, který následuje výše uvedené, kde voláme `CreateContainerIfNotExistAsync`, abychom vytvořili kontejnery.</span><span class="sxs-lookup"><span data-stu-id="5c153-198">An example of this is in the code block that immediately follows the above, where we call `CreateContainerIfNotExistAsync` to actually create the containers.</span></span>

```csharp
// Use the blob client to create the containers in Azure Storage if they don't
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

<span data-ttu-id="5c153-199">Po vytvoření kontejnerů může aplikace začít odesílat soubory, které budou úkoly používat.</span><span class="sxs-lookup"><span data-stu-id="5c153-199">Once the containers have been created, the application can now upload the files that will be used by the tasks.</span></span>

> [!TIP]
> <span data-ttu-id="5c153-200">Článek [Použití služby Blob Storage pomocí technologie .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) nabízí pěkný přehled o práci s kontejnery a objekty blob ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5c153-200">[How to use Blob Storage from .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) provides a good overview of working with Azure Storage containers and blobs.</span></span> <span data-ttu-id="5c153-201">Když začnete pracovat se službou Batch, je určitě na místě si ten článek přečíst.</span><span class="sxs-lookup"><span data-stu-id="5c153-201">It should be near the top of your reading list as you start working with Batch.</span></span>
>
>

## <a name="step-2-upload-task-application-and-data-files"></a><span data-ttu-id="5c153-202">Krok 2: Nahrání aplikačních a datových souborů úkolů</span><span class="sxs-lookup"><span data-stu-id="5c153-202">Step 2: Upload task application and data files</span></span>
<span data-ttu-id="5c153-203">![Odeslání aplikačních a vstupních (datových) souborů úkolů do kontejnerů][2]
</span><span class="sxs-lookup"><span data-stu-id="5c153-203">![Upload task application and input (data) files to containers][2]
</span></span><br/>

<span data-ttu-id="5c153-204">*DotNetTutorial* v rámci operace nahrávání souborů nejdřív definuje kolekce cest k **aplikačním** a **vstupním** souborům, které jsou v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="5c153-204">In the file upload operation, *DotNetTutorial* first defines collections of **application** and **input** file paths as they exist on the local machine.</span></span> <span data-ttu-id="5c153-205">Potom tyto soubory odešle do kontejnerů, které jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="5c153-205">Then it uploads these files to the containers that you created in the previous step.</span></span>

```csharp
// Paths to the executable and its dependencies that will be executed by the tasks
List<string> applicationFilePaths = new List<string>
{
    // The DotNetTutorial project includes a project reference to TaskApplication,
    // allowing us to determine the path of the task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// The collection of data files that are to be processed by the tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload the application and its dependencies to Azure Storage. This is the
// application that will process the data files, and will be executed by each
// of the tasks on the compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload the data files. This is the data that will be processed by each of
// the tasks that are executed on the compute nodes within the pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

<span data-ttu-id="5c153-206">V souboru `Program.cs` existují dvě metody, které se účastní procesu nahrávání:</span><span class="sxs-lookup"><span data-stu-id="5c153-206">There are two methods in `Program.cs` that are involved in the upload process:</span></span>

* <span data-ttu-id="5c153-207">`UploadFilesToContainerAsync`: Tato metoda vrátí kolekci objektů [ResourceFile][net_resourcefile] (viz následující popis) a interně volá `UploadFileToContainerAsync` kvůli nahrání každého souboru, který se předává v parametru *filePaths*.</span><span class="sxs-lookup"><span data-stu-id="5c153-207">`UploadFilesToContainerAsync`: This method returns a collection of [ResourceFile][net_resourcefile] objects (discussed below) and internally calls `UploadFileToContainerAsync` to upload each file that is passed in the *filePaths* parameter.</span></span>
* <span data-ttu-id="5c153-208">`UploadFileToContainerAsync`: Toto je metoda, která provádí samotné nahrávání souborů a vytváří objekty [ResourceFile][net_resourcefile].</span><span class="sxs-lookup"><span data-stu-id="5c153-208">`UploadFileToContainerAsync`: This is the method that actually performs the file upload and creates the [ResourceFile][net_resourcefile] objects.</span></span> <span data-ttu-id="5c153-209">Po nahrání souboru získá sdílený přístupový podpis (SAS) souboru a vrátí objekt ResourceFile, který ho zastupuje.</span><span class="sxs-lookup"><span data-stu-id="5c153-209">After uploading the file, it obtains a shared access signature (SAS) for the file and returns a ResourceFile object that represents it.</span></span> <span data-ttu-id="5c153-210">Sdílené přístupové podpisy jsou také popsány níže.</span><span class="sxs-lookup"><span data-stu-id="5c153-210">Shared access signatures are also discussed below.</span></span>

```csharp
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} to container [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath);

        // Set the expiry time and permissions for the blob shared access signature.
        // In this case, no start time is specified, so the shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct the SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a><span data-ttu-id="5c153-211">ResourceFiles</span><span class="sxs-lookup"><span data-stu-id="5c153-211">ResourceFiles</span></span>
<span data-ttu-id="5c153-212">[ResourceFile][net_resourcefile] poskytuje úkolům v Batch adresu URL k souboru ve službě Azure Storage, který se před spuštěním úkolu stáhne do výpočetního uzlu.</span><span class="sxs-lookup"><span data-stu-id="5c153-212">A [ResourceFile][net_resourcefile] provides tasks in Batch with the URL to a file in Azure Storage that is downloaded to a compute node before that task is run.</span></span> <span data-ttu-id="5c153-213">Vlastnost [ResourceFile.BlobSource][net_resourcefile_blobsource] určuje úplnou adresu URL souboru, protože existuje ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5c153-213">The [ResourceFile.BlobSource][net_resourcefile_blobsource] property specifies the full URL of the file as it exists in Azure Storage.</span></span> <span data-ttu-id="5c153-214">Adresa URL může obsahovat také sdílený přístupový podpis (SAS), který zajišťuje zabezpečený přístup k souboru.</span><span class="sxs-lookup"><span data-stu-id="5c153-214">The URL may also include a shared access signature (SAS) that provides secure access to the file.</span></span> <span data-ttu-id="5c153-215">Většina typů úkolů v rámci v Batch .NET obsahuje vlastnost *ResourceFiles* včetně:</span><span class="sxs-lookup"><span data-stu-id="5c153-215">Most tasks types within Batch .NET include a *ResourceFiles* property, including:</span></span>

* <span data-ttu-id="5c153-216">[CloudTask][net_task]</span><span class="sxs-lookup"><span data-stu-id="5c153-216">[CloudTask][net_task]</span></span>
* <span data-ttu-id="5c153-217">[StartTask][net_pool_starttask]</span><span class="sxs-lookup"><span data-stu-id="5c153-217">[StartTask][net_pool_starttask]</span></span>
* <span data-ttu-id="5c153-218">[JobPreparationTask][net_jobpreptask]</span><span class="sxs-lookup"><span data-stu-id="5c153-218">[JobPreparationTask][net_jobpreptask]</span></span>
* <span data-ttu-id="5c153-219">[JobReleaseTask][net_jobreltask]</span><span class="sxs-lookup"><span data-stu-id="5c153-219">[JobReleaseTask][net_jobreltask]</span></span>

<span data-ttu-id="5c153-220">Ukázková aplikace DotNetTutorial nepoužívá typy úloh JobPreparationTask nebo JobReleaseTask, ale můžete si o nich přečíst v článku [Spouštění úkolů přípravy a dokončení úlohy na výpočetních uzlech Azure Batch](batch-job-prep-release.md).</span><span class="sxs-lookup"><span data-stu-id="5c153-220">The DotNetTutorial sample application does not use the JobPreparationTask or JobReleaseTask task types, but you can read more about them in [Run job preparation and completion tasks on Azure Batch compute nodes](batch-job-prep-release.md).</span></span>

### <a name="shared-access-signature-sas"></a><span data-ttu-id="5c153-221">Sdílený přístupový podpis (SAS)</span><span class="sxs-lookup"><span data-stu-id="5c153-221">Shared access signature (SAS)</span></span>
<span data-ttu-id="5c153-222">Sdílené přístupové podpisy jsou řetězce, které (když jsou součástí adresy URL) zajišťují zabezpečený přístup ke kontejnerům a objektům blob ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5c153-222">Shared access signatures are strings which—when included as part of a URL—provide secure access to containers and blobs in Azure Storage.</span></span> <span data-ttu-id="5c153-223">Aplikace DotNetTutorial používá adresy URL se sdíleným přístupovým podpisem objektu blob i kontejneru a ukazuje, jak můžete tyto řetězce sdíleného přístupového podpisu získat ze služby Storage.</span><span class="sxs-lookup"><span data-stu-id="5c153-223">The DotNetTutorial application uses both blob and container shared access signature URLs, and demonstrates how to obtain these shared access signature strings from the Storage service.</span></span>

* <span data-ttu-id="5c153-224">**Sdílené přístupové podpisy objektů blob**: StartTask fondu v aplikaci DotNetTutorial používá sdílené přístupové podpisy objektů blob při stahování aplikačních binárních souborů a vstupních datových souborů ze služby Storage (viz krok 3 níže).</span><span class="sxs-lookup"><span data-stu-id="5c153-224">**Blob shared access signatures**: The pool's StartTask in DotNetTutorial uses blob shared access signatures when it downloads the application binaries and input data files from Storage (see Step #3 below).</span></span> <span data-ttu-id="5c153-225">Metoda `UploadFileToContainerAsync` v souboru `Program.cs` aplikace DotNetTutorial obsahuje kód, který získá sdílený přístupový podpis jednotlivých objektů blob.</span><span class="sxs-lookup"><span data-stu-id="5c153-225">The `UploadFileToContainerAsync` method in DotNetTutorial's `Program.cs` contains the code that obtains each blob's shared access signature.</span></span> <span data-ttu-id="5c153-226">Dělá to tak, že volá [CloudBlob.GetSharedAccessSignature][net_sas_blob].</span><span class="sxs-lookup"><span data-stu-id="5c153-226">It does so by calling [CloudBlob.GetSharedAccessSignature][net_sas_blob].</span></span>
* <span data-ttu-id="5c153-227">**Sdílené přístupové podpisy kontejnerů**: Když každý úkol dokončí svojí práci ve výpočetním uzlu, odešle svůj výstupní soubor do kontejneru *output* ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5c153-227">**Container shared access signatures**: As each task finishes its work on the compute node, it uploads its output file to the *output* container in Azure Storage.</span></span> <span data-ttu-id="5c153-228">Aby to mohl provést, používá TaskApplication sdílený přístupový podpis kontejneru, který zajišťuje oprávnění k zápisu do kontejneru jako součást cesty při nahrávání souboru.</span><span class="sxs-lookup"><span data-stu-id="5c153-228">To do so, TaskApplication uses a container shared access signature that provides write access to the container as part of the path when it uploads the file.</span></span> <span data-ttu-id="5c153-229">Získání sdíleného přístupového podpisu kontejneru se provádí podobným způsobem jako získávání sdíleného přístupového podpisu objektu blob.</span><span class="sxs-lookup"><span data-stu-id="5c153-229">Obtaining the container shared access signature is done in a similar fashion as when obtaining the blob shared access signature.</span></span> <span data-ttu-id="5c153-230">V aplikaci DotNetTutorial zjistíte, že pomocná metoda `GetContainerSasUrl` za tímto účelem volá [CloudBlobContainer.GetSharedAccessSignature][net_sas_container].</span><span class="sxs-lookup"><span data-stu-id="5c153-230">In DotNetTutorial, you will find that the `GetContainerSasUrl` helper method calls [CloudBlobContainer.GetSharedAccessSignature][net_sas_container] to do so.</span></span> <span data-ttu-id="5c153-231">Další informace o tom, jak TaskApplication používá sdílený přístupový podpis kontejneru, se dočtete v kroku 6: Sledování úkolů.</span><span class="sxs-lookup"><span data-stu-id="5c153-231">You'll read more about how TaskApplication uses the container shared access signature in "Step 6: Monitor Tasks."</span></span>

> [!TIP]
> <span data-ttu-id="5c153-232">Přečtěte si dvoudílný článek, který pojednává o sdíleném přístupovém podpisu: [Část 1: Vysvětlení modelu sdíleného přístupového podpisu (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) a [Část 2: Vytvoření a používání sdíleného přístupového podpisu (SAS) se službou Blob Storage](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md). Dozvíte se další informace o zajišťování bezpečného přístupu k datům v účtu služby Storage.</span><span class="sxs-lookup"><span data-stu-id="5c153-232">Check out the two-part series on shared access signatures, [Part 1: Understanding the shared access signature (SAS) model](../storage/common/storage-dotnet-shared-access-signature-part-1.md) and [Part 2: Create and use a shared access signature (SAS) with Blob storage](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), to learn more about providing secure access to data in your Storage account.</span></span>
>
>

## <a name="step-3-create-batch-pool"></a><span data-ttu-id="5c153-233">Krok 3: Vytvoření fondu služby Batch</span><span class="sxs-lookup"><span data-stu-id="5c153-233">Step 3: Create Batch pool</span></span>
<span data-ttu-id="5c153-234">![Vytvoření fondu Batch][3]
</span><span class="sxs-lookup"><span data-stu-id="5c153-234">![Create a Batch pool][3]
</span></span><br/>

<span data-ttu-id="5c153-235">**Fond** Batch je kolekce výpočetních uzlů (virtuálních počítačů), na kterých služba Batch provádí úkoly z úlohy.</span><span class="sxs-lookup"><span data-stu-id="5c153-235">A Batch **pool** is a collection of compute nodes (virtual machines) on which Batch executes a job's tasks.</span></span>

<span data-ttu-id="5c153-236">Po nahrání aplikačních a datových souborů do účtu úložiště pomocí rozhraní Azure Storage API zahájí *DotNetTutorial* volání služby Batch pomocí rozhraní API poskytovaných knihovnou Batch .NET.</span><span class="sxs-lookup"><span data-stu-id="5c153-236">After uploading the application and data files to the Storage account with Azure Storage APIs, *DotNetTutorial* begins making calls to the Batch service with APIs provided by the Batch .NET library.</span></span> <span data-ttu-id="5c153-237">Kód nejprve vytvoří [BatchClient][net_batchclient]:</span><span class="sxs-lookup"><span data-stu-id="5c153-237">The code first creates a [BatchClient][net_batchclient]:</span></span>

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

<span data-ttu-id="5c153-238">Na účtu Batch potom příklad pomocí volání `CreatePoolIfNotExistsAsync` vytvoří fond výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="5c153-238">Next, the sample creates a pool of compute nodes in the Batch account with a call to `CreatePoolIfNotExistsAsync`.</span></span> <span data-ttu-id="5c153-239">`CreatePoolIfNotExistsAsync` používá k vytvoření nového fondu ve službě Batch metodu [BatchClient.PoolOperations.CreatePool][net_pool_create]:</span><span class="sxs-lookup"><span data-stu-id="5c153-239">`CreatePoolIfNotExistsAsync` uses the [BatchClient.PoolOperations.CreatePool][net_pool_create] method to create a new pool in the Batch service:</span></span>

```csharp
private static async Task CreatePoolIfNotExistAsync(BatchClient batchClient, string poolId, IList<ResourceFile> resourceFiles)
{
    CloudPool pool = null;
    try
    {
        Console.WriteLine("Creating pool [{0}]...", poolId);

        // Create the unbound pool. Until we call CloudPool.Commit() or CommitAsync(), no pool is actually created in the
        // Batch service. This CloudPool instance is therefore considered "unbound," and we can modify its properties.
        pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicatedComputeNodes: 3,                                             // 3 compute nodes
            virtualMachineSize: "small",                                                // single-core, 1.75 GB memory, 225 GB disk
            cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));   // Windows Server 2012 R2

        // Create and assign the StartTask that will be executed when compute nodes join the pool.
        // In this case, we copy the StartTask's resource files (that will be automatically downloaded
        // to the node by the StartTask) into the shared directory that all tasks will have access to.
        pool.StartTask = new StartTask
        {
            // Specify a command line for the StartTask that copies the task application files to the
            // node's shared directory. Every compute node in a Batch pool is configured with a number
            // of pre-defined environment variables that can be referenced by commands or applications
            // run by tasks.

            // Since a successful execution of robocopy can return a non-zero exit code (e.g. 1 when one or
            // more files were successfully copied) we need to manually exit with a 0 for Batch to recognize
            // StartTask execution success.
            CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
            ResourceFiles = resourceFiles,
            WaitForSuccess = true
        };

        await pool.CommitAsync();
    }
    catch (BatchException be)
    {
        // Swallow the specific error code PoolExists since that is expected if the pool already exists
        if (be.RequestInformation?.BatchError != null && be.RequestInformation.BatchError.Code == BatchErrorCodeStrings.PoolExists)
        {
            Console.WriteLine("The pool {0} already existed when we tried to create it", poolId);
        }
        else
        {
            throw; // Any other exception is unexpected
        }
    }
}
```

<span data-ttu-id="5c153-240">Když vytváříte fond pomocí [CreatePool][net_pool_create], zadáváte několik parametrů, třeba počet výpočetních uzlů, [velikost uzlů](../cloud-services/cloud-services-sizes-specs.md) a operační systém uzlů.</span><span class="sxs-lookup"><span data-stu-id="5c153-240">When you create a pool with [CreatePool][net_pool_create], you specify several parameters such as the number of compute nodes, the [size of the nodes](../cloud-services/cloud-services-sizes-specs.md), and the nodes' operating system.</span></span> <span data-ttu-id="5c153-241">V aplikaci *DotNetTutorial* používáme [CloudServiceConfiguration][net_cloudserviceconfiguration], abychom ve službě [Cloud Services](../cloud-services/cloud-services-guestos-update-matrix.md) zadali systém Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="5c153-241">In *DotNetTutorial*, we use [CloudServiceConfiguration][net_cloudserviceconfiguration] to specify Windows Server 2012 R2 from [Cloud Services](../cloud-services/cloud-services-guestos-update-matrix.md).</span></span> 

<span data-ttu-id="5c153-242">Zadáním [VirtualMachineConfiguration][net_virtualmachineconfiguration] pro váš fond můžete také vytvořit fondy výpočetních uzlů, které jsou virtuálními počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="5c153-242">You can also create pools of compute nodes that are Azure Virtual Machines (VMs) by specifying the [VirtualMachineConfiguration][net_virtualmachineconfiguration] for your pool.</span></span> <span data-ttu-id="5c153-243">Fond výpočetních uzlů virtuálních počítačů můžete vytvořit z [image systému Linux](batch-linux-nodes.md) nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="5c153-243">You can create a pool of VM compute nodes from either Windows or [Linux images](batch-linux-nodes.md).</span></span> <span data-ttu-id="5c153-244">Zdrojem vašich imagí virtuálních počítačů může být:</span><span class="sxs-lookup"><span data-stu-id="5c153-244">The source for your VM images can be either:</span></span>

- <span data-ttu-id="5c153-245">Web [Azure Virtual Machines Marketplace][vm_marketplace] poskytující image systému Windows i Linux, které jsou připravené k použití.</span><span class="sxs-lookup"><span data-stu-id="5c153-245">The [Azure Virtual Machines Marketplace][vm_marketplace], which provides both Windows and Linux images that are ready-to-use.</span></span> 
- <span data-ttu-id="5c153-246">Vlastní image, kterou připravíte a poskytnete.</span><span class="sxs-lookup"><span data-stu-id="5c153-246">A custom image that you prepare and provide.</span></span> <span data-ttu-id="5c153-247">Další informace o vlastních imagích najdete v tématu [Vývoj rozsáhlých paralelních výpočetních řešení pomocí služby Batch](batch-api-basics.md#pool).</span><span class="sxs-lookup"><span data-stu-id="5c153-247">For more details about custom images, see [Develop large-scale parallel compute solutions with Batch](batch-api-basics.md#pool).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5c153-248">Za výpočetní prostředky ve službě Batch vám budou účtované poplatky.</span><span class="sxs-lookup"><span data-stu-id="5c153-248">You are charged for compute resources in Batch.</span></span> <span data-ttu-id="5c153-249">Pokud chcete náklady minimalizovat, můžete před spuštěním ukázky snížit `targetDedicatedComputeNodes` na hodnotu 1.</span><span class="sxs-lookup"><span data-stu-id="5c153-249">To minimize costs, you can lower `targetDedicatedComputeNodes` to 1 before you run the sample.</span></span>
>
>

<span data-ttu-id="5c153-250">Spolu s těmito fyzickými vlastnostmi uzlu můžete určit také vlastnost [StartTask][net_pool_starttask] fondu.</span><span class="sxs-lookup"><span data-stu-id="5c153-250">Along with these physical node properties, you may also specify a [StartTask][net_pool_starttask] for the pool.</span></span> <span data-ttu-id="5c153-251">StartTask se spustí na každém uzlu, když se takový uzel připojí k fondu, a taky pokaždé, když se uzel restartuje.</span><span class="sxs-lookup"><span data-stu-id="5c153-251">The StartTask executes on each node as that node joins the pool, and each time a node is restarted.</span></span> <span data-ttu-id="5c153-252">StartTask je zvláště užitečná pro instalaci aplikací na výpočetní uzly před spuštěním úkolů.</span><span class="sxs-lookup"><span data-stu-id="5c153-252">The StartTask is especially useful for installing applications on compute nodes prior to the execution of tasks.</span></span> <span data-ttu-id="5c153-253">Pokud vaše úkoly například zpracovávají data pomocí skriptů Python, můžete StartTask použít k instalaci Pythonu na výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="5c153-253">For example, if your tasks process data by using Python scripts, you could use a StartTask to install Python on the compute nodes.</span></span>

<span data-ttu-id="5c153-254">V této ukázkové aplikaci StartTask zkopíruje soubory, které stáhne ze služby Storage (které je určené vlastností [StartTask][net_starttask].[ResourceFiles][net_starttask_resourcefiles]) z pracovního adresáře StartTask do sdíleného adresáře, ke kterému mají přístup *všechny* úkoly spuštěné v takovém uzlu.</span><span class="sxs-lookup"><span data-stu-id="5c153-254">In this sample application, the StartTask copies the files that it downloads from Storage (which are specified by using the [StartTask][net_starttask].[ResourceFiles][net_starttask_resourcefiles] property) from the StartTask working directory to the shared directory that *all* tasks running on the node can access.</span></span> <span data-ttu-id="5c153-255">V podstatě zkopíruje soubor `TaskApplication.exe` a jeho závislé položky do sdíleného adresáře v každém uzlu v okamžiku, kdy se uzel připojí k fondu, aby každý úkol spuštěný v uzlu měl k tomuto souboru přístup.</span><span class="sxs-lookup"><span data-stu-id="5c153-255">Essentially, this copies `TaskApplication.exe` and its dependencies to the shared directory on each node as the node joins the pool, so that any tasks that run on the node can access it.</span></span>

> [!TIP]
> <span data-ttu-id="5c153-256">Funkce **balíčků aplikací** v Azure Batch nabízí další způsob, jak dostat aplikaci na výpočetní uzly v rámci fondu.</span><span class="sxs-lookup"><span data-stu-id="5c153-256">The **application packages** feature of Azure Batch provides another way to get your application onto the compute nodes in a pool.</span></span> <span data-ttu-id="5c153-257">Podrobnosti najdete v tématu [Nasazení aplikací do výpočetních uzlů pomocí balíčků aplikací Batch](batch-application-packages.md).</span><span class="sxs-lookup"><span data-stu-id="5c153-257">See [Deploy applications to compute nodes with Batch application packages](batch-application-packages.md) for details.</span></span>
>
>

<span data-ttu-id="5c153-258">Ve výše uvedeném fragmentu kódu je také zajímavé použití dvou proměnných prostředí ve vlastnosti *CommandLine* v StartTask: `%AZ_BATCH_TASK_WORKING_DIR%` a `%AZ_BATCH_NODE_SHARED_DIR%`.</span><span class="sxs-lookup"><span data-stu-id="5c153-258">Also notable in the code snippet above is the use of two environment variables in the *CommandLine* property of the StartTask: `%AZ_BATCH_TASK_WORKING_DIR%` and `%AZ_BATCH_NODE_SHARED_DIR%`.</span></span> <span data-ttu-id="5c153-259">Každý výpočetní uzel v rámci fondu Batch je automaticky nakonfigurovaný pomocí řady proměnných prostředí, které se týkají služby Batch.</span><span class="sxs-lookup"><span data-stu-id="5c153-259">Each compute node within a Batch pool is automatically configured with several environment variables that are specific to Batch.</span></span> <span data-ttu-id="5c153-260">Jakýkoli proces spuštěný úkolem má přístup k těmto proměnným prostředí.</span><span class="sxs-lookup"><span data-stu-id="5c153-260">Any process that is executed by a task has access to these environment variables.</span></span>

> [!TIP]
> <span data-ttu-id="5c153-261">Další informace o proměnných prostředí, které jsou dostupné na výpočetní uzlech ve fondu Batch, a taky informace o pracovních adresářích úkolů najdete v částech [Nastavení prostředí pro úkoly](batch-api-basics.md#environment-settings-for-tasks) a [Soubory a adresáře](batch-api-basics.md#files-and-directories) v článku [Přehled funkcí služby Batch pro vývojáře](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="5c153-261">To find out more about the environment variables that are available on compute nodes in a Batch pool, and information on task working directories, see the [Environment settings for tasks](batch-api-basics.md#environment-settings-for-tasks) and [Files and directories](batch-api-basics.md#files-and-directories) sections in the [Batch feature overview for developers](batch-api-basics.md).</span></span>
>
>

## <a name="step-4-create-batch-job"></a><span data-ttu-id="5c153-262">Krok 4: Vytvoření úlohy Batch</span><span class="sxs-lookup"><span data-stu-id="5c153-262">Step 4: Create Batch job</span></span>
<span data-ttu-id="5c153-263">![Vytvoření úlohy Batch][4]</span><span class="sxs-lookup"><span data-stu-id="5c153-263">![Create Batch job][4]</span></span><br/>

<span data-ttu-id="5c153-264">**Úloha** Batch je kolekcí úkolů a je přidružená k fondu výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="5c153-264">A Batch **job** is a collection of tasks, and is associated with a pool of compute nodes.</span></span> <span data-ttu-id="5c153-265">Úkoly v úloze se spustit na přidružených výpočetních uzlech fondu.</span><span class="sxs-lookup"><span data-stu-id="5c153-265">The tasks in a job execute on the associated pool's compute nodes.</span></span>

<span data-ttu-id="5c153-266">Úlohu můžete použít nejen k uspořádání a sledování úkolů v souvisejících úlohách, ale také k nastavení určitých omezení – například maximálního runtime úlohy (a při rozšíření i pro její úkoly) a také priority úloh ve vztahu k dalším úlohám na účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="5c153-266">You can use a job not only for organizing and tracking tasks in related workloads, but also for imposing certain constraints--such as the maximum runtime for the job (and by extension, its tasks) as well as job priority in relation to other jobs in the Batch account.</span></span> <span data-ttu-id="5c153-267">V tomto příkladu je úloha přidružená jenom k fondu, který byl vytvořen v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="5c153-267">In this example, however, the job is associated only with the pool that was created in step #3.</span></span> <span data-ttu-id="5c153-268">Žádné další vlastnosti se nekonfigurují.</span><span class="sxs-lookup"><span data-stu-id="5c153-268">No additional properties are configured.</span></span>

<span data-ttu-id="5c153-269">Všechny úlohy Batch jsou přidružené ke konkrétnímu fondu.</span><span class="sxs-lookup"><span data-stu-id="5c153-269">All Batch jobs are associated with a specific pool.</span></span> <span data-ttu-id="5c153-270">Toto přidružení označuje uzly, na kterých se úkoly úlohy spustí.</span><span class="sxs-lookup"><span data-stu-id="5c153-270">This association indicates which nodes the job's tasks will execute on.</span></span> <span data-ttu-id="5c153-271">Toto určíte pomocí vlastnosti [CloudJob.PoolInformation][net_job_poolinfo], jak je ukázáno v následujícím fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="5c153-271">You specify this by using the [CloudJob.PoolInformation][net_job_poolinfo] property, as shown in the code snippet below.</span></span>

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

<span data-ttu-id="5c153-272">Po vytvoření úlohy budou přidány úkoly, které budou provádět práci.</span><span class="sxs-lookup"><span data-stu-id="5c153-272">Now that a job has been created, tasks are added to perform the work.</span></span>

## <a name="step-5-add-tasks-to-job"></a><span data-ttu-id="5c153-273">Krok 5: Přidání úkolů do úlohy</span><span class="sxs-lookup"><span data-stu-id="5c153-273">Step 5: Add tasks to job</span></span>
<span data-ttu-id="5c153-274">![Přidání úkolů do úlohy][5]</span><span class="sxs-lookup"><span data-stu-id="5c153-274">![Add tasks to job][5]</span></span><br/><span data-ttu-id="5c153-275">
*(1) Úkoly jsou přidány do úlohy, (2) úkoly jsou naplánovány ke spuštění na uzlech a (3) úkoly stahují datové soubory ke zpracování*</span><span class="sxs-lookup"><span data-stu-id="5c153-275">
*(1) Tasks are added to the job, (2) the tasks are scheduled to run on nodes, and (3) the tasks download the data files to process*</span></span>

<span data-ttu-id="5c153-276">**Úkoly** Batch jsou jednotlivé jednotky práce, které se spouští na výpočetních uzlech.</span><span class="sxs-lookup"><span data-stu-id="5c153-276">Batch **tasks** are the individual units of work that execute on the compute nodes.</span></span> <span data-ttu-id="5c153-277">Úkol má příkazový řádek a spouští skripty nebo spustitelné soubory, které jste v takovém příkazovém řádku určili.</span><span class="sxs-lookup"><span data-stu-id="5c153-277">A task has a command line and runs the scripts or executables that you specify in that command line.</span></span>

<span data-ttu-id="5c153-278">Aby mohly skutečně provést nějakou práci, musí úkoly nejprve přidat do úlohy.</span><span class="sxs-lookup"><span data-stu-id="5c153-278">To actually perform work, tasks must be added to a job.</span></span> <span data-ttu-id="5c153-279">Každý [CloudTask][net_task] je nakonfigurovaný pomocí vlastnosti příkazového řádku a [ResourceFiles][net_task_resourcefiles] (stejně jako u StartTask fondu), kterou si úkol stáhne do uzlu předtím, než se jeho příkazový řádek automaticky spustí.</span><span class="sxs-lookup"><span data-stu-id="5c153-279">Each [CloudTask][net_task] is configured by using a command-line property and [ResourceFiles][net_task_resourcefiles] (as with the pool's StartTask) that the task downloads to the node before its command line is automatically executed.</span></span> <span data-ttu-id="5c153-280">V ukázkovém projektu *DotNetTutorial* každý úkol zpracovává jenom jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="5c153-280">In the *DotNetTutorial* sample project, each task processes only one file.</span></span> <span data-ttu-id="5c153-281">Proto jeho kolekce ResourceFiles obsahuje jen jeden prvek.</span><span class="sxs-lookup"><span data-stu-id="5c153-281">Thus, its ResourceFiles collection contains a single element.</span></span>

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks to job [{1}]...", inputFiles.Count, jobId);

    // Create a collection to hold the tasks that we'll be adding to the job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of the tasks. Because we copied the task application to the
    // node's shared directory with the pool's StartTask, we can access it via
    // the shared directory on the node that the task runs on.
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

    // Add the tasks as a collection, as opposed to issuing a separate AddTask call
    // for each. Bulk task submission helps to ensure efficient underlying API calls
    // to the Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [!IMPORTANT]
> <span data-ttu-id="5c153-282">Když přistupují k proměnným prostředí, například k `%AZ_BATCH_NODE_SHARED_DIR%`, nebo když spouští aplikaci, která se nedá najít na `PATH` uzlu, musí příkazové řádky úkolu obsahovat předponu `cmd /c`.</span><span class="sxs-lookup"><span data-stu-id="5c153-282">When they access environment variables such as `%AZ_BATCH_NODE_SHARED_DIR%` or execute an application not found in the node's `PATH`, task command lines must be prefixed with `cmd /c`.</span></span> <span data-ttu-id="5c153-283">Tím se explicitně spustí překladač příkazů a dostane pokyn, aby se po provedení příkazu ukončil.</span><span class="sxs-lookup"><span data-stu-id="5c153-283">This will explicitly execute the command interpreter and instruct it to terminate after carrying out your command.</span></span> <span data-ttu-id="5c153-284">Tento požadavek není nutný, pokud úkoly spouštějí jen aplikace nacházející se na `PATH` uzlu (například *robocopy.exe* nebo *powershell.exe*) a nepoužívají se žádné proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="5c153-284">This requirement is unnecessary if your tasks execute an application in the node's `PATH` (such as *robocopy.exe* or *powershell.exe*) and no environment variables are used.</span></span>
>
>

<span data-ttu-id="5c153-285">Ve smyčce `foreach` ve výše uvedeném fragmentu kódu můžete vidět, že příkazový řádek úkolu je vytvořený tak, aby se aplikaci *TaskApplication.exe* předávaly tři argumenty příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="5c153-285">Within the `foreach` loop in the code snippet above, you can see that the command line for the task is constructed such that three command-line arguments are passed to *TaskApplication.exe*:</span></span>

1. <span data-ttu-id="5c153-286">**První argument** je cesta k souboru, který má být zpracován.</span><span class="sxs-lookup"><span data-stu-id="5c153-286">The **first argument** is the path of the file to process.</span></span> <span data-ttu-id="5c153-287">Jedná se o místní cestu k souboru, protože soubor existuje na uzlu.</span><span class="sxs-lookup"><span data-stu-id="5c153-287">This is the local path to the file as it exists on the node.</span></span> <span data-ttu-id="5c153-288">Když byl objekt ResourceFile v `UploadFileToContainerAsync` v předchozí části vytvořen, použil se pro tuto vlastnost název souboru (jako parametr pro konstruktor ResourceFile).</span><span class="sxs-lookup"><span data-stu-id="5c153-288">When the ResourceFile object in `UploadFileToContainerAsync` was first created above, the file name was used for this property (as a parameter to the ResourceFile constructor).</span></span> <span data-ttu-id="5c153-289">To znamená, že se soubor nachází ve stejném adresáři jako *TaskApplication.exe*.</span><span class="sxs-lookup"><span data-stu-id="5c153-289">This indicates that the file can be found in the same directory as *TaskApplication.exe*.</span></span>
2. <span data-ttu-id="5c153-290">**Druhý argument** určuje, že nejčastější slova v počtu *N* mají být zapsána do výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="5c153-290">The **second argument** specifies that the top *N* words should be written to the output file.</span></span> <span data-ttu-id="5c153-291">V ukázce je to pevně zakódované, aby se do výstupního souboru zapisovala tři nejčastější slova.</span><span class="sxs-lookup"><span data-stu-id="5c153-291">In the sample, this is hard-coded so that the top three words are written to the output file.</span></span>
3. <span data-ttu-id="5c153-292">**Třetí argument** je sdílený přístupový podpis (SAS), který zajišťuje oprávnění k zápisu do kontejneru **output** ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5c153-292">The **third argument** is the shared access signature (SAS) that provides write access to the **output** container in Azure Storage.</span></span> <span data-ttu-id="5c153-293">*TaskApplication.exe* používá tuto adresu URL se sdíleným přístupovým podpisem při nahrávání výstupního souboru do služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5c153-293">*TaskApplication.exe* uses this shared access signature URL when it uploads the output file to Azure Storage.</span></span> <span data-ttu-id="5c153-294">Kód pro metodu `UploadFileToContainer` můžete najít v souboru `Program.cs` z projektu TaskApplication:</span><span class="sxs-lookup"><span data-stu-id="5c153-294">You can find the code for this in the `UploadFileToContainer` method in the TaskApplication project's `Program.cs` file:</span></span>

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload the file (as a new blob) to the container
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

                // Indicate that a failure has occurred so that when the Batch service
                // sets the CloudTask.ExecutionInformation.ExitCode for the task that
                // executed this application, it properly indicates that there was a
                // problem with the task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a><span data-ttu-id="5c153-295">Krok 6: Sledování úkolů</span><span class="sxs-lookup"><span data-stu-id="5c153-295">Step 6: Monitor tasks</span></span>
<span data-ttu-id="5c153-296">![Sledujte úkoly.][6]</span><span class="sxs-lookup"><span data-stu-id="5c153-296">![Monitor tasks][6]</span></span><br/><span data-ttu-id="5c153-297">
*Klientská aplikace (1) sleduje stav dokončení a úspěšnosti úkolů a (2) úkoly nahrávají výsledná data do služby Azure Storage*.</span><span class="sxs-lookup"><span data-stu-id="5c153-297">
*The client application (1) monitors the tasks for completion and success status, and (2) the tasks upload result data to Azure Storage*</span></span>

<span data-ttu-id="5c153-298">Pokud úkoly přidáte do úlohy, budou automaticky zařazeny do fronty a bude naplánováno jejich spuštění na výpočetních uzlech ve fondu, který je k úloze přidružený.</span><span class="sxs-lookup"><span data-stu-id="5c153-298">When tasks are added to a job, they are automatically queued and scheduled for execution on compute nodes within the pool associated with the job.</span></span> <span data-ttu-id="5c153-299">Na základě vámi zadaných nastavení služba Batch zpracuje veškeré řazení úkolů do fronty, plánování úkolů, opakované spouštění a další povinnosti spojené se správou úkolů místo vás.</span><span class="sxs-lookup"><span data-stu-id="5c153-299">Based on the settings you specify, Batch handles all task queuing, scheduling, retrying, and other task administration duties for you.</span></span>

<span data-ttu-id="5c153-300">Ke sledování provádění úkolů existuje mnoho přístupů.</span><span class="sxs-lookup"><span data-stu-id="5c153-300">There are many approaches to monitoring task execution.</span></span> <span data-ttu-id="5c153-301">DotNetTutorial ukazuje jednoduchý příklad, který hlásí jenom dokončení a stavy úspěchu/neúspěchu úkolu.</span><span class="sxs-lookup"><span data-stu-id="5c153-301">DotNetTutorial shows a simple example that reports only on completion and task failure or success states.</span></span> <span data-ttu-id="5c153-302">V rámci metody `MonitorTasks` v souboru `Program.cs` z projektu DotNetTutorial existují tři koncepty Batch .NET, které je na místě prodiskutovat.</span><span class="sxs-lookup"><span data-stu-id="5c153-302">Within the `MonitorTasks` method in DotNetTutorial's `Program.cs`, there are three Batch .NET concepts that warrant discussion.</span></span> <span data-ttu-id="5c153-303">Jsou uvedené níže v pořadí podle jejich výskytu:</span><span class="sxs-lookup"><span data-stu-id="5c153-303">They are listed below in their order of appearance:</span></span>

1. <span data-ttu-id="5c153-304">**ODATADetailLevel**: Zadání [ODATADetailLevel][net_odatadetaillevel] v operaci vypsání seznamu (například získání seznamu úkolů úlohy) je důležité pro zajištění výkonu aplikace Batch.</span><span class="sxs-lookup"><span data-stu-id="5c153-304">**ODATADetailLevel**: Specifying [ODATADetailLevel][net_odatadetaillevel] in list operations (such as obtaining a list of a job's tasks) is essential in ensuring Batch application performance.</span></span> <span data-ttu-id="5c153-305">Pokud máte v úmyslu provádět jakékoli sledování stavu v aplikacích Batch, přidejte si do seznamu svých materiálů k prostudování článek [Efektivní dotazování na službu Azure Batch](batch-efficient-list-queries.md).</span><span class="sxs-lookup"><span data-stu-id="5c153-305">Add [Query the Azure Batch service efficiently](batch-efficient-list-queries.md) to your reading list if you plan on doing any sort of status monitoring within your Batch applications.</span></span>
2. <span data-ttu-id="5c153-306">**TaskStateMonitor**: [TaskStateMonitor][net_taskstatemonitor] poskytuje aplikacím Batch .NET pomocné nástroje ke sledování stavů úkolů.</span><span class="sxs-lookup"><span data-stu-id="5c153-306">**TaskStateMonitor**: [TaskStateMonitor][net_taskstatemonitor] provides Batch .NET applications with helper utilities for monitoring task states.</span></span> <span data-ttu-id="5c153-307">V `MonitorTasks` aplikace *DotNetTutorial* počká, až všechny úkoly dosáhnou ve stanoveném časovém limitu stavu [TaskState.Completed][net_taskstate].</span><span class="sxs-lookup"><span data-stu-id="5c153-307">In `MonitorTasks`, *DotNetTutorial* waits for all tasks to reach [TaskState.Completed][net_taskstate] within a time limit.</span></span> <span data-ttu-id="5c153-308">Potom úlohu ukončí.</span><span class="sxs-lookup"><span data-stu-id="5c153-308">Then it terminates the job.</span></span>
3. <span data-ttu-id="5c153-309">**TerminateJobAsync**: Ukončení úlohy pomocí [JobOperations.TerminateJobAsync][net_joboperations_terminatejob] (nebo blokování JobOperations.TerminateJob) označí tuto úlohu jako dokončenou.</span><span class="sxs-lookup"><span data-stu-id="5c153-309">**TerminateJobAsync**: Terminating a job with [JobOperations.TerminateJobAsync][net_joboperations_terminatejob] (or the blocking JobOperations.TerminateJob) marks that job as completed.</span></span> <span data-ttu-id="5c153-310">To je velmi důležité provést, pokud vaše řešení Batch používá [JobReleaseTask][net_jobreltask].</span><span class="sxs-lookup"><span data-stu-id="5c153-310">It is essential to do so if your Batch solution uses a [JobReleaseTask][net_jobreltask].</span></span> <span data-ttu-id="5c153-311">Jedná se o zvláštní typ úkolu, který je popsaný v článku [Úkoly přípravy a dokončení úlohy](batch-job-prep-release.md).</span><span class="sxs-lookup"><span data-stu-id="5c153-311">This is a special type of task, which is described in [Job preparation and completion tasks](batch-job-prep-release.md).</span></span>

<span data-ttu-id="5c153-312">Metodu `MonitorTasks` ze souboru `Program.cs` v aplikaci *DotNetTutorial* vidíte zde:</span><span class="sxs-lookup"><span data-stu-id="5c153-312">The `MonitorTasks` method from *DotNetTutorial*'s `Program.cs` appears below:</span></span>

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed to reach the Completed state within the timeout period.";

    // Obtain the collection of tasks currently managed by the job. Note that we use
    // a detail level to  specify that only the "id" property of each task should be
    // populated. Using a detail level for all list operations helps to lower
    // response time from the Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor to monitor the state of our tasks. In this case, we
    // will wait for all tasks to reach the Completed state.
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

    // All tasks have reached the "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property to ensure that it did not encounter a failure
    // or return a non-zero exit code.

    // Update the detail level to populate only the task id and executionInfo
    // properties. We refresh the tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate the task's properties with the latest info from the Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.Result == TaskExecutionResult.Failure)
        {
            // A task with failure information set indicates there was a problem with the task. It is important to note that
            // the task's state can be "Completed," yet still have encountered a failure.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a failure: {1}", task.Id, task.ExecutionInformation.FailureInformation.Message);
            if (task.ExecutionInformation.ExitCode != 0)
            {
                // A non-zero exit code may indicate that the application executed by the task encountered an error
                // during execution. As not every application returns non-zero on failure by default (e.g. robocopy),
                // your implementation of error checking may differ from this example.

                Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
            }
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within the specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a><span data-ttu-id="5c153-313">Krok 7: Stažení výstupu úkolu</span><span class="sxs-lookup"><span data-stu-id="5c153-313">Step 7: Download task output</span></span>
<span data-ttu-id="5c153-314">![Stažení výstupu úkolu ze služby Storage][7]</span><span class="sxs-lookup"><span data-stu-id="5c153-314">![Download task output from Storage][7]</span></span><br/>

<span data-ttu-id="5c153-315">Po dokončení úlohy můžete ze služby Azure Storage stáhnout výstup úkolů.</span><span class="sxs-lookup"><span data-stu-id="5c153-315">Now that the job is completed, the output from the tasks can be downloaded from Azure Storage.</span></span> <span data-ttu-id="5c153-316">To provedete pomocí volání metody `DownloadBlobsFromContainerAsync` v souboru `Program.cs` z aplikace *DotNetTutorial*:</span><span class="sxs-lookup"><span data-stu-id="5c153-316">This is done with a call to `DownloadBlobsFromContainerAsync` in *DotNetTutorial*'s `Program.cs`:</span></span>

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference to a previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all the block blobs in the specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference to the current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents to a file in the specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded to {0}", directoryPath);
}
```

> [!NOTE]
> <span data-ttu-id="5c153-317">Volání `DownloadBlobsFromContainerAsync` v aplikaci *DotNetTutorial* určuje, že soubory se mají stahovat do složky `%TEMP%`.</span><span class="sxs-lookup"><span data-stu-id="5c153-317">The call to `DownloadBlobsFromContainerAsync` in the *DotNetTutorial* application specifies that the files should be downloaded to your `%TEMP%` folder.</span></span> <span data-ttu-id="5c153-318">Umístění výstupu můžete podle libosti změnit.</span><span class="sxs-lookup"><span data-stu-id="5c153-318">Feel free to modify this output location.</span></span>
>
>

## <a name="step-8-delete-containers"></a><span data-ttu-id="5c153-319">Krok 8: Odstranění kontejnerů</span><span class="sxs-lookup"><span data-stu-id="5c153-319">Step 8: Delete containers</span></span>
<span data-ttu-id="5c153-320">Vzhledem k tomu, že musíte platit za data, která si necháváte ve službě Azure Storage, doporučujeme odebrat objekty blob, které už pro úlohy Batch nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="5c153-320">Because you are charged for data that resides in Azure Storage, it's always a good idea to remove blobs that are no longer needed for your Batch jobs.</span></span> <span data-ttu-id="5c153-321">V souboru `Program.cs` z aplikace DotNetTutorial se to provádí pomocí tří volání pomocné metody `DeleteContainerAsync`:</span><span class="sxs-lookup"><span data-stu-id="5c153-321">In DotNetTutorial's `Program.cs`, this is done with three calls to the helper method `DeleteContainerAsync`:</span></span>

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

<span data-ttu-id="5c153-322">Metoda sama jenom získá referencí na kontejner a potom zavolá [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:</span><span class="sxs-lookup"><span data-stu-id="5c153-322">The method itself merely obtains a reference to the container, and then calls [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:</span></span>

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

## <a name="step-9-delete-the-job-and-the-pool"></a><span data-ttu-id="5c153-323">Krok 9: Odstranění úlohy a fondu</span><span class="sxs-lookup"><span data-stu-id="5c153-323">Step 9: Delete the job and the pool</span></span>
<span data-ttu-id="5c153-324">V posledním kroku budete vyzváni k odstranění úlohy a fondu, které vytvořila aplikace DotNetTutorial.</span><span class="sxs-lookup"><span data-stu-id="5c153-324">In the final step, you're prompted to delete the job and the pool that were created by the DotNetTutorial application.</span></span> <span data-ttu-id="5c153-325">I když se vám neúčtují poplatky za úlohy a úlohy samotné, *účtují* se vám poplatky za výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="5c153-325">Although you're not charged for jobs and tasks themselves, you *are* charged for compute nodes.</span></span> <span data-ttu-id="5c153-326">Proto doporučujeme, abyste uzly přidělovali, jen když je to potřeba.</span><span class="sxs-lookup"><span data-stu-id="5c153-326">Thus, we recommend that you allocate nodes only as needed.</span></span> <span data-ttu-id="5c153-327">Odstraňování nepoužívaných fondů by mělo být součástí vašeho standardního procesu údržby.</span><span class="sxs-lookup"><span data-stu-id="5c153-327">Deleting unused pools can be part of your maintenance process.</span></span>

<span data-ttu-id="5c153-328">[JobOperations][net_joboperations] a [PoolOperations][net_pooloperations] z BatchClient mají odpovídající metody odstranění, které se volají, pokud uživatel potvrdí odstranění:</span><span class="sxs-lookup"><span data-stu-id="5c153-328">The BatchClient's [JobOperations][net_joboperations] and [PoolOperations][net_pooloperations] both have corresponding deletion methods, which are called if the user confirms deletion:</span></span>

```csharp
// Clean up the resources we've created in the Batch account if the user so chooses
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
> <span data-ttu-id="5c153-329">Pamatujte, že se vám účtují poplatky za výpočetní prostředky, takže odstranění nepoužívaných fondů vám ušetří náklady.</span><span class="sxs-lookup"><span data-stu-id="5c153-329">Keep in mind that you are charged for compute resources—deleting unused pools will minimize cost.</span></span> <span data-ttu-id="5c153-330">Musíme ale upozornit, že odstraněním fondu odstraníte všechny výpočetní uzly v takovém fondu a veškerá data na uzlech budou po odstranění fondu ztracená.</span><span class="sxs-lookup"><span data-stu-id="5c153-330">Also, be aware that deleting a pool deletes all compute nodes within that pool, and that any data on the nodes will be unrecoverable after the pool is deleted.</span></span>
>
>

## <a name="run-the-dotnettutorial-sample"></a><span data-ttu-id="5c153-331">Spuštění ukázkové aplikace *DotNetTutorial*</span><span class="sxs-lookup"><span data-stu-id="5c153-331">Run the *DotNetTutorial* sample</span></span>
<span data-ttu-id="5c153-332">Když spustíte ukázkovou aplikaci, bude výstup konzoly podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="5c153-332">When you run the sample application, the console output will be similar to the following.</span></span> <span data-ttu-id="5c153-333">Během provádění dojde k pozastavení při `Awaiting task completion, timeout in 00:30:00...` a mezitím se spustí výpočetní uzly fondu.</span><span class="sxs-lookup"><span data-stu-id="5c153-333">During execution, you will experience a pause at `Awaiting task completion, timeout in 00:30:00...` while the pool's compute nodes are started.</span></span> <span data-ttu-id="5c153-334">Ke sledování fondu, výpočetních uzlů, úlohy a úkolů během a po spuštění použijte [Azure Portal][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="5c153-334">Use the [Azure portal][azure_portal] to monitor your pool, compute nodes, job, and tasks during and after execution.</span></span> <span data-ttu-id="5c153-335">K zobrazení prostředků služby Storage (kontejnerů a objektů blob), které vytvořila aplikace, použijte [Azure Portal][azure_portal] nebo [Azure Storage Explorer][storage_explorers].</span><span class="sxs-lookup"><span data-stu-id="5c153-335">Use the [Azure portal][azure_portal] or the [Azure Storage Explorer][storage_explorers] to view the Storage resources (containers and blobs) that are created by the application.</span></span>

<span data-ttu-id="5c153-336">Typická doba provádění je **přibližně 5 minut**, když aplikaci spouštíte v její výchozí konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="5c153-336">Typical execution time is **approximately 5 minutes** when you run the application in its default configuration.</span></span>

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe to container [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll to container [application]...
Uploading file ..\..\taskdata1.txt to container [input]...
Uploading file ..\..\taskdata2.txt to container [input]...
Uploading file ..\..\taskdata3.txt to container [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks to job [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within the specified timeout period.
Downloading all files from container [output]...
All files downloaded to C:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a><span data-ttu-id="5c153-337">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5c153-337">Next steps</span></span>
<span data-ttu-id="5c153-338">Nebojte se provádět v projektu *DotNetTutorial* a *TaskApplication* změny a experimentovat s různými výpočetními scénáři.</span><span class="sxs-lookup"><span data-stu-id="5c153-338">Feel free to make changes to *DotNetTutorial* and *TaskApplication* to experiment with different compute scenarios.</span></span> <span data-ttu-id="5c153-339">Zkuste třeba do *TaskApplication* přidat prodlevu provádění, jaká je u [Thread.Sleep][net_thread_sleep], abyste mohli simulovat dlouhotrvající úlohy a sledovat je na portálu.</span><span class="sxs-lookup"><span data-stu-id="5c153-339">For example, try adding an execution delay to *TaskApplication*, such as with [Thread.Sleep][net_thread_sleep], to simulate long-running tasks and monitor them in the portal.</span></span> <span data-ttu-id="5c153-340">Zkuste přidat další úkoly nebo upravit počet výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="5c153-340">Try adding more tasks or adjusting the number of compute nodes.</span></span> <span data-ttu-id="5c153-341">Přidejte logiku pro kontrolu a povolte použití existujícího fondu, abyste urychlili čas provádění (*tip*: podívejte se na soubor `ArticleHelpers.cs` v projektu [Microsoft.Azure.Batch.Samples.Common][github_samples_common] v [azure-batch-samples][github_samples]).</span><span class="sxs-lookup"><span data-stu-id="5c153-341">Add logic to check for and allow the use of an existing pool to speed execution time (*hint*: check out `ArticleHelpers.cs` in the [Microsoft.Azure.Batch.Samples.Common][github_samples_common] project in [azure-batch-samples][github_samples]).</span></span>

<span data-ttu-id="5c153-342">Teď, když jste se seznámili se základním pracovním postupem řešení Batch, je čas proniknout do dalších funkcí služby Batch.</span><span class="sxs-lookup"><span data-stu-id="5c153-342">Now that you're familiar with the basic workflow of a Batch solution, it's time to dig in to the additional features of the Batch service.</span></span>

* <span data-ttu-id="5c153-343">Přečtěte si článek [Přehled funkcí Azure Batch](batch-api-basics.md), který doporučujeme všem novým uživatelům služby.</span><span class="sxs-lookup"><span data-stu-id="5c153-343">Review the [Overview of Azure Batch features](batch-api-basics.md) article, which we recommend if you're new to the service.</span></span>
* <span data-ttu-id="5c153-344">Začněte u dalších článků o vývoji pro Batch, které najdete v [Postupu výuky pro Batch][batch_learning_path] v části **Podrobný popis vývoje**.</span><span class="sxs-lookup"><span data-stu-id="5c153-344">Start on the other Batch development articles under **Development in-depth** in the [Batch learning path][batch_learning_path].</span></span>
* <span data-ttu-id="5c153-345">Podívejte se na různé implementace zpracování úlohy „N nejčastějších slov“ a použijte k tomu Batch v ukázce [TopNWords][github_topnwords].</span><span class="sxs-lookup"><span data-stu-id="5c153-345">Check out a different implementation of processing the "top N words" workload by using Batch in the [TopNWords][github_topnwords] sample.</span></span>
* <span data-ttu-id="5c153-346">Zkontrolujte [poznámky k verzi](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/Batch/DataPlane/changelog.md#azurebatch-release-notes) Batch .NET, kde najdete nejnovější změny v knihovně.</span><span class="sxs-lookup"><span data-stu-id="5c153-346">Review the Batch .NET [release notes](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/Batch/DataPlane/changelog.md#azurebatch-release-notes) for the latest changes in the library.</span></span>

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
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Odeslání aplikačních a vstupních (datových) souborů úkolů do kontejnerů"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Vytvoření fondu Batch"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Vytvoření úlohy Batch"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "Přidání úkolů do úlohy"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Sledování úkolů"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Stažení výstupu úkolu ze služby Storage"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "Pracovní postup řešení Batch (úplný diagram)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "Přihlašovací údaje Batch na portálu"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "Přihlašovací údaje služby Storage na portálu"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "Pracovní postup řešení Batch (minimální diagram)"
