---
title: "Podpora sdílení (CORS) aaaCross-Origin prostředků | Microsoft Docs"
description: "Zjistěte, jak tooenable podpora CORS pro hello služeb úložiště Microsoft Azure."
services: storage
documentationcenter: .net
author: cbrooksmsft
manager: carmonm
editor: tysonn
ms.assetid: a0229595-5b64-4898-b8d6-fa2625ea6887
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 2/22/2017
ms.author: cbrooks
ms.openlocfilehash: 0a6ec3bf6999c5faa7f0912dc2a47921aa01d3d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="cross-origin-resource-sharing-cors-support-for-hello-azure-storage-services"></a>Podpora sdílení prostředků různých původů (CORS) pro hello služby úložiště Azure
Počínaje verzí 2013-08-15, podporu služby Azure storage hello sdílení prostředků různých původů (CORS) pro služby objektů Blob, tabulky, fronty a soubor hello. CORS je funkce protokolu HTTP, která umožňuje webová aplikace spuštěna v rámci jednoho prostředkům tooaccess domény v jiné doméně. Webové prohlížeče implementovat omezení zabezpečení, označuje jako [stejného původu zásad](http://www.w3.org/Security/wiki/Same_Origin_Policy) která brání webové stránky z volání rozhraní API v jiné doméně. CORS poskytuje bezpečný tooallow jednu doménu (doménu původu hello) toocall rozhraní API v jiné doméně. V tématu hello [specifikace CORS](http://www.w3.org/TR/cors/) podrobnosti o CORS.

Můžete nastavit pravidla CORS samostatně pro každou hello služby úložiště, voláním [nastavit vlastnosti služby objektů Blob](https://msdn.microsoft.com/library/hh452235.aspx), [nastavit vlastnosti fronty služby](https://msdn.microsoft.com/library/hh452232.aspx), a [nastavit vlastnosti služby tabulky](https://msdn.microsoft.com/library/hh452240.aspx). Jakmile nastavíte hello pravidla CORS pro službu hello, pak správně ověřeného požadavku odeslaného službě hello z jiné domény bude vyhodnotí toodetermine zda je povoleno, podle toohello pravidla, která jste zadali.

> [!NOTE]
> Všimněte si, že CORS není ověřovací mechanismus. Všechny požadavek směřovaný na prostředků úložiště, pokud je povoleno CORS musí mít buď správné ověření podpisu, nebo musí být provedeny pro prostředek veřejné.
> 
> 

## <a name="understanding-cors-requests"></a>Pochopení požadavků CORS
Požadavek CORS z počátku domény může obsahovat dva samostatné požadavky:

* Předběžný požadavek, který se dotazuje hello CORS omezení vynucená službou hello. Hello předběžný požadavek není nutná, pokud je metoda žádosti hello [jednoduše](http://www.w3.org/TR/cors/), což znamená, GET, HEAD nebo POST.
* Hello skutečné požadavek směřovaný na hello požadovaného prostředku.

### <a name="preflight-request"></a>Předběžný požadavek
dotazy předběžný požadavek Hello hello CORS omezení, které byly vytvořeny pro službu úložiště hello hello vlastníka účtu. Hello webový prohlížeč (nebo jiné uživatelský agent) odešle požadavek možnosti, která zahrnuje hlavičky požadavku hello, metoda a původu domény. Služba úložiště Hello vyhodnotí operaci hello určené na základě sady předem nakonfigurovaných pravidel CORS, která určit, které zdrojové domény, žádat o metody a hlavičky požadavku může být určen na požadavek skutečné u prostředků úložiště.

Pokud je povolená CORS pro službu hello a existuje pravidlo CORS, která odpovídá předběžný požadavek hello, služba hello odpoví se stavovým kódem 200 (OK) a zahrnuje hello požadované hlavičky Access-Control v odpovědi hello.

Pokud žádné pravidlo CORS odpovídá hello předběžný požadavek, nebo pro hello služby není povoleno CORS, bude odpovídat hello služby se stavovým kódem 403 (zakázáno).

Pokud hello možnosti žádost neobsahuje hello požadované hlavičky CORS (hello původ a hlavičky Access-Control-Request-Method), bude odpovídat hello služby se stavovým kódem 400 (Chybný požadavek).

Všimněte si, že předběžný požadavek je porovnán s hello služby (Blob, fronty a tabulky) a ne proti hello požadovaný prostředek. vlastníka účtu Hello musí ji mít povoleny CORS jako součást hello účet služby vlastností v pořadí pro požadavek toosucceed hello.

### <a name="actual-request"></a>Skutečné požadavku
Po přijato hello předběžný požadavek a odpověď hello se vrátí, bude hello prohlížeče dispatch hello skutečné požadavku vůči hello prostředků úložiště. Hello prohlížeč bude hello skutečné požadavek je odepřen, okamžitě pokud hello předběžný požadavek byl odmítnut.

Hello skutečné požadavek považován za normální požadavku vůči hello služby úložiště. Hello přítomnost hlavičky počátku hello označuje, že je požadavek hello požadavek CORS a hello služba zkontroluje hello odpovídající pravidla CORS. Pokud je nalezena shoda, hlavičky hello řízení přístupu se přidané toohello odpovědi a odesílají zpět toohello klienta. Pokud není nalezena shoda, nebudou zobrazeny hello hlavičky CORS Access-Control.

## <a name="enabling-cors-for-hello-azure-storage-services"></a>Povolení CORS pro služby Azure Storage hello
Pravidla CORS jsou nastavené na úrovni služby hello, takže třeba tooenable nebo zákazu sdílení CORS pro každou službu (objekt Blob, fronty a tabulky) samostatně. Ve výchozím nastavení je zakázané CORS pro každou službu. tooenable CORS, je nutné tooset hello příslušnou službu vlastnosti pomocí verze 2013-08-15 nebo novější a přidejte vlastnosti služby toohello pravidla CORS. Podrobnosti o tooenable nebo zákazu sdílení CORS pro služby a jak tooset CORS pravidla, prosím odkazovat příliš[nastavit vlastnosti služby objektů Blob](https://msdn.microsoft.com/library/hh452235.aspx), [nastavit vlastnosti fronty služby](https://msdn.microsoft.com/library/hh452232.aspx), a [nastavit tabulky Vlastnosti služby](https://msdn.microsoft.com/library/hh452240.aspx).

Zde je ukázka jednoho pravidla CORS, zadaný prostřednictvím operace nastavit vlastnosti služby:

```xml
<Cors>    
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
        <AllowedMethods>PUT,GET</AllowedMethods>
        <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
        <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
        <MaxAgeInSeconds>200</MaxAgeInSeconds>
    </CorsRule>
<Cors>
```

Každý prvek uvedené v pravidle CORS hello je popsán dále:

* **AllowedOrigins**: hello zdrojové domény, které jsou povoleny toomake požadavek hello úložiště služby prostřednictvím CORS. doménu původu Hello je hello domény, ze které hello požadavek pochází. Všimněte si, že hello počátku musí být velká a malá písmena přesně shodovat s hello původu, aby stáří hello uživatel odešle toohello služby. Můžete také použít zástupný znak hello ' *' tooallow všechny požadavky toomake domén počátek prostřednictvím CORS. V předchozím příkladu hello, hello domén [http://www.contoso.com](http://www.contoso.com) a [http://www.fabrikam.com](http://www.fabrikam.com) provádět požadavky DHCP proti hello služby pomocí CORS.
* **AllowedMethods**: metody hello (příkazy požadavku HTTP) tuto doménu původu hello může použít pro požadavek CORS. V předchozím příkladu hello jsou povoleny pouze požadavky PUT a GET.
* **AllowedHeaders**: hlavičky požadavku hello tuto doménu původu hello může určovat na požadavek CORS hello. V předchozím příkladu hello jsou povolené všechny hlavičky metadata počínaje x-ms-meta-data, x-ms-meta cíl a x-ms-meta-abc. Všimněte si, že hello zástupný znak ' *' znamená, že všechny hlavičky počínaje hello určeného předpona je povolen.
* **ExposedHeaders**: hello hlavičky odpovědi, které může odesílat v požadavek CORS toohello hello odpovědi a vystavené hello prohlížeče toohello požadavek vystavitele. V příkladu hello výše hello prohlížeče je pokyn tooexpose počínaje x-ms-meta všechny hlavičky.
* **MaxAgeInSeconds**: hello maximální množství času, prohlížeče by měl mezipaměti hello předběžný požadavek možnosti.

Hello služby Azure storage podpora zadání předponou hlavičky pro obě hello **AllowedHeaders** a **ExposedHeaders** elementy. tooallow kategorii hlaviček, můžete zadat běžné kategorii toothat předponu. Například zadání *x-ms-meta** jako hlavičku předponou Určuje pravidlo, které bude odpovídat všechny hlavičky, které začínají s x-ms-meta.

tooCORS pravidla použít Hello následující omezení:

* Můžete zadat až toofive pravidla CORS pro službu úložiště (objekt Blob, Table a Queue).
* maximální velikost Hello všechny pravidla nastavení CORS na hello požadavku, s výjimkou značky XML nesmí být delší než 2 KB.
* Hello povolené hlavičky, vystaveného hlavičce nebo povolený původ by neměl být delší než 256 znaků.
* Povolené hlavičky a zveřejněné záhlaví může být buď:
  * Literál hlavičky, kde hello přesný záhlaví je zadaný název, například **x-ms-meta zpracovat**. U požadavku hello je možné zadat maximálně 64 literálu hlavičky.
  * Předponu hlavičky, kde předponu hello záhlaví je k dispozici, jako například **x-ms-meta-data***. Zadání předpony tímto způsobem umožňuje nebo zpřístupní všechny hlavičky, který začíná textem hello zadané předpony. U požadavku hello je možné zadat maximálně dvě předponou hlavičky.
* Hello metody (nebo příkazy HTTP) zadaný v hello **AllowedMethods** element musí odpovídat toohello metod podporovaných službou Azure storage rozhraní API. Jsou podporované metody odstranění, GET, HEAD, SLOUČENÍ, POST, možnosti a PUT.

## <a name="understanding-cors-rule-evaluation-logic"></a>Principy logiku vyhodnocení pravidla CORS
Když služba úložiště obdrží žádost o předběžný nebo skutečné, vyhodnotí tohoto požadavku na základě pravidel hello CORS, které jste vytvořili pro službu hello prostřednictvím hello příslušné operace nastavit vlastnosti služby. Pravidla CORS jsou vyhodnocovány v hello pořadí, ve které byly nastavené v textu žádosti hello Dobrý den operaci nastavit vlastnosti služby.

CORS pravidla se vyhodnocují následujícím způsobem:

1. Nejprve je doménu původu hello hello požadavku zkontrolován hello domén uvedené pro hello **AllowedOrigins** element. Pokud doménu původu hello je obsažena v seznamu hello nebo se zástupným znakem hello jsou povoleny všechny domény ' *', potom pravidla vyhodnocení bude pokračovat. Pokud doménu původu hello neuvedete, hello požadavku se nezdaří.
2. V dalším kroku metoda hello (nebo příkaz HTTP) hello požadavku je zkontrolován hello metody uvedené v hello **AllowedMethods** element. Pokud metoda hello je obsažena v seznamu hello, potom pokračuje pravidla vyhodnocení; Pokud ne, pak hello požadavek selže.
3. Pokud hello požadavek odpovídá pravidla v jeho zdrojovou doménu a jeho metoda, toto pravidlo je vybrané tooprocess hello požadavku a vyhodnocují se žádná další pravidla. Předtím, než mohlo být úspěšné hello požadavku, však kontroluje všechny hlavičky zadaný u žádosti hello vůči hello hlavičky uvedené v hello **AllowedHeaders** element. Pokud hello hlavičky odeslané neshodují hello povolené hlavičky, hello požadavek selže.

Vzhledem k tomu, že hello pravidla se zpracovávají v pořadí hello, že se nachází v textu žádosti hello, osvědčené postupy doporučujeme zadat, že hello nejvíc omezující pravidla s respektují tooorigins první v seznamu hello tak, aby tyto se vyhodnocují jako první. Zadejte pravidla, která jsou méně omezující – například pravidlo tooallow všechny původy – na konci hello hello seznamu.

### <a name="example--cors-rules-evaluation"></a>Příklad – CORS pravidel vyhodnocení
Hello následující příklad ukazuje obsah částečné žádosti pro operaci tooset CORS pravidla pro služby úložiště hello. V tématu [nastavit vlastnosti služby objektů Blob](https://msdn.microsoft.com/library/hh452235.aspx), [nastavit vlastnosti fronty služby](https://msdn.microsoft.com/library/hh452232.aspx), a [nastavit vlastnosti služby Table](https://msdn.microsoft.com/library/hh452240.aspx) informace o vytváření hello požadavku.

```xml
<Cors>
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
        <AllowedMethods>PUT,HEAD</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
    </CorsRule>
    <CorsRule>
        <AllowedOrigins>*</AllowedOrigins>
        <AllowedMethods>PUT,GET</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
    </CorsRule>
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
        <AllowedMethods>GET</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-client-request-id</AllowedHeaders>
    </CorsRule>
</Cors>
```

Dále je třeba zvážit následující požadavků CORS hello:

| Žádost |  |  | Odpověď |  |
| --- | --- | --- | --- | --- |
| **– Metoda** |**Počátek** |**Hlavičky požadavku** |**Pravidla shody** |**Výsledek** |
| **PUT** |http://www.contoso.com |x-ms-blob-content-type |První pravidlo |Úspěch |
| **GET** |http://www.contoso.com |x-ms-blob-content-type |Druhé pravidlo |Úspěch |
| **GET** |http://www.contoso.com |x-ms-client-request-id |Druhé pravidlo |Selhání |

Hello první požadavek odpovídá první pravidlo hello – doménu původu hello odpovídá hello povolené zdroje, hello metoda odpovídá hello povolené metody a hlavičky hello odpovídá hello povolené hlavičky – a proto úspěšné.

druhý žádosti Hello neodpovídá hello první pravidlo, protože metoda hello neodpovídá hello povolené metody. Ho, však neshoduje druhého pravidla text hello, takže operace proběhne úspěšně.

Hello třetí požadavek odpovídá hello druhé pravidlo v jeho zdrojovou doménu a metodu, takže žádná další pravidla se vyhodnocují. Ale hello *hlavičky x-ms-client-request-id* není povolena druhého pravidla text hello, takže hello požadavek selže bez ohledu hello fakt, že sémantika hello hello třetí pravidlo by se povolí jeho toosucceed.

> [!NOTE]
> I když tento příklad ukazuje méně přísná pravidla před více omezující jeden, obecně hello osvědčeným postupem je toolist hello nejvíc omezující pravidla nejprve.
> 
> 

## <a name="understanding-how-hello-vary-header-is-set"></a>Pochopení, jak nastavit hello měnit hlavičky
Hello *měnit* záhlaví je standardní hlavičku HTTP/1.1 skládající se z sadu pole hlavičky požadavku, které poradit hello prohlížeče nebo uživatelského agenta hello kritéria, které byly vybrány požadavkem hello tooprocess server hello. Hello *měnit* záhlaví se většinou používá pro ukládání do mezipaměti podle proxy, prohlížečů a sítím CDN, které ji používají toodetermine jak hello odpověď do mezipaměti. Podrobnosti najdete v tématu hello specifikace hello [měnit hlavičky](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

Když hello prohlížeče nebo jiné uživatelský agent ukládá do mezipaměti odpovědi hello z požadavek CORS, doménu původu hello se uloží do mezipaměti jako hello povolené původu. Když druhý problémy v doméně hello stejné žádost o prostředek úložiště při hello mezipaměti je aktivní, uživatelský agent hello načte hello uložené v mezipaměti zdrojovou doménu. Druhá doména Hello neodpovídá hello domény v mezipaměti, proto hello požadavek selže, když by jinak úspěšné. V některých případech, Azure Storage nastaví hello měnit hlavičky příliš**původu** tooinstruct hello agenta toosend hello následné CORS požadavku toohello služba uživatele při hello požaduje domény se liší od hello mezipaměti původu.

Úložiště Azure nastaví hello *měnit* záhlaví příliš**původu** skutečné požadavky GET nebo HEAD v následujících případech hello:

* Kdy hello žádosti počátek přesně odpovídá hello povoleno počátek definované pravidlem CORS. toobe přesnou shodu hello CORS pravidlo nesmí obsahovat zástupný znak ' * ' znak.
* Neexistuje žádná pravidla odpovídající hello požadovaný původ, ale je povolená CORS pro službu úložiště hello.

V případě hello kde požadavek GET nebo HEAD odpovídá pravidla CORS, která umožňuje všechny původy hello odpovědi označuje, že jsou povoleny všechny původy a hello uživatele agenta mezipaměti umožní následné žádosti z jakékoli doménu původu při hello mezipaměti je aktivní.

Všimněte si, že pro požadavky na použití jiných metod než GET nebo HEAD, služby úložiště hello nenastaví hello měnit hlavičky, vzhledem k tomu, že se neukládají do mezipaměti odpovědi toothese metody Uživatelští agenti.

Hello následující tabulka určuje, jak Azure storage bude odpovídat požadavky tooGET nebo HEAD hello podle dříve uvedených případech:

| Žádost | Nastavení účtu a výsledek vyhodnocení pravidla |  |  | Odpověď |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Původ hlavičky v požadavku** |**CORS pravidel zadaný pro tuto službu** |**Existuje odpovídající pravidlo, které umožňuje všechny původy (*)** |**Existuje odpovídající pravidlo pro shodu přesný počátek** |**Odpověď obsahuje měnit hlavičky set tooOrigin** |**Odpověď obsahuje Access-Control-povolené-Origin: "*"** |**Odpověď obsahuje Access-Control-zveřejněné-Headers** |
| Ne |Ne |Ne |Ne |Ne |Ne |Ne |
| Ne |Ano |Ne |Ne |Ano |Ne |Ne |
| Ne |Ano |Ano |Ne |Ne |Ano |Ano |
| Ano |Ne |Ne |Ne |Ne |Ne |Ne |
| Ano |Ano |Ne |Ano |Ano |Ne |Ano |
| Ano |Ano |Ne |Ne |Ano |Ne |Ne |
| Ano |Ano |Ano |Ne |Ne |Ano |Ano |

## <a name="billing-for-cors-requests"></a>Fakturace požadavků CORS
Kontrola před výstupem úspěšné požadavky se účtují, pokud jste povolili CORS pro všechny služby hello úložiště pro váš účet (voláním [nastavit vlastnosti služby objektů Blob](https://msdn.microsoft.com/library/hh452235.aspx), [nastavit vlastnosti fronty služby](https://msdn.microsoft.com/library/hh452232.aspx), nebo [ Nastavit vlastnosti služby Table](https://msdn.microsoft.com/library/hh452240.aspx)). toominimize poplatky, zvažte nastavení hello **MaxAgeInSeconds** element ve vaší CORS pravidla tooa velké hodnoty tak, aby hello uživatelský agent ukládá do mezipaměti žádosti hello.

Neúspěšná předběžných požadavků, nebude platit.

## <a name="next-steps"></a>Další kroky
[Nastavit vlastnosti služby objektů Blob](https://msdn.microsoft.com/library/hh452235.aspx)

[Nastavit vlastnosti fronty služby](https://msdn.microsoft.com/library/hh452232.aspx)

[Nastavit vlastnosti služby Table](https://msdn.microsoft.com/library/hh452240.aspx)

[Specifikace pro sdílení prostředků různého původu W3C](http://www.w3.org/TR/cors/)

