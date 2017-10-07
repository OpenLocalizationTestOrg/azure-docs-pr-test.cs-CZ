---
title: "datový proud aaaLive s místními kodéry pomocí hello portálu Azure | Microsoft Docs"
description: "Tento kurz vás provede kroky hello k vytvoření kanálu, který je nakonfigurován pro průchozí doručování."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 6f4acd95-cc64-4dd9-9e2d-8734707de326
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 1fb341e022f66f33903e13e07d3e84c0216cad77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-live-streaming-with-on-premises-encoders-using-hello-azure-portal"></a>Jak tooperform živé streamování s místními kodéry pomocí hello portálu Azure
> [!div class="op_single_selector"]
> * [Azure Portal](media-services-portal-live-passthrough-get-started.md)
> * [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/channel)
> 
> 

Tento kurz vás provede kroky hello hello Azure portálu toocreate **kanál** který je nakonfigurován pro průchozí doručování. 

## <a name="prerequisites"></a>Požadavky
Hello následují požadované toocomplete hello kurzu:

* Účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
* Účet Media Services. toocreate účet Media Services najdete v části [jak tooCreate účtu Media Services](media-services-portal-create-account.md).
* Webová kamera. Například [kodér Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm).

Důrazně doporučujeme tooreview hello následující články:

