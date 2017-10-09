---
title: "AAA \"počáteční událost změny velikosti fondu Azure Batch | Microsoft Docs\""
description: "Referenční dokumentace pro událost počáteční změny velikosti fondu Batch."
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
ms.openlocfilehash: 2ca2a4f1195c3f785ae5b051b63340f70eecbc22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="pool-resize-start-event"></a>Událost spuštění změny velikosti fondu

 Tato událost je vygenerované při bylo zahájeno změny velikosti fondu. Vzhledem k tomu, že změny velikosti fondu hello je asynchronní událostí, můžete očekávat toobe fondu změny velikosti událost complete vygenerované po hello změnit velikost operace dokončí.

 změnit velikost Hello následující ukázka, že zobrazuje hello tělo události spuštění změny velikosti fondu pro změnu velikosti fondu z 0 too2 uzlů s ruční.

```
{
    "poolId": "myPool1",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 0,
    "targetDedicated": 2,
    "enableAutoScale": false,
    "isAutoPool": false
}
```

|Element|Typ|Poznámky|
|-------------|----------|-----------|
|poolId|Řetězec|Hello id fondu hello.|
|nodeDeallocationOption|Řetězec|Určuje, kdy může odebrat uzly hello fondu, pokud zmenšování velikosti fondu hello.<br /><br /> Možné hodnoty:<br /><br /> **requeue** – ukončí se všechny spuštěné úlohy a znovu je. Hello úkoly se znovu spustí, když je povolena úloha hello. Odeberte uzly, jakmile úkoly ukončí.<br /><br /> **Ukončit** – ukončit spuštěné úkoly. Hello úkoly se znovu nespustí. Odeberte uzly, jakmile úkoly ukončí.<br /><br /> **taskcompletion** – povolit aktuálně spuštěné úlohy toocomplete. Plánovat žádné nové úkoly při čekání. Odeberte uzly po dokončení všech úloh.<br /><br /> **Retaineddata** – povolit aktuálně spuštěné úlohy toocomplete a potom počkejte, než všech úkolů tooexpire období uchovávání dat. Plánovat žádné nové úkoly při čekání. Odeberte uzly, pokud vypršela platnost období uchovávání všech úloh.<br /><br /> Hello výchozí hodnota je requeue.<br /><br /> Pokud je zvýšení velikosti fondu hello pak hello hodnota příliš**neplatný**.|
|currentDedicated|Int32|Hello počet výpočetních uzlů aktuálně přiřazená toohello fondu.|
|targetDedicated|Int32|Hello počet výpočetních uzlů, které jsou požadovány pro fond hello.|
|enableAutoScale|BOOL|Určuje, zda velikost fondu hello automaticky přizpůsobí v čase.|
|isAutoPool|BOOL|Speficies jestli hello fondu se vytvořil prostřednictvím mechanismu AutoPool úlohy.|
