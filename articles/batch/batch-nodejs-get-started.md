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
# <a name="get-started-with-batch-sdk-for-nodejs"></a>Začínáme se sadou SDK služby Batch pro Node.js

> [!div class="op_single_selector"]
> * [.NET](batch-dotnet-get-started.md)
> * [Python](batch-python-tutorial.md)
> * [Node.js](batch-nodejs-get-started.md)
>
>

Další hello základy vytváření Batch klienta v Node.js pomocí [Azure Batch Node.js SDK](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/). Pro pochopení scénáře pro aplikaci služby Batch si ho projdeme krok za krokem a pak aplikaci nastavíme pomocí klienta Node.js.  

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že máte praktické znalosti Node.js a umíte do jisté míry pracovat s Linuxem. Předpokládá také, že máte instalaci účet Azure pomocí služby Batch a Storage práva toocreate přístupu.

Doporučujeme, abyste čtení [technický přehled Azure Batch](batch-technical-overview.md) před projít hello kroků uvedených v tomto článku.

## <a name="hello-tutorial-scenario"></a>kurz scénář Hello
Dejte nám Pochopte scénář hello batch pracovního postupu. Máme jednoduchého skriptu napsané v Pythonu, který stahuje všechny csv souborů z kontejner úložiště objektů Blob v Azure a je převede tooJSON. tooprocess více úložiště účet kontejnery paralelně, můžeme nasadit hello skriptu jako úlohu služby Azure Batch.

## <a name="azure-batch-architecture"></a>Architektura služby Azure Batch
Hello následující diagram znázorňuje jak jsme můžete škálovat skript v jazyce Python hello pomocí Azure Batch a Node.js klienta.

![Scénář služby Azure Batch](./media/batch-nodejs-get-started/BatchScenario.png)

Klient node.js Hello nasadí dávkovou úlohu s přípravy úlohy (podrobně vysvětleny později) a sadu úloh v závislosti na hello počet kontejnerů v účtu úložiště hello. Hello skripty si můžete stáhnout z úložiště github hello.

* [Node.js](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/nodejs_batch_client_sample.js)
* [Skripty prostředí pro přípravný úkol](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/startup_prereq.sh)
* [Procesor tooJSON csv Python](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/processcsv.py)

> [!TIP]
> klientovi Node.js Hello v zadaný odkaz hello neobsahuje nasadit jako aplikaci Azure funkce toobe konkrétního kódu. Následující odkazy na pokyny toocreate jeden toohello lze odkazovat.
> - [Vytvoření aplikace Function App](../azure-functions/functions-create-first-azure-function.md)
> - [Vytvoření funkce pro aktivaci časovače](../azure-functions/functions-bindings-timer.md)
>
>

## <a name="build-hello-application"></a>Vytvoření aplikace hello

Nyní dejte nám postupujte podle procesu hello krok za krokem do vytváření hello Node.js klienta:

### <a name="step-1-install-azure-batch-sdk"></a>Krok 1: Nainstalování sady SDK služby Azure Batch

Můžete nainstalovat Azure Batch SDK pro Node.js pomocí hello npm install příkazu.

`npm install azure-batch`

Tento příkaz nainstaluje nejnovější verzi azure-batch uzlu SDK hello.

>[!Tip]
> V aplikaci funkce Azure, můžete přejít příliš nastavení "Kudu konzoly" v hello Azure funkce na kartě toorun hello npm nainstalujte příkazy. V této případu tooinstall Azure Batch SDK pro Node.js.
>
>

### <a name="step-2-create-an-azure-batch-account"></a>Krok 2: Vytvoření účtu Azure Batch

Můžete ho vytvořit z hello [portál Azure](batch-account-create-portal.md) nebo z příkazového řádku ([prostředí Powershell](batch-powershell-cmdlets-get-started.md) /[rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview)).

Následují hello příkazy toocreate jeden prostřednictvím rozhraní příkazového řádku Azure.

Vytvořte skupinu prostředků, tento krok přeskočte, pokud již účet máte místo toocreate hello účtu Batch:

`az group create -n "<resource-group-name>" -l "<location>"`

Dále vytvořte účet Azure Batch.

`az batch account create -l "<location>"  -g "<resource-group-name>" -n "<batch-account-name>"`

