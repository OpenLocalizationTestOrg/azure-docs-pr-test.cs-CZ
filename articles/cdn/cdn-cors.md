---
title: aaaUsing Azure CDN s CORS | Microsoft Docs
description: "Zjistěte, jak toouse hello Azure Content Delivery Network (CDN) toowith sdílení prostředků různých původů (CORS)."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 86740a96-4269-4060-aba3-a69f00e6f14e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 6c743b56c32a2d3aacc9a77094cb87d61b95d2f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-cdn-with-cors"></a>Azure CDN pomocí CORS
## <a name="what-is-cors"></a>Co je CORS?
CORS (mezi sdílení prostředků původu) je funkce protokolu HTTP, která umožňuje webová aplikace spuštěna v rámci jednoho prostředkům tooaccess domény v jiné doméně. V pořadí tooreduce hello možnost útoky skriptování mezi weby, všechny moderní prohlížeče implementovat omezení zabezpečení známé jako [stejného původu zásad](http://www.w3.org/Security/wiki/Same_Origin_Policy).  Zabrání se tak na webové stránce z volání rozhraní API v jiné doméně.  CORS poskytuje bezpečný tooallow jeden počátek (doménu původu hello) toocall rozhraní API v jiný počátek.

## <a name="how-it-works"></a>Jak to funguje
Existují dva typy požadavků CORS *jednoduchých požadavků* a *komplexní požadavky.*

### <a name="for-simple-requests"></a>Pro jednoduché požadavky:

1. Hello prohlížeč odešle požadavek CORS hello s další **původu** hlavičku požadavku HTTP. Hello hodnotu této hlavičky je hello původ, který obsluhuje hello nadřazená stránka, která je definována jako kombinace hello *protokol,* *domény,* a *portu.*  Když se na stránce z https://www.contoso.com pokusí tooaccess uživatelská data v hello fabrikam.com původu, mají být odeslány toofabrikam.com hello následující hlavičky žádosti:

   `Origin: https://www.contoso.com`

2. Hello server může reagovat s žádným z následujících hello:

   * **Access-Control-Allow-Origin** hlavičky v odpovědi označující původ lokality, která je povolena. Například:

     `Access-Control-Allow-Origin: https://www.contoso.com`

   * Chyby protokolu HTTP code například 403, není-li hello server hello cross-origin požadavek po zkontrolování hlavičky počátku hello

   * **Access-Control-Allow-Origin** záhlaví se zástupnými znaky, které umožňuje všechny původy:

     `Access-Control-Allow-Origin: *`

### <a name="for-complex-requests"></a>Pro komplexní požadavky:

Žádost o komplexní je požadavek CORS, kde hello prohlížeče je požadovaná toosend *předběžný požadavek* (tj. předběžné kontroly) před odesláním hello aktuální požadavek CORS. Hello předběžný požadavek požádá oprávnění server hello Pokud hello původní požadavek CORS můžete pokračovat a je `OPTIONS` požadavku toohello stejnou adresu URL.

> [!TIP]
> Další informace o CORS toky a běžné nástrahy zobrazit hello [Průvodce tooCORS pro rozhraní REST API](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).
>
>

## <a name="wildcard-or-single-origin-scenarios"></a>Zástupný znak nebo jeden počátek scénáře
CORS v Azure CDN budou automaticky fungovat žádnou další konfiguraci, když hello **Access-Control-Allow-Origin** záhlaví nastavena toowildcard (*) nebo jeden počátek.  Hello CDN bude ukládat do mezipaměti první odpověď hello a následné žádosti budou používat hello stejné záhlaví.

Pokud požadavky již byly provedeny toohello CDN předchozí tooCORS se nastavuje na hello do zdrojového umístění, budete potřebovat toopurge obsah na váš koncový bod obsahu tooreload hello obsahu s hello **Access-Control-Allow-Origin** záhlaví.

## <a name="multiple-origin-scenarios"></a>Více scénářů počátek
Pokud potřebujete tooallow na konkrétní seznam původů toobe povolené pro CORS, získat věcí trochu složitější. Hello potížím dochází, pokud hello CDN ukládá do mezipaměti hello **Access-Control-Allow-Origin** záhlaví pro první zdroj CORS hello.  Pokud jiný zdroj CORS provede následného požadavku, hello CDN bude sloužit hello mezipaměti **Access-Control-Allow-Origin** hlavičky, která nebude odpovídat.  Existuje několik způsobů toocorrect to.

### <a name="azure-cdn-premium-from-verizon"></a>Azure CDN Premium od Verizonu
Hello tooenable nejlepší způsob, jak jde toouse **Azure CDN Premium od společnosti Verizon**, který zpřístupňuje některé rozšířené funkce. 

Budete potřebovat příliš[vytvořit pravidlo](cdn-rules-engine.md) toocheck hello **původu** hlavičky v požadavku hello.  Pokud je platný původu, pravidla nastaví hello **Access-Control-Allow-Origin** hlavička s hello původu uvedených v žádosti o hello.  Pokud hello počátek zadané v hello **původu** hlavičky není povolena, pravidla měli vynechejte hello **Access-Control-Allow-Origin** záhlaví, což způsobí hello prohlížeče tooreject hello požadavku. 

Existují dva způsoby toodo to s stroj pravidel hello.  V obou případech hello **Access-Control-Allow-Origin** hlavička ze souboru hello zdrojový server je zcela ignorován, stroj pravidel hello CDN provádí kompletní správu hello povolené zdroje CORS.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Jeden regulární výraz s všechny původy platný
V tomto případě vytvoříte regulární výraz, který obsahuje všechny hello původu chcete tooallow: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$

> [!TIP]
> **Azure CDN společnosti Verizon** používá [Perl kompatibilní regulární výrazy](http://pcre.org/) jako jeho modul pro regulární výrazy.  Můžete použít nástroje, jako je [regulární výrazy 101](https://regex101.com/) toovalidate regulární výraz.  Všimněte si, že hello "/" znak je platný v regulárních výrazech a nepotřebuje toobe řídicí sekvencí, ale uvozovací znaky tento znak se považuje za osvědčený postup a očekává se některé validátory regulární výraz.
> 
> 

Pokud regulární výraz hello odpovídá, nahradí pravidla hello **Access-Control-Allow-Origin** záhlaví (pokud existuje) ze zdroje hello s hello původ, který poslal žádost hello.  Můžete také přidat další hlavičky CORS, jako například **přístup – ovládací prvek-Allow-Methods**.

![Příklad pravidla s regulárním výrazem](./media/cdn-cors/cdn-cors-regex.png)

#### <a name="request-header-rule-for-each-origin"></a>Žádost o pravidlo záhlaví pro každý počátek.
Místo regulární výrazy, můžete místo toho vytvořit samostatné pravidlo pro každý původ chcete tooallow pomocí hello **zástupné hlavičky požadavku** [vyhovují podmínce](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1). Jako regulární výraz metodou hello, modul hello pravidel samostatně nastaví hlavičky CORS hello. 

![Příklad pravidla bez regulární výraz](./media/cdn-cors/cdn-cors-no-regex.png)

> [!TIP]
> V předchozím příkladu hello, hello použití hello zástupný znak * informuje hello pravidla modul toomatch HTTP a HTTPS.
> 
> 

### <a name="azure-cdn-standard"></a>Azure CDN Standard
Na Azure CDN Standard profily hello pouze tooallow mechanismus pro více zdroje bez použití hello původu zástupný znak hello je toouse [řetězce dotazu do mezipaměti](cdn-query-string.md).  Nastavení řetězec dotazu tooenable potřebovat pro koncový bod CDN hello a potom pomocí řetězce dotazu jedinečné požadavky z každé povolené domény. To způsobí hello CDN ukládání do mezipaměti samostatný objekt pro každý jedinečný dotazu řetězec. Tento přístup není ideální, ale, jak ho bude mít za následek více kopií hello stejný soubor v mezipaměti na hello CDN.  

