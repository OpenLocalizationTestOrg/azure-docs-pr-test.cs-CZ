---
title: "AAA \"Azure Analysis Services kurz lekce 8 vytvořit perspektivy | Microsoft Docs\""
description: Popisuje, jak hello toocreate perspektivy v kurzu projektu Azure Analysis Services.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.openlocfilehash: 25391813e1969ecb22af4d6f9c1ccd8358d812fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="lesson-8-create-perspectives"></a>Lekce 8: Vytvoření perspektiv

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této lekci vytvoříte perspektivu Internet Sales. Perspektiva definuje zobrazitelnou podmnožinu modelu poskytující zacílené pohledy specifické pro firmu nebo aplikaci. Když se uživatel připojí tooa modelu s použitím perspektivu, uvidí jenom tyto objekty modelu (tabulky, sloupce, míry, hierarchie a klíčových ukazatelů výkonu) jako polí definovaných v této perspektivě. Další, najdete v části toolearn [perspektivy](https://docs.microsoft.com/sql/analysis-services/tabular-models/perspectives-ssas-tabular).
  
Hello perspektivy internetového prodeje, vytvořených v této lekci vyloučí hello DimCustomer tabulky objektu. Když vytvoříte perspektivu, která vyloučí určité objekty z zobrazení, tento objekt se stále existuje v modelu hello. Nebudou se ale zobrazovat v seznamu polí klienta sestav. Počítané sloupce a míry, ať už jsou do perspektivy zahnuté či nikoli, můžou dál provádět výpočty z dat vyloučeného objektu.  
  
účelem této lekci Hello je toodescribe jak toocreate perspektivy a seznámit se s hello nástroje pro tvorbu tabulkový model. Pokud rozbalíte později tento model tooinclude další tabulky, můžete vytvořit další perspektivy toodefine různých hlediska hello modelu, například inventář a organizační jednotky prodej.  
  
Odhadovaný čas toocomplete této lekci: **pět minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí. Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [Lekce 7: vytvoření klíčové ukazatele výkonu](../tutorials/aas-lesson-7-create-key-performance-indicators.md).  
  
## <a name="create-perspectives"></a>Vytvoření perspektiv  
  
#### <a name="toocreate-an-internet-sales-perspective"></a>toocreate Internetu prodej perspektivy  
  
1.  Klikněte na tlačítko hello **modelu** nabídky > **perspektivy** > **vytvořit a spravovat**.  
  
2.  V hello **perspektivy** dialogové okno, klikněte na tlačítko **nové perspektivy**.  
  
3.  Klikněte dvakrát na hello **nové perspektivy** záhlaví sloupce a poté změňte **Internet prodej**.  
  
4.  Vyberte hello všechny tabulky hello *s výjimkou* **DimCustomer**.  
  
    ![aas-lesson8-perspectives](../tutorials/media/aas-lesson8-perspectives.png)
  
    V později lekce použijete hello analyzovat v tootest funkce aplikace Excel se tato perspektiva. Hello seznamu polí kontingenční tabulky aplikace Excel obsahuje každou tabulku s výjimkou hello DimCustomer tabulky.  

## <a name="whats-next"></a>Co dále?
[Lekce 9: Vytvoření hierarchií](../tutorials/aas-lesson-9-create-hierarchies.md)
  
  
  
  