Každý účet Batch má odpovídající přístupové klíče. Tyto klíče jsou potřebné toocreate další prostředky v účtu Azure batch. Vhodné pro produkční prostředí je Azure Key Vault toostore toouse tyto klíče. Potom můžete vytvořit službu objektu zabezpečení pro aplikace hello. Pomocí této aplikace hello hlavní služby můžete vytvořit z trezoru klíčů hello klíče tooaccess tokenu OAuth.

`az batch account keys list -g "<resource-group-name>" -n "<batch-account-name>"`

Zkopírujte a uložte hello klíče toobe použit v následné kroky hello.

### <a name="step-3-create-an-azure-batch-service-client"></a>Krok 3: Vytvoření klienta služby Azure Batch
Následující fragment kódu nejprve importuje modul Node.js hello azure-batch a poté vytvoří služba Batch klienta. Je třeba toofirst vytvořit objekt SharedKeyCredentials s klíč účtu Batch hello zkopírovali v předchozím kroku hello.

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

Hello Azure Batch URI najdete v kartě Přehled hello hello portálu Azure. Je hello formátu:

`https://accountname.location.batch.azure.com`

Odkažte toohello – snímek obrazovky:

![Identifikátor URI služby Azure Batch](./media/batch-nodejs-get-started/azurebatchuri.png)



### <a name="step-4-create-an-azure-batch-pool"></a>Krok 4: Vytvoření fondu služby Azure Batch
Fond služby Azure Batch se skládá z několika virtuálních počítačů (označovaných také jako uzly služby Batch). Služba Azure Batch nasadí hello úlohy na tyto uzly a je spravuje. Můžete definovat hello následující parametry konfigurace pro váš fond.

* Typ image virtuálních počítačů
* Velikost uzlů virtuálních počítačů
* Počet uzlů virtuálních počítačů

> [!Tip]
> Hello velikost a počet uzlů virtuálního počítače do značné míry závisí na hello počet úloh, které chcete toorun v paralelní a také vlastní úloha hello. Doporučujeme, abyste testování toodetermine hello ideální počet a velikost.
>
>

