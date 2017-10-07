---
title: "aaaInteract s sestav pomocí hello rozhraní API jazyka JavaScript | Microsoft Docs"
description: "Power BI Embedded, interakce se zprávami pomocí hello rozhraní API jazyka JavaScript"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: bdd885d3-1b00-4dcf-bdff-531eb1f97bfb
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
ms.openlocfilehash: 657e4d5cee031bdda173ab3f451cc19b93ddb17b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="interact-with-power-bi-reports-using-hello-javascript-api"></a>Ovládejte sestavy Power BI pomocí hello rozhraní API jazyka JavaScript
umožňuje rozhraní API jazyka JavaScript Power BI Hello je tooeasily vložení sestavy Power BI do vaší aplikace. S hello rozhraní API mohou vaše aplikace prostřednictvím kódu programu komunikovat s prvky různé sestavy jako stránky a filtry. Díky této interaktivitě se sestavy Power BI stanou integrálnější součástí vaší aplikace.

Vložení sestavy Power BI ve vaší aplikaci pomocí elementu iframe, který je hostován v rámci aplikace hello. Hello iframe slouží jako hranice mezi aplikací a hello sestavy, jak můžete vidět v hello následující obrázek. 

![Vložený element iframe Power BI bez rozhraní API pro Javascript](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-1.png)

Hello iframe hello vložení bylo mnohem snazší proces se však není hello rozhraní API jazyka JavaScript hello sestavy a aplikace nemohou komunikovat s navzájem. Kvůli chybějící interakce může být působí jako skutečně hello sestava není součástí aplikace hello. Hello sestavy a aplikaci skutečně potřebujete toocommunicate a zpět, stejně jako hello následující obrázek.

![Vložený element iframe Power BI s rozhraním API pro Javascript](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-2.png)

Hello rozhraní API jazyka JavaScript Power BI vám umožňuje toowrite kód, který můžete bezpečně předávat hello iframe hranic. Díky této může vaše aplikace tooprogrammatically provedení akce v sestavu a toolisten pro události z akcí, které uživatelé provádět v rámci sestavy hello.

## <a name="what-can-you-do-with-hello-power-bi-javascript-api"></a>Co se děje s hello rozhraní API jazyka JavaScript Power BI?
Hello JavaScript API můžete spravovat sestavy, přejděte toopages v sestavě, filtrování sestavy a zpracování události vložení. Hello následující diagram znázorňuje hello struktura hello rozhraní API.

![Diagram rozhraní API pro JavaScript Power BI](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-3.png)

### <a name="manage-reports"></a>Správa sestav
Hello Javascript rozhraní API umožňuje toomanage chování na úrovni sestavy a stránku hello:

* Vložení konkrétní sestavy Power BI bezpečně ve vaší aplikaci – zkuste hello [vložení ukázkové aplikace](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)
  * Nastavení přístupového tokenu
* Konfigurace sestav hello
  * Povolit a zakázat podokno filtru hello a podokno navigace stránky – zkuste hello [aktualizovat nastavení ukázkové aplikace](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  * Nastavit výchozí hodnoty pro stránky a filtry – zkuste hello [ukázkové sady výchozí hodnoty](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
* Nastavení a ukončení režimu zobrazení na celé obrazovce

[Další informace o vkládání sestav](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)

### <a name="navigate-toopages-in-a-report"></a>Přejděte toopages v sestavě
rozhraní API jazyka JavaScript enbales Hello je toodiscover všechny stránky na sestavu a tooset hello aktuální stránce. Zkuste hello [navigační ukázkové aplikace](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[Další informace o navigaci stránkami](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a>Filtrování sestavy
Hello JavaScript API poskytuje základní a pokročilé schopnosti filtrování pro vložené sestavy a stránky sestavy. Zkuste hello [filtrování ukázkové aplikace](http://azure-samples.github.io/powerbi-angular-client/#/scenario4)a zkontrolujte sem některé úvodní kód.  

#### <a name="basic-filters"></a>Základní filtry
Základní filtr je umístěn na úrovni sloupce nebo hierarchie a obsahuje seznam hodnot tooinclude nebo vyloučení.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### <a name="advanced-filters"></a>Rozšířené filtry
Rozšířené filtry pomocí logického operátoru hello a nebo OR a přijměte podmínky jedno nebo dvě, každou s vlastní operátor a hodnotu. Podporované podmínky:

* Žádný
* LessThan
* LessThanOrEqual
* GreaterThan
* GreaterThanOrEqual
* Contains
* DoesNotContain
* StartsWith
* DoesNotStartWith
* Is
* IsNot
* IsBlank
* IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[Další informace o filtrování](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)

### <a name="handling-events"></a>Zpracování událostí
Kromě toho toosending informace do hello iframe, vaše aplikace můžete také získat informace o hello následující události pocházející ze hello iframe:

* Embed
  * loaded
  * error
* Reports
  * pageChanged
  * dataSelected (už brzy)

[Další informace o zpracování událostí](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)

## <a name="next-steps"></a>Další kroky
Další informace o hello Power BI JavaScript rozhraní API projděte si hello následující odkazy:

* [Wikiweb rozhraní API pro JavaScript](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
* [Referenční informace k objektovému modelu](https://microsoft.github.io/powerbi-models/modules/_models_.html)
* Ukázky
  * [Angular](http://azure-samples.github.io/powerbi-angular-client)
  * [Ember](https://github.com/Microsoft/powerbi-ember)
* [Živá ukázka](https://microsoft.github.io/PowerBI-JavaScript/demo/)

