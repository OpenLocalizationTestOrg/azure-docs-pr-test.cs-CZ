---
title: "aaaMedia streamování optimalizace prostřednictvím hello Azure Content Delivery Network"
description: "Optimalizovat datové proudy mediálních souborů pro smooth doručení"
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: v-semcev
ms.openlocfilehash: a05a86204708c7ea7ef1f9be04323cdda6a2d403
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="media-streaming-optimization-via-hello-azure-content-delivery-network"></a>Optimalizace prostřednictvím hello Azure Content Delivery Network streamování médií 
 
Použití vysokým rozlišením video roste na hello Internetu, které vytvoří problémy pro efektivní doručování velkých souborů. Zákazníci očekávat plynulé přehrávání videa na vyžádání nebo live video prostředků v různých sítí a klienti po celém hello, world. Doručení rychlé a efektivní mechanismus pro streamování soubory médií je důležité tooensure prostředí hladký a zábavná příjemce.  

Živé streamování médií je obzvláště složité toodeliver z důvodu velikostí velkých hello a počet souběžných prohlížeče. Velká zpoždění způsobit tooleave uživatele. Protože živé datové proudy nelze uložit do mezipaměti dopředu a velké latence nejsou přijatelné tooviewers, videa fragmenty musí doručit včas. 

vzory požadavku Hello streamování také poskytují další výzvy. Uvolnění oblíbených živý datový proud nebo nové řady pro video na vyžádání, tisíc toomillions prohlížeče si mohou vyžádat hello datový proud na hello stejnou dobu. V takovém případě inteligentní žádosti konsolidace je životně důležité toonot zahlcovat hello počátek serverů při hello prostředky nejsou ještě do mezipaměti.
 
Hello Azure Content Delivery Network společnosti Akamai teď nabízí funkce, která poskytuje prostředky ke streamování médií efektivně toousers napříč hello zeměkouli ve velkém měřítku. Funkce Hello snižuje latenci, protože snižuje zatížení hello hello počátek serverů. Tato funkce je k dispozici hello Standard Akamai cenovou úroveň. 

Hello Azure Content Delivery Network od společnosti Verizon přináší mediálních datových proudů přímo v hello obecné webové doručení optimalizace typu.
 
## <a name="configure-an-endpoint-toooptimize-media-streaming-in-hello-azure-content-delivery-network-from-akamai"></a>Konfigurace média toooptimize koncový bod streamování v hello Azure Content Delivery Network společnosti Akamai
 
Můžete nakonfigurovat doručení doručování obsahu sítě (CDN) koncový bod toooptimize pro velkých souborů prostřednictvím hello portálu Azure. Můžete také použít rozhraní API REST nebo žádné z hello klientské sady SDK toodo to. Hello následující kroky ukazují hello proces prostřednictvím hello portálu Azure:

1. tooadd nový koncový bod na hello **profil CDN** vyberte **koncový bod**.
  
    ![Nový koncový bod](./media/cdn-media-streaming-optimization/01_Adding.png)

2. V hello **optimalizované pro** rozevíracího seznamu vyberte **videa na vyžádání streamování médií** pro vyžádání video-on-demand prostředky. Pokud tak učiníte, kombinace za provozu a na vyžádání video-on-demand streamování, vyberte **datové proudy obecné media**.

    ![Streamování vybrané](./media/cdn-media-streaming-optimization/02_Creating.png) 
 
Po vytvoření koncového bodu hello, bude se vztahovat hello optimalizace pro všechny soubory, které splňují určitá kritéria. Hello následující část popisuje tento proces. 
 
## <a name="media-streaming-optimizations-for-hello-azure-content-delivery-network-from-akamai"></a>Optimalizace pro hello Azure Content Delivery Network společnosti Akamai streamování médií
 
Médium, které za provozu je platná pro streamování optimalizace společnosti Akamai nebo na vyžádání video-on-demand streamování média, které používá jednotlivých média fragmenty pro doručení. Tento proces se liší od jednoho velkého asset přenést přes progresivní stahování nebo pomocí žádosti o rozsah bajtů. Informace v tomto styl doručování médií najdete v tématu [velkých souborů optimalizace](cdn-large-file-optimization.md).


Hello obecné doručení nebo na vyžádání video-on-demand média doručení optimalizace typy médií používají název CDN s back-end optimalizace toodeliver média prostředky rychlejší. Také používají konfigurace pro média prostředky na základě osvědčených postupů se naučili v čase.

### <a name="caching"></a>Ukládání do mezipaměti

