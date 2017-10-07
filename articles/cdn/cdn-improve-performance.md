---
title: "aaaImprove výkonu komprimací souborů v Azure CDN | Microsoft Docs"
description: "Zjistěte, jak přenos souborů tooimprove rychlost a zvyšuje zatížení stránky komprimací souborů v Azure CDN."
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
ms.openlocfilehash: 9a1698b6c29233c2e2e6fb17cdd8e919ae8bfa1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>Zvýšení výkonu komprimací souborů v Azure CDN
Komprese je jednoduchá ale účinná metoda tooimprove souboru rychlost přenosu dat na a zvýšení výkonu zatížení stránky snížením velikosti souboru dříve, než je odeslána ze serveru hello. Umožňuje snížit náklady na šířku pásma a poskytuje rychlejší reakce prostředí pro uživatele.

Existují dva způsoby tooenable komprese:

* Můžete povolit kompresi na původním serveru, v takovém případě hello předává CDN prostřednictvím hello komprimované soubory a poskytovat tooclients komprimované soubory, které o ně požádat.
* Můžete povolit kompresi přímo na CDN hraniční servery, ve kterých případu hello CDN zkomprimuje hello soubory a je i v případě, že nejsou komprimované serverem počátek hello sloužit tooend uživatele.

> [!IMPORTANT]
> Změny konfigurace CDN trvat některé čas toopropagate přes síť hello.  Pro <b>Azure CDN společnosti Akamai</b> profily, šíření obvykle dokončení v části jednu minutu.  Pro <b>Azure CDN společnosti Verizon</b> profily, obvykle uvidíte změny použít během 90 minut.  Pokud je to hello poprvé, co jste nastavili komprese pro koncový bod CDN, měli byste zvážit čeká se, že rozšíření nastavení komprese hello bodů POP toohello před řešení potíží 1 – 2 hodiny toobe
> 
> 

## <a name="enabling-compression"></a>Povolení komprese
> [!NOTE]
> Zadejte technologie Hello úrovních Standard a Premium CDN hello stejné funkce komprese, ale hello uživatelské rozhraní se liší.  Další informace o hello rozdíly mezi úrovních Standard a Premium CDN najdete v tématu [přehled CDN Azure](cdn-overview.md).
> 
> 

### <a name="standard-tier"></a>Úroveň Standard
> [!NOTE]
> Tato část se týká příliš**Azure CDN Standard od společnosti Verizon** a **Azure CDN Standard od společnosti Akamai** profily.
> 
> 

1. Na stránce profil CDN hello klikněte na koncový bod CDN hello chcete toomanage.
   
    ![Koncové body stránky profilu CDN](./media/cdn-file-compression/cdn-endpoints.png)
   
    Otevře se stránka koncový bod CDN Hello.
2. Klikněte na tlačítko hello **konfigurace** tlačítko.
   
    ![Tlačítko Spravovat stránky profil CDN](./media/cdn-file-compression/cdn-config-btn.png)
   
    Otevře se stránka konfigurace CDN Hello.
3. Zapnout **komprese**.
   
    ![Možnosti komprese CDN](./media/cdn-file-compression/cdn-compress-standard.png)
4. Použijte výchozí typy hello nebo změňte hello seznamu odebrání nebo přidáním typy souborů.
   
   > [!TIP]
   > Chvíli je možné, nedoporučuje se tooapply komprese toocompressed formátů, jako je například ZIP, MP3, MP4, JPG, atd.
   > 
   > 
5. Po provedení změny, klikněte na hello **Uložit** tlačítko.

### <a name="premium-tier"></a>Úroveň Premium
> [!NOTE]
> Tato část se týká příliš**Azure CDN Premium od společnosti Verizon** profily.
> 
> 

1. Na stránce profil hello CDN, klikněte na tlačítko hello **spravovat** tlačítko.
   
    ![Tlačítko Spravovat stránky profil CDN](./media/cdn-file-compression/cdn-manage-btn.png)
   
    Otevře portál pro správu Hello CDN.
2. Hover přes hello **HTTP velké** kartu a potom hover přes hello **nastavení mezipaměti** plovoucím panelem.  Klikněte na **komprese**.

    ![Výběr komprese souboru](./media/cdn-file-compression/cdn-compress-select.png)
   
    Zobrazí se možnosti komprese.
   
    ![Kompresí souborů](./media/cdn-file-compression/cdn-compress-files.png)
