---
title: "Azure Batch úloh selhání událostí | Microsoft Docs"
description: "Referenční informace pro úlohy Batch nezdaří událostí."
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
ms.openlocfilehash: 08feb4ec34bb1635f8ea744b54a10b677b94ab3e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="task-fail-event"></a>Událost selhání úlohy

 Tato událost je vygenerované při úloha skončí s chybou. Aktuálně všechny nenulový ukončovací kódy se považují za selhání. Tato událost bude vygenerované. *kromě* úlohu dokončit událostí a slouží k rozpoznat, kdy se úloha se nezdařila.


 Následující příklad ukazuje, těle úlohy nezdaří událostí.

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
        "exitCode": 1,
        "retryCount": 2,
        "requeueCount": 0
    }
}
```

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|JobId|Řetězec|Id úlohy, která obsahuje úlohu.|
|id|Řetězec|Id úkolu.|
|taskType|Řetězec|Typ úlohy. To může být JobManager oznamující, že je úkol správce nebo uživatel oznamující, že se nejedná o úkolu Správce úloh. Tato událost není vygenerované pro spuštění úlohy, uvolnění úloh nebo přípravy úlohy.|
|systemTaskVersion|Int32|Toto je Čítač opakovaných pokusů interní na úlohu. Služba Batch interně může pokus zopakovat úlohu, aby se zohlednily přechodné problémy. Tyto problémy mohou zahrnovat plánování s interními chybami nebo pokusy o obnovení z výpočetních uzlů ve špatném stavu.|
|[nodeInfo](#nodeInfo)|Komplexní typ|Obsahuje informace o výpočetním uzlu, na kterém byla úloha spuštěna.|
|[multiInstanceSettings](#multiInstanceSettings)|Komplexní typ|Určuje, že úkol je úlohu s více instancemi nutnosti několika výpočetních uzlech.  V tématu [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) podrobnosti.|
|[omezení](#constraints)|Komplexní typ|Provádění omezení, které se vztahují k tomuto úkolu.|
|[executionInfo](#executionInfo)|Komplexní typ|Obsahuje informace o provádění úlohy.|

###  <a name="nodeInfo"></a>nodeInfo

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|poolId|Řetězec|Id fondu, ve kterém byla spuštěna úloha.|
|nodeId|Řetězec|Id uzlu, na kterém byla úloha spuštěna.|

###  <a name="multiInstanceSettings"></a>multiInstanceSettings

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|numberOfInstances|Int32|Celkový počet výpočetních uzlů požadovaných úkolem|

###  <a name="constraints"></a>omezení

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|Maximální počet, který může být Opakovat úlohu. Služba Batch úkol zopakuje, pokud je jeho ukončovací kód nenulové hodnoty.<br /><br /> Všimněte si, že tato hodnota konkrétně určuje počet opakovaných pokusů. Služba Batch bude opakujte úlohu jednou a mohou zkuste až toto omezení. Například pokud je maximální počet opakování je 3, Batch pokusů a úloh až 4 případech (jeden počáteční pokus a 3 opakování).<br /><br /> Pokud je maximální počet opakování 0, služba Batch neopakuje úlohy.<br /><br /> Pokud je maximální počet opakování −1, služba Batch opakuje úlohy bez omezení.<br /><br /> Výchozí hodnota je 0 (bez opakování).|


###  <a name="executionInfo"></a>executionInfo

|Název elementu|Typ|Poznámky|
|------------------|----------|-----------|
|startTime|Data a času|Čas, kdy úloha spustí systémem. "Spuštěný" odpovídá **systémem** stavu, takže pokud úloha Určuje soubory prostředků nebo balíčky aplikací, pak počáteční čas zobrazí dobu, kdy úloha spustí stahování nebo nasazování těchto.  Pokud byl restartován nebo Opakovat úlohu, je to poslední čas, kdy úloha spustí systémem.|
|endTime|Data a času|Čas, kdy je úloha dokončena.|
|exitCode|Int32|Kód ukončení úlohy.|
|retryCount|Int32|Počet, kolikrát má byla úloha opakovat službou Batch. Úlohy se pokus o Pokud ukončí nenulový ukončovací kód, až do zadaného MaxTaskRetryCount.|
|requeueCount|Int32|Počet, kolikrát má byla zařazena úkol službou Batch jako výsledek požadavku uživatele.<br /><br /> Pokud uzly odebere uživatele z fondu (nebo změnou velikosti zmenšení fondu) nebo když je úloha zakázaná, může uživatel zadat, že spuštění úlohy v uzlech být zařazena pro provedení. Tento počet sleduje počet opakování úlohy byla zařazena. z těchto důvodů.|
