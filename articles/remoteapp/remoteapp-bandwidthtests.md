---
title: "aaaAzure vzdálené aplikace RemoteApp – testování využití šířky pásma sítě s některé běžné scénáře | Microsoft Docs"
description: "Další informace o tom, jak obvyklé scénáře použití, které vám mohou pomoci zjistit potřeb šířky pásma sítě pro Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 06417c75-0c63-4ecf-b9d1-66a7af6b7b91
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 22c1dbb61d956d58d01eb4e11363266168e337e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-remoteapp---testing-your-network-bandwidth-usage-with-some-common-scenarios"></a>Azure RemoteApp – testování využití šířky pásma sítě s některé běžné scénáře
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Jak již bylo zmíněno [využití šířky pásma sítě odhad Azure RemoteApp](remoteapp-bandwidth.md), hello nejlepší způsob, jak toofigure, jaký vliv hello tooyour sítě Azure RemoteApp je toorun některé testy využití. Tyto testy pro sady času období a míru hello šířky pásma potřebné pro jednotlivé scénáře. Pokud máte možnost hello, můžete také měřit hello síťových paketů ztrátě a sítě kolísání toounderstand hello sítě vzorů, které budou vytvořeny v konkrétní prostředí.

Při vyhodnocování hello využití šířky pásma, mějte na paměti, že využití se liší mezi různé uživatele ve vaší společnosti. Například text čtení a zápis obvykle využívat menší šířku pásma než uživatelů, které pracují s video. Nejlepších výsledků dosáhnete prostudovali potřeb uživatelů a vytvořte směs hello následující scénáře, které co nejlíp vystihuje hello uživatele ve vaší společnosti. Mějte na paměti, příliš[zkontrolujte hello faktory, které mají vliv na využití šířky pásma a uživatelské prostředí](remoteapp-bandwidthexperience.md) – které vám pomohou identifikovat hello ideální testy.

Nejdřív přečíst informace o testech hello, vyberte si kompilace a pak je spustit. Můžete použít hello tabulce toohelp sledování výkonu.

> [!NOTE]
> Pokud nelze provést vlastní testování sítě nebo hello čas toodo tak nemáte, podívejte se na naše [základní síťové šířky pásma odhady/doporučení](remoteapp-bandwidthguidelines.md). Vaše vzdálenost mohou lišit, ale, tak pokud jste *můžete* vlastní testy, které byste měli.
> 
> 

## <a name="hello-usage-tests"></a>testy využití Hello
Každý z těchto testů spusťte u různých množství času a testování různých funkcí nebo funkcí, které využívají šířku pásma sítě. Mějte na paměti, toochoose hello směs test, který nejlépe odpovídá vaši uživatelé jednotlivých společnosti.

### <a name="executivecomplex-powerpoint---run-for-900-1000-seconds"></a>PowerPoint Executive nebo složité - spustit 1 900 000 sekund
Uživatel uvede mezi snímky zachováním 45 – 50 pomocí aplikace Microsoft Office PowerPoint v režimu celé obrazovky. snímky Hello by měly obsahovat bitové kopie, přechody (s animací) a pozadí s přechod barev, které jsou typické pro vaši společnost. Hello uživatel by měl aspoň 20 sekund vynaložit na každý snímek.

V tomto scénáři vytváří shlukovým přenosem dat snímku přejde toohello další snímek prezentace hello.

### <a name="simple-powerpoint---run-for-620-seconds"></a>Jednoduché PowerPoint - spustit ~ 620 sekund
Uživatel uvede jednoduchý soubor PowerPointu se přibližně 30 snímky aplikace Microsoft Office PowerPoint v režimu celé obrazovky. Hello snímky jsou vyšší text nároky než v případě Executive nebo složité PowerPoint hello a jejich jednodušší pozadí a bitové kopie (černé diagramy). 

