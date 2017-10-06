---
title: "aaaAzure aplikace Insights Telemetrie datový Model - požadavku Telemetrie | Microsoft Docs"
description: "Aplikace Insights datový model pro požadavek telemetrie"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: bwren
ms.openlocfilehash: 6042975a35f5e672e5adb5390feecc63d0b284b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="request-telemetry-application-insights-data-model"></a>Žádosti o telemetrie: Application Insights datový model

Položku telemetrie požadavku (v [Application Insights](app-insights-overview.md)) představuje hello logické pořadí provádění aktivovány aplikace tooyour externí požadavek. Každé provedení požadavku je identifikována jedinečný `ID` a `url` obsahující všechny parametry spuštění hello. Požadavky můžete seskupovat podle logické `name` a definovat hello `source` této žádosti. Může mít za následek provádění kódu `success` nebo `fail` a má určitou `duration`. Úspěchy a chyby jednotlivými spuštěními může být seskupené ještě `resultCode`. Počáteční čas pro telemetrická žádost hello definované na úrovni obálky hello.

Žádost o telemetrie podporuje model rozšiřitelnosti standardní hello pomocí vlastní `properties` a `measurements`.

## <a name="name"></a>Name (Název)

Název žádosti hello představuje požadavek hello tooprocess kód trasu. Mohutnost nízkou hodnotu tooallow lépe seskupování požadavků. Pro požadavky HTTP hello představuje metodu HTTP a šablonu cesty URL jako `GET /values/{id}` bez hello skutečné `id` hodnotu.

Aplikace Insights web SDK odešle žádost o název "tak, jak je namapoval tooletter případem. Seskupení v uživatelském rozhraní je malá a velká písmena, `GET /Home/Index` se počítá samostatně z `GET /home/INDEX` Přestože často vedou hello stejné provádění kontroleru a akce. Hello důvod k tomu je obecně se adresy URL [malá a velká písmena](http://www.w3.org/TR/WD-html40-970708/htmlweb.html). Pokud všechny vhodné toosee `404` došlo pro adresy URL hello zadali velkými písmeny. Můžete si přečíst další na žádost o název kolekce pomocí sady SDK webové technologie ASP.Net v hello [příspěvku na blogu](http://apmtips.com/blog/2015/02/23/request-name-and-url/).

Maximální délka: 1024 znaků

## <a name="id"></a>ID

Identifikátor instance volání požadavku. Slouží pro korelaci mezi žádostí a dalším položkám telemetrie. ID by měly být globálně jedinečný. Další informace najdete v tématu [korelace](application-insights-correlation.md) stránky.

Maximální délka: 128 znaků.

## <a name="url"></a>URL

Adresa URL požadavku se všemi parametry řetězce dotazu.

Maximální délka: 2 048 znaků

## <a name="source"></a>Zdroj

Zdroj žádosti hello. Příklady jsou klíč instrumentace hello hello volající nebo ip adresu hello hello volajícího. Další informace najdete v tématu [korelace](application-insights-correlation.md) stránky.

Maximální délka: 1024 znaků

## <a name="duration"></a>Doba trvání

Žádosti o trvání ve formátu: `DD.HH:MM:SS.MMMMMM`. Musí být kladné a menší než `1000` dnů. Toto pole je povinné, protože telemetrická žádost představuje operaci hello s hello začátek a konec hello.

## <a name="response-code"></a>Kód odpovědi

Výsledek provádění požadavku. Stavový kód HTTP pro požadavky HTTP. To může být `HRESULT` hodnotu nebo výjimka typu pro ostatní typy požadavků.

Maximální délka: 1024 znaků

## <a name="success"></a>Úspěch

Údaj o volání úspěšná nebo neúspěšná. Toto pole je povinné. Pokud není explicitně nastaven příliš`false` -považuje za úspěšnou toobe požadavku. Nastavení této hodnoty příliš`false` Pokud operace byla přerušena výjimkou nebo vrátila kód chyby výsledek.

Application Insights pro webové aplikace hello, definovat požadavku, jako neúspěšná po hello kód odpovědi menší hello `400` nebo roven hodnotě příliš`401`. Ale existují případy, když toto výchozí mapování se neshoduje s hello sémantického aplikace hello. Kód odpovědi `404` může znamenat, že žádné záznamy,", které můžou být součástí regulární toku. Také může to znamenat poškozený odkaz. Pro hello nefunkční odkazy můžete implementovat i pokročilejší logiku. Poškozených odkazů můžete označit jako selhání pouze v případě, že tyto odkazy jsou umístěné ve stejné lokalitě analýzou adresy url odkazující server hello. Nebo označit je jako selhání při přístupu z mobilních aplikací společnosti hello. Podobně `301` a `302` označuje selhání při přístupu z hello klienta, který nepodporuje přesměrování.

Částečně přijmout obsah `206` může znamenat selhání celkové požadavku. Koncový bod služby Application Insights pro instanci přijímá dávky položky telemetrie jako jeden požadavek. Vrátí `206` při některé položky v dávce hello nebyly úspěšně zpracovány. Roste počet `206` označuje problém, který potřebuje toobe prozkoumat. Podobně jako logika platí příliš`207` více stav, kdy může být úspěšné hello hello nejhorší ze samostatných kódů odpovědí.

Můžete si přečíst další výsledek na žádost kód a kód stavu v hello [příspěvku na blogu](http://apmtips.com/blog/2016/12/03/request-success-and-response-code/).

## <a name="custom-properties"></a>Vlastní vlastnosti

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Vlastní měření

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Další kroky

- [Psát vlastní požadavek telemetrii](app-insights-api-custom-events-metrics.md#trackrequest)
- V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.
- Zjistěte, jak příliš[konfigurace ASP.NET Core](app-insights-asp-net.md) aplikace s Application Insights.
- Podívejte se na [platformy](app-insights-platforms.md) nepodporuje Application Insights.
