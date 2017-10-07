---
title: "aaaOptimize doručování obsahu Azure pro váš scénář"
description: "Jak toooptimize doručování obsahu pro konkrétní scénáře"
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
ms.openlocfilehash: 922a92fdbf7e6e21f2b5ae9a2fb4ac32735fc15a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-azure-content-delivery-for-your-scenario"></a>Optimalizace doručování obsahu Azure pro váš scénář

Při předvádění obsahu tooa velkou globální cílovou skupinu, je důležité tooensure hello optimalizované doručování obsahu. Hello sítě pro doručování obsahu Azure můžete optimalizovat hello doručení prostředí založené na hello typ obsahu, které máte. Obsah může být web, živý datový proud, video nebo velký soubor ke stažení. Když vytvoříte koncový bod doručování obsahu (CDN) sítě, zadáte scénáři hello **optimalizované pro** možnost. Vaše volba určuje, které optimalizace je obsahu použité toohello doručit od koncový bod CDN hello.

Možnosti optimalizace jsou navrženou toouse osvědčené postupy chování tooimprove doručování obsahu výkonu a snižování zátěže lepší původu. Vaše volby scénář ovlivnit výkon změnou konfigurace pro částečné ukládání do mezipaměti, objekt rozdělování a zásady opakování selhání hello původu. 

Tento článek obsahuje přehled různých funkcí optimalizace a kdy byste měli použít. Další informace o funkcích a omezení naleznete příslušné článcích hello na každý typ jednotlivých optimalizace.

> [!NOTE]
> Vaše **optimalizované pro** možnosti může lišit v závislosti na zprostředkovateli hello vyberete. Vylepšení zprostředkovatele CDN použít různými způsoby v závislosti na scénáři hello. 

## <a name="provider-options"></a>Možnosti zprostředkovatele

Hello Azure Content Delivery Network společnosti Akamai podporuje:

* Obecné webové doručení 

* Obecné streamování médií

* Streamování videa na přání médií

* Stažení velkých souborů

* Akcelerace dynamických webů 

Hello Azure Content Delivery Network od společnosti Verizon podporuje pouze obecné webové doručení. Slouží pro video na vyžádání a stahování velkých souborů. Nemáte tooselect typ optimalizace.

Důrazně doporučujeme, abyste otestovali výkonu rozdíly mezi různé zprostředkovatele tooselect hello optimální zprostředkovatele pro vaše doručení.

## <a name="select-and-configure-optimization-types"></a>Vyberte a nakonfigurujte optimalizace typy

toocreate nový koncový bod, vyberte typ optimalizace, který nejlépe odpovídá hello scénář a typ obsahu, který chcete hello toodeliver koncový bod. **Obecné webové doručení** je hello výchozí výběr. Můžete aktualizovat hello možnost optimalizace pro jakékoli existující koncový bod Akamai kdykoli. Tato změna nemá přerušit doručování z hello CDN. 

1. Vyberte koncový bod v rámci profilu Standard Akamai.

    ![Výběr koncového bodu ](./media/cdn-optimization-overview/01_Akamai.png)

2. V části **nastavení**, vyberte **optimalizace**. Pak vyberte typ ze hello **optimalizované pro** rozevíracího seznamu.

    ![Optimalizace a typ výběru](./media/cdn-optimization-overview/02_Select.png)

## <a name="optimization-for-specific-scenarios"></a>Optimalizace pro konkrétní scénáře

Můžete optimalizovat hello koncový bod CDN pro jednu z následujících scénářů hello. 

### <a name="general-web-delivery"></a>Obecné webové doručení

Obecné webové doručení je nejběžnější možnosti optimalizace hello. Je určený pro optimalizaci obecné webového obsahu, například webových stránek a webových aplikací. Tato optimalizace můžete použít také pro soubor, a stáhne video.

Typické webu obsahuje obsah statické a dynamické. Statický obsah zahrnuje obrázků, knihovny jazyka JavaScript a stylů, které lze do mezipaměti a doručit toodifferent uživatele. Dynamický obsah je přizpůsobené pro jednotlivé uživatele, například položky zpráv, které jsou přizpůsobené tooa uživatelský profil. Dynamický obsah neukládá do mezipaměti, protože je jedinečný tooeach uživatele, například nákupního košíku obsah. Obecné webové doručení můžete optimalizovat celý web. 

