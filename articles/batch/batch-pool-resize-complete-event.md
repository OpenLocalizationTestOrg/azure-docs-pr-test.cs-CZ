---
title: "AAA \"fondu Azure Batch změnit velikost událost complete | Microsoft Docs\""
description: "Referenční dokumentace pro fondu Batch změnit velikost událost complete."
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
ms.openlocfilehash: dc64711a01aa4cf6192edba1a2c4cad56f953766
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="pool-resize-complete-event"></a>Dokončení událost změny velikosti fondu

 Tato událost je vygenerované při změny velikosti fondu má byla dokončena nebo se nezdařilo.

 Hello následující příklad ukazuje textu hello událost complete změny velikosti fondu pro skupinu, která tento nárůst velikosti a byla úspěšně dokončena.

```
{
    "id": "p_1_0_01503750-252d-4e57-bd96-d6aa05601ad8",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 4,
    "targetDedicated": 4,
    "enableAutoScale": false,
    "isAutoPool": false,
    "startTime": "2016-09-09T22:13:06.573Z",
    "endTime": "2016-09-09T22:14:01.727Z",
    "result": "Success",
    "resizeError": "hello operation succeeded"
}
```

|Element|Typ|Poznámky|
|-------------|----------|-----------|
|id|Řetězec|Hello id fondu hello.|
|nodeDeallocationOption|Řetězec|Určuje, kdy může odebrat uzly hello fondu, pokud zmenšování velikosti fondu hello.<br /><br /> Možné hodnoty:<br /><br /> **requeue** – ukončí se všechny spuštěné úlohy a znovu je. Hello úkoly se znovu spustí, když je povolena úloha hello. Odeberte uzly, jakmile úkoly ukončí.<br /><br /> **Ukončit** – ukončit spuštěné úkoly. Hello úkoly se znovu nespustí. Odeberte uzly, jakmile úkoly ukončí.<br /><br /> **taskcompletion** – povolit aktuálně spuštěné úlohy toocomplete. Plánovat žádné nové úkoly při čekání. Odeberte uzly po dokončení všech úloh.<br /><br /> **Retaineddata** – povolit aktuálně spuštěné úlohy toocomplete a potom počkejte, než všech úkolů tooexpire období uchovávání dat. Plánovat žádné nové úkoly při čekání. Odeberte uzly, pokud vypršela platnost období uchovávání všech úloh.<br /><br /> Hello výchozí hodnota je requeue.<br /><br /> Pokud je zvýšení velikosti fondu hello pak hello hodnota příliš**neplatný**.|
|currentDedicated|Int32|Hello počet výpočetních uzlů aktuálně přiřazená toohello fondu.|
|targetDedicated|Int32|Hello počet výpočetních uzlů, které jsou požadovány pro fond hello.|
|enableAutoScale|BOOL|Určuje, zda velikost fondu hello automaticky přizpůsobí v čase.|
|isAutoPool|BOOL|Určuje, zda hello fondu se vytvořil prostřednictvím úlohy AutoPool mechanismus.|
|startTime|Data a času|Dobrý den, když velikost fondu hello spustit.|
|endTime|Data a času|Dobrý den, když velikost fondu hello dokončit.|
|resultCode|Řetězec|změnit velikost Hello výsledek hello.|
|resultMessage|Řetězec|Chyba změny velikosti Hello obsahuje hello podrobnosti výsledku hello.<br /><br /> Pokud velikost hello byla úspěšně dokončena ho stavy, které hello operace byla úspěšná.|
