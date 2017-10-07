---
title: "aaaConfigure hello elementární Live toosend kodér živý datový proud s jednou přenosovou rychlostí | Microsoft Docs"
description: "Toto téma ukazuje, jak tooconfigure hello elementární Live toosend kodér jednou přenosovou rychlostí datového proudu tooAMS kanály, které jsou povolené pro kódování v reálném čase."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 9c6bf6a9-6273-4fdd-9477-f0e565280b5b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: cenkd;anilmur;juliako
ms.openlocfilehash: 9a5de6189bfb123768a9da038b8c8db69cf85e91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-elemental-live-encoder-toosend-a-single-bitrate-live-stream"></a>Použít hello elementární Live kodér toosend živý datový proud s jednou přenosovou rychlostí
> [!div class="op_single_selector"]
> * [Elemental za provozu](media-services-configure-elemental-live-encoder.md)
> * [Čase](media-services-configure-tricaster-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

Toto téma ukazuje, jak tooconfigure hello [elementární Live](http://www.elementaltechnologies.com/products/elemental-live) kodér toosend tooAMS kanály, které jsou povolené kódování v reálném čase datového proudu s jednou přenosovou rychlostí.  Další informace najdete v tématu [práce s kanály, že jsou povolené tooPerform živé kódování službou Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Tento kurz ukazuje, jak toomanage Azure Media Services (AMS) s nástrojem Azure Media Services Explorer (AMSE). Tento nástroj lze spustit pouze na počítačích s Windows. Pokud jste na Mac nebo Linux, použijte hello Azure portálu toocreate [kanály](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) a [programy](media-services-portal-creating-live-encoder-enabled-channel.md).

## <a name="prerequisites"></a>Požadavky
* Musí mít praktické znalosti pomocí elementární Live webové rozhraní toocreate živé události.
* [Vytvoření účtu Azure Media Services](media-services-portal-create-account.md)
* Ujistěte se, je koncový bod streamování, spuštěná. Další informace najdete v tématu [spravovat koncové body streamování v účtu Media Services](media-services-portal-manage-streaming-endpoints.md).
* Nainstalujte nejnovější verzi hello hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) nástroj.
* Spusťte nástroj hello a připojte se účet tooyour AMS.

## <a name="tips"></a>Tipy
* Pokud je to možné, použijte standardní kabelové internetové připojení.
* Obvykle při určování nároky na šířku pásma je toodouble hello streamování přenosových rychlostí. Přestože není povinný požadavek, pomůže zmírnit dopad hello zahlcení sítě.
* Při použití softwaru na základě kodéry, zavřete se všechny nepotřebné programy.

## <a name="elemental-live-with-rtp-ingest"></a>Ingestování elementární živé s RTP
Tato část uvádí, jak tooconfigure hello elementární Live kodér, který odešle s jednou přenosovou rychlostí živý datový proud využívající RTP.  Další informace najdete v tématu [stream MPEG-TS využívající RTP](media-services-manage-live-encoder-enabled-channels.md#channel).

### <a name="create-a-channel"></a>Vytvoření kanálu

1. Přejděte v hello nástroj AMSE, toohello **živé** kartě a klikněte pravým tlačítkem v rámci oblasti kanál hello. Vyberte **vytvořit kanál...** v nabídce hello.

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. Zadejte název kanálu, hello pole popisu je volitelné. V části Nastavení kanál, vyberte **standardní** pro hello Live Encoding možnost s hello vstupní protokol nastaven příliš**RTP (MPEG-TS)**. Všechna ostatní nastavení jako je můžete nechat.

    Ujistěte se, zda text hello **počáteční hello nový kanál teď** je vybrána.

3. Klikněte na tlačítko **vytvořit kanál**.

   ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

> [!NOTE]
> Hello kanálu může trvat stejně dlouho jako toostart 20 minut.
>
>

Při spouštění hello kanál můžete [konfigurace hello kodér](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).

> [!IMPORTANT]
> Všimněte si, že fakturace začne hned, jak kanál přejde do stavu Připraveno. Další informace najdete v tématu [kanálu stavy](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

### <a id=configure_elemental_rtp></a>Konfigurace kodér hello elementární za provozu
V tento kurz hello se používají následující výstup nastavení. Hello zbývající část tohoto oddílu popisuje kroky konfigurace podrobněji.

**Video**:

* Kodeků: H.264
* Profil: Vysoká (úroveň 4.0)
* Přenosovou rychlostí: 5000 kb/s
* Klíčový snímek: 2 sekund (60 sekund)
* Míra s rámečkem: 30

**Zvuk**:

* Kodeků: AAC (LC)
* Přenosovou rychlostí: 192 kb /
* Vzorkovací frekvence: 44,1 kHz

#### <a name="configuration-steps"></a>Kroky konfigurace
1. Přejděte toohello **elementární Live** webové rozhraní a nastavte hello kodéru pro **UDP/TS** streamování.
2. Jakmile dojde k vytvoření nové události, posuňte se dolů toohello výstup skupiny a přidejte hello **UDP/TS** skupiny výstupu.
3. Vytvořit nový výstupní výběrem **nového datového proudu** a pak levým na **přidat výstup**.  

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental13.png)

   > [!NOTE]
   > Doporučujeme tuto událost elementární hello má časový kód hello nastavit také "Systémové hodiny" toohelp hello kodér znovu připojit v případě hello selhání datového proudu.
   >
   >
4. Teď, když hello výstup byl vytvořen, klikněte na tlačítko **přidat datový proud**. nastavení výstupní Hello se teď dá nakonfigurovat.
5. Posuňte se dolů toohello "Stream 1", kterou jste právě vytvořili, klikněte na tlačítko hello **Video** na levé straně hello a rozbalte hello **Upřesnit** v oddílu nastavení.

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    Zatímco elementární Live obsahuje širokou škálu dostupné přizpůsobení, hello, doporučujeme následující nastavení pro zahájení práce s streamování tooAMS.

   * Řešení: 1280 × 720
   * Kmitočet snímků: 30
   * GOP velikost: 60 rámce
   * Prokládání režim: progresivní
   * Přenosovou rychlostí: 5000000 bitů/s (to se dá upravit podle omezení sítě)

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

1. Získáte vstupní adresa URL kanálu hello.

    Přejděte zpět toohello nástroj AMSE a zkontrolovat stav dokončení kanálu hello. Jakmile hello stav se změnil z **počáteční** příliš**systémem**, můžete získat hello vstupní adresa URL.

    Když běží hello kanál, klikněte pravým tlačítkem na název kanálu hello, přejděte dolů toohover přes **kopie vstupu URL tooclipboard** a pak vyberte **primární adresa URL vstupu**.  

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
2. Tyto informace vložte hello **primární cílové** pole z hello Elemental. Všechna ostatní nastavení může zůstat výchozí hello.

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    Pro další redundanci opakujte tyto kroky s hello sekundární adresa URL vstupu tak, že vytvoříte samostatné kartě "Výstupní" pro streamování UDP/TS.
3. Klikněte na tlačítko **vytvořit** (Pokud byla vytvořena novou událost) nebo **aktualizace** (Pokud úpravy existující událostí) a poté pokračujte toostart hello kodér.

> [!IMPORTANT]
> Před kliknutím na **spustit** na hello elementární Live webové rozhraní, můžete **musí** zajistěte, aby byl kanál hello připraven.
> Ujistěte se také, není tooleave hello kanál ve stavu Připraveno bez události po dobu delší než > 15 minut.
>
>

Po spuštění hello datového proudu pro 30 sekund, přejděte zpět toohello AMSE nástroj a testování přehrávání.  

### <a name="test-playback"></a>Přehrávání testu

Nástroj AMSE toohello přejděte a klikněte pravým tlačítkem na toobe kanál hello testována. V nabídce hello, najeďte myší na **přehrávání hello Preview** a vyberte **s Azure Media Player**.  

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

Pokud datový proud hello objeví v hello player, hello kodér bylo správně nakonfigurované tooconnect tooAMS.

Je-li k chybě, bude nutné hello kanál toobe resetování a kodér nastavení upravit. Najdete v tématu hello [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.   

### <a name="create-a-program"></a>Vytvořit program
1. Po potvrzení kanálu přehrávání vytvořte program. V části hello **živé** v nástroj AMSE hello, klikněte v oblasti programu hello pravým tlačítkem a vyberte **vytvořit nový Program**.  

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental9.png)
2. Název programu hello a v případě potřeby upravit hello **délka archivačního okna** (které hodiny too4 výchozí nastavení). Můžete také určit umístění úložiště nebo ponechte jako výchozí hello.  
3. Zkontrolujte hello **počáteční hello teď Program** pole.
4. Klikněte na tlačítko **vytvořit Program**.  

    >[!NOTE]
    > Vytváření programu trvá kratší dobu, než vytvoření kanálu.   
      
5. Jakmile hello aplikaci, potvrďte přehrávání tak, že kliknete pravým tlačítkem programu hello a navigace příliš**přehrávání hello programech** a potom vyberete **s Azure Media Player**.  
6. Po potvrzení, klikněte pravým tlačítkem na programu hello znovu a vyberte **zkopírujte tooClipboard URL výstup hello** (nebo načtení těchto informací z hello **programu informace a nastavení** možnost nabídce hello).

datový proud Hello je nyní připraven toobe vložených v přehrávač nebo cílovou skupinu distribuované tooan live zobrazení.  

## <a name="troubleshooting"></a>Řešení potíží
Najdete v tématu hello [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
