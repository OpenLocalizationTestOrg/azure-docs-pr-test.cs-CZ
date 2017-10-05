---
title: "Kurz – použití sady Azure Batch SDK pro Python | Dokumentace Microsoftu"
description: "Informace o základních konceptech služby Azure Batch a vytvoření jednoduchého řešení pomocí Pythonu"
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
ms.openlocfilehash: bd5a977c10d3955639beb893cd7a37581b14f7c0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-the-batch-sdk-for-python"></a><span data-ttu-id="71a12-103">Začínáme se sadou SDK služby Batch pro Python</span><span class="sxs-lookup"><span data-stu-id="71a12-103">Get started with the Batch SDK for Python</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="71a12-104">.NET</span><span class="sxs-lookup"><span data-stu-id="71a12-104">.NET</span></span>](batch-dotnet-get-started.md)
> * [<span data-ttu-id="71a12-105">Python</span><span class="sxs-lookup"><span data-stu-id="71a12-105">Python</span></span>](batch-python-tutorial.md)
> * [<span data-ttu-id="71a12-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="71a12-106">Node.js</span></span>](batch-nodejs-get-started.md)
>
>

<span data-ttu-id="71a12-107">V tomto článku probereme malou aplikaci Batch napsanou v Pythonu a vy se seznámíte se základními informacemi o službě [Azure Batch][azure_batch] a klientovi [Batch Python][py_azure_sdk].</span><span class="sxs-lookup"><span data-stu-id="71a12-107">Learn the basics of [Azure Batch][azure_batch] and the [Batch Python][py_azure_sdk] client as we discuss a small Batch application written in Python.</span></span> <span data-ttu-id="71a12-108">Podíváme se, jak dva ukázkové skripty využívají službu Batch ke zpracování paralelní úlohy na linuxových virtuálních počítačích v cloudu, a také, jak tyto počítače komunikují se službou [Azure Storage](../storage/common/storage-introduction.md) při přípravě a načítání souborů.</span><span class="sxs-lookup"><span data-stu-id="71a12-108">We look at how two sample scripts use the Batch service to process a parallel workload on Linux virtual machines in the cloud, and how they interact with [Azure Storage](../storage/common/storage-introduction.md) for file staging and retrieval.</span></span> <span data-ttu-id="71a12-109">Seznámíte se s běžným pracovním postupem aplikací Batch a získáte základní přehled o součástech služby Batch, například o úlohách, úkolech, fondech a výpočetních uzlech.</span><span class="sxs-lookup"><span data-stu-id="71a12-109">You'll learn a common Batch application workflow and gain a base understanding of the major components of Batch such as jobs, tasks, pools, and compute nodes.</span></span>

<span data-ttu-id="71a12-110">![Pracovní postup řešení Batch (Basic)][11]</span><span class="sxs-lookup"><span data-stu-id="71a12-110">![Batch solution workflow (basic)][11]</span></span><br/>

## <a name="prerequisites"></a><span data-ttu-id="71a12-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="71a12-111">Prerequisites</span></span>
<span data-ttu-id="71a12-112">Tento článek předpokládá, že máte praktické znalosti Pythonu a umíte do jisté míry pracovat s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="71a12-112">This article assumes that you have a working knowledge of Python and familiarity with Linux.</span></span> <span data-ttu-id="71a12-113">Předpokládá také, že dokážete splnit požadavky na vytvoření účtů Azure, služby Batch a služby Storage, které jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="71a12-113">It also assumes that you're able to satisfy the account creation requirements that are specified below for Azure and the Batch and Storage services.</span></span>

### <a name="accounts"></a><span data-ttu-id="71a12-114">Účty</span><span class="sxs-lookup"><span data-stu-id="71a12-114">Accounts</span></span>
* <span data-ttu-id="71a12-115">**Účet Azure**: Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet Azure][azure_free_account].</span><span class="sxs-lookup"><span data-stu-id="71a12-115">**Azure account**: If you don't already have an Azure subscription, [create a free Azure account][azure_free_account].</span></span>
* <span data-ttu-id="71a12-116">**Účet Batch**: Po pořízení předplatného Azure si [vytvořte účet Azure Batch](batch-account-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="71a12-116">**Batch account**: Once you have an Azure subscription, [create an Azure Batch account](batch-account-create-portal.md).</span></span>
* <span data-ttu-id="71a12-117">**Účet Storage**: Viz část [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) v článku [Informace o účtech Azure Storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="71a12-117">**Storage account**: See [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

### <a name="code-sample"></a><span data-ttu-id="71a12-118">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="71a12-118">Code sample</span></span>
<span data-ttu-id="71a12-119">[Ukázka kódu][github_article_samples] Pythonu pro tento kurz je jednou z mnoha ukázek kódu Batch, které najdete v úložišti na GitHubu [azure-batch-samples][github_samples].</span><span class="sxs-lookup"><span data-stu-id="71a12-119">The Python tutorial [code sample][github_article_samples] is one of the many Batch code samples found in the [azure-batch-samples][github_samples] repository on GitHub.</span></span> <span data-ttu-id="71a12-120">Všechny ukázky můžete stáhnout kliknutím na **Klonovat nebo stáhnout > Stáhnout ZIP** na domovské stránce úložiště, nebo kliknutím na přímý odkaz ke stažení [azure-batch-samples-master.zip][github_samples_zip].</span><span class="sxs-lookup"><span data-stu-id="71a12-120">You can download all the samples by clicking **Clone or download > Download ZIP** on the repository home page, or by clicking the [azure-batch-samples-master.zip][github_samples_zip] direct download link.</span></span> <span data-ttu-id="71a12-121">Po extrahování obsahu souboru ZIP najdete oba skripty pro tento kurzu v adresáři `article_samples`:</span><span class="sxs-lookup"><span data-stu-id="71a12-121">Once you've extracted the contents of the ZIP file, the two scripts for this tutorial are found in the `article_samples` directory:</span></span>

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a><span data-ttu-id="71a12-122">Prostředí Python</span><span class="sxs-lookup"><span data-stu-id="71a12-122">Python environment</span></span>
<span data-ttu-id="71a12-123">Abyste mohli spustit ukázkový skript *python_tutorial_client.py* na místní pracovní stanici, budete potřebovat **překladač Pythonu**, který je kompatibilní s verzí **2.7** nebo **3.3+**.</span><span class="sxs-lookup"><span data-stu-id="71a12-123">To run the *python_tutorial_client.py* sample script on your local workstation, you need a **Python interpreter** compatible with version **2.7** or **3.3+**.</span></span> <span data-ttu-id="71a12-124">Skript byl otestován v Linuxu i Windows.</span><span class="sxs-lookup"><span data-stu-id="71a12-124">The script has been tested on both Linux and Windows.</span></span>

### <a name="cryptography-dependencies"></a><span data-ttu-id="71a12-125">závislosti kryptografie</span><span class="sxs-lookup"><span data-stu-id="71a12-125">cryptography dependencies</span></span>
<span data-ttu-id="71a12-126">Je nutné nainstalovat závislosti pro knihovnu [kryptografie][crypto], které vyžadují balíčky Pythonu `azure-batch` a `azure-storage`.</span><span class="sxs-lookup"><span data-stu-id="71a12-126">You must install the dependencies for the [cryptography][crypto] library, required by the `azure-batch` and `azure-storage` Python packages.</span></span> <span data-ttu-id="71a12-127">Proveďte jednu z následujících operací, které jsou vhodné pro vaši platformu, nebo si přečtěte podrobnosti o [instalaci kryptografie][crypto_install], kde najdete další informace:</span><span class="sxs-lookup"><span data-stu-id="71a12-127">Perform one of the following operations appropriate for your platform, or refer to the [cryptography installation][crypto_install] details for more information:</span></span>

* <span data-ttu-id="71a12-128">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="71a12-128">Ubuntu</span></span>

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`
* <span data-ttu-id="71a12-129">CentOS</span><span class="sxs-lookup"><span data-stu-id="71a12-129">CentOS</span></span>

    `yum update && yum install -y gcc openssl-devel libffi-devel python-devel`
* <span data-ttu-id="71a12-130">SLES/OpenSUSE</span><span class="sxs-lookup"><span data-stu-id="71a12-130">SLES/OpenSUSE</span></span>

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`
* <span data-ttu-id="71a12-131">Windows</span><span class="sxs-lookup"><span data-stu-id="71a12-131">Windows</span></span>

    `pip install cryptography`

> [!NOTE]
> <span data-ttu-id="71a12-132">Pokud instalujete Python 3.3+ na Linuxu, použijte pro závislosti Pythonu ekvivalenty python3.</span><span class="sxs-lookup"><span data-stu-id="71a12-132">If installing for Python 3.3+ on Linux, use the python3 equivalents for the Python dependencies.</span></span> <span data-ttu-id="71a12-133">Například na Ubuntu: `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`</span><span class="sxs-lookup"><span data-stu-id="71a12-133">For example, on Ubuntu: `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`</span></span>
>
>

### <a name="azure-packages"></a><span data-ttu-id="71a12-134">Balíčky Azure</span><span class="sxs-lookup"><span data-stu-id="71a12-134">Azure packages</span></span>
<span data-ttu-id="71a12-135">Následně nainstalujte balíčky Pythonu pro **Azure Batch** a **Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="71a12-135">Next, install the **Azure Batch** and **Azure Storage** Python packages.</span></span> <span data-ttu-id="71a12-136">Oba balíčky můžete nainstalovat pomocí funkce **pip** a souboru *requirements.txt*, které najdete tady:</span><span class="sxs-lookup"><span data-stu-id="71a12-136">You can install both packages by using **pip** and the *requirements.txt* found here:</span></span>

`/azure-batch-samples/Python/Batch/requirements.txt`

<span data-ttu-id="71a12-137">Zadejte následující příkaz **pip** pro instalaci balíčků Batch a Storage:</span><span class="sxs-lookup"><span data-stu-id="71a12-137">Issue following **pip** command to install the Batch and Storage packages:</span></span>

`pip install -r requirements.txt`

<span data-ttu-id="71a12-138">Nebo můžete balíčky Pythonu [azure-batch][pypi_batch] a [azure-storage][pypi_storage] nainstalovat ručně:</span><span class="sxs-lookup"><span data-stu-id="71a12-138">Or, you can install the [azure-batch][pypi_batch] and [azure-storage][pypi_storage] Python packages manually:</span></span>

`pip install azure-batch`<br/>
`pip install azure-storage`

> [!TIP]
> <span data-ttu-id="71a12-139">Pokud používáte neprivilegovaný účet, může být potřeba, abyste k příkazům přidali předponu `sudo`.</span><span class="sxs-lookup"><span data-stu-id="71a12-139">If you are using an unprivileged account, you may need to prefix your commands with `sudo`.</span></span> <span data-ttu-id="71a12-140">Například, `sudo pip install -r requirements.txt`.</span><span class="sxs-lookup"><span data-stu-id="71a12-140">For example, `sudo pip install -r requirements.txt`.</span></span> <span data-ttu-id="71a12-141">Další informace o instalaci balíčků Pythonu najdete v článku [Instalace balíčků][pypi_install] na webu python.org.</span><span class="sxs-lookup"><span data-stu-id="71a12-141">For more information on installing Python packages, see [Installing Packages][pypi_install] on python.org.</span></span>
>
>

## <a name="batch-python-tutorial-code-sample"></a><span data-ttu-id="71a12-142">Ukázka kódu Pythonu pro službu Batch</span><span class="sxs-lookup"><span data-stu-id="71a12-142">Batch Python tutorial code sample</span></span>
<span data-ttu-id="71a12-143">Ukázka kódu Pythonu pro službu Batch se skládá ze dvou skriptů Pythonu a několika datových souborů.</span><span class="sxs-lookup"><span data-stu-id="71a12-143">The Batch Python tutorial code sample consists of two Python scripts and a few data files.</span></span>

* <span data-ttu-id="71a12-144">**python_tutorial_client.py**: Komunikuje se službou Batch a se službou Storage při spouštění paralelní úlohy na výpočetních uzlech (virtuálních počítačích).</span><span class="sxs-lookup"><span data-stu-id="71a12-144">**python_tutorial_client.py**: Interacts with the Batch and Storage services to execute a parallel workload on compute nodes (virtual machines).</span></span> <span data-ttu-id="71a12-145">Skript *python_tutorial_client.py* se spouští na místní pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="71a12-145">The *python_tutorial_client.py* script runs on your local workstation.</span></span>
* <span data-ttu-id="71a12-146">**python_tutorial_task.py**: Skript, který běží na výpočetních uzlech v Azure a provádí samotnou práci.</span><span class="sxs-lookup"><span data-stu-id="71a12-146">**python_tutorial_task.py**: The script that runs on compute nodes in Azure to perform the actual work.</span></span> <span data-ttu-id="71a12-147">Skript *python_tutorial_task.py* v tomto příkladu analyzuje text v souboru staženém ze služby Azure Storage (vstupní soubor).</span><span class="sxs-lookup"><span data-stu-id="71a12-147">In the sample, *python_tutorial_task.py* parses the text in a file downloaded from Azure Storage (the input file).</span></span> <span data-ttu-id="71a12-148">Potom vytvoří textový soubor (výstupní soubor), který obsahuje seznam nejčastějších tří slov, která se zobrazují ve vstupním souboru.</span><span class="sxs-lookup"><span data-stu-id="71a12-148">Then it produces a text file (the output file) that contains a list of the top three words that appear in the input file.</span></span> <span data-ttu-id="71a12-149">Po vytvoření výstupního souboru skript *python_tutorial_task.py* odešle soubor do služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="71a12-149">After it creates the output file, *python_tutorial_task.py* uploads the file to Azure Storage.</span></span> <span data-ttu-id="71a12-150">Soubor tak bude dostupný pro stažení do klientského skriptu, který běží na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="71a12-150">This makes it available for download to the client script running on your workstation.</span></span> <span data-ttu-id="71a12-151">Skript *python_tutorial_task.py* běží paralelně v několika výpočetních uzlech v rámci služby Batch.</span><span class="sxs-lookup"><span data-stu-id="71a12-151">The *python_tutorial_task.py* script runs in parallel on multiple compute nodes in the Batch service.</span></span>
* <span data-ttu-id="71a12-152">**./data/taskdata\*.txt**: Tyto tři textové soubory zajišťují vstup pro úkoly, které jsou spouštěné na výpočetních uzlech.</span><span class="sxs-lookup"><span data-stu-id="71a12-152">**./data/taskdata\*.txt**: These three text files provide the input for the tasks that run on the compute nodes.</span></span>

<span data-ttu-id="71a12-153">Následující diagram znázorňuje primární operace, které provádí klientský skript a skript úkolu.</span><span class="sxs-lookup"><span data-stu-id="71a12-153">The following diagram illustrates the primary operations that are performed by the client and task scripts.</span></span> <span data-ttu-id="71a12-154">Tento základní pracovní postup je typický pro mnoho výpočetních řešení, která jsou vytvořená pomocí služby Batch.</span><span class="sxs-lookup"><span data-stu-id="71a12-154">This basic workflow is typical of many compute solutions that are created with Batch.</span></span> <span data-ttu-id="71a12-155">I když nepředvádí všechny funkce, které jsou ve službě Batch dostupné, téměř každý scénář Batch bude obsahovat části tohoto pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="71a12-155">While it does not demonstrate every feature available in the Batch service, nearly every Batch scenario includes portions of this workflow.</span></span>

<span data-ttu-id="71a12-156">![Ukázkový pracovní postup služby Batch][8]</span><span class="sxs-lookup"><span data-stu-id="71a12-156">![Batch example workflow][8]</span></span><br/>

[<span data-ttu-id="71a12-157">**Krok 1.**</span><span class="sxs-lookup"><span data-stu-id="71a12-157">**Step 1.**</span></span>](#step-1-create-storage-containers) <span data-ttu-id="71a12-158">Ve službě Azure Blob Storage vytvořte **kontejnery** .</span><span class="sxs-lookup"><span data-stu-id="71a12-158">Create **containers** in Azure Blob Storage.</span></span><br/><span data-ttu-id="71a12-159">
[**Krok 2.**](#step-2-upload-task-script-and-data-files)</span><span class="sxs-lookup"><span data-stu-id="71a12-159">
[**Step 2.**](#step-2-upload-task-script-and-data-files)</span></span> <span data-ttu-id="71a12-160">Odešlete skript úkolu a vstupní soubory do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="71a12-160">Upload task script and input files to containers.</span></span><br/><span data-ttu-id="71a12-161">
[**Krok 3.**](#step-3-create-batch-pool)</span><span class="sxs-lookup"><span data-stu-id="71a12-161">
[**Step 3.**](#step-3-create-batch-pool)</span></span> <span data-ttu-id="71a12-162">Vytvořte **fond** Batch.</span><span class="sxs-lookup"><span data-stu-id="71a12-162">Create a Batch **pool**.</span></span><br/>
  <span data-ttu-id="71a12-163">&nbsp;&nbsp;&nbsp;&nbsp;**3a.**</span><span class="sxs-lookup"><span data-stu-id="71a12-163">&nbsp;&nbsp;&nbsp;&nbsp;**3a.**</span></span> <span data-ttu-id="71a12-164">Když se uzly připojí k fondu, fond **StartTask** stáhne skript úkolu (python_tutorial_task.py).</span><span class="sxs-lookup"><span data-stu-id="71a12-164">The pool **StartTask** downloads the task script (python_tutorial_task.py) to nodes as they join the pool.</span></span><br/><span data-ttu-id="71a12-165">
[**Krok 4.**](#step-4-create-batch-job)</span><span class="sxs-lookup"><span data-stu-id="71a12-165">
[**Step 4.**](#step-4-create-batch-job)</span></span> <span data-ttu-id="71a12-166">Vytvořte **úlohu** Batch.</span><span class="sxs-lookup"><span data-stu-id="71a12-166">Create a Batch **job**.</span></span><br/><span data-ttu-id="71a12-167">
[**Krok 5.**](#step-5-add-tasks-to-job)</span><span class="sxs-lookup"><span data-stu-id="71a12-167">
[**Step 5.**](#step-5-add-tasks-to-job)</span></span> <span data-ttu-id="71a12-168">Přidejte do úlohy **úkoly**.</span><span class="sxs-lookup"><span data-stu-id="71a12-168">Add **tasks** to the job.</span></span><br/>
  <span data-ttu-id="71a12-169">&nbsp;&nbsp;&nbsp;&nbsp;**5a.**</span><span class="sxs-lookup"><span data-stu-id="71a12-169">&nbsp;&nbsp;&nbsp;&nbsp;**5a.**</span></span> <span data-ttu-id="71a12-170">Úkoly jsou naplánované, aby se spustily na uzlech.</span><span class="sxs-lookup"><span data-stu-id="71a12-170">The tasks are scheduled to execute on nodes.</span></span><br/>
    <span data-ttu-id="71a12-171">&nbsp;&nbsp;&nbsp;&nbsp;**5b.**</span><span class="sxs-lookup"><span data-stu-id="71a12-171">&nbsp;&nbsp;&nbsp;&nbsp;**5b.**</span></span> <span data-ttu-id="71a12-172">Každý úkol stáhne svoje vstupní data ze služby Azure Storage a potom zahájí spuštění.</span><span class="sxs-lookup"><span data-stu-id="71a12-172">Each task downloads its input data from Azure Storage, then begins execution.</span></span><br/><span data-ttu-id="71a12-173">
[**Krok 6.**](#step-6-monitor-tasks)</span><span class="sxs-lookup"><span data-stu-id="71a12-173">
[**Step 6.**](#step-6-monitor-tasks)</span></span> <span data-ttu-id="71a12-174">Sledujte úkoly.</span><span class="sxs-lookup"><span data-stu-id="71a12-174">Monitor tasks.</span></span><br/>
  <span data-ttu-id="71a12-175">&nbsp;&nbsp;&nbsp;&nbsp;**6a.**</span><span class="sxs-lookup"><span data-stu-id="71a12-175">&nbsp;&nbsp;&nbsp;&nbsp;**6a.**</span></span> <span data-ttu-id="71a12-176">Úkoly při dokončení odesílají svoje výstupní data do služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="71a12-176">As tasks are completed, they upload their output data to Azure Storage.</span></span><br/><span data-ttu-id="71a12-177">
[**Krok 7.**](#step-7-download-task-output)</span><span class="sxs-lookup"><span data-stu-id="71a12-177">
[**Step 7.**](#step-7-download-task-output)</span></span> <span data-ttu-id="71a12-178">Stáhněte si výstup úkolu ze služby Storage.</span><span class="sxs-lookup"><span data-stu-id="71a12-178">Download task output from Storage.</span></span>

<span data-ttu-id="71a12-179">Jak jsme už zmínili, ne každé řešení Batch provede právě tyto kroky a může jich dokonce obsahovat mnohem víc, ale tato ukázka představuje procesy, které běžně bývají v řešení Batch.</span><span class="sxs-lookup"><span data-stu-id="71a12-179">As mentioned, not every Batch solution performs these exact steps, and may include many more, but this sample demonstrates common processes found in a Batch solution.</span></span>

## <a name="prepare-client-script"></a><span data-ttu-id="71a12-180">Příprava klientského skriptu</span><span class="sxs-lookup"><span data-stu-id="71a12-180">Prepare client script</span></span>
<span data-ttu-id="71a12-181">Před spuštěním ukázky přidejte do skriptu *python_tutorial_client.py* přihlašovací údaje k účtu Batch a účtu Storage.</span><span class="sxs-lookup"><span data-stu-id="71a12-181">Before you run the sample, add your Batch and Storage account credentials to *python_tutorial_client.py*.</span></span> <span data-ttu-id="71a12-182">Pokud jste to ještě neudělali, otevřete soubor ve svém oblíbeném editoru a aktualizujte následující řádky pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="71a12-182">If you have not done so already, open the file in your favorite editor and update the following lines with your credentials.</span></span>

```python
# Update the Batch and Storage account credential strings below with the values
# unique to your accounts. These are used when constructing connection strings
# for the Batch and Storage client objects.

# Batch account credentials
BATCH_ACCOUNT_NAME = ""
BATCH_ACCOUNT_KEY = ""
BATCH_ACCOUNT_URL = ""

# Storage account credentials
STORAGE_ACCOUNT_NAME = ""
STORAGE_ACCOUNT_KEY = ""
```

<span data-ttu-id="71a12-183">Přihlašovací údaje k účtu Batch a k účtu služby Storage najdete v okně účtu každé služby na webu [Azure Portal][azure_portal]:</span><span class="sxs-lookup"><span data-stu-id="71a12-183">You can find your Batch and Storage account credentials within the account blade of each service in the [Azure portal][azure_portal]:</span></span>

<span data-ttu-id="71a12-184">![Přihlašovací údaje služby Batch na portálu][9]
![Přihlašovací údaje služby Storage na portálu][10]</span><span class="sxs-lookup"><span data-stu-id="71a12-184">![Batch credentials in the portal][9]
![Storage credentials in the portal][10]</span></span><br/>

<span data-ttu-id="71a12-185">V následujících částech budeme analyzovat kroky, které skripty používají ke zpracování úloh ve službě Batch.</span><span class="sxs-lookup"><span data-stu-id="71a12-185">In the following sections, we analyze the steps used by the scripts to process a workload in the Batch service.</span></span> <span data-ttu-id="71a12-186">Doporučujeme, abyste během procházení zbytku článku průběžně nahlíželi do skriptů v editoru.</span><span class="sxs-lookup"><span data-stu-id="71a12-186">We encourage you to refer regularly to the scripts in your editor while you work your way through the rest of the article.</span></span>

<span data-ttu-id="71a12-187">Přejděte do následujícího řádku ve skriptu **python_tutorial_client.py** a začněte krokem 1:</span><span class="sxs-lookup"><span data-stu-id="71a12-187">Navigate to the following line in **python_tutorial_client.py** to start with Step 1:</span></span>

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a><span data-ttu-id="71a12-188">Krok 1: Vytvoření kontejnerů služby Storage</span><span class="sxs-lookup"><span data-stu-id="71a12-188">Step 1: Create Storage containers</span></span>
<span data-ttu-id="71a12-189">![Vytvoření kontejnerů ve službě Azure Storage][1]
</span><span class="sxs-lookup"><span data-stu-id="71a12-189">![Create containers in Azure Storage][1]
</span></span><br/>

<span data-ttu-id="71a12-190">Batch obsahuje vestavěnou podporu pro komunikaci se službou Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="71a12-190">Batch includes built-in support for interacting with Azure Storage.</span></span> <span data-ttu-id="71a12-191">Kontejnery v účtu Storage poskytnou soubory, které potřebují úkoly spuštěné v účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="71a12-191">Containers in your Storage account will provide the files needed by the tasks that run in your Batch account.</span></span> <span data-ttu-id="71a12-192">Kontejnery také poskytují místo pro ukládání výstupních dat, která úkoly vytvářejí.</span><span class="sxs-lookup"><span data-stu-id="71a12-192">The containers also provide a place to store the output data that the tasks produce.</span></span> <span data-ttu-id="71a12-193">Skript *python_tutorial_client.py* nejdřív vytvoří tři kontejnery ve službě [Azure Blob Storage](../storage/common/storage-introduction.md#blob-storage):</span><span class="sxs-lookup"><span data-stu-id="71a12-193">The first thing the *python_tutorial_client.py* script does is create three containers in [Azure Blob Storage](../storage/common/storage-introduction.md#blob-storage):</span></span>

* <span data-ttu-id="71a12-194">**aplikace**: Tento kontejner bude ukládat skript Pythonu spuštěný úkoly, *python_tutorial_task.py*.</span><span class="sxs-lookup"><span data-stu-id="71a12-194">**application**: This container will store the Python script run by the tasks, *python_tutorial_task.py*.</span></span>
* <span data-ttu-id="71a12-195">**input**: Datové soubory ke zpracování budou úkoly stahovat z kontejneru *input*.</span><span class="sxs-lookup"><span data-stu-id="71a12-195">**input**: Tasks will download the data files to process from the *input* container.</span></span>
* <span data-ttu-id="71a12-196">**output**: Když úkoly dokončí zpracování vstupního souboru, odešlou výsledky do kontejneru *output*.</span><span class="sxs-lookup"><span data-stu-id="71a12-196">**output**: When tasks complete input file processing, they will upload the results to the *output* container.</span></span>

<span data-ttu-id="71a12-197">K práci s účtem služby Storage a k vytvoření kontejnerů používáme balíček [azure-storage][pypi_storage], abychom vytvořili objekt [BlockBlobService][py_blockblobservice] – klienta objektů blob.</span><span class="sxs-lookup"><span data-stu-id="71a12-197">In order to interact with a Storage account and create containers, we use the [azure-storage][pypi_storage] package to create a [BlockBlobService][py_blockblobservice] object--the "blob client."</span></span> <span data-ttu-id="71a12-198">Potom pomocí klienta objektů blob vytvoříme tři kontejnery v účtu Storage.</span><span class="sxs-lookup"><span data-stu-id="71a12-198">We then create three containers in the Storage account using the blob client.</span></span>

```python
import azure.storage.blob as azureblob

# Create the blob client, for use in obtaining references to
# blob storage containers and uploading files to containers.
blob_client = azureblob.BlockBlobService(
    account_name=STORAGE_ACCOUNT_NAME,
    account_key=STORAGE_ACCOUNT_KEY)

# Use the blob client to create the containers in Azure Storage if they
# don't yet exist.
APP_CONTAINER_NAME = 'application'
INPUT_CONTAINER_NAME = 'input'
OUTPUT_CONTAINER_NAME = 'output'
blob_client.create_container(APP_CONTAINER_NAME, fail_on_exist=False)
blob_client.create_container(INPUT_CONTAINER_NAME, fail_on_exist=False)
blob_client.create_container(OUTPUT_CONTAINER_NAME, fail_on_exist=False)
```

<span data-ttu-id="71a12-199">Po vytvoření kontejnerů může aplikace začít odesílat soubory, které budou úkoly používat.</span><span class="sxs-lookup"><span data-stu-id="71a12-199">Once the containers have been created, the application can now upload the files that will be used by the tasks.</span></span>

> [!TIP]
> <span data-ttu-id="71a12-200">Článek [Použití služby Azure Blob Storage z Pythonu](../storage/blobs/storage-python-how-to-use-blob-storage.md) nabízí pěkný přehled o práci s kontejnery a objekty blob ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="71a12-200">[How to use Azure Blob storage from Python](../storage/blobs/storage-python-how-to-use-blob-storage.md) provides a good overview of working with Azure Storage containers and blobs.</span></span> <span data-ttu-id="71a12-201">Když začnete pracovat se službou Batch, je určitě na místě si ten článek přečíst.</span><span class="sxs-lookup"><span data-stu-id="71a12-201">It should be near the top of your reading list as you start working with Batch.</span></span>
>
>

## <a name="step-2-upload-task-script-and-data-files"></a><span data-ttu-id="71a12-202">Krok 2: Odeslání skriptu úkolu a datových souborů</span><span class="sxs-lookup"><span data-stu-id="71a12-202">Step 2: Upload task script and data files</span></span>
<span data-ttu-id="71a12-203">![Odeslání aplikačních a vstupních (datových) souborů úkolů do kontejnerů][2]
</span><span class="sxs-lookup"><span data-stu-id="71a12-203">![Upload task application and input (data) files to containers][2]
</span></span><br/>

<span data-ttu-id="71a12-204">Během operace odesílání souborů skript *python_tutorial_client.py* nejdřív definuje kolekce cest k souborům **aplikace** a **vstup**, které jsou v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="71a12-204">In the file upload operation, *python_tutorial_client.py* first defines collections of **application** and **input** file paths as they exist on the local machine.</span></span> <span data-ttu-id="71a12-205">Potom tyto soubory odešle do kontejnerů, které jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="71a12-205">Then it uploads these files to the containers that you created in the previous step.</span></span>

```python
# Paths to the task script. This script will be executed by the tasks that
# run on the compute nodes.
application_file_paths = [os.path.realpath('python_tutorial_task.py')]

# The collection of data files that are to be processed by the tasks.
input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                    os.path.realpath('./data/taskdata2.txt'),
                    os.path.realpath('./data/taskdata3.txt')]

# Upload the application script to Azure Storage. This is the script that
# will process the data files, and is executed by each of the tasks on the
# compute nodes.
application_files = [
    upload_file_to_container(blob_client, APP_CONTAINER_NAME, file_path)
    for file_path in application_file_paths]

# Upload the data files. This is the data that will be processed by each of
# the tasks executed on the compute nodes in the pool.
input_files = [
    upload_file_to_container(blob_client, INPUT_CONTAINER_NAME, file_path)
    for file_path in input_file_paths]
```

<span data-ttu-id="71a12-206">Když používáte obsah seznamu, bude se pro každý soubor v kolekci volat funkce `upload_file_to_container` a zaplní se dvě kolekce [ResourceFile][py_resource_file].</span><span class="sxs-lookup"><span data-stu-id="71a12-206">Using list comprehension, the `upload_file_to_container` function is called for each file in the collections, and two [ResourceFile][py_resource_file] collections are populated.</span></span> <span data-ttu-id="71a12-207">Funkce `upload_file_to_container` se zobrazí níže:</span><span class="sxs-lookup"><span data-stu-id="71a12-207">The `upload_file_to_container` function appears below:</span></span>

```python
def upload_file_to_container(block_blob_client, container_name, path):
    """
    Uploads a local file to an Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: The name of the Azure Blob storage container.
    :param str file_path: The local path to the file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """

    import datetime
    import azure.storage.blob as azureblob
    import azure.batch.models as batchmodels

    blob_name = os.path.basename(path)

    print('Uploading file {} to container [{}]...'.format(path,
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

### <a name="resourcefiles"></a><span data-ttu-id="71a12-208">ResourceFiles</span><span class="sxs-lookup"><span data-stu-id="71a12-208">ResourceFiles</span></span>
<span data-ttu-id="71a12-209">[ResourceFile][py_resource_file] poskytuje úkolům v Batch adresu URL k souboru ve službě Azure Storage, který se před spuštěním úkolu stáhne do výpočetního uzlu.</span><span class="sxs-lookup"><span data-stu-id="71a12-209">A [ResourceFile][py_resource_file] provides tasks in Batch with the URL to a file in Azure Storage that is downloaded to a compute node before that task is run.</span></span> <span data-ttu-id="71a12-210">Vlastnost [ResourceFile][py_resource_file].**blob_source** určuje úplnou adresu URL souboru, protože existuje ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="71a12-210">The [ResourceFile][py_resource_file].**blob_source** property specifies the full URL of the file as it exists in Azure Storage.</span></span> <span data-ttu-id="71a12-211">Adresa URL může obsahovat také sdílený přístupový podpis (SAS), který zajišťuje zabezpečený přístup k souboru.</span><span class="sxs-lookup"><span data-stu-id="71a12-211">The URL may also include a shared access signature (SAS) that provides secure access to the file.</span></span> <span data-ttu-id="71a12-212">Většina typů úkolů ve službě Batch obsahuje vlastnost *ResourceFiles* včetně:</span><span class="sxs-lookup"><span data-stu-id="71a12-212">Most task types in Batch include a *ResourceFiles* property, including:</span></span>

* <span data-ttu-id="71a12-213">[CloudTask][py_task]</span><span class="sxs-lookup"><span data-stu-id="71a12-213">[CloudTask][py_task]</span></span>
* <span data-ttu-id="71a12-214">[StartTask][py_starttask]</span><span class="sxs-lookup"><span data-stu-id="71a12-214">[StartTask][py_starttask]</span></span>
* <span data-ttu-id="71a12-215">[JobPreparationTask][py_jobpreptask]</span><span class="sxs-lookup"><span data-stu-id="71a12-215">[JobPreparationTask][py_jobpreptask]</span></span>
* <span data-ttu-id="71a12-216">[JobReleaseTask][py_jobreltask]</span><span class="sxs-lookup"><span data-stu-id="71a12-216">[JobReleaseTask][py_jobreltask]</span></span>

<span data-ttu-id="71a12-217">Ukázka nepoužívá typy úloh JobPreparationTask nebo JobReleaseTask, ale můžete si o nich přečíst v článku [Spouštění úkolů přípravy a dokončení úlohy na výpočetních uzlech Azure Batch](batch-job-prep-release.md).</span><span class="sxs-lookup"><span data-stu-id="71a12-217">This sample does not use the JobPreparationTask or JobReleaseTask task types, but you can read more about them in [Run job preparation and completion tasks on Azure Batch compute nodes](batch-job-prep-release.md).</span></span>

### <a name="shared-access-signature-sas"></a><span data-ttu-id="71a12-218">Sdílený přístupový podpis (SAS)</span><span class="sxs-lookup"><span data-stu-id="71a12-218">Shared access signature (SAS)</span></span>
<span data-ttu-id="71a12-219">Sdílené přístupové podpisy jsou řetězce, které zajišťují zabezpečený přístup ke kontejnerům a objektům blob ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="71a12-219">Shared access signatures are strings that provide secure access to containers and blobs in Azure Storage.</span></span> <span data-ttu-id="71a12-220">Skript *python_tutorial_client.py* používá sdílené přístupové podpisy objektu blob i kontejneru a ukazuje, jak můžete tyto řetězce sdíleného přístupového podpisu získat ze služby Storage.</span><span class="sxs-lookup"><span data-stu-id="71a12-220">The *python_tutorial_client.py* script uses both blob and container shared access signatures, and demonstrates how to obtain these shared access signature strings from the Storage service.</span></span>

* <span data-ttu-id="71a12-221">**Sdílené přístupové podpisy objektů blob**: StartTask fondu používá sdílené přístupové podpisy objektů blob při stahování skriptu úkolu a vstupních datových souborů ze služby Storage (viz [krok 3](#step-3-create-batch-pool) níže).</span><span class="sxs-lookup"><span data-stu-id="71a12-221">**Blob shared access signatures**: The pool's StartTask uses blob shared access signatures when it downloads the task script and input data files from Storage (see [Step #3](#step-3-create-batch-pool) below).</span></span> <span data-ttu-id="71a12-222">Funkce `upload_file_to_container` v skriptu *python_tutorial_client.py* obsahuje kód, který získá sdílený přístupový podpis jednotlivých objektů blob.</span><span class="sxs-lookup"><span data-stu-id="71a12-222">The `upload_file_to_container` function in *python_tutorial_client.py* contains the code that obtains each blob's shared access signature.</span></span> <span data-ttu-id="71a12-223">Provede to voláním [BlockBlobService.make_blob_url][py_make_blob_url] v modulu služby Storage.</span><span class="sxs-lookup"><span data-stu-id="71a12-223">It does so by calling [BlockBlobService.make_blob_url][py_make_blob_url] in the Storage module.</span></span>
* <span data-ttu-id="71a12-224">**Sdílený přístupový podpis kontejneru**: Když každý úkol dokončí svojí práci ve výpočetním uzlu, odešle svůj výstupní soubor do kontejneru *výstupního* kontejneru ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="71a12-224">**Container shared access signature**: As each task finishes its work on the compute node, it uploads its output file to the *output* container in Azure Storage.</span></span> <span data-ttu-id="71a12-225">Aby to mohl udělat, skript *python_tutorial_task.py* použije sdílený přístupový podpis kontejneru, který nabízí oprávnění k zápisu do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="71a12-225">To do so, *python_tutorial_task.py* uses a container shared access signature that provides write access to the container.</span></span> <span data-ttu-id="71a12-226">Funkce `get_container_sas_token` ve skriptu *python_tutorial_client.py* získá sdílený přístupový podpis kontejneru, který se potom předá do úkolů jako argument příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="71a12-226">The `get_container_sas_token` function in *python_tutorial_client.py* obtains the container's shared access signature, which is then passed as a command-line argument to the tasks.</span></span> <span data-ttu-id="71a12-227">Krok 5 [Přidání úkolů do úlohy](#step-5-add-tasks-to-job) popisuje použití sdíleného přístupového podpisu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="71a12-227">Step #5, [Add tasks to a job](#step-5-add-tasks-to-job), discusses the usage of the container SAS.</span></span>

> [!TIP]
> <span data-ttu-id="71a12-228">Přečtěte si dvoudílný článek, který pojednává o sdíleném přístupovém podpisu [Část 1: Vysvětlení modelu sdíleného přístupového podpisu (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) a [Část 2: Vytvoření a používání sdíleného přístupového podpisu (SAS) se službou objektů blob](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md). Dozvíte se další informace o zajišťování bezpečného přístupu k datům v účtu Storage.</span><span class="sxs-lookup"><span data-stu-id="71a12-228">Check out the two-part series on shared access signatures, [Part 1: Understanding the SAS model](../storage/common/storage-dotnet-shared-access-signature-part-1.md) and [Part 2: Create and use a SAS with the Blob service](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), to learn more about providing secure access to data in your Storage account.</span></span>
>
>

## <a name="step-3-create-batch-pool"></a><span data-ttu-id="71a12-229">Krok 3: Vytvoření fondu služby Batch</span><span class="sxs-lookup"><span data-stu-id="71a12-229">Step 3: Create Batch pool</span></span>
<span data-ttu-id="71a12-230">![Vytvoření fondu Batch][3]
</span><span class="sxs-lookup"><span data-stu-id="71a12-230">![Create a Batch pool][3]
</span></span><br/>

<span data-ttu-id="71a12-231">**Fond** Batch je kolekce výpočetních uzlů (virtuálních počítačů), na kterých služba Batch provádí úkoly z úlohy.</span><span class="sxs-lookup"><span data-stu-id="71a12-231">A Batch **pool** is a collection of compute nodes (virtual machines) on which Batch executes a job's tasks.</span></span>

<span data-ttu-id="71a12-232">Po odeslání skriptu úkolu a datových souborů do účtu Storage zahájí skript *python_tutorial_client.py* pomocí modulu Batch Python komunikaci se službou Batch.</span><span class="sxs-lookup"><span data-stu-id="71a12-232">After it uploads the task script and data files to the Storage account, *python_tutorial_client.py* starts its interaction with the Batch service by using the Batch Python module.</span></span> <span data-ttu-id="71a12-233">Aby to mohl provést, vytvoří se [BatchServiceClient][py_batchserviceclient]:</span><span class="sxs-lookup"><span data-stu-id="71a12-233">To do so, a [BatchServiceClient][py_batchserviceclient] is created:</span></span>

```python
# Create a Batch service client. We'll now be interacting with the Batch
# service in addition to Storage.
credentials = batchauth.SharedKeyCredentials(BATCH_ACCOUNT_NAME,
                                             BATCH_ACCOUNT_KEY)

batch_client = batch.BatchServiceClient(
    credentials,
    base_url=BATCH_ACCOUNT_URL)
```

<span data-ttu-id="71a12-234">Na účtu Batch potom pomocí volání `create_pool` vytvoří fond výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="71a12-234">Next, a pool of compute nodes is created in the Batch account with a call to `create_pool`.</span></span>

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with the specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for the new pool.
    :param list resource_files: A collection of resource files for the pool's
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

    # Specify the commands for the pool's start task. The start task is run
    # on each node as it joins the pool, and when it's rebooted or re-imaged.
    # We use the start task to prep the node for running our task script.
    task_commands = [
        # Copy the python_tutorial_task.py script to the "shared" directory
        # that all tasks that run on the node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and the dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install the azure-storage module so that the task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get the node agent SKU and image reference for the virtual machine
    # configuration.
    # For more information about the virtual machine configuration, see:
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

<span data-ttu-id="71a12-235">Při vytváření fondu můžete definovat [PoolAddParameter][py_pooladdparam], který určuje několik vlastností fondu:</span><span class="sxs-lookup"><span data-stu-id="71a12-235">When you create a pool, you define a [PoolAddParameter][py_pooladdparam] that specifies several properties for the pool:</span></span>

* <span data-ttu-id="71a12-236">**ID** fondu (*id* – povinné)</span><span class="sxs-lookup"><span data-stu-id="71a12-236">**ID** of the pool (*id* - required)</span></span><p/><span data-ttu-id="71a12-237">Stejně jako u většiny entit ve službě Batch musí mít nový fond v rámci účtu Batch jedinečné ID.</span><span class="sxs-lookup"><span data-stu-id="71a12-237">As with most entities in Batch, your new pool must have a unique ID within your Batch account.</span></span> <span data-ttu-id="71a12-238">Váš kód bude na tento fond odkazovat pomocí jeho ID, podle kterého tento fond můžete také identifikovat na webu [Azure Portal][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="71a12-238">Your code refers to this pool using its ID, and it's how you identify the pool in the Azure [portal][azure_portal].</span></span>
* <span data-ttu-id="71a12-239">**Počet výpočetních uzlů** (*target_dedicated* – povinné)</span><span class="sxs-lookup"><span data-stu-id="71a12-239">**Number of compute nodes** (*target_dedicated* - required)</span></span><p/><span data-ttu-id="71a12-240">Tato vlastnost určuje, kolik virtuálních počítačů má být ve fondu nasazeno.</span><span class="sxs-lookup"><span data-stu-id="71a12-240">This property specifies how many VMs should be deployed in the pool.</span></span> <span data-ttu-id="71a12-241">Je důležité, abyste si všimli, že všechny účty Batch mají výchozí **kvótu**, která omezuje počet **jader** (a tedy výpočetních uzlů) na účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="71a12-241">It is important to note that all Batch accounts have a default **quota** that limits the number of **cores** (and thus, compute nodes) in a Batch account.</span></span> <span data-ttu-id="71a12-242">Výchozí kvóty a pokyny pro [navýšení kvóty](batch-quota-limit.md#increase-a-quota) (například maximální počet jader na účtu Batch) najdete v článku [Kvóty a omezení služby Azure Batch](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="71a12-242">You can find the default quotas and instructions on how to [increase a quota](batch-quota-limit.md#increase-a-quota) (such as the maximum number of cores in your Batch account) in [Quotas and limits for the Azure Batch service](batch-quota-limit.md).</span></span> <span data-ttu-id="71a12-243">Možná vás někdy napadne otázka, proč váš fond nedosahuje víc než X uzlů.</span><span class="sxs-lookup"><span data-stu-id="71a12-243">If you find yourself asking "Why won't my pool reach more than X nodes?"</span></span> <span data-ttu-id="71a12-244">příčinou může být tato kvóta na jádra.</span><span class="sxs-lookup"><span data-stu-id="71a12-244">this core quota may be the cause.</span></span>
* <span data-ttu-id="71a12-245">**Operační systém** uzlů (*virtual_machine_configuration* **nebo** *cloud_service_configuration* – povinné)</span><span class="sxs-lookup"><span data-stu-id="71a12-245">**Operating system** for nodes (*virtual_machine_configuration* **or** *cloud_service_configuration* - required)</span></span><p/><span data-ttu-id="71a12-246">Ve skriptu *python_tutorial_client.py* vytvoříme fond linuxových uzlů pomocí [VirtualMachineConfiguration][py_vm_config].</span><span class="sxs-lookup"><span data-stu-id="71a12-246">In *python_tutorial_client.py*, we create a pool of Linux nodes using a [VirtualMachineConfiguration][py_vm_config].</span></span> <span data-ttu-id="71a12-247">Funkce `select_latest_verified_vm_image_with_node_agent_sku` v `common.helpers` zjednodušuje práci s imagemi z [Azure Virtual Machines Marketplace][vm_marketplace].</span><span class="sxs-lookup"><span data-stu-id="71a12-247">The `select_latest_verified_vm_image_with_node_agent_sku` function in `common.helpers` simplifies working with [Azure Virtual Machines Marketplace][vm_marketplace] images.</span></span> <span data-ttu-id="71a12-248">Další informace o používání imagí z Marketplace najdete v tématu [Zřízení linuxových výpočetních uzlů ve fondech Azure Batch](batch-linux-nodes.md).</span><span class="sxs-lookup"><span data-stu-id="71a12-248">See [Provision Linux compute nodes in Azure Batch pools](batch-linux-nodes.md) for more information about using Marketplace images.</span></span>
* <span data-ttu-id="71a12-249">**Velikost výpočetních uzlů** (*vm_size* – povinné)</span><span class="sxs-lookup"><span data-stu-id="71a12-249">**Size of compute nodes** (*vm_size* - required)</span></span><p/><span data-ttu-id="71a12-250">Vzhledem k tomu, že zadáváme linuxové uzly pro naší [VirtualMachineConfiguration][py_vm_config], zadáme velikost virtuálního počítače (v této ukázce `STANDARD_A1`) podle článku [Velikosti virtuálních počítačů v Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="71a12-250">Since we're specifying Linux nodes for our [VirtualMachineConfiguration][py_vm_config], we specify a VM size (`STANDARD_A1` in this sample) from [Sizes for virtual machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="71a12-251">Další informace opět najdete v článku [Zřízení linuxových výpočetních uzlů ve fondech Azure Batch](batch-linux-nodes.md) </span><span class="sxs-lookup"><span data-stu-id="71a12-251">Again, see [Provision Linux compute nodes in Azure Batch pools](batch-linux-nodes.md) for more information.</span></span>
* <span data-ttu-id="71a12-252">**Spustit úkol** (*start_task* – nepovinné)</span><span class="sxs-lookup"><span data-stu-id="71a12-252">**Start task** (*start_task* - not required)</span></span><p/><span data-ttu-id="71a12-253">Spolu s výše uvedenými fyzickými vlastnostmi uzlu můžete určit také [StartTask][py_starttask] fondu (nepovinné).</span><span class="sxs-lookup"><span data-stu-id="71a12-253">Along with the above physical node properties, you may also specify a [StartTask][py_starttask] for the pool (it is not required).</span></span> <span data-ttu-id="71a12-254">StartTask se spustí na každém uzlu, když se takový uzel připojí k fondu, a taky pokaždé, když se uzel restartuje.</span><span class="sxs-lookup"><span data-stu-id="71a12-254">The StartTask executes on each node as that node joins the pool, and each time a node is restarted.</span></span> <span data-ttu-id="71a12-255">StartTask je zvláště užitečný pro přípravu výpočetních uzlů k provádění úkolů, například k instalaci aplikací, které budou vaše úkoly spouštět.</span><span class="sxs-lookup"><span data-stu-id="71a12-255">The StartTask is especially useful for preparing compute nodes for the execution of tasks, such as installing the applications that your tasks run.</span></span><p/><span data-ttu-id="71a12-256">V této ukázkové aplikaci StartTask zkopíruje soubory, které stáhne ze služby Storage (které je určené vlastností **resource_files** ze StartTask) z *pracovního adresáře* StartTask do *sdíleného* adresáře, ke kterému mají přístup všechny úkoly spuštěné v takovém uzlu.</span><span class="sxs-lookup"><span data-stu-id="71a12-256">In this sample application, the StartTask copies the files that it downloads from Storage (which are specified by using the StartTask's **resource_files** property) from the StartTask *working directory* to the *shared* directory that all tasks running on the node can access.</span></span> <span data-ttu-id="71a12-257">V podstatě zkopíruje soubor `python_tutorial_task.py` do sdíleného adresáře v každém uzlu v okamžiku, kdy se uzel připojí k fondu, aby každý úkol spuštěný v uzlu měl k tomuto souboru přístup.</span><span class="sxs-lookup"><span data-stu-id="71a12-257">Essentially, this copies `python_tutorial_task.py` to the shared directory on each node as the node joins the pool, so that any tasks that run on the node can access it.</span></span>

<span data-ttu-id="71a12-258">Můžete si povšimnout volání pomocné funkce `wrap_commands_in_shell`.</span><span class="sxs-lookup"><span data-stu-id="71a12-258">You may notice the call to the `wrap_commands_in_shell` helper function.</span></span> <span data-ttu-id="71a12-259">Tato funkce vezme kolekci samostatných příkazů a vytvoří jeden příkazový řádek, který odpovídá vlastnosti příkazového řádku úkolu.</span><span class="sxs-lookup"><span data-stu-id="71a12-259">This function takes a collection of separate commands and creates a single command line appropriate for a task's command-line property.</span></span>

<span data-ttu-id="71a12-260">Ve výše uvedeném fragmentu kódu je také zajímavé použití dvou proměnných prostředí ve vlastnosti **command_line** v StartTask: `AZ_BATCH_TASK_WORKING_DIR` a `AZ_BATCH_NODE_SHARED_DIR`.</span><span class="sxs-lookup"><span data-stu-id="71a12-260">Also notable in the code snippet above is the use of two environment variables in the **command_line** property of the StartTask: `AZ_BATCH_TASK_WORKING_DIR` and `AZ_BATCH_NODE_SHARED_DIR`.</span></span> <span data-ttu-id="71a12-261">Každý výpočetní uzel v rámci fondu Batch je automaticky nakonfigurovaný pomocí řady proměnných prostředí, které se týkají služby Batch.</span><span class="sxs-lookup"><span data-stu-id="71a12-261">Each compute node within a Batch pool is automatically configured with several environment variables that are specific to Batch.</span></span> <span data-ttu-id="71a12-262">Jakýkoli proces spuštěný úkolem má přístup k těmto proměnným prostředí.</span><span class="sxs-lookup"><span data-stu-id="71a12-262">Any process that is executed by a task has access to these environment variables.</span></span>

> [!TIP]
> <span data-ttu-id="71a12-263">Další informace o proměnných prostředí, které jsou dostupné na výpočetní uzlech ve fondu Batch, a také informace o pracovních adresářích úkolu najdete v částech **Nastavení prostředí pro úkoly** a **Soubory a adresáře** v článku [Přehled funkcí Azure Batch](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="71a12-263">To find out more about the environment variables that are available on compute nodes in a Batch pool, as well as information on task working directories, see **Environment settings for tasks** and **Files and directories** in the [overview of Azure Batch features](batch-api-basics.md).</span></span>
>
>

## <a name="step-4-create-batch-job"></a><span data-ttu-id="71a12-264">Krok 4: Vytvoření úlohy Batch</span><span class="sxs-lookup"><span data-stu-id="71a12-264">Step 4: Create Batch job</span></span>
<span data-ttu-id="71a12-265">![Vytvoření úlohy Batch][4]</span><span class="sxs-lookup"><span data-stu-id="71a12-265">![Create Batch job][4]</span></span><br/>

<span data-ttu-id="71a12-266">**Úloha** Batch je kolekcí úkolů a je přidružená k fondu výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="71a12-266">A Batch **job** is a collection of tasks, and is associated with a pool of compute nodes.</span></span> <span data-ttu-id="71a12-267">Úkoly v úloze se spustit na přidružených výpočetních uzlech fondu.</span><span class="sxs-lookup"><span data-stu-id="71a12-267">The tasks in a job execute on the associated pool's compute nodes.</span></span>

<span data-ttu-id="71a12-268">Úlohu můžete použít nejen k uspořádání a sledování úkolů v souvisejících úlohách, ale také k nastavení určitých omezení – například maximálního runtime úlohy (a při rozšíření i pro její úkoly) a také priority úloh ve vztahu k dalším úlohám na účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="71a12-268">You can use a job not only for organizing and tracking tasks in related workloads, but also for imposing certain constraints--such as the maximum runtime for the job (and by extension, its tasks) and job priority in relation to other jobs in the Batch account.</span></span> <span data-ttu-id="71a12-269">V tomto příkladu je úloha přidružená jenom k fondu, který byl vytvořen v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="71a12-269">In this example, however, the job is associated only with the pool that was created in step #3.</span></span> <span data-ttu-id="71a12-270">Žádné další vlastnosti se nekonfigurují.</span><span class="sxs-lookup"><span data-stu-id="71a12-270">No additional properties are configured.</span></span>

<span data-ttu-id="71a12-271">Všechny úlohy Batch jsou přidružené ke konkrétnímu fondu.</span><span class="sxs-lookup"><span data-stu-id="71a12-271">All Batch jobs are associated with a specific pool.</span></span> <span data-ttu-id="71a12-272">Toto přidružení označuje uzly, na kterých se úkoly úlohy spustí.</span><span class="sxs-lookup"><span data-stu-id="71a12-272">This association indicates which nodes the job's tasks execute on.</span></span> <span data-ttu-id="71a12-273">Fond určíte použitím vlastnosti [PoolInformation][py_poolinfo], jak znázorňuje následující fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="71a12-273">You specify the pool by using the [PoolInformation][py_poolinfo] property, as shown in the code snippet below.</span></span>

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with the specified ID, associated with the specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID for the job.
    :param str pool_id: The ID for the pool.
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

<span data-ttu-id="71a12-274">Po vytvoření úlohy budou přidány úkoly, které budou provádět práci.</span><span class="sxs-lookup"><span data-stu-id="71a12-274">Now that a job has been created, tasks are added to perform the work.</span></span>

## <a name="step-5-add-tasks-to-job"></a><span data-ttu-id="71a12-275">Krok 5: Přidání úkolů do úlohy</span><span class="sxs-lookup"><span data-stu-id="71a12-275">Step 5: Add tasks to job</span></span>
<span data-ttu-id="71a12-276">![Přidání úkolů do úlohy][5]</span><span class="sxs-lookup"><span data-stu-id="71a12-276">![Add tasks to job][5]</span></span><br/><span data-ttu-id="71a12-277">
*(1) Úkoly jsou přidány do úlohy, (2) úkoly jsou naplánovány ke spuštění na uzlech a (3) úkoly stahují datové soubory ke zpracování*</span><span class="sxs-lookup"><span data-stu-id="71a12-277">
*(1) Tasks are added to the job, (2) the tasks are scheduled to run on nodes, and (3) the tasks download the data files to process*</span></span>

<span data-ttu-id="71a12-278">**Úkoly** Batch jsou jednotlivé jednotky práce, které se spouští na výpočetních uzlech.</span><span class="sxs-lookup"><span data-stu-id="71a12-278">Batch **tasks** are the individual units of work that execute on the compute nodes.</span></span> <span data-ttu-id="71a12-279">Úkol má příkazový řádek a spouští skripty nebo spustitelné soubory, které jste v takovém příkazovém řádku určili.</span><span class="sxs-lookup"><span data-stu-id="71a12-279">A task has a command line and runs the scripts or executables that you specify in that command line.</span></span>

<span data-ttu-id="71a12-280">Aby mohly skutečně provést nějakou práci, musí úkoly nejprve přidat do úlohy.</span><span class="sxs-lookup"><span data-stu-id="71a12-280">To actually perform work, tasks must be added to a job.</span></span> <span data-ttu-id="71a12-281">Každý [CloudTask][py_task] je nakonfigurovaný pomocí vlastnosti příkazového řádku a [ResourceFiles][py_resource_file] (stejně jako u StartTask fondu), kterou si úkol stáhne do uzlu předtím, než se jeho příkazový řádek automaticky spustí.</span><span class="sxs-lookup"><span data-stu-id="71a12-281">Each [CloudTask][py_task] is configured with a command-line property and [ResourceFiles][py_resource_file] (as with the pool's StartTask) that the task downloads to the node before its command line is automatically executed.</span></span> <span data-ttu-id="71a12-282">Každý úkol v ukázce zpracovává jenom jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="71a12-282">In the sample, each task processes only one file.</span></span> <span data-ttu-id="71a12-283">Proto jeho kolekce ResourceFiles obsahuje jen jeden prvek.</span><span class="sxs-lookup"><span data-stu-id="71a12-283">Thus, its ResourceFiles collection contains a single element.</span></span>

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in the collection to the specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID of the job to which to add the tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: The ID of an Azure Blob storage container to
    which the tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    the specified Azure Blob storage container.
    """

    print('Adding {} tasks to job [{}]...'.format(len(input_files), job_id))

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
> <span data-ttu-id="71a12-284">Když přistupují k proměnným prostředí, například k `$AZ_BATCH_NODE_SHARED_DIR`, nebo když spouští aplikaci, která se nedá najít na `PATH` uzlu, musí příkazové řádky úkolu vyvolat prostředí explicitně, například pomocí `/bin/sh -c MyTaskApplication $MY_ENV_VAR`.</span><span class="sxs-lookup"><span data-stu-id="71a12-284">When they access environment variables such as `$AZ_BATCH_NODE_SHARED_DIR` or execute an application not found in the node's `PATH`, task command lines must invoke the shell explicitly, such as with `/bin/sh -c MyTaskApplication $MY_ENV_VAR`.</span></span> <span data-ttu-id="71a12-285">Tento požadavek není nutný, pokud vaše úkoly spouští aplikace v `PATH` uzlu a neodkazují na žádné proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="71a12-285">This requirement is unnecessary if your tasks execute an application in the node's `PATH` and do not reference any environment variables.</span></span>
>
>

<span data-ttu-id="71a12-286">Ve smyčce `for` ve výše uvedeném fragmentu kódu můžete vidět, že příkazový řádek úkolu je vytvořený pomocí pěti argumentů příkazového řádku, které se předávají do skriptu *python_tutorial_task.py*:</span><span class="sxs-lookup"><span data-stu-id="71a12-286">Within the `for` loop in the code snippet above, you can see that the command line for the task is constructed with five command-line arguments that are passed to *python_tutorial_task.py*:</span></span>

1. <span data-ttu-id="71a12-287">**filepath**: Jedná se o místní cestu k souboru, protože soubor existuje na uzlu.</span><span class="sxs-lookup"><span data-stu-id="71a12-287">**filepath**: This is the local path to the file as it exists on the node.</span></span> <span data-ttu-id="71a12-288">Když byl objekt ResourceFile v `upload_file_to_container` ve výše uvedeném kroku 2 vytvořený, použil se pro tuto vlastnost název souboru (parametr `file_path` v konstruktoru ResourceFile).</span><span class="sxs-lookup"><span data-stu-id="71a12-288">When the ResourceFile object in `upload_file_to_container` was created in Step 2 above, the file name was used for this property (the `file_path` parameter in the ResourceFile constructor).</span></span> <span data-ttu-id="71a12-289">To znamená, že soubor můžete najít ve stejném adresáři na uzlu jako skript *python_tutorial_task.py*.</span><span class="sxs-lookup"><span data-stu-id="71a12-289">This indicates that the file can be found in the same directory on the node as *python_tutorial_task.py*.</span></span>
2. <span data-ttu-id="71a12-290">**numwords**: *N* nejčastějších slov, která musí být zapsaná do výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="71a12-290">**numwords**: The top *N* words should be written to the output file.</span></span>
3. <span data-ttu-id="71a12-291">**storageaccount**: Název účtu Storage, který vlastní kontejner, do kterého se musí odesílat výstup úkolu.</span><span class="sxs-lookup"><span data-stu-id="71a12-291">**storageaccount**: The name of the Storage account that owns the container to which the task output should be uploaded.</span></span>
4. <span data-ttu-id="71a12-292">**storagecontainer**: Název kontejneru Storage, do kterého se musí odesílat výstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="71a12-292">**storagecontainer**: The name of the Storage container to which the output files should be uploaded.</span></span>
5. <span data-ttu-id="71a12-293">**sastoken**: Sdílený přístupový podpis (SAS), který zajišťuje oprávnění k zápisu do **výstupního** kontejneru ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="71a12-293">**sastoken**: The shared access signature (SAS) that provides write access to the **output** container in Azure Storage.</span></span> <span data-ttu-id="71a12-294">Skript *python_tutorial_task.py* používá tento sdílený přístupový podpis při vytváření svého odkazu BlockBlobService.</span><span class="sxs-lookup"><span data-stu-id="71a12-294">The *python_tutorial_task.py* script uses this shared access signature when creates its BlockBlobService reference.</span></span> <span data-ttu-id="71a12-295">Tím je zajištěné oprávnění k zápisu do kontejneru bez potřeby přístupového klíče k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="71a12-295">This provides write access to the container without requiring an access key for the storage account.</span></span>

```python
# NOTE: Taken from python_tutorial_task.py

# Create the blob client using the container's SAS token.
# This allows us to create a client that provides write
# access only to the container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a><span data-ttu-id="71a12-296">Krok 6: Sledování úkolů</span><span class="sxs-lookup"><span data-stu-id="71a12-296">Step 6: Monitor tasks</span></span>
<span data-ttu-id="71a12-297">![Sledujte úkoly.][6]</span><span class="sxs-lookup"><span data-stu-id="71a12-297">![Monitor tasks][6]</span></span><br/><span data-ttu-id="71a12-298">
*Skript (1) sleduje stav dokončení úkolů a (2) úkoly odesílají výsledná data do služby Azure Storage.*</span><span class="sxs-lookup"><span data-stu-id="71a12-298">
*The script (1) monitors the tasks for completion status, and (2) the tasks upload result data to Azure Storage*</span></span>

<span data-ttu-id="71a12-299">Pokud úkoly přidáte do úlohy, budou automaticky zařazeny do fronty a bude naplánováno jejich spuštění na výpočetních uzlech ve fondu, který je k úloze přidružený.</span><span class="sxs-lookup"><span data-stu-id="71a12-299">When tasks are added to a job, they are automatically queued and scheduled for execution on compute nodes within the pool associated with the job.</span></span> <span data-ttu-id="71a12-300">Na základě vámi zadaných nastavení služba Batch zpracuje veškeré řazení úkolů do fronty, plánování úkolů, opakované spouštění a další povinnosti spojené se správou úkolů místo vás.</span><span class="sxs-lookup"><span data-stu-id="71a12-300">Based on the settings you specify, Batch handles all task queuing, scheduling, retrying, and other task administration duties for you.</span></span>

<span data-ttu-id="71a12-301">Ke sledování provádění úkolů existuje mnoho přístupů.</span><span class="sxs-lookup"><span data-stu-id="71a12-301">There are many approaches to monitoring task execution.</span></span> <span data-ttu-id="71a12-302">Funkce `wait_for_tasks_to_complete` ve skriptu *python_tutorial_client.py* nabízí jednoduchý příklad sledování úkolů a jejich určitého stavu, v tomto případě stav [dokončeno][py_taskstate].</span><span class="sxs-lookup"><span data-stu-id="71a12-302">The `wait_for_tasks_to_complete` function in *python_tutorial_client.py* provides a simple example of monitoring tasks for a certain state, in this case, the [completed][py_taskstate] state.</span></span>

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in the specified job reach the Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The id of the job whose tasks should be to monitored.
    :param timedelta timeout: The duration to wait for task completion. If all
    tasks in the specified job do not reach Completed state within this time
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

## <a name="step-7-download-task-output"></a><span data-ttu-id="71a12-303">Krok 7: Stažení výstupu úkolu</span><span class="sxs-lookup"><span data-stu-id="71a12-303">Step 7: Download task output</span></span>
<span data-ttu-id="71a12-304">![Stažení výstupu úkolu ze služby Storage][7]</span><span class="sxs-lookup"><span data-stu-id="71a12-304">![Download task output from Storage][7]</span></span><br/>

<span data-ttu-id="71a12-305">Po dokončení úlohy můžete ze služby Azure Storage stáhnout výstup úkolů.</span><span class="sxs-lookup"><span data-stu-id="71a12-305">Now that the job is completed, the output from the tasks can be downloaded from Azure Storage.</span></span> <span data-ttu-id="71a12-306">Provádí se to voláním `download_blobs_from_container` ve skriptu *python_tutorial_client.py*:</span><span class="sxs-lookup"><span data-stu-id="71a12-306">This is done with a call to `download_blobs_from_container` in *python_tutorial_client.py*:</span></span>

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from the specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: The Azure Blob storage container from which to
     download files.
    :param directory_path: The local directory to which to download the files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] to {}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [!NOTE]
> <span data-ttu-id="71a12-307">Volání `download_blobs_from_container` ve skriptu *python_tutorial_client.py* určuje, že soubory mají být stažené do vašeho domovského adresáře.</span><span class="sxs-lookup"><span data-stu-id="71a12-307">The call to `download_blobs_from_container` in *python_tutorial_client.py* specifies that the files should be downloaded to your home directory.</span></span> <span data-ttu-id="71a12-308">Umístění výstupu můžete podle libosti změnit.</span><span class="sxs-lookup"><span data-stu-id="71a12-308">Feel free to modify this output location.</span></span>
>
>

## <a name="step-8-delete-containers"></a><span data-ttu-id="71a12-309">Krok 8: Odstranění kontejnerů</span><span class="sxs-lookup"><span data-stu-id="71a12-309">Step 8: Delete containers</span></span>
<span data-ttu-id="71a12-310">Vzhledem k tomu, že musíte platit za data, která si necháváte ve službě Azure Storage, doporučujeme odebrat všechny objekty blob, které už pro úlohy Batch nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="71a12-310">Because you are charged for data that resides in Azure Storage, it is always a good idea to remove any blobs that are no longer needed for your Batch jobs.</span></span> <span data-ttu-id="71a12-311">Ve skriptu *python_tutorial_client.py* se to provádí pomocí tří volání [BlockBlobService.delete_container][py_delete_container]:</span><span class="sxs-lookup"><span data-stu-id="71a12-311">In *python_tutorial_client.py*, this is done with three calls to [BlockBlobService.delete_container][py_delete_container]:</span></span>

```python
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-the-job-and-the-pool"></a><span data-ttu-id="71a12-312">Krok 9: Odstranění úlohy a fondu</span><span class="sxs-lookup"><span data-stu-id="71a12-312">Step 9: Delete the job and the pool</span></span>
<span data-ttu-id="71a12-313">V posledním kroku budete vyzváni k odstranění úlohy a fondu, které vytvořil skript *python_tutorial_client.py*.</span><span class="sxs-lookup"><span data-stu-id="71a12-313">In the final step, you are prompted to delete the job and the pool that were created by the *python_tutorial_client.py* script.</span></span> <span data-ttu-id="71a12-314">I když se vám neúčtují poplatky za úlohy a úkoly samotné, *účtují* se vám poplatky za výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="71a12-314">Although you are not charged for jobs and tasks themselves, you *are* charged for compute nodes.</span></span> <span data-ttu-id="71a12-315">Proto doporučujeme, abyste uzly přidělovali, jen když je to potřeba.</span><span class="sxs-lookup"><span data-stu-id="71a12-315">Thus, we recommend that you allocate nodes only as needed.</span></span> <span data-ttu-id="71a12-316">Odstraňování nepoužívaných fondů by mělo být součástí vašeho standardního procesu údržby.</span><span class="sxs-lookup"><span data-stu-id="71a12-316">Deleting unused pools can be part of your maintenance process.</span></span>

<span data-ttu-id="71a12-317">[JobOperations][py_job] a [PoolOperations][py_pool] z BatchServiceClient mají odpovídající metody odstranění, které se volají, pokud potvrdíte odstranění:</span><span class="sxs-lookup"><span data-stu-id="71a12-317">The BatchServiceClient's [JobOperations][py_job] and [PoolOperations][py_pool] both have corresponding deletion methods, which are called if you confirm deletion:</span></span>

```python
# Clean up Batch resources (if the user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [!IMPORTANT]
> <span data-ttu-id="71a12-318">Pamatujte, že se vám účtují poplatky za výpočetní prostředky, takže odstranění nepoužívaných fondů vám ušetří náklady.</span><span class="sxs-lookup"><span data-stu-id="71a12-318">Keep in mind that you are charged for compute resources--deleting unused pools will minimize cost.</span></span> <span data-ttu-id="71a12-319">Musíme ale upozornit, že odstraněním fondu odstraníte všechny výpočetní uzly v takovém fondu a veškerá data na uzlech budou po odstranění fondu ztracená.</span><span class="sxs-lookup"><span data-stu-id="71a12-319">Also, be aware that deleting a pool deletes all compute nodes within that pool, and that any data on the nodes will be unrecoverable after the pool is deleted.</span></span>
>
>

## <a name="run-the-sample-script"></a><span data-ttu-id="71a12-320">Spuštění ukázkového skriptu</span><span class="sxs-lookup"><span data-stu-id="71a12-320">Run the sample script</span></span>
<span data-ttu-id="71a12-321">Při spuštění skriptu *python_tutorial_client.py* z [ukázky kódu][github_article_samples] pro tento kurz bude výstup konzoly podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="71a12-321">When you run the *python_tutorial_client.py* script from the tutorial [code sample][github_article_samples], the console output is similar to the following.</span></span> <span data-ttu-id="71a12-322">Zatímco se vytvářejí a spouští výpočetní uzly fondu a provádí se příkazy ve spouštěcím úkolu fondu, uvidíte pozastavení na `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...`.</span><span class="sxs-lookup"><span data-stu-id="71a12-322">There is a pause at `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` while the pool's compute nodes are created, started, and the commands in the pool's start task are executed.</span></span> <span data-ttu-id="71a12-323">Ke sledování fondu, výpočetních uzlů, úlohy a úkolů během a po spuštění použijte [Azure Portal][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="71a12-323">Use the [Azure portal][azure_portal] to monitor your pool, compute nodes, job, and tasks during and after execution.</span></span> <span data-ttu-id="71a12-324">K zobrazení prostředků služby Storage (kontejnerů a objektů blob), které vytvořila aplikace, použijte [Azure Portal][azure_portal] nebo [Microsoft Azure Storage Explorer][storage_explorer].</span><span class="sxs-lookup"><span data-stu-id="71a12-324">Use the [Azure portal][azure_portal] or the [Microsoft Azure Storage Explorer][storage_explorer] to view the Storage resources (containers and blobs) that are created by the application.</span></span>

> [!TIP]
> <span data-ttu-id="71a12-325">Z adresáře `azure-batch-samples/Python/Batch/article_samples` spusťte skript *python_tutorial_client.py*.</span><span class="sxs-lookup"><span data-stu-id="71a12-325">Run the *python_tutorial_client.py* script from within the `azure-batch-samples/Python/Batch/article_samples` directory.</span></span> <span data-ttu-id="71a12-326">Protože pro import modulu `common.helpers` používá relativní cestu, může se při spuštění mimo tento adresář zobrazit chyba `ImportError: No module named 'common'`.</span><span class="sxs-lookup"><span data-stu-id="71a12-326">It uses a relative path for the `common.helpers` module import, so you might see `ImportError: No module named 'common'` if you don't run the script from within this directory.</span></span>
>
>

<span data-ttu-id="71a12-327">Typická doba provádění je **přibližně 5-7 minut**, pokud ukázku spustíte ve výchozí konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="71a12-327">Typical execution time is **approximately 5-7 minutes** when you run the sample in its default configuration.</span></span>

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py to container [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt to container [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks to job [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached the 'Completed' state within the specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] to /home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] to /home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] to /home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER to exit...
```

## <a name="next-steps"></a><span data-ttu-id="71a12-328">Další kroky</span><span class="sxs-lookup"><span data-stu-id="71a12-328">Next steps</span></span>
<span data-ttu-id="71a12-329">Nebojte se provést ve skriptech *python_tutorial_client.py* a *python_tutorial_task.py* změny a experimentovat s různými výpočetními scénáři.</span><span class="sxs-lookup"><span data-stu-id="71a12-329">Feel free to make changes to *python_tutorial_client.py* and *python_tutorial_task.py* to experiment with different compute scenarios.</span></span> <span data-ttu-id="71a12-330">Zkuste například do skriptu *python_tutorial_task.py* přidat prodlevu provádění, abyste mohli simulovat dlouhotrvající úkoly a sledovat je na portálu.</span><span class="sxs-lookup"><span data-stu-id="71a12-330">For example, try adding an execution delay to *python_tutorial_task.py* to simulate long-running tasks and monitor them in the portal.</span></span> <span data-ttu-id="71a12-331">Zkuste přidat další úkoly nebo upravit počet výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="71a12-331">Try adding more tasks or adjusting the number of compute nodes.</span></span> <span data-ttu-id="71a12-332">Přidejte logiku pro kontrolu a povolení použití existujícího fondu, abyste urychlili dobu spouštění.</span><span class="sxs-lookup"><span data-stu-id="71a12-332">Add logic to check for and allow the use of an existing pool to speed execution time.</span></span>

<span data-ttu-id="71a12-333">Teď, když jste se seznámili se základním pracovním postupem řešení Batch, je čas proniknout do dalších funkcí služby Batch.</span><span class="sxs-lookup"><span data-stu-id="71a12-333">Now that you're familiar with the basic workflow of a Batch solution, it's time to dig in to the additional features of the Batch service.</span></span>

* <span data-ttu-id="71a12-334">Přečtěte si článek [Přehled funkcí Azure Batch](batch-api-basics.md), který doporučujeme všem novým uživatelům služby.</span><span class="sxs-lookup"><span data-stu-id="71a12-334">Review the [Overview of Azure Batch features](batch-api-basics.md) article, which we recommend if you're new to the service.</span></span>
* <span data-ttu-id="71a12-335">Začněte u dalších článků o vývoji pro Batch, které najdete v [Postupu výuky pro Batch][batch_learning_path] v části **Podrobný popis vývoje**.</span><span class="sxs-lookup"><span data-stu-id="71a12-335">Start on the other Batch development articles under **Development in-depth** in the [Batch learning path][batch_learning_path].</span></span>
* <span data-ttu-id="71a12-336">Podívejte se na různé implementace zpracování úlohy „N nejčastějších slov“ a použijte k tomu Batch v ukázce [TopNWords][github_topnwords].</span><span class="sxs-lookup"><span data-stu-id="71a12-336">Check out a different implementation of processing the "top N words" workload with Batch in the [TopNWords][github_topnwords] sample.</span></span>

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
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "Odeslání aplikačních a vstupních (datových) souborů úkolů do kontejnerů"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "Vytvoření fondu Batch"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "Vytvoření úlohy Batch"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "Přidání úkolů do úlohy"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "Sledování úkolů"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "Stažení výstupu úkolu ze služby Storage"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "Pracovní postup řešení Batch (úplný diagram)"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "Přihlašovací údaje Batch na portálu"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "Přihlašovací údaje služby Storage na portálu"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "Pracovní postup řešení Batch (minimální diagram)"
