---
title: "aaaDynamic zrychlení webu prostřednictvím Azure CDN"
description: "Podrobné informace akcelerace dynamických webů"
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
ms.date: 08/02/2017
ms.author: v-semcev
ms.openlocfilehash: 37e6312ae5e83448f2d87c95ef48c387355748bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-site-acceleration-via-azure-cdn"></a>Akcelerace dynamických webů prostřednictvím Azure CDN

S hello prudký nárůst počtu sociálních médií, elektronické obchodování a webové technologie hyper přizpůsobené hello se generuje rychle roste procento hello obsahu zpracovaných tooend uživatelů v reálném čase. Uživatelé očekávají, že rychlé, spolehlivé a přizpůsobené webových stránek, nezávisle na jejich prohlížeče, umístění, zařízení nebo sítě. Ale hello velmi inovací, které tyto činnosti, takže zapojení také zpomalit stránce stahování a ohrozit hello kvalitní uživatelský dojem hello příjemce. 

Standardní možnost CDN zahrnuje hello možnost toocache soubory blíže tooend uživatelé toospeed až doručení statických souborů. Však s dynamické webové aplikace, ukládání do mezipaměti tohoto obsahu v hraniční umístění není možné, protože hello server generuje hello obsah v odpovědi toouser chování. Urychlení hello doručování takový obsah je mnohem složitější než tradiční edge ukládání do mezipaměti a vyžaduje-komplexní řešení, která přesné vyladění každý element společně hello celého datového cestu z toodelivery zahájení. S Azure CDN dynamické lokality akcelerace (DSA), hello webových stránek s dynamickým obsahem měřitelný výkon vyšší výkon.

Azure CDN společnosti Akamai a Verizon nabízí DSA optimalizace prostřednictvím hello **optimalizované pro** nabídky během vytvoření koncového bodu.

## <a name="configuring-cdn-endpoint-tooaccelerate-delivery-of-dynamic-files"></a>Konfigurace koncového bodu tooaccelerate doručování dynamických souborů

Váš koncový bod toooptimize doručování dynamických souborů prostřednictvím portálu Azure můžete nakonfigurovat tak, že vyberete hello **dynamických webů akcelerace** možnosti v části hello **optimalizované pro** vlastnost výběr během Vytvoření koncového bodu Hello. Můžete také použít rozhraní API REST nebo žádné z hello klientské sady SDK toodo samé hello prostřednictvím kódu programu. 

### <a name="probe-path"></a>Cesta k testu
Test cesta je konkrétní tooDynamic funkce akcelerace lokality, a musí se platný pro vytvoření. DSA používá malá *testu cesta* umístit soubor do hello počátek toooptimize směrování konfigurace sítě pro hello CDN. Můžete stáhnout a nahrát naše ukázkového souboru tooyour serveru nebo použijte existující prostředek na vaše původ, který se zhruba 10 KB pro cestu testu hello místo toho pokud existuje hello asset.

> [!Note]
> DSA budou vám účtovány další poplatky. Další informace najdete v tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/cdn/) Další informace.

Hello následující snímky obrazovky ilustraci hello proces prostřednictvím portálu Azure.
 
![Přidání nového koncového bodu CDN](./media/cdn-dynamic-site-acceleration/01_Endpoint_Profile.png) 

*Obrázek 1: Přidání nového koncového bodu CDN z hello profil CDN*
 
![Vytvoření nového koncového bodu CDN pomocí agenta DSA](./media/cdn-dynamic-site-acceleration/02_Optimized_DSA.png)  

*Obrázek 2: Vytvoření koncového bodu CDN pomocí dynamických webů akcelerace optimalizace vybrané*

Po vytvoření koncového bodu CDN hello platí hello DSA optimalizace pro všechny soubory, které splňují určitá kritéria. Hello následující část popisuje optimalizace DSA podrobně.

## <a name="dsa-optimization-using-azure-cdn"></a>Optimalizace DSA pomocí Azure CDN

Dynamické akcelerace lokality na Azure CDN urychluje doručování dynamické prostředky pomocí hello následující techniky:

-   Optimalizace směrování
-   Optimalizace TCP
-   Předběžné načtení objektu (pouze Akamai)
-   Komprese mobilní bitové kopie (pouze Akamai)

### <a name="route-optimization"></a>Optimalizace směrování

