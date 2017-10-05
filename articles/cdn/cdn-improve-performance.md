---
title: "Zvýšení výkonu komprimací souborů v Azure CDN | Microsoft Docs"
description: "Naučte se zlepšit rychlost přenosu souborů a zvyšuje zatížení stránky komprimací souborů v Azure CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 7546650e6096a880f4fb4d0c94dd4ecc00b70160
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>Zvýšení výkonu komprimací souborů v Azure CDN
Komprese je jednoduchá ale účinná metoda a zvýšit rychlost přenosu souborů zvyšuje zatížení stránky omezení velikosti souboru před odesláním ze serveru. Umožňuje snížit náklady na šířku pásma a poskytuje rychlejší reakce prostředí pro uživatele.

Existují dva způsoby, jak povolit kompresi:

* Můžete povolit kompresi na serveru původu, ve kterém případ CDN projdou komprimované soubory a poskytovat komprimovaných souborů na klienty, kteří požadují je.
* Můžete povolit kompresi přímo na servery edge CDN, ve kterých případ CDN komprimaci soubory a poskytovat koncovým uživatelům, i v případě, že nejsou komprimované serverem původu.

> [!IMPORTANT]
> Změny konfigurace CDN trvat nějakou dobu šířit přes síť.  Pro <b>Azure CDN společnosti Akamai</b> profily, šíření obvykle dokončení v části jednu minutu.  Pro <b>Azure CDN společnosti Verizon</b> profily, obvykle uvidíte změny použít během 90 minut.  Pokud je to poprvé, které jste nastavili komprese pro koncový bod CDN, měli byste zvážit čekání 1 – 2 hodiny jistotu komprese, které se mají nastavení rozšíří do bodů POP před řešení potíží s
> 
> 

## <a name="enabling-compression"></a>Povolení komprese
> [!NOTE]
> Úrovních Standard a Premium CDN nabízí stejnou funkčnost kompresi, ale uživatelské rozhraní se liší.  Další informace o rozdílech mezi úrovních Standard a Premium CDN najdete v tématu [přehled CDN Azure](cdn-overview.md).
> 
> 

### <a name="standard-tier"></a>Úroveň Standard
> [!NOTE]
> Tato část se týká **Azure CDN Standard od společnosti Verizon** a **Azure CDN Standard od společnosti Akamai** profily.
> 
> 

1. Na stránce profil CDN klikněte na koncový bod CDN, které chcete spravovat.
   
    ![Koncové body stránky profilu CDN](./media/cdn-file-compression/cdn-endpoints.png)
   
    Otevře se stránka koncového bodu CDN.
2. Klikněte **konfigurace** tlačítko.
   
    ![Tlačítko Spravovat stránky profil CDN](./media/cdn-file-compression/cdn-config-btn.png)
   
    Otevře se stránka konfigurace CDN.
3. Zapnout **komprese**.
   
    ![Možnosti komprese CDN](./media/cdn-file-compression/cdn-compress-standard.png)
4. Použijte výchozí typy nebo upravte seznam, odebrání nebo přidáním typy souborů.
   
   > [!TIP]
   > Chvíli to možné, není doporučeno použít komprimaci pro komprimovaný formáty, například ZIP, MP3, MP4, JPG, atd.
   > 
   > 
5. Po provedení změny, klikněte na **Uložit** tlačítko.

### <a name="premium-tier"></a>Úroveň Premium
> [!NOTE]
> Tato část se týká **Azure CDN Premium od společnosti Verizon** profily.
> 
> 

1. Na stránce profil CDN, klikněte **spravovat** tlačítko.
   
    ![Tlačítko Spravovat stránky profil CDN](./media/cdn-file-compression/cdn-manage-btn.png)
   
    Otevře se na portálu pro správu CDN.
2. Najeďte myší **HTTP velké** a potom přejděte myší **nastavení mezipaměti** plovoucím panelem.  Klikněte na **komprese**.

    ![Výběr komprese souboru](./media/cdn-file-compression/cdn-compress-select.png)
   
    Zobrazí se možnosti komprese.
   
    ![Kompresí souborů](./media/cdn-file-compression/cdn-compress-files.png)
