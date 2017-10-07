---
title: "Optimalizace stažení souboru aaaLarge prostřednictvím hello Azure Content Delivery Network"
description: "Optimalizace vysvětlené podrobněji stahování velkých souborů"
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
ms.openlocfilehash: 2646979bfb38e997037bcff5b1cdda34e22c394a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="large-file-download-optimization-via-hello-azure-content-delivery-network"></a>Optimalizace stahování velkých souborů prostřednictvím hello Azure Content Delivery Network

Velikosti souborů obsahu doručit přes hello Internet pokračovat toogrow kvůli tooenhanced funkce, vylepšené grafika a bohatý mediální obsah. Tento nárůst vycházejí z hlavních faktorů: širokopásmové průnikům, větší zařízení nenákladné úložiště, zvýšení rozšířeným vysokým rozlišením video a připojené k Internetu zařízení (IoT). Doručení rychlé a efektivní mechanismus pro velkých souborů je důležité tooensure prostředí hladký a zábavná příjemce.

Doručování velkých souborů má několik problémy. Hello Průměrná doba toodownload velký soubor nejdřív může být důležité, protože aplikace nemusí stahovat všechna data sekvenčně. V některých případech může aplikace stahovat hello poslední část souboru před první část hello. Pokud se požaduje jenom malé množství soubor nebo nastavení stahování, hello stahování může selhat. stažení Hello také může dojít ke zpoždění až po hello sítě pro doručování obsahu (CDN) načte hello celý soubor z hello zdrojový server. 

Druhý hello latenci mezi počítačem uživatele a soubor hello určuje rychlost hello, kdy se může zobrazit obsah. Kromě toho problémy zahlcení a kapacitu sítě také ovlivnit propustnost. Větší vzdálenosti mezi servery a uživatelé vytvoření dalších možností pro toooccur ke ztrátě paketů, což snižuje kvality. Hello snížení kvality způsobené omezená propustnost a ztráta paketů vyšší může zvýšit hello čekací dobu toofinish stažení souboru. 

Třetí mnoho velkých souborů nejsou doručeny jako celek. Uživatelé mohou zrušit uprostřed stahování nebo si pusťte hello pouze první několik minut dlouhé video MP4. Software a média doručení společností proto vhodné toodeliver pouze hello část souboru, který je požadovaný. Efektivní distribuci hello požadované části snižuje hello odchozí provoz z hello zdrojový server. Efektivní distribuční zmenšuje hello paměti a vstupně-výstupních operací tlak na původním serveru hello. 

Hello Azure Content Delivery Network společnosti Akamai teď nabízí funkce, která poskytuje velkých souborů efektivně toousers napříč hello zeměkouli ve velkém měřítku. Funkce Hello snižuje latenci, protože snižuje zatížení hello hello počátek serverů. Tato funkce je k dispozici hello Standard Akamai cenovou úroveň.

## <a name="configure-a-cdn-endpoint-toooptimize-delivery-of-large-files"></a>Konfigurace koncový bod toooptimize doručování velkých souborů

Můžete nakonfigurovat váš koncový bod toooptimize doručování velkých souborů prostřednictvím hello portálu Azure. Můžete také použít rozhraní API REST nebo žádné z hello klientské sady SDK toodo to. Hello následující kroky ukazují hello proces prostřednictvím hello portálu Azure.

1. tooadd nový koncový bod na hello **profil CDN** vyberte **koncový bod**.

    ![Nový koncový bod](./media/cdn-large-file-optimization/01_Adding.png)  
 
2. V hello **optimalizované pro** rozevíracího seznamu vyberte **stahování velkých souborů**.

    ![Vybrané optimalizace velkých souborů](./media/cdn-large-file-optimization/02_Creating.png)


Po vytvoření koncového bodu CDN hello, bude se vztahovat hello velkých souborů optimalizace pro všechny soubory, které splňují určitá kritéria. Hello následující část popisuje tento proces.

## <a name="optimize-for-delivery-of-large-files-with-hello-azure-content-delivery-network-from-akamai"></a>Optimalizace pro doručování velkých souborů s hello Azure Content Delivery Network společnosti Akamai

