---
title: "aaaConfigure hello NewTek čase kodér toosend živý datový proud s jednou přenosovou rychlostí | Microsoft Docs"
description: "Toto téma ukazuje, jak tooconfigure hello čase live kodér toosend jednou přenosovou rychlostí datového proudu tooAMS kanály, které jsou povolené pro kódování v reálném čase."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 8973181a-3059-471a-a6bb-ccda7d3ff297
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkd;anilmur
ms.openlocfilehash: 57dcf62a6a76b04e69f147a738be78ccb3c3ecdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-newtek-tricaster-encoder-toosend-a-single-bitrate-live-stream"></a>Použít hello NewTek čase kodér toosend živý datový proud s jednou přenosovou rychlostí
> [!div class="op_single_selector"]
> * [Čase](media-services-configure-tricaster-live-encoder.md)
> * [Elemental za provozu](media-services-configure-elemental-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

Toto téma ukazuje, jak tooconfigure hello [čase NewTek](http://newtek.com/products/tricaster-40.html) live kodér toosend jednou přenosovou rychlostí datového proudu tooAMS kanály, které jsou povolené pro kódování v reálném čase. Další informace najdete v tématu [práce s kanály, že jsou povolené tooPerform živé kódování službou Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Tento kurz ukazuje, jak toomanage Azure Media Services (AMS) s nástrojem Azure Media Services Explorer (AMSE). Tento nástroj lze spustit pouze na počítačích s Windows. Pokud jste na Mac nebo Linux, použijte hello Azure portálu toocreate [kanály](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) a [programy](media-services-portal-creating-live-encoder-enabled-channel.md).

> [!NOTE]
> Při použití čase pro odesílání v příspěvek kanálu tooAMS kanály, které jsou povolené kódování v reálném čase, může být video nebo zvuk chyb v živé události Pokud určité funkce čase, jako je rychlé vyjímání mezi informační kanály nebo přepnutí z slaty . Hello AMS tým pracuje na řešení těchto problémů do té doby, se nedoporučuje toouse tyto funkce.
>
>

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

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. Zadejte název kanálu, hello pole popisu je volitelné. V části Nastavení kanál, vyberte **standardní** pro hello Live Encoding možnost s hello vstupní protokol nastaven příliš**RTMP**. Všechna ostatní nastavení jako je můžete nechat.

    Ujistěte se, zda text hello **počáteční hello nový kanál teď** je vybrána.

3. Klikněte na tlačítko **vytvořit kanál**.

   ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

> [!NOTE]
> Hello kanálu může trvat stejně dlouho jako toostart 20 minut.
>
>

Při spouštění hello kanál můžete [konfigurace hello kodér](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).

> [!IMPORTANT]
> Všimněte si, že fakturace začne hned, jak kanál přejde do stavu Připraveno. Další informace najdete v tématu [kanálu stavy](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a id=configure_tricaster_rtmp></a>Konfigurace hello kodér NewTek čase
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
1. Vytvořte novou **čase NewTek** projektu v závislosti na tom, jaké vstupní zdroj videa se používá.
2. Jednou v rámci projektu, najde hello **datového proudu** tlačítko a klikněte na tlačítko hello ozubené kolečko ikonu další tooit tooaccess hello datového proudu konfigurace nabídky.

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. Jakmile nabídce hello otevře, klikněte na tlačítko **nový** pod nadpisem hello připojení. Po zobrazení výzvy pro typ připojení hello vyberte **Adobe Flash**.

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)
4. Klikněte na **OK**.
5. Profil aplikace FMLE lze importovat teď kliknutím hello šipkou rozevíracího seznamu v části **streamování profil** procházet příliš**Procházet**.

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)
6. Přejděte toowhere hello nakonfigurované FMLE profil byl uložen.
7. Vyberte ji a stiskněte klávesu **OK**.

    Po nahrání profilu hello pokračujte dalším krokem toohello.
8. Získání vstupní adresa URL kanálu hello v pořadí tooassign ho toohello čase **koncový bod RTMP**.

    Přejděte zpět toohello nástroj AMSE a zkontrolovat stav dokončení kanálu hello. Jakmile hello stav se změnil z **počáteční** příliš**systémem**, můžete získat hello vstupní adresa URL.

    Když běží hello kanál, klikněte pravým tlačítkem na název kanálu hello, přejděte dolů toohover přes **kopie vstupu URL tooclipboard** a pak vyberte **primární adresa URL vstupu**.  

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)
9. Tyto informace vložte hello **umístění** pole v části **serveru Flash** v rámci projektu čase hello. Přiřadit také název datového proudu v hello **ID datového proudu** pole.

    Pokud informace datový proud byl přidán toohello FMLE profil, se můžete také naimportovat toothis část kliknutím **importovat nastavení**, navigace profil FMLE toohello uložit a kliknutím na **OK**. Hello příslušná pole Flash serveru by měl naplnění hello informace z FMLE.

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)
10. Po dokončení klikněte na tlačítko **OK** v hello dolní části obrazovky hello. Jakmile jsou připravené videosoubory a zvukové vstupy do hello čase, začněte streamování tooAMS kliknutím hello **datového proudu** tlačítko.

     ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

> [!IMPORTANT]
> Před kliknutím na **datového proudu**, můžete **musí** zajistěte, aby byl kanál hello připraven.
> Ujistěte se také, není tooleave hello kanál ve stavu Připraveno bez vstupní příspěvku kanálu po dobu delší než > 15 minut.
>
>

## <a name="test-playback"></a>Přehrávání testu
Nástroj AMSE toohello přejděte a klikněte pravým tlačítkem na toobe kanál hello testována. V nabídce hello, najeďte myší na **přehrávání hello Preview** a vyberte **s Azure Media Player**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

Pokud datový proud hello objeví v hello player, hello kodér bylo správně nakonfigurované tooconnect tooAMS.

Je-li k chybě, bude nutné hello kanál toobe resetování a kodér nastavení upravit. Najdete v tématu hello [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.  

## <a name="create-a-program"></a>Vytvořit program
1. Po potvrzení kanálu přehrávání vytvořte program. V části hello **živé** v nástroj AMSE hello, klikněte v oblasti programu hello pravým tlačítkem a vyberte **vytvořit nový Program**.  

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)
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

## <a name="next-step"></a>Další krok
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
