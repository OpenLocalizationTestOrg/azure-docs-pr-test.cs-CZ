---
title: aaaAzure CDN pravidla modul funkce | Microsoft Docs
description: "Referenční dokumentace pro Azure CDN pravidla shody stav motoru a funkce."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: c10b8ef58e3d209b12fbb0ac2173e1ca51ff7538
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cdn-rules-engine-features"></a>Pravidla ve službě Azure CDN modul funkce
Toto téma obsahuje podrobný popis dostupných funkcí hello pro Azure Content Delivery Network (CDN) [stroj pravidel](cdn-rules-engine.md).

Hello třetí část pravidla je funkce hello. Funkce definuje hello typ akce, které budou použity toohello typ požadavku identifikovaný sadu podmínek shodu.

## <a name="access"></a>Access

Tyto funkce jsou toocontent navrženou toocontrol přístup.


Name (Název) | Účel
-----|--------
Odepřít přístup | Určuje, zda všechny požadavky byly zamítnuty 403 Zakázáno odpovědi.
Token ověřování | Určuje, zda na základě tokenu ověřování použité tooa požadavku.
Odmítnutí kód tokenu ověřování | Určuje typ hello odpovědi, který bude vrácen tooa uživatele při požadavku byl odepřen z důvodu ověřování na základě tooToken.
Token Auth ignorovat případ adresy URL | Určuje, zda bude adresa URL porovnání provedené na základě tokenu ověřování malá a velká písmena.
Parametr tokenu ověřování | Určuje, zda by měl parametr řetězce dotazu ověřování na základě tokenu hello přejmenovat.

### <a name="deny-access"></a>Odepřít přístup
**Účel**: Určuje, zda všechny požadavky byly zamítnuty 403 Zakázáno odpovědi.

Hodnota | výsledek
------|-------
Povoleno| Způsobí, že všechny požadavky, které odpovídají hello odpovídající kritéria toobe odmítne kvůli 403 Zakázáno odpovědi.
Zakázáno| Obnoví hello výchozí chování. Hello výchozí chování je tooallow hello serveru toodetermine hello typ původu odpovědi, který bude vrácen.

**Výchozí chování**: zakázáno

> [!TIP]
   > Možné použití této funkce je tooassociate její hlavičku požadavku odpovídat odkazující podmínku tooblock přístup tooHTTP servery, které používají vložený obsah tooyour odkazy.

### <a name="token-auth"></a>Token ověřování
**Účel:** Určuje, zda na základě tokenu ověřování použité tooa požadavku.

Pokud je povoleno ověřování na základě tokenu, bude použito pouze požadavků, které poskytují zašifrovaný token a vyhovovat požadavkům toohello určeným tento token.

Hello šifrovací klíč, který bude používané tooencrypt a dešifrování tokenů hodnoty je určen podle primární klíč a možnosti zálohování klíče na stránce tokenu ověřování. Mějte na paměti, že šifrovací klíče jsou specifické pro platformu.

Hodnota | výsledek
------|---------
Povoleno | Chrání hello požadovaný obsah s ověřováním na základě tokenu. Pouze požadavky od klientů, které poskytují platný token a splňovat požadavky na jeho bude dodržet. FTP transakce jsou vyloučeny z ověřování na základě tokenu.
Zakázáno| Obnoví hello výchozí chování. Hello výchozí chování je tooallow vaší toodetermine konfigurace na základě tokenu ověřování, zda požadavek nebude zabezpečené.

**Výchozí chování:** zakázané.

###<a name="token-auth-denial-code"></a>Odmítnutí kód tokenu ověřování
**Účel:** určuje hello typ odpovědi, který bude vrácen tooa uživatele při požadavku byl odepřen z důvodu ověřování na základě tooToken.

Níže jsou uvedeny kódy Hello k dispozici odpověď.

Kód odezvy|Název odpovědí|Popis
----------------|-----------|--------
301|Trvale přesunut|Tento kód stavu přesměrování URL toohello neoprávnění uživatelé uvedená v hlavičce umístění.
302|Najít|Tento kód stavu přesměrování URL toohello neoprávnění uživatelé uvedená v hlavičce umístění. Tento kód stavu je hello standardní způsob provedení přesměrování.
307|Dočasné přesměrování|Tento kód stavu přesměrování URL toohello neoprávnění uživatelé uvedená v hlavičce umístění.
401|Neautorizováno|Kombinování tento kód stavu se hlavička WWW-Authenticate odpovědi vám umožní tooprompt uživatele pro ověřování.
403|Je zakázané|Toto je hello standardní 403 Zakázáno stavovou zprávu, že neoprávněný uživatel uvidí při pokusu o tooaccess chráněného obsahu.
404|Soubor nebyl nalezen.|Tento kód stavu označuje, že klienta HTTP hello bylo možné toocommunicate hello serveru, ale hello požádal obsah nebyl nalezen.

#### <a name="url-redirection"></a>Adresa URL přesměrování

Tato funkce podporuje URL tooa vlastní adresu URL pro přesměrování, pokud je nakonfigurované tooreturn 3xx stavový kód. Tato adresa URL uživatelem definované lze zadat provedením hello následující kroky:

1. Vyberte kód odpovědi 3xx pro funkci hello Denial kód tokenu ověřování.
2. Vyberte "Místo" pomocí volby volitelný název hlavičky.
3. Nastavení adresy URL toohello potřeby možnost Volitelná hodnota hlavičky.

Pokud adresu URL není definován pro 3xx stavový kód, pak hello stránky standardní odpovědi pro stavový kód 3xx, bude vrácen toohello uživatele.

Adresa URL přesměrování platí jenom pro 3xx kódů odpovědi.

Možnost Volitelná hodnota hlavičky podporuje alfanumerické znaky, znaky uvozovek a mezery.

#### <a name="authentication"></a>Authentication

Tato funkce podporuje hlavičky WWW-Authenticate tooinclude schopností hello při odpovědi tooan neoprávněného požadavku pro obsah chráněný na základě tokenu ověřování. Pokud hlavička WWW-Authenticate byla nastavena příliš "základní" v konfiguraci, bude neoprávněný uživatel hello výzva pro pověření účtu.

provedením následujících kroků hello jde dosáhnout Hello výše konfigurace:

1. Vyberte "401" jako hello kód odpovědi pro funkci hello Denial kód tokenu ověřování.
2. Vyberte možnost volitelný název hlavičky "WWW-Authenticate".
3. Nastavte možnost Volitelná hodnota hlavičky příliš "základní."

Hlavička WWW-Authenticate platí pouze pro kódy 401 odpovědi.

### <a name="token-auth-ignore-url-case"></a>Token Auth ignorovat případ adresy URL
**Účel:** Určuje, zda bude adresa URL porovnání provedené na základě tokenu ověřování malá a velká písmena.

Parametry Hello vliv této funkce jsou:

- ec_url_allow
- ec_ref_allow
- ec_ref_deny

Platné hodnoty jsou:

Hodnota|výsledek
---|----
Povoleno|Způsobí, že naše hraniční server tooignore případu při porovnávání adres URL pro ověřování na základě tokenu parametry.
Zakázáno|Obnoví hello výchozí chování. Hello výchozí chování je pro porovnání adresu URL pro ověřování tokenem toobe malá a velká písmena.

**Výchozí chování:** zakázané.
 
### <a name="token-auth-parameter"></a>Parametr tokenu ověřování
**Účel:** Určuje, zda by měl parametr řetězce dotazu ověřování na základě tokenu hello přejmenovat.

Informace o klíči:

- Možnost Hodnota definuje hello název parametru řetězce dotazu pomocí něhož je možné zadat token.
- Příliš nelze nastavit možnost hodnotu "ec_token."
- Zajistěte, aby tento název hello definované v jenom možnost Hodnota 
- obsahuje platné znaky adresy URL.

Hodnota|výsledek
----|----
Povoleno|Možnost Hodnota definuje hello název parametru řetězce dotazu přes který by měl být definován tokeny.
Zakázáno|Token je možné zadat jako parametr řetězce dotazu Nedefinovaná v adrese URL žádosti hello.

**Výchozí chování:** zakázané. Token je možné zadat jako parametr řetězce dotazu Nedefinovaná v adrese URL žádosti hello.

## <a name="caching"></a>Ukládání do mezipaměti

Tyto funkce jsou navrženou toocustomize, kdy a jak je obsah uložený v mezipaměti.

