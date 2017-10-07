---
title: "Průvodce aaaTroubleshooting pro živé streamování | Microsoft Docs"
description: "Toto téma nabízí návrhy na tom, jak tootroubleshoot živé streamování problémy."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 3a7f6c1d-ce57-4fa4-a7a6-edb526b3ffbf
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 8549bae947ff3b225ce624220d1e48b63f90208c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-live-streaming"></a>Průvodce řešením potíží s živým streamováním
Toto téma nabízí návrhy na postupy tootroubleshoot některé živé streamování problémy.

## <a name="issues-related-tooon-premises-encoders"></a>Problémy související s tooon místních kodérů
Tato část nabízí návrhy na tom, jak tootroubleshoot problémy související tooon místní kodéry, které jsou nakonfigurované toosend jednou přenosovou rychlostí datového proudu tooAMS kanály, které jsou povolené pro kódování v reálném čase.

### <a name="problem-would-like-toosee-logs"></a>Problém: Byste chtěli toosee protokoly
* **Potenciální problém**: Nelze najít protokoly kodér, který vám může pomoci při ladění problémů.
  
  * **Telestream Wirecast**: obvykle najdete v části C:\Users protokoly\{uživatelské jméno} \AppData\Roaming\Wirecast\ 
  * **Elementární živé**: můžete najít má toologs odkazy na portálu pro správu hello. Klikněte na **statistiky**, pak **protokoly**. Na hello **soubory protokolu** stránky, se zobrazí seznam protokolů pro všechny položky LiveEvent hello; vyberte hello jeden odpovídající aktuální relace. 
  * **Flash Live kodér médií**: můžete najít hello **adresář protokolu...**  přechodem toohello **kódování protokolu** kartě.

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Problém: Není žádná možnost pro výstup vytvořeného progresivní datového proudu
* **Potenciální problém**: kodér hello používá nebude automaticky zrušit prokládání. 
  
    **Řešení potíží**: Vyhledejte odstraňování prokládání možnost v rámci rozhraní kodér hello. Po povolení algoritmy pro odstranění prokládání znovu zkontrolujte nastavení progresivní výstup. 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-tooconnect"></a>Problém: Pokus o několik nastavení výstupní kodér a stále se nedaří tooconnect.
* **Potenciální problém**: Azure kódování kanál nebyl správně resetován. 
  
    **Řešení potíží**: Ujistěte se, hello encoder je už vkládání tooAMS, zastavení a obnovení hello kanálu. Jednou spustit znovu, zkuste se připojit kodér s novým nastavením hello. Pokud tento problém hello stále nevyřeší, zkuste vytvořit nový kanál zcela, někdy kanály se může stát poškozená po několika nezdařených pokusů o zadání.  
* **Potenciální problém**: nejsou optimální velikost GOP hello nebo nastavení klíčových snímků. 
  
    **Řešení potíží**: GOP doporučená velikost nebo @keyframe, které určuje interval je 2 sekundy. Některé kodéry vypočítat toto nastavení v počet snímků, jiné zase sekund. Příklad: při výstupu 30fps, hello GOP velikost bude 60 rámce, který je ekvivalentní too2 sekund.  
* **Potenciální problém**: uzavřené porty blokují hello datového proudu. 
  
    **Řešení potíží**: při streamování prostřednictvím RTMP, zkontrolovat brány firewall nebo proxy nastavení tooconfirm, že jsou otevřené odchozí porty 1935 a 1936. Při použití RTP streamování, potvrďte, že odchozí port 2010 je otevřen. 

### <a name="problem-when-configuring-hello-encoder-toostream-with-hello-rtp-protocol-there-is-no-place-tooenter-a-host-name"></a>Problém: Při konfiguraci hello kodér toostream s hello protokol RTP, není prostor tooenter název hostitele.
* **Potenciální problém**: mnoho RTP kodéry zakázat u názvů hostitele a IP adresu, bude nutné toobe získali.  
  
    **Řešení potíží**: toofind hello IP adresu, otevřete příkazový řádek na libovolném počítači. toodo v systému Windows, otevře hello spustit Spouštěče (WIN + R) a zadejte tooopen "cmd".  
  
    Jakmile hello příkazového řádku je otevřený, zadejte "Ping [AMS název hostitele]". 
  
    název hostitele Hello lze odvodit vynecháním hello číslo portu od hello Azure Ingestované adresy URL, jak je znázorněno v hello následující ukázka: 
  
    RTP://Test2-amstest009.RTP.Channel.mediaservices.Windows.NET:2010 / 
  
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

> [!NOTE]
> Pokud po kroků hello řešení potíží, které stále nemůžete použít datový proud úspěšně, odešlete lístek podpory pomocí hello portálu Azure.
> 
> 

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

