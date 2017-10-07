---
title: "aaaUse hello nebo zobrazení provádění vrcholů v nástrojů Data Lake pro Visual Studio | Microsoft Docs"
description: "Zjistěte, jak toouse hello úloh Data Lake Analytics tooexam nebo zobrazení provádění vrcholů."
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 5366d852-e7d6-44cf-a88c-e9f52f15f7df
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/13/2016
ms.author: jgao
ms.openlocfilehash: fb54d2af8a32aa27a54ff50a73c1b4903677a21e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Použití hello nebo zobrazení provádění vrcholů v nástrojů Data Lake pro Visual Studio
Zjistěte, jak toouse hello úloh Data Lake Analytics tooexam nebo zobrazení provádění vrcholů.

## <a name="prerequisites"></a>Požadavky

Potřebujete základní znalosti o pomocí nástrojů Data Lake pro Visual Studio toodevelop U-SQL skriptů.  V tématu [kurz: vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

## <a name="open-hello-vertex-execution-view"></a>Otevřete zobrazení provádění vrcholů hello
Otevřete úlohy U-SQL v nástrojů Data Lake pro Visual Studio. Klikněte na tlačítko **nebo zobrazení provádění vrcholů** v levém dolním rohu hello. Profily výzvami tooload může být nejprve a může trvat nějakou dobu v závislosti na připojení k síti.

![Zobrazení provádění vrcholů nástroje data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Pochopení nebo zobrazení provádění vrcholů
Hello nebo zobrazení provádění vrcholů má tři části:

![Zobrazení provádění vrcholů nástroje data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

Hello **vrchol selektor** na levém umožňuje hello vyberte vrcholy funkce (například top 10 dat pro čtení, nebo zvolte fázi). Jednou z nejčastěji používaných filtry hello je toosee hello **vrcholy kritické cesty**. Hello **kritické cesty** je hello nejdelší řetězu vrcholy úlohy U-SQL. Principy hello kritická cesta je užitečné pro optimalizaci úlohách kontrolou, které vrchol trvá hello nejdelší dobu.
  
![Zobrazení provádění vrcholů nástroje data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

Hello nejvyšší prostředním podokně zobrazí hello **systémem stav všechny vrcholy hello**.
  
![Zobrazení provádění vrcholů nástroje data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

Hello dolní prostředním podokně se zobrazují informace o jednotlivých vrchol:
* Název procesu: název hello hello vrchol instance. Se skládá z různých částí v StageName | VertexName | VertexRunInstance. Například hello SV7_Split [62] .v1 vrchol znamená hello druhé spuštěné instance (.v1, index od 0) čísla vrchol 62 ve fázi SV7_Split.
* Celkový počet dat pro čtení nebo Written: hello data byla číst nebo zapisovat pomocí této vrchol.
* Stav stavu nebo ukončení: hello konečného stavu po ukončení vrchol hello.
* Ukončovací kód nebo selhání typ: hello došlo k chybě při vrchol hello se nezdařilo.
* Vytvoření důvod: Proč hello vrchol byl vytvořen.
* Latence fronty latence/PN prostředků latenci nebo proces: hello doba hello vrchol toowait pro prostředky, tooprocess data a toostay ve frontě hello.
* Identifikátor GUID procesů nebo autora: Identifikátor GUID pro aktuální spuštěné vrchol hello nebo jeho tvůrce.
* Verze: hello N-tý instanci hello systémem vrchol (hello systému se může naplánovat nové instance třídy vrchol, pro mnoho důvodů, například převzetí služeb při selhání, výpočetní redundance atd.)
* Verze vytvořena, když.
* Zpracování vytvořit počáteční čas nebo proces zařazeno ve frontě čas nebo proces počáteční čas nebo proces dokončete čas: při zahájení procesu vrchol hello vytvoření; Při spuštění procesu vrchol hello tooqueue; Když hello určité spustí proces vrchol; Když hello určité vrchol byla dokončena.

## <a name="next-steps"></a>Další kroky
* toolog diagnostické informace, najdete v části [přístup k protokolů diagnostiky pro Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)
* toosee komplexnější dotaz, najdete v části [analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Podrobnosti úlohy tooview, najdete v tématu [použití úlohy prohlížeče a zobrazení úloh pro úlohy Azure Data lake Analytics](data-lake-analytics-data-lake-tools-view-jobs.md)