3. Povolit kompresi kliknutím hello **povolená komprese** přepínač.  Zadejte typy MIME hello chcete toocompress jako seznam s položkami oddělenými čárkou (bez mezer) v hello **typy souborů** textové pole.
   
   > [!TIP]
   > Chvíli je možné, nedoporučuje se tooapply komprese toocompressed formátů, jako je například ZIP, MP3, MP4, JPG, atd. 
   > 
   > 
4. Po provedení změny, klikněte na hello **aktualizace** tlačítko.

## <a name="compression-rules"></a>Komprese pravidla
Tyto tabulky popisují chování Azure CDN komprese pro každý scénář.

> [!IMPORTANT]
> Pro **Azure CDN společnosti Verizon** (Standard a Premium), jenom vhodné soubory jsou komprimovány.  toobe vhodné pro kompresi, musíte do souboru:
> 
> * Být větší než 128 bajtů.
> * Být menší než 1 MB.
> 
> Pro **Azure CDN společnosti Akamai**, všechny soubory, které jsou způsobilé pro kompresi.
> 
> Pro všechny produkty Azure CDN, soubor musí být typ MIME, který byl [nakonfigurované pro kompresi](#enabling-compression).
> 
> **Azure CDN společnosti Verizon** profilů (Standard a Premium) podporu **gzip** (GNU zip), **deflate**, **bzip2**, nebo **Brazílie**Kódování (Brotli). Pro Brotli kódování, komprese hello se provádí pouze na hranici hello. Hello nebo prohlížeče klienta, musí poslat žádost o hello pro kódování Brotli a komprimované asset hello musí komprimované na straně původu hello nejdřív. 
>
>**Azure CDN společnosti Akamai** profily podporují pouze **gzip** kódování.
> 
> **Azure CDN společnosti Akamai** koncových bodů se vždy požadavku **gzip** kódovaný soubory z počátku hello, bez ohledu na žádost klienta hello. 

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>Komprese vypnuta nebo souboru je není vhodná pro kompresi
| Klient požádal o formátu (prostřednictvím hlavičky Accept-Encoding) | Formát souborů uložených v mezipaměti | CDN odpovědi toohello klienta | Poznámky |
| --- | --- | --- | --- |
| Komprimované |Komprimované |Komprimované | |
| Komprimované |Nekomprimované |Nekomprimované | |
| Komprimované |Není v mezipaměti |Komprimované nebo nekomprimovaným |Závisí na počátku odpovědi |
| Nekomprimované |Komprimované |Nekomprimované | |
| Nekomprimované |Nekomprimované |Nekomprimované | |
| Nekomprimované |Není v mezipaměti |Nekomprimované | |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>Komprese zapnuta a souboru je vhodné pro kompresi
| Klient požádal o formátu (prostřednictvím hlavičky Accept-Encoding) | Formát souborů uložených v mezipaměti | CDN odpovědi toohello klienta | Poznámky |
| --- | --- | --- | --- |
| Komprimované |Komprimované |Komprimované |CDN transcodes mezi podporované formáty |
| Komprimované |Nekomprimované |Komprimované |CDN provede kompresi |
| Komprimované |Není v mezipaměti |Komprimované |CDN provede kompresi, pokud zdroj vrátí nekomprimované.  **Azure CDN společnosti Verizon** předává hello nekomprimovaného souboru na první požadavek hello a zkomprimuje potom a mezipamětí hello souboru pro následné požadavky.  Soubory s `Cache-Control: no-cache` záhlaví nikdy komprimuje. |
| Nekomprimované |Komprimované |Nekomprimované |Při dekompresi provede CDN |
| Nekomprimované |Nekomprimované |Nekomprimované | |
| Nekomprimované |Není v mezipaměti |Nekomprimované | |

## <a name="media-services-cdn-compression"></a>Komprese CDN služby Media Services
Pro Media Services CDN povoleno koncových bodů streamování, je komprese povolena ve výchozím nastavení pro následující typy obsahu hello: aplikace nebo vnd.ms-sstr + xml, application/dash+xml,application/vnd.apple.mpegurl, aplikace nebo f4m + xml. Můžete nelze povolit nebo zakázat komprese hello uvedených typů pomocí hello portálu Azure.  

## <a name="see-also"></a>Viz také
* [Řešení potíží s kompresí souborů v síti CDN](cdn-troubleshoot-compression.md)    

