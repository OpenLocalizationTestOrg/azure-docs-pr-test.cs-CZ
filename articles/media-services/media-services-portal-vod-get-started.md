---
title: "aaaGet spuštění s doručováním VoD pomocí hello portálu Azure | Microsoft Docs"
description: "Tento kurz vás provede procesem hello kroky implementace základní služby doručování obsahu vyžádání pomocí Azure Media Services (AMS) aplikace pomocí hello portálu Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 6c98fcfa-39e6-43a5-83a5-d4954788f8a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 5c1c1b1f74ec1f1301120fe8e5a5ae183fe0338f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-hello-azure-portal"></a>Začínáme s doručováním obsahu na vyžádání pomocí portálu Azure hello
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Tento kurz vás provede procesem hello kroky implementace základní služby doručování obsahu vyžádání pomocí Azure Media Services (AMS) aplikace pomocí hello portálu Azure.

## <a name="prerequisites"></a>Požadavky
Hello následují požadované toocomplete hello kurzu:

* Účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
* Účet Media Services. toocreate účet Media Services najdete v části [jak tooCreate účtu Media Services](media-services-portal-create-account.md).

Tento kurz zahrnuje hello následující úlohy:

1. Spusťte koncový bod streamování.
2. Nahrání videosouboru
3. Zakódujte hello zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí.
4. Publikujte hello asset a get streamování a progresivní stahování adresy URL.  
5. Přehrání obsahu

## <a name="start-streaming-endpoints"></a>Spusťte koncové body streamování. 

Při práci se službou Azure Media Services je jedním hello nejběžnějších scénářů doručování videa přes streamování s adaptivní přenosovou rychlostí. Služba Media Services poskytuje dynamické balení, což vám umožní toodeliver kódováním MP4 obsah ve formátech streamování podporovaných službou Media Services (MPEG DASH, HLS, technologie Smooth Streaming) v běhu, aniž byste museli toostore předem zabalené vaší s adaptivní přenosovou rychlostí verze pro každý z těchto formátů streamování.

>[!NOTE]
>Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu. toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu. 

toostart hello koncový bod streamování, hello následující:

1. Přihlaste se na hello [portál Azure](https://portal.azure.com/).
2. V okně Nastavení hello klikněte Streaming koncové body. 
3. Klikněte na tlačítko hello výchozího koncového bodu streamování. 

    Zobrazí se okno Hello výchozí podrobnosti koncový bod STREAMOVÁNÍ.

4. Klikněte na ikonu Start hello.
5. Klikněte na tlačítko toosave tlačítko hello uložit provedené změny.

## <a name="upload-files"></a>Nahrání souborů
toostream videa pomocí služby Azure Media Services, budete potřebovat tooupload hello zdrojová videa, zakódovat je do více přenosových rychlostí a publikujte hello výsledek. prvním krokem Hello je popsaná v této části. 

1. V hello **nastavení** okně klikněte na tlačítko **prostředky**.
   
    ![Nahrání souborů](./media/media-services-portal-vod-get-started/media-services-upload.png)
2. Klikněte na tlačítko hello **nahrát** tlačítko.
   
    Hello **nahrát asset videa** se zobrazí v okně.
   
   > [!NOTE]
   > Velikost souboru není nijak omezená.
   > 
   > 
3. Procházet toohello požadovaného video ve vašem počítači, vyberte ho a klikněte na OK.  
   
    Spustí nahrávání Hello a zobrazí se průběh hello pod názvem souboru hello.  

Po dokončení nahrávání hello by se zobrazit hello nový prostředek zobrazí v hello **prostředky** okno. 

## <a name="encode-assets"></a>Kódování assetů

Při práci se službou Azure Media Services je jedním nejběžnější scénářů hello doručování tooyour klienti streamování s adaptivní přenosovou rychlostí. Služba Media Services podporuje následující adaptivní přenosové rychlosti streamování technologie hello: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH. tooprepare videa pro streamování s adaptivní přenosovou rychlostí, je nutné tooencode svůj zdroj videa do souborů s více přenosovými rychlostmi. Měli byste použít hello **Media Encoder Standard** tooencode kodér videa.  

Služba Media Services také poskytuje dynamické balení, což vám umožní toodeliver vaše soubory MP4 více přenosovými rychlostmi v hello následující streamování formáty: MPEG DASH, HLS, technologie Smooth Streaming, aniž byste museli toorepackage do těchto formátů streamování. Při dynamickém balení stačí pouze toostore a sestavení platím hello souborů v jednom úložném formátu a služba Media Services a slouží hello odpovídající reakci na požadavky klientů.

tootake výhod dynamického balení, je nutné tooencode zdrojového souboru do sady souborů MP4 s více přenosovými rychlostmi (postup hello kódování je ukázán později v této části).

### <a name="toouse-hello-portal-tooencode"></a>portál tooencode toouse hello
Tato část popisuje kroky hello tooencode může trvat svůj obsah pomocí procesoru Media Encoder Standard.

1. V hello **nastavení** vyberte **prostředky**.  
2. V hello **prostředky** okno, vyberte hello asset, které chcete tooencode.
3. Stiskněte klávesu hello **kódovat** tlačítko.
4. V hello **kódovat asset** oken, vyberte hello procesor "Media Encoder Standard" a jedno z přednastavení. Informace o předvolbách najdete v tématu popisujícím [automatické generování žebříčku přenosových rychlostí](media-services-autogen-bitrate-ladder-with-mes.md) a v tématu [Předvolby úloh pro MES](media-services-mes-presets-overview.md). Pokud máte v plánu toocontrol které předvolby kódování se používá, mějte na paměti: je důležité tooselect hello přednastavení, která je nejvhodnější pro vaše vstupní video. Například pokud znáte vaše vstupní video má rozlišení 1920 × 1080 pixelů, pak můžete použít hello "H264 Multiple Bitrate 1080p" přednastavené. Pokud máte video s nízkým rozlišením (640 × 360), neměli byste používat předvolbu H264 Multiple Bitrate 1080p.
   
   Pro snadnější správu máte možnost úpravy hello název hello výstupní asset a název hello hello úlohy.
   
   ![Kódování assetů](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Stiskněte **Vytvořit**.

### <a name="monitor-encoding-job-progress"></a>Monitorování průběhu úlohy kódování
Klikněte na tlačítko toomonitor hello průběh úlohy kódování hello **nastavení** (v hello horní části stránky hello) a potom vyberte **úlohy**.

![Úlohy](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a>Publikování obsahu
tooprovide uživatelů s adresou URL, která se dá použít toostream nebo stažení vašeho obsahu, je nejprve nutné příliš "publikovat" asset vytvořením lokátoru. Lokátory zajišťují přístup toofiles obsažené v hello asset. Služba Media Services podporuje dva typy lokátorů: 

* Streamování (OnDemandOrigin) lokátory, používají pro adaptivní streamování (například toostream MPEG DASH, HLS nebo technologie Smooth Streaming). toocreate Lokátor streamování váš asset musí obsahovat soubor .ism. 
* Progresivní lokátory (SAS), které se používají pro doručení videa přes progresivní stahování.

Adresu URL streamování má následující formát hello a můžete ji použít tooplay assetů technologie Smooth Streaming.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

připojit toobuild na adresu URL, streamování HLS (format = m3u8-aapl) toohello adresy URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

připojit toobuild na adresu URL, streamování MPEG DASH (formát = mpd. čas csf) toohello adresy URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Adresa URL typu SAS má následující formát hello.

    {blob container name}/{asset name}/{file name}/{SAS signature}

> [!NOTE]
> Pokud jste použili hello portálu toocreate lokátorů před březnem 2015, byly vytvořeny lokátory s platností dva roky.  
> 
> 

tooupdate na datum vypršení platnosti lokátoru, použijte [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) nebo [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) rozhraní API. Při aktualizaci hello datum vypršení platnosti lokátoru SAS se změní adresa URL hello.

### <a name="toouse-hello-portal-toopublish-an-asset"></a>toouse hello portálu toopublish prostředek
toouse hello portálu toopublish prostředek, hello následující:

1. Vyberte **Nastavení** > **Assety**.
2. Vyberte, které chcete toopublish asset hello.
3. Klikněte na tlačítko hello **publikovat** tlačítko.
4. Vyberte typ lokátoru hello.
5. Stiskněte **Přidat**.
   
    ![Publikování](./media/media-services-portal-vod-get-started/media-services-publish1.png)

Adresa URL Hello je přidána toohello seznam **publikovaných adres URL**.

## <a name="play-content-from-hello-portal"></a>Přehrávání obsahu z portálu hello
Hello portál Azure nabízí přehrávač obsahu, které můžete použít tootest videa.

Klikněte na tlačítko hello požadovaného video a potom klikněte na hello **přehrání** tlačítko.

![Publikování](./media/media-services-portal-vod-get-started/media-services-play.png)

Musí být splněny určité předpoklady:

* toobegin streamování, spusťte spuštěné hello **výchozí** koncový bod streamování.
* Zajistěte, aby byla publikována hello videa.
* To **přehrávač médií** přehrává z výchozího hello koncového bodu streamování. Pokud chcete, aby tooplay z jiného než výchozího koncového bodu, streamování klikněte toocopy hello adresu URL a použijte jiný přehrávač. Například můžete použít [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

## <a name="next-steps"></a>Další kroky
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