Optimalizace směrování je důležité, protože hello Internetu je dynamická místo, kde provozu a dočasně výpadků se neustále mění hello síťové topologie. Hello protokol BGP (Border Gateway) je hello směrovací protokol hello Internet, ale může být rychlejší trasy prostřednictvím zprostředkující serverů bodů přítomnosti (PoP). 

Optimalizace směrování rozhodne, že hello optimální cesta toohello původu tak, aby lokalita se nepřetržitě dostupné a dynamický obsah doručuje tooend uživatele prostřednictvím hello nejrychlejší a možná nejspolehlivější trasy. 

Hello Akamai sítě používá data v reálném čase toocollect technik a porovnání různé cesty prostřednictvím různých uzlů v hello Akamai serveru, jakož i trasa protokolu BGP výchozí hello napříč hello otevřete Internet toodetermine hello nejrychlejší trasy mezi hello původ a hello CDN okraj. Tyto postupy vyhnout internetové body zahlcení a dlouho trasy. 

Podobně hello Verizon sítě používá kombinaci DNS všesměrového vysílání, vysoké kapacity podporují bodů POP a kontroly stavu toodetermine hello nejlepší brány toobest data trasy z hello klienta toohello původu.
 
V důsledku toho plně dynamické a transakční obsah doručuje rychleji a spolehlivěji tooend uživatele, i když se uncacheable. 

### <a name="tcp-optimizations"></a>Optimalizace TCP

Protokol TCP (Transmission Control) je standard hello sady Internet protocol hello používá toodeliver informace mezi aplikacemi v síti IP.  Ve výchozím nastavení existuje několik zpět a stanovilo požadavky požadované tooset až připojení TCP, jakož i síť congestions tooavoid omezení, které mít za následek umožňuje zvýšit efektivitu ve velkém měřítku. Azure CDN společnosti Akamai přistupuje k tomuto problému pomocí optimalizace do tří oblastí: 

 - Odstranění pomalé spuštění
 - Využívání trvalé připojení
 - ladění parametry paketu protokolu TCP (pouze Akamai)

#### <a name="eliminating-slow-start"></a>Odstranění pomalé spuštění

*Zpomalit spuštění* je součástí hello protokolu TCP, která zabraňuje zahlcení sítě omezením hello množství dat posílaných prostřednictvím sítě hello. Začíná velikosti okna malé zahlcení mezi odesílatele a příjemce až do dosažení maximální hello nebo je zjištěna ztráta paketů.

Azure CDN společnosti Akamai a Verizon eliminuje pomalé začne v tři kroky:

1.  Akamai i společnosti Verizon síť používat stavu a šířku pásma monitorování toomeasure hello šířky pásma připojení mezi servery edge PoP.
2. metriky Hello jsou sdílené mezi servery PoP edge tak, aby každý server si je vědoma hello síťových podmínkách a stavu serveru hello dalších bodů POP je obcházet.  
3. servery edge Hello CDN jsou teď dokáže toomake předpoklady o některé parametry přenosu, například jaké hello optimální velikost okna musí být při komunikaci s ostatními servery edge CDN v jeho blízkosti. Tento krok znamená, že velikost okna počáteční zahlcení hello může být zvýšena Pokud hello stavu hello připojení mezi servery edge CDN hello umožňuje přenosech souborů vyšší paketů.  

#### <a name="leveraging-persistent-connections"></a>Využívání trvalé připojení

Pomocí název CDN, menší počet jedinečných počítačů připojit přímo ve srovnání s uživateli připojení přímo tooyour původu tooyour zdrojový server. Azure CDN společnosti Akamai a Verizon také fondu společně tooestablish požadavků uživatele méně připojení s hello původu.

Jak už bylo zmíněno dříve, připojení TCP několik požadavků a zpět provádět v handshake tooestablish nové připojení. Trvalé připojení, také známé jako "HTTP Keep-Alive," u HTTP požadavků toosave odezvy několikrát znovu použít existující připojení TCP a urychlit doručení. 

pravidelné udržovací pakety Hello Verizon sítě rovněž odesílá přes tooprevent připojení TCP hello otevřené připojení z dochází k uzavření.

#### <a name="tuning-tcp-packet-parameters"></a>Ladění parametry paketu protokolu TCP

Azure CDN společnosti Akamai také vyladí hello parametry, které řídí připojení k serveru a snižuje hello množství dlouhé vzdálenosti zaokrouhlí služebních cest požadované tooretrieve obsah vložený v lokalitě hello pomocí hello následující techniky:

1.  Zvýšit hello počáteční zahlcení okna tak, aby pakety další můžete odeslat bez čekání na potvrzení.
2.  Časový limit opakování počáteční přenosu hello snížení tak, že se detekuje ztrátu a opakování přenosu dojde rychleji.
3.  Klesá hello minimální a maximální znovu pošlou časový limit tooreduce hello doba čekání před za předpokladu, že byly ztrátě paketů při přenosu.

### <a name="object-prefetch-akamai-only"></a>Předběžné načtení objektu (pouze Akamai)

Většina webů obsahovat stránku HTML, který odkazuje na různé prostředky, jako obrázků a skriptů. Obvykle když klient požádá o webovou stránku, hello prohlížeče nejprve stáhne a analyzuje hello HTML objekt a potom provede další požadavky, že toolinked prostředky, které jsou požadované toofully načíst stránku hello. 

*Předběžné načtení* je technika tooretrieve bitové kopie a skripty vložené do hello stránku HTML hello HTML obsluhovaného toohello prohlížeče, a před hello prohlížeče i umožňuje tyto požadavky objektu. 

S hello **předběžné načtení** možnost zapnutá v době hello při hello CDN slouží hello HTML základní stránky toohello prohlížeče klienta, hello CDN analyzuje soubor hello HTML a další požadavky pro všechny propojené prostředky a ukládá je do své mezipaměti. Pokud hello klient provede hello požadavky pro hello propojené prostředky, hello CDN hraniční server již má hello vyžádané objekty a může je sloužit bez toohello původ odezvy. Tato optimalizace výhody lze uložit do mezipaměti a neurčené obsahu.

### <a name="adaptive-image-compression-akamai-only"></a>Komprese Adaptivní bitové kopie (pouze Akamai)

Některá zařízení, zejména mobilní ty prostředí nižší rychlost sítě tootime čas. V těchto scénářích platí, je více vhodné hello uživatele tooreceive menší obrázků ve své webové stránky rychleji místo čekání na dlouhou dobu pro úplné řešení bitové kopie.

Tato funkce automaticky monitoruje sítě kvality a využívá standardní metody komprese JPEG při rychlost sítě jsou pomalejší tooimprove čas doručení.

Komprese Adaptivní bitové kopie | Přípony souborů  
--- | ---  
Komprese JPEG | JPG, JPEG, JPE, .jig, .jgig, .jgi

## <a name="caching"></a>Ukládání do mezipaměti

S DSA ukládání do mezipaměti je vypnuto na hello CDN, ve výchozím nastavení i v případě, že hello počátek zahrnuje mezipaměti – ovládací prvek nebo vyprší hlavičky v odpovědi hello. Toto výchozí nastavení je vypnutá, protože DSA se obvykle používá pro dynamické prostředky, které by neměly uložené v mezipaměti, protože jsou jedinečné tooeach klienta a zapnutí ukládání do mezipaměti ve výchozím nastavení může dojít k narušení toto chování.

Pokud máte web se smíšenými statické a dynamické prostředky, je nejlepší tootake hybridní přístup tooget hello nejlepší výkon. 

Pokud používáte ADN s Verizon Premium, chcete-li ukládání do mezipaměti zpět na pro určité případy použití hello stroj pravidel.  

Alternativou je toouse dva koncové body CDN. S prostředky dynamické toodeliver DSA a jiný koncový bod se statickou optimalizace typ, jako je například obecné webové přenosy toodelivery Uložitelný prostředky. V pořadí tooaccomplish tento alternativní upravíte toolink vaše webová stránka adresy URL přímo toohello asset na hello plánování toouse koncový bod CDN. 

Příklad: `mydynamic.azureedge.net/index.html` dynamické stránky a je načíst z koncového bodu DSA hello.  Hello stránku html odkazuje na více statických prostředků, jako je například knihoven jazyka JavaScript nebo bitové kopie, které jsou načteny z hello statické koncový bod CDN, jako například `mystatic.azureedge.net/banner.jpg` a `mystatic.azureedge.net/scripts.js`. 

Příklad můžete nalézt [sem](https://docs.microsoft.com/azure/cdn/cdn-cloud-service-with-cdn#controller) na tom, jak toouse řadičů v technologie ASP.NET webový obsah tooserve aplikace prostřednictvím konkrétní adresy URL CDN.