Hello následující fragment kódu vytvoří hello objekty parametr konfigurace.

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
> Seznam hello k dispozici pro Azure Batch a jejich ID SKU bitových kopií virtuálního počítače s Linuxem najdete v tématu [seznam bitové kopie virtuálních počítačů](batch-linux-nodes.md#list-of-virtual-machine-images).
>
>

Po konfiguraci fondu hello je definována, můžete vytvořit fondu Azure Batch hello. Hello příkaz fondu Batch vytvoří virtuální počítač Azure uzly a připraví je toobe připraven tooreceive úlohy tooexecute. Každý fond musí mít jedinečné ID, abyste na něj mohli odkazovat v dalších krocích.

Následující fragment kódu Hello vytvoří fondu Azure Batch.

```nodejs
// Create a unique Azure Batch pool ID
var poolid = "pool" + customerDetails.customerid;
var poolConfig = {id:poolid, displayName:poolid,vmSize:vmSize,virtualMachineConfiguration:vmconfig,targetDedicatedComputeNodes:numVms,enableAutoScale:false };
// Creating hello Pool for hello specific customer
var pool = batch_client.pool.add(poolConfig,function(error,result){
    if(error!=null){console.log(error.response)};
});
```

Můžete zkontrolovat stav hello hello fond vytvořen a zajistěte, aby byl stav hello v "aktivní" před pokračovat odeslání úlohy toothat fondu.

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

Následuje ukázka výsledný objekt, který vrácené funkcí pool.get hello.

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


### <a name="step-4-submit-an-azure-batch-job"></a>Krok 4: Odeslání úlohy služby Azure Batch
Úloha služby Azure Batch je logická skupina podobných úkolů. V našem scénáři je "Proces csv tooJSON." Každý z těchto úkolů může zpracovávat soubory CSV v jednotlivých kontejnerech služby Azure Storage.

Tyto úlohy by spouští paralelně a nasazení ve více uzlech, řízená služby Azure Batch hello.

> [!Tip]
> Můžete použít hello [maxTasksPerNode](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) vlastnost toospecify maximální počet úkolů, které můžou běžet současně na jednom uzlu.
>
>

#### <a name="preparation-task"></a>Přípravný úkol

Hello virtuálních počítačů uzlů vytvořené jsou prázdné Ubuntu uzly. Často je nutné tooinstall sadu programy jako požadované součásti.
Obvykle pro uzly Linux můžete mít skript prostředí, který nainstaluje hello požadavky před hello skutečné úlohy spustit. Může se ale jednat o jakýkoli programovatelný spustitelný soubor.
Hello [prostředí shell skriptu](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/startup_prereq.sh) v tomto příkladu nainstaluje Python pip a hello sada SDK úložiště Azure pro jazyk Python.

Můžete nahrát hello skriptu na účet úložiště Azure a vygenerovat skript hello tooaccess identifikátor URI pro SAS. Tento proces je také možné automatizovat pomocí hello SDK pro Node.js úložiště Azure.

> [!Tip]
> Přípravy úlohy pro úlohu funguje pouze na uzly hello virtuálních počítačů, kdy je konkrétní úkol hello toorun. Pokud chcete, aby toobe požadované součásti nainstalované na všech uzlech bez ohledu na hello úlohy, které pro něj spustit, můžete použít hello [startTask](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) vlastnost při přidávání fondu. Můžete použít následující definici přípravy úlohy pro referenci hello.
>
>

Během odesílání hello úlohy Azure Batch je zadán přípravy úlohy. Následující jsou hello přípravy úlohy konfigurační parametry:

* **ID**: Jedinečný identifikátor hello přípravy úlohy
* **commandLine**: spustitelný soubor příkazového řádku tooexecute hello úlohy
* **resourceFiles**: pole objektů, které poskytují podrobnosti o souborech potřeby toobe stáhnout pro tento toorun úloh.  Následují jeho možnosti:
    - blobSource: hello identifikátor URI pro SAS hello souboru
    - Cesta k souboru: toodownload místní cestu a uložte soubor hello
    - fileMode: Režim souboru, který lze použít pouze pro uzly s Linuxem, v osmičkovém formátu a s výchozí hodnotou 0770.
* **waitForSuccess**: Pokud sada tootrue, hello úloh není spuštěna na selhání úkolů přípravy
* **runElevated**: nastavte ji tootrue, pokud zvýšená oprávnění jsou potřebné toorun hello úloh.

Následující fragment kódu ukazuje ukázka konfigurace hello přípravy úlohy skriptu:

```nodejs
var job_prep_task_config = {id:"installprereq",commandLine:"sudo sh startup_prereq.sh > startup.log",resourceFiles:[{'blobSource':'Blob SAS URI','filePath':'startup_prereq.sh'}],waitForSuccess:true,runElevated:true}
```

Pokud neexistují žádné požadavky toobe pro vaše úlohy toorun nainstalována, můžete přeskočit hello přípravných kroků. Následující kód vytvoří úlohu se zobrazovaným názvem „process csv files“.

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


### <a name="step-5-submit-azure-batch-tasks-for-a-job"></a>Krok 5: Odeslání úkolů služby Azure Batch pro úlohu

Nyní, když máme vytvořenou úlohu pro zpracování formátu CSV, vytvoříme pro tuto úlohu úkoly. Za předpokladu, že máme čtyři kontejnery, máme čtyři úlohy toocreate, jednu pro každý kontejner.

Pokud se podíváme na hello [skript v jazyce Python](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/processcsv.py), přijímá dva parametry:

* název kontejneru: hello soubory toodownload kontejneru úložiště z
* pattern: Volitelný parametr se vzorem názvu souboru.

Za předpokladu, že máme čtyři kontejnery "con1", "con2", "con3", "con4" následující kód ukazuje odesílání pro úlohy toohello Azure batch úlohy "proces csv" jsme vytvořili předtím.

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

Hello kód přidá více fondu toohello úlohy. A jednotlivé úlohy hello je provést na uzlu ve fondu hello virtuálních počítačů vytvořena. Pokud hello počet úloh překračuje hello počet virtuálních počítačů ve fondu nebo hello vlastnosti maxTasksPerNode, úlohy hello Počkejte, až uzel je k dispozici. Tuto orchestraci automaticky zařizuje služba Azure Batch.

portál Hello má podrobné zobrazení na hello úlohy a stavy úlohy. Můžete také použít hello seznamu a získat funkce v hello Azure SDK uzlu. Podrobnosti jsou uvedeny v dokumentaci hello [odkaz](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Job.html).

## <a name="next-steps"></a>Další kroky

- Zkontrolujte hello [funkcí přehled Azure Batch](batch-api-basics.md) článek, který doporučujeme, pokud jste novou službu toohello.
- V tématu hello [Batch Node.js odkaz](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/) tooexplore hello Batch API.

