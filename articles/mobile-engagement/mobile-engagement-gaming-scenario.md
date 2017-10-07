---
title: "implementace aaaAzure Mobile Engagementu pro herní aplikace"
description: "Herní aplikace scénář tooimplement Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2cafc044-4902-4058-8037-49399bf6bf7f
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: b82b4a868a33f42e5b759e43e66103556c097f9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="implement-mobile-engagement-with-gaming-app"></a>Implementace s herní aplikace Mobile Engagement
## <a name="overview"></a>Přehled
Spuštění herní má spuštění nové aplikace herní play role, strategie rybolovu na základě. herní Hello byl spuštěný a funkční po dobu 6 měsíců. Tato hra je velký úspěšné a má miliony souborů ke stažení a uchování hello je velmi vysoká porovnání tooother spuštění herní aplikace. Na zasedání čtvrtletně zkontrolujte hello zúčastněným stranám souhlas, že které potřebují tooincrease průměrný výnos na uživatele. Premium ve hře balíčky jsou k dispozici jako speciální nabídky. Tyto sady pro herní povolit uživatelům tooupgrade hello vzhled a výkon svých rybářských čar a lures nebo tackles v herním hello. Balíček prodeje jsou však velmi nízké. Proto se rozhodnou první tooanalyze hello na základě zkušeností uživatelů se nástroj pro analýzu a pak toodevelop k engagement program tooincrease prodeje pomocí pokročilé segmentace.

Podle hello [Azure Mobile Engagement – Příručka Začínáme s osvědčenými postupy](mobile-engagement-getting-started-best-practices.md) vytváří strategii zapojení.

## <a name="objectives-and-kpis"></a>Cíle a klíčové ukazatele výkonu.
Klíčových zúčastněných stran pro herní splňují hello. Všechny ke shodě o jeden hlavní cíl - tooincrease premium balíček prodeje podle 15 %. Uživatel vytvořit toomeasure obchodní klíčové ukazatele výkonu (KPI) a disku tohoto cíle

* Na kterou úroveň ve hře hello jsou tyto balíčky zakoupili?
* Co je hello tržby na uživatele, na relaci, za týden a za měsíc?
* Jaké jsou typy Oblíbené nákupu hello?

Část 1 / hello [– Příručka Začínáme](mobile-engagement-getting-started-best-practices.md) vysvětluje, jak toodefine hello cílů a klíčových ukazatelů výkonu. 

U hello, které teď definované klíčové ukazatele výkonu vytvoří hello Mobile produktu Manager klíčové ukazatele zapojení toodetermine nové uživatele trendy a jejich uchovávání.

* Monitorování uchovávání a použít v rámci hello následující intervaly: každý den, každý 2 dní, každý týden, měsíc a každé 3 měsíce
* Počty aktivního uživatele
* hodnocení aplikace Hello v hello uložit

Na základě doporučení z hello IT tým, hello následující technické klíčové ukazatele výkonu byly přidány hello tooanswer následující otázky:

* Co je cesta k uživatele (je navštívené stránky, která, kolik času stráví na něm)
* Počet havárií nebo oznámení chyb došlo na relaci
* Jaké verze operačního systému jsou mé uživatelé používají?
* Jaká je průměrná velikost hello obrazovky pro svoje uživatele?
* Jaký druh připojení k Internetu Moji uživatelé mají?

Pro každý ukazatel KPI hello Mobile produktu Manager určuje hello data Jana potřebuje, a kde se nachází v jeho scénářem.

## <a name="engagement-program-and-integration"></a>Integrace produktů a program zapojení
Před vytvořením program k pokročilé zapojení, měli hello Mobile projektu ředitel starosti hello projektu hluboké znalosti jak a kdy jsou uživatelé hello uplatníte produkty.

Po 3 měsíce shromáždil hello Mobile projektu ředitel dostatek dat tooenhance jeho prodej v aplikaci nabízená oznámení. Dozví, který:

* první nákup Hello obecně se odehrává na úrovni hello 14. Hello nákupu pro 90 % případy, je nové Legendární zbraní $3.
* V těchto případech 80 % uživatelé, kteří provedli nákup, pokračovat s produktem hello a nastavit další uzavře.
* Uživatelé, kteří uplynulo hello úrovně 20, spusťte toospend více než 10 za týden.
* Uživatelé zpravidla toobuy premium balíčky na úrovni 16, 24 a 32.

Děkujeme vám toothis analytických hello Mobile projektu ředitel rozhodne toocreate konkrétní nabízená oznámení pořadí tooincrease v aplikaci prodej. Vytvoří tři sekvencí nabízených oznámení, které mu volá: Vítejte program, Program prodeje a neaktivní programu. Další informace najdete v části toohello [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)![][1]

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
