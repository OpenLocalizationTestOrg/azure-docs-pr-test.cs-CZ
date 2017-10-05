---
title: "Azure RemoteApp – testování využití šířky pásma sítě s některé běžné scénáře | Microsoft Docs"
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
ms.openlocfilehash: 8ad172a06e34cd0647079d787097cb2696cf116e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-remoteapp---testing-your-network-bandwidth-usage-with-some-common-scenarios"></a>Azure RemoteApp – testování využití šířky pásma sítě s některé běžné scénáře
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Jak již bylo zmíněno [využití šířky pásma sítě odhad Azure RemoteApp](remoteapp-bandwidth.md), nejlepší způsob, jak zjistit, co je dopad Azure Remoteappu k vaší síti některé testy využití. Tyto testy pro nastavené časové období a měřit šířku pásma potřebné pro jednotlivé scénáře. Pokud máte možnost, budete také vyhodnocovat síťových paketů ztrátě a sítě kolísání pochopit vzory sítě, které budou vytvořeny v konkrétní prostředí.

Při vyhodnocování využití šířky pásma, mějte na paměti, že využití se liší mezi různé uživatele ve vaší společnosti. Například text čtení a zápis obvykle využívat menší šířku pásma než uživatelů, které pracují s video. Nejlepších výsledků dosáhnete prostudovali potřeb uživatelů a vytvořte směs následující scénáře nejlépe odpovídající uživatelé ve vaší společnosti. Nezapomeňte [zkontrolujte faktorů, které mít vliv na využití šířky pásma a uživatelské prostředí](remoteapp-bandwidthexperience.md) – které vám pomohou identifikovat ideální testy.

Nejdřív přečíst o testy, vyberte si kompilace a spusťte je. Následující tabulka slouží k usnadnění sledování výkonu.

> [!NOTE]
> Pokud nelze provést vlastní testování sítě, nebo nemáte čas potřebný k tomu, podívejte se na naše [základní síťové šířky pásma odhady/doporučení](remoteapp-bandwidthguidelines.md). Vaše vzdálenost mohou lišit, ale, tak pokud jste *můžete* vlastní testy, které byste měli.
> 
> 

## <a name="the-usage-tests"></a>Testy využití
Každý z těchto testů spusťte u různých množství času a testování různých funkcí nebo funkcí, které využívají šířku pásma sítě. Nezapomeňte vybrat směs test, který nejlépe odpovídá vaši uživatelé jednotlivých společnosti.

### <a name="executivecomplex-powerpoint---run-for-900-1000-seconds"></a>PowerPoint Executive nebo složité - spustit 1 900 000 sekund
Uživatel uvede mezi snímky zachováním 45 – 50 pomocí aplikace Microsoft Office PowerPoint v režimu celé obrazovky. Snímky by měly obsahovat bitové kopie, přechody (s animací) a pozadí s přechod barev, které jsou typické pro vaši společnost. Uživatel by měl aspoň 20 sekund vynaložit na každý snímek.

V tomto scénáři vytváří shlukovým přenosem dat snímku přejde na další snímek v prezentaci.

### <a name="simple-powerpoint---run-for-620-seconds"></a>Jednoduché PowerPoint - spustit ~ 620 sekund
Uživatel uvede jednoduchý soubor PowerPointu se přibližně 30 snímky aplikace Microsoft Office PowerPoint v režimu celé obrazovky. Snímky jsou vyšší text nároky než ve scénáři PowerPoint Executive nebo složité a jejich jednodušší pozadí a bitové kopie (černé diagramy). 

### <a name="internet-explorer---run-for-250-seconds"></a>Internet Explorer - spustit ~ 250 sekund
Uživatel přejde web v aplikaci Internet Explorer. Uživatel přejde a pomocí kombinace text, obrázky přirozené a některé schémata posouvá. Webové stránky uložené na místní diskové jednotce serveru hostitele relace vzdálené plochy (hostitele relace VP) jako. Soubor MHT. Uživatel posune stránku nahoru, Page Down, nahoru a dolů klíče, pomocí různých intervalů pro každý klíč nebo typ scroll:

    - Dolů – 250 stisknutí kláves velmi 500 ms
    - Page Up - 36 stisknutí kláves každých 1000 ms
    - Nižší - 75 stisknutí kláves každých 100 ms
    - Page Down - 20 stisknutí kláves každých 500 ms
    - Až - 120 kláves každých 300 ms

### <a name="pdf-document---simple---run-for-610-seconds"></a>-Jednoduché - dokumentu PDF spustit ~ 610 sekund
Uživatel přečte a hledá dokumentu PDF různými způsoby pomocí Adobe Acrobat Reader. Dokument by měla obsahovat tabulky, grafy jednoduché a víc písem text. Dokument je 35-40 stránky dlouho. Uživatel posune prostřednictvím dvěma různými rychlostmi, zpětné a předává na čtyři různé zvětšení velikosti (na celou stránku, na celou šířku, 100 % a další dle vlastního výběru). Přibližování zajistí, že text (písma) vykreslí v různých velikostech. Posouvání je mimo provoz Page Up, Page Down, nahoru a dolů klíče, pomocí různých intervalů pro každý scroll.

