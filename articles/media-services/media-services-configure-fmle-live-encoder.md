---
title: "aaaConfigure hello FMLE kodér toosend živý datový proud s jednou přenosovou rychlostí | Microsoft Docs"
description: "Toto téma ukazuje, jak tooconfigure hello toosend kodér rozhraní Flash média Live Encoder (FMLE) jednou přenosovou rychlostí datového proudu tooAMS kanály, které jsou povolené pro kódování v reálném čase."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3113f333-517a-47a1-a1b3-57e200c6b2a2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: 780d911c5186ec09c784264f9a0d0c3f8059d305
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-fmle-encoder-toosend-a-single-bitrate-live-stream"></a>Použít hello FMLE kodér toosend živý datový proud s jednou přenosovou rychlostí
> [!div class="op_single_selector"]
> * [FMLE](media-services-configure-fmle-live-encoder.md)
> * [Elemental za provozu](media-services-configure-elemental-live-encoder.md)
> * [Čase](media-services-configure-tricaster-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
>
>

Toto téma ukazuje, jak tooconfigure hello [Flash Live kodér médií](http://www.adobe.com/products/flash-media-encoder.html) (FMLE) kodér toosend tooAMS kanály, které jsou povolené kódování v reálném čase datového proudu s jednou přenosovou rychlostí. Další informace najdete v tématu [práce s kanály, že jsou povolené tooPerform živé kódování službou Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Tento kurz ukazuje, jak toomanage Azure Media Services (AMS) s nástrojem Azure Media Services Explorer (AMSE). Tento nástroj lze spustit pouze na počítačích s Windows. Pokud jste na Mac nebo Linux, použijte hello Azure portálu toocreate [kanály](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) a [programy](media-services-portal-creating-live-encoder-enabled-channel.md).

Všimněte si, že tento kurz popisuje použití AAC. Však nepodporuje FMLE podporuje AAC ve výchozím nastavení. Potřebovali byste toopurchase modulu plug-in pro kódování AAC například kvůli MainConcept: [AAC modulu plug-in](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)

## <a name="prerequisites"></a>Požadavky
* [Vytvoření účtu Azure Media Services](media-services-portal-create-account.md)
* Ujistěte se, je koncový bod streamování, spuštěná. Další informace najdete v tématu [spravovat koncové body streamování v účtu Media Services](media-services-portal-manage-streaming-endpoints.md)
* Nainstalujte nejnovější verzi hello hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) nástroj.
* Spusťte nástroj hello a připojte se účet tooyour AMS.

## <a name="tips"></a>Tipy
* Pokud je to možné, použijte standardní kabelové internetové připojení.
* Obvykle při určování nároky na šířku pásma je toodouble hello streamování přenosových rychlostí. Přestože není povinný požadavek, pomůže zmírnit dopad hello zahlcení sítě.
* Při použití softwaru na základě kodéry, zavřete se všechny nepotřebné programy.

## <a name="create-a-channel"></a>Vytvoření kanálu
1. Přejděte v hello nástroj AMSE, toohello **živé** kartě a klikněte pravým tlačítkem v rámci oblasti kanál hello. Vyberte **vytvořit kanál...** v nabídce hello.

    ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle1.png)

2. Zadejte název kanálu, hello pole popisu je volitelné. V části Nastavení kanál, vyberte **standardní** pro hello Live Encoding možnost s hello vstupní protokol nastaven příliš**RTMP**. Všechna ostatní nastavení jako je můžete nechat.

    Ujistěte se, zda text hello **počáteční hello nový kanál teď** je vybrána.

3. Klikněte na tlačítko **vytvořit kanál**.

   ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle2.png)

> [!NOTE]
> Hello kanálu může trvat stejně dlouho jako toostart 20 minut.
>
>

Při spouštění hello kanál můžete [konfigurace hello kodér](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).

