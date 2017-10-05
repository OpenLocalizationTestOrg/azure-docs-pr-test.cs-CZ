---
title: "Správa Azure Data Lake Analytics pomocí Python | Microsoft Docs"
description: "Zjistěte, jak pomocí Pythonu vytvořit účet Data Lake Store a odesílat úlohy. "
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
ms.openlocfilehash: 31326a32f8748e6cfb8bfe24cda46c511ab59352
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="manage-azure-data-lake-analytics-using-python"></a><span data-ttu-id="c1890-103">Správa Azure Data Lake Analytics používá Python</span><span class="sxs-lookup"><span data-stu-id="c1890-103">Manage Azure Data Lake Analytics using Python</span></span>

## <a name="python-versions"></a><span data-ttu-id="c1890-104">Verze jazyka Python</span><span class="sxs-lookup"><span data-stu-id="c1890-104">Python versions</span></span>

* <span data-ttu-id="c1890-105">Použijte 64bitovou verzi jazyka Python.</span><span class="sxs-lookup"><span data-stu-id="c1890-105">Use a 64-bit version of Python.</span></span>
* <span data-ttu-id="c1890-106">Můžete použít standardní distribuci jazyka Python nalezený na  **[Python.org stáhne](https://www.python.org/downloads/)**.</span><span class="sxs-lookup"><span data-stu-id="c1890-106">You can use the standard Python distribution found at **[Python.org downloads](https://www.python.org/downloads/)**.</span></span> 
* <span data-ttu-id="c1890-107">Celá řada vývojářů být vhodné použít  **[distribuci jazyka Python Anaconda](https://www.continuum.io/downloads)**.</span><span class="sxs-lookup"><span data-stu-id="c1890-107">Many developers find it convenient to use the **[Anaconda Python distribution](https://www.continuum.io/downloads)**.</span></span>  
* <span data-ttu-id="c1890-108">Tento článek byl napsané v Pythonu verze 3.6 ze standardního distribučního Python</span><span class="sxs-lookup"><span data-stu-id="c1890-108">This article was written using Python version 3.6 from the standard Python distribution</span></span>

## <a name="install-azure-python-sdk"></a><span data-ttu-id="c1890-109">Instalace sady Azure Python SDK</span><span class="sxs-lookup"><span data-stu-id="c1890-109">Install Azure Python SDK</span></span>

<span data-ttu-id="c1890-110">Nainstalujte následující moduly:</span><span class="sxs-lookup"><span data-stu-id="c1890-110">Install the following modules:</span></span>

* <span data-ttu-id="c1890-111">**Prostředků azure mgmt** modul obsahuje další moduly Azure Active Directory, atd.</span><span class="sxs-lookup"><span data-stu-id="c1890-111">The **azure-mgmt-resource** module includes other Azure modules for Active Directory, etc.</span></span>
* <span data-ttu-id="c1890-112">**Azure mgmt datalake úložiště** modul obsahuje operace správy účtů Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c1890-112">The **azure-mgmt-datalake-store** module includes the Azure Data Lake Store account management operations.</span></span>
* <span data-ttu-id="c1890-113">**Úložiště azure datalake** modul obsahuje operace systému souborů Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c1890-113">The **azure-datalake-store** module includes the Azure Data Lake Store filesystem operations.</span></span> 
* <span data-ttu-id="c1890-114">**Azure-datalake-analytics** modulu zahrnuje operace Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="c1890-114">The **azure-datalake-analytics** module includes the Azure Data Lake Analytics operations.</span></span> 

<span data-ttu-id="c1890-115">První, zajistěte, abyste měli nejnovější `pip` spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="c1890-115">First, ensure you have the latest `pip` by running the following command:</span></span>

```
python -m pip install --upgrade pip
```

<span data-ttu-id="c1890-116">Tento dokument byla zapsána pomocí `pip version 9.0.1`.</span><span class="sxs-lookup"><span data-stu-id="c1890-116">This document was written using `pip version 9.0.1`.</span></span>

<span data-ttu-id="c1890-117">Použijte následující `pip` příkazy pro instalaci modulů z příkazovému řádku:</span><span class="sxs-lookup"><span data-stu-id="c1890-117">Use the following `pip` commands to install the modules from the commandline:</span></span>

```
pip install azure-mgmt-resource
pip install azure-datalake-store
pip install azure-mgmt-datalake-store
pip install azure-mgmt-datalake-analytics
```

## <a name="create-a-new-python-script"></a><span data-ttu-id="c1890-118">Vytvořit nový skript v jazyce Python</span><span class="sxs-lookup"><span data-stu-id="c1890-118">Create a new Python script</span></span>

<span data-ttu-id="c1890-119">Vložte následující kód do skriptu:</span><span class="sxs-lookup"><span data-stu-id="c1890-119">Paste the following code into the script:</span></span>

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

<span data-ttu-id="c1890-120">Spusťte tento skript k ověření, že můžete naimportovat moduly.</span><span class="sxs-lookup"><span data-stu-id="c1890-120">Run this script to verify that the modules can be imported.</span></span>

## <a name="authentication"></a><span data-ttu-id="c1890-121">Authentication</span><span class="sxs-lookup"><span data-stu-id="c1890-121">Authentication</span></span>

### <a name="interactive-user-authentication-with-a-pop-up"></a><span data-ttu-id="c1890-122">Interaktivním ověřování uživatelů s automaticky otevíraného okna</span><span class="sxs-lookup"><span data-stu-id="c1890-122">Interactive user authentication with a pop-up</span></span>

<span data-ttu-id="c1890-123">Tato metoda není podporována.</span><span class="sxs-lookup"><span data-stu-id="c1890-123">This method is not supported.</span></span>

### <a name="interactive-user-authentication-with-a-device-code"></a><span data-ttu-id="c1890-124">Interaktivním ověřování uživatelů s kódem zařízení</span><span class="sxs-lookup"><span data-stu-id="c1890-124">Interactive user authentication with a device code</span></span>

```python
user = input('Enter the user to authenticate with that has permission to subscription: ')
password = getpass.getpass()
credentials = UserPassCredentials(user, password)
```

### <a name="noninteractive-authentication-with-spi-and-a-secret"></a><span data-ttu-id="c1890-125">Neinteraktivní ověřování s SPI a tajný klíč</span><span class="sxs-lookup"><span data-stu-id="c1890-125">Noninteractive authentication with SPI and a secret</span></span>

```python
credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')
```

### <a name="noninteractive-authentication-with-api-and-a-certificate"></a><span data-ttu-id="c1890-126">Neinteraktivní ověřování pomocí rozhraní API a certifikátu</span><span class="sxs-lookup"><span data-stu-id="c1890-126">Noninteractive authentication with API and a certificate</span></span>

<span data-ttu-id="c1890-127">Tato metoda není podporována.</span><span class="sxs-lookup"><span data-stu-id="c1890-127">This method is not supported.</span></span>

## <a name="common-script-variables"></a><span data-ttu-id="c1890-128">Běžné proměnné skriptu</span><span class="sxs-lookup"><span data-stu-id="c1890-128">Common script variables</span></span>

<span data-ttu-id="c1890-129">Tyto proměnné se používají v ukázky.</span><span class="sxs-lookup"><span data-stu-id="c1890-129">These variables are used in the samples.</span></span>

```python
subid= '<Azure Subscription ID>'
rg = '<Azure Resource Group Name>'
location = '<Location>' # i.e. 'eastus2'
adls = '<Azure Data Lake Store Account Name>'
adla = '<Azure Data Lake Analytics Account Name>'
```

## <a name="create-the-clients"></a><span data-ttu-id="c1890-130">Vytvoření klientů</span><span class="sxs-lookup"><span data-stu-id="c1890-130">Create the clients</span></span>

```python
resourceClient = ResourceManagementClient(credentials, subid)
adlaAcctClient = DataLakeAnalyticsAccountManagementClient(credentials, subid)
adlaJobClient = DataLakeAnalyticsJobManagementClient( credentials, 'azuredatalakeanalytics.net')
```

## <a name="create-an-azure-resource-group"></a><span data-ttu-id="c1890-131">Vytvoření skupiny prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="c1890-131">Create an Azure Resource Group</span></span>

```python
armGroupResult = resourceClient.resource_groups.create_or_update( rg, ResourceGroup( location=location ) )
```

## <a name="create-data-lake-analytics-account"></a><span data-ttu-id="c1890-132">Vytvoření účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="c1890-132">Create Data Lake Analytics account</span></span>

<span data-ttu-id="c1890-133">Nejprve vytvořte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c1890-133">First create a store account.</span></span>

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
<span data-ttu-id="c1890-134">Pak vytvořte ADLA účtu, který použije toto úložiště.</span><span class="sxs-lookup"><span data-stu-id="c1890-134">Then create an ADLA account that uses that store.</span></span>

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

## <a name="submit-a-job"></a><span data-ttu-id="c1890-135">Odeslání úlohy</span><span class="sxs-lookup"><span data-stu-id="c1890-135">Submit a job</span></span>

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
    TO "/data.csv"
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

## <a name="wait-for-a-job-to-end"></a><span data-ttu-id="c1890-136">Počkejte na ukončení úlohy</span><span class="sxs-lookup"><span data-stu-id="c1890-136">Wait for a job to end</span></span>

```python
jobResult = adlaJobClient.job.get(adla, jobId)
while(jobResult.state != JobState.ended):
    print('Job is not yet done, waiting for 3 seconds. Current state: ' + jobResult.state.value)
    time.sleep(3)
    jobResult = adlaJobClient.job.get(adla, jobId)

print ('Job finished with result: ' + jobResult.result.value)
```

## <a name="list-pipelines-and-recurrences"></a><span data-ttu-id="c1890-137">Seznam kanálů a opakování</span><span class="sxs-lookup"><span data-stu-id="c1890-137">List pipelines and recurrences</span></span>
<span data-ttu-id="c1890-138">Podle toho, jestli vaše úlohy mají kanálu nebo opakování metadata připojen, můžete seznam kanálů a opakování.</span><span class="sxs-lookup"><span data-stu-id="c1890-138">Depending whether your jobs have pipeline or recurrence metadata attached, you can list pipelines and recurrences.</span></span>

```python
pipelines = adlaJobClient.pipeline.list(adla)
for p in pipelines:
    print('Pipeline: ' + p.name + ' ' + p.pipelineId)

recurrences = adlaJobClient.recurrence.list(adla)
for r in recurrences:
    print('Recurrence: ' + r.name + ' ' + r.recurrenceId)
```

## <a name="manage-compute-policies"></a><span data-ttu-id="c1890-139">Správa zásad výpočetní</span><span class="sxs-lookup"><span data-stu-id="c1890-139">Manage compute policies</span></span>

<span data-ttu-id="c1890-140">Objekt DataLakeAnalyticsAccountManagementClient poskytuje metody pro správu výpočetních zásady pro účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="c1890-140">The DataLakeAnalyticsAccountManagementClient object provides methods for managing the compute policies for a Data Lake Analytics account.</span></span>

### <a name="list-compute-policies"></a><span data-ttu-id="c1890-141">Seznam výpočetní zásad</span><span class="sxs-lookup"><span data-stu-id="c1890-141">List compute policies</span></span>

<span data-ttu-id="c1890-142">Následující kód načte seznam výpočetní zásad pro účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="c1890-142">The following code retrieves a list of compute policies for a Data Lake Analytics account.</span></span>

```python
policies = adlaAccountClient.computePolicies.listByAccount(rg, adla)
for p in policies:
    print('Name: ' + p.name + 'Type: ' + p.objectType + 'Max AUs / job: ' + p.maxDegreeOfParallelismPerJob + 'Min priority / job: ' + p.minPriorityPerJob)
```

### <a name="create-a-new-compute-policy"></a><span data-ttu-id="c1890-143">Vytvořit novou zásadu výpočetní</span><span class="sxs-lookup"><span data-stu-id="c1890-143">Create a new compute policy</span></span>

<span data-ttu-id="c1890-144">Následující kód vytvoří novou zásadu výpočetní pro účet Data Lake Analytics, nastavení maximální Austrálie dostupná pro zadaného uživatele na 50 a priority minimální úloh na 250.</span><span class="sxs-lookup"><span data-stu-id="c1890-144">The following code creates a new compute policy for a Data Lake Analytics account, setting the maximum AUs available to the specified user to 50, and the minimum job priority to 250.</span></span>

```python
userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde"
newPolicyParams = ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250)
adlaAccountClient.computePolicies.createOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams)
```

## <a name="next-steps"></a><span data-ttu-id="c1890-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c1890-145">Next steps</span></span>

- <span data-ttu-id="c1890-146">Pokud chcete použít jiné podporované nástroje a zobrazit stejný kurz, klikněte na selektory karet v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="c1890-146">To see the same tutorial using other tools, click the tab selectors on the top of the page.</span></span>
- <span data-ttu-id="c1890-147">Pokud se chcete naučit jazyk U-SQL, informace najdete v tématu [Začínáme s jazykem U-SQL Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c1890-147">To learn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
- <span data-ttu-id="c1890-148">Informace týkající se úloh správy najdete v tématu [Správa služby Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c1890-148">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>

