---
title: "aaaAPI verzích Azure Search | Microsoft Docs"
description: "Zásady verze rozhraní API REST služby Azure Search a hello knihovny klienta v hello .NET SDK."
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 0458053a-164e-4682-a802-00097ecde981
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/11/2017
ms.author: brjohnst
ms.openlocfilehash: 4fa722fad5577c6b254be7fa673eb240fff316a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="api-versions-in-azure-search"></a>Verze rozhraní API ve službě Azure Search
Služba Azure Search zavede funkce aktualizace pravidelně. V některých případech, ale ne vždy tyto aktualizace nám vyžadují toopublish novou verzi naše rozhraní API toopreserve zpětné kompatibility. Publikování nové verze umožňuje toocontrol kdy a jak integrovat aktualizace služby search v kódu.

Platí pokusíme toopublish nové verze pouze v případě potřeby, protože se může týkat některé tooupgrade úsilí toouse nové rozhraní API verze vašeho kódu. Nová verze jsme bude publikovat, pouze pokud bychom někdy potřebovali toochange některých aspektů hello API tak, aby dělí zpětné kompatibility. Tomu může dojít z důvodu opravy tooexisting funkce nebo z důvodu nové funkce, které změnit stávající útoku na rozhraní API.

Jsme podle hello stejná pravidla pro aktualizace sady SDK. Hello SDK služby Azure Search následuje hello [sémantické verze](http://semver.org/) pravidla, která znamená, že jeho verze má tři části: hlavní, malé a číslo (například 1.1.0) sestavení. Vydáváme nové hlavní verze hello SDK jenom v případě změny, které rozdělit zpětné kompatibility. Pro pevné funkce aktualizace jsme se zvýší hello podverze a pro oprav chyb jsme zvýší pouze hello verzi sestavení.

> [!NOTE]
> Instanci služby Azure Search podporuje několik verzí rozhraní REST API, včetně hello nejnovějšího. Toouse verze můžete pokračovat v případě, že je už hello nejnovějšího, ale doporučujeme migraci nejnovější verze vašeho kódu toouse hello. Při použití hello REST API, musíte zadat hello verze rozhraní API v každé žádosti o prostřednictvím parametr api-version hello. Při použití hello .NET SDK, určuje hello verzi hello SDK používáte hello odpovídající verzi systému hello REST API. Pokud používáte starší SDK, můžete dál toorun tento kód se žádné změny i v případě, že je služba hello upgradovaný toosupport novější rozhraní API verze.

## <a name="snapshot-of-current-versions"></a>Snímek aktuální verze
Zde je snímek hello aktuální verze všech tooAzure programovací rozhraní vyhledávání.

| Rozhraní | Nejnovější hlavní verzi | Status |
| --- | --- | --- |
| [.NET SDK](https://aka.ms/search-sdk) |3.0 |Obecně k dispozici, vydání listopadu 2016 |
| [.NET SDK Preview](https://aka.ms/search-sdk-preview) |2.0-preview |Ve verzi Preview vydané srpna 2016 |
| [Rozhraní API služby REST](https://docs.microsoft.com/rest/api/searchservice/) |2016-09-01 |Obecně k dispozici |
| [Verze Preview rozhraní API služby REST](search-api-2015-02-28-preview.md) |2015-02-28-preview |Preview |
| [Správa .NET SDK](https://aka.ms/search-mgmt-sdk) |2015-08-19 |Obecně k dispozici |
| [Rozhraní REST API pro správu](https://docs.microsoft.com/rest/api/searchmanagement/) |2015-08-19 |Obecně k dispozici |

Pro hello rozhraní REST API, včetně hello `api-version` každé volání je vyžadována. Díky tomu snadno tootarget konkrétní verzi, jako je například preview rozhraní API. Hello následující příklad ukazuje, jak hello `api-version` je zadán parametr:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2016-09-01

> [!NOTE]
> I když má každý požadavek `api-version`, doporučujeme použít hello stejnou verzi pro všechny požadavky rozhraní API. To platí hlavně při zavádění nové verze rozhraní API atributů a operací, které nejsou rozpoznány v předchozích verzích. Kombinování verze rozhraní API může mít nežádoucích důsledků a je nutno.
>
> Hello rozhraní API služby REST a REST API pro správu jsou verzí nezávisle na sobě navzájem. Každá podobnost v číslech verzí je náhodný.

Obecně dostupné (nebo GA) rozhraní API lze použít v produkčním prostředí a jsou subjektu tooAzure smlouvy o úrovni služeb. Verze Preview mají povolenými experimentálními funkcemi, které nejsou vždycky migrované tooa GA verze. **Důrazně nedoporučujeme používání preview rozhraní API v produkční aplikace.**

## <a name="about-preview-and-generally-available-versions"></a>O verze Preview a obecně k dispozici
Služba Azure Search vždy předem uvolní povolenými experimentálními funkcemi prostřednictvím hello REST API nejprve pak prostřednictvím předprodejní verze nástroje hello .NET SDK.

Funkce Preview se nezaručuje, že toobe migrovat tooa GA verze. Zatímco funkce verze GA jsou považovány za stabilní a nepravděpodobný toochange s výjimkou hello malé zpětně kompatibilní opravy a vylepšení, jsou k dispozici pro testování a experimentování, s cílem hello shromažďování zpětné vazby na funkce preview funkce návrhu a implementace.

Protože funkce preview jsou toochange předmětu, doporučujeme však proti zápisu produkčním kódu, který přebírá závislost na verze preview. Pokud používáte starší verzi preview, doporučujeme migraci toohello všeobecně dostupná (GA) verze.

Pro hello .NET SDK: pokyny pro migraci kódu najdete na [upgradu hello .NET SDK](search-dotnet-sdk-migration.md).

Obecné dostupnosti znamená, že Azure Search je nyní v části hello smlouvu o úrovni služeb (SLA). Hello SLA naleznete na adrese [smlouvy o úrovni služeb Azure Search](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