### <a name="pdf-document---mixed---run-for-320-seconds"></a>-Smíšený - spustit ~ 320 sekund dokumentu PDF
Uživatel přečte a hledá dokumentu PDF různými způsoby pomocí Adobe Acrobat Reader. Dokument se skládá z vysoce kvalitní obrázků (včetně fotografií), tabulky, jednoduché grafy a víc písem text. Uživatel posune prostřednictvím dvěma různými rychlostmi, zpětné a předává na čtyři různé zvětšení velikosti (na celou stránku, na celou šířku, 100 % a další dle vlastního výběru). Přibližování zajistí, že text (písma) vykreslí v různých velikostech. Posouvání je mimo provoz Page Up, Page Down, nahoru a dolů klíče, pomocí různých intervalů pro každý scroll.

### <a name="flash-video-playback---run-for-180-seconds"></a>Flash přehrávání videa - spustit ~ 180 sekund
Uživatel zobrazení kódováním Adobe Flash video vložených na webové stránce. Webové stránky jsou uloženy v místním pevném disku serveru hostitele relace VP. Video přehraje podle modul plug-in embedded player v Internet Exploreru.

Tento scénář emuluje uživatelé bohaté obsah webových stránek obsahující multimédií. Většina dat by měla provést prostřednictvím VOBR.

### <a name="word-remote-typing---run-for-1800-seconds"></a>Aplikace Word vzdálené zadáním – spustit ~ 1 800 sekund
Uživatel zadá dokumentu přes relaci protokolu RDP. Stisknutí kláves jsou odesílány ze strany klienta prostřednictvím protokolu RDP relace dokumentu v Microsoft Wordu spuštěné v relaci vzdálené plochy. Zadáním míra je jeden znak každých 250 ms (celkový počet 7050 znaků). 

Toto je jedním nejběžnější scénářů pro pracovní znalostní báze. Tento scénář testy odezvy uživatele, který zadáte do moderní pracovní procesoru. Tento scénář je citlivá na i malým změnám ve využití šířky pásma.

## <a name="tracking-the-test-results"></a>Sledování výsledků testu
V následující tabulce slouží k vyhodnocení scénáře ve vašem prostředí. Níže uvedené údaje platí jenom pro obrázek – může být významně liší od můžete sledovat. 

Pro jednoduchost předpokládáme, že ve všech scénářích je testována pomocí stejné rozlišení 1920 × 1080 pixelů a zmenší TCP přenosy v síti s latencí (zpoždění) menší než 200 ms a sítě v označit 120 ms + asi 1 %.

O tabulce:

* **Průměrná prostředí** obsahuje šířky pásma sítě, kde není ovlivněná výrazně produktivitu uživatelů, ale není vyloučit příležitostně video nebo zvuk chyb. Systém je možné obnovit rychle a využívají k dynamické logiky. Šířky pásma sítě odhadne pokus zaručit kvalitu činnost koncového uživatele.
  * **Znatelné problémy (bod rozdělení)** obsahuje šířky pásma sítě, kde mohou uživatelé všimnout značné problémy v jejich prostředí a jejich produktivitu ovlivní pro měřitelné časová období. V tomto okamžiku RDP algoritmy jsou usilující a nemůže zaručit kvalitní uživatelský dojem uživatele z důvodu nedostatečných šířku pásma sítě.
  * **Doporučená** obsahuje doporučené pro dobrý nebo vynikající šířku pásma sítě. Je obvykle jeden krok vyšší než hodnota v příslušném **průměrná prostředí** sloupce.
  * **Poznámky k** zahrnují připomínek a poznámek.

| Test | Průměrná prostředí | Problémy s výraznou (bod rozdělení) | Doporučené šířku pásma sítě | Poznámky |
| --- | --- | --- | --- | --- |
| Executive nebo komplexní, PPT |10 MB/s |1 MB/s |> 10 MB/s, upřednostňovaný 100 MB/s |S rychlostí 1 MB/s mnoha animace jsou ztraceny |
| Jednoduché PPT |5 MB/s |256 KB/s |10 MB/s |Na 256 KB/s snímky zatížení s významnému zpoždění |
| Internet Explorer |10 MB/s |1 MB/s |> 10 MB/s, upřednostňovaný 100 MB/s |S rychlostí 1 MB/s webové videa jsou rozmazaně a nestabilní, rychlé posouvání má problémy |
| Jednoduché PDF |1 MB/s |256 KB/s |5 MB/s |Na 256 KB/s trvá dobu načtení stránky |
| Smíšená PDF |1 MB/s |256 KB/s |5 MB/s |V 256 KB/s stránce udělá značné množství času se načíst |
| Flash přehrávání videa |10 MB/s |1 MB/s |> 10 MB/s, upřednostňovaný 100 MB/s |S rychlostí 1 MB/s je zrnitý a některé rámce jsou vyřadit. |
| Aplikace Word vzdálené zadáním |256 KB/s |128 KB/s |1 MB/s |Na 256 KB/s uživatele povšimnout čas mezi stisknutí kláves |

Abyste mohli vyhodnotit šířku pásma sítě na uživatele, vytvořte kombinace výše uvedených scénářů a odpovídající část požadované šířky pásma sítě. Vyberte nejvyšší číslo potřebné pro vaše scénáře. Vzhledem k tomu, že uživatelé používají téměř žádné samostatně systém, vezměte v úvahu některé rezerva pro uživatele, které pracují současně ve stejné síti.

## <a name="learn-more"></a>Další informace
* [Odhadnout využití šířky pásma sítě Azure RemoteApp](remoteapp-bandwidth.md)
* [Azure RemoteApp – jak šířku pásma sítě a kvalitu prostředí pracovní společně?](remoteapp-bandwidthexperience.md)
* [Azure RemoteApp šířka pásma sítě – obecné pokyny (Pokud nelze testovat vlastní)](remoteapp-bandwidthguidelines.md)

