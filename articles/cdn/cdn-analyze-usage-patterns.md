---
title: "vzorce používání Azure CDN aaaAnalyze | Microsoft Docs"
description: "Vzorce můžete zobrazit pro vaše CDN pomocí hello následující sestavy: šířky pásma, přenesená Data, přístupů, mezipaměti stavy, poměr přístupů do mezipaměti, přenesená Data IPV4 nebo IPV6."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 5a0d9018-8bdb-48ff-84df-23648ebcf763
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: d27e6f60acaed66abb27d860c3a3e2e81c9f60cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Analýza vzorce používání Azure CDN

[!INCLUDE[cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Průvodce Hello níže projde hello kroky tooview hello základní sestavy prostřednictvím portálu spravovat hello Verizon profilů. Můžete také exportovat základní analýzy dat toostorage, centra událostí nebo analýzy protokolů (oms) pro profily Verizon a Akamai [prostřednictvím portálu hello azure](cdn-log-analysis.md).

Vzorce můžete zobrazit pro vaše CDN pomocí hello následující sestavy:

* Šířka pásma
* Data přenesená
* Přístupů
* Stavy mezipaměti
* Poměr přístupů do mezipaměti
* IPV4/IPV6 Data přenesená

## <a name="accessing-core-reports"></a>Přístup k základní sestavy
1. Z hello okno profil CDN, klikněte na tlačítko hello **spravovat** tlačítko.
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-reports/cdn-manage-btn.png)
   
    Otevře portál pro správu Hello CDN.
2. Hover přes hello **Analytics** kartu a potom hover přes hello **základní sestavy** plovoucím panelem.  Klikněte na sestavu hello potřeby v nabídce hello.
   
    ![Portál pro správu CDN - nabídka základní sestavy](./media/cdn-reports/cdn-core-reports.png)

## <a name="bandwidth"></a>Šířka pásma
Sestava Hello šířky pásma se skládá z graf a datovou tabulku označující hello využití šířky pásma pro protokol HTTP a HTTPS za určité časové období. Zobrazí se využití šířky pásma hello mezi všechny servery CDN POP nebo konkrétní POP. Díky tomu můžete tooview hello provoz špičky a distribuci napříč CDN POP v MB/s.

* Vyberte všechny uzly okraj toosee provoz ze všech uzlů nebo zvolte konkrétní oblasti nebo uzel z rozevíracího seznamu hello.
* Vyberte datum rozsahu tooview dat pro dnešní, tato týdnu/tohoto měsíce, atd. nebo zadejte vlastní data a potom klikněte na tlačítko "jít" toomake se, že se aktualizuje váš výběr.
* Můžete exportovat a stahování dat hello kliknutím hello v aplikaci excel list Ikona vedle nachází příliš "spustit".

Sestava Hello se aktualizuje každých 5 minut.

![Sestava šířky pásma](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Data přenesená
Tato sestava se skládá z grafu a data tabulky označující využití hello přenosy pro protokol HTTP a HTTPS za určité časové období. Využití hello provozu můžete zobrazit mezi všechny servery CDN POP nebo konkrétní POP. Díky tomu můžete tooview hello provoz špičky a distribuci napříč CDN POP v GB.

* Vyberte všechny uzly okraj toosee provoz ze všech poznámky nebo zvolte konkrétní oblasti nebo uzel z rozevíracího seznamu hello.
* Vyberte datum rozsahu tooview dat pro dnešní, tato týdnu/tohoto měsíce, atd. nebo zadejte vlastní data a potom klikněte na tlačítko "jít" toomake se, že se aktualizuje váš výběr.
* Můžete exportovat a stahování dat hello kliknutím hello v aplikaci excel list Ikona vedle nachází příliš "spustit".

Sestava Hello se aktualizuje každých 5 minut.

![Přenesená data sestavy](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Přístupů (stavové kódy)
Tato sestava popisuje hello distribuční požadavek stavové kódy pro obsah. Každý požadavek pro obsah vygeneruje stavový kód HTTP. Hello stavový kód popisuje, jak bodů POP edge zpracování požadavku hello. Například 2xx stavové kódy označují, že tento hello žádost úspěšně vyřídila tooa klienta, zatímco stavový kód 4xx značí, že došlo k chybě. Další podrobnosti o stavový kód HTTP najdete v tématu [stavové kódy](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

* Vyberte datum rozsahu tooview dat pro dnešní, tato týdnu/tohoto měsíce, atd. nebo zadejte vlastní data a potom klikněte na tlačítko "jít" toomake se, že se aktualizuje váš výběr.
* Můžete exportovat a stahování dat hello kliknutím hello v aplikaci excel list umístěný vedle příliš "spustit".

![Sestava přístupů](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Stavy mezipaměti
Tato sestava popisuje hello distribuce přístupů k mezipaměti a Neúspěšné přístupy do mezipaměti pro požadavek klienta. Vzhledem k tomu, že nejrychlejší výkonu hello pochází z přístupů k mezipaměti, můžete optimalizovat rychlosti doručování dat minimalizovat Neúspěšné přístupy do mezipaměti a přístupů k mezipaměti vypršela platnost. Neúspěšné přístupy do mezipaměti se může snížit nakonfigurováním tooavoid serveru váš počátek přiřazení hlavičky odpovědi "no-cache", vyhnout, řetězec dotazu do mezipaměti s výjimkou, kde je nezbytně nutné a zabráněním kódy neurčené odpovědi. Platnost mezipaměti přístupů můžete vyhnout tím, že je prostředek, max-age tak dlouho, dokud možné toominimize hello počet požadavků toohello zdrojový server.

![Sestavy stavů mezipaměti](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Hlavní mezipaměti stavy, které patří:
* TCP_HIT: Zpracovat od okraje. objekt Hello byla v mezipaměti a kdyby překročil jeho maximální stáří.
* TCP_MISS: Zpracovat z počátku. Hello objekt nebyl v mezipaměti a hello odpověď byla back tooorigin.
* TCP_EXPIRED _MISS: obsluhovat z počátku po opětovné ověření s původu. objekt Hello byla v mezipaměti, ale překročil jeho maximální stáří. Opětovné ověření s původem výsledkem objekt mezipaměti hello nahrazují novou odpověď z počátku.
* TCP_EXPIRED _HIT: zpracovaných od okraje po opětovné ověření s původu. objekt Hello byla v mezipaměti, ale překročil jeho maximální stáří. Opětovné ověření hello zdrojový server má za následek objekt mezipaměti hello se ponechat beze změny.
* Vyberte datum rozsahu tooview dat pro dnešní, tato týdnu/tohoto měsíce, atd. nebo zadejte vlastní data a potom klikněte na tlačítko "jít" toomake se, že se aktualizuje váš výběr.
* Můžete exportovat a stahování dat hello kliknutím hello v aplikaci excel list Ikona vedle nachází příliš "spustit".

### <a name="full-list-of-cache-statuses"></a>Úplný seznam stavů mezipaměti
* TCP_HIT – tento stav se zobrazí, když požadavek pochází přímo z hello POP toohello klienta. Prostředek se okamžitě zpracovat z POP, když se uloží do mezipaměti na klientovi nejbližší toohello hello POP a má platný time to live, nebo hodnotu TTL. Hodnota TTL je dáno hello následující hlavičky odpovědi:
  
  * Cache-Control: s-maxage
  * Cache-Control: maximální stáří
  * Vypršení platnosti
* TCP_MISS – tento stav indikuje, že v mezipaměti verzi hello požadovaný prostředek nebyl nalezen na hello POP nejbližší toohello klienta. Hello asset bude vyžádána ze zdrojového serveru nebo serveru štítu původu. Pokud hello zdrojový server nebo server štítu počátek hello vrátí prostředek, bude obsluhovat toohello klienta a ukládat do mezipaměti na hello klient i hello edge server. Jinak hodnota bez 200 stavový kód (například 403 Zakázáno, 404 nebyl nalezen, apod.) bude vrácen.
* TCP_EXPIRED _HIT – tento stav se zobrazí při zpracování žádosti, která cílem prostředek s vypršenou platností TTL, například pokud vypršela platnost hello asset, max-age, přímo z klienta toohello hello POP.
  
    Vypršela platnost žádosti obvykle výsledkem opětovné ověření požadavku toohello zdrojový server. Aby toooccur _HIT TCP_EXPIRED musí označovat hello zdrojový server, že na novější verzi hello prostředek neexistuje. Takové situaci se obvykle aktualizace Cache-Control a Expires záhlaví tohoto prostředku.
* TCP_EXPIRED _MISS – tento stav se zobrazí při na novější verzi prostředek s vypršenou platností v mezipaměti pochází z klienta toohello hello POP. K tomu dojde, pokud vypršela platnost hello TTL pro prostředek v mezipaměti (např. platnost maximální stáří) a zdrojový server hello vrátí na novější verzi tohoto prostředku. Tato nová verze hello asset se zpracuje klienta toohello místo v mezipaměti verze hello. Kromě toho se bude do mezipaměti na serveru edge hello a hello klientovi.
* CONFIG_NOCACHE – tento stav indikuje, že zákaznické konfigurace na našem edge POP zabránila hello asset ukládat do mezipaměti.
* ŽÁDNÁ – tento stav označuje, že nebyla provedena kontrola aktuálnost obsahu mezipaměti.
* TCP_ CLIENT_REFRESH _MISS – tento stav je hlášené klientem HTTP (například prohlížeče) vynutí hraniční POP tooretrieve novou verzi zastaralé asset z hello zdrojový server.
  
    Ve výchozím nastavení naše servery zabránit klientem HTTP vynucení našich hraniční servery tooretrieve novou verzi hello asset z hello zdrojový server.
* TCP_ PARTIAL_HIT – tento stav se zobrazí, pokud žádost o rozsah bajtů, které jsou výsledkem stiskněte klávesu pro prostředek částečně v mezipaměti. Hello požadovaného rozsahu bajtů je okamžitě obsluhovat z hello POP toohello klienta.
* UNCACHEABLE – tento stav se zobrazí, když prostředek služby Cache-Control a hlavičky Expires indikovat, že by neměl být uložen do mezipaměti, na serveru POP nebo klientem hello protokolu HTTP. Tyto typy požadavků je zobrazovaná zdrojový server hello

## <a name="cache-hit-ratio"></a>Poměr přístupů do mezipaměti
Tato sestava ukazuje procento hello uložené v mezipaměti požadavků, které se spouští přímo z mezipaměti.

Sestava Hello obsahuje hello následující podrobnosti:

* Hello požadovaného obsah se ukládá do mezipaměti na hello POP nejbližší toohello žadatel.
* zpracování žádosti Hello přímo z hello hrany naše síť.
* žádost o Hello nebylo nutné opětovné ověření hello zdrojový server.

Hello sestava neobsahuje:

* Požadavků, kterým je odepřen z důvodu možnosti filtrování toocountry.
* Požadavky na prostředky, jejichž hlavičky znamenat, že by neměl být mezipaměti. Například Cache-Control: privátní, Cache-Control: bez mezipaměti, nebo – direktiva Pragma: Ne mezipaměti hlavičky zabrání prostředek v mezipaměti.
* Požadavky na rozsah bajtů částečně v mezipaměti obsahu.

Vzorec Hello je: (stiskněte KLÁVESU TCP_ / (stiskněte KLÁVESU TCP_ + TCP_MISS)) * 100

* Vyberte datum rozsahu tooview dat pro dnešní, tato týdnu/tohoto měsíce, atd. nebo zadejte vlastní data a potom klikněte na tlačítko "jít" toomake se, že se aktualizuje váš výběr.
* Můžete exportovat a stahování dat hello kliknutím hello v aplikaci excel list Ikona vedle nachází příliš "spustit".

![Sestava poměr přístupů do mezipaměti](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>Přenesená Data IPV4/IPV6
Tato sestava zobrazí distribuce využití přenosů hello v sadě vs IPV4 protokol IPV6.

![Přenesená Data IPV4/IPV6](./media/cdn-reports/cdn-ipv4-ipv6.png)

* Vyberte datum rozsahu tooview dat pro dnešní, tato týdnu/tohoto měsíce, atd., nebo zadejte vlastní data.
* Potom klikněte na "jít" toomake se, že se aktualizuje váš výběr.

## <a name="considerations"></a>Požadavky
Sestavy můžete generovat pouze v rámci hello poslední 18 měsíců.

