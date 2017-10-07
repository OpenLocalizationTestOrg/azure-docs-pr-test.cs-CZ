---
title: "AAA \"události po dokončení úlohy Azure Batch | Microsoft Docs\""
description: "Referenční dokumentace pro událost po dokončení úlohy Batch."
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
ms.openlocfilehash: c126bf897071c008be3d24190cf77bba5878b807
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="task-complete-event"></a>Události po dokončení úloh

 Tato událost je vygenerované po dokončení úlohy, bez ohledu na to hello ukončovací kód. Tato událost může být použité toodetermine hello dobu trvání nějakého úkolu, kde byla spuštěna úloha hello a jestli byl opakovat.


 Hello následující příklad ukazuje textu hello události dokončení úlohy.

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
        "startTime": "2016-09-08T16:32:23.799Z",
        "endTime": "2016-09-08T16:34:00.666Z",
        "exitCode": 0,
        "retryCount": 0,
        "requeueCount": 0
    }
}
```

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|JobId|Řetězec|id Hello hello úlohy, která obsahuje úlohu hello.|
|id|Řetězec|id Hello hello úlohy.|
|taskType|Řetězec|Typ Hello hello úlohy. To může být JobManager oznamující, že je úkol správce nebo uživatel oznamující, že se nejedná o úkolu Správce úloh. Tato událost není vygenerované pro spuštění úlohy, uvolnění úloh nebo přípravy úlohy.|
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
|numberOfInstances|Int32|Hello počet výpočetních uzlů, které vyžadují hello úloh.|

###  <a name="constraints"></a>omezení

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|Hello maximální počet opakovaných úkolů hello. Hello služba Batch úkol zopakuje, pokud je jeho ukončovací kód nenulové hodnoty.<br /><br /> Všimněte si, že tato hodnota řídí konkrétně hello počet opakování. Služba Batch Hello se pokusí hello úloh jednou a mohou zkuste si toothis limit. Například pokud hello maximální počet opakování je 3, Batch pokusí úlohu až too4 dobu (jeden počáteční pokus a 3 opakování).<br /><br /> Pokud hello maximální počet opakování 0, služba Batch hello neopakuje úlohy.<br /><br /> Pokud hello maximální počet opakování −1, služba Batch hello opakuje úlohy bez omezení.<br /><br /> Hello výchozí hodnota je 0 (bez opakování).|

###  <a name="executionInfo"></a>executionInfo

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|startTime|Data a času|Hello čas, který úkol hello spuštění. "Spuštěný" odpovídá toohello **systémem** stavu, takže pokud úloha hello Určuje soubory prostředků nebo balíčky aplikací, pak počáteční čas hello odráží hello čas, který hello úloha spuštěna stahování nebo nasazování těchto.  Pokud úloha hello byl restartován nebo opakovat, je to hello spuštění poslední čas, který úkol hello.|
|endTime|Data a času|Hello čas, který hello úkol dokončit.|
|exitCode|Int32|ukončovací kód Hello hello úlohy.|
|retryCount|Int32|Hello počet oznámení, která má byla hello úloha opakovat hello služby Batch. Hello úlohy se pokus o Pokud ukončí nenulový ukončovací kód, až toohello zadaný MaxTaskRetryCount.|
|requeueCount|Int32|Hello kolikrát hello úloh má byla zařazena službou Batch hello hello důsledku požadavku uživatele.<br /><br /> Pokud odebere uživatele hello uzly z fondu (nebo změnou velikosti zmenšení fondu hello) nebo když je úloha hello zakázaná, hello uživatele můžete určit, že spuštění úlohy v uzlech hello být zařazena pro provedení. Tento počet sleduje počet opakování úkolů hello byla zařazena. z těchto důvodů.|