Name (Název) | Účel
-----|--------
Parametry šířky pásma | Určuje, jestli parametry (tj. ec_rate a ec_prebuf) omezení šířky pásma bude aktivní.
Omezení šířky pásma | Omezí generovaný hello šířky pásma pro odpověď hello poskytované naše servery edge.
Nepoužívat mezipaměti | Určuje, zda text hello žádost můžete využít naše ukládání do mezipaměti technologie.
Zpracování hlavička Cache-Control | Ovládací prvky hello generování hlavičky Cache-Control hello hraniční server, když je aktivní funkce externí Max-Age.
Řetězec dotazu klíče mezipaměti | Určuje, zda bude mezipaměti klíče hello zahrnout nebo vyloučit parametrů řetězce dotazu přidružený k požadavku.
Přepište klíče mezipaměti | Přepíše hello přidružený k požadavku na klíče mezipaměti.
Dokončení výplně mezipaměti | Určuje, co se stane, když požadavek výsledky v k neúspěšnému přístupu do mezipaměti částečné na hraniční server.
Komprimovat typy souborů | Definuje hello formáty souborů, které komprimuje na serveru hello.
Výchozí interní Max-Age | Určuje hello výchozí maximální stáří intervalu pro opětovné ověření hraniční server tooorigin server mezipaměti.
Vyprší platnost zacházení záhlaví | Ovládací prvky hello generování hlavičky Expires server edge, když je aktivní funkce externí Max-Age hello.
Externí Max-Age | Určuje hello maximální stáří intervalu pro opětovné ověření mezipaměti prohlížeče tooedge serveru.
Vynutit interní Max-Age | Určuje hello maximální stáří intervalu pro opětovné ověření hraniční server tooorigin server mezipaměti.
Podpora H.264 (HTTP progresivního stahování) | Určuje, že typy hello H.264 formáty souborů, které může být použité toostream obsahu.
Dodržet No Cache požadavku | Určuje, zda budou požadavků na Ne mezipaměť klienta HTTP předány toohello zdrojový server.
Ignorovat počátek No-Cache | Určuje, zda bude naše CDN ignorovat určité direktivy zpracování zdrojovému serveru.
Ignorovat Unsatisfiable rozsahů | Určuje odpověď hello, který bude vrácen tooclients při žádost vygeneruje stavový kód 416 požadovaný rozsah nelze uspokojit.
Interní Max zastaralé | Určuje, jak dlouho po čase hello normální vypršení platnosti mezipaměti asset může zpracovat z hraniční server po hello hraniční server nelze toorevalidate hello uložené v mezipaměti asset hello zdrojový server.
Sdílení částečné mezipaměti | Určuje, zda žádost může generovat obsahu částečně v mezipaměti.
Prevalidate obsah uložený v mezipaměti | Určuje, zda obsah uložený v mezipaměti vhodné pro včasné opětovné ověření před jeho hodnota TTL nevyprší.
Aktualizujte souborů z mezipaměti nula bajtů | Určuje, jak zpracovává požadavek klienta HTTP pro prostředek 0 bajtů mezipaměti servery edge.
Nastavit lze uložit do mezipaměti stavové kódy | Definuje sadu hello stavové kódy, které mohou způsobovat obsah uložený v mezipaměti.
Zastaralé doručování obsahu při chybě | Určuje, zda vypršela platnost, že budou doručeny obsah uložený v mezipaměti, když dojde k chybě během opětovné ověření mezipaměti nebo když načítání hello požadovaný obsah z hello zákazníka zdrojový server.
Zastaralých při Revalidate | Zlepšuje výkon tím, že naše servery edge tooserve zastaralý klient toohello žadatel, zatímco probíhá opětovné ověření.
Komentář | Funkce komentář Hello umožňuje Poznámka toobe, přidat v pravidle.

###<a name="bandwidth-parameters"></a>Parametry šířky pásma
**Účel:** Určuje, jestli parametry (tj. ec_rate a ec_prebuf) omezení šířky pásma bude aktivní.

Parametry omezení šířky pásma určení, zda hello rychlost přenosu dat pro požadavek klienta budou omezené tooa vlastní míry.

Hodnota|výsledek
--|--
Povoleno|Umožňuje naše servery edge toohonor šířky pásma, omezování požadavků.
Zakázáno|Způsobí, že naše servery edge tooignore omezení parametry šířky pásma. Hello požadovaného obsahu se zpracuje normálně (tj. bez omezení šířky pásma).

**Výchozí chování:** povolena.

###<a name="bandwidth-throttling"></a>Omezení šířky pásma
**Účel:** hello omezení šířky pásma pro odpověď hello poskytované naše servery edge.

Obě následující možnosti hello musí být definované tooproperly nastavení omezení šířky pásma.

Možnost|Popis
--|--
KB za sekundu|Nastavte tuto možnost toohello maximální šířka pásma (Kb za sekundu), může být použité toodeliver hello odpovědi.
Prebuf sekund|Nastavte tuto možnost toohello počet sekund, po které bude naše servery edge čekat omezení šířky pásma. účelem Hello tohoto období bez omezení šířky pásma je tooprevent přehrávač médií z zaznamenat přerušování nebo ukládání do vyrovnávací paměti z důvodu omezení toobandwidth problémy.

**Výchozí chování:** zakázané.

###<a name="bypass-cache"></a>Nepoužívat mezipaměti
**Účel:** Určuje, zda text hello žádost můžete využít naše ukládání do mezipaměti technologie.

Hodnota|výsledek
--|--
Povoleno|I v případě, že obsah hello byl dříve uložené v mezipaměti serverů edge způsobí, že všechny požadavky toofall prostřednictvím toohello zdrojový server.
Zakázáno|Způsobí, že servery edge toocache prostředky podle zásady mezipaměti toohello definované v jeho hlavičky odpovědi.

**Výchozí chování:**

- **Velké HTTP:** zakázáno

<!---
- **ADN:** Enabled
--->

###<a name="cache-control-header-treatment"></a>Mezipaměti ovládacího prvku záhlaví zpracování
**Účel:** ovládací prvky hello generování hlavičky Cache-Control hello hraniční server, když je aktivní externí funkce Max-Age.

Nejjednodušší způsob, jak tooachieve tento typ konfigurace je tooplace hello externí Max-Age a zacházení hlavička Cache-Control funkcí hello Hello hello stejného příkazu.

Hodnota|výsledek
--|--
Přepsání|Zajišťuje, že bude probíhat této hello následující akce:<br/> -Přepíše hlavičku Cache-Control generované hello zdrojový server. <br/>-Přidá hlavičku Cache-Control vyprodukované hello externí Max-Age funkce toohello odpovědi.
Předání|Zajišťuje, že hlavičku Cache-Control vyprodukované hello externí Max-Age funkce nikdy přidáno toohello odpovědi. <br/> Pokud zdrojový server hello vytvoří hlavičku Cache-Control, předá toohello koncového uživatele. <br/> Pokud zdrojový server hello nevytváří hlavičku Cache-Control, pak se tato možnost může příčina hello odpověď záhlaví toonot obsahovat hlavičku Cache-Control.
Přidejte Pokud chybí|Pokud hlavička Cache-Control nebyla přijata od hello zdrojový server, tato možnost přidá hlavičku Cache-Control vyprodukované hello externí Max-Age funkce. Tato možnost je užitečná pro zajištění, že všechny prostředky se přiřadí hlavičku Cache-Control.
Odebrat| Tato možnost zajistí, že není součástí hello hlavičky odpovědi hlavičku Cache-Control. Pokud již byla přiřazena hlavičku Cache-Control, pak jej se odstraní z hello hlavičky odpovědi.

**Výchozí chování:** přepsat.

###<a name="cache-key-query-string"></a>Řetězec dotazu klíče mezipaměti
**Účel:** Určuje, zda bude klíč mezipaměti zahrnout nebo vyloučit parametrů řetězce dotazu přidružený k požadavku.

Informace o klíči:

- Zadejte jeden nebo více názvy parametrů řetězce dotazu. Název každého parametru by měl být oddělené mezerou.
- Tato funkce určuje, zda bude parametrů řetězce dotazu zahrnout nebo vyloučit z mezipaměti klíče hello. Další informace naleznete u každé níže uvedené možnosti.

Typ|Popis
--|--
 Zahrnout|  Označuje, že každý zadaný parametr by měl být součástí klíče mezipaměti hello. Pro každý požadavek, který obsahuje jedinečná hodnota pro parametr řetězce dotazu definované v tato funkce se budou generovat jedinečný klíč mezipaměti. 
 Zahrnout všechny  |Označuje, že je vytvořen jedinečný klíč mezipaměti pro každý požadavek tooan asset, který obsahuje řetězec dotazu jedinečný. Tento typ konfigurace se nedoporučuje obvykle vzhledem k tomu může dojít tooa malým procentem přístupů do mezipaměti. Tím se zvýší zatížení hello hello zdrojový server, vzhledem k tomu, že bude mít tooserve další požadavky. Tato konfigurace duplikuje hello ukládání do mezipaměti chování označuje jako "jedinečný mezipaměti" na stránce ukládání do mezipaměti řetězce dotazu. 
 Vyloučení | Označuje, že pouze hello zadané parametry budou vyloučeny z mezipaměti klíče hello. V mezipaměti klíče hello budou zahrnuty všechny ostatní parametrů řetězce dotazu. 
 Vyloučit všechny výsledky kategorie  |Označuje, že všechny parametrů řetězce dotazu budou vyloučeny z mezipaměti klíče hello. Tato konfigurace duplikuje hello výchozí chování, která se označuje jako "standard-cache" na stránce ukládání do mezipaměti řetězce dotazu ukládání do mezipaměti. 

výkon Hello stroj pravidel HTTP umožňuje toocustomize hello způsobem, ve kterém se implementuje ukládání do mezipaměti řetězce dotazu. Například můžete zadat, že dotaz řetězec ukládání do mezipaměti pouze provést na určené umístění nebo typy souborů.

Pokud chcete tooduplicate hello dotazu chování ukládání řetězců označuje jako "no-cache" na stránce ukládání do mezipaměti řetězce dotazu, budete potřebovat toocreate pravidlo, které obsahuje podmínku shodu adresy URL dotazu zástupný znak a funkce mezipaměti jednorázové přihlášení. Hello podmínku shodu adresy URL dotazu zástupný znak, musí být nastaven tooan hvězdičky (*).

#### <a name="sample-scenarios"></a>Vzorové scénáře

Využití vzorků pro tuto funkci je uveden níže. Ukázková žádost a hello výchozí klíč mezipaměti jsou uvedeny níže.

