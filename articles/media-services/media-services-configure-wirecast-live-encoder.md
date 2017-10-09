---
title: "aaaConfigure hello toosend kodér Telestream Wirecast živý datový proud s jednou přenosovou rychlostí | Microsoft Docs"
description: "Toto téma ukazuje, jak tooconfigure hello Wirecast live kodér toosend jednou přenosovou rychlostí datového proudu tooAMS kanály, které jsou povolené pro kódování v reálném čase. "
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 0d2f1e81-51a6-4ca9-894a-6dfa51ce4c70
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: e373f6c08232c652e65db584ded409c405d8cffe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-wirecast-encoder-toosend-a-single-bitrate-live-stream"></a>Použít hello Wirecast kodér toosend živý datový proud s jednou přenosovou rychlostí
> [!div class="op_single_selector"]
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [Elemental za provozu](media-services-configure-elemental-live-encoder.md)
> * [Čase](media-services-configure-tricaster-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

Toto téma ukazuje, jak tooconfigure hello [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) live kodér toosend jednou přenosovou rychlostí datového proudu tooAMS kanály, které jsou povolené pro kódování v reálném čase.  Další informace najdete v tématu [práce s kanály, že jsou povolené tooPerform živé kódování službou Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Tento kurz ukazuje, jak toomanage Azure Media Services (AMS) s nástrojem Azure Media Services Explorer (AMSE). Tento nástroj lze spustit pouze na počítačích s Windows. Pokud jste na Mac nebo Linux, použijte hello Azure portálu toocreate [kanály](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) a [programy](media-services-portal-creating-live-encoder-enabled-channel.md).

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

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. Zadejte název kanálu, hello pole popisu je volitelné. V části Nastavení kanál, vyberte **standardní** pro hello Live Encoding možnost s hello vstupní protokol nastaven příliš**RTMP**. Všechna ostatní nastavení jako je můžete nechat.

    Ujistěte se, zda text hello **počáteční hello nový kanál teď** je vybrána.

3. Klikněte na tlačítko **vytvořit kanál**.

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

> [!NOTE]
> Hello kanálu může trvat stejně dlouho jako toostart 20 minut.
>
>

Při spouštění hello kanál můžete [konfigurace hello kodér](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).

> [!IMPORTANT]
> Všimněte si, že fakturace začne hned, jak kanál přejde do stavu Připraveno. Další informace najdete v tématu [kanálu stavy](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a id=configure_wirecast_rtmp></a>Konfigurace hello kodér Telestream wirecast
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
1. Otevřete hello Telestream Wirecast aplikace na hello počítač se používá a nastavte pro streamování RTMP.
2. Výstup hello nakonfigurovat tak, že přejdete toohello **výstup** kartě a výběrem **nastavení výstupní...** .

    Ujistěte se, zda text hello **cíl výstupu** je nastaven příliš**RTMP serveru**.
3. Klikněte na **OK**.
4. Na stránce nastavení hello nastavit hello **cílové** pole toobe **Azure Media Services**.

    Hello kódování profilu je předem vybraná příliš**Azure H.264 720 p 16:9 (1280 × 720)**. toocustomize tato nastavení, vyberte hello ozubené kolečko ikonu toohello napravo od hello rozevírací nabídku a pak zvolte **nové přednastavení**.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)
5. Nakonfigurujte kodér přednastavení.

    Název hello přednastavení a vyhledejte hello následující doporučené nastavení:

    **Video**

   * Kodér: MainConcept H.264
   * Počet snímků za sekundu: 30
   * Průměrná přenosová rychlost: 5000 kbits za sekundu (lze upravit podle omezení sítě)
   * Profil: Main
   * Klíče rámce každých: 60 rámce

    **Zvuk**

   * Cílová přenosová rychlost: 192 kbits za sekundu
   * Vzorkovací frekvence: 44 100 kHz

     ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)
6. Stiskněte klávesu **Uložit**.

    pole Encoding Hello má nyní k dispozici pro výběr hello nově vytvořený profil.

    Ujistěte se, že je vybraný nový profil hello.
7. Získání vstupní adresa URL kanálu hello v pořadí tooassign ho toohello Wirecast **koncový bod RTMP**.

    Přejděte zpět toohello nástroj AMSE a zkontrolovat stav dokončení kanálu hello. Jakmile hello stav se změnil z **počáteční** příliš**systémem**, můžete získat hello vstupní adresa URL.

    Když běží hello kanál, klikněte pravým tlačítkem na název kanálu hello, přejděte dolů toohover přes **kopie vstupu URL tooclipboard** a pak vyberte **primární adresa URL vstupu**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)
8. V hello Wirecast **nastavení výstupní** okno, vložte tyto informace hello **adresu** pole části výstup hello a přiřadit název datového proudu.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

1. Vyberte **OK**.
2. Na hlavní hello **Wirecast** zkontrolujte vstupní zdroje pro připravení videa a zvuku a pak klikněte na tlačítko **datového proudu** v levém horním rohu hello.

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

> [!IMPORTANT]
> Před kliknutím na **datového proudu**, můžete **musí** zajistěte, aby byl kanál hello připraven.
> Ujistěte se také, není tooleave hello kanál ve stavu Připraveno bez vstupní příspěvku kanálu po dobu delší než > 15 minut.
>
>

## <a name="test-playback"></a>Přehrávání testu

Nástroj AMSE toohello přejděte a klikněte pravým tlačítkem na toobe kanál hello testována. V nabídce hello, najeďte myší na **přehrávání hello Preview** a vyberte **s Azure Media Player**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

Pokud datový proud hello objeví v hello player, hello kodér bylo správně nakonfigurované tooconnect tooAMS.

Je-li k chybě, bude nutné hello kanál toobe resetování a kodér nastavení upravit. Najdete v tématu hello [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.  

## <a name="create-a-program"></a>Vytvořit program
1. Po potvrzení kanálu přehrávání vytvořte program. V části hello **živé** v nástroj AMSE hello, klikněte v oblasti programu hello pravým tlačítkem a vyberte **vytvořit nový Program**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)
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