Funkce typ optimalizace velkých souborů Hello zapne síťové optimalizace a konfigurace toodeliver velké soubory rychlejší a více responsively. Obecné webové přenosy s Akamai ukládá do mezipaměti pouze následující 1,8 GB soubory a tunelového propojení (ne mezipaměť) můžete soubory až too150 GB. Optimalizace velkých souborů ukládá do mezipaměti souborů až too150 GB.

Optimalizace velkých souborů je účinný, pokud jsou splněny určité podmínky. Podmínky zahrnují jak funguje hello zdrojový server, hello velikosti a typy hello souborů, které jsou požadovány. Předtím, než se nám získat do podrobnosti o těchto tématech, byste měli porozumět, jak funguje hello optimalizace. 

### <a name="object-chunking"></a>Objekt rozdělování 

Hello Azure Content Delivery Network společnosti Akamai využívá techniku, názvem rozdělování objektu. Pokud se požaduje velký soubor, hello CDN načte menší části souboru hello z počátku hello. Po hello CDN POP hraniční server přijme požadavek souboru plná nebo rozsah bajtů, zkontroluje, zda je typ souboru hello nepodporuje pro tyto optimalizace. Také zkontroluje, zda typ souboru hello splňuje požadavky na velikost souboru hello. Pokud velikost souboru hello je větší než 10 MB, hello CDN edge server požádá o soubor hello z počátku hello v bloky 2 MB. 

Po hello bloku dorazí na hello CDN edge, nemá v mezipaměti a okamžitě obsluhovat toohello uživatele. Hello CDN pak prefetches hello další blok paralelně. Toto předběžné načtení zajistí, že hello obsah zůstane jeden blok před hello uživatele, což snižuje latence. Tento proces pokračuje až hello celý stažení souboru (je-li požadována), všechny rozsahů bajtů jsou k dispozici (je-li požadována), nebo ukončí hello klient hello připojení. 