- **Ukázková žádost:** http://wpc.0001.&lt; Domény&gt;/800001/Origin/folder/asset.htm?sessionid=1234 a jazyk = EN & userid = 01
- **Výchozí klíč mezipaměti:** /800001/Origin/folder/asset.htm

##### <a name="include"></a>Zahrnout

Ukázková konfigurace:

- **Typ:** patří
- **Parametry:** jazyk

Tento typ konfigurace by vygeneroval hello následující parametr řetězce dotazu mezipaměti klíč:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="include-all"></a>Zahrnout všechny

Ukázková konfigurace:

- **Typ:** zahrnout všechny

Tento typ konfigurace by vygeneroval hello následující parametr řetězce dotazu mezipaměti klíč:

    /800001/Origin/folder/asset.htm?sessionid=1234&language=EN&userid=01

##### <a name="exclude"></a>Vyloučení

Ukázková konfigurace:

- **Typ:** vyloučit
- **Parametry:** sessionid ID uživatele

Tento typ konfigurace by vygeneroval hello následující parametr řetězce dotazu mezipaměti klíč:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="exclude-all"></a>Vyloučit všechny výsledky kategorie

Ukázková konfigurace:

- **Typ:** vyloučit všechny výsledky kategorie

Tento typ konfigurace by vygeneroval hello následující parametr řetězce dotazu mezipaměti klíč:

    /800001/Origin/folder/asset.htm

###<a name="cache-key-rewrite"></a>Přepište klíče mezipaměti
**Účel:** přepisů hello klíče mezipaměti přidružený k požadavku.

Klíč mezipaměti je hello relativní cestu, která identifikuje prostředek pro účely hello ukládání do mezipaměti. Jinými slovy naše servery zkontroluje uložené v mezipaměti verzi prostředek podle tooits cestu, jak jsou definovány pomocí jeho klíče mezipaměti.

Nakonfigurujte tuto funkci tak, že definujete obě hello následující možnosti:

Možnost|Popis
--|--
Původní cesta| Definování typů toohello relativní cestu hello požadavků, jejichž klíče mezipaměti bude nutné přepsat. Relativní cesta může být definováno výběrem základní původní cestu a poté definování vzor regulárního výrazu.
Nová cesta|Definujte hello relativní cestu k nové klíče na mezipaměti hello. Relativní cesta může být definováno výběrem základní původní cestu a poté definování vzor regulárního výrazu. Tuto relativní cestu lze dynamicky sestavit prostřednictvím hello použití proměnných HTTP
**Výchozí chování:** klíče mezipaměti požadavek je určen podle identifikátoru URI žádosti hello.

###<a name="complete-cache-fill"></a>Dokončení výplně mezipaměti
**Účel:** Určuje, co se stane, když požadavek výsledkem k neúspěšnému přístupu do mezipaměti částečné na hraniční server.

K neúspěšnému přístupu do mezipaměti částečné popisuje hello mezipaměti stavu pro určitý prostředek, který nebyl zcela stažené tooan hraničního serveru. Pokud prostředek jen částečně v mezipaměti na serveru edge, pak hello další požadavek pro tento prostředek předá se znovu toohello zdrojový server.
<!---
This feature is not available for hello ADN platform. hello typical traffic on this platform consists of relatively small assets. hello size of hello assets served through these platforms helps mitigate hello effects of partial cache misses, since hello next request will typically result in hello asset being cached on that POP.
--->
K neúspěšnému přístupu do mezipaměti částečné obvykle dochází, až uživatel zruší stahování nebo pro prostředky, které jsou vyžadovány výhradně pomocí protokolu HTTP rozsah požadavků. Tato funkce je nejvhodnější pro velké prostředky, kde uživatelé nebudou stahovat obvykle je ze spuštění toofinish (například videa). V důsledku toho je tato funkce povolená ve výchozím nastavení na platformě HTTP velké hello. Na jiných platformách je zakázaná.

Výchozí konfigurace hello tooleave hello HTTP velké platformy, se doporučuje vzhledem k tomu, že bude snížit zatížení hello na vašem serveru počátek zákazníka a zvýšit rychlost Dobrý den, kdy vašim zákazníkům stažení vašeho obsahu.

Z důvodu toohello způsobem v mezipaměti, které jsou sledovány nastavení, nemůže být tato funkce související s hello následující podmínky shody: Edge Cname, literálu hlavičky požadavku, žádosti o záhlaví zástupný znak, adresa URL dotazu literálu a adresa URL dotazu.

Hodnota|výsledek
--|--
Povoleno|Obnoví hello výchozí chování. Hello výchozí chování je tooforce hello hraniční server tooinitiate načítání na pozadí hello majetku z hello zdrojový server. Po kterém hello asset se bude nacházet v místní mezipaměti hello hraničního serveru.
Zakázáno|Hraniční server bránit v provádění načítání na pozadí pro prostředek hello. To znamená že hello další požadavek pro tento prostředek z této oblasti způsobí, že hraniční server toorequest z hello zákazníka zdrojový server.

**Výchozí chování:** povolena.

###<a name="compress-file-types"></a>Komprimovat typy souborů
**Účel:** definuje hello formáty souborů, které komprimuje na serveru hello.

Formát souboru lze zadat pomocí jeho typ média Internetu (tj, Content-Type). Typ média Internetu je nezávislé na platformě metadata, která umožňuje naše servery tooidentify hello formát souboru konkrétní asset. Seznam běžné typy médií Internetu je uvedený níže.

Typ média Internetu|Popis
--|--
text/plain|Soubory ve formátu prostého textu
text/html| Soubory HTML
text/css|Listů kaskádových stylů (CSS)
Application/x-javascript|JavaScript
aplikace/javascript|JavaScript
Informace o klíči:

- Zadejte víc typů médií Internet omezující každé z nich mezerou. 
- Tato funkce bude komprimovat pouze prostředky, jejíž aktuální velikost je menší než 1 MB. Větší prostředky nebude komprimována naše servery.
- Určité typy obsahu, například bitové kopie, video a zvukových médiích prostředky (například JPG, MP3, MP4, atd.), jsou již v komprimovaném tvaru. Další kompresi na tyto typy prostředků nebude dojít k výraznému snížení velikosti souboru. Proto se doporučuje, nepovolujte kompresi na tyto typy prostředků.
- Zástupné znaky, jako je například hvězdičky, nejsou podporovány.
- Před přidáním tohoto pravidla tooa funkce, zkontrolujte, zda tooset komprese zakázaná možnost na stránce komprese toowhich hello platformy, které bude použito toto pravidlo.

###<a name="default-internal-max-age"></a>Výchozí interní Max-Age
**Účel:** určuje hello výchozí maximální stáří intervalu pro opětovné ověření hraniční server tooorigin server mezipaměti. Jinými slovy hello množství času, který bude předat před hraniční server zkontrolujte, zda v mezipaměti asset odpovídá hello asset uložené na původním serveru hello.

Informace o klíči:

- Tato akce uskuteční pouze pro odpovědi ze serveru původu nepřiřadili indikace maximální stáří v hlavičce Cache-Control nebo Expires.
- Tato akce neproběhne u prostředků, které se považují za lze uložit do mezipaměti.
- Tato akce nemá vliv revalidations mezipaměti serveru tooedge prohlížeče. Tyto typy revalidations určuje Cache-Control nebo Expires hlavičky odeslaných toohello prohlížeč, který je možné přizpůsobit prostřednictvím funkce externí Max-Age.
- Hello výsledky této akce nemáte pozorovatelné vliv na hlavičky odpovědi hello a obsah hello vrácená ze serverů edge pro obsah, ale může mít vliv na velikost hello opětovné ověření přenosy z hraniční servery tooyour zdrojový server.
- Nakonfigurujte tuto funkci pomocí:
    - Výběr hello stavový kód, pro který lze použít výchozí vnitřní, max-age.
    - Určení celočíselnou hodnotu a pak vyberete hello požadovanou časovou jednotku (tj, sekund, minut, hodin, atd.). Tato hodnota definuje hello výchozí vnitřní maximální stáří interval.

- Nastavení hello časovou jednotku příliš "Off" přiřadí intervalu výchozí vnitřní maximální stáří 7 dní pro požadavky, které nebyly přiřazeny indikace maximální stáří v jejich Cache-Control nebo hlavička Expires.
- Z důvodu toohello způsobem v mezipaměti, které jsou sledovány nastavení nemůže být tato funkce související s hello následující podmínky shody: 
    - Edge 
    - CNAME
    - Literál hlavičky požadavku
    - Zástupný znak hlavičky požadavku
    - Request – metoda
    - Adresa URL dotazu literál
    - Adresa URL dotazu zástupný znak

**Výchozí hodnota:** 7 dnů

###<a name="expires-header-treatment"></a>Vyprší platnost zacházení záhlaví
**Účel:** ovládací prvky hello generování hlavičky Expires server edge, když funkci externí Max-Age je aktivní.

Hello nejjednodušší způsob, jak tooachieve tento typ konfigurace je tooplace hello externí Max-Age a funkcí vyprší platnost zacházení záhlaví hello hello stejného příkazu.