> [!IMPORTANT]
> Všimněte si, že fakturace začne hned, jak kanál přejde do stavu Připraveno. Další informace najdete v tématu [kanálu stavy](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a id=configure_fmle_rtmp></a>Konfigurace kodér FMLE hello
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

### <a name="configuration-steps"></a>Kroky konfigurace
1. Přejděte toohello, které rozhraní Flash Live kodér médií na (FMLE) na počítači hello používá.

    rozhraní Hello je jeden hlavní stránce nastavení. Věnujte prosím na vědomí následující hello doporučené nastavení tooget začít s streamování využívající FMLE.

   * Formát: H.264 obnovovací frekvence: 30,00
   * Vstupní velikost: 1280 × 720
   * Přenosová rychlost: 5000 kb/s (lze upravit podle omezení sítě)  

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle3.png)

     Při použití prokládaných zdroje, prosím hello zaškrtnout možnost "Volby Odstranit prokládání"
2. Vyberte hello klíče ikonu další tooFormat, tyto další nastavení by mělo být:

   * Profil: Main
   * Úroveň: 4.0
   * Frekvence @keyframe, které určuje: 2 sekund

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle4.png)
3. Nastavte hello následující důležité zvuk nastavení:

   * Formát: AAC
   * Vzorkovací frekvence: 44100 Hz
   * Přenosovou rychlostí: 192 kb /

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle5.png)
4. Získání vstupní adresa URL kanálu hello v pořadí tooassign ho toohello FMLE na **koncový bod RTMP**.

    Přejděte zpět toohello nástroj AMSE a zkontrolovat stav dokončení kanálu hello. Jakmile hello stav se změnil z **počáteční** příliš**systémem**, můžete získat hello vstupní adresa URL.

    Když běží hello kanál, klikněte pravým tlačítkem na název kanálu hello, přejděte dolů toohover přes **kopie vstupu URL tooclipboard** a pak vyberte **primární adresa URL vstupu**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle6.png)
5. Tyto informace vložte hello **FMS URL** pole části výstup hello a přiřadit název datového proudu.

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle7.png)

    Pro další redundanci opakujte tyto kroky s hello sekundární adresa URL vstupu.
6. Vyberte **Connect** (Připojit).

> [!IMPORTANT]
> Před kliknutím na **připojit**, můžete **musí** zajistěte, aby byl kanál hello připraven.
> Ujistěte se také, není tooleave hello kanál ve stavu Připraveno bez vstupní příspěvku kanálu po dobu delší než > 15 minut.
>
>

## <a name="test-playback"></a>Přehrávání testu

Nástroj AMSE toohello přejděte a klikněte pravým tlačítkem na toobe kanál hello testována. V nabídce hello, najeďte myší na **přehrávání hello Preview** a vyberte **s Azure Media Player**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle8.png)

Pokud datový proud hello objeví v hello player, hello kodér bylo správně nakonfigurované tooconnect tooAMS.

Je-li k chybě, bude nutné hello kanál toobe resetování a kodér nastavení upravit. Najdete v tématu hello [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.  

## <a name="create-a-program"></a>Vytvořit program
1. Po potvrzení kanálu přehrávání vytvořte program. V části hello **živé** v nástroj AMSE hello, klikněte v oblasti programu hello pravým tlačítkem a vyberte **vytvořit nový Program**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle9.png)
2. Název programu hello a v případě potřeby upravit hello **délka archivačního okna** (které hodiny too4 výchozí nastavení). Můžete také určit umístění úložiště nebo ponechte jako výchozí hello.  
3. Zkontrolujte hello **počáteční hello teď Program** pole.
4. Klikněte na tlačítko **vytvořit Program**.  

    >[!NOTE]
    >Vytváření programu trvá kratší dobu, než vytvoření kanálu.
        
5. Jakmile hello aplikaci, potvrďte přehrávání tak, že kliknete pravým tlačítkem programu hello a navigace příliš**přehrávání hello programech** a potom vyberete **s Azure Media Player**.  
6. Po potvrzení, klikněte pravým tlačítkem na programu hello znovu a vyberte **zkopírujte tooClipboard URL výstup hello** (nebo načtení těchto informací z hello **programu informace a nastavení** možnost nabídce hello).

datový proud Hello je nyní připraven toobe vložených v přehrávač nebo cílovou skupinu distribuované tooan live zobrazení.  

## <a name="troubleshooting"></a>Řešení potíží
Najdete v tématu hello [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
