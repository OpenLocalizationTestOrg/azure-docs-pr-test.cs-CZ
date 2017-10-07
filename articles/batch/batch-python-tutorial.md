---
title: "aaaTutorial - použití hello Azure Batch SDK pro jazyk Python | Microsoft Docs"
description: "Informace hello základními koncepty Azure Batch a vytvoření jednoduché řešení používá Python."
services: batch
documentationcenter: python
author: tamram
manager: timlt
editor: 
ms.assetid: 42cae157-d43d-47f8-88f5-486ccfd334f4
ms.service: batch
ms.devlang: python
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c4d5152aeef31848c50a7f2aae5e7a7e0e1e9535
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-batch-sdk-for-python"></a><span data-ttu-id="deb6c-103">Začínáme s hello Batch SDK pro jazyk Python</span><span class="sxs-lookup"><span data-stu-id="deb6c-103">Get started with hello Batch SDK for Python</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="deb6c-104">.NET</span><span class="sxs-lookup"><span data-stu-id="deb6c-104">.NET</span></span>](batch-dotnet-get-started.md)
> * [<span data-ttu-id="deb6c-105">Python</span><span class="sxs-lookup"><span data-stu-id="deb6c-105">Python</span></span>](batch-python-tutorial.md)
> * [<span data-ttu-id="deb6c-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="deb6c-106">Node.js</span></span>](batch-nodejs-get-started.md)
>
>

<span data-ttu-id="deb6c-107">Další hello Základy [Azure Batch] [ azure_batch] a hello [Batch Python] [ py_azure_sdk] klienta probereme malou aplikaci Batch napsanou v Pythonu.</span><span class="sxs-lookup"><span data-stu-id="deb6c-107">Learn hello basics of [Azure Batch][azure_batch] and hello [Batch Python][py_azure_sdk] client as we discuss a small Batch application written in Python.</span></span> <span data-ttu-id="deb6c-108">Podíváme na to jak dva ukázkové skripty použití hello Batch služby tooprocess paralelní úlohy na virtuálních počítačích s Linuxem v cloudu hello a způsob, jakým interagují s [Azure Storage](../storage/common/storage-introduction.md) pro přípravě a načítání souborů.</span><span class="sxs-lookup"><span data-stu-id="deb6c-108">We look at how two sample scripts use hello Batch service tooprocess a parallel workload on Linux virtual machines in hello cloud, and how they interact with [Azure Storage](../storage/common/storage-introduction.md) for file staging and retrieval.</span></span> <span data-ttu-id="deb6c-109">Budete další běžné pracovním postupem aplikací Batch a získáte základní přehled o hello hlavním součástem služby Batch například úlohách, úkolech, fondech a výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="deb6c-109">You'll learn a common Batch application workflow and gain a base understanding of hello major components of Batch such as jobs, tasks, pools, and compute nodes.</span></span>

<span data-ttu-id="deb6c-110">![Pracovní postup řešení Batch (Basic)][11]</span><span class="sxs-lookup"><span data-stu-id="deb6c-110">![Batch solution workflow (basic)][11]</span></span><br/>

## <a name="prerequisites"></a><span data-ttu-id="deb6c-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="deb6c-111">Prerequisites</span></span>
<span data-ttu-id="deb6c-112">Tento článek předpokládá, že máte praktické znalosti Pythonu a umíte do jisté míry pracovat s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="deb6c-112">This article assumes that you have a working knowledge of Python and familiarity with Linux.</span></span> <span data-ttu-id="deb6c-113">Předpokládá také, že jste možnost toosatisfy hello účet vytvoření požadavky, které jsou uvedeny níže Azure a hello Batch a služby úložiště.</span><span class="sxs-lookup"><span data-stu-id="deb6c-113">It also assumes that you're able toosatisfy hello account creation requirements that are specified below for Azure and hello Batch and Storage services.</span></span>

### <a name="accounts"></a><span data-ttu-id="deb6c-114">Účty</span><span class="sxs-lookup"><span data-stu-id="deb6c-114">Accounts</span></span>
* <span data-ttu-id="deb6c-115">**Účet Azure**: Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet Azure][azure_free_account].</span><span class="sxs-lookup"><span data-stu-id="deb6c-115">**Azure account**: If you don't already have an Azure subscription, [create a free Azure account][azure_free_account].</span></span>
* <span data-ttu-id="deb6c-116">**Účet Batch**: Po pořízení předplatného Azure si [vytvořte účet Azure Batch](batch-account-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="deb6c-116">**Batch account**: Once you have an Azure subscription, [create an Azure Batch account](batch-account-create-portal.md).</span></span>
* <span data-ttu-id="deb6c-117">**Účet Storage**: Viz část [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) v článku [Informace o účtech Azure Storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="deb6c-117">**Storage account**: See [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

### <a name="code-sample"></a><span data-ttu-id="deb6c-118">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="deb6c-118">Code sample</span></span>
<span data-ttu-id="deb6c-119">kurz Python Hello [ukázka kódu] [ github_article_samples] je jedním z mnoha ukázek kódu služby Batch najít v hello hello [azure-batch-samples] [ github_samples] úložiště v GitHub.</span><span class="sxs-lookup"><span data-stu-id="deb6c-119">hello Python tutorial [code sample][github_article_samples] is one of hello many Batch code samples found in hello [azure-batch-samples][github_samples] repository on GitHub.</span></span> <span data-ttu-id="deb6c-120">Všechny ukázky hello si můžete stáhnout kliknutím **klonovat nebo stáhnout > stáhnout ZIP** na domovskou stránku hello úložiště nebo kliknutím hello [azure-batch-samples-master.zip] [ github_samples_zip]přímý odkaz ke stažení.</span><span class="sxs-lookup"><span data-stu-id="deb6c-120">You can download all hello samples by clicking **Clone or download > Download ZIP** on hello repository home page, or by clicking hello [azure-batch-samples-master.zip][github_samples_zip] direct download link.</span></span> <span data-ttu-id="deb6c-121">Po extrahování obsahu souboru ZIP hello hello hello dva skripty v tomto kurzu se nacházejí v hello `article_samples` directory:</span><span class="sxs-lookup"><span data-stu-id="deb6c-121">Once you've extracted hello contents of hello ZIP file, hello two scripts for this tutorial are found in hello `article_samples` directory:</span></span>

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a><span data-ttu-id="deb6c-122">Prostředí Python</span><span class="sxs-lookup"><span data-stu-id="deb6c-122">Python environment</span></span>
<span data-ttu-id="deb6c-123">toorun hello *python_tutorial_client.py* ukázkový skript na místní pracovní stanici, musíte **překladač Pythonu** kompatibilní s verzí **2.7** nebo **3.3 +**.</span><span class="sxs-lookup"><span data-stu-id="deb6c-123">toorun hello *python_tutorial_client.py* sample script on your local workstation, you need a **Python interpreter** compatible with version **2.7** or **3.3+**.</span></span> <span data-ttu-id="deb6c-124">Hello skript byl otestován v Linuxu i Windows.</span><span class="sxs-lookup"><span data-stu-id="deb6c-124">hello script has been tested on both Linux and Windows.</span></span>

### <a name="cryptography-dependencies"></a><span data-ttu-id="deb6c-125">závislosti kryptografie</span><span class="sxs-lookup"><span data-stu-id="deb6c-125">cryptography dependencies</span></span>
<span data-ttu-id="deb6c-126">Je nutné nainstalovat hello závislosti pro hello [kryptografie] [ crypto] knihovny, vyžadují hello `azure-batch` a `azure-storage` balíčků Python.</span><span class="sxs-lookup"><span data-stu-id="deb6c-126">You must install hello dependencies for hello [cryptography][crypto] library, required by hello `azure-batch` and `azure-storage` Python packages.</span></span> <span data-ttu-id="deb6c-127">Proveďte jeden z hello následující operace, které jsou vhodné pro vaši platformu nebo prostudujte toohello [kryptografie instalace] [ crypto_install] podrobnosti o další informace:</span><span class="sxs-lookup"><span data-stu-id="deb6c-127">Perform one of hello following operations appropriate for your platform, or refer toohello [cryptography installation][crypto_install] details for more information:</span></span>

* <span data-ttu-id="deb6c-128">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="deb6c-128">Ubuntu</span></span>

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`
* <span data-ttu-id="deb6c-129">CentOS</span><span class="sxs-lookup"><span data-stu-id="deb6c-129">CentOS</span></span>

    `yum update && yum install -y gcc openssl-devel libffi-devel python-devel`
* <span data-ttu-id="deb6c-130">SLES/OpenSUSE</span><span class="sxs-lookup"><span data-stu-id="deb6c-130">SLES/OpenSUSE</span></span>

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`
* <span data-ttu-id="deb6c-131">Windows</span><span class="sxs-lookup"><span data-stu-id="deb6c-131">Windows</span></span>

    `pip install cryptography`

> [!NOTE]
> <span data-ttu-id="deb6c-132">Pokud instalace pro jazyk Python 3.3 + v systému Linux, použijte pro závislosti Python hello hello python3 ekvivalenty.</span><span class="sxs-lookup"><span data-stu-id="deb6c-132">If installing for Python 3.3+ on Linux, use hello python3 equivalents for hello Python dependencies.</span></span> <span data-ttu-id="deb6c-133">Například na Ubuntu: `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`</span><span class="sxs-lookup"><span data-stu-id="deb6c-133">For example, on Ubuntu: `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`</span></span>
>
>

### <a name="azure-packages"></a><span data-ttu-id="deb6c-134">Balíčky Azure</span><span class="sxs-lookup"><span data-stu-id="deb6c-134">Azure packages</span></span>
<span data-ttu-id="deb6c-135">Dále nainstalujte hello **Azure Batch** a **Azure Storage** balíčků Python.</span><span class="sxs-lookup"><span data-stu-id="deb6c-135">Next, install hello **Azure Batch** and **Azure Storage** Python packages.</span></span> <span data-ttu-id="deb6c-136">Oba balíčky můžete nainstalovat pomocí **pip** a hello *requirements.txt* nachází tady:</span><span class="sxs-lookup"><span data-stu-id="deb6c-136">You can install both packages by using **pip** and hello *requirements.txt* found here:</span></span>

`/azure-batch-samples/Python/Batch/requirements.txt`

<span data-ttu-id="deb6c-137">Zadejte následující **pip** příkaz tooinstall balíčků Batch a Storage hello:</span><span class="sxs-lookup"><span data-stu-id="deb6c-137">Issue following **pip** command tooinstall hello Batch and Storage packages:</span></span>

`pip install -r requirements.txt`

<span data-ttu-id="deb6c-138">Nebo můžete nainstalovat hello [azure-batch] [ pypi_batch] a [azure-storage] [ pypi_storage] balíčky Pythonu ručně:</span><span class="sxs-lookup"><span data-stu-id="deb6c-138">Or, you can install hello [azure-batch][pypi_batch] and [azure-storage][pypi_storage] Python packages manually:</span></span>

`pip install azure-batch`<br/>
`pip install azure-storage`

> [!TIP]
> <span data-ttu-id="deb6c-139">Pokud používáte Neprivilegovaný účet, může být nutné tooprefix příkazům `sudo`.</span><span class="sxs-lookup"><span data-stu-id="deb6c-139">If you are using an unprivileged account, you may need tooprefix your commands with `sudo`.</span></span> <span data-ttu-id="deb6c-140">Například, `sudo pip install -r requirements.txt`.</span><span class="sxs-lookup"><span data-stu-id="deb6c-140">For example, `sudo pip install -r requirements.txt`.</span></span> <span data-ttu-id="deb6c-141">Další informace o instalaci balíčků Pythonu najdete v článku [Instalace balíčků][pypi_install] na webu python.org.</span><span class="sxs-lookup"><span data-stu-id="deb6c-141">For more information on installing Python packages, see [Installing Packages][pypi_install] on python.org.</span></span>
>
>

## <a name="batch-python-tutorial-code-sample"></a><span data-ttu-id="deb6c-142">Ukázka kódu Pythonu pro službu Batch</span><span class="sxs-lookup"><span data-stu-id="deb6c-142">Batch Python tutorial code sample</span></span>
<span data-ttu-id="deb6c-143">Ukázka kódu Pythonu Batch Hello se skládá ze dvou skriptů Pythonu a několika datových souborů.</span><span class="sxs-lookup"><span data-stu-id="deb6c-143">hello Batch Python tutorial code sample consists of two Python scripts and a few data files.</span></span>

* <span data-ttu-id="deb6c-144">**python_tutorial_client.PY**: komunikuje s tooexecute služby Batch a Storage hello paralelní úlohy na výpočetních uzlech (virtuálních počítačů).</span><span class="sxs-lookup"><span data-stu-id="deb6c-144">**python_tutorial_client.py**: Interacts with hello Batch and Storage services tooexecute a parallel workload on compute nodes (virtual machines).</span></span> <span data-ttu-id="deb6c-145">Hello *python_tutorial_client.py* skript se spustí na místní pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="deb6c-145">hello *python_tutorial_client.py* script runs on your local workstation.</span></span>
* <span data-ttu-id="deb6c-146">**python_tutorial_task.PY**: hello skript, který běží na výpočetních uzlech v Azure tooperform hello samotnou práci.</span><span class="sxs-lookup"><span data-stu-id="deb6c-146">**python_tutorial_task.py**: hello script that runs on compute nodes in Azure tooperform hello actual work.</span></span> <span data-ttu-id="deb6c-147">V ukázce hello *python_tutorial_task.py* hello analyzuje text v souboru staženém ze služby Azure Storage (vstupní soubor hello).</span><span class="sxs-lookup"><span data-stu-id="deb6c-147">In hello sample, *python_tutorial_task.py* parses hello text in a file downloaded from Azure Storage (hello input file).</span></span> <span data-ttu-id="deb6c-148">Pak se vytvoří textový soubor (hello výstupního souboru), obsahuje seznam hello první tři slova, která se zobrazují ve vstupním souboru hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-148">Then it produces a text file (hello output file) that contains a list of hello top three words that appear in hello input file.</span></span> <span data-ttu-id="deb6c-149">Po vytvoření výstupního souboru hello, *python_tutorial_task.py* nahrávání hello tooAzure soubor úložiště.</span><span class="sxs-lookup"><span data-stu-id="deb6c-149">After it creates hello output file, *python_tutorial_task.py* uploads hello file tooAzure Storage.</span></span> <span data-ttu-id="deb6c-150">Díky tomu je k dispozici ke stažení toohello klientského skriptu běží na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="deb6c-150">This makes it available for download toohello client script running on your workstation.</span></span> <span data-ttu-id="deb6c-151">Hello *python_tutorial_task.py* skript se spustí paralelně v několika výpočetních uzlech v hello služby Batch.</span><span class="sxs-lookup"><span data-stu-id="deb6c-151">hello *python_tutorial_task.py* script runs in parallel on multiple compute nodes in hello Batch service.</span></span>
* <span data-ttu-id="deb6c-152">**./data/taskdata\*.txt**: tyto tři textové soubory zajišťují vstup hello pro hello úlohy, které běží na hello výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="deb6c-152">**./data/taskdata\*.txt**: These three text files provide hello input for hello tasks that run on hello compute nodes.</span></span>

<span data-ttu-id="deb6c-153">Hello následující diagram znázorňuje primární operace hello, které provádí hello klientský skript a skript úkolu.</span><span class="sxs-lookup"><span data-stu-id="deb6c-153">hello following diagram illustrates hello primary operations that are performed by hello client and task scripts.</span></span> <span data-ttu-id="deb6c-154">Tento základní pracovní postup je typický pro mnoho výpočetních řešení, která jsou vytvořená pomocí služby Batch.</span><span class="sxs-lookup"><span data-stu-id="deb6c-154">This basic workflow is typical of many compute solutions that are created with Batch.</span></span> <span data-ttu-id="deb6c-155">Když nepředvádí všechny funkce, které jsou k dispozici v hello služby Batch, téměř každý scénář Batch zahrnuje části tohoto pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="deb6c-155">While it does not demonstrate every feature available in hello Batch service, nearly every Batch scenario includes portions of this workflow.</span></span>

<span data-ttu-id="deb6c-156">![Ukázkový pracovní postup služby Batch][8]</span><span class="sxs-lookup"><span data-stu-id="deb6c-156">![Batch example workflow][8]</span></span><br/>

[<span data-ttu-id="deb6c-157">**Krok 1.**</span><span class="sxs-lookup"><span data-stu-id="deb6c-157">**Step 1.**</span></span>](#step-1-create-storage-containers) <span data-ttu-id="deb6c-158">Ve službě Azure Blob Storage vytvořte **kontejnery** .</span><span class="sxs-lookup"><span data-stu-id="deb6c-158">Create **containers** in Azure Blob Storage.</span></span><br/><span data-ttu-id="deb6c-159">
[**Krok 2.**](#step-2-upload-task-script-and-data-files)</span><span class="sxs-lookup"><span data-stu-id="deb6c-159">
[**Step 2.**](#step-2-upload-task-script-and-data-files)</span></span> <span data-ttu-id="deb6c-160">Nahrajte toocontainers úloh skript a vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="deb6c-160">Upload task script and input files toocontainers.</span></span><br/><span data-ttu-id="deb6c-161">
[**Krok 3.**](#step-3-create-batch-pool)</span><span class="sxs-lookup"><span data-stu-id="deb6c-161">
[**Step 3.**](#step-3-create-batch-pool)</span></span> <span data-ttu-id="deb6c-162">Vytvořte **fond** Batch.</span><span class="sxs-lookup"><span data-stu-id="deb6c-162">Create a Batch **pool**.</span></span><br/>
  <span data-ttu-id="deb6c-163">&nbsp;&nbsp;&nbsp;&nbsp;**3a.**</span><span class="sxs-lookup"><span data-stu-id="deb6c-163">&nbsp;&nbsp;&nbsp;&nbsp;**3a.**</span></span> <span data-ttu-id="deb6c-164">Hello fondu **StartTask** stahování hello úlohy skriptu (python_tutorial_task.py) toonodes připojí hello fondu.</span><span class="sxs-lookup"><span data-stu-id="deb6c-164">hello pool **StartTask** downloads hello task script (python_tutorial_task.py) toonodes as they join hello pool.</span></span><br/><span data-ttu-id="deb6c-165">
[**Krok 4.**](#step-4-create-batch-job)</span><span class="sxs-lookup"><span data-stu-id="deb6c-165">
[**Step 4.**](#step-4-create-batch-job)</span></span> <span data-ttu-id="deb6c-166">Vytvořte **úlohu** Batch.</span><span class="sxs-lookup"><span data-stu-id="deb6c-166">Create a Batch **job**.</span></span><br/><span data-ttu-id="deb6c-167">
[**Krok 5.**](#step-5-add-tasks-to-job)</span><span class="sxs-lookup"><span data-stu-id="deb6c-167">
[**Step 5.**](#step-5-add-tasks-to-job)</span></span> <span data-ttu-id="deb6c-168">Přidat **úlohy** toohello úlohy.</span><span class="sxs-lookup"><span data-stu-id="deb6c-168">Add **tasks** toohello job.</span></span><br/>
  <span data-ttu-id="deb6c-169">&nbsp;&nbsp;&nbsp;&nbsp;**5a.**</span><span class="sxs-lookup"><span data-stu-id="deb6c-169">&nbsp;&nbsp;&nbsp;&nbsp;**5a.**</span></span> <span data-ttu-id="deb6c-170">Hello úkoly jsou naplánované tooexecute na uzlech.</span><span class="sxs-lookup"><span data-stu-id="deb6c-170">hello tasks are scheduled tooexecute on nodes.</span></span><br/>
    <span data-ttu-id="deb6c-171">&nbsp;&nbsp;&nbsp;&nbsp;**5b.**</span><span class="sxs-lookup"><span data-stu-id="deb6c-171">&nbsp;&nbsp;&nbsp;&nbsp;**5b.**</span></span> <span data-ttu-id="deb6c-172">Každý úkol stáhne svoje vstupní data ze služby Azure Storage a potom zahájí spuštění.</span><span class="sxs-lookup"><span data-stu-id="deb6c-172">Each task downloads its input data from Azure Storage, then begins execution.</span></span><br/><span data-ttu-id="deb6c-173">
[**Krok 6.**](#step-6-monitor-tasks)</span><span class="sxs-lookup"><span data-stu-id="deb6c-173">
[**Step 6.**](#step-6-monitor-tasks)</span></span> <span data-ttu-id="deb6c-174">Sledujte úkoly.</span><span class="sxs-lookup"><span data-stu-id="deb6c-174">Monitor tasks.</span></span><br/>
  <span data-ttu-id="deb6c-175">&nbsp;&nbsp;&nbsp;&nbsp;**6a.**</span><span class="sxs-lookup"><span data-stu-id="deb6c-175">&nbsp;&nbsp;&nbsp;&nbsp;**6a.**</span></span> <span data-ttu-id="deb6c-176">Jako úkoly při dokončení odesílají svoje výstupní data tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="deb6c-176">As tasks are completed, they upload their output data tooAzure Storage.</span></span><br/><span data-ttu-id="deb6c-177">
[**Krok 7.**](#step-7-download-task-output)</span><span class="sxs-lookup"><span data-stu-id="deb6c-177">
[**Step 7.**](#step-7-download-task-output)</span></span> <span data-ttu-id="deb6c-178">Stáhněte si výstup úkolu ze služby Storage.</span><span class="sxs-lookup"><span data-stu-id="deb6c-178">Download task output from Storage.</span></span>

<span data-ttu-id="deb6c-179">Jak jsme už zmínili, ne každé řešení Batch provede právě tyto kroky a může jich dokonce obsahovat mnohem víc, ale tato ukázka představuje procesy, které běžně bývají v řešení Batch.</span><span class="sxs-lookup"><span data-stu-id="deb6c-179">As mentioned, not every Batch solution performs these exact steps, and may include many more, but this sample demonstrates common processes found in a Batch solution.</span></span>

## <a name="prepare-client-script"></a><span data-ttu-id="deb6c-180">Příprava klientského skriptu</span><span class="sxs-lookup"><span data-stu-id="deb6c-180">Prepare client script</span></span>
<span data-ttu-id="deb6c-181">Než spustíte ukázkový text hello, přidat přihlašovací údaje účtu Batch a Storage příliš*python_tutorial_client.py*.</span><span class="sxs-lookup"><span data-stu-id="deb6c-181">Before you run hello sample, add your Batch and Storage account credentials too*python_tutorial_client.py*.</span></span> <span data-ttu-id="deb6c-182">Pokud jste tak ještě neučinili, otevřete soubor hello v vaše oblíbené hello editor a aktualizace následující řádky pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="deb6c-182">If you have not done so already, open hello file in your favorite editor and update hello following lines with your credentials.</span></span>

```python
# Update hello Batch and Storage account credential strings below with hello values
# unique tooyour accounts. These are used when constructing connection strings
# for hello Batch and Storage client objects.

# Batch account credentials
BATCH_ACCOUNT_NAME = ""
BATCH_ACCOUNT_KEY = ""
BATCH_ACCOUNT_URL = ""

# Storage account credentials
STORAGE_ACCOUNT_NAME = ""
STORAGE_ACCOUNT_KEY = ""
```

<span data-ttu-id="deb6c-183">Přihlašovací údaje účtu Batch a Storage v rámci hello okně účtu každé služby můžete najít v hello [portál Azure][azure_portal]:</span><span class="sxs-lookup"><span data-stu-id="deb6c-183">You can find your Batch and Storage account credentials within hello account blade of each service in hello [Azure portal][azure_portal]:</span></span>

<span data-ttu-id="deb6c-184">![Přihlašovací údaje hello portálu služby batch][9]
![přihlašovací údaje Storage na portálu hello][10]</span><span class="sxs-lookup"><span data-stu-id="deb6c-184">![Batch credentials in hello portal][9]
![Storage credentials in hello portal][10]</span></span><br/>

<span data-ttu-id="deb6c-185">V následujících částech hello, budeme analyzovat kroky hello používá hello skriptů tooprocess úloh ve hello služby Batch.</span><span class="sxs-lookup"><span data-stu-id="deb6c-185">In hello following sections, we analyze hello steps used by hello scripts tooprocess a workload in hello Batch service.</span></span> <span data-ttu-id="deb6c-186">Doporučujeme vám pravidelně toorefer toohello skriptů v editoru při práci procházení hello zbývající části článku hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-186">We encourage you toorefer regularly toohello scripts in your editor while you work your way through hello rest of hello article.</span></span>

<span data-ttu-id="deb6c-187">Přejděte toohello následující řádek ve **python_tutorial_client.py** toostart s krok 1:</span><span class="sxs-lookup"><span data-stu-id="deb6c-187">Navigate toohello following line in **python_tutorial_client.py** toostart with Step 1:</span></span>

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a><span data-ttu-id="deb6c-188">Krok 1: Vytvoření kontejnerů služby Storage</span><span class="sxs-lookup"><span data-stu-id="deb6c-188">Step 1: Create Storage containers</span></span>
<span data-ttu-id="deb6c-189">![Vytvoření kontejnerů ve službě Azure Storage][1]
</span><span class="sxs-lookup"><span data-stu-id="deb6c-189">![Create containers in Azure Storage][1]
</span></span><br/>

<span data-ttu-id="deb6c-190">Batch obsahuje vestavěnou podporu pro komunikaci se službou Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="deb6c-190">Batch includes built-in support for interacting with Azure Storage.</span></span> <span data-ttu-id="deb6c-191">Kontejnery v účtu úložiště bude poskytovat hello soubory potřebné hello úlohy, které běží v účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="deb6c-191">Containers in your Storage account will provide hello files needed by hello tasks that run in your Batch account.</span></span> <span data-ttu-id="deb6c-192">Hello kontejnery také poskytují místě toostore hello výstupní data, která hello úkoly vytvářejí.</span><span class="sxs-lookup"><span data-stu-id="deb6c-192">hello containers also provide a place toostore hello output data that hello tasks produce.</span></span> <span data-ttu-id="deb6c-193">Hello nejprve thing hello *python_tutorial_client.py* skript se vytvoří tři kontejnery ve [Azure Blob Storage](../storage/common/storage-introduction.md#blob-storage):</span><span class="sxs-lookup"><span data-stu-id="deb6c-193">hello first thing hello *python_tutorial_client.py* script does is create three containers in [Azure Blob Storage](../storage/common/storage-introduction.md#blob-storage):</span></span>

* <span data-ttu-id="deb6c-194">**aplikace**: Tento kontejner bude ukládat skript v jazyce Python hello spouštěné úkoly hello *python_tutorial_task.py*.</span><span class="sxs-lookup"><span data-stu-id="deb6c-194">**application**: This container will store hello Python script run by hello tasks, *python_tutorial_task.py*.</span></span>
* <span data-ttu-id="deb6c-195">**vstupní**: budou úkoly stahovat z hello hello datové soubory tooprocess *vstupní* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="deb6c-195">**input**: Tasks will download hello data files tooprocess from hello *input* container.</span></span>
* <span data-ttu-id="deb6c-196">**výstup**: když úkoly dokončí zpracování vstupního souboru, odešlou výsledky toohello hello *výstup* kontejneru.</span><span class="sxs-lookup"><span data-stu-id="deb6c-196">**output**: When tasks complete input file processing, they will upload hello results toohello *output* container.</span></span>

<span data-ttu-id="deb6c-197">V pořadí toointeract s úložiště účtu a vytvoření kontejnerů používáme hello [azure-storage] [ pypi_storage] balíček toocreate [BlockBlobService] [ py_blockblobservice] objekt – hello "klienta objektů blob."</span><span class="sxs-lookup"><span data-stu-id="deb6c-197">In order toointeract with a Storage account and create containers, we use hello [azure-storage][pypi_storage] package toocreate a [BlockBlobService][py_blockblobservice] object--hello "blob client."</span></span> <span data-ttu-id="deb6c-198">Poté vytvoříme tři kontejnery v účtu úložiště hello pomocí klienta objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-198">We then create three containers in hello Storage account using hello blob client.</span></span>

```python
import azure.storage.blob as azureblob

# Create hello blob client, for use in obtaining references to
# blob storage containers and uploading files toocontainers.
blob_client = azureblob.BlockBlobService(
    account_name=STORAGE_ACCOUNT_NAME,
    account_key=STORAGE_ACCOUNT_KEY)

# Use hello blob client toocreate hello containers in Azure Storage if they
# don't yet exist.
APP_CONTAINER_NAME = 'application'
INPUT_CONTAINER_NAME = 'input'
OUTPUT_CONTAINER_NAME = 'output'
blob_client.create_container(APP_CONTAINER_NAME, fail_on_exist=False)
blob_client.create_container(INPUT_CONTAINER_NAME, fail_on_exist=False)
blob_client.create_container(OUTPUT_CONTAINER_NAME, fail_on_exist=False)
```

<span data-ttu-id="deb6c-199">Po vytvoření kontejnerů hello hello aplikaci teď můžete nahrát hello soubory, které se použijí tak, že úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-199">Once hello containers have been created, hello application can now upload hello files that will be used by hello tasks.</span></span>

> [!TIP]
> <span data-ttu-id="deb6c-200">[Jak toouse Azure Blob storage z Pythonu](../storage/blobs/storage-python-how-to-use-blob-storage.md) nabízí pěkný přehled o práci s Azure Storage kontejnery a objekty BLOB ve službě.</span><span class="sxs-lookup"><span data-stu-id="deb6c-200">[How toouse Azure Blob storage from Python](../storage/blobs/storage-python-how-to-use-blob-storage.md) provides a good overview of working with Azure Storage containers and blobs.</span></span> <span data-ttu-id="deb6c-201">Musí být v hello horní části seznamu čtení jako začátek práce s Batch.</span><span class="sxs-lookup"><span data-stu-id="deb6c-201">It should be near hello top of your reading list as you start working with Batch.</span></span>
>
>

## <a name="step-2-upload-task-script-and-data-files"></a><span data-ttu-id="deb6c-202">Krok 2: Odeslání skriptu úkolu a datových souborů</span><span class="sxs-lookup"><span data-stu-id="deb6c-202">Step 2: Upload task script and data files</span></span>
<span data-ttu-id="deb6c-203">![Soubory toocontainers odeslání úloh aplikace a vstupních (data)][2]
</span><span class="sxs-lookup"><span data-stu-id="deb6c-203">![Upload task application and input (data) files toocontainers][2]
</span></span><br/>

<span data-ttu-id="deb6c-204">V souboru hello operace nahrávání *python_tutorial_client.py* nejdřív definuje kolekce **aplikace** a **vstupní** cesty k souborům, které jsou v místním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-204">In hello file upload operation, *python_tutorial_client.py* first defines collections of **application** and **input** file paths as they exist on hello local machine.</span></span> <span data-ttu-id="deb6c-205">Pak odešle tyto soubory toohello kontejnery, které jste vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-205">Then it uploads these files toohello containers that you created in hello previous step.</span></span>

```python
# Paths toohello task script. This script will be executed by hello tasks that
# run on hello compute nodes.
application_file_paths = [os.path.realpath('python_tutorial_task.py')]

# hello collection of data files that are toobe processed by hello tasks.
input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                    os.path.realpath('./data/taskdata2.txt'),
                    os.path.realpath('./data/taskdata3.txt')]

# Upload hello application script tooAzure Storage. This is hello script that
# will process hello data files, and is executed by each of hello tasks on the
# compute nodes.
application_files = [
    upload_file_to_container(blob_client, APP_CONTAINER_NAME, file_path)
    for file_path in application_file_paths]

# Upload hello data files. This is hello data that will be processed by each of
# hello tasks executed on hello compute nodes in hello pool.
input_files = [
    upload_file_to_container(blob_client, INPUT_CONTAINER_NAME, file_path)
    for file_path in input_file_paths]
```

<span data-ttu-id="deb6c-206">Pomocí seznamu, hello `upload_file_to_container` funkce je volána pro každý soubor v kolekcích hello a dva [ResourceFile] [ py_resource_file] se vyplní kolekce.</span><span class="sxs-lookup"><span data-stu-id="deb6c-206">Using list comprehension, hello `upload_file_to_container` function is called for each file in hello collections, and two [ResourceFile][py_resource_file] collections are populated.</span></span> <span data-ttu-id="deb6c-207">Hello `upload_file_to_container` funkce se zobrazí níže:</span><span class="sxs-lookup"><span data-stu-id="deb6c-207">hello `upload_file_to_container` function appears below:</span></span>

```python
def upload_file_to_container(block_blob_client, container_name, path):
    """
    Uploads a local file tooan Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: hello name of hello Azure Blob storage container.
    :param str file_path: hello local path toohello file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """

    import datetime
    import azure.storage.blob as azureblob
    import azure.batch.models as batchmodels

    blob_name = os.path.basename(path)

    print('Uploading file {} toocontainer [{}]...'.format(path,
                                                          container_name))

    block_blob_client.create_blob_from_path(container_name,
                                            blob_name,
                                            file_path)

    sas_token = block_blob_client.generate_blob_shared_access_signature(
        container_name,
        blob_name,
        permission=azureblob.BlobPermissions.READ,
        expiry=datetime.datetime.utcnow() + datetime.timedelta(hours=2))

    sas_url = block_blob_client.make_blob_url(container_name,
                                              blob_name,
                                              sas_token=sas_token)

    return batchmodels.ResourceFile(file_path=blob_name,
                                    blob_source=sas_url)
```

### <a name="resourcefiles"></a><span data-ttu-id="deb6c-208">ResourceFiles</span><span class="sxs-lookup"><span data-stu-id="deb6c-208">ResourceFiles</span></span>
<span data-ttu-id="deb6c-209">A [ResourceFile] [ py_resource_file] poskytuje úlohy v dávce s soubor tooa hello adresy URL ve službě Azure Storage, který je stažený tooa výpočetního uzlu před spuštěním této úlohy.</span><span class="sxs-lookup"><span data-stu-id="deb6c-209">A [ResourceFile][py_resource_file] provides tasks in Batch with hello URL tooa file in Azure Storage that is downloaded tooa compute node before that task is run.</span></span> <span data-ttu-id="deb6c-210">Hello [ResourceFile][py_resource_file]. **blob_source** vlastnost určuje úplnou adresu URL hello hello souboru, protože existuje ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="deb6c-210">hello [ResourceFile][py_resource_file].**blob_source** property specifies hello full URL of hello file as it exists in Azure Storage.</span></span> <span data-ttu-id="deb6c-211">Adresa URL Hello mohou obsahovat také sdílený přístupový podpis (SAS), který poskytuje soubor toohello zabezpečený přístup.</span><span class="sxs-lookup"><span data-stu-id="deb6c-211">hello URL may also include a shared access signature (SAS) that provides secure access toohello file.</span></span> <span data-ttu-id="deb6c-212">Většina typů úkolů ve službě Batch obsahuje vlastnost *ResourceFiles* včetně:</span><span class="sxs-lookup"><span data-stu-id="deb6c-212">Most task types in Batch include a *ResourceFiles* property, including:</span></span>

* <span data-ttu-id="deb6c-213">[CloudTask][py_task]</span><span class="sxs-lookup"><span data-stu-id="deb6c-213">[CloudTask][py_task]</span></span>
* <span data-ttu-id="deb6c-214">[StartTask][py_starttask]</span><span class="sxs-lookup"><span data-stu-id="deb6c-214">[StartTask][py_starttask]</span></span>
* <span data-ttu-id="deb6c-215">[JobPreparationTask][py_jobpreptask]</span><span class="sxs-lookup"><span data-stu-id="deb6c-215">[JobPreparationTask][py_jobpreptask]</span></span>
* <span data-ttu-id="deb6c-216">[JobReleaseTask][py_jobreltask]</span><span class="sxs-lookup"><span data-stu-id="deb6c-216">[JobReleaseTask][py_jobreltask]</span></span>

<span data-ttu-id="deb6c-217">Tato ukázka nepoužívá hello JobPreparationTask nebo JobReleaseTask typy úloh, ale si můžete přečíst více o nich [spustit přípravy a dokončení úlohy na Azure Batch výpočetních uzlech](batch-job-prep-release.md).</span><span class="sxs-lookup"><span data-stu-id="deb6c-217">This sample does not use hello JobPreparationTask or JobReleaseTask task types, but you can read more about them in [Run job preparation and completion tasks on Azure Batch compute nodes](batch-job-prep-release.md).</span></span>

### <a name="shared-access-signature-sas"></a><span data-ttu-id="deb6c-218">Sdílený přístupový podpis (SAS)</span><span class="sxs-lookup"><span data-stu-id="deb6c-218">Shared access signature (SAS)</span></span>
<span data-ttu-id="deb6c-219">Sdílené přístupové podpisy jsou řetězce, které zajišťují zabezpečený přístup toocontainers a objekty BLOB ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="deb6c-219">Shared access signatures are strings that provide secure access toocontainers and blobs in Azure Storage.</span></span> <span data-ttu-id="deb6c-220">Hello *python_tutorial_client.py* skript používá objektu blob i a sdílené přístupové podpisy kontejneru a ukazuje, jak tyto sdílené tooobtain přistupovat k řetězce podpisu ze hello služby úložiště.</span><span class="sxs-lookup"><span data-stu-id="deb6c-220">hello *python_tutorial_client.py* script uses both blob and container shared access signatures, and demonstrates how tooobtain these shared access signature strings from hello Storage service.</span></span>

* <span data-ttu-id="deb6c-221">**Sdílené přístupové podpisy objektů blob**: StartTask fondu hello používá sdílené přístupové podpisy objektů blob při stahování hello skriptu úkolu a vstupní data souborů ze služby Storage (viz [krok 3](#step-3-create-batch-pool) níže).</span><span class="sxs-lookup"><span data-stu-id="deb6c-221">**Blob shared access signatures**: hello pool's StartTask uses blob shared access signatures when it downloads hello task script and input data files from Storage (see [Step #3](#step-3-create-batch-pool) below).</span></span> <span data-ttu-id="deb6c-222">Hello `upload_file_to_container` fungovat v *python_tutorial_client.py* obsahuje hello kód, který získá sdílený přístupový podpis jednotlivých objektů blob.</span><span class="sxs-lookup"><span data-stu-id="deb6c-222">hello `upload_file_to_container` function in *python_tutorial_client.py* contains hello code that obtains each blob's shared access signature.</span></span> <span data-ttu-id="deb6c-223">Dělá to tak, že zavoláte [BlockBlobService.make_blob_url] [ py_make_blob_url] v modulu hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="deb6c-223">It does so by calling [BlockBlobService.make_blob_url][py_make_blob_url] in hello Storage module.</span></span>
* <span data-ttu-id="deb6c-224">**Sdílený přístupový podpis kontejneru**: když každý úkol dokončí svojí práci ve výpočetním uzlu hello, odešle svůj výstupní soubor toohello *výstup* kontejneru ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="deb6c-224">**Container shared access signature**: As each task finishes its work on hello compute node, it uploads its output file toohello *output* container in Azure Storage.</span></span> <span data-ttu-id="deb6c-225">toodo, *python_tutorial_task.py* používá sdílený přístupový podpis kontejneru, který poskytuje přístup pro zápis toohello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="deb6c-225">toodo so, *python_tutorial_task.py* uses a container shared access signature that provides write access toohello container.</span></span> <span data-ttu-id="deb6c-226">Hello `get_container_sas_token` fungovat v *python_tutorial_client.py* získá sdílený přístupový podpis kontejneru hello, který se potom předá jako toohello úlohy argument příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="deb6c-226">hello `get_container_sas_token` function in *python_tutorial_client.py* obtains hello container's shared access signature, which is then passed as a command-line argument toohello tasks.</span></span> <span data-ttu-id="deb6c-227">Krok #5 [přidat úlohy tooa úlohy](#step-5-add-tasks-to-job), popisuje použití hello hello kontejneru SAS.</span><span class="sxs-lookup"><span data-stu-id="deb6c-227">Step #5, [Add tasks tooa job](#step-5-add-tasks-to-job), discusses hello usage of hello container SAS.</span></span>

> [!TIP]
> <span data-ttu-id="deb6c-228">Podívejte se na dvě části řady hello na sdílených přístupových podpisů [část 1: vysvětlení modelu SAS hello](../storage/common/storage-dotnet-shared-access-signature-part-1.md) a [část 2: vytvoření a použití SAS s hello služby objektů Blob](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), toolearn informace o zajišťování bezpečného přístupu toodata ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="deb6c-228">Check out hello two-part series on shared access signatures, [Part 1: Understanding hello SAS model](../storage/common/storage-dotnet-shared-access-signature-part-1.md) and [Part 2: Create and use a SAS with hello Blob service](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), toolearn more about providing secure access toodata in your Storage account.</span></span>
>
>

## <a name="step-3-create-batch-pool"></a><span data-ttu-id="deb6c-229">Krok 3: Vytvoření fondu služby Batch</span><span class="sxs-lookup"><span data-stu-id="deb6c-229">Step 3: Create Batch pool</span></span>
<span data-ttu-id="deb6c-230">![Vytvoření fondu Batch][3]
</span><span class="sxs-lookup"><span data-stu-id="deb6c-230">![Create a Batch pool][3]
</span></span><br/>

<span data-ttu-id="deb6c-231">**Fond** Batch je kolekce výpočetních uzlů (virtuálních počítačů), na kterých služba Batch provádí úkoly z úlohy.</span><span class="sxs-lookup"><span data-stu-id="deb6c-231">A Batch **pool** is a collection of compute nodes (virtual machines) on which Batch executes a job's tasks.</span></span>

<span data-ttu-id="deb6c-232">Po odeslání hello úlohy skriptu a data souborů toohello účet úložiště, *python_tutorial_client.py* spustí jeho komunikaci se službou Batch hello pomocí modulu Batch Python hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-232">After it uploads hello task script and data files toohello Storage account, *python_tutorial_client.py* starts its interaction with hello Batch service by using hello Batch Python module.</span></span> <span data-ttu-id="deb6c-233">toodo, [BatchServiceClient] [ py_batchserviceclient] je vytvořena:</span><span class="sxs-lookup"><span data-stu-id="deb6c-233">toodo so, a [BatchServiceClient][py_batchserviceclient] is created:</span></span>

```python
# Create a Batch service client. We'll now be interacting with hello Batch
# service in addition tooStorage.
credentials = batchauth.SharedKeyCredentials(BATCH_ACCOUNT_NAME,
                                             BATCH_ACCOUNT_KEY)

batch_client = batch.BatchServiceClient(
    credentials,
    base_url=BATCH_ACCOUNT_URL)
```

<span data-ttu-id="deb6c-234">Dále fond výpočetních uzlů je vytvořen v hello účtu Batch pomocí volání příliš`create_pool`.</span><span class="sxs-lookup"><span data-stu-id="deb6c-234">Next, a pool of compute nodes is created in hello Batch account with a call too`create_pool`.</span></span>

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with hello specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for hello new pool.
    :param list resource_files: A collection of resource files for hello pool's
    start task.
    :param str publisher: Marketplace image publisher
    :param str offer: Marketplace image offer
    :param str sku: Marketplace image sku
    """
    print('Creating pool [{}]...'.format(pool_id))

    # Create a new pool of Linux compute nodes using an Azure Virtual Machines
    # Marketplace image. For more information about creating pools of Linux
    # nodes, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/

    # Specify hello commands for hello pool's start task. hello start task is run
    # on each node as it joins hello pool, and when it's rebooted or re-imaged.
    # We use hello start task tooprep hello node for running our task script.
    task_commands = [
        # Copy hello python_tutorial_task.py script toohello "shared" directory
        # that all tasks that run on hello node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and hello dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install hello azure-storage module so that hello task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get hello node agent SKU and image reference for hello virtual machine
    # configuration.
    # For more information about hello virtual machine configuration, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/
    sku_to_use, image_ref_to_use = \
        common.helpers.select_latest_verified_vm_image_with_node_agent_sku(
            batch_service_client, publisher, offer, sku)

    new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            node_agent_sku_id=sku_to_use),
        vm_size=_POOL_VM_SIZE,
        target_dedicated=_POOL_NODE_COUNT,
        start_task=batch.models.StartTask(
            command_line=
            common.helpers.wrap_commands_in_shell('linux', task_commands),
            run_elevated=True,
            wait_for_success=True,
            resource_files=resource_files),
        )

    try:
        batch_service_client.pool.add(new_pool)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

<span data-ttu-id="deb6c-235">Při vytváření fondu můžete definovat [PoolAddParameter] [ py_pooladdparam] který určuje několik vlastností fondu hello:</span><span class="sxs-lookup"><span data-stu-id="deb6c-235">When you create a pool, you define a [PoolAddParameter][py_pooladdparam] that specifies several properties for hello pool:</span></span>

* <span data-ttu-id="deb6c-236">**ID** hello fondu (*id* – povinné)</span><span class="sxs-lookup"><span data-stu-id="deb6c-236">**ID** of hello pool (*id* - required)</span></span><p/><span data-ttu-id="deb6c-237">Stejně jako u většiny entit ve službě Batch musí mít nový fond v rámci účtu Batch jedinečné ID.</span><span class="sxs-lookup"><span data-stu-id="deb6c-237">As with most entities in Batch, your new pool must have a unique ID within your Batch account.</span></span> <span data-ttu-id="deb6c-238">Váš kód odkazoval toothis fond pomocí jeho ID, a je identifikaci fondu hello v hello Azure [portál][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="deb6c-238">Your code refers toothis pool using its ID, and it's how you identify hello pool in hello Azure [portal][azure_portal].</span></span>
* <span data-ttu-id="deb6c-239">**Počet výpočetních uzlů** (*target_dedicated* – povinné)</span><span class="sxs-lookup"><span data-stu-id="deb6c-239">**Number of compute nodes** (*target_dedicated* - required)</span></span><p/><span data-ttu-id="deb6c-240">Tato vlastnost určuje, kolik virtuálních počítačů musí být nasazené ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-240">This property specifies how many VMs should be deployed in hello pool.</span></span> <span data-ttu-id="deb6c-241">Je důležité toonote, aby všechny účty Batch mají výchozí **kvóty** , limity hello počet **jader** (a tedy výpočetních uzlů) v účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="deb6c-241">It is important toonote that all Batch accounts have a default **quota** that limits hello number of **cores** (and thus, compute nodes) in a Batch account.</span></span> <span data-ttu-id="deb6c-242">Můžete najít hello výchozí kvóty a pokyny o tom, příliš[navýšení kvóty](batch-quota-limit.md#increase-a-quota) (například hello maximální počet jader na účtu Batch) v [kvóty a omezení pro hello služby Azure Batch](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="deb6c-242">You can find hello default quotas and instructions on how too[increase a quota](batch-quota-limit.md#increase-a-quota) (such as hello maximum number of cores in your Batch account) in [Quotas and limits for hello Azure Batch service](batch-quota-limit.md).</span></span> <span data-ttu-id="deb6c-243">Možná vás někdy napadne otázka, proč váš fond nedosahuje víc než X uzlů.</span><span class="sxs-lookup"><span data-stu-id="deb6c-243">If you find yourself asking "Why won't my pool reach more than X nodes?"</span></span> <span data-ttu-id="deb6c-244">hello příčinou může být tato kvóta na jádra.</span><span class="sxs-lookup"><span data-stu-id="deb6c-244">this core quota may be hello cause.</span></span>
* <span data-ttu-id="deb6c-245">**Operační systém** uzlů (*virtual_machine_configuration***nebo***cloud_service_configuration* – povinné)</span><span class="sxs-lookup"><span data-stu-id="deb6c-245">**Operating system** for nodes (*virtual_machine_configuration* **or** *cloud_service_configuration* - required)</span></span><p/><span data-ttu-id="deb6c-246">Ve skriptu *python_tutorial_client.py* vytvoříme fond linuxových uzlů pomocí [VirtualMachineConfiguration][py_vm_config].</span><span class="sxs-lookup"><span data-stu-id="deb6c-246">In *python_tutorial_client.py*, we create a pool of Linux nodes using a [VirtualMachineConfiguration][py_vm_config].</span></span> <span data-ttu-id="deb6c-247">Hello `select_latest_verified_vm_image_with_node_agent_sku` fungovat v `common.helpers` usnadňuje práci s [Marketplace virtuálních počítačů Azure] [ vm_marketplace] bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="deb6c-247">hello `select_latest_verified_vm_image_with_node_agent_sku` function in `common.helpers` simplifies working with [Azure Virtual Machines Marketplace][vm_marketplace] images.</span></span> <span data-ttu-id="deb6c-248">Další informace o používání imagí z Marketplace najdete v tématu [Zřízení linuxových výpočetních uzlů ve fondech Azure Batch](batch-linux-nodes.md).</span><span class="sxs-lookup"><span data-stu-id="deb6c-248">See [Provision Linux compute nodes in Azure Batch pools](batch-linux-nodes.md) for more information about using Marketplace images.</span></span>
* <span data-ttu-id="deb6c-249">**Velikost výpočetních uzlů** (*vm_size* – povinné)</span><span class="sxs-lookup"><span data-stu-id="deb6c-249">**Size of compute nodes** (*vm_size* - required)</span></span><p/><span data-ttu-id="deb6c-250">Vzhledem k tomu, že zadáváme linuxové uzly pro naší [VirtualMachineConfiguration][py_vm_config], zadáme velikost virtuálního počítače (v této ukázce `STANDARD_A1`) podle článku [Velikosti virtuálních počítačů v Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="deb6c-250">Since we're specifying Linux nodes for our [VirtualMachineConfiguration][py_vm_config], we specify a VM size (`STANDARD_A1` in this sample) from [Sizes for virtual machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="deb6c-251">Další informace opět najdete v článku [Zřízení linuxových výpočetních uzlů ve fondech Azure Batch](batch-linux-nodes.md) </span><span class="sxs-lookup"><span data-stu-id="deb6c-251">Again, see [Provision Linux compute nodes in Azure Batch pools](batch-linux-nodes.md) for more information.</span></span>
* <span data-ttu-id="deb6c-252">**Spustit úkol** (*start_task* – nepovinné)</span><span class="sxs-lookup"><span data-stu-id="deb6c-252">**Start task** (*start_task* - not required)</span></span><p/><span data-ttu-id="deb6c-253">Společně s hello výše fyzickými vlastnostmi uzlu můžete určit také [StartTask] [ py_starttask] hello fondu (není nutné).</span><span class="sxs-lookup"><span data-stu-id="deb6c-253">Along with hello above physical node properties, you may also specify a [StartTask][py_starttask] for hello pool (it is not required).</span></span> <span data-ttu-id="deb6c-254">Hello StartTask se spustí na každém uzlu jako takový uzel připojí hello fondu a pokaždé, když je uzel restartován.</span><span class="sxs-lookup"><span data-stu-id="deb6c-254">hello StartTask executes on each node as that node joins hello pool, and each time a node is restarted.</span></span> <span data-ttu-id="deb6c-255">Hello StartTask je zvláště užitečná pro přípravu výpočetních uzlů k provádění hello úkolů, například při instalaci aplikace hello, které běží vaše úkoly.</span><span class="sxs-lookup"><span data-stu-id="deb6c-255">hello StartTask is especially useful for preparing compute nodes for hello execution of tasks, such as installing hello applications that your tasks run.</span></span><p/><span data-ttu-id="deb6c-256">V této ukázkové aplikaci hello StartTask zkopíruje hello soubory, které stáhne ze služby Storage (které jsou určené pomocí hello StartTask **resource_files** vlastnost) z hello StartTask *pracovní adresář* toohello *sdílené* adresář, kterému mají přístup všechny úkoly spuštěné na uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-256">In this sample application, hello StartTask copies hello files that it downloads from Storage (which are specified by using hello StartTask's **resource_files** property) from hello StartTask *working directory* toohello *shared* directory that all tasks running on hello node can access.</span></span> <span data-ttu-id="deb6c-257">V podstatě zkopíruje `python_tutorial_task.py` toohello sdílet adresáře v každém uzlu hello uzel připojí hello fondu, takže všechny úlohy, které běží na uzlu hello k němu přístup.</span><span class="sxs-lookup"><span data-stu-id="deb6c-257">Essentially, this copies `python_tutorial_task.py` toohello shared directory on each node as hello node joins hello pool, so that any tasks that run on hello node can access it.</span></span>

<span data-ttu-id="deb6c-258">Můžete si povšimnout volání toohello hello `wrap_commands_in_shell` pomocné funkce.</span><span class="sxs-lookup"><span data-stu-id="deb6c-258">You may notice hello call toohello `wrap_commands_in_shell` helper function.</span></span> <span data-ttu-id="deb6c-259">Tato funkce vezme kolekci samostatných příkazů a vytvoří jeden příkazový řádek, který odpovídá vlastnosti příkazového řádku úkolu.</span><span class="sxs-lookup"><span data-stu-id="deb6c-259">This function takes a collection of separate commands and creates a single command line appropriate for a task's command-line property.</span></span>

<span data-ttu-id="deb6c-260">Také je zajímavé ve fragmentu kódu hello výše hello použití dvou proměnných prostředí v hello **command_line** vlastnost hello StartTask: `AZ_BATCH_TASK_WORKING_DIR` a `AZ_BATCH_NODE_SHARED_DIR`.</span><span class="sxs-lookup"><span data-stu-id="deb6c-260">Also notable in hello code snippet above is hello use of two environment variables in hello **command_line** property of hello StartTask: `AZ_BATCH_TASK_WORKING_DIR` and `AZ_BATCH_NODE_SHARED_DIR`.</span></span> <span data-ttu-id="deb6c-261">Každý výpočetní uzel v rámci fondu Batch je automaticky nakonfigurovaný pomocí řady proměnných prostředí, které jsou specifické tooBatch.</span><span class="sxs-lookup"><span data-stu-id="deb6c-261">Each compute node within a Batch pool is automatically configured with several environment variables that are specific tooBatch.</span></span> <span data-ttu-id="deb6c-262">Jakýkoli proces spuštěný úkolem má přístup k proměnným prostředí toothese.</span><span class="sxs-lookup"><span data-stu-id="deb6c-262">Any process that is executed by a task has access toothese environment variables.</span></span>

> [!TIP]
> <span data-ttu-id="deb6c-263">toofind Další informace o hello proměnné prostředí, které jsou dostupné na výpočetní uzlech ve fondu Batch, také a informace o pracovních adresářích úkolu najdete v části **nastavení prostředí pro úkoly** a **souborů a adresářů**  v hello [přehled funkcí Azure Batch](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="deb6c-263">toofind out more about hello environment variables that are available on compute nodes in a Batch pool, as well as information on task working directories, see **Environment settings for tasks** and **Files and directories** in hello [overview of Azure Batch features](batch-api-basics.md).</span></span>
>
>

## <a name="step-4-create-batch-job"></a><span data-ttu-id="deb6c-264">Krok 4: Vytvoření úlohy Batch</span><span class="sxs-lookup"><span data-stu-id="deb6c-264">Step 4: Create Batch job</span></span>
<span data-ttu-id="deb6c-265">![Vytvoření úlohy Batch][4]</span><span class="sxs-lookup"><span data-stu-id="deb6c-265">![Create Batch job][4]</span></span><br/>

<span data-ttu-id="deb6c-266">**Úloha** Batch je kolekcí úkolů a je přidružená k fondu výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="deb6c-266">A Batch **job** is a collection of tasks, and is associated with a pool of compute nodes.</span></span> <span data-ttu-id="deb6c-267">Hello úlohy pro úlohu provést hello přidružené fondu výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="deb6c-267">hello tasks in a job execute on hello associated pool's compute nodes.</span></span>

<span data-ttu-id="deb6c-268">Úlohu můžete použít nejen k uspořádání a sledování úkolů v souvisejících úlohách, ale také pro nastavení určitých omezení – například maximálního runtime hello hello úlohy (a rozšíření, její úkoly) a priority úloh ve vztahu tooother úloh v hello účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="deb6c-268">You can use a job not only for organizing and tracking tasks in related workloads, but also for imposing certain constraints--such as hello maximum runtime for hello job (and by extension, its tasks) and job priority in relation tooother jobs in hello Batch account.</span></span> <span data-ttu-id="deb6c-269">V tomto příkladu však hello úlohy je přidruženo pouze hello fondu, který byl vytvořen v kroku #3.</span><span class="sxs-lookup"><span data-stu-id="deb6c-269">In this example, however, hello job is associated only with hello pool that was created in step #3.</span></span> <span data-ttu-id="deb6c-270">Žádné další vlastnosti se nekonfigurují.</span><span class="sxs-lookup"><span data-stu-id="deb6c-270">No additional properties are configured.</span></span>

<span data-ttu-id="deb6c-271">Všechny úlohy Batch jsou přidružené ke konkrétnímu fondu.</span><span class="sxs-lookup"><span data-stu-id="deb6c-271">All Batch jobs are associated with a specific pool.</span></span> <span data-ttu-id="deb6c-272">Toto přidružení označuje uzlů, které hello úlohy provést.</span><span class="sxs-lookup"><span data-stu-id="deb6c-272">This association indicates which nodes hello job's tasks execute on.</span></span> <span data-ttu-id="deb6c-273">Zadejte fond hello pomocí hello [PoolInformation] [ py_poolinfo] vlastnost, jak je znázorněno v následující fragment kódu hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-273">You specify hello pool by using hello [PoolInformation][py_poolinfo] property, as shown in hello code snippet below.</span></span>

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with hello specified ID, associated with hello specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: hello ID for hello job.
    :param str pool_id: hello ID for hello pool.
    """
    print('Creating job [{}]...'.format(job_id))

    job = batch.models.JobAddParameter(
        job_id,
        batch.models.PoolInformation(pool_id=pool_id))

    try:
        batch_service_client.job.add(job)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

<span data-ttu-id="deb6c-274">Teď, když vytvořil úlohu úlohy se přidají pracovní tooperform hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-274">Now that a job has been created, tasks are added tooperform hello work.</span></span>

## <a name="step-5-add-tasks-toojob"></a><span data-ttu-id="deb6c-275">Krok 5: Přidejte toojob úlohy</span><span class="sxs-lookup"><span data-stu-id="deb6c-275">Step 5: Add tasks toojob</span></span>
<span data-ttu-id="deb6c-276">![Přidat toojob úlohy][5]</span><span class="sxs-lookup"><span data-stu-id="deb6c-276">![Add tasks toojob][5]</span></span><br/><span data-ttu-id="deb6c-277">
*(1) úkoly jsou přidány toohello úlohy, hello (2) úkoly jsou naplánované toorun na uzlech a hello (3) úkoly stahují hello data souborů tooprocess*</span><span class="sxs-lookup"><span data-stu-id="deb6c-277">
*(1) Tasks are added toohello job, (2) hello tasks are scheduled toorun on nodes, and (3) hello tasks download hello data files tooprocess*</span></span>

<span data-ttu-id="deb6c-278">Batch **úlohy** jsou hello jednotlivé jednotky práce, které jsou spouštěny na hello výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="deb6c-278">Batch **tasks** are hello individual units of work that execute on hello compute nodes.</span></span> <span data-ttu-id="deb6c-279">Úkol má příkazový řádek a spouští hello skripty nebo spustitelné soubory, které zadáte v takovém příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="deb6c-279">A task has a command line and runs hello scripts or executables that you specify in that command line.</span></span>

<span data-ttu-id="deb6c-280">tooactually práci, musí úkoly nejprve přidat úloha tooa.</span><span class="sxs-lookup"><span data-stu-id="deb6c-280">tooactually perform work, tasks must be added tooa job.</span></span> <span data-ttu-id="deb6c-281">Každý [CloudTask] [ py_task] nastavena vlastnost příkazového řádku a [ResourceFiles] [ py_resource_file] (stejně jako u StartTask fondu hello) této hello úkol stáhne toohello uzlu předtím, než se jeho příkazový řádek automaticky spustí.</span><span class="sxs-lookup"><span data-stu-id="deb6c-281">Each [CloudTask][py_task] is configured with a command-line property and [ResourceFiles][py_resource_file] (as with hello pool's StartTask) that hello task downloads toohello node before its command line is automatically executed.</span></span> <span data-ttu-id="deb6c-282">Každý úkol v ukázce hello zpracovává jenom jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="deb6c-282">In hello sample, each task processes only one file.</span></span> <span data-ttu-id="deb6c-283">Proto jeho kolekce ResourceFiles obsahuje jen jeden prvek.</span><span class="sxs-lookup"><span data-stu-id="deb6c-283">Thus, its ResourceFiles collection contains a single element.</span></span>

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in hello collection toohello specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: hello ID of hello job toowhich tooadd hello tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: hello ID of an Azure Blob storage container to
    which hello tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    hello specified Azure Blob storage container.
    """

    print('Adding {} tasks toojob [{}]...'.format(len(input_files), job_id))

    tasks = list()

    for input_file in input_files:

        command = ['python $AZ_BATCH_NODE_SHARED_DIR/python_tutorial_task.py '
                   '--filepath {} --numwords {} --storageaccount {} '
                   '--storagecontainer {} --sastoken "{}"'.format(
                    input_file.file_path,
                    '3',
                    _STORAGE_ACCOUNT_NAME,
                    output_container_name,
                    output_container_sas_token)]

        tasks.append(batch.models.TaskAddParameter(
                'topNtask{}'.format(input_files.index(input_file)),
                wrap_commands_in_shell('linux', command),
                resource_files=[input_file]
                )
        )

    batch_service_client.task.add_collection(job_id, tasks)
```

> [!IMPORTANT]
> <span data-ttu-id="deb6c-284">Když přistoupí k proměnným prostředí, jako například `$AZ_BATCH_NODE_SHARED_DIR` nebo spuštění aplikace nebyla nalezena v uzlu hello `PATH`, musí příkazové řádky úkolu vyvolání hello skořápce explicitně, jako třeba s `/bin/sh -c MyTaskApplication $MY_ENV_VAR`.</span><span class="sxs-lookup"><span data-stu-id="deb6c-284">When they access environment variables such as `$AZ_BATCH_NODE_SHARED_DIR` or execute an application not found in hello node's `PATH`, task command lines must invoke hello shell explicitly, such as with `/bin/sh -c MyTaskApplication $MY_ENV_VAR`.</span></span> <span data-ttu-id="deb6c-285">Tento požadavek není nutný, pokud vaše úkoly spouští aplikace v uzlu hello `PATH` a neodkazují žádné proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="deb6c-285">This requirement is unnecessary if your tasks execute an application in hello node's `PATH` and do not reference any environment variables.</span></span>
>
>

<span data-ttu-id="deb6c-286">V rámci hello `for` ve smyčce ve fragmentu kódu hello výše, uvidíte, že hello příkazového řádku pro úlohu hello je vytvořený pomocí pěti argumentů příkazového řádku, které se předávají příliš*python_tutorial_task.py*:</span><span class="sxs-lookup"><span data-stu-id="deb6c-286">Within hello `for` loop in hello code snippet above, you can see that hello command line for hello task is constructed with five command-line arguments that are passed too*python_tutorial_task.py*:</span></span>

1. <span data-ttu-id="deb6c-287">**FilePath**: Toto je soubor toohello hello místní cestu, protože existuje na uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-287">**filepath**: This is hello local path toohello file as it exists on hello node.</span></span> <span data-ttu-id="deb6c-288">Když hello objekt ResourceFile v `upload_file_to_container` byl vytvořen v kroku 2 výše, název souboru hello použitou pro tuto vlastnost (hello `file_path` parametr v konstruktoru ResourceFile hello).</span><span class="sxs-lookup"><span data-stu-id="deb6c-288">When hello ResourceFile object in `upload_file_to_container` was created in Step 2 above, hello file name was used for this property (hello `file_path` parameter in hello ResourceFile constructor).</span></span> <span data-ttu-id="deb6c-289">To znamená, že hello soubor najdete v hello stejné adresáře na uzlu hello jako *python_tutorial_task.py*.</span><span class="sxs-lookup"><span data-stu-id="deb6c-289">This indicates that hello file can be found in hello same directory on hello node as *python_tutorial_task.py*.</span></span>
2. <span data-ttu-id="deb6c-290">**NUMWORDS**: hello horní *N* slova zasílány toohello výstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="deb6c-290">**numwords**: hello top *N* words should be written toohello output file.</span></span>
3. <span data-ttu-id="deb6c-291">**storageaccount**: musí být nahrán hello název hello účet úložiště, který vlastní výstup úlohy hello kontejneru toowhich hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-291">**storageaccount**: hello name of hello Storage account that owns hello container toowhich hello task output should be uploaded.</span></span>
4. <span data-ttu-id="deb6c-292">**storagecontainer**: název hello hello úložiště kontejneru toowhich hello výstupní soubory musí být nahrán.</span><span class="sxs-lookup"><span data-stu-id="deb6c-292">**storagecontainer**: hello name of hello Storage container toowhich hello output files should be uploaded.</span></span>
5. <span data-ttu-id="deb6c-293">**sastoken**: hello sdílený přístupový podpis (SAS), který poskytuje přístup pro zápis toohello **výstup** kontejneru ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="deb6c-293">**sastoken**: hello shared access signature (SAS) that provides write access toohello **output** container in Azure Storage.</span></span> <span data-ttu-id="deb6c-294">Hello *python_tutorial_task.py* skript používá tento sdílený přístupový podpis při vytváření svého odkazu BlockBlobService.</span><span class="sxs-lookup"><span data-stu-id="deb6c-294">hello *python_tutorial_task.py* script uses this shared access signature when creates its BlockBlobService reference.</span></span> <span data-ttu-id="deb6c-295">To poskytuje přístup pro zápis toohello kontejneru bez potřeby přístupového klíče pro účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-295">This provides write access toohello container without requiring an access key for hello storage account.</span></span>

```python
# NOTE: Taken from python_tutorial_task.py

# Create hello blob client using hello container's SAS token.
# This allows us toocreate a client that provides write
# access only toohello container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a><span data-ttu-id="deb6c-296">Krok 6: Sledování úkolů</span><span class="sxs-lookup"><span data-stu-id="deb6c-296">Step 6: Monitor tasks</span></span>
<span data-ttu-id="deb6c-297">![Sledujte úkoly.][6]</span><span class="sxs-lookup"><span data-stu-id="deb6c-297">![Monitor tasks][6]</span></span><br/><span data-ttu-id="deb6c-298">
*Hello skript (1) monitorování hello stav dokončení úkolů a hello (2) úkoly nahrávají výsledek data tooAzure úložiště*</span><span class="sxs-lookup"><span data-stu-id="deb6c-298">
*hello script (1) monitors hello tasks for completion status, and (2) hello tasks upload result data tooAzure Storage*</span></span>

<span data-ttu-id="deb6c-299">Pokud úkoly přidáte tooa úlohy, jsou automaticky zařazeny do fronty a naplánovaných pro spuštění na výpočetních uzlech ve fondu hello spojené s úlohou hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-299">When tasks are added tooa job, they are automatically queued and scheduled for execution on compute nodes within hello pool associated with hello job.</span></span> <span data-ttu-id="deb6c-300">Na základě hello nastavení, které zadáte, služba Batch zpracovává všechny služby Řízení front úloh, plánování, opakování a dalších úloh správy povinností za vás.</span><span class="sxs-lookup"><span data-stu-id="deb6c-300">Based on hello settings you specify, Batch handles all task queuing, scheduling, retrying, and other task administration duties for you.</span></span>

<span data-ttu-id="deb6c-301">Toomonitoring provádění úkolů existuje mnoho přístupů.</span><span class="sxs-lookup"><span data-stu-id="deb6c-301">There are many approaches toomonitoring task execution.</span></span> <span data-ttu-id="deb6c-302">Hello `wait_for_tasks_to_complete` fungovat v *python_tutorial_client.py* nabízí jednoduchý příklad sledování úkolů určité stavu, v tomto případě hello [Dokončit] [ py_taskstate] stav.</span><span class="sxs-lookup"><span data-stu-id="deb6c-302">hello `wait_for_tasks_to_complete` function in *python_tutorial_client.py* provides a simple example of monitoring tasks for a certain state, in this case, hello [completed][py_taskstate] state.</span></span>

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in hello specified job reach hello Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: hello id of hello job whose tasks should be toomonitored.
    :param timedelta timeout: hello duration toowait for task completion. If all
    tasks in hello specified job do not reach Completed state within this time
    period, an exception will be raised.
    """
    timeout_expiration = datetime.datetime.now() + timeout

    print("Monitoring all tasks for 'Completed' state, timeout in {}..."
          .format(timeout), end='')

    while datetime.datetime.now() < timeout_expiration:
        print('.', end='')
        sys.stdout.flush()
        tasks = batch_service_client.task.list(job_id)

        incomplete_tasks = [task for task in tasks if
                            task.state != batchmodels.TaskState.completed]
        if not incomplete_tasks:
            print()
            return True
        else:
            time.sleep(1)

    print()
    raise RuntimeError("ERROR: Tasks did not reach 'Completed' state within "
                       "timeout period of " + str(timeout))
```

## <a name="step-7-download-task-output"></a><span data-ttu-id="deb6c-303">Krok 7: Stažení výstupu úkolu</span><span class="sxs-lookup"><span data-stu-id="deb6c-303">Step 7: Download task output</span></span>
<span data-ttu-id="deb6c-304">![Stažení výstupu úkolu ze služby Storage][7]</span><span class="sxs-lookup"><span data-stu-id="deb6c-304">![Download task output from Storage][7]</span></span><br/>

<span data-ttu-id="deb6c-305">Teď, když hello dokončení úlohy hello výstup z úlohy hello si můžete stáhnout ze služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="deb6c-305">Now that hello job is completed, hello output from hello tasks can be downloaded from Azure Storage.</span></span> <span data-ttu-id="deb6c-306">To lze provést pomocí volání příliš`download_blobs_from_container` v *python_tutorial_client.py*:</span><span class="sxs-lookup"><span data-stu-id="deb6c-306">This is done with a call too`download_blobs_from_container` in *python_tutorial_client.py*:</span></span>

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from hello specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: hello Azure Blob storage container from which to
     download files.
    :param directory_path: hello local directory toowhich toodownload hello files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] too{}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [!NOTE]
> <span data-ttu-id="deb6c-307">Hello volání příliš`download_blobs_from_container` v *python_tutorial_client.py* Určuje, že hello soubory by měly být stažené tooyour domovský adresář.</span><span class="sxs-lookup"><span data-stu-id="deb6c-307">hello call too`download_blobs_from_container` in *python_tutorial_client.py* specifies that hello files should be downloaded tooyour home directory.</span></span> <span data-ttu-id="deb6c-308">Myslíte, že volné toomodify to výstupní umístění.</span><span class="sxs-lookup"><span data-stu-id="deb6c-308">Feel free toomodify this output location.</span></span>
>
>

## <a name="step-8-delete-containers"></a><span data-ttu-id="deb6c-309">Krok 8: Odstranění kontejnerů</span><span class="sxs-lookup"><span data-stu-id="deb6c-309">Step 8: Delete containers</span></span>
<span data-ttu-id="deb6c-310">Vzhledem k tomu, že musíte platit za data uložená ve službě Azure Storage, vždycky je vhodné tooremove, všechny objekty BLOB, které jsou už nutná pro úlohy Batch.</span><span class="sxs-lookup"><span data-stu-id="deb6c-310">Because you are charged for data that resides in Azure Storage, it is always a good idea tooremove any blobs that are no longer needed for your Batch jobs.</span></span> <span data-ttu-id="deb6c-311">V *python_tutorial_client.py*, to se provádí pomocí tří volání příliš[BlockBlobService.delete_container][py_delete_container]:</span><span class="sxs-lookup"><span data-stu-id="deb6c-311">In *python_tutorial_client.py*, this is done with three calls too[BlockBlobService.delete_container][py_delete_container]:</span></span>

```python
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-hello-job-and-hello-pool"></a><span data-ttu-id="deb6c-312">Krok 9: Odstranění hello úlohy a fondu hello</span><span class="sxs-lookup"><span data-stu-id="deb6c-312">Step 9: Delete hello job and hello pool</span></span>
<span data-ttu-id="deb6c-313">V posledním kroku hello jsou výzvami toodelete hello úlohy a hello fondu, které byly vytvořeny hello *python_tutorial_client.py* skriptu.</span><span class="sxs-lookup"><span data-stu-id="deb6c-313">In hello final step, you are prompted toodelete hello job and hello pool that were created by hello *python_tutorial_client.py* script.</span></span> <span data-ttu-id="deb6c-314">I když se vám neúčtují poplatky za úlohy a úkoly samotné, *účtují* se vám poplatky za výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="deb6c-314">Although you are not charged for jobs and tasks themselves, you *are* charged for compute nodes.</span></span> <span data-ttu-id="deb6c-315">Proto doporučujeme, abyste uzly přidělovali, jen když je to potřeba.</span><span class="sxs-lookup"><span data-stu-id="deb6c-315">Thus, we recommend that you allocate nodes only as needed.</span></span> <span data-ttu-id="deb6c-316">Odstraňování nepoužívaných fondů by mělo být součástí vašeho standardního procesu údržby.</span><span class="sxs-lookup"><span data-stu-id="deb6c-316">Deleting unused pools can be part of your maintenance process.</span></span>

<span data-ttu-id="deb6c-317">Hello BatchServiceClient na [JobOperations] [ py_job] a [PoolOperations] [ py_pool] mají odpovídající metody odstranění, které jsou voláno, pokud Potvrdit odstranění:</span><span class="sxs-lookup"><span data-stu-id="deb6c-317">hello BatchServiceClient's [JobOperations][py_job] and [PoolOperations][py_pool] both have corresponding deletion methods, which are called if you confirm deletion:</span></span>

```python
# Clean up Batch resources (if hello user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [!IMPORTANT]
> <span data-ttu-id="deb6c-318">Pamatujte, že se vám účtují poplatky za výpočetní prostředky, takže odstranění nepoužívaných fondů vám ušetří náklady.</span><span class="sxs-lookup"><span data-stu-id="deb6c-318">Keep in mind that you are charged for compute resources--deleting unused pools will minimize cost.</span></span> <span data-ttu-id="deb6c-319">Mějte také, že odstraněním fondu odstraníte všechny výpočetní uzly v tomto fondu, a že všechna data na uzlech hello neopravitelné po odstranění fondu hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-319">Also, be aware that deleting a pool deletes all compute nodes within that pool, and that any data on hello nodes will be unrecoverable after hello pool is deleted.</span></span>
>
>

## <a name="run-hello-sample-script"></a><span data-ttu-id="deb6c-320">Spustit ukázkový skript hello</span><span class="sxs-lookup"><span data-stu-id="deb6c-320">Run hello sample script</span></span>
<span data-ttu-id="deb6c-321">Když spustíte hello *python_tutorial_client.py* skript z hello kurzu [ukázka kódu][github_article_samples], výstup konzoly hello je podobné toohello následující.</span><span class="sxs-lookup"><span data-stu-id="deb6c-321">When you run hello *python_tutorial_client.py* script from hello tutorial [code sample][github_article_samples], hello console output is similar toohello following.</span></span> <span data-ttu-id="deb6c-322">Dojde k pozastavení při `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` hello fondu výpočetních uzlů se vytvoření, spuštění, a jsou provedeny hello příkazy ve spouštěcím úkolu fondu hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-322">There is a pause at `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` while hello pool's compute nodes are created, started, and hello commands in hello pool's start task are executed.</span></span> <span data-ttu-id="deb6c-323">Použití hello [portál Azure] [ azure_portal] toomonitor fondu, výpočetních uzlů, úlohy a úkolů během a po spuštění.</span><span class="sxs-lookup"><span data-stu-id="deb6c-323">Use hello [Azure portal][azure_portal] toomonitor your pool, compute nodes, job, and tasks during and after execution.</span></span> <span data-ttu-id="deb6c-324">Použití hello [portál Azure] [ azure_portal] nebo hello [Microsoft Azure Storage Explorer] [ storage_explorer] tooview hello úložiště prostředků (kontejnerů a objektů BLOB) vytvořená aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-324">Use hello [Azure portal][azure_portal] or hello [Microsoft Azure Storage Explorer][storage_explorer] tooview hello Storage resources (containers and blobs) that are created by hello application.</span></span>

> [!TIP]
> <span data-ttu-id="deb6c-325">Spustit hello *python_tutorial_client.py* skript z v rámci hello `azure-batch-samples/Python/Batch/article_samples` adresáře.</span><span class="sxs-lookup"><span data-stu-id="deb6c-325">Run hello *python_tutorial_client.py* script from within hello `azure-batch-samples/Python/Batch/article_samples` directory.</span></span> <span data-ttu-id="deb6c-326">Používá relativní cesta pro hello `common.helpers` importu modulu, může se zobrazit `ImportError: No module named 'common'` Pokud nespouštět skript hello v tomto adresáři.</span><span class="sxs-lookup"><span data-stu-id="deb6c-326">It uses a relative path for hello `common.helpers` module import, so you might see `ImportError: No module named 'common'` if you don't run hello script from within this directory.</span></span>
>
>

<span data-ttu-id="deb6c-327">Typická doba provádění je **přibližně 5 – 7 minut** při spuštění ukázkové hello ve výchozí konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="deb6c-327">Typical execution time is **approximately 5-7 minutes** when you run hello sample in its default configuration.</span></span>

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py toocontainer [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt toocontainer [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt toocontainer [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt toocontainer [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks toojob [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached hello 'Completed' state within hello specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] too/home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] too/home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] too/home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER tooexit...
```

## <a name="next-steps"></a><span data-ttu-id="deb6c-328">Další kroky</span><span class="sxs-lookup"><span data-stu-id="deb6c-328">Next steps</span></span>
<span data-ttu-id="deb6c-329">Působí volné toomake změny příliš*python_tutorial_client.py* a *python_tutorial_task.py* výpočetní tooexperiment jiné scénáře.</span><span class="sxs-lookup"><span data-stu-id="deb6c-329">Feel free toomake changes too*python_tutorial_client.py* and *python_tutorial_task.py* tooexperiment with different compute scenarios.</span></span> <span data-ttu-id="deb6c-330">Zkuste například přidat prodlevu provádění příliš*python_tutorial_task.py* toosimulate dlouho běžící úlohy a monitorovat je hello portálu.</span><span class="sxs-lookup"><span data-stu-id="deb6c-330">For example, try adding an execution delay too*python_tutorial_task.py* toosimulate long-running tasks and monitor them in hello portal.</span></span> <span data-ttu-id="deb6c-331">Zkuste přidat další úkoly nebo upravte hello počet výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="deb6c-331">Try adding more tasks or adjusting hello number of compute nodes.</span></span> <span data-ttu-id="deb6c-332">Přidejte logiku toocheck pro a povolit hello použití existující doba provádění toospeed fondu.</span><span class="sxs-lookup"><span data-stu-id="deb6c-332">Add logic toocheck for and allow hello use of an existing pool toospeed execution time.</span></span>

<span data-ttu-id="deb6c-333">Teď, když jste obeznámeni s hello základní pracovní postup řešení Batch, je čas toodig v toohello další funkce hello služby Batch.</span><span class="sxs-lookup"><span data-stu-id="deb6c-333">Now that you're familiar with hello basic workflow of a Batch solution, it's time toodig in toohello additional features of hello Batch service.</span></span>

* <span data-ttu-id="deb6c-334">Zkontrolujte hello [funkcí přehled Azure Batch](batch-api-basics.md) článek, který doporučujeme, pokud jste novou službu toohello.</span><span class="sxs-lookup"><span data-stu-id="deb6c-334">Review hello [Overview of Azure Batch features](batch-api-basics.md) article, which we recommend if you're new toohello service.</span></span>
* <span data-ttu-id="deb6c-335">Spuštění na hello další články vývoj Batch pod **vývoje** v hello [Batch studijní][batch_learning_path].</span><span class="sxs-lookup"><span data-stu-id="deb6c-335">Start on hello other Batch development articles under **Development in-depth** in hello [Batch learning path][batch_learning_path].</span></span>
* <span data-ttu-id="deb6c-336">Podívejte se na různé implementace zpracování úlohy hello "nejčastějších N slov" pomocí služby Batch v hello [TopNWords] [ github_topnwords] ukázka.</span><span class="sxs-lookup"><span data-stu-id="deb6c-336">Check out a different implementation of processing hello "top N words" workload with Batch in hello [TopNWords][github_topnwords] sample.</span></span>

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[blog_linux]: http://blogs.technet.com/b/windowshpc/archive/2016/03/30/introducing-linux-support-on-azure-batch.aspx
[crypto]: https://cryptography.io/en/latest/
[crypto_install]: https://cryptography.io/en/latest/installation/
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[github_article_samples]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples

[nuget_packagemgr]: https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build

[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_batchserviceclient]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html#azure.batch.BatchServiceClient
[py_blockblobservice]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.blockblobservice.html#azure.storage.blob.blockblobservice.BlockBlobService
[py_cloudtask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_cs_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudServiceConfiguration
[py_delete_container]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.delete_container
[py_gen_blob_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_blob_shared_access_signature
[py_gen_container_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_container_shared_access_signature
[py_image_ref]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_job]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.JobOperations
[py_jobpreptask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobPreparationTask
[py_jobreltask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobReleaseTask
[py_list_skus]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[py_make_blob_url]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.make_blob_url
[py_pool]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.PoolOperations
[py_pooladdparam]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolAddParameter
[py_poolinfo]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolInformation
[py_resource_file]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ResourceFile
[py_samples_github]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_task]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_taskstate]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.TaskState
[py_vm_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.VirtualMachineConfiguration
[pypi_batch]: https://pypi.python.org/pypi/azure-batch
[pypi_storage]: https://pypi.python.org/pypi/azure-storage
[pypi_install]: https://packaging.python.org/installing/
[storage_explorer]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-python-tutorial/batch_workflow_01_sm.png "Vytvoření kontejnerů ve službě Azure Storage"
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "Soubory toocontainers odeslání úloh aplikace a vstupních (data)"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "Vytvoření fondu Batch"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "Vytvoření úlohy Batch"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "Přidat toojob úlohy"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "Sledování úkolů"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "Stažení výstupu úkolu ze služby Storage"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "Pracovní postup řešení Batch (úplný diagram)"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "Přihlašovací údaje Batch na portálu"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "Přihlašovací údaje služby Storage na portálu"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "Pracovní postup řešení Batch (minimální diagram)"
