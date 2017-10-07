---
title: "aaaMobile Engagement exportovat přehled rozhraní API"
description: "Další informace, že hello základní informace o exportu nezpracovaných dat generované tooleverage zařízení uživatele ho ve vlastní nástroje pro správu"
services: mobile-engagement
documentationcenter: mobile
author: kpiteira
manager: erikre
editor: 
ms.assetid: 9380d47b-d7fa-4d4c-888f-97e6482196bb
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 04/26/2016
ms.author: kapiteir
ms.openlocfilehash: f55be29a29878e74f6a33419f08a5574a07a7478
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="mobile-engagement-export-api-overview"></a>Přehled rozhraní API Export Mobile Engagementu
## <a name="introduction"></a>Úvod
V tomto dokumentu se dozvíte, že hello základní informace o exportu nezpracovaných dat generované tooleverage zařízení uživatele ho ve vlastní nástroje pro správu.

## <a name="pre-requisites"></a>Požadavky
Export hello nezpracovaná data z Mobile Engagement vyžaduje:

* Rozhraní API ověřování instalace toobe možné toouse hello rozhraní API (viz [ruční instalaci ověřování](mobile-engagement-api-authentication-manual.md)),
* Použít hello rozhraní REST API nebo hello [.net SDK](mobile-engagement-dotnet-sdk-service-api.md),
* Účet úložiště Azure.

> [!NOTE]
> Doporučujeme také hello vynikající [Microsoft Azure Storage Explorer](http://storageexplorer.com/), alespoň v průběhu fáze vývoje hello jak poskytuje snadno toouse uživatelského rozhraní pro interakci s Azure Storage.
> 
> 

## <a name="what-can-be-exported"></a>Co je možné exportovat?
Mobile Engagement umožňuje jeho uživatelé toocollect mnoho typů dat, a proto má několik typů export vhodné toothese různé datové typy.
Existují 2 základní typy exportu:

* Snímek: používá obvykle tooexport data představující stavu a pro které Mobile Engagement nemá historie. To zahrnuje třeba značky (app-info), tokeny nebo nabízené kampaně názory. V důsledku těchto typů export nejsou související tooa datum.
* historické: Tento typ exportu se používá pro data, která shromažďuje časem například události nebo aktivity, například.

Následující tabulka Hello popisuje podrobně všechny hello možné export:

| Typ exportu | Datový typ | Popis |
| --- | --- | --- |
| Snímek |Nabízená oznámení |Generuje o export nabízené kampaně názory na základě za deviceid/ID uživatele |
| Snímek |Značka |Generuje o export hello značky (app-info) přidružené tooeach zařízení |
| Snímek |Zařízení |Generuje o export většinu hello data o zařízení, jako jsou technicals hello (modelu, národní prostředí, časové pásmo,...), hello značky, zaznamenané poprvé... |
| Snímek |Token |Generuje o export všechny platné tokeny hello |
| Historie |Aktivita |Generuje o export všechny hello aktivity pro každé zařízení v daném časovém období |
| Historie |Událost |Generuje o export všechny hello aktivity pro každé zařízení v daném časovém období |
| Historie |Úloha |Generuje o export všechny hello úlohy pro každé zařízení v daném časovém období |
| Historie |Chyba |Generuje o export všech hello chyb pro každé zařízení v daném časovém období |

## <a name="how-does-it-work"></a>Jak to funguje?
Exportuje jsou dlouho spuštěných úloh, který může vytvořit velké datové soubory. Z tohoto důvodu nelze vyvolaná tooreturn okamžitě toodownload souboru.
V pořadí tooexport data z Mobile Engagement, bude mít toocreate **úlohy exportu** prostřednictvím rozhraní API, kde můžete určit obecně:

* Typ Hello export (snímek nebo historické),
* Hello datový typ
* Hello **kontejneru úložiště Azure** (včetně platný SAS s přístup pro zápis) zápis hello výsledek hello export.
* například parametr URL kontejneru příkladu by https://[StorageAccountName].blob.core.windows.net/[ContainerName]? [SASWritePermissionsToken]  

Tady je příklad skutečném světě. https://testazmeexport.BLOB.Core.Windows.NET/test1234azme?SV=2015-12-11&SS=b&SRT=SCO&SP=rwdlac&se=2016-12-17T04:59:26Z & st = 2016-12-16T20:59:26Z & spr = https & sig = KRF3aVWjp2NEJDzjlmoplmu0M9HHlLdkBWRPAFmw90Q % 3D

Upozorňujeme, že může trvat několik minut, než vaše toobe úloha spuštěna, a pak může spustit za několik sekund hodin tooseveral jen nepatrnou aplikací pro aplikace s velkým množstvím uživatelů nebo aktivity.

Po vytvoření úlohy hello je možné toocheck toosee její stav, pokud je stále spuštěná nebo pokud byla dokončena.

Jakmile úloha hello je úspěšně, je k dispozici na hello zadaný kontejner úložiště hello výsledný datový soubor.