> [!NOTE]
> Pokud používáte hello Azure Content Delivery Network společnosti Akamai, můžete chtít toouse optimalizace Pokud vaše Průměrná velikost je menší než 10 MB. Pokud vaše Průměrná velikost souboru je větší než 10 MB, vyberte **stahování velkých souborů** z hello **optimalizované pro** rozevíracího seznamu.

### <a name="general-media-streaming"></a>Obecné streamování médií

Pokud potřebujete toouse hello koncový bod pro živé streamování a streamování videa na přání, doporučujeme, abyste streamování optimalizace obecné médií.

Datové proudy Media je čas citlivé, protože paketů, které přicházejí pozdní hello klienta může způsobit sníženou zobrazení prostředí, jako je například časté ukládání do vyrovnávací paměti obsahu videa. Optimalizace streamování médií snižuje latence hello doručování obsahu pro média a poskytuje technologie smooth streaming prostředí pro uživatele. 

Tento scénář je běžný u Zákazníci využívající službu Azure media. Když používáte službu Azure media services, zobrazí jeden streamování koncový bod, který lze použít pro datové proudy živě i na vyžádání. Tento scénář Zákazníci nepotřebují tooswitch tooanother endpoint když se změní z datových proudů za provozu tooon na vyžádání. Obecné média streamování optimalizace podporuje tento typ scénáře.

Hello sítě pro doručování obsahu Azure od společnosti Verizon používá hello obecné webové doručení optimalizace typ toodeliver streamování mediální obsah.

toolearn Další informace o optimalizaci streamování médií najdete v části [optimalizace streamování médií](cdn-media-streaming-optimization.md).

### <a name="video-on-demand-media-streaming"></a>Streamování videa na přání médií

Optimalizace streamování na vyžádání Video-on-Demand média zlepšuje streamování obsahu na vyžádání video-on-demand. Pokud používáte koncový bod pro vyžádání video-on-demand streamování, můžete chtít toouse tuto možnost.

Hello sítě pro doručování obsahu Azure od společnosti Verizon používá hello obecné webové doručení optimalizace typ toodeliver streamování mediální obsah.

toolearn Další informace o optimalizaci streamování médií najdete v části [optimalizace streamování médií](cdn-media-streaming-optimization.md).

> [!NOTE]
> Pokud koncový bod hello slouží především obsahu na vyžádání video-on-demand, použijte tento typ optimalizace. Hello hlavní rozdíl mezi tato optimalizace a optimalizace streamování hello obecné média je časový limit opakování hello připojení. časový limit Hello je mnohem kratší toowork s živé streamování scénáře.

### <a name="large-file-download"></a>Stažení velkých souborů

Pokud používáte hello Azure Content Delivery Network společnosti Akamai, musíte použít velkých souborů stažení toodeliver soubory větší než 1,8 GB. Hello Azure Content Delivery Network od společnosti Verizon nemá omezení na soubor stáhnout velikost v jeho doručení optimalizaci obecné webů.

Pokud používáte hello sítě pro doručování obsahu Azure společnosti Akamai, stahování velkých souborů jsou optimalizované pro obsah větší než 10 MB. Pokud vaše Průměrná velikost souboru je menší než 10 MB, můžete chtít toouse obecné webové doručení. Pokud vaše soubory průměrné velikosti jsou konzistentně větší než 10 MB, může to být efektivnější toocreate samostatné koncový bod pro velkých souborů. Aktualizace firmwaru a softwaru jsou například obvykle velkých souborů.

Hello sítě pro doručování obsahu Azure od společnosti Verizon používá hello obecné webové doručení optimalizace typ toodeliver streamování mediální obsah.

toolearn Další informace o optimalizaci velkých souborů, najdete v části [velkých souborů optimalizace](cdn-large-file-optimization.md).

### <a name="dynamic-site-acceleration"></a>Akcelerace dynamických webů

 Akcelerace dynamických webů je k dispozici z Akamai a Verizon Content Delivery Network profily. Tato optimalizace zahrnuje toouse dalších poplatků. Další informace najdete v tématu hello stránce s cenami.

Akcelerace dynamických webů zahrnuje různé techniky využívající hello latenci a výkonu dynamického obsahu. Mezi dostupné techniky patří trasy a síťové optimalizace, optimalizace TCP a další. 

Můžete použít tento tooaccelerate Optimalizace webové aplikace, která zahrnuje mnoho odpovědí, které nejsou lze uložit do mezipaměti. Příklady jsou výsledky hledání, najdete v článku věnovaném transakce nebo dat v reálném čase. Můžete pokračovat v ukládání do mezipaměti možnosti CDN toouse jádra pro statické data. 