Další informace o žádosti o rozsah bajtů hello najdete v tématu [RFC 7233](https://tools.ietf.org/html/rfc7233).

Hello CDN ukládá do mezipaměti všechny bloky dat po přijetí. Hello celý soubor nemá toobe uložené v mezipaměti CDN hello mezipaměti. Odeslání dalších žádostí o hello souboru nebo bajtů rozsahů je zobrazovaná hello CDN mezipaměti. Pokud nejsou všechny bloky hello ukládají do mezipaměti na hello CDN, je předběžné načtení bloků dat použité toorequest z počátku hello. Tato optimalizace spoléhá na možnost hello hello původní server toosupport rozsah bajtů požadavků. _Pokud hello zdrojový server nepodporuje požadavky rozsah bajtů, optimalizace není platná._ 

### <a name="caching"></a>Ukládání do mezipaměti
Optimalizace velkých souborů používá jiný výchozí doba ukládání do mezipaměti vypršení platnosti z obecné webové doručení. Rozlišuje ukládání do mezipaměti kladné a záporné ukládání do mezipaměti na základě kódů odpovědi HTTP. Pokud zdrojový server hello určuje čas vypršení platnosti prostřednictvím ovládacího prvku mezipaměti nebo vyprší platnost hlavičky v odpovědi hello, hello CDN ctí tuto hodnotu. Když neurčuje hello původ a hello soubor odpovídá hello typ a velikost podmínky pro tento typ optimalizace, využívá hello CDN hello výchozí hodnoty pro optimalizaci velkých souborů. Hello CDN, jinak používá výchozí hodnoty pro obecné webové doručení.


|    | Obecné web | Optimalizace velkých souborů 
--- | --- | --- 
Ukládání do mezipaměti: kladné <br> HTTP 200, 203, 300, <br> 301, 302 a 410 | 7 dní |1 den  
Ukládání do mezipaměti: záporná <br> HTTP 204, 305, 404, <br> a 405 | Žádný | 1 sekunda 

### <a name="deal-with-origin-failure"></a>Řešení s chybou počátek

Délka časového limitu pro čtení počátek Hello zvyšuje ze dvou sekund obecné webové doručení tootwo minut pro typ optimalizace hello velkých souborů. Toto zvýšení účty pro hello větší soubor velikosti tooavoid předčasné vypršení časového limitu připojení.

Pokud vyprší časový limit připojení, hello CDN opakuje několikrát, než odešle klient toohello chyba "504 – časový limit brány". 

### <a name="conditions-for-large-file-optimization"></a>Podmínky pro optimalizaci velkých souborů

Hello následující tabulka uvádí hello sadu kritérií toobe splněna pro optimalizaci velkých souborů:

Podmínka | Hodnoty 
--- | --- 
Podporované typy souborů | 3g, 2, 3gp, amp, avi, bz2, dmg, exe, f4v, flv, <br> GZ, hdp, iso, jxr, m4v, mkv, mov, mp4, <br> MPEG, mpg, serveru mts, pkg, RT, rm, swf, vkládání, <br> TGZ, wdp, WBEM, webp, wma, wmv, zip  
Minimální velikost souboru | 10 MB. 
Maximální velikost souboru | 150 GB 
Vlastnosti serveru počátek | Žádosti o rozsah bajtů musí podporovat. 

## <a name="optimize-for-delivery-of-large-files-with-hello-azure-content-delivery-network-from-verizon"></a>Optimalizace pro doručování velkých souborů s hello Azure Content Delivery Network od společnosti Verizon

Hello Azure Content Delivery Network od společnosti Verizon nabízí velké soubory bez cap na velikost souboru. Další funkce jsou zapnuté ve výchozí toomake doručování velkých souborů rychlejší.

### <a name="complete-cache-fill"></a>Dokončení mezipaměti výplně

Hello výchozí dokončení mezipaměti výplně funkce umožňuje hello CDN toopull soubor do mezipaměti hello při opuštění nebo ke ztrátě počáteční požadavku. 

Výplně dokončení mezipaměti je nejvhodnější pro velké prostředky. Obvykle uživatele nemáte je stáhnout od start toofinish. Používají progresivní stahování. výchozí chování Hello vynutí hello hraniční server tooinitiate načítání na pozadí hello majetku z hello zdrojový server. Potom hello asset je v místní mezipaměti hello hraničního serveru. Po hello úplné objektu v mezipaměti hello hello hraniční server plnit požadavky toohello rozsah bajtů CDN pro objekt uložený v mezipaměti hello.

Hello výchozí chování lze zakázat pomocí hello stroj pravidel v hello úroveň Verizon Premium.

### <a name="peer-cache-fill-hot-filing"></a>Sdílená mezipaměť vyplnění horkou podání

Hello výchozí sdílené mezipaměti výplně horkou podání funkce používá sofistikované vlastního algoritmu. Používá další hraniční ukládání do mezipaměti servery založené na šířku pásma a agregace požadavky na požadavky klienta toofulfill metriky pro velkých, vysoce oblíbených objekty. Tato funkce zabraňuje situaci, v níž jsou odesílány velký počet požadavků navíc tooa uživatele zdrojový server. 

### <a name="conditions-for-large-file-optimization"></a>Podmínky pro optimalizaci velkých souborů

ve výchozím nastavení jsou zapnuté funkce optimalizace Hello Verizon. Neexistují žádná omezení na maximální velikost souboru. 

## <a name="additional-considerations"></a>Další aspekty

Vezměte v úvahu následující další aspekty pro tento typ optimalizace hello.
 
### <a name="azure-content-delivery-network-from-akamai"></a>Síť pro doručování obsahu Azure společnosti Akamai

- Hello rozdělování proces generuje dalších žádostí toohello zdrojový server. Hello celkový objem dat doručit od počátku hello je však mnohem menší. Bloku dat za následek lepší ukládání do mezipaměti charakteristiky v hello CDN.

- Paměť a vstupně-výstupních operací zatížení jsou omezeny na počátku hello vzhledem k tomu, že jsou doručovány na menší části souboru hello.

- U bloky uloží do mezipaměti na hello CDN neexistují žádné další požadavky toohello původu až vyprší platnost obsahu hello nebo je vyřazena z mezipaměti hello.

- Mohou uživatelé provádět rozsah požadavky toohello CDN a jste zacházet jako jakýkoli normální soubor. Optimalizace platí jenom v případě, že je platný typ souboru a rozsah bajtů hello je mezi 10 MB a 150 GB. Pokud požadovaná hello souboru Průměrná velikost je menší než 10 MB, můžete chtít toouse obecné webové doručení místo.

### <a name="azure-content-delivery-network-from-verizon"></a>Doručování obsahu Azure sítě od společnosti Verizon

typ optimalizace doručení obecné webové Hello doručovat velkých souborů.
