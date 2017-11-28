---
title: "aaaManage Azure Data Lake Analytics pomocí Python | Microsoft Docs"
description: "Zjistěte, jak toouse Python toocreate Data Lake ukládání účtu a odesílání úloh. "
services: data-lake-analytics
documentationcenter: 
author: matt1883
manager: jhubbard
editor: cgronlun
ms.assetid: d4213a19-4d0f-49c9-871c-9cd6ed7cf731
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: saveenr
ms.openlocfilehash: 3c0fff155db7c4fd4e84c2562816995eb156be16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-using-python"></a><span data-ttu-id="03720-103">Správa Azure Data Lake Analytics používá Python</span><span class="sxs-lookup"><span data-stu-id="03720-103">Manage Azure Data Lake Analytics using Python</span></span>

## <a name="python-versions"></a><span data-ttu-id="03720-104">Verze jazyka Python</span><span class="sxs-lookup"><span data-stu-id="03720-104">Python versions</span></span>

* <span data-ttu-id="03720-105">Použijte 64bitovou verzi jazyka Python.</span><span class="sxs-lookup"><span data-stu-id="03720-105">Use a 64-bit version of Python.</span></span>
* <span data-ttu-id="03720-106">Můžete použít standardní distribuci jazyka Python hello nalezený na  **[Python.org stáhne](https://www.python.org/downloads/)**.</span><span class="sxs-lookup"><span data-stu-id="03720-106">You can use hello standard Python distribution found at **[Python.org downloads](https://www.python.org/downloads/)**.</span></span> 
* <span data-ttu-id="03720-107">Celá řada vývojářů vyhledání pohodlnou toouse hello  **[distribuci jazyka Python Anaconda](https://www.continuum.io/downloads)**.</span><span class="sxs-lookup"><span data-stu-id="03720-107">Many developers find it convenient toouse hello **[Anaconda Python distribution](https://www.continuum.io/downloads)**.</span></span>  
* <span data-ttu-id="03720-108">Tento článek byl napsán pomocí Pythonu verze 3.6 z hello standardní distribuci jazyka Python</span><span class="sxs-lookup"><span data-stu-id="03720-108">This article was written using Python version 3.6 from hello standard Python distribution</span></span>

## <a name="install-azure-python-sdk"></a><span data-ttu-id="03720-109">Instalace sady Azure Python SDK</span><span class="sxs-lookup"><span data-stu-id="03720-109">Install Azure Python SDK</span></span>

<span data-ttu-id="03720-110">Nainstalujte následující moduly hello:</span><span class="sxs-lookup"><span data-stu-id="03720-110">Install hello following modules:</span></span>

* <span data-ttu-id="03720-111">Hello **prostředků azure mgmt** modul obsahuje další moduly Azure Active Directory, atd.</span><span class="sxs-lookup"><span data-stu-id="03720-111">hello **azure-mgmt-resource** module includes other Azure modules for Active Directory, etc.</span></span>
* <span data-ttu-id="03720-112">Hello **azure mgmt datalake úložiště** modul obsahuje operace správy účtů Azure Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="03720-112">hello **azure-mgmt-datalake-store** module includes hello Azure Data Lake Store account management operations.</span></span>
* <span data-ttu-id="03720-113">Hello **úložiště azure datalake** modul obsahuje hello operace systému souborů Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="03720-113">hello **azure-datalake-store** module includes hello Azure Data Lake Store filesystem operations.</span></span> 
* <span data-ttu-id="03720-114">Hello **azure-datalake-analytics** modulu zahrnuje operace, hello Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="03720-114">hello **azure-datalake-analytics** module includes hello Azure Data Lake Analytics operations.</span></span> 

<span data-ttu-id="03720-115">Nejprve zkontrolujte nejnovější máte hello `pip` spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="03720-115">First, ensure you have hello latest `pip` by running hello following command:</span></span>

```
python -m pip install --upgrade pip
```

<span data-ttu-id="03720-116">Tento dokument byla zapsána pomocí `pip version 9.0.1`.</span><span class="sxs-lookup"><span data-stu-id="03720-116">This document was written using `pip version 9.0.1`.</span></span>

<span data-ttu-id="03720-117">Použijte hello `pip` příkazy moduly hello tooinstall z příkazového řádku hello:</span><span class="sxs-lookup"><span data-stu-id="03720-117">Use hello following `pip` commands tooinstall hello modules from hello commandline:</span></span>

```
pip install azure-mgmt-resource
pip install azure-datalake-store
pip install azure-mgmt-datalake-store
pip install azure-mgmt-datalake-analytics
```

## <a name="create-a-new-python-script"></a><span data-ttu-id="03720-118">Vytvořit nový skript v jazyce Python</span><span class="sxs-lookup"><span data-stu-id="03720-118">Create a new Python script</span></span>

<span data-ttu-id="03720-119">Vložte následující kód do skriptu hello hello:</span><span class="sxs-lookup"><span data-stu-id="03720-119">Paste hello following code into hello script:</span></span>

```python
## Use this only for Azure AD service-to-service authentication
#from azure.common.credentials import ServicePrincipalCredentials

## Use this only for Azure AD end-user authentication
#from azure.common.credentials import UserPassCredentials

## Required for Azure Resource Manager
from azure.mgmt.resource.resources import ResourceManagementClient
from azure.mgmt.resource.resources.models import ResourceGroup

## Required for Azure Data Lake Store account management
from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
from azure.mgmt.datalake.store.models import DataLakeStoreAccount

## Required for Azure Data Lake Store filesystem management
from azure.datalake.store import core, lib, multithread

## Required for Azure Data Lake Analytics account management
from azure.mgmt.datalake.analytics.account import DataLakeAnalyticsAccountManagementClient
from azure.mgmt.datalake.analytics.account.models import DataLakeAnalyticsAccount, DataLakeStoreAccountInfo

## Required for Azure Data Lake Analytics job management
from azure.mgmt.datalake.analytics.job import DataLakeAnalyticsJobManagementClient
from azure.mgmt.datalake.analytics.job.models import JobInformation, JobState, USqlJobProperties

## Required for Azure Data Lake Analytics catalog management
from azure.mgmt.datalake.analytics.catalog import DataLakeAnalyticsCatalogManagementClient

## Use these as needed for your application
import logging, getpass, pprint, uuid, time
```

<span data-ttu-id="03720-120">Spusťte tento skript tooverify této hello, který lze importovat moduly.</span><span class="sxs-lookup"><span data-stu-id="03720-120">Run this script tooverify that hello modules can be imported.</span></span>

## <a name="authentication"></a><span data-ttu-id="03720-121">Authentication</span><span class="sxs-lookup"><span data-stu-id="03720-121">Authentication</span></span>

### <a name="interactive-user-authentication-with-a-pop-up"></a><span data-ttu-id="03720-122">Interaktivním ověřování uživatelů s automaticky otevíraného okna</span><span class="sxs-lookup"><span data-stu-id="03720-122">Interactive user authentication with a pop-up</span></span>

<span data-ttu-id="03720-123">Tato metoda není podporována.</span><span class="sxs-lookup"><span data-stu-id="03720-123">This method is not supported.</span></span>

### <a name="interactive-user-authentication-with-a-device-code"></a><span data-ttu-id="03720-124">Interaktivním ověřování uživatelů s kódem zařízení</span><span class="sxs-lookup"><span data-stu-id="03720-124">Interactive user authentication with a device code</span></span>

```python
user = input('Enter hello user tooauthenticate with that has permission toosubscription: ')
password = getpass.getpass()
credentials = UserPassCredentials(user, password)
```

### <a name="noninteractive-authentication-with-spi-and-a-secret"></a><span data-ttu-id="03720-125">Neinteraktivní ověřování s SPI a tajný klíč</span><span class="sxs-lookup"><span data-stu-id="03720-125">Noninteractive authentication with SPI and a secret</span></span>

```python
credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')
```

### <a name="noninteractive-authentication-with-api-and-a-certificate"></a><span data-ttu-id="03720-126">Neinteraktivní ověřování pomocí rozhraní API a certifikátu</span><span class="sxs-lookup"><span data-stu-id="03720-126">Noninteractive authentication with API and a certificate</span></span>

<span data-ttu-id="03720-127">Tato metoda není podporována.</span><span class="sxs-lookup"><span data-stu-id="03720-127">This method is not supported.</span></span>

## <a name="common-script-variables"></a><span data-ttu-id="03720-128">Běžné proměnné skriptu</span><span class="sxs-lookup"><span data-stu-id="03720-128">Common script variables</span></span>

<span data-ttu-id="03720-129">Tyto proměnné se používají v hello ukázky.</span><span class="sxs-lookup"><span data-stu-id="03720-129">These variables are used in hello samples.</span></span>

```python
subid= '<Azure Subscription ID>'
rg = '<Azure Resource Group Name>'
location = '<Location>' # i.e. 'eastus2'
adls = '<Azure Data Lake Store Account Name>'
adla = '<Azure Data Lake Analytics Account Name>'
```

## <a name="create-hello-clients"></a><span data-ttu-id="03720-130">Vytvořit klienti hello</span><span class="sxs-lookup"><span data-stu-id="03720-130">Create hello clients</span></span>

```python
resourceClient = ResourceManagementClient(credentials, subid)
adlaAcctClient = DataLakeAnalyticsAccountManagementClient(credentials, subid)
adlaJobClient = DataLakeAnalyticsJobManagementClient( credentials, 'azuredatalakeanalytics.net')
```

## <a name="create-an-azure-resource-group"></a><span data-ttu-id="03720-131">Vytvoření skupiny prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="03720-131">Create an Azure Resource Group</span></span>

```python
armGroupResult = resourceClient.resource_groups.create_or_update( rg, ResourceGroup( location=location ) )
```

## <a name="create-data-lake-analytics-account"></a><span data-ttu-id="03720-132">Vytvoření účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="03720-132">Create Data Lake Analytics account</span></span>

<span data-ttu-id="03720-133">Nejprve vytvořte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="03720-133">First create a store account.</span></span>

```python
adlaAcctResult = adlaAcctClient.account.create(
    rg,
    adla,
    DataLakeAnalyticsAccount(
        location=location,
        default_data_lake_store_account=adls,
        data_lake_store_accounts=[DataLakeStoreAccountInfo(name=adls)]
    )
).wait()
```
<span data-ttu-id="03720-134">Pak vytvořte ADLA účtu, který použije toto úložiště.</span><span class="sxs-lookup"><span data-stu-id="03720-134">Then create an ADLA account that uses that store.</span></span>

```python
adlaAcctResult = adlaAcctClient.account.create(
    rg,
    adla,
    DataLakeAnalyticsAccount(
        location=location,
        default_data_lake_store_account=adls,
        data_lake_store_accounts=[DataLakeStoreAccountInfo(name=adls)]
    )
).wait()
```

## <a name="submit-a-job"></a><span data-ttu-id="03720-135">Odeslání úlohy</span><span class="sxs-lookup"><span data-stu-id="03720-135">Submit a job</span></span>

```python
script = """
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
"""

jobId = str(uuid.uuid4())
jobResult = adlaJobClient.job.create(
    adla,
    jobId,
    JobInformation(
        name='Sample Job',
        type='USql',
        properties=USqlJobProperties(script=script)
    )
)
```

## <a name="wait-for-a-job-tooend"></a><span data-ttu-id="03720-136">Počkejte tooend úlohy</span><span class="sxs-lookup"><span data-stu-id="03720-136">Wait for a job tooend</span></span>

```python
jobResult = adlaJobClient.job.get(adla, jobId)
while(jobResult.state != JobState.ended):
    print('Job is not yet done, waiting for 3 seconds. Current state: ' + jobResult.state.value)
    time.sleep(3)
    jobResult = adlaJobClient.job.get(adla, jobId)

print ('Job finished with result: ' + jobResult.result.value)
```

## <a name="list-pipelines-and-recurrences"></a><span data-ttu-id="03720-137">Seznam kanálů a opakování</span><span class="sxs-lookup"><span data-stu-id="03720-137">List pipelines and recurrences</span></span>
<span data-ttu-id="03720-138">Podle toho, jestli vaše úlohy mají kanálu nebo opakování metadata připojen, můžete seznam kanálů a opakování.</span><span class="sxs-lookup"><span data-stu-id="03720-138">Depending whether your jobs have pipeline or recurrence metadata attached, you can list pipelines and recurrences.</span></span>

```python
pipelines = adlaJobClient.pipeline.list(adla)
for p in pipelines:
    print('Pipeline: ' + p.name + ' ' + p.pipelineId)

recurrences = adlaJobClient.recurrence.list(adla)
for r in recurrences:
    print('Recurrence: ' + r.name + ' ' + r.recurrenceId)
```

## <a name="manage-compute-policies"></a><span data-ttu-id="03720-139">Správa zásad výpočetní</span><span class="sxs-lookup"><span data-stu-id="03720-139">Manage compute policies</span></span>

<span data-ttu-id="03720-140">objekt DataLakeAnalyticsAccountManagementClient Hello poskytuje metody pro správu hello výpočetní zásady pro účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="03720-140">hello DataLakeAnalyticsAccountManagementClient object provides methods for managing hello compute policies for a Data Lake Analytics account.</span></span>

### <a name="list-compute-policies"></a><span data-ttu-id="03720-141">Seznam výpočetní zásad</span><span class="sxs-lookup"><span data-stu-id="03720-141">List compute policies</span></span>

<span data-ttu-id="03720-142">Hello následující kód načte seznam výpočetní zásad pro účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="03720-142">hello following code retrieves a list of compute policies for a Data Lake Analytics account.</span></span>

```python
policies = adlaAccountClient.computePolicies.listByAccount(rg, adla)
for p in policies:
    print('Name: ' + p.name + 'Type: ' + p.objectType + 'Max AUs / job: ' + p.maxDegreeOfParallelismPerJob + 'Min priority / job: ' + p.minPriorityPerJob)
```

### <a name="create-a-new-compute-policy"></a><span data-ttu-id="03720-143">Vytvořit novou zásadu výpočetní</span><span class="sxs-lookup"><span data-stu-id="03720-143">Create a new compute policy</span></span>

<span data-ttu-id="03720-144">Hello následující kód vytvoří novou zásadu výpočetní pro účet Data Lake Analytics, nastavení hello maximální Austrálie dostupné toohello zadané uživatele too50 a too250 s prioritou hello minimální úlohy.</span><span class="sxs-lookup"><span data-stu-id="03720-144">hello following code creates a new compute policy for a Data Lake Analytics account, setting hello maximum AUs available toohello specified user too50, and hello minimum job priority too250.</span></span>

```python
userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde"
newPolicyParams = ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250)
adlaAccountClient.computePolicies.createOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams)
```

## <a name="next-steps"></a><span data-ttu-id="03720-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="03720-145">Next steps</span></span>

- <span data-ttu-id="03720-146">toosee hello stejný kurz pomocí jiných nástrojů, klikněte na selektory karet hello na hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="03720-146">toosee hello same tutorial using other tools, click hello tab selectors on hello top of hello page.</span></span>
- <span data-ttu-id="03720-147">toolearn U-SQL, najdete v části [Začínáme s jazykem Azure Data Lake Analytics U-SQL](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="03720-147">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
- <span data-ttu-id="03720-148">Informace týkající se úloh správy najdete v tématu [Správa služby Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="03720-148">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>