### <a name="internet-explorer---run-for-250-seconds"></a>Internet Explorer - spustit ~ 250 sekund
Uživatel přistoupí hello web v aplikaci Internet Explorer. Umožňuje a pomocí kombinace text, obrázky přirozené a některé schémata posouvá Hello uživatele. Hello webové stránky uložené na hello místní diskové jednotce serveru hostitele relace vzdálené plochy (hostitele relace VP) hello jako. Soubor MHT. Page Up, Page Down, nahoru a dolů klíče, pomocí různých intervalů pro každý klíč nebo typ posuňte se posouvá společně Hello uživatele:

    - Dolů – 250 stisknutí kláves velmi 500 ms
    - Page Up - 36 stisknutí kláves každých 1000 ms
    - Nižší - 75 stisknutí kláves každých 100 ms
    - Page Down - 20 stisknutí kláves každých 500 ms
    - Až - 120 kláves každých 300 ms

### <a name="pdf-document---simple---run-for-610-seconds"></a>-Jednoduché - dokumentu PDF spustit ~ 610 sekund
Uživatel přečte a hledá dokumentu PDF různými způsoby pomocí Adobe Acrobat Reader. Hello dokumentu by měla obsahovat tabulky, grafy jednoduché a víc písem text. dokument Hello je 35-40 stránky dlouho. Hello uživatel posune prostřednictvím dvěma různými rychlostmi, zpětné a předává na čtyři různé zvětšení velikosti (podle toopage shody toowidth 100 % a další dle vlastního výběru). Hello přiblížení a oddálení zajistí, že v různých velikostech vykreslí textu hello (písma). Posouvání je mimo provoz hello Page Up, Page Down, nahoru a dolů klíče, pomocí různých intervalů pro každý scroll.

### <a name="pdf-document---mixed---run-for-320-seconds"></a>-Smíšený - spustit ~ 320 sekund dokumentu PDF
Uživatel přečte a hledá dokumentu PDF různými způsoby pomocí Adobe Acrobat Reader. Hello dokument se skládá z vysoce kvalitní obrázků (včetně fotografií), tabulky, jednoduché grafy a víc písem text. Hello uživatel posune prostřednictvím dvěma různými rychlostmi, zpětné a předává na čtyři různé zvětšení velikosti (podle toopage shody toowidth 100 % a další dle vlastního výběru). Hello přiblížení a oddálení zajistí, že v různých velikostech vykreslí textu hello (písma). Posouvání je mimo provoz hello Page Up, Page Down, nahoru a dolů klíče, pomocí různých intervalů pro každý scroll.

### <a name="flash-video-playback---run-for-180-seconds"></a>Flash přehrávání videa - spustit ~ 180 sekund
Uživatel zobrazení kódováním Adobe Flash video vložených na webové stránce. Hello webové stránky jsou uloženy v místní pevný disk hello serveru hostitele relace VP hello. Hello video přehraje podle modul plug-in embedded player v Internet Exploreru.

Tento scénář emuluje uživatelé bohaté obsah webových stránek obsahující multimédií. Většina hello data by měla provést prostřednictvím VOBR.

### <a name="word-remote-typing---run-for-1800-seconds"></a>Aplikace Word vzdálené zadáním – spustit ~ 1 800 sekund
Uživatel zadá dokumentu přes relaci protokolu RDP. Stisknutí kláves jsou odesílány z hello na straně klienta prostřednictvím hello RDP relace tooa dokumentu v Microsoft Wordu, spuštěné v hello vzdálené relace. Hello zadáte rychlost je jeden znak každých 250 ms (celkový počet 7050 znaků). 

Toto je jedním nejběžnější scénářů hello pro pracovní znalostní báze. Tento scénář testy hello odezvy uživatele, který zadáte do moderní pracovní procesoru. Tento scénář je citlivé tooeven malým změnám ve využití šířky pásma.

## <a name="tracking-hello-test-results"></a>Sledování výsledků testu hello
Můžete použít následující tabulky tooevaluate hello scénáře ve vašem prostředí hello. níže uvedené údaje Hello je jen pro ilustraci – může být významně liší od můžete sledovat. 

