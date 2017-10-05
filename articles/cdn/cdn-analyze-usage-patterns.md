---
title: "Analýza vzorce používání Azure CDN | Microsoft Docs"
description: "Vzorce můžete zobrazit pro vaše CDN pomocí následující sestavy: šířky pásma, přenesená Data, přístupů, mezipaměti stavy, poměr přístupů do mezipaměti, přenesená Data IPV4 nebo IPV6."
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
ms.openlocfilehash: aadbe872dd3384c8d337b432fb3be69422ca322b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Analýza vzorce používání Azure CDN

[!INCLUDE[cdn-verizon-only](../../includes/cdn-verizon-only.md)]

V Průvodci níže projde kroky k zobrazení sestav základní prostřednictvím portálu spravovat Verizon profilů. Můžete také exportovat základní analytická data do úložiště, se v Centru událostí nebo protokolu analytics (oms) pro profily Verizon a Akamai [prostřednictvím portálu azure](cdn-log-analysis.md).

Vzorce můžete zobrazit pro vaše CDN pomocí následující sestavy:

* Šířka pásma
* Data přenesená
* Přístupů
* Stavy mezipaměti
* Poměr přístupů do mezipaměti
* IPV4/IPV6 Data přenesená

## <a name="accessing-core-reports"></a>Přístup k základní sestavy
1. Okno profil CDN, klikněte **spravovat** tlačítko.
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-reports/cdn-manage-btn.png)
   
    Otevře se na portálu pro správu CDN.
2. Najeďte myší **Analytics** a potom přejděte myší **základní sestavy** plovoucím panelem.  Klikněte na požadovanou sestavu v nabídce.
   
    ![Portál pro správu CDN - nabídka základní sestavy](./media/cdn-reports/cdn-core-reports.png)

## <a name="bandwidth"></a>Šířka pásma
Sestava šířky pásma se skládá z graf a datovou tabulku označující využití šířky pásma pro protokol HTTP a HTTPS za určité časové období. Zobrazí se využití šířky pásma mezi všechny servery CDN POP nebo konkrétní POP. To vám umožní zobrazit provoz špičky a distribuci napříč CDN POP v MB/s.

* Vyberte všechny uzly okraj přenosy ze všech uzlů nebo zvolit konkrétní oblasti nebo uzel z rozevíracího seznamu.
* Vyberte rozsah dat pro zobrazení dat pro dnešní, tato týdnu/tohoto měsíce, atd. nebo zadejte vlastní data a potom klepněte na tlačítko "Vyhledat" a ujistěte se, že se aktualizuje váš výběr.
* Můžete exportovat a stáhnout data kliknutím na ikonu listu aplikace excel vedle "přejděte".

Sestava je aktualizováno každých 5 minut.

