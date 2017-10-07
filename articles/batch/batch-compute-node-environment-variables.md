---
title: "AAA \"výpočetní uzel proměnné prostředí Azure Batch | Microsoft Docs\""
description: "Výpočetní uzel odkazu na proměnnou prostředí pro Azure Batch Analytics."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/05/2017
ms.author: tamram
ms.openlocfilehash: 860f34b530579a81fbd5cf8ffa31df79d917c080
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-batch-compute-node-environment-variables"></a>Azure Batch výpočetní uzel prostředí proměnné
Hello [služby Azure Batch](https://azure.microsoft.com/services/batch/) nastaví následující proměnné prostředí na výpočetních uzlech hello. Můžete odkazovat těchto proměnných prostředí v příkazové řádky úkolu a hello programy a skripty spouštěné hello příkazové řádky.

Další informace o použití proměnných prostředí pomocí služby Batch najdete v tématu [nastavení prostředí pro úkoly](https://docs.microsoft.com/azure/batch/batch-api-basics#environment-settings-for-tasks).

## <a name="environment-variable-visibility"></a>Viditelnost proměnné prostředí

Tyto proměnné prostředí jsou viditelné pouze v kontextu hello hello **úloh uživatele**, hello uživatelského účtu na hello uzlu, ve kterém se spustí úlohu. Budete *není* podívejte na tyto Pokud jste [vzdáleně připojit](https://azure.microsoft.com/documentation/articles/batch-api-basics/#connecting-to-compute-nodes) tooa výpočetního uzlu přes protokol RDP (Remote Desktop) nebo Secure Shell (SSH) a seznamu proměnných prostředí hello. To je proto není hello hello uživatelský účet, který se používá pro připojení ke vzdálené stejné jako hello účet, který se používá úlohou hello.

## <a name="command-line-expansion-of-environment-variables"></a>Příkazového řádku rozšíření proměnných prostředí

příkazové řádky Hello provést úlohy na výpočetní uzly nespouštějte v rámci prostředí. Proto tyto příkazy na příkazových řádcích nemohou využívat nativně prostředí funkcí, jako je například rozšíření proměnné prostředí (to zahrnuje hello `PATH`). tootake využít těchto funkcí, musíte **vyvolání hello prostředí** v hello příkazového řádku. Například spusťte `cmd.exe` v systému Windows, výpočetní uzly nebo `/bin/sh` na Linuxových uzlů:

`cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

`/bin/sh -c MyTaskApplication $MY_ENV_VAR`

## <a name="environment-variables"></a>Proměnné prostředí

| Název proměnné                     | Popis                                                              | Dostupnost | Příklad |
|-----------------------------------|--------------------------------------------------------------------------|--------------|---------|
| AZ_BATCH_ACCOUNT_NAME           | Hello název účtu Batch hello, který hello úloh patří.                  | Všechny úlohy.   | mybatchaccount |
| AZ_BATCH_CERTIFICATES_DIR       | Adresáře v rámci hello [pracovního adresáře úkolu] [ files_dirs] v certifikáty jsou uložené pro Linux výpočetních uzlů. Všimněte si, že této proměnné prostředí se nevztahuje tooWindows výpočetních uzlů.                                                  | Všechny úlohy.   |  /mnt/batch/Tasks/workitems/batchjob001/Job-1/task001/certs |
| AZ_BATCH_JOB_ID                 | ID Hello hello úlohy, která hello úloh patří. | Všechny úlohy s výjimkou spouštěcí úkol. | batchjob001 |
| AZ_BATCH_JOB_PREP_DIR           | Úplná cesta Hello přípravy úlohy hello [adresáře úkolu] [ files_dirs] na uzlu hello. | Všechny úlohy s výjimkou spouštěcího úkolu a úkol přípravy úlohy. Dostupná, jenom Pokud hello úlohy je nakonfigurovaný s úkol přípravy úlohy. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation |
| AZ_BATCH_JOB_PREP_WORKING_DIR   | Úplná cesta Hello přípravy úlohy hello [pracovního adresáře úkolu] [ files_dirs] na uzlu hello. | Všechny úlohy s výjimkou spouštěcího úkolu a úkol přípravy úlohy. Dostupná, jenom Pokud hello úlohy je nakonfigurovaný s úkol přípravy úlohy. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation\wd |
| AZ_BATCH_NODE_ID                | ID Hello hello uzlu, který hello úloha je přiřazená. | Všechny úlohy. | TVM. 1219235766_3 20160919t172711z |
| AZ_BATCH_NODE_ROOT_DIR          | Úplná cesta Hello hello kořenové všech [dávky adresáře] [ files_dirs] na uzlu hello. | Všechny úlohy. | C:\user\tasks |
| AZ_BATCH_NODE_SHARED_DIR        | Úplná cesta Hello hello [sdíleného adresáře] [ files_dirs] na uzlu hello. Všechny úlohy, které jsou spouštěny na uzlu mít adresář toothis přístup pro čtení a zápis. Úlohy, které jsou spouštěny na jiných uzlech nemají directory toothis vzdálený přístup (není adresář "sdílené" sítě). | Všechny úlohy. | C:\user\tasks\shared |
| AZ_BATCH_NODE_STARTUP_DIR       | Úplná cesta Hello hello [spustit adresáře úkolu] [ files_dirs] na uzlu hello. | Všechny úlohy. | C:\user\tasks\startup |
| AZ_BATCH_POOL_ID                | ID Hello hello fondu, který hello úloha je spuštěna. | Všechny úlohy. | batchpool001 |
| AZ_BATCH_TASK_DIR               | Úplná cesta Hello hello [adresáře úkolu] [ files_dirs] na uzlu hello. Tento adresář obsahuje hello `stdout.txt` a `stderr.txt` hello úloh a hello AZ_BATCH_TASK_WORKING_DIR. | Všechny úlohy. | C:\user\tasks\workitems\batchjob001\job-1\task001 |
| AZ_BATCH_TASK_ID                | ID Hello hello aktuální úlohy. | Všechny úlohy s výjimkou spouštěcí úkol. | task001 |
| AZ_BATCH_TASK_WORKING_DIR       | Úplná cesta Hello hello [pracovního adresáře úkolu] [ files_dirs] na uzlu hello. Hello aktuálně spuštěných úloh má adresář toothis přístup pro čtení a zápis. | Všechny úlohy. | C:\user\tasks\workitems\batchjob001\job-1\task001\wd |
| CCP_NODES                       | seznam Hello uzly a počet jader na uzel, které jsou přiděleny tooa [úkol s více instancemi][multi_instance]. Uzly a jader jsou uvedeny ve formátu hello`numNodes<space>node1IP<space>node1Cores<space>`<br/>`node2IP<space>node2Cores<space> ...`, kde hello počet uzlů následuje nejméně jeden uzel IP adresy a hello počet jader pro každou. |  S více instancemi primární a dílčí úkoly. |`2 10.0.0.4 1 10.0.0.5 1` |
| AZ_BATCH_NODE_LIST              | Hello seznam uzlů, které jsou přiděleny tooa [úkol s více instancemi] [ multi_instance] ve formátu hello `nodeIP;nodeIP`. | S více instancemi primární a dílčí úkoly. | `10.0.0.4;10.0.0.5` |
| AZ_BATCH_HOST_LIST              | Hello seznam uzlů, které jsou přiděleny tooa [úkol s více instancemi] [ multi_instance] ve formátu hello `nodeIP,nodeIP`. | S více instancemi primární a dílčí úkoly. | `10.0.0.4,10.0.0.5` |
| AZ_BATCH_MASTER_NODE            | Hello IP adresu a port hello výpočetní uzel, na které hello primární úlohy [úkol s více instancemi] [ multi_instance] běží. | S více instancemi primární a dílčí úkoly. | `10.0.0.4:6000`|
| AZ_BATCH_TASK_SHARED_DIR | Cestu k adresáři, který je stejný pro hello primární úlohy a dílčí každý úkol [úkol s více instancemi][multi_instance]. Hello cesta existuje na všech uzlech, ve kterém je spuštěn s více instancemi úloh hello a je pro čtení a zápis přístupné toohello úloh příkazy spuštěné v tomto uzlu (obě hello [koordinaci příkaz] [ coord_cmd] a hello [příkaz aplikace][app_cmd]). Dílčí úkoly nebo primární úlohu, která spustit na jiných uzlech nemá directory toothis vzdálený přístup (není adresář "sdílené" sítě). | S více instancemi primární a dílčí úkoly. | C:\user\tasks\workitems\multiinstancesamplejob\job-1\multiinstancesampletask |
| AZ_BATCH_IS_CURRENT_NODE_MASTER | Určuje, zda aktuální uzel hello hello hlavní uzel [úkol s více instancemi][multi_instance]. Možné hodnoty jsou `true` a `false`.| S více instancemi primární a dílčí úkoly. | `true` |
| AZ_BATCH_NODE_IS_DEDICATED | Pokud `true`, aktuální uzel hello je vyhrazené uzlu. Pokud `false`, je [nízkou prioritu uzlu](batch-low-pri-vms.md). | Všechny úlohy. | `true` |

[files_dirs]: https://azure.microsoft.com/documentation/articles/batch-api-basics/#files-and-directories
[multi_instance]: https://azure.microsoft.com/documentation/articles/batch-mpi/
[coord_cmd]: https://azure.microsoft.com/documentation/articles/batch-mpi/#coordination-command
[app_cmd]: https://azure.microsoft.com/documentation/articles/batch-mpi/#application-command
