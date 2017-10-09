---
ms.assetid: 
title: "aaaAzure omezení pokyny Key Vault | Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 06/21/2017
ms.openlocfilehash: a75cf96bc6503e51f14378bee598bad57e85be82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-throttling-guidance"></a>Azure Key Vault omezení pokyny

Omezení je proces, který zahájení, který omezí hello počet souběžných volání toohello služba Azure nadměrné využití tooprevent prostředků. Službou Azure Key Vault (AZURE) je navrženou toohandle k velkému počtu požadavků. Pokud dojde k čtenáře počet požadavků, omezení požadavků vašeho klienta pomáhá udržovat optimální výkon a spolehlivost hello služby službou AZURE.

Omezení omezení lišit v závislosti na scénáři hello. Například pokud provádíte značný zápisů, možnost hello omezení je vyšší než pokud pouze provádíte čtení.

## <a name="how-does-key-vault-handle-its-limits"></a>Jak Key Vault zpracovávat jeho omezení?

Omezení služby v Key Vault existují tooprevent zneužití prostředků a ujistěte se kvality služeb pro všechny klienty Key Vault. Pokud je překročena prahová hodnota služby, limity Key Vault žádné další požadavky z tohoto klienta v časovém intervalu. V takovém případě Key Vault vrátí stavový kód HTTP 429 (příliš mnoho požadavků), a hello požadavky služeb při selhání. Navíc neúspěšné požadavky, které vrátí 429 směrem omezení omezení hello sledovanými Key Vault. 

Pokud máte platný obchodní případu pro vyšší šířku pásma, kontaktujte nás.


## <a name="how-toothrottle-your-app-in-response-tooservice-limits"></a>Jak toothrottle aplikace v odpovědi tooservice omezení

Hello následují **osvědčené postupy** omezení aplikace:
- Snižte počet hello operací na základě požadavku.
- Snižte četnost hello požadavků.
- Vyhněte se okamžitě opakování. 
    - Všechny požadavky nabíhat proti vaší omezení použití.

Pokud implementujete zpracování chyb vaší aplikace, použijte hello HTTP Chyba kód 429 toodetect hello nutnost omezení na straně klienta. Pokud hello požadavek selže s kódem chyby HTTP 429 znovu, k stále dochází omezení služby Azure. Pokračujte, toouse hello doporučená metoda omezení na straně klienta, opakování hello požadavku, dokud neproběhne úspěšně.

### <a name="recommended-client-side-throttling-method"></a>Doporučený způsob omezení na straně klienta

Na kód chyby protokolu HTTP 429 začněte omezení vašeho klienta pomocí exponenciálního omezení rychlosti přístup:

1. Počkejte 1 sekundu, opakujte žádost
2. Pokud stále omezeny počkejte 2 sekundy, opakujte žádost
3. Pokud stále omezeny čekání 4 a víc sekund, opakujte žádost
4. Pokud stále omezeny čekání 8 sekund, opakujte žádost
5. Pokud stále omezeny čekání 16 sekund, opakujte žádost

Nesmí být v tomto okamžiku získávání kódy odpovědí HTTP 429.

## <a name="see-also"></a>Viz také

Hlubší orientaci omezení na hello cloudu Microsoftu, najdete v části [omezení vzor](https://docs.microsoft.com/azure/architecture/patterns/throttling).