Hodnota|výsledek
--|--
Přepsání|Zajišťuje, že bude probíhat této hello následující akce:<br/>-Přepíše hlavičku Expires generované hello zdrojový server.<br/>-Přidá hlavičku Expires vyprodukované hello externí Max-Age funkce toohello odpovědi.
Předání|Zajišťuje, že hlavičku Expires vyprodukované hello externí Max-Age funkce nikdy přidáno toohello odpovědi. <br/> Pokud zdrojový server hello vytvoří hlavičku Expires, předá toohello koncového uživatele. <br/>Pokud zdrojový server hello nevytváří hlavičku Expires, pak se tato možnost může příčina hello odpověď záhlaví toonot obsahovat hlavičku Expires.
Přidejte Pokud chybí| Pokud hlavička Expires nebyla přijata od hello zdrojový server, tato možnost přidá hlavičku Expires vyprodukované hello externí Max-Age funkce. Tato možnost je užitečná pro zajištění, že všechny prostředky se přiřadí hlavičku Expires.
Odebrat| Zajišťuje, že není součástí hello hlavičky odpovědi hlavičku Expires. Pokud již byla přiřazena hlavičku Expires, pak jej se odstraní z hello hlavičky odpovědi.

**Výchozí chování:** přepsat

###<a name="external-max-age"></a>Externí Max-Age
**Účel:** určuje hello maximální stáří intervalu pro opětovné ověření mezipaměti prohlížeče tooedge serveru. Jinými slovy můžete zkontrolovat hello množství času, který předá před prohlížeče pro novou verzi prostředek z hraniční server.

Povolení této funkce vytvoří mezipaměť – ovládací prvek: maximální-stáří a vyprší hlaviček z naše servery edge a jejich odeslání toohello HTTP klienta. Ve výchozím nastavení bude tato záhlaví přepsat nebyla vytvořena v hello zdrojový server. Ale způsob zpracování hlavička Cache-Control a funkce vyprší platnost zacházení záhlaví může být použité tooalter toto chování.

Informace o klíči:

- Tato akce nemá vliv na hraniční server tooorigin server mezipaměti revalidations. Tyto typy revalidations vyplývají z mezipaměti – ovládací prvek nebo Expires hlavičky přijal od hello zdrojový server a lze přizpůsobit pomocí výchozí vnitřní Max-Age a funkcí Force interní Max-Age.
- Nakonfigurujte tuto funkci zadáním celočíselnou hodnotu a vyberte požadovanou časovou jednotku hello (tj, sekund, minut, hodin, atd.).
- Nastavení této funkce tooa záporné hodnoty způsobí, že naše servery toosend hraniční mezipaměti – ovládací prvek: Ne-mezipaměti a Expires čas, který je nastavený v hello minulosti s každým prohlížečem toohello odpovědi. I když hello odpověď nebude mezipaměti klientem HTTP, toto nastavení nebude mít vliv na naše servery edge možnost toocache hello odpověď ze serveru původu hello.
- Nastavení hello časovou jednotku příliš "Off" bude tuto funkci zakázat. Hlavičky Cache – ovládací prvek nebo Expires do mezipaměti odpovědi hello hello počátek serveru předá toohello prohlížeče.

**Výchozí chování:** vypnuto

###<a name="force-internal-max-age"></a>Vynutit interní Max-Age
**Účel:** určuje hello maximální stáří intervalu pro opětovné ověření hraniční server tooorigin server mezipaměti. Jinými slovy hello množství času, který bude uplynout, než hraniční server můžete zkontrolovat, zda v mezipaměti asset odpovídá hello asset uložené na původním serveru hello.

Informace o klíči:

- Tato funkce se přepíše hello maximální stáří intervalu určeném v Cache-Control nebo Expires hlavičky generované ze zdrojového serveru.
- Tato funkce nemá vliv revalidations mezipaměti serveru tooedge prohlížeče. Tyto typy revalidations určuje Cache-Control nebo Expires hlavičky odeslaných toohello prohlížeče.
- Tato funkce nemá pozorovatelné ovlivnění hello odpovědi doručil hraniční server toohello žadatel. Ale může mít vliv na velikost hello opětovné ověření přenosy z našich hraniční servery toohello zdrojový server.
- Nakonfigurujte tuto funkci pomocí:
    - Výběr hello stavový kód, pro které se použijí interní maximální stáří.
    - Určení celočíselná hodnota a výběrem hello požadovanou časovou jednotku (tj, sekund, minut, hodin, atd.). Tato hodnota definuje interval maximální stáří hello požadavku.

- Nastavení hello časovou jednotku příliš "Off" Zakáže tuto funkci. Interní maximální stáří intervalu nesmí být přiděleno toorequested prostředky. Pokud původní hlavičku hello neobsahuje pokyny pro ukládání do mezipaměti, pak hello asset bude do mezipaměti podle toohello active nastavení ve funkci výchozí vnitřní Max-Age.
- Z důvodu toohello způsobem v mezipaměti, které jsou sledovány nastavení nemůže být tato funkce související s hello následující podmínky shody: 
    - Edge 
    - CNAME
    - Literál hlavičky požadavku
    - Zástupný znak hlavičky požadavku
    - Request – metoda
    - Adresa URL dotazu literál
    - Adresa URL dotazu zástupný znak

**Výchozí chování:** vypnuto

###<a name="h264-support-http-progressive-download"></a>Podpora H.264 (HTTP progresivního stahování)
**Účel:** Určuje typy hello H.264 formáty souborů, které může být použité toostream obsahu.

Informace o klíči:

- Definujte sadu povolených H.264 přípony názvů souborů oddělených mezerami v možnost přípony souborů. Přípony souborů možnost přepíše hello výchozí chování. Podpora MP4 a F4V udržujte zahrnutím tyto přípony názvů souborů, když nastavení této možnosti. 
- Ujistěte se, že tooinclude a období při zadávání každou příponu názvu souboru (například .f4v MP4).

**Výchozí chování:** progresivní stahování HTTP podporuje média MP4 a F4V ve výchozím nastavení.

###<a name="honor-no-cache-request"></a>Žádost o dodržet bez mezipaměti
**Účel:** Určuje, zda budou požadavků na Ne mezipaměť klienta HTTP předány toohello zdrojový server.

Požadavek ne mezipaměti nastane, když klient hello HTTP odešle mezipaměti – ovládací prvek: Ne-mezipaměti nebo Pragma:no-hello HTTP požadavek obsahoval hlavičku cache.

Hodnota|výsledek
--|--
Povoleno|Umožňuje ne mezipaměť klienta HTTP požadavků toobe přesměrovaná toohello zdrojový server, a hello zdrojový server vrátí hlavičky odpovědi hello a text hello prostřednictvím hello hraniční server back toohello HTTP klienta.
Zakázáno|Obnoví hello výchozí chování. Hello výchozí chování je tooprevent ne mezipaměti požadavky z předávaná toohello zdrojový server.

Pro veškeré přenosy z výroby, je důrazně doporučuje tooleave tuto funkci ve svém výchozím zakázáno stavu. Jinak hodnota počátku servery nebude Stíněný od koncových uživatelů, kteří mohou nechtěně aktivovat mnoho požadavků bez mezipaměti, při aktualizaci webové stránky, nebo z hello mnoho přehrávače oblíbených médií, které jsou pevně nastavená toosend hlavičku bez mezipaměti každou žádost videa. Nicméně tato funkce může být užitečné tooapply toocertain mimo produkční pracovní nebo testování adresáře, v pořadí tooallow čerstvého obsahu toobe stažen na vyžádání z hello zdrojový server.

Stav Hello mezipaměti, která bude hlášena, pro požadavek, který je povolen toobe předané tooan zdrojový server z důvodu toothis funkce je TCP_Client_Refresh_Miss. Stavy mezipaměti zprávu, která je k dispozici v hello základní modul pro vytváření sestav, poskytuje statistické údaje podle stavu mezipaměti. To vám umožní tootrack hello počet a procento žádostí, které jsou předávány tooan zdrojový server z důvodu toothis funkce.

**Výchozí chování:** zakázané.

###<a name="ignore-origin-no-cache"></a>Ignorovat počátek no-cache
**Účel:** Určuje, zda bude naše CDN ignorovat hello následující direktivy zpracování zdrojovému serveru:

- Cache-Control: privátní
- Cache-Control: Ne úložiště
- Cache-Control: no-cache
- Direktiva pragma: bez ukládání do mezipaměti

Informace o klíči:

- Nakonfigurujte tuto funkci tak, že definujete mezerami oddělený seznam stavové kódy, pro které se budou ignorovat hello výše direktivy.
- Hello platný stavové kódy pro tuto funkci, které jsou: 200, 203, 300, 301, 302, 305, 307, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502, 503, 504 a 505.
- Tuto funkci zakážete nastavením tooa prázdnou hodnotu.
- Z důvodu toohello způsobem v mezipaměti, které jsou sledovány nastavení nemůže být tato funkce související s hello následující podmínky shody: 
    - Edge 
    - CNAME
    - Literál hlavičky požadavku
    - Zástupný znak hlavičky požadavku
    - Request – metoda
    - Adresa URL dotazu literál
    - Adresa URL dotazu zástupný znak

**Výchozí chování:** výchozí chování je toohonor hello výše direktivy.

###<a name="ignore-unsatisfiable-ranges"></a>Ignorovat Unsatisfiable rozsahů 
**Účel:** určuje hello odpověď, který bude vrácen tooclients při žádost vygeneruje stavový kód 416 požadovaný rozsah nelze uspokojit.

Ve výchozím nastavení tento kód stavu se vrátí, když hello zadaný rozsah bajtů požadavek nelze uspokojit, server edge a nebyl zadán pole hlavičky požadavku If-Range.