3. Povolit kompresi kliknutím **povolená komprese** přepínač.  Zadejte typy MIME, který chcete komprimovat jako seznam s položkami oddělenými čárkou (bez mezer) **typy souborů** textové pole.
   
   > [!TIP]
   > Chvíli to možné, není doporučeno použít komprimaci pro komprimovaný formáty, například ZIP, MP3, MP4, JPG, atd. 
   > 
   > 
4. Po provedení změny, klikněte na **aktualizace** tlačítko.

## <a name="compression-rules"></a>Komprese pravidla
Tyto tabulky popisují chování Azure CDN komprese pro každý scénář.

> [!IMPORTANT]
> Pro **Azure CDN společnosti Verizon** (Standard a Premium), jenom vhodné soubory jsou komprimovány.  Způsobilé k komprese, musíte do souboru:
> 
> * Být větší než 128 bajtů.
> * Být menší než 1 MB.
> 
> Pro **Azure CDN společnosti Akamai**, všechny soubory, které jsou způsobilé pro kompresi.
> 
> Pro všechny produkty Azure CDN, soubor musí být typ MIME, který byl [nakonfigurované pro kompresi](#enabling-compression).
> 
> **Azure CDN společnosti Verizon** profilů (Standard a Premium) podporu **gzip** (GNU zip), **deflate**, **bzip2**, nebo **Brazílie**Kódování (Brotli). Pro Brotli kódování, komprese provádí pouze na hranici. V klientském prohlížeči musíte odeslat žádost o Brotli kódování a komprimované asset musí mít komprimované na straně původu nejdřív. 
>
>**Azure CDN společnosti Akamai** profily podporují pouze **gzip** kódování.
> 
> **Azure CDN společnosti Akamai** koncových bodů se vždy požadavku **gzip** kódovaný soubory z tohoto počátku, a to bez ohledu na žádost klienta. 

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>Komprese vypnuta nebo souboru je není vhodná pro kompresi
| Klient požádal o formátu (prostřednictvím hlavičky Accept-Encoding) | Formát souborů uložených v mezipaměti | CDN odpověď klientovi | Poznámky |
| --- | --- | --- | --- |
| Komprimované |Komprimované |Komprimované | |
| Komprimované |Nekomprimované |Nekomprimované | |
| Komprimované |Není v mezipaměti |Komprimované nebo nekomprimovaným |Závisí na počátku odpovědi |
| Nekomprimované |Komprimované |Nekomprimované | |
| Nekomprimované |Nekomprimované |Nekomprimované | |
| Nekomprimované |Není v mezipaměti |Nekomprimované | |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>Komprese zapnuta a souboru je vhodné pro kompresi
| Klient požádal o formátu (prostřednictvím hlavičky Accept-Encoding) | Formát souborů uložených v mezipaměti | CDN odpověď klientovi | Poznámky |
| --- | --- | --- | --- |
| Komprimované |Komprimované |Komprimované |CDN transcodes mezi podporované formáty |
| Komprimované |Nekomprimované |Komprimované |CDN provede kompresi |
| Komprimované |Není v mezipaměti |Komprimované |CDN provede kompresi, pokud zdroj vrátí nekomprimované.  **Azure CDN společnosti Verizon** předá nekomprimovaného souboru na první požadavek a pak komprimuje a ukládá do mezipaměti souborů pro následné požadavky.  Soubory s `Cache-Control: no-cache` záhlaví nikdy komprimuje. |
| Nekomprimované |Komprimované |Nekomprimované |Při dekompresi provede CDN |
| Nekomprimované |Nekomprimované |Nekomprimované | |
| Nekomprimované |Není v mezipaměti |Nekomprimované | |

## <a name="media-services-cdn-compression"></a>Komprese CDN služby Media Services
Pro Media Services CDN povoleno koncových bodů streamování, je komprese povolena ve výchozím nastavení pro následující typy obsahu: aplikace nebo vnd.ms-sstr + xml, application/dash+xml,application/vnd.apple.mpegurl, aplikace nebo f4m + xml. Můžete nelze povolit nebo zakázat komprese uvedených typů pomocí portálu Azure.  

## <a name="see-also"></a>Viz také
* [Řešení potíží s kompresí souborů v síti CDN](cdn-troubleshoot-compression.md)    