* [Podpora RTMP ve službě Azure Media Services a kodéry služby Live Encoding](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
* [Přehled živého streamování pomocí služby Azure Media Services](media-services-manage-channels-overview.md)
* [Živé streamování pomocí místních kodérů, které vytvářejí datové proudy s více přenosovými rychlostmi](media-services-live-streaming-with-onprem-encoders.md)

## <a id="scenario"></a>Běžný scénář živého streamování
Hello následující kroky popisují úlohy týkající se vytváření běžné aplikací pro živé streamování využívající kanály, které jsou nakonfigurované pro průchozí doručování. Tento kurz ukazuje, jak toocreate a spravovat průchozí kanál a živé události.

>[!NOTE]
>Zkontrolujte, zda text hello, ze kterého chcete obsah toostream koncový bod streamování je v hello **systémem** stavu. 
    
1. Připojení počítače tooa videokameru. Spusťte a nakonfigurujte místní kodér pro kódování v reálném čase, který produkuje RTMP s více přenosovými rychlostmi nebo fragmentovaný proud MP4. Další informace najdete v článku [Podpora RTMP ve službě Azure Media Services a kodéry pro kódování v reálném čase](http://go.microsoft.com/fwlink/?LinkId=532824).
   
    Tento krok můžete provést i po vytvoření kanálu.
2. Vytvořit a spustit průchozí kanál.
3. Načtení hello kanál ingestovanou adresu URL. 
   
    Hello URL ingestování používá hello za provozu kodér toosend hello datového proudu toohello kanál.
4. Načíst URL náhledu kanálu hello. 
   
    Pomocí této adresy URL tooverify, jestli kanál správně přijímá živý datový proud hello.
5. Vytvořte živou událost nebo program. 
   
    Při použití hello portálu Azure, vytváření živé události vytvoří také asset. 

6. Jakmile jsou připravené toostart Streamovat a archivovat, spusťte hello událost nebo program.
7. Volitelně lze za provozu kodér hello signalizovaného toostart oznámení o inzerovaném programu. Hello reklama bude vložena do výstupního datového proudu hello.
8. Vždy, když chcete toostop streamování a archivaci hello události, zastavte hello událost nebo program.
9. Odstraňte hello událost nebo program (a volitelně můžete odstranit hello asset).     

> [!IMPORTANT]
> Zkontrolujte [živé streamování s místními kodéry, které vytvářejí proudy s více přenosovými rychlostmi](media-services-live-streaming-with-onprem-encoders.md) toolearn o konceptech a důležité informace související s toolive streamování s místními kodéry a průchozími kanály.
> 
> 

## <a name="tooview-notifications-and-errors"></a>tooview upozornění a chyb
Pokud chcete, aby tooview oznámení a chyby vytvořené hello portálu Azure, klikněte na ikonu oznámení hello.

![Oznámení](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

## <a name="create-and-start-pass-through-channels-and-events"></a>Vytvoření a spuštění průchozího kanálu.
Kanál, který je přidružen k událostem a programům, které umožňují toocontrol hello publikování a ukládání segmentů v živém datovém proudu. Kanály spravují události. 

Můžete zadat hello počet hodin, které chcete obsah hello zaznamenávají tooretain programu hello podle nastavení hello **archivačního okna** délka. Tuto hodnotu můžete nastavit v rozmezí od 5 minut tooa maximálně 25 hodin. Délka archivačního okna také určuje maximální množství času, které klienty můžete hledat zpět v čase od aktuální živé pozice hello hello. Události můžete spustit přes hello určenou dobu a obsah, který hello délky okna nevejde je vždy zahozen. Hodnota této vlastnosti také určuje, jak dlouho hello klienta můžou růst manifesty.

Každá událost je přidružena k assetu. toopublish hello událostí, je nutné vytvořit lokátor OnDemand pro hello související prostředek. Tento Lokátor umožňuje toobuild adresu URL pro streamování, kterou potom poskytnete tooyour klientů.

Kanál podporuje až toothree souběžně s události, takže si můžete vytvořit několik archivů hello stejného příchozího datového proudu. To vám umožní toopublish a archivovat různé části události podle potřeby. Například vaše firemní požadavky je tooarchive 6 hodin programu, ale toobroadcast pouze posledních 10 minut. tooaccomplish, je nutné toocreate dva současně spuštěné programy. Jeden program nastaven tooarchive 6 hodin hello události ale programu hello není publikována. Hello jiný program je sada tooarchive 10 minut a tento program budete publikovat.

Neměli byste znovu používat existující živé události. Místo toho vytvořte a spusťte novou událost pro každou jednotlivou událost.

Spusťte hello událost v případě, že jsou připravené toostart streamování a archivaci. Zastavte programu hello vždy, když chcete toostop streamování a archivaci události hello. 

obsah toodelete archivovat, zastavte a odstranit hello událost a potom odstraňte přidružený asset hello. Asset nemůžete odstranit, pokud se používá událost; Nejprve je třeba odstranit Hello událostí. 

I po zastavení a odstranění události hello, hello uživatelé by byl schopný toostream archivovaný obsah jako video na vyžádání, tak dlouho, dokud neodstraníte hello asset.

Pokud chcete archivovat hello tooretain obsahu, ale není ho mít dostupný pro streamování, odstraňte Lokátor streamování hello.

### <a name="toouse-hello-portal-toocreate-a-channel"></a>toouse hello portálu toocreate kanál
Tato část uvádí, jak toouse hello **rychle vytvořit** možnost toocreate průchozí kanál.

Další podrobnosti o průchozích kanálech najdete v tématu [Živé streamování pomocí místních kodérů, které vytvářejí datové proudy s více přenosovými rychlostmi](media-services-live-streaming-with-onprem-encoders.md).

1. V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.
2. V hello **nastavení** okně klikněte na tlačítko **živé streamování**. 
   
    ![Začínáme](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
   
    Hello **živé streamování** se zobrazí v okně.
3. Klikněte na tlačítko **rychle vytvořit** ingestování toocreate průchozí kanál s hello RTMP.
   
    Hello **vytvořit nový kanál** se zobrazí v okně.
4. Zadejte název nového kanálu hello a klikněte na tlačítko **vytvořit**. 
   
    Tím vytvoříte průchozí kanál s hello protokolem ingestování RTMP.

## <a name="create-events"></a>Vytvoření událostí
1. Vyberte kanál toowhich, chcete-li tooadd událost.
2. Stiskněte tlačítko **Živá událost**.

![Událost](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)

## <a name="get-ingest-urls"></a>Získání ingestovaných adres URL
Po vytvoření kanálu hello lze získat ingestovaných adres URL, které poskytnete kodéru toohello za provozu. Kodér Hello používá tyto adresy URL tooinput živý datový proud.

![Vytvořeno](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

## <a name="watch-hello-event"></a>Sledování událostí hello
toowatch hello událostí, klikněte na tlačítko **sledovat** v hello Azure portal nebo kopírování hello adresu URL streamování a použijte přehrávač dle svého výběru. 

![Vytvořeno](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

Živé události se automaticky převedený tooon vyžádání obsah při zastavení.

## <a name="clean-up"></a>Vyčištění
Další podrobnosti o průchozích kanálech najdete v tématu [Živé streamování pomocí místních kodérů, které vytvářejí datové proudy s více přenosovými rychlostmi](media-services-live-streaming-with-onprem-encoders.md).

* Kanál se dá zastavit jenom v případě, že byly zastaveny všechny události nebo programy na hello kanálu.  Jakmile hello kanál zastaví nedojde žádné poplatky. Když potřebujete toostart ho znovu, bude mít hello stejnou ingestovanou adresu URL, takže nebude nutné tooreconfigure kodér.
* Kanál se dá odstranit jenom v případě, že byly odstraněny všechny jeho živé události ve hello kanálu.

## <a name="view-archived-content"></a>Zobrazení archivovaného obsahu
I po zastavení a odstranění události hello, hello uživatelé by byl schopný toostream archivovaný obsah jako video na vyžádání, tak dlouho, dokud neodstraníte hello asset. Asset nemůžete odstranit, pokud se používá událost; Nejprve je třeba odstranit Hello událostí. 

Vyberte prostředky, toomanage **nastavení** a klikněte na tlačítko **prostředky**.

![Prostředky](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

## <a name="next-step"></a>Další krok
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