Pokud hello Azure Content Delivery Network společnosti Akamai zjistí, že tohoto prostředku hello je streamování manifest nebo fragment, používá ukládání do mezipaměti vypršení platnosti odděleně od obecné webové doručení. (Viz úplný seznam hello v hello následující tabulka). Jako vždy jsou dodržení cache-control nebo Expires hlavičky odeslaný hello původu. Pokud hello asset není asset média, se ukládá do mezipaměti pomocí hello vypršení doby pro obecné webové doručení.

Hello krátké ukládání do mezipaměti záporný čas je užitečné pro přesměrování zpracování počátku, když mnoho uživatelů požaduje fragment, která ještě neexistuje. Příkladem je živý datový proud, kde pakety hello nejsou k dispozici z hello počátku této sekundu. už ukládání do mezipaměti interval Hello také pomáhá přesměrovat zpracování požadavků z počátku hello, protože obsahu videa se obvykle nemění.
 

|    | Obecné<br> webové<br>doručení | Obecné<br> média<br> streamování | Vyžádání Video-on-Demand <br>média<br> streamování  
--- | --- | --- | ---
Ukládání do mezipaměti: kladné <br> HTTP 200, 203, 300, <br> 301, 302 a 410 | 7 dní |365 dnů | 365 dnů   
Ukládání do mezipaměti: záporná <br> HTTP 204, 305, 404, <br> a 405 | Žádný | 1 sekunda | 1 sekunda
 
### <a name="deal-with-origin-failure"></a>Řešení s chybou počátek  

Obecné média doručení a doručování média na vyžádání video-on-demand také mají původ časový limit a protokolu opakování podle osvědčené postupy pro vzory typické požadavku. Protože doručení obecné média je pro za provozu a doručování média na vyžádání video-on-demand, používá kratší časový limit připojení z důvodu povaze dobou toohello živé například streamování.

Pokud vyprší časový limit připojení, hello CDN opakuje několikrát, než odešle klient toohello chyba "504 – časový limit brány". 

Když soubor odpovídá hello typ a velikost podmínky seznam souborů, používá hello CDN hello chování pro streamování médií. Jinak použije obecné webové doručení.
   
### <a name="conditions-for-media-streaming-optimization"></a>Podmínky pro optimalizaci streamování médií 

Hello následující tabulka uvádí hello sadu kritérií toobe splněna pro streamování optimalizace médií: 
 
Podporované typy datových proudů | Přípony souborů  
--- | ---  
Apple HLS | m3u8, m3u, m3ub, klíč, ts, aac
Adobe HDS | f4m, f4x, drmmeta, bootstrap, f4f,<br>Adresa URL KŘEH seg – struktura <br> (odpovídající regulární výraz: ^(/.*)Seq(\d+)-Frag(\d+)
POMLČKA | MPD, dash, divx, ismv, m4s, m4v, mp4, mp4v, <br> sidx, WBEM, mp4a, m4a, isma
Technologie Smooth streaming | / manifest /, fragmenty/QualityLevels / /
  

 
## <a name="media-streaming-optimizations-for-hello-azure-content-delivery-network-from-verizon"></a>Optimalizace pro hello Azure Content Delivery Network od společnosti Verizon streamování médií

Hello Azure Content Delivery Network od společnosti Verizon poskytuje prostředky ke streamování médií přímo pomocí hello obecné webové doručení optimalizace typu. Několik funkcí na hello CDN přímo pomáhají doručovat média prostředky ve výchozím nastavení.

### <a name="partial-cache-sharing"></a>Sdílení částečné mezipaměti

Sdílení částečné mezipaměti umožňuje hello CDN tooserve částečně uložené v mezipaměti obsahu toonew požadavky. Například pokud hello první požadavek toohello CDN výsledkem k neúspěšnému přístupu do mezipaměti, hello požadavek odeslán toohello původu. I když tento obsah neúplné je načten do hello mezipaměti CDN, můžete spustit další požadavky toohello CDN získávání tato data. 

### <a name="cache-fill-wait-time"></a>Doba čekání výplně mezipaměti

 Hello mezipaměti výplně čekací doba funkce vynutí hello hraniční server toohold všechny následné žádosti pro stejný zdroj hello dokud odpověď HTTP, které hlavičky přicházejí z hello zdrojový server. Pokud hlavičky HTTP odpovědi z počátku hello přicházejí před vypršením platnosti hello časovače, se zpracovávají všechny požadavky, které byly blokovat mimo hello narůstají mezipaměti. V hello stejný čas, hello mezipaměti vyplní dat z počátku hello. Ve výchozím nastavení hello mezipaměti výplně čekání doba too3 000 milisekund. 