Hodnota|výsledek
-|-
Povoleno|Zabraňuje naše servery edge z odpovídá požadavku tooan neplatný rozsah bajtů s 416 požadovaný rozsah nelze uspokojit stavový kód. Místo toho bude naše servery poskytování hello požadovaný prostředek a vrátí 200 OK hello klientovi.
Zakázáno|Obnoví hello výchozí chování. Hello výchozí chování je toohonor 416 požadovaný rozsah nelze uspokojit stavový kód.

**Výchozí chování:** zakázané.

###<a name="internal-max-stale"></a>Interní Max zastaralé
**Účel:** řídí, jak dlouho posledních čas normální vypršení platnosti hello uložené v mezipaměti asset může dodávat z hraniční server, hello edge server je nelze toorevalidate hello mezipaměti asset hello zdrojový server.

Za normálních okolností po uplynutí této doby pro maximální stáří prostředek služby, odešle hello hraniční server opětovné ověření požadavku toohello zdrojový server. Hello zdrojový server se jako odpověď buď 304 nedojde ke změně umožnit hello hraniční server novou zapůjčení na uložené v mezipaměti asset hello or else s 200 OK poskytnout aktualizovaná verze sady v mezipaměti asset hello hello hraničního serveru.

Pokud je hello edge server nelze tooestablish připojení hello zdrojový server při pokusu o takové opětovné ověření, pak tato interní Max-zastaralé funkce řídí, zda a jak dlouho hello hraniční server může pokračovat tooserve hello nyní zastaralé asset.

Všimněte si, že tento časový interval spustí, když vyprší hello asset, max-age, nejsou při hello opětovné ověření se nezdařilo. Hello množství času určeného hello kombinace maximální stáří plus maximální zastaralé je proto hello maximální doba, během které prostředek může zpracovat bez úspěšné opětovné ověření. Například pokud prostředek se ukládá do mezipaměti v 9:00 s maximální stáří 30 minut a maximální zastaralé 15 minut, poté selhání opětovné ověření v 9:44 by způsobilo koncového uživatele přijímající hello zastaralé v mezipaměti prostředek, zatímco by způsobilo selhání opětovné ověření pokus o 9:46 koncový uživatel Hello přijetí 504 limitu brány.

Žádnou hodnotu pro tuto funkci je nahrazena mezipaměti nakonfigurován-ovládacího prvku: musí-znovu ověřit nebo ukládat do mezipaměti – ovládací prvek: proxy-znovu ověřit hlavičky přijal od hello zdrojový server. Pokud některý z těchto hlavičky je obdrželi od hello zdrojový server, pokud prostředek je původně uložené v mezipaměti, hello edge server neprovede zastaralé v mezipaměti asset. V takovém případě pokud je hello edge server nelze toorevalidate s původem hello po vypršení intervalu maximální stáří hello asset, pak hello hraniční server vrátí 504 limitu brány.

Informace o klíči:

- Nakonfigurujte tuto funkci pomocí:
    - Výběr hello stavový kód, pro které se použijí maximální zastaralé.
    - Určení celočíselnou hodnotu a pak vyberete hello požadovanou časovou jednotku (tj, sekund, minut, hodin, atd.). Tato hodnota definuje hello interní max zastaralé, se použijí.

- Nastavení hello časovou jednotku příliš "Off" bude tuto funkci zakázat. V mezipaměti asset nebudou poskytnuty času její normální vypršení platnosti.
- Z důvodu toohello způsobem v mezipaměti, které jsou sledovány nastavení nemůže být tato funkce související s hello následující podmínky shody: 
    - Edge 
    - CNAME
    - Literál hlavičky požadavku
    - Zástupný znak hlavičky požadavku
    - Request – metoda
    - Adresa URL dotazu literál
    - Adresa URL dotazu zástupný znak

**Výchozí chování:** 2 minuty

###<a name="partial-cache-sharing"></a>Sdílení částečné mezipaměti
**Účel:** Určuje, zda žádost může generovat obsahu částečně v mezipaměti.

Tato mezipaměť částečné může pak být použité toofulfill nové požadavky pro tento obsah, dokud hello požádal plně do mezipaměti obsah.

Hodnota|výsledek
-|-
Povoleno|Požadavky můžete vygenerovat obsahu částečně v mezipaměti.
Zakázáno|Požadavky můžete generovat jenom plně mezipaměti verzi hello požadovaný obsah.

**Výchozí chování:** zakázané.

###<a name="prevalidate-cached-content"></a>Prevalidate obsah uložený v mezipaměti
**Účel:** Určuje, zda obsah uložený v mezipaměti vhodné pro včasné opětovné ověření před jeho hodnota TTL nevyprší.

Definujte hello množství času předchozí toohello vypršení platnosti hello požádal o TTL obsahu, během kterého může být poskytnut časná opětovné ověření.

Informace o klíči:

- Výběr "Off" jako hello časovou jednotku vyžaduje opětovné ověření tootake místní po vypršení platnosti obsahu v mezipaměti hello TTL. Nesmí být zadaný čas a budou ignorovány.

**Výchozí chování:** vypnout. Opětovné ověření může proběhnout pouze po vypršení platnosti obsahu v mezipaměti TTL hello.

###<a name="refresh-zero-byte-cache-files"></a>Aktualizujte souborů z mezipaměti nula bajtů
**Účel:** určuje zpracování požadavku klienta HTTP pro prostředek 0 bajtů mezipaměti servery edge.

Platné hodnoty jsou:

Hodnota|výsledek
--|--
Povoleno|Způsobí, že naše hraniční server toore načítání hello asset z hello zdrojový server.
Zakázáno|Obnoví hello výchozí chování. Hello výchozí chování je tooserve až platný mezipaměti prostředky na vyžádání.
Tato funkce není vyžadován pro správné ukládání do mezipaměti a doručování obsahu, ale můžou být užitečné jako alternativní řešení. Například dynamického obsahu generátory na počátku serverech může způsobit nechtěně odesílány servery edge toohello odpovědí 0 bajtů. Tyto typy odpovědí jsou obvykle ukládají do mezipaměti servery edge. Pokud znáte odpovědi 0 bajtů se nikdy platné odezvy 

takový obsah pak tato funkce zabránit tyto typy prostředků zpracování tooyour klientů.

**Výchozí chování:** zakázané.

###<a name="set-cacheable-status-codes"></a>Nastavit lze uložit do mezipaměti stavové kódy
**Účel:** definuje hello sadu stavové kódy, které mohou způsobovat obsah uložený v mezipaměti.

Ve výchozím nastavení ukládání do mezipaměti je povolena pouze pro odpovědi 200 OK.

Definujte sadu hello potřeby stavové kódy oddělených mezerami.

Informace o klíči:

- Povolte funkci ignorovat No Cache počátek také. Pokud je tato funkce není povoleno, nemusí být mezipaměti odpovědi 200 OK.
- Hello platný stavové kódy pro tuto funkci, které jsou: 203, 300, 301, 302, 305, 307, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502, 503, 504 a 505.
- Tuto funkci nelze použít toodisable ukládání do mezipaměti pro odpovědi, které generují 200 OK stavový kód.

**Výchozí chování:** ukládání do mezipaměti je povoleno pouze pro odpovědi, které generují 200 OK stavový kód.
###<a name="stale-content-delivery-on-error"></a>Zastaralé doručování obsahu při chybě
**Účel:** 

Určuje, zda vypršela platnost, že budou doručeny obsah uložený v mezipaměti, když dojde k chybě během opětovné ověření mezipaměti nebo když načítání hello požadovaný obsah z hello zákazníka zdrojový server.

Hodnota|výsledek
-|-
Povoleno|Zastaralé obsah se zpracuje toohello žadatel, když dojde k chybě během počátečního serveru tooan připojení.
Zakázáno|Chyba Hello zdrojový server se předají toohello žadatel.

**Výchozí chování:** zakázáno

###<a name="stale-while-revalidate"></a>Zastaralých při Revalidate
**Účel:** zlepšuje výkon tím, že naše servery edge žadatel zastaralé obsahu toohello tooserve při opětovné ověření probíhá.

Informace o klíči:

- chování Hello této funkce se bude lišit podle toohello vybrané časovou jednotku.
    - **Časová jednotka:** zadejte dobu a vyberte a čas jednotky (například sekund, minut, hodin, atd.) tooallow zastaralé doručování obsahu. Tento typ instalačního programu umožňuje hello CDN tooextend hello dobu, po který může poskytovat obsahu před vyžadování ověření podle toohello následující vzorec:**TTL** + **zastaralé při znovu ověřit čas** 
    - **Vypnutí:** vyberte "Off" toorequire opětovné ověření před žádost pro zastaralé obsahu může zpracovat.
        - Nezadávejte časový interval, protože je nepoužitelných a budou ignorovány.

**Výchozí chování:** vypnout. Opětovné ověření musí být provedeno před hello požádal obsah nelze zpracovat.

###<a name="comment"></a>Komentář
**Účel:** umožňuje Poznámka toobe, přidat v pravidle.

Jedno použití pro tuto funkci je tooprovide Další informace o obecné účely hello pravidla nebo proč neodpovídá konkrétní podmínky nebo funkce byla přidána toohello pravidlo.

Informace o klíči:

- Je možné zadat maximálně 150 znaků.
- Ujistěte se, že tooonly alfanumerických znaků.
- Tato funkce nemá vliv na chování hello hello pravidla. Smyslem je jenom tooprovide, které oblasti, ve kterém můžete zadat informace pro budoucí použití nebo který mohou pomoci při řešení potíží s hello pravidlo.
 
## <a name="headers"></a>Záhlaví

Tyto funkce jsou navrženou tooadd, upravit nebo odstranit hlavičky z hello požadavku nebo odpovědi.

