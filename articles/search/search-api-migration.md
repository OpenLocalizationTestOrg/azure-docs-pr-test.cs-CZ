---
title: "aaaUpgrading toohello rozhraní API služby REST Azure vyhledávání verze 2016-09-01 | Microsoft Docs"
description: "Upgrade toohello rozhraní API služby REST Azure vyhledávání verze 2016-09-01"
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 6183fa6c-48bb-4af7-adae-4be3bc43c3ed
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: brjohnst
ms.openlocfilehash: d0276b9cc52996a59f9aa726c27e62c6082eb908
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrading-toohello-azure-search-service-rest-api-version-2016-09-01"></a>Upgrade toohello rozhraní API služby REST Azure vyhledávání verze 2016-09-01
Pokud používáte verze 2015-02-28 nebo 2015-02-28-Preview Dobrý den [rozhraní REST API služby Azure Search](https://msdn.microsoft.com/library/azure/dn798935.aspx), tento článek vám pomůže při upgradu vaší aplikace toouse hello další všeobecně dostupná verze rozhraní API, 2016-09-01.

Verze 2016-09-01 Dobrý den REST API obsahuje některé změny z předchozích verzí. Toto jsou většinou zpětně kompatibilní, takže měnili kód měli vyžadovat jen minimální úsilí, v závislosti na verzi, které jste používali před. V tématu [tooupgrade kroky](#UpgradeSteps) pokyny toochange váš kód toouse hello nové verze rozhraní API.

> [!NOTE]
> Instanci služby Azure Search podporuje několik verzí rozhraní REST API, včetně hello nejnovějšího. Toouse verze můžete pokračovat v případě, že je už hello nejnovějšího, ale doporučujeme migraci nejnovější verze vašeho kódu toouse hello.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2016-09-01"></a>Co je nového ve verzi 2016-09-01
Verze 2016-09-01 je hello druhý všeobecně dostupná verze hello rozhraní REST API služby Azure Search. Nové funkce v této verzi rozhraní API:

* [Vlastní analyzátorů](https://aka.ms/customanalyzers), které umožňují tootake kontrolu nad hello proces převodu textu indexovanou a vyhledávat tokenů.
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) a [Azure Table Storage](search-howto-indexing-azure-tables.md) indexery, které umožňují tooeasily importovat data ze služby Azure storage do Azure Search v plánu nebo na vyžádání.
* [Pole mapování](search-indexer-field-mappings.md), které umožňují toocustomize indexery jak importovat data do Azure Search.
* Značky etag binárním rozsáhlým, díky tooupdate hello Definice indexy, indexery a zdroje dat způsobem bezpečných souběžnosti. 

<a name="UpgradeSteps"></a>

## <a name="steps-tooupgrade"></a>Kroky tooupgrade
Pokud provádíte upgrade z verze 2015-02-28, pravděpodobně nebudete mít toomake tooyour žádné změny kódu než číslo verze toochange hello. Hello pouze situace, ve kterých budete pravděpodobně toochange kódu jsou při:

* Váš kód selže, když nerozpoznané vlastnosti jsou vráceny v odpovědi rozhraní API. Ve výchozím nastavení je třeba vlastnosti, které nerozumí ignorovat vaší aplikace.
* Váš kód potrvají žádostí o rozhraní API a pokusí tooresend je nová verze rozhraní API toohello. Například tomu může dojít, pokud vaše aplikace bude pokračování tokeny vrácená z hello rozhraní API služby Search (Další informace vyhledejte `@search.nextPageParameters` v hello [referenční dokumentace rozhraní API pro vyhledávání](https://msdn.microsoft.com/library/azure/dn798927.aspx#Anchor_1)).

Pokud některý z těchto situacích použít tooyou, pak musíte toochange kódu odpovídajícím způsobem. Jinak, žádné změny by měl být nutné, pokud chcete, aby toostart pomocí hello [nové funkce](#WhatsNew) verze 2016-09-01.

Pokud provádíte upgrade z verze 2015-02-28-Preview, hello výše uvedené platí také, ale také musí být vědět, že některé funkce preview nejsou dostupné ve verzi 2016-09-01:

* Podpora indexeru Azure Blob Storage pro soubory CSV a objekty BLOB obsahující pole JSON.
* Synonyma.
* "Informace jako to" dotazy

Pokud váš kód používá tyto funkce, nebudete moct tooupgrade too2016-09-01 bez odebrání vašeho využití z nich.

> [!IMPORTANT]
> Prosím mějte na paměti, verzi preview rozhraní API jsou určené pro testování a vyhodnocení a neměl by se používat v produkčním prostředí.
> 
> 

## <a name="conclusion"></a>Závěr
Pokud potřebujete další podrobnosti o použití hello rozhraní REST API služby Azure Search, přečtěte si hello nedávno aktualizován [referenční dokumentace rozhraní API](https://msdn.microsoft.com/library/azure/dn798935.aspx) na webu MSDN.

Uvítáme vaše názory na Azure Search. Pokud narazíte na potíže, myslíte, že volné tooask nám nápovědu k hello [fórum Azure Search MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) nebo [StackOverflow](http://stackoverflow.com/). Pokud dotaz se žádostí o Azure hledání StackOverflow, ujistěte se, že tootag její `azure-search`.

Děkujeme za používání služby Azure Search.

