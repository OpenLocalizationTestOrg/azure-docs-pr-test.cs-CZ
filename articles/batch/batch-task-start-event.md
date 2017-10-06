---
title: "AAA \"události spuštění úlohy Azure Batch | Microsoft Docs\""
description: "Referenční informace pro úlohy Batch spusťte událost."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 2cb066be1578741125e9081a84a2b7c74dc8356a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="task-start-event"></a>Událost spuštění úlohy

 Tato událost je vygenerované Jakmile úloha už naplánované toostart na výpočetním uzlu plánovačem hello. Všimněte si, že pokud je úloha hello opakovat nebo zařazena Tato událost bude být vygenerované znovu hello stejný úkol, ale hello počet opakování a verze systému úloh bude příslušným způsobem aktualizuje.


 Hello následující příklad ukazuje textu hello události spuštění úloh.

```
{
    "jobId": "job-0000000001",
    "id": "task-5",
    "taskType": "User",
    "systemTaskVersion": 0,
    "nodeInfo": {
        "poolId": "pool-001",
        "nodeId": "tvm-257509324_1-20160908t162728z"
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 2
    },
    "executionInfo": {
        "retryCount": 0
    }
}
```

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|JobId|Řetězec|id Hello hello úlohy, která obsahuje úlohu hello.|
|id|Řetězec|id Hello hello úlohy.|
|taskType|Řetězec|Typ Hello hello úlohy. To může být JobManager oznamující, že je úkol správce nebo uživatel oznamující, že se nejedná o úkolu Správce úloh.|
|systemTaskVersion|Int32|Toto je interní opakování čítač hello na úlohu. Služba Batch hello interně může pokus zopakovat tooaccount úloh pro přechodné problémy. Tyto problémy mohou zahrnovat interní plánování chyby nebo pokusy o toorecover z výpočetních uzlů ve špatném stavu.|
|[nodeInfo](#nodeInfo)|Komplexní typ|Obsahuje informace o hello výpočetním uzlu, na které hello úloha spustila.|
|[multiInstanceSettings](#multiInstanceSettings)|Komplexní typ|Určuje, že tuto úlohu hello je úkol s více instancemi nutnosti několika výpočetních uzlech.  V tématu [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) podrobnosti.|
|[omezení](#constraints)|Komplexní typ|Hello provádění omezení, která platí toothis úloh.|
|[executionInfo](#executionInfo)|Komplexní typ|Obsahuje informace o provádění hello hello úlohy.|

###  <a name="nodeInfo"></a>nodeInfo

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|poolId|Řetězec|id Hello hello fond, na které hello úloha spustila.|
|nodeId|Řetězec|id Hello hello uzlu, na které hello úloha spustila.|

###  <a name="multiInstanceSettings"></a>multiInstanceSettings

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|numberOfInstances|celá čísla|Hello počet výpočetních uzlů, které vyžadují hello úloh.|

###  <a name="constraints"></a>omezení

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|Hello maximální počet opakovaných úkolů hello. Hello služba Batch úkol zopakuje, pokud je jeho ukončovací kód nenulové hodnoty.<br /><br /> Všimněte si, že tato hodnota řídí konkrétně hello počet opakování. Služba Batch Hello se pokusí hello úloh jednou a mohou zkuste si toothis limit. Například pokud hello maximální počet opakování je 3, Batch pokusí úlohu až too4 dobu (jeden počáteční pokus a 3 opakování).<br /><br /> Pokud hello maximální počet opakování 0, služba Batch hello neopakuje úlohy.<br /><br /> Pokud hello maximální počet opakování −1, služba Batch hello opakuje úlohy bez omezení.<br /><br /> Hello výchozí hodnota je 0 (bez opakování).|

###  <a name="executionInfo"></a>executionInfo

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|retryCount|Int32|Hello počet oznámení, která má byla hello úloha opakovat hello služby Batch. Úloha Hello je zopakován, pokud bude nenulový ukončovací kód, až toohello zadaný MaxTaskRetryCount|