Name (Název) | Účel
-----|--------
Hlavička odpovědi stáří | Určuje, zda hlavičku odpovědi stáří budou zahrnuty v odpovědi hello odeslané toohello žadatel.
Ladění hlavičky odpovědi v mezipaměti | Určuje, zda odpověď může zahrnovat hello hlavička odpovědi X-ES-Debug, který obsahuje informace o hello zásady mezipaměti pro požadovaný prostředek hello.
Upravit hlavička požadavku klienta | Přepíše, připojí nebo odstraní hlavičky v požadavku.
Upravit hlavičku odpovědi klienta | Přepíše, připojí nebo odstraní hlavičku z odpovědi.
Nastavit vlastní záhlaví IP klienta | Umožňuje hello IP adresu klienta, který hello toobe přidané toohello žádost jako hlavičku požadavku vlastní.

###<a name="age-response-header"></a>Hlavička odpovědi stáří
**Účel**: Určuje, zda hlavičku odpovědi stáří zahrnuta v hello odpovědi odeslané toohello žadatel.
Hodnota|výsledek
--|--
Povoleno | hlavička odpovědi stáří Hello budou zahrnuty v odpovědi hello odeslané toohello žadatel.
Zakázáno | hlavička odpovědi stáří Hello budou vyloučeny z odpovědi hello odeslané toohello žadatel.

**Výchozí chování**: zakázáno.

###<a name="debug-cache-response-headers"></a>Ladění hlavičky odpovědi v mezipaměti
**Účel:** Určuje, zda odpověď může zahrnovat hlavičku odpovědi X-ES-Debug, který obsahuje informace o hello zásady mezipaměti pro požadovaný prostředek hello.

Ladění odpovědi v mezipaměti, záhlaví budou zahrnuty v odpovědi hello, pokud jsou splněny obě následující hello:

- Hello ladění funkce hlavičky odpovědi mezipaměti byla zapnuta hello požadované žádosti.
- Hello výše požadavek definuje hello sadu hlaviček odpovědí mezipaměti ladění, které budou zahrnuty v odpovědi hello.

Ladění mezipaměti odpověď záhlaví mohou být vyžádány zahrnutím hello následující hlavičky a hello požadované direktivy v žádosti o hello:

X-ES Debug: _Directive1_,_Directive2_,_DirectiveN_

**Příklad:**

X-ES-Debug: x-ec-cache,x-ec-check-cacheable,x-ec-cache-key,x-ec-cache-state

Hodnota|výsledek
-|-
Povoleno|Požadavky pro ladění hlavičky odpovědi mezipaměti bude vracet odpovědi obsahující hlavičku X-ES-Debug.
Zakázáno|Hlavička odpovědi X-ES-Debug budou vyloučeny z hello odpovědi.

**Výchozí chování:** zakázané.

###<a name="modify-client-response-header"></a>Upravit hlavičku odpovědi klienta
**Účel:** každý požadavek obsahuje sadu [hlavičky požadavku]() popisují ho. Tato funkce může buď:

- Připojení nebo přepsat hello hodnotu přiřazenou tooa hlavičky žádosti. Pokud hello zadaného záhlaví žádosti neexistuje, tato funkce ho pak přidá toohello požadavku.
- Odstraňte hlavičku požadavku z požadavku hello.

Požadavky, které jsou předávány tooan zdrojový server se projeví hello změny provedené při této funkce.

Jeden z následujících akcí hello lze provést na hlavičku požadavku:

Možnost|Popis|Příklad
-|-|-
Připojit|Hello zadaný toend hello existující hodnoty hlavičky požadavku bude přidána hodnota.|**Žádosti o hodnotu hlavičky (klient):**Value1 <br/> **Žádosti o hodnotu hlavičky (stroj pravidel HTTP):** hodnota2 <br/>**Nová hodnota hlavičky požadavku:** Value1Value2
Přepsání|žádost o Hello, bude použita hodnota hlavičky set toohello zadané hodnotě.|**Žádosti o hodnotu hlavičky (klient):**Value1 <br/>**Žádosti o hodnotu hlavičky (stroj pravidel HTTP):** hodnota2 <br/>**Nová hodnota hlavičky požadavku:** hodnota2 <br/>
Odstranění|Odstraní hello zadaného záhlaví žádosti.|**Žádosti o hodnotu hlavičky (klient):**Value1 <br/> **Změňte konfiguraci klienta hlavička požadavku:** dotyčném hlavička požadavku hello odstranit. <br/>**Výsledek:** hello Zadaná hlavička požadavku nebude předají toohello zdrojový server.

Informace o klíči:

- Ujistěte se, že hodnota hello zadaný v možnosti název je přesnou shodu hello požadované žádosti hlavičky.
- Případ nebere v úvahu hello za účelem identifikace hlavičku. Například některé z následujících variace název hlavičky Cache-Control hello lze použít tooidentify ho:
    - ovládací prvek mezipaměti
    - CACHE-CONTROL
    - cachE-Control
- Ujistěte se, že tooonly použít alfanumerické znaky, pomlčky nebo podtržítka při zadávání název hlavičky.
- Odstraňování hlavičku zabraňují předávaná tooan zdrojový server edge servery.
- Hello následující hlavičky jsou vyhrazeny a nelze změnit pomocí této funkce:
    - předané
    - hostitele
    - prostřednictvím
    - Upozornění
    - x předávaných pro
    - Všechny názvy záhlaví, které začínají "x ES" jsou vyhrazeny.

###<a name="modify-client-response-header"></a>Upravit hlavičku odpovědi klienta
Každá odpověď obsahuje sadu [hlavičky odpovědi]() popisují ho. Tato funkce může buď:

- Připojení nebo přepsat hello hodnotu přiřazenou tooa hlavičky odpovědi. Pokud hello zadaného záhlaví žádosti neexistuje, tato funkce ho pak přidá toohello odpovědi.
- Z odpovědi hello odstraňte hlavičky odpovědi.

Ve výchozím nastavení jsou definovány hodnoty hlavičky odpovědi původním serveru a naše servery edge.

Jeden z následujících akcí hello lze provést na hlavičku odpovědi:

Možnost|Popis|Příklad
-|-|-
Připojit|Hello zadaný toend hello existující hodnoty hlavičky požadavku bude přidána hodnota.|**Hodnota hlavičky odpovědi (klient):**Value1 <br/> **Hodnota hlavičky odpovědi (stroj pravidel HTTP):** hodnota2 <br/>**Nová hodnota hlavičky odpovědi:** Value1Value2
Přepsání|žádost o Hello, bude použita hodnota hlavičky set toohello zadané hodnotě.|**Hodnota hlavičky odpovědi (klient):**Value1 <br/>**Hodnota hlavičky odpovědi (stroj pravidel HTTP):** hodnota2 <br/>**Nová hodnota hlavičky odpovědi:** hodnota2 <br/>
Odstranění|Odstraní hello zadaného záhlaví žádosti.|**Žádosti o hodnotu hlavičky (klient):** Value1 <br/> **Změna konfigurace hlaviček žádostí klienta:** dotyčném hlavička odpovědi hello odstranit. <br/>**Výsledek:** hello Zadaná hlavička odpovědi nebude předají toohello žadatel.

Informace o klíči:

- Ujistěte se, že hello hodnota zadaný v možnosti název je přesnou shodu pro hlavičku odpovědi požadované hello. 
- Případ nebere v úvahu hello za účelem identifikace hlavičku. Například některé z následujících variace název hlavičky Cache-Control hello lze použít tooidentify ho:
    - ovládací prvek mezipaměti
    - CACHE-CONTROL
    - cachE-Control
- Odstraňování hlavičku zabraňují předávaná toohello žadatel.
- Hello následující hlavičky jsou vyhrazeny a nelze změnit pomocí této funkce:
    - Přijměte kódování
    - stáří
    - připojení
    - kódování obsahu
    - Délka obsahu
    - rozsah obsahu
    - Datum
    - server
    - přípojného
    - kódování přenosu
    - upgrade
    - lišit
    - prostřednictvím
    - Upozornění
    - Všechny názvy záhlaví, které začínají "x ES" jsou vyhrazeny.

###<a name="set-client-ip-custom-header"></a>Nastavit vlastní záhlaví IP klienta
**Účel:** přidá hlavičku vlastní, který identifikuje klienta, který hello požadavkem toohello IP adresu.

Možnost název hlavičky definuje hello název hlavičky vlastní žádosti hello uložení IP adresa klienta hello.

Tato funkce umožňuje zákazníkovi počátek toofind serveru na IP adresy klienta pomocí hlavička vlastní požadavku. Pokud je požadavek hello z mezipaměti, nebudou hello zdrojový server informováni o IP adresa klienta hello. Proto se doporučuje, aby tuto funkci používat s ADN nebo prostředky, které nebudou zapisována do mezipaměti.

Prosím zkontrolujte, zda že tento název Zadaná hlavička hello neodpovídá žádnému hello následující:

- Názvy záhlaví standardní žádosti. Seznam názvů standardní hlavičku naleznete v [dokumentu RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).
- Názvy vyhrazené záhlaví:
    - předané pro
    - hostitele
    - lišit
    - prostřednictvím
    - Upozornění
    - x předávaných pro
    - Všechny názvy záhlaví, které začínají "x ES" jsou vyhrazeny.
 
## <a name="logs"></a>Logs

Tyto funkce jsou navrženou toocustomize hello data uložená v souborech protokolů nezpracované.

