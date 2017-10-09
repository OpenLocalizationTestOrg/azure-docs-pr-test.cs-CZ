---
title: "aaaTutorial - použití hello Azure Batch Klientská knihovna pro Node.js | Microsoft Docs"
description: "Informace hello základními koncepty Azure Batch a vytvoření jednoduché řešení pomocí Node.js."
services: batch
author: shwetams
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: nodejs
ms.topic: hero-article
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: shwetams
ms.openlocfilehash: d2b0ecbe764e7100affd7b02839aef3077b073cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-batch-sdk-for-nodejs"></a><span data-ttu-id="39a60-103">Začínáme se sadou SDK služby Batch pro Node.js</span><span class="sxs-lookup"><span data-stu-id="39a60-103">Get started with Batch SDK for Node.js</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="39a60-104">.NET</span><span class="sxs-lookup"><span data-stu-id="39a60-104">.NET</span></span>](batch-dotnet-get-started.md)
> * [<span data-ttu-id="39a60-105">Python</span><span class="sxs-lookup"><span data-stu-id="39a60-105">Python</span></span>](batch-python-tutorial.md)
> * [<span data-ttu-id="39a60-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="39a60-106">Node.js</span></span>](batch-nodejs-get-started.md)
>
>

<span data-ttu-id="39a60-107">Další hello základy vytváření Batch klienta v Node.js pomocí [Azure Batch Node.js SDK](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/).</span><span class="sxs-lookup"><span data-stu-id="39a60-107">Learn hello basics of building a Batch client in Node.js using [Azure Batch Node.js SDK](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/).</span></span> <span data-ttu-id="39a60-108">Pro pochopení scénáře pro aplikaci služby Batch si ho projdeme krok za krokem a pak aplikaci nastavíme pomocí klienta Node.js.</span><span class="sxs-lookup"><span data-stu-id="39a60-108">We take a step by step approach of understanding a scenario for a batch application and then setting it up using a Node.js client.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="39a60-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="39a60-109">Prerequisites</span></span>
<span data-ttu-id="39a60-110">Tento článek předpokládá, že máte praktické znalosti Node.js a umíte do jisté míry pracovat s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="39a60-110">This article assumes that you have a working knowledge of Node.js and familiarity with Linux.</span></span> <span data-ttu-id="39a60-111">Předpokládá také, že máte instalaci účet Azure pomocí služby Batch a Storage práva toocreate přístupu.</span><span class="sxs-lookup"><span data-stu-id="39a60-111">It also assumes that you have an Azure account setup with access rights toocreate Batch and Storage services.</span></span>

<span data-ttu-id="39a60-112">Doporučujeme, abyste čtení [technický přehled Azure Batch](batch-technical-overview.md) před projít hello kroků uvedených v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="39a60-112">We recommend reading [Azure Batch Technical Overview](batch-technical-overview.md) before you go through hello steps outlined this article.</span></span>

## <a name="hello-tutorial-scenario"></a><span data-ttu-id="39a60-113">kurz scénář Hello</span><span class="sxs-lookup"><span data-stu-id="39a60-113">hello tutorial scenario</span></span>
<span data-ttu-id="39a60-114">Dejte nám Pochopte scénář hello batch pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="39a60-114">Let us understand hello batch workflow scenario.</span></span> <span data-ttu-id="39a60-115">Máme jednoduchého skriptu napsané v Pythonu, který stahuje všechny csv souborů z kontejner úložiště objektů Blob v Azure a je převede tooJSON.</span><span class="sxs-lookup"><span data-stu-id="39a60-115">We have a simple script written in Python that downloads all csv files from an Azure Blob storage container and converts them tooJSON.</span></span> <span data-ttu-id="39a60-116">tooprocess více úložiště účet kontejnery paralelně, můžeme nasadit hello skriptu jako úlohu služby Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="39a60-116">tooprocess multiple storage account containers in parallel, we can deploy hello script as an Azure Batch job.</span></span>

## <a name="azure-batch-architecture"></a><span data-ttu-id="39a60-117">Architektura služby Azure Batch</span><span class="sxs-lookup"><span data-stu-id="39a60-117">Azure Batch Architecture</span></span>
<span data-ttu-id="39a60-118">Hello následující diagram znázorňuje jak jsme můžete škálovat skript v jazyce Python hello pomocí Azure Batch a Node.js klienta.</span><span class="sxs-lookup"><span data-stu-id="39a60-118">hello following diagram depicts how we can scale hello Python script using Azure Batch and a Node.js client.</span></span>

![Scénář služby Azure Batch](./media/batch-nodejs-get-started/BatchScenario.png)

<span data-ttu-id="39a60-120">Klient node.js Hello nasadí dávkovou úlohu s přípravy úlohy (podrobně vysvětleny později) a sadu úloh v závislosti na hello počet kontejnerů v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="39a60-120">hello node.js client deploys a batch job with a preparation task (explained in detail later) and a set of tasks depending on hello number of containers in hello storage account.</span></span> <span data-ttu-id="39a60-121">Hello skripty si můžete stáhnout z úložiště github hello.</span><span class="sxs-lookup"><span data-stu-id="39a60-121">You can download hello scripts from hello github repository.</span></span>

* [<span data-ttu-id="39a60-122">Node.js</span><span class="sxs-lookup"><span data-stu-id="39a60-122">Node.js client</span></span>](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/nodejs_batch_client_sample.js)
* [<span data-ttu-id="39a60-123">Skripty prostředí pro přípravný úkol</span><span class="sxs-lookup"><span data-stu-id="39a60-123">Preparation task shell scripts</span></span>](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/startup_prereq.sh)
* [<span data-ttu-id="39a60-124">Procesor tooJSON csv Python</span><span class="sxs-lookup"><span data-stu-id="39a60-124">Python csv tooJSON processor</span></span>](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/processcsv.py)

> [!TIP]
> <span data-ttu-id="39a60-125">klientovi Node.js Hello v zadaný odkaz hello neobsahuje nasadit jako aplikaci Azure funkce toobe konkrétního kódu.</span><span class="sxs-lookup"><span data-stu-id="39a60-125">hello Node.js client in hello link specified does not contain specific code toobe deployed as an Azure function app.</span></span> <span data-ttu-id="39a60-126">Následující odkazy na pokyny toocreate jeden toohello lze odkazovat.</span><span class="sxs-lookup"><span data-stu-id="39a60-126">You can refer toohello following links for instructions toocreate one.</span></span>
> - [<span data-ttu-id="39a60-127">Vytvoření aplikace Function App</span><span class="sxs-lookup"><span data-stu-id="39a60-127">Create function app</span></span>](../azure-functions/functions-create-first-azure-function.md)
> - [<span data-ttu-id="39a60-128">Vytvoření funkce pro aktivaci časovače</span><span class="sxs-lookup"><span data-stu-id="39a60-128">Create timer trigger function</span></span>](../azure-functions/functions-bindings-timer.md)
>
>

## <a name="build-hello-application"></a><span data-ttu-id="39a60-129">Vytvoření aplikace hello</span><span class="sxs-lookup"><span data-stu-id="39a60-129">Build hello application</span></span>

<span data-ttu-id="39a60-130">Nyní dejte nám postupujte podle procesu hello krok za krokem do vytváření hello Node.js klienta:</span><span class="sxs-lookup"><span data-stu-id="39a60-130">Now, let us follow hello process step by step into building hello Node.js client:</span></span>

### <a name="step-1-install-azure-batch-sdk"></a><span data-ttu-id="39a60-131">Krok 1: Nainstalování sady SDK služby Azure Batch</span><span class="sxs-lookup"><span data-stu-id="39a60-131">Step 1: Install Azure Batch SDK</span></span>

<span data-ttu-id="39a60-132">Můžete nainstalovat Azure Batch SDK pro Node.js pomocí hello npm install příkazu.</span><span class="sxs-lookup"><span data-stu-id="39a60-132">You can install Azure Batch SDK for Node.js using hello npm install command.</span></span>

`npm install azure-batch`

<span data-ttu-id="39a60-133">Tento příkaz nainstaluje nejnovější verzi azure-batch uzlu SDK hello.</span><span class="sxs-lookup"><span data-stu-id="39a60-133">This command installs hello latest version of azure-batch node SDK.</span></span>

>[!Tip]
> <span data-ttu-id="39a60-134">V aplikaci funkce Azure, můžete přejít příliš nastavení "Kudu konzoly" v hello Azure funkce na kartě toorun hello npm nainstalujte příkazy.</span><span class="sxs-lookup"><span data-stu-id="39a60-134">In an Azure Function app, you can go too"Kudu Console" in hello Azure function's Settings tab toorun hello npm install commands.</span></span> <span data-ttu-id="39a60-135">V této případu tooinstall Azure Batch SDK pro Node.js.</span><span class="sxs-lookup"><span data-stu-id="39a60-135">In this case tooinstall Azure Batch SDK for Node.js.</span></span>
>
>

### <a name="step-2-create-an-azure-batch-account"></a><span data-ttu-id="39a60-136">Krok 2: Vytvoření účtu Azure Batch</span><span class="sxs-lookup"><span data-stu-id="39a60-136">Step 2: Create an Azure Batch account</span></span>

<span data-ttu-id="39a60-137">Můžete ho vytvořit z hello [portál Azure](batch-account-create-portal.md) nebo z příkazového řádku ([prostředí Powershell](batch-powershell-cmdlets-get-started.md) /[rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview)).</span><span class="sxs-lookup"><span data-stu-id="39a60-137">You can create it from hello [Azure portal](batch-account-create-portal.md) or from command line ([Powershell](batch-powershell-cmdlets-get-started.md) /[Azure cli](https://docs.microsoft.com/cli/azure/overview)).</span></span>

<span data-ttu-id="39a60-138">Následují hello příkazy toocreate jeden prostřednictvím rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="39a60-138">Following are hello commands toocreate one through Azure CLI.</span></span>

<span data-ttu-id="39a60-139">Vytvořte skupinu prostředků, tento krok přeskočte, pokud již účet máte místo toocreate hello účtu Batch:</span><span class="sxs-lookup"><span data-stu-id="39a60-139">Create a Resource Group, skip this step if you already have one where you want toocreate hello Batch Account:</span></span>

`az group create -n "<resource-group-name>" -l "<location>"`

<span data-ttu-id="39a60-140">Dále vytvořte účet Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="39a60-140">Next, create an Azure Batch account.</span></span>

`az batch account create -l "<location>"  -g "<resource-group-name>" -n "<batch-account-name>"`

<span data-ttu-id="39a60-141">Každý účet Batch má odpovídající přístupové klíče.</span><span class="sxs-lookup"><span data-stu-id="39a60-141">Each Batch account has its corresponding access keys.</span></span> <span data-ttu-id="39a60-142">Tyto klíče jsou potřebné toocreate další prostředky v účtu Azure batch.</span><span class="sxs-lookup"><span data-stu-id="39a60-142">These keys are needed toocreate further resources in Azure batch account.</span></span> <span data-ttu-id="39a60-143">Vhodné pro produkční prostředí je Azure Key Vault toostore toouse tyto klíče.</span><span class="sxs-lookup"><span data-stu-id="39a60-143">A good practice for production environment is toouse Azure Key Vault toostore these keys.</span></span> <span data-ttu-id="39a60-144">Potom můžete vytvořit službu objektu zabezpečení pro aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="39a60-144">You can then create a Service principal for hello application.</span></span> <span data-ttu-id="39a60-145">Pomocí této aplikace hello hlavní služby můžete vytvořit z trezoru klíčů hello klíče tooaccess tokenu OAuth.</span><span class="sxs-lookup"><span data-stu-id="39a60-145">Using this service principal hello application can create an OAuth token tooaccess keys from hello key vault.</span></span>

`az batch account keys list -g "<resource-group-name>" -n "<batch-account-name>"`

<span data-ttu-id="39a60-146">Zkopírujte a uložte hello klíče toobe použit v následné kroky hello.</span><span class="sxs-lookup"><span data-stu-id="39a60-146">Copy and store hello key toobe used in hello subsequent steps.</span></span>

### <a name="step-3-create-an-azure-batch-service-client"></a><span data-ttu-id="39a60-147">Krok 3: Vytvoření klienta služby Azure Batch</span><span class="sxs-lookup"><span data-stu-id="39a60-147">Step 3: Create an Azure Batch service client</span></span>
<span data-ttu-id="39a60-148">Následující fragment kódu nejprve importuje modul Node.js hello azure-batch a poté vytvoří služba Batch klienta.</span><span class="sxs-lookup"><span data-stu-id="39a60-148">Following code snippet first imports hello azure-batch Node.js module and then creates a Batch Service client.</span></span> <span data-ttu-id="39a60-149">Je třeba toofirst vytvořit objekt SharedKeyCredentials s klíč účtu Batch hello zkopírovali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="39a60-149">You need toofirst create a SharedKeyCredentials object with hello Batch account key copied from hello previous step.</span></span>

```nodejs
// Initializing Azure Batch variables

var batch = require('azure-batch');

var accountName = '<azure-batch-account-name>';

var accountKey = '<account-key-downloaded>';

var accountUrl = '<account-url>'

// Create Batch credentials object using account name and account key

var credentials = new batch.SharedKeyCredentials(accountName,accountKey);

// Create Batch service client

var batch_client = new batch.ServiceClient(credentials,accountUrl);

```

<span data-ttu-id="39a60-150">Hello Azure Batch URI najdete v kartě Přehled hello hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="39a60-150">hello Azure Batch URI can be found in hello Overview tab of hello Azure portal.</span></span> <span data-ttu-id="39a60-151">Je hello formátu:</span><span class="sxs-lookup"><span data-stu-id="39a60-151">It is of hello format:</span></span>

`https://accountname.location.batch.azure.com`

<span data-ttu-id="39a60-152">Odkažte toohello – snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="39a60-152">Refer toohello screenshot:</span></span>

![Identifikátor URI služby Azure Batch](./media/batch-nodejs-get-started/azurebatchuri.png)



### <a name="step-4-create-an-azure-batch-pool"></a><span data-ttu-id="39a60-154">Krok 4: Vytvoření fondu služby Azure Batch</span><span class="sxs-lookup"><span data-stu-id="39a60-154">Step 4: Create an Azure Batch pool</span></span>
<span data-ttu-id="39a60-155">Fond služby Azure Batch se skládá z několika virtuálních počítačů (označovaných také jako uzly služby Batch).</span><span class="sxs-lookup"><span data-stu-id="39a60-155">An Azure Batch pool consists of multiple VMs (also known as Batch Nodes).</span></span> <span data-ttu-id="39a60-156">Služba Azure Batch nasadí hello úlohy na tyto uzly a je spravuje.</span><span class="sxs-lookup"><span data-stu-id="39a60-156">Azure Batch service deploys hello tasks on these nodes and manages them.</span></span> <span data-ttu-id="39a60-157">Můžete definovat hello následující parametry konfigurace pro váš fond.</span><span class="sxs-lookup"><span data-stu-id="39a60-157">You can define hello following configuration parameters for your pool.</span></span>

* <span data-ttu-id="39a60-158">Typ image virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="39a60-158">Type of Virtual Machine image</span></span>
* <span data-ttu-id="39a60-159">Velikost uzlů virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="39a60-159">Size of Virtual Machine nodes</span></span>
* <span data-ttu-id="39a60-160">Počet uzlů virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="39a60-160">Number of Virtual Machine nodes</span></span>

> [!Tip]
> <span data-ttu-id="39a60-161">Hello velikost a počet uzlů virtuálního počítače do značné míry závisí na hello počet úloh, které chcete toorun v paralelní a také vlastní úloha hello.</span><span class="sxs-lookup"><span data-stu-id="39a60-161">hello size and number of Virtual Machine nodes largely depend on hello number of tasks you want toorun in parallel and also hello task itself.</span></span> <span data-ttu-id="39a60-162">Doporučujeme, abyste testování toodetermine hello ideální počet a velikost.</span><span class="sxs-lookup"><span data-stu-id="39a60-162">We recommend testing toodetermine hello ideal number and size.</span></span>
>
>

<span data-ttu-id="39a60-163">Hello následující fragment kódu vytvoří hello objekty parametr konfigurace.</span><span class="sxs-lookup"><span data-stu-id="39a60-163">hello following code snippet creates hello configuration parameter objects.</span></span>

```nodejs
// Creating Image reference configuration for Ubuntu Linux VM
var imgRef = {publisher:"Canonical",offer:"UbuntuServer",sku:"14.04.2-LTS",version:"latest"}

// Creating hello VM configuration object with hello SKUID
var vmconfig = {imageReference:imgRef,nodeAgentSKUId:"batch.node.ubuntu 14.04"}

// Setting hello VM size tooStandard F4
var vmSize = "STANDARD_F4"

//Setting number of VMs in hello pool too4
var numVMs = 4
```

> [!Tip]
> <span data-ttu-id="39a60-164">Seznam hello k dispozici pro Azure Batch a jejich ID SKU bitových kopií virtuálního počítače s Linuxem najdete v tématu [seznam bitové kopie virtuálních počítačů](batch-linux-nodes.md#list-of-virtual-machine-images).</span><span class="sxs-lookup"><span data-stu-id="39a60-164">For hello list of Linux VM images available for Azure Batch and their SKU IDs, see [List of virtual machine images](batch-linux-nodes.md#list-of-virtual-machine-images).</span></span>
>
>

<span data-ttu-id="39a60-165">Po konfiguraci fondu hello je definována, můžete vytvořit fondu Azure Batch hello.</span><span class="sxs-lookup"><span data-stu-id="39a60-165">Once hello pool configuration is defined, you can create hello Azure Batch pool.</span></span> <span data-ttu-id="39a60-166">Hello příkaz fondu Batch vytvoří virtuální počítač Azure uzly a připraví je toobe připraven tooreceive úlohy tooexecute.</span><span class="sxs-lookup"><span data-stu-id="39a60-166">hello Batch pool command creates Azure Virtual Machine nodes and prepares them toobe ready tooreceive tasks tooexecute.</span></span> <span data-ttu-id="39a60-167">Každý fond musí mít jedinečné ID, abyste na něj mohli odkazovat v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="39a60-167">Each pool should have a unique ID for reference in subsequent steps.</span></span>

<span data-ttu-id="39a60-168">Následující fragment kódu Hello vytvoří fondu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="39a60-168">hello following code snippet creates an Azure Batch pool.</span></span>

```nodejs
// Create a unique Azure Batch pool ID
var poolid = "pool" + customerDetails.customerid;
var poolConfig = {id:poolid, displayName:poolid,vmSize:vmSize,virtualMachineConfiguration:vmconfig,targetDedicatedComputeNodes:numVms,enableAutoScale:false };
// Creating hello Pool for hello specific customer
var pool = batch_client.pool.add(poolConfig,function(error,result){
    if(error!=null){console.log(error.response)};
});
```

<span data-ttu-id="39a60-169">Můžete zkontrolovat stav hello hello fond vytvořen a zajistěte, aby byl stav hello v "aktivní" před pokračovat odeslání úlohy toothat fondu.</span><span class="sxs-lookup"><span data-stu-id="39a60-169">You can check hello status of hello pool created and ensure that hello state is in "active" before going ahead with submission of a Job toothat pool.</span></span>

```nodejs
var cloudPool = batch_client.pool.get(poolid,function(error,result,request,response){
        if(error == null)
        {

            if(result.state == "active")
            {
                console.log("Pool is active");
            }
        }
        else
        {
            if(error.statusCode==404)
            {
                console.log("Pool not found yet returned 404...");    

            }
            else
            {
                console.log("Error occurred while retrieving pool data");
            }
        }
        });
```

<span data-ttu-id="39a60-170">Následuje ukázka výsledný objekt, který vrácené funkcí pool.get hello.</span><span class="sxs-lookup"><span data-stu-id="39a60-170">Following is a sample result object returned by hello pool.get function.</span></span>

```
{ id: 'processcsv_201721152',
  displayName: 'processcsv_201721152',
  url: 'https://<batch-account-name>.centralus.batch.azure.com/pools/processcsv_201721152',
  eTag: '<eTag>',
  lastModified: 2017-03-27T10:28:02.398Z,
  creationTime: 2017-03-27T10:28:02.398Z,
  state: 'active',
  stateTransitionTime: 2017-03-27T10:28:02.398Z,
  allocationState: 'resizing',
  allocationStateTransitionTime: 2017-03-27T10:28:02.398Z,
  vmSize: 'standard_a1',
  virtualMachineConfiguration:
   { imageReference:
      { publisher: 'Canonical',
        offer: 'UbuntuServer',
        sku: '14.04.2-LTS',
        version: 'latest' },
     nodeAgentSKUId: 'batch.node.ubuntu 14.04' },
  resizeTimeout:
   { [Number: 900000]
     _milliseconds: 900000,
     _days: 0,
     _months: 0,
     _data:
      { milliseconds: 0,
        seconds: 0,
        minutes: 15,
        hours: 0,
        days: 0,
        months: 0,
        years: 0 },
     _locale:
      Locale {
        _calendar: [Object],
        _longDateFormat: [Object],
        _invalidDate: 'Invalid date',
        ordinal: [Function: ordinal],
        _ordinalParse: /\d{1,2}(th|st|nd|rd)/,
        _relativeTime: [Object],
        _months: [Object],
        _monthsShort: [Object],
        _week: [Object],
        _weekdays: [Object],
        _weekdaysMin: [Object],
        _weekdaysShort: [Object],
        _meridiemParse: /[ap]\.?m?\.?/i,
        _abbr: 'en',
        _config: [Object],
        _ordinalParseLenient: /\d{1,2}(th|st|nd|rd)|\d{1,2}/ } },
  currentDedicated: 0,
  targetDedicated: 4,
  enableAutoScale: false,
  enableInterNodeCommunication: false,
  maxTasksPerNode: 1,
  taskSchedulingPolicy: { nodeFillType: 'Spread' } }
```


### <a name="step-4-submit-an-azure-batch-job"></a><span data-ttu-id="39a60-171">Krok 4: Odeslání úlohy služby Azure Batch</span><span class="sxs-lookup"><span data-stu-id="39a60-171">Step 4: Submit an Azure Batch job</span></span>
<span data-ttu-id="39a60-172">Úloha služby Azure Batch je logická skupina podobných úkolů.</span><span class="sxs-lookup"><span data-stu-id="39a60-172">An Azure Batch job is a logical group of similar tasks.</span></span> <span data-ttu-id="39a60-173">V našem scénáři je "Proces csv tooJSON."</span><span class="sxs-lookup"><span data-stu-id="39a60-173">In our scenario, it is "Process csv tooJSON."</span></span> <span data-ttu-id="39a60-174">Každý z těchto úkolů může zpracovávat soubory CSV v jednotlivých kontejnerech služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="39a60-174">Each task here could be processing csv files present in each Azure Storage container.</span></span>

<span data-ttu-id="39a60-175">Tyto úlohy by spouští paralelně a nasazení ve více uzlech, řízená služby Azure Batch hello.</span><span class="sxs-lookup"><span data-stu-id="39a60-175">These tasks would run in parallel and deployed across multiple nodes, orchestrated by hello Azure Batch service.</span></span>

> [!Tip]
> <span data-ttu-id="39a60-176">Můžete použít hello [maxTasksPerNode](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) vlastnost toospecify maximální počet úkolů, které můžou běžet současně na jednom uzlu.</span><span class="sxs-lookup"><span data-stu-id="39a60-176">You can use hello [maxTasksPerNode](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) property toospecify maximum number of tasks that can run concurrently on a single node.</span></span>
>
>

#### <a name="preparation-task"></a><span data-ttu-id="39a60-177">Přípravný úkol</span><span class="sxs-lookup"><span data-stu-id="39a60-177">Preparation task</span></span>

<span data-ttu-id="39a60-178">Hello virtuálních počítačů uzlů vytvořené jsou prázdné Ubuntu uzly.</span><span class="sxs-lookup"><span data-stu-id="39a60-178">hello VM nodes created are blank Ubuntu nodes.</span></span> <span data-ttu-id="39a60-179">Často je nutné tooinstall sadu programy jako požadované součásti.</span><span class="sxs-lookup"><span data-stu-id="39a60-179">Often, you need tooinstall a set of programs as prerequisites.</span></span>
<span data-ttu-id="39a60-180">Obvykle pro uzly Linux můžete mít skript prostředí, který nainstaluje hello požadavky před hello skutečné úlohy spustit.</span><span class="sxs-lookup"><span data-stu-id="39a60-180">Typically, for Linux nodes you can have a shell script that installs hello prerequisites before hello actual tasks run.</span></span> <span data-ttu-id="39a60-181">Může se ale jednat o jakýkoli programovatelný spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="39a60-181">However it could be any programmable executable.</span></span>
<span data-ttu-id="39a60-182">Hello [prostředí shell skriptu](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/startup_prereq.sh) v tomto příkladu nainstaluje Python pip a hello sada SDK úložiště Azure pro jazyk Python.</span><span class="sxs-lookup"><span data-stu-id="39a60-182">hello [shell script](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/startup_prereq.sh) in this example installs Python-pip and hello Azure Storage SDK for Python.</span></span>

<span data-ttu-id="39a60-183">Můžete nahrát hello skriptu na účet úložiště Azure a vygenerovat skript hello tooaccess identifikátor URI pro SAS.</span><span class="sxs-lookup"><span data-stu-id="39a60-183">You can upload hello script on an Azure Storage Account and generate a SAS URI tooaccess hello script.</span></span> <span data-ttu-id="39a60-184">Tento proces je také možné automatizovat pomocí hello SDK pro Node.js úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="39a60-184">This process can also be automated using hello Azure Storage Node.js SDK.</span></span>

> [!Tip]
> <span data-ttu-id="39a60-185">Přípravy úlohy pro úlohu funguje pouze na uzly hello virtuálních počítačů, kdy je konkrétní úkol hello toorun.</span><span class="sxs-lookup"><span data-stu-id="39a60-185">A preparation task for a job runs only on hello VM nodes where hello specific task needs toorun.</span></span> <span data-ttu-id="39a60-186">Pokud chcete, aby toobe požadované součásti nainstalované na všech uzlech bez ohledu na hello úlohy, které pro něj spustit, můžete použít hello [startTask](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) vlastnost při přidávání fondu.</span><span class="sxs-lookup"><span data-stu-id="39a60-186">If you want prerequisites toobe installed on all nodes irrespective of hello tasks that run on it, you can use hello [startTask](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) property while adding a pool.</span></span> <span data-ttu-id="39a60-187">Můžete použít následující definici přípravy úlohy pro referenci hello.</span><span class="sxs-lookup"><span data-stu-id="39a60-187">You can use hello following preparation task definition for reference.</span></span>
>
>

<span data-ttu-id="39a60-188">Během odesílání hello úlohy Azure Batch je zadán přípravy úlohy.</span><span class="sxs-lookup"><span data-stu-id="39a60-188">A preparation task is specified during hello submission of Azure Batch job.</span></span> <span data-ttu-id="39a60-189">Následující jsou hello přípravy úlohy konfigurační parametry:</span><span class="sxs-lookup"><span data-stu-id="39a60-189">Following are hello preparation task configuration parameters:</span></span>

* <span data-ttu-id="39a60-190">**ID**: Jedinečný identifikátor hello přípravy úlohy</span><span class="sxs-lookup"><span data-stu-id="39a60-190">**ID**: A unique identifier for hello preparation task</span></span>
* <span data-ttu-id="39a60-191">**commandLine**: spustitelný soubor příkazového řádku tooexecute hello úlohy</span><span class="sxs-lookup"><span data-stu-id="39a60-191">**commandLine**: Command line tooexecute hello task executable</span></span>
* <span data-ttu-id="39a60-192">**resourceFiles**: pole objektů, které poskytují podrobnosti o souborech potřeby toobe stáhnout pro tento toorun úloh.</span><span class="sxs-lookup"><span data-stu-id="39a60-192">**resourceFiles**: Array of objects that provide details of files needed toobe downloaded for this task toorun.</span></span>  <span data-ttu-id="39a60-193">Následují jeho možnosti:</span><span class="sxs-lookup"><span data-stu-id="39a60-193">Following are its options</span></span>
    - <span data-ttu-id="39a60-194">blobSource: hello identifikátor URI pro SAS hello souboru</span><span class="sxs-lookup"><span data-stu-id="39a60-194">blobSource: hello SAS URI of hello file</span></span>
    - <span data-ttu-id="39a60-195">Cesta k souboru: toodownload místní cestu a uložte soubor hello</span><span class="sxs-lookup"><span data-stu-id="39a60-195">filePath: Local path toodownload and save hello file</span></span>
    - <span data-ttu-id="39a60-196">fileMode: Režim souboru, který lze použít pouze pro uzly s Linuxem, v osmičkovém formátu a s výchozí hodnotou 0770.</span><span class="sxs-lookup"><span data-stu-id="39a60-196">fileMode: Only applicable for Linux nodes, fileMode is in octal format with a default value of 0770</span></span>
* <span data-ttu-id="39a60-197">**waitForSuccess**: Pokud sada tootrue, hello úloh není spuštěna na selhání úkolů přípravy</span><span class="sxs-lookup"><span data-stu-id="39a60-197">**waitForSuccess**: If set tootrue, hello task does not run on preparation task failures</span></span>
* <span data-ttu-id="39a60-198">**runElevated**: nastavte ji tootrue, pokud zvýšená oprávnění jsou potřebné toorun hello úloh.</span><span class="sxs-lookup"><span data-stu-id="39a60-198">**runElevated**: Set it tootrue if elevated privileges are needed toorun hello task.</span></span>

<span data-ttu-id="39a60-199">Následující fragment kódu ukazuje ukázka konfigurace hello přípravy úlohy skriptu:</span><span class="sxs-lookup"><span data-stu-id="39a60-199">Following code snippet shows hello preparation task script configuration sample:</span></span>

```nodejs
var job_prep_task_config = {id:"installprereq",commandLine:"sudo sh startup_prereq.sh > startup.log",resourceFiles:[{'blobSource':'Blob SAS URI','filePath':'startup_prereq.sh'}],waitForSuccess:true,runElevated:true}
```

<span data-ttu-id="39a60-200">Pokud neexistují žádné požadavky toobe pro vaše úlohy toorun nainstalována, můžete přeskočit hello přípravných kroků.</span><span class="sxs-lookup"><span data-stu-id="39a60-200">If there are no prerequisites toobe installed for your tasks toorun, you can skip hello preparation tasks.</span></span> <span data-ttu-id="39a60-201">Následující kód vytvoří úlohu se zobrazovaným názvem „process csv files“.</span><span class="sxs-lookup"><span data-stu-id="39a60-201">Following code creates a job with display name "process csv files."</span></span>

 ```nodejs
 // Setting up Batch pool configuration
 var pool_config = {poolId:poolid}
 // Setting up Job configuration along with preparation task
 var jobId = "processcsvjob"
 var job_config = {id:jobId,displayName:"process csv files",jobPreparationTask:job_prep_task_config,poolInfo:pool_config}
 // Adding Azure batch job toohello pool
 var job = batch_client.job.add(job_config,function(error,result){
     if(error != null)
     {
         console.log("Error submitting job : " + error.response);
     }});
```


### <a name="step-5-submit-azure-batch-tasks-for-a-job"></a><span data-ttu-id="39a60-202">Krok 5: Odeslání úkolů služby Azure Batch pro úlohu</span><span class="sxs-lookup"><span data-stu-id="39a60-202">Step 5: Submit Azure Batch tasks for a job</span></span>

<span data-ttu-id="39a60-203">Nyní, když máme vytvořenou úlohu pro zpracování formátu CSV, vytvoříme pro tuto úlohu úkoly.</span><span class="sxs-lookup"><span data-stu-id="39a60-203">Now that our process csv job is created, let us create tasks for that job.</span></span> <span data-ttu-id="39a60-204">Za předpokladu, že máme čtyři kontejnery, máme čtyři úlohy toocreate, jednu pro každý kontejner.</span><span class="sxs-lookup"><span data-stu-id="39a60-204">Assuming we have four containers, we have toocreate four tasks, one for each container.</span></span>

<span data-ttu-id="39a60-205">Pokud se podíváme na hello [skript v jazyce Python](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/processcsv.py), přijímá dva parametry:</span><span class="sxs-lookup"><span data-stu-id="39a60-205">If we look at hello [Python script](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/processcsv.py), it accepts two parameters:</span></span>

* <span data-ttu-id="39a60-206">název kontejneru: hello soubory toodownload kontejneru úložiště z</span><span class="sxs-lookup"><span data-stu-id="39a60-206">container name: hello Storage container toodownload files from</span></span>
* <span data-ttu-id="39a60-207">pattern: Volitelný parametr se vzorem názvu souboru.</span><span class="sxs-lookup"><span data-stu-id="39a60-207">pattern: An optional parameter of file name pattern</span></span>

<span data-ttu-id="39a60-208">Za předpokladu, že máme čtyři kontejnery "con1", "con2", "con3", "con4" následující kód ukazuje odesílání pro úlohy toohello Azure batch úlohy "proces csv" jsme vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="39a60-208">Assuming we have four containers "con1", "con2", "con3","con4" following code shows submitting for tasks toohello Azure batch job "process csv" we created earlier.</span></span>

```nodejs
// storing container names in an array
var container_list = ["con1","con2","con3","con4"]
    container_list.forEach(function(val,index){           

           var container_name = val;
           var taskID = container_name + "_process";
           var task_config = {id:taskID,displayName:'process csv in ' + container_name,commandLine:'python processcsv.py --container ' + container_name,resourceFiles:[{'blobSource':'<blob SAS URI>','filePath':'processcsv.py'}]}
           var task = batch_client.task.add(poolid,task_config,function(error,result){
                if(error != null)
                {
                    console.log(error.response);     
                }
                else
                {
                    console.log("Task for container : " + container_name + "submitted successfully");
                }



           });

    });
```

<span data-ttu-id="39a60-209">Hello kód přidá více fondu toohello úlohy.</span><span class="sxs-lookup"><span data-stu-id="39a60-209">hello code adds multiple tasks toohello pool.</span></span> <span data-ttu-id="39a60-210">A jednotlivé úlohy hello je provést na uzlu ve fondu hello virtuálních počítačů vytvořena.</span><span class="sxs-lookup"><span data-stu-id="39a60-210">And each of hello tasks is executed on a node in hello pool of VMs created.</span></span> <span data-ttu-id="39a60-211">Pokud hello počet úloh překračuje hello počet virtuálních počítačů ve fondu nebo hello vlastnosti maxTasksPerNode, úlohy hello Počkejte, až uzel je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="39a60-211">If hello number of tasks exceeds hello number of VMs in a pool or hello maxTasksPerNode property, hello tasks wait until a node is made available.</span></span> <span data-ttu-id="39a60-212">Tuto orchestraci automaticky zařizuje služba Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="39a60-212">This orchestration is handled by Azure Batch automatically.</span></span>

<span data-ttu-id="39a60-213">portál Hello má podrobné zobrazení na hello úlohy a stavy úlohy.</span><span class="sxs-lookup"><span data-stu-id="39a60-213">hello portal has detailed views on hello tasks and job statuses.</span></span> <span data-ttu-id="39a60-214">Můžete také použít hello seznamu a získat funkce v hello Azure SDK uzlu.</span><span class="sxs-lookup"><span data-stu-id="39a60-214">You can also use hello list and get functions in hello Azure Node SDK.</span></span> <span data-ttu-id="39a60-215">Podrobnosti jsou uvedeny v dokumentaci hello [odkaz](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Job.html).</span><span class="sxs-lookup"><span data-stu-id="39a60-215">Details are provided in hello documentation [link](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Job.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="39a60-216">Další kroky</span><span class="sxs-lookup"><span data-stu-id="39a60-216">Next steps</span></span>

- <span data-ttu-id="39a60-217">Zkontrolujte hello [funkcí přehled Azure Batch](batch-api-basics.md) článek, který doporučujeme, pokud jste novou službu toohello.</span><span class="sxs-lookup"><span data-stu-id="39a60-217">Review hello [Overview of Azure Batch features](batch-api-basics.md) article, which we recommend if you're new toohello service.</span></span>
- <span data-ttu-id="39a60-218">V tématu hello [Batch Node.js odkaz](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/) tooexplore hello Batch API.</span><span class="sxs-lookup"><span data-stu-id="39a60-218">See hello [Batch Node.js reference](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/) tooexplore hello Batch API.</span></span>