![Sestava šířky pásma](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Data přenesená
Tato sestava se skládá z grafu a data tabulky označující využití přenosy pro protokol HTTP a HTTPS za určité časové období. Využití provozu můžete zobrazit mezi všechny servery CDN POP nebo konkrétní POP. To vám umožní zobrazit provoz špičky a distribuci napříč CDN POP v GB.

* Vyberte všechny uzly okraj vidět provoz v všechny poznámky nebo vyberte konkrétní oblasti nebo uzel z rozevíracího seznamu.
* Vyberte rozsah dat pro zobrazení dat pro dnešní, tato týdnu/tohoto měsíce, atd. nebo zadejte vlastní data a potom klepněte na tlačítko "Vyhledat" a ujistěte se, že se aktualizuje váš výběr.
* Můžete exportovat a stáhnout data kliknutím na ikonu listu aplikace excel vedle "přejděte".

Sestava je aktualizováno každých 5 minut.

![Přenesená data sestavy](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Přístupů (stavové kódy)
Tato sestava popisuje distribuci požadavek stavové kódy pro obsah. Každý požadavek pro obsah vygeneruje stavový kód HTTP. Stavový kód popisuje, jak bodů POP edge zpracování žádosti. Například 2xx stavové kódy znamenat, že žádost úspěšně vyřídila ke klientovi, zatímco stavový kód 4xx značí, že došlo k chybě. Další podrobnosti o stavový kód HTTP najdete v tématu [stavové kódy](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

* Vyberte rozsah dat pro zobrazení dat pro dnešní, tato týdnu/tohoto měsíce, atd. nebo zadejte vlastní data a potom klepněte na tlačítko "Vyhledat" a ujistěte se, že se aktualizuje váš výběr.
* Můžete exportovat a stáhnout data kliknutím excelovém listu vedle "přejděte".

![Sestava přístupů](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Stavy mezipaměti
Tato sestava popisuje distribuci přístupů k mezipaměti a Neúspěšné přístupy do mezipaměti pro požadavek klienta. Vzhledem k tomu, že nejrychlejší výkon pochází z přístupů k mezipaměti, můžete optimalizovat rychlosti doručování dat minimalizovat Neúspěšné přístupy do mezipaměti a přístupů k mezipaměti vypršela platnost. Neúspěšné přístupy do mezipaměti se může snížit konfigurací serveru počátek nepřiřazujte hlavičky odpovědi "no-cache", vyhnout, řetězec dotazu do mezipaměti s výjimkou, kde je nezbytně nutné a zabráněním kódy neurčené odpovědi. Vypršela platnost mezipaměti přístupů můžete vyhnout tím, že prostředek je maximální stáří stejně dlouho chcete-li minimalizovat počet požadavků na zdrojový server.

![Sestavy stavů mezipaměti](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Hlavní mezipaměti stavy, které patří:
* TCP_HIT: Zpracovat od okraje. Objekt byl v mezipaměti a kdyby překročil jeho maximální stáří.
* TCP_MISS: Zpracovat z počátku. Objekt nebyl v mezipaměti a odpověď byla zpět na počátku.
* TCP_EXPIRED _MISS: obsluhovat z počátku po opětovné ověření s původu. Objekt byl v mezipaměti, ale překročil jeho maximální stáří. Opětovné ověření s původem výsledkem objekt mezipaměti nahrazují novou odpověď z počátku.
* TCP_EXPIRED _HIT: zpracovaných od okraje po opětovné ověření s původu. Objekt byl v mezipaměti, ale překročil jeho maximální stáří. Opětovné ověření je zdrojový server má za následek objekt mezipaměti se ponechat beze změny.
* Vyberte rozsah dat pro zobrazení dat pro dnešní, tato týdnu/tohoto měsíce, atd. nebo zadejte vlastní data a potom klepněte na tlačítko "Vyhledat" a ujistěte se, že se aktualizuje váš výběr.
* Můžete exportovat a stáhnout data kliknutím na ikonu listu aplikace excel vedle "přejděte".

### <a name="full-list-of-cache-statuses"></a>Úplný seznam stavů mezipaměti
* TCP_HIT – tento stav se zobrazí, když požadavek pochází přímo z POP do klienta. Prostředek se okamžitě zpracovat z POP, když se uloží do mezipaměti na serveru POP nejbližší klientovi a má platný time to live, nebo hodnotu TTL. Hodnota TTL je dáno následující hlavičky odpovědi:
  
  * Cache-Control: s-maxage
  * Cache-Control: maximální stáří
  * Vypršení platnosti
* TCP_MISS – tento stav indikuje, že v mezipaměti verzi požadovaný prostředek nebyl nalezen na serveru POP nejbližší klientovi. Asset bude vyžádána ze zdrojového serveru nebo serveru štítu původu. Pokud je zdrojový server nebo server štítu počátek vrátí prostředek, bude obsluhovat klientovi a ukládat do mezipaměti na klientovi i serveru edge. Jinak hodnota bez 200 stavový kód (například 403 Zakázáno, 404 nebyl nalezen, apod.) bude vrácen.
* TCP_EXPIRED _HIT – tento stav se zobrazí při zpracování žádosti, která cílem prostředek s vypršenou platností TTL, například pokud vypršela platnost assetu, max-age, přímo z POP do klienta.
  
    Vypršela platnost žádosti o žádost o opětovné ověření obvykle výsledkem na zdrojový server. Aby TCP_EXPIRED _HIT proběhnout musí na zdrojový server znamenat, že na novější verzi asset neexistuje. Takové situaci se obvykle aktualizace Cache-Control a Expires záhlaví tohoto prostředku.
* TCP_EXPIRED _MISS – tento stav se zobrazí při na novější verzi prostředek s vypršenou platností v mezipaměti pochází z POP do klienta. K tomu dojde, pokud vypršela platnost TTL pro prostředek v mezipaměti (např. platnost maximální stáří) a zdrojový server vrátí na novější verzi tohoto prostředku. Tuto novou verzi prostředku je rezervována pro klienta místo v mezipaměti. Kromě toho se bude do mezipaměti na hraniční server a klienta.
* CONFIG_NOCACHE – tento stav indikuje, že zákaznické konfigurace na našem edge POP zabránila asset ukládat do mezipaměti.
* ŽÁDNÁ – tento stav označuje, že nebyla provedena kontrola aktuálnost obsahu mezipaměti.
* TCP_ CLIENT_REFRESH _MISS – tento stav se zobrazí, když klientem HTTP (například prohlížeče) vynutí okraj POP načíst nové verze zastaralé asset ze zdrojového serveru.
  
    Ve výchozím nastavení naše servery zabránit klientem HTTP vynucení našich edge serverům pro načtení novou verzi asset ze zdrojového serveru.
* TCP_ PARTIAL_HIT – tento stav se zobrazí, pokud žádost o rozsah bajtů, které jsou výsledkem stiskněte klávesu pro prostředek částečně v mezipaměti. Požadovaný rozsah bajtů je okamžitě zpracovat ze serveru POP do klienta.
* UNCACHEABLE – tento stav se zobrazí, když hlavičky Cache-Control a Expires prostředek služby znamenat, že by neměl být uložené v mezipaměti, na serveru POP nebo klienta HTTP. Tyto typy požadavků se zpracovávají ze zdrojového serveru

## <a name="cache-hit-ratio"></a>Poměr přístupů do mezipaměti
Tato sestava informuje o procentu uložené v mezipaměti požadavků, které se spouští přímo z mezipaměti.

Sestava obsahuje následující podrobnosti:

* Požadovaný obsah se ukládá do mezipaměti na serveru POP nejblíže k žadatel.
* Zpracování žádosti přímo z okraje naše sítě.
* Požadavek nebylo nutné opětovné ověření je zdrojový server.

Sestava neobsahuje:

* Požadavky, které jsou odepřen v důsledku země možnosti filtrování.
* Požadavky na prostředky, jejichž hlavičky znamenat, že by neměl být mezipaměti. Například Cache-Control: privátní, Cache-Control: bez mezipaměti, nebo – direktiva Pragma: Ne mezipaměti hlavičky zabrání prostředek v mezipaměti.
* Požadavky na rozsah bajtů částečně v mezipaměti obsahu.

Vzorec je: (stiskněte KLÁVESU TCP_ / (stiskněte KLÁVESU TCP_ + TCP_MISS)) * 100

* Vyberte rozsah dat pro zobrazení dat pro dnešní, tato týdnu/tohoto měsíce, atd. nebo zadejte vlastní data a potom klepněte na tlačítko "Vyhledat" a ujistěte se, že se aktualizuje váš výběr.
* Můžete exportovat a stáhnout data kliknutím na ikonu listu aplikace excel vedle "přejděte".

![Sestava poměr přístupů do mezipaměti](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>Přenesená Data IPV4/IPV6
Tato sestava zobrazuje využití distribuce přenosů v sadě vs IPV4 protokol IPV6.

![Přenesená Data IPV4/IPV6](./media/cdn-reports/cdn-ipv4-ipv6.png)

* Vyberte rozsah dat k zobrazení dat pro dnešní, tato týdnu/tohoto měsíce, atd., nebo zadejte vlastní data.
* Potom klepněte na tlačítko "Vyhledat" a ujistěte se, že se aktualizuje váš výběr.

## <a name="considerations"></a>Požadavky
Sestavy můžete generovat pouze během posledních 18 měsíců.