Name (Název) | Účel
-----|--------
Pole vlastní protokol 1 | Určuje formát hello a hello obsah, který se přiřadí toohello vlastní protokol pole v souboru protokolu nezpracovaná.
Řetězec protokolu dotazu | Určuje, zda řetězec dotazu se uloží spolu s hello adresy URL v protokolech přístup.

###<a name="custom-log-field-1"></a>Pole vlastní protokol 1
**Účel:** Určuje formát hello a hello obsah, který se přiřadí toohello vlastní protokol pole v souboru protokolu nezpracovaná.

hlavním účelem Hello za toto vlastní pole je tooallow toodetermine které hodnoty hlavičky požadavku a odpovědi se uloží do souborů protokolu.

Ve výchozím nastavení pole vlastní protokol hello se nazývá "x-ec_custom-1." Ale hello název tohoto pole lze přizpůsobit z [stránka nastavení protokolu Raw]().

Níže jsou uvedeny Hello formátování, měli byste použít toospecify hlavičkách žádostí a odpovědí.

Záhlaví – typ|Formát|Příklady
-|-|-
Hlavička požadavku|%{[RequestHeader]()}[i]() | % {Přijmout Encoding} i <br/> {Odkazující server} i <br/> % {Autorizace} i
Hlavička odezvy|%{[ResponseHeader]()}[o]()| % {Stáří} o <br/> % {Content-Type} o <br/> % {Soubor cookie} o

Informace o klíči:

- Pole vlastní protokol může obsahovat libovolnou kombinaci pole hlavičky a prostý text.
- Platné znaky pro toto pole zahrnout následující hello: alfanumerické znaky (tj, 0 – 9, a-z a A-Z), pomlčky, dvojtečky, středníky, apostrofy, čárky, tečky, podtržítka, znaky rovná, závorky, závorky a mezery. Hello procento symbolů a složené závorky jsou povoleny pouze při používá toospecify pole hlavičky.
- Hello pravopis pro každé pole Zadaná hlavička musí odpovídat názvu záhlaví hello požadované žádosti a odpovědi.
- Pokud chcete toospecify více hlavičky a pak ho se doporučuje použít oddělovače tooindicate každá hlavička. Můžete například použít zkratkou pro každý záhlaví. Ukázka syntaxi najdete níže.
    - AE: % {přijmout Encoding} i odpověď: % {autorizace} i Berte: % {Content-Type} o 

**Výchozí hodnota:** -

###<a name="log-query-string"></a>Řetězec protokolu dotazu
**Účel:** Určuje, zda řetězec dotazu se uloží spolu s hello adresy URL v protokolech přístup.

Hodnota|výsledek
-|-
Povoleno|Při nahrávání adresy URL v protokolu přístup, umožňuje hello úložiště řetězců dotazů. Pokud adresu URL neobsahuje řetězec dotazu, tato možnost nebude mít vliv.
Zakázáno|Obnoví hello výchozí chování. Hello výchozí chování je tooignore řetězce dotazů při nahrávání adresy URL v přístupu k protokolu.

**Výchozí chování:** zakázané.

<!---
## Optimize

These features determine whether a request will undergo hello optimizations provided by Edge Optimizer.

Name | Purpose
-----|--------
Edge Optimizer | Determines whether Edge Optimizer can be applied tooa request.
Edge Optimizer – Instantiate Configuration | Instantiates or activates hello Edge Optimizer configuration associated with a site.

###Edge Optimizer
**Purpose:** Determines whether Edge Optimizer can be applied tooa request.

If this feature has been enabled, then hello following criteria must also be met before hello request will be processed by Edge Optimizer:

- hello requested content must use an edge CNAME URL.
- hello edge CNAME referenced in hello URL must correspond tooa site whose configuration has been activated in a rule.

This feature requires the ADN platform and hello Edge Optimizer feature.

Value|Result
-|-
Enabled|Indicates that hello request is eligible for Edge Optimizer processing.
Disabled|Restores hello default behavior. hello default behavior is toodeliver content over the ADN platform without any additional processing.

**Default Behavior:** Disabled
 

###Edge Optimizer - Instantiate Configuration
**Purpose:** Instantiates or activates hello Edge Optimizer configuration associated with a site.

This feature requires the ADN platform and hello Edge Optimizer feature.

Key information:

- Instantiation of a site configuration is required before requests toohello corresponding edge CNAME can be processed by Edge Optimizer.
- This instantiation only needs toobe performed a single time per site configuration. A site configuration that has been instantiated will remain in that state until hello Edge Optimizer – Instantiate Configuration feature that references it is removed from hello rule.
- hello instantiation of a site configuration does not mean that all requests toohello corresponding edge CNAME will automatically be processed by Edge Optimizer. The Edge Optimizer feature determines whether an individual request will be processed.

If hello desired site does not appear in hello list, then you should edit its configuration and verify that the Active option has been marked.

**Default Behavior:** Site configurations are inactive by default.
--->

## <a name="origin"></a>Zdroj

Tyto funkce jsou navrženou toocontrol jak hello CDN komunikuje s původním serveru.

Name (Název) | Účel
-----|--------
Udržování požadavky (maximum) | Definuje hello maximální počet požadavků pro zachování připojení, než je uzavřený.
Speciálními záhlavími proxy | Definuje hello sadu hlaviček požadavků specifické CDN, které budou předány z hraniční server tooan původní server.


###<a name="maximum-keep-alive-requests"></a>Udržování požadavky (maximum)
**Účel:** definuje hello maximální počet požadavků pro zachování připojení, než je uzavřený.

Nastavení maximální počet požadavků tooa nízkou hodnotu hello se důrazně nedoporučuje a může způsobit snížení výkonu.

Informace o klíči:

- Tuto hodnotu zadejte jako celé číslo.
- Nezahrnujte čárky nebo tečky v hello zadaná hodnota.

**Výchozí hodnota:** 10 000 požadavků

###<a name="proxy-special-headers"></a>Speciálními záhlavími proxy
**Účel:** definuje sadu hello [hlavičky požadavku CDN specifické]() , budou předány z hraniční server tooan původní server.

Informace o klíči:

- Každá hlavička požadavku CDN konkrétní definované v tato funkce se předají tooan zdrojový server.
- Hlavička požadavku CDN konkrétní zabránit předávaná tooan zdrojový server odebráním z tohoto seznamu.

**Výchozí chování:** všechny [hlavičky požadavku CDN specifické]() se předají toohello zdrojový server.

## <a name="specialty"></a>Speciální

Tyto funkce poskytují pokročilé funkce, která by měla být použita pouze zkušené uživatele.

Name (Název) | Účel
-----|--------
Metody HTTP lze uložit do mezipaměti | Určuje hello sadu další metody HTTP, které lze uložit do mezipaměti na naše síť.
Velikost textu lze uložit do mezipaměti žádosti | Definuje hello prahová hodnota pro určení, zda POST odpověď do mezipaměti.

###<a name="cacheable-http-methods"></a>Metody HTTP lze uložit do mezipaměti
**Účel:** určuje hello sadu další metody HTTP, které lze uložit do mezipaměti na naše síť.

Informace o klíči:

- Tato funkce se předpokládá, že GET odpovědi by měl vždycky být ukládat do mezipaměti. Hello metodu GET HTTP v důsledku toho by neměl být zahrnuty při nastavování této funkce.
- Tato funkce podporuje jenom hello metodu POST HTTP. Povolit POST ukládání odpovědí do mezipaměti podle nastavení této funkce: POST 
- Ve výchozím nastavení bude do mezipaměti pouze požadavky, jejichž text je menší než 14 Kb. Použijte funkci velikost textu lze uložit do mezipaměti žádosti nastavit velikost textu hello maximální požadavku.

**Výchozí chování:** pouze GET odpovědi bude ukládat do mezipaměti.

###<a name="cacheable-request-body-size"></a>Velikost textu lze uložit do mezipaměti žádosti

**Účel:** definuje hello prahová hodnota pro určení, zda POST odpověď do mezipaměti.

Tato prahová hodnota je určen podle určení velikosti textu maximální požadavku. Požadavky, které obsahují větší textu žádosti nebudou zapisována do mezipaměti.

Informace o klíči:

- Tato funkce se vztahuje pouze při odpovědi POST jsou způsobilé pro ukládání do mezipaměti. Povolit ukládání do mezipaměti v požadavku POST pomocí hello Uložitelný funkce metody HTTP.
- text žádosti Hello je brát v úvahu při:
    - hodnoty x--www-form-urlencoded
    - Zajištění jedinečný klíč mezipaměti
- Definování velkého požadavku maximální velikost textu může mít vliv na data výkonu doručení.
    - **Doporučená hodnota:** 14 Kb
    - **Minimální hodnota:** 1 Kb

**Výchozí chování:** 14 Kb
 
## <a name="url"></a>ADRESA URL

Tyto funkce umožňují toobe žádost o přesměrování nebo přepsaná tooa jinou adresu URL.

Name (Název) | Účel
-----|--------
Držet se přesměrování | Určuje, zda požadavky se dají přesměrovaného toohello hostname definované v záhlaví umístění hello vrácený zdrojový server zákazníka.
Adresa URL přesměrování | Přesměruje požadavky prostřednictvím hello hlavička umístění.
Přepisování adres URL  | Přepíše hello adrese URL žádosti.

###<a name="follow-redirects"></a>Držet se přesměrování
**Účel:** Určuje, zda požadavky se dají přesměrovaného toohello hostname definované v hlavičce umístění vrácený zdrojový server zákazníka.

Informace o klíči:

