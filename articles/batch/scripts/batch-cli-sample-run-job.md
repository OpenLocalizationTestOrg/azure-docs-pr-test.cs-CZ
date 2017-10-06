---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku – spuštění úlohy pomocí služby Batch | Microsoft Docs"
description: "Azure ukázka skriptu rozhraní příkazového řádku – spuštění úlohy pomocí služby Batch"
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.openlocfilehash: f73a7cb341e550fd1c92f92201e20b27fa20d238
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="running-jobs-on-azure-batch-with-azure-cli"></a>Spuštění úlohy v Azure Batch pomocí rozhraní příkazového řádku Azure

Tento skript vytvoří dávkovou úlohu a přidá řadu úloh toohello úlohy. Také ukazuje, jak toomonitor úlohy a její úkoly. Nakonec ukazuje, jak tooquery hello služby Batch efektivně pro informace o hello úlohy.

## <a name="prerequisites"></a>Požadavky

- Instalace hello hello pokyny uvedené v hello pomocí rozhraní příkazového řádku Azure [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), pokud jste tak již neučinili.
- Pokud jste již nemáte, vytvořte účet Batch. V tématu [vytvořit dávkový účet s hello rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) ukázkový skript, který vytvoří účet.
- Pokud jste tak ještě neučinili, nakonfigurujte toorun aplikaci ze spouštěcí úkol. V tématu [přidání tooAzure aplikace Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) ukázkový skript, který se vytvoří aplikace a odesílá tooAzure balíčku aplikace.
- Nakonfigurujte fond, na které hello bude úloha spuštěna. V tématu [fondech Správa Azure Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/batch-cli-sample-manage-pool) ukázkový skript, který vytvoří fond s konfigurace cloudové služby nebo konfigurace virtuálního počítače.

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/batch/run-job/run-job.sh "Run Job")]

## <a name="clean-up-job"></a>Vyčištění úlohy

Po spuštění hello výše ukázkový skript, spusťte následující příkaz tooremove hello úlohy a všechny její úkoly. Všimněte si, že fond hello potřebovat toobe odstranit samostatně. V tématu [fondech Správa Azure Batch pomocí rozhraní příkazového řádku Azure](./batch-cli-sample-manage-pool.md) Další informace o vytváření a odstraňování fondy.

```azurecli
az batch job delete --job-id myjob
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy toocreate hello dávkové úlohy a úlohy. Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.

| Příkaz | Poznámky |
|---|---|
| [připojení k účtu batch az](https://docs.microsoft.com/cli/azure/batch/account#login) | Ověřování na základě účtu Batch.  |
| [vytvoření úlohy batch az](https://docs.microsoft.com/cli/azure/batch/job#create) | Vytvoří dávkovou úlohu.  |
| [sady úloh az batch](https://docs.microsoft.com/cli/azure/batch/job#set) | Aktualizace vlastnosti dávkovou úlohu.  |
| [Zobrazit úlohy batch az](https://docs.microsoft.com/cli/azure/batch/job#show) | Načte podrobnosti o zadaný dávkovou úlohu.  |
| [vytvoření úlohy batch az](https://docs.microsoft.com/cli/azure/batch/task#create) | Přidá že určenou toohello úloh dávkovou úlohu.  |
| [Zobrazit úlohy batch az](https://docs.microsoft.com/cli/azure/batch/task#show) | Načte hello podrobnosti úlohy z hello zadat dávkovou úlohu.  |
| [Seznam úkolů az batch](https://docs.microsoft.com/cli/azure/batch/task#list) | Obsahuje hello úkoly spojené s hello zadanou úlohu.  |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku Batch lze nalézt v hello [dokumentaci k rozhraní příkazového řádku Azure Batch](../batch-cli-samples.md).
