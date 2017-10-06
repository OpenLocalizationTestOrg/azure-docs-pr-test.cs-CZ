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
# <a name="manage-azure-data-lake-analytics-using-python"></a>Správa Azure Data Lake Analytics používá Python

## <a name="python-versions"></a>Verze jazyka Python

* Použijte 64bitovou verzi jazyka Python.
* Můžete použít standardní distribuci jazyka Python hello nalezený na  **[Python.org stáhne](https://www.python.org/downloads/)**. 
* Celá řada vývojářů vyhledání pohodlnou toouse hello  **[distribuci jazyka Python Anaconda](https://www.continuum.io/downloads)**.  
* Tento článek byl napsán pomocí Pythonu verze 3.6 z hello standardní distribuci jazyka Python

## <a name="install-azure-python-sdk"></a>Instalace sady Azure Python SDK

Nainstalujte následující moduly hello:

* Hello **prostředků azure mgmt** modul obsahuje další moduly Azure Active Directory, atd.
* Hello **azure mgmt datalake úložiště** modul obsahuje operace správy účtů Azure Data Lake Store hello.
* Hello **úložiště azure datalake** modul obsahuje hello operace systému souborů Azure Data Lake Store. 
* Hello **azure-datalake-analytics** modulu zahrnuje operace, hello Azure Data Lake Analytics. 

Nejprve zkontrolujte nejnovější máte hello `pip` spuštěním hello následující příkaz:

```
python -m pip install --upgrade pip
```

Tento dokument byla zapsána pomocí `pip version 9.0.1`.

Použijte hello `pip` příkazy moduly hello tooinstall z příkazového řádku hello:

```
pip install azure-mgmt-resource
pip install azure-datalake-store
pip install azure-mgmt-datalake-store
pip install azure-mgmt-datalake-analytics
```

## <a name="create-a-new-python-script"></a>Vytvořit nový skript v jazyce Python

Vložte následující kód do skriptu hello hello:

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

Spusťte tento skript tooverify této hello, který lze importovat moduly.

## <a name="authentication"></a>Authentication

### <a name="interactive-user-authentication-with-a-pop-up"></a>Interaktivním ověřování uživatelů s automaticky otevíraného okna

Tato metoda není podporována.

### <a name="interactive-user-authentication-with-a-device-code"></a>Interaktivním ověřování uživatelů s kódem zařízení

```python
user = input('Enter hello user tooauthenticate with that has permission toosubscription: ')
password = getpass.getpass()
credentials = UserPassCredentials(user, password)
```

### <a name="noninteractive-authentication-with-spi-and-a-secret"></a>Neinteraktivní ověřování s SPI a tajný klíč

```python
credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')
```

### <a name="noninteractive-authentication-with-api-and-a-certificate"></a>Neinteraktivní ověřování pomocí rozhraní API a certifikátu

Tato metoda není podporována.

## <a name="common-script-variables"></a>Běžné proměnné skriptu

Tyto proměnné se používají v hello ukázky.

```python
subid= '<Azure Subscription ID>'
rg = '<Azure Resource Group Name>'
location = '<Location>' # i.e. 'eastus2'
adls = '<Azure Data Lake Store Account Name>'
adla = '<Azure Data Lake Analytics Account Name>'
```

## <a name="create-hello-clients"></a>Vytvořit klienti hello

```python
resourceClient = ResourceManagementClient(credentials, subid)
adlaAcctClient = DataLakeAnalyticsAccountManagementClient(credentials, subid)
adlaJobClient = DataLakeAnalyticsJobManagementClient( credentials, 'azuredatalakeanalytics.net')
```

## <a name="create-an-azure-resource-group"></a>Vytvoření skupiny prostředků Azure

```python
armGroupResult = resourceClient.resource_groups.create_or_update( rg, ResourceGroup( location=location ) )
```

## <a name="create-data-lake-analytics-account"></a>Vytvoření účtu Data Lake Analytics

Nejprve vytvořte účet úložiště.

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
Pak vytvořte ADLA účtu, který použije toto úložiště.

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

## <a name="submit-a-job"></a>Odeslání úlohy

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

## <a name="wait-for-a-job-tooend"></a>Počkejte tooend úlohy

```python
jobResult = adlaJobClient.job.get(adla, jobId)
while(jobResult.state != JobState.ended):
    print('Job is not yet done, waiting for 3 seconds. Current state: ' + jobResult.state.value)
    time.sleep(3)
    jobResult = adlaJobClient.job.get(adla, jobId)

print ('Job finished with result: ' + jobResult.result.value)
```

## <a name="list-pipelines-and-recurrences"></a>Seznam kanálů a opakování
Podle toho, jestli vaše úlohy mají kanálu nebo opakování metadata připojen, můžete seznam kanálů a opakování.

```python
pipelines = adlaJobClient.pipeline.list(adla)
for p in pipelines:
    print('Pipeline: ' + p.name + ' ' + p.pipelineId)

recurrences = adlaJobClient.recurrence.list(adla)
for r in recurrences:
    print('Recurrence: ' + r.name + ' ' + r.recurrenceId)
```

## <a name="manage-compute-policies"></a>Správa zásad výpočetní

objekt DataLakeAnalyticsAccountManagementClient Hello poskytuje metody pro správu hello výpočetní zásady pro účet Data Lake Analytics.

### <a name="list-compute-policies"></a>Seznam výpočetní zásad

Hello následující kód načte seznam výpočetní zásad pro účet Data Lake Analytics.

```python
policies = adlaAccountClient.computePolicies.listByAccount(rg, adla)
for p in policies:
    print('Name: ' + p.name + 'Type: ' + p.objectType + 'Max AUs / job: ' + p.maxDegreeOfParallelismPerJob + 'Min priority / job: ' + p.minPriorityPerJob)
```

### <a name="create-a-new-compute-policy"></a>Vytvořit novou zásadu výpočetní

Hello následující kód vytvoří novou zásadu výpočetní pro účet Data Lake Analytics, nastavení hello maximální Austrálie dostupné toohello zadané uživatele too50 a too250 s prioritou hello minimální úlohy.

```python
userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde"
newPolicyParams = ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250)
adlaAccountClient.computePolicies.createOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams)
```

## <a name="next-steps"></a>Další kroky

- toosee hello stejný kurz pomocí jiných nástrojů, klikněte na selektory karet hello na hello horní části stránky hello.
- toolearn U-SQL, najdete v části [Začínáme s jazykem Azure Data Lake Analytics U-SQL](data-lake-analytics-u-sql-get-started.md).
- Informace týkající se úloh správy najdete v tématu [Správa služby Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).