- Požadavky se dají jenom přesměrovaného tooedge záznamů CNAME, které odpovídají toohello stejnou platformu.

Hodnota|výsledek
-|-
Povoleno|Požadavky můžete přesměrovat.
Zakázáno|Požadavky nebude přesměrovat.

**Výchozí chování:** zakázané.
###<a name="url-redirect"></a>Adresa URL přesměrování
**Účel:** přesměruje požadavky prostřednictvím hlavička umístění.

Hello konfigurace této funkce vyžaduje nastavení hello následující možnosti:

Možnost|Popis
-|-
Kód|Vyberte kód odpovědi hello, který bude vrácen toohello žadatel.
Zdroj & vzor| Tato nastavení definovat vzor identifikátor URI požadavku, který identifikuje hello typ požadavků, které může být přesměrována. Bude přesměrovat pouze požadavky, jejichž adresa URL splňují obě hello následující kritéria: <br/> <br/> **Zdroj:** (nebo přístup k obsahu bodu) vyberte relativní cestu, která identifikuje zdrojový server. Toto je část "/XXXX/" hello a název koncového bodu. <br/> **Zdroj (vzor):** vzor, který identifikuje požadavky relativní cestou musí být definován. Tento vzor regulárního výrazu musí definovat cestu, která spustí přímo po hello dříve vybraném bodu přístup k obsahu (viz výše). <br/> -Zkontrolujte, zda že hello požadavku URI kritéria (tj. zdroj & vzor) definovaný nad není v konfliktu s veškeré podmínky shody definované pro tuto funkci. <br/> -Zkontrolujte, zda toospecify vzor. Pomocí prázdné hodnoty jako vzor hello bude odpovídat pouze požadavky toohello kořenové složky hello vybraný zdrojový server (například http://cdn.mydomain.com/).
Cíl| Zadejte adresu URL hello přesměrováni toowhich hello výše požadavky. <br/> Vytvořte dynamicky pomocí této adresy URL: <br/> -Vzor regulárního výrazu <br/>-HTTP proměnné <br/> Nahraďte hodnoty hello zachyceny ve vzorku zdroj hello vzoru cílové hello pomocí $_ n _ kde _ n _ identifikuje hodnotu podle hello pořadí, ve kterém byla zaznamenána. Například $1 představuje první hodnotu hello zaznamenané v hello zdroj vzor, zatímco $2 představuje hello druhá hodnota. <br/> 
Vysoce je doporučeno toouse absolutní adresu URL. použití Hello relativní adresa URL může přesměrovat neplatná cesta tooan CDN adresy URL.

**Vzorový scénář**

V tomto příkladu si předvedeme jak tooredirect okraj CNAME adresu URL, která přeloží toothis základní adresa URL CDN: http://marketing.azureedge.net/brochures

Určení požadavků bude přesměrované toothis edge základní adresa URL CNAME: http://cdn.mydomain.com/resources

Tato adresa URL přesměrování může dosáhnout pomocí hello následující konfigurace:![](./media/cdn-rules-engine-reference/cdn-rules-engine-redirect.png)

**Klíčové body:**

- Funkce přesměrování URL Hello definuje hello žádosti adresy URL, které bude přesměrován. V důsledku toho nejsou vyžadovány další shodu podmínky. I když podmínky shody hello byl definován jako "Vždy", pouze požadavky tohoto bodu toohello "brožury" přesměrováni složky na počátku zákazníka "marketing" hello. 
- Všechny odpovídající požadavky bude přesměrované toohello okraje, který CNAME URL definované v cílovém možnosti. 
    - Ukázkový scénář #1: 
        - Ukázková žádost (CDN URL): http://marketing.azureedge.net/brochures/widgets.pdf 
        - Adresa URL požadavku (po přesměrování): http://cdn.mydomain.com/resources/widgets.pdf  
    - Vzorový scénář #2: 
        - Ukázková žádost (hraniční CNAME URL): http://marketing.mydomain.com/brochures/widgets.pdf 
        - Adresa URL požadavku (po přesměrování): http://cdn.mydomain.com/resources/widgets.pdf vzorový scénář
    - Vzorový scénář #3: 
        - Ukázková žádost (hraniční CNAME URL): http://brochures.mydomain.com/campaignA/final/productC.ppt 
        - Adresa URL požadavku (po přesměrování): http://cdn.mydomain.com/resources/campaignA/final/productC.ppt  
- Proměnná Hello požadavku schématu (% {schéma}) byl využity v cílovém možnost. Tím se zajistí, že schéma této žádosti hello zůstává beze změny po přesměrování.
- Hello segmenty adres URL, které zaznamenalo z požadavku hello jsou nové adrese URL připojením toohello prostřednictvím "1 USD."
 
###<a name="url-rewrite"></a>Přepisování adres URL
**Účel:** přepíše hello adrese URL žádosti.

Informace o klíči:

- Hello konfigurace této funkce vyžaduje nastavení hello následující možnosti:

Možnost|Popis
-|-
 Zdroj & vzor | Tato nastavení definovat vzor identifikátoru URI požadavku, který identifikuje hello typ požadavků, které může být přepsána. Bude nutné přepsat pouze požadavky, jejichž adresa URL splňují obě hello následující kritéria: <br/>     - **Zdroj (nebo přístup k obsahu bodu):** vyberte relativní cestu, která identifikuje zdrojový server. Toto je část "/XXXX/" hello a název koncového bodu. <br/> - **Zdroj (vzor):** vzor, který identifikuje požadavky relativní cestou musí být definován. Tento vzor regulárního výrazu musí definovat cestu, která spustí přímo po hello dříve vybraném bodu přístup k obsahu (viz výše). <br/> Ujistěte se, že hello požadavku URI kritéria (tj. zdroj & vzor) definovaný nad není v konfliktu s některá z podmínek shodu hello definované pro tuto funkci. Ujistěte se, že toospecify vzor. Pomocí prázdné hodnoty jako vzor hello bude odpovídat pouze požadavky toohello kořenové složky hello vybraný zdrojový server (například http://cdn.mydomain.com/). 
 Cíl  |Zadejte relativní adresu URL hello bude přepsal toowhich hello výše požadavky: <br/>    1. Výběr bodu přístup k obsahu, který identifikuje zdrojový server. <br/>    2. Definování relativní cestu pomocí: <br/>        -Vzor regulárního výrazu <br/>        -HTTP proměnné <br/> <br/> Nahraďte hodnoty hello zachyceny ve vzorku zdroj hello vzoru cílové hello pomocí $_ n _ kde _ n _ identifikuje hodnotu podle hello pořadí, ve kterém byla zaznamenána. Například $1 představuje první hodnotu hello zaznamenané v hello zdroj vzor, zatímco $2 představuje hello druhá hodnota. 
 Tato funkce umožňuje naše servery edge toorewrite hello URL bez tradičních přesměrování. To znamená, že žadatel hello obdrží hello kódu odpovědi stejné jako v případě, kdyby byla požadována adresa URL hello přepsaná.

**Vzorový scénář 1**

V tomto příkladu si předvedeme jak tooredirect okraj CNAME adresu URL, která přeloží toothis základní adresa URL CDN: http://marketing.azureedge.net/brochures/

Určení požadavků bude přesměrované toothis edge základní adresa URL CNAME: http://MyOrigin.azureedge.net/resources/

Tato adresa URL přesměrování může dosáhnout pomocí hello následující konfigurace:![](./media/cdn-rules-engine-reference/cdn-rules-engine-rewrite.png)

**Vzorový scénář 2**

V tomto příkladu si předvedeme jak tooredirect okraj CNAME URL z velká toolowercase pomocí regulárních výrazů.

Tato adresa URL přesměrování může dosáhnout pomocí hello následující konfigurace:![](./media/cdn-rules-engine-reference/cdn-rules-engine-to-lowercase.png)


**Klíčové body:**

- Funkce přepisování adres URL Hello definuje hello žádosti adresy URL, které bude přepsán. V důsledku toho nejsou vyžadovány další shodu podmínky. I když podmínky shody hello byl definován jako "Vždy", pouze požadavky tohoto bodu toohello "brožury", bude nutné přepsat složky na počátku zákazníka "marketing" hello.

- Hello segmenty adres URL, které zaznamenalo z požadavku hello jsou nové adrese URL připojením toohello prostřednictvím "1 USD."



###<a name="compatibility"></a>Kompatibilita

Tato funkce zahrnuje odpovídající kritériím, které je nutné splnit, než může být použité tooa požadavku. V pořadí tooprevent nastavení konfliktní kritéria shody, tato funkce není kompatibilní s hello následující podmínky shody:

- JAKO počet
- Původu CDN
- IP adresa klienta
- Původ zákazníka
- Schéma požadavku
- Adresa URL cesta adresáře
- Rozšíření cesty adresy URL
- Název souboru cestu adresy URL
- Literál cestu adresy URL
- Regulární výraz cesty adresy URL
- Cesta URL zástupný znak
- Adresa URL dotazu literál
- Parametr URL dotazu
- Adresa URL dotazu Regex
- Adresa URL dotazu zástupný znak


## <a name="next-steps"></a>Další kroky
* [Referenční dokumentace pravidel modulu](cdn-rules-engine-reference.md)
* [Podmíněné výrazy stroj pravidel](cdn-rules-engine-reference-conditional-expressions.md)
* [Stav shody motoru pravidla](cdn-rules-engine-reference-match-conditions.md)
* [Přepsání výchozího nastavení HTTP používá stroj pravidel hello](cdn-rules-engine.md)
* [Přehled Azure CDN](cdn-overview.md)