Pro jednoduchost předpokládáme, že všechny scénáře jsou testovány pomocí rozlišení obrazovky hello stejné 1920 × 1080 pixelů a zmenší TCP přenosy v síti s latencí (zpoždění) menší než 200 ms a sítě v hello 120 ms + označení asi 1 %.

O hello tabulky:

* **Průměrná prostředí** obsahuje hello šířku pásma sítě, kde není ovlivněná výrazně produktivitu uživatelů, ale není vyloučit příležitostně video nebo zvuk chyb. Hello systému, je možné toorecover rychle využívat výhod hello dynamické logiku. Hello síťové šířky pásma odhady pokus o tooguarantee hello kvality hello uživatelské prostředí.
  * **Znatelné problémy (bod rozdělení)** obsahuje hello šířku pásma sítě, kde mohou uživatelé všimnout značné problémy v jejich prostředí a jejich produktivitu ovlivní pro měřitelné časová období. V tomto okamžiku hello RDP algoritmy jsou usilující a nemůže zaručit hello uživatele kvalitní uživatelský dojem z důvodu nedostatečných šířku pásma sítě.
  * **Doporučená** obsahuje hello šířku pásma sítě, doporučujeme pro dobrý nebo vynikající prostředí. Je obvykle jeden krok vyšší než hodnota hello hello odpovídajícího **průměrná prostředí** sloupce.
  * **Poznámky k** zahrnují připomínek a poznámek.

| Test | Průměrná prostředí | Problémy s výraznou (bod rozdělení) | Doporučené šířku pásma sítě | Poznámky |
| --- | --- | --- | --- | --- |
| Executive nebo komplexní, PPT |10 MB/s |1 MB/s |> 10 MB/s, upřednostňovaný 100 MB/s |S rychlostí 1 MB/s mnoha animace jsou ztraceny |
| Jednoduché PPT |5 MB/s |256 KB/s |10 MB/s |Na 256 KB/s hello snímky zatížení s významnému zpoždění |
| Internet Explorer |10 MB/s |1 MB/s |> 10 MB/s, upřednostňovaný 100 MB/s |S rychlostí 1 MB/s webové videa jsou rozmazaně a nestabilní, rychlé posouvání má problémy |
| Jednoduché PDF |1 MB/s |256 KB/s |5 MB/s |Na 256 KB/s trvá chvíli tooload stránku hello |
| Smíšená PDF |1 MB/s |256 KB/s |5 MB/s |V 256 KB/s stránku hello udělá značné množství času tooload |
| Flash přehrávání videa |10 MB/s |1 MB/s |> 10 MB/s, upřednostňovaný 100 MB/s |S rychlostí 1 MB/s je zrnitý hello video a některé rámce jsou vyřadit. |
| Aplikace Word vzdálené zadáním |256 KB/s |128 KB/s |1 MB/s |Na 256 KB/s uživatele povšimnout hello čas mezi stisknutí kláves |

tooevaluate hello šířku pásma sítě na uživatele, vytvořit směs hello výše scénáře a hello odpovídající část požadované šířky pásma sítě. Vyberte nejvyšší číslo hello potřebné pro vaše scénáře. Vzhledem k tomu, že uživatelé používají téměř žádné samostatně hello systém, vezměte v úvahu některé rezerva pro uživatele, kteří práci současně na hello stejné síti.

## <a name="learn-more"></a>Další informace
* [Odhadnout využití šířky pásma sítě Azure RemoteApp](remoteapp-bandwidth.md)
* [Azure RemoteApp – jak šířku pásma sítě a kvalitu prostředí pracovní společně?](remoteapp-bandwidthexperience.md)
* [Azure RemoteApp šířka pásma sítě – obecné pokyny (Pokud nelze testovat vlastní)](remoteapp-bandwidthguidelines.md)

