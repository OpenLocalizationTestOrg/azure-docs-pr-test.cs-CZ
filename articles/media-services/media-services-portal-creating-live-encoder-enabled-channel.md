---
title: "aaaHow tooperform živé streamování využívající Azure Media Services toocreate více přenosovými rychlostmi datových proudů s hello portálu Azure | Microsoft Docs"
description: "Tento kurz nevystavíte slabé stránky zabezpečení vás provede kroky postupu hello vytvoření kanálu, který přijímá datový proud s jednou přenosovou rychlostí a kóduje ho datového proudu toomulti přenosovými rychlostmi pomocí hello portálu Azure."
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 504f74c2-3103-42a0-897b-9ff52f279e23
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 963a25b8ba4683a2ce34d9fb0e19499874b4707c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-live-streaming-using-azure-media-services-toocreate-multi-bitrate-streams-with-hello-azure-portal"></a>Jak datové proudy tooperform živé streamování využívající Azure Media Services toocreate více přenosovými rychlostmi pomocí hello portálu Azure
> [!div class="op_single_selector"]
> * [Azure Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
> * [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
> * [REST API](https://docs.microsoft.com/rest/api/media/operations/channel)
> 
> 

Tento kurz vás provede kroky hello k vytvoření **kanál** který přijímá datový proud s jednou přenosovou rychlostí a kóduje ho datového proudu toomulti přenosovými rychlostmi.

> [!NOTE]
> Další koncepční informace najdete v části související tooChannels, které jsou povolené kódování v reálném čase, [živé streamování využívající Azure Media Services toocreate více přenosovými rychlostmi datové proudy](media-services-manage-live-encoder-enabled-channels.md).
> 
> 

## <a name="common-live-streaming-scenario"></a>Běžný scénář živého streamování
Hello následují obecné kroky při vytváření aplikací pro běžné živé streamování.

> [!NOTE]
> V současné době hello maximální doporučená doba živé události je 8 hodin. Pokud potřebujete toorun kanál pro delší časové období, obraťte se na adrese amslived@Microsoft.com.
> 
> 

1. Připojení počítače tooa videokameru. Spusťte a nakonfigurujte místní kodér za provozu, který můžete výstupní datový proud s jednou přenosovou rychlostí v jednom z následujících protokolů hello: RTMP, technologie Smooth Streaming nebo RTP (MPEG-TS). Další informace najdete v článku [Podpora RTMP ve službě Azure Media Services a kodéry pro kódování v reálném čase](http://go.microsoft.com/fwlink/?LinkId=532824).
   
    Tento krok můžete provést i po vytvoření kanálu.
2. Vytvořte a spusťte kanál. 
3. Načtení hello kanál ingestovanou adresu URL. 
   
    Hello URL ingestování používá hello za provozu kodér toosend hello datového proudu toohello kanál.
4. Načíst URL náhledu kanálu hello. 
   
    Pomocí této adresy URL tooverify, jestli kanál správně přijímá živý datový proud hello.
5. Vytvořte událost nebo program (tím se vytvoří také asset). 
6. Publikování událostí hello, (tím se vytvoří Lokátor OnDemand pro přidružený asset hello).    
7. Spusťte hello událost v případě, že jsou připravené toostart streamování a archivaci.
8. Volitelně lze za provozu kodér hello signalizovaného toostart oznámení o inzerovaném programu. Hello reklama bude vložena do výstupního datového proudu hello.
9. Zastavte hello událost vždy, když chcete toostop streamování a archivaci události hello.
10. Odstranit událost hello (a volitelně můžete odstranit hello asset).   

## <a name="in-this-tutorial"></a>V tomto kurzu
V tomto kurzu hello portál Azure je použité tooaccomplish hello následující úlohy: 

1. Vytvoření kanálu tedy tooperform povolené kódování v reálném čase.
2. Get hello Ingestované adresy URL, v pořadí toosupply ho toolive kodér. Kodér za provozu Hello použije tento datový proud hello tooingest adresu URL do hello kanálu.
3. Vytvoření události nebo programu (a assetu)
4. Publikování hello prostředku a získání adresy URL pro streamování.  
5. Přehrání obsahu
6. Čištění

## <a name="prerequisites"></a>Požadavky
Hello následují požadované toocomplete hello kurzu.

* toocomplete tohoto kurzu potřebujete účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. 
  Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* Účet Media Services. toocreate účet Media Services najdete v části [vytvořit účet](media-services-portal-create-account.md).
* Webová kamera a kodér, který dokáže odesílat živý datový proud s jednou přenosovou rychlostí.

## <a name="create-a-channel"></a>Vytvoření kanálu
1. V hello [portál Azure](https://portal.azure.com/)vyberte Media Services a potom klikněte na název účtu Media Services.
2. Vyberte **Živé streamování**.
3. Vyberte **Vytvořit vlastní**. Tato možnost vám umožní vytvořit kanál, který má povolené kódování v reálném čase.
   
    ![Vytvoření kanálu](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel.png)
4. Klikněte na **Nastavení**.
   
   1. Zvolte hello **Live Encoding** typ kanálu. Tento typ Určuje, že chcete toocreate kanál, který je povolený pro kódování v reálném čase. Aby znamená hello příchozí jednou přenosovou rychlostí se odesílají toohello kanálu a překóduje se na datový proud s více přenosovými rychlostmi pomocí zadané za provozu kodér nastavení datového proudu. Další informace najdete v tématu [živé streamování využívající Azure Media Services toocreate více přenosovými rychlostmi datové proudy](media-services-manage-live-encoder-enabled-channels.md). Klikněte na tlačítko OK.
   2. Zadejte název kanálu.
   3. Kliknutím na OK dole hello úvodní obrazovka.
5. Vyberte hello **Ingestování** kartě.
   
   1. Na této stránce můžete vybrat protokol streamování. Pro hello **Live Encoding** typ kanálu, protokol platné možnosti jsou:
      
      * Fragmentovaný soubor MP4 s jednou přenosovou rychlostí (technologie Smooth Streaming)
      * RTMP s jednou přenosovou rychlostí
      * RTP (MPEG-TS): přenosový stream MPEG-2 využívající RTP.
        
        Podrobné vysvětlení jednotlivých protokolů najdete v článku [živé streamování využívající Azure Media Services toocreate více přenosovými rychlostmi datové proudy](media-services-manage-live-encoder-enabled-channels.md).
        
        Nelze změnit možnost protokol hello při hello kanál nebo jeho přidružené události nebo programy běží. Pokud požadujete různé protokoly, vytvořte samostatné kanály pro každý protokol streamování.  
   2. Omezení IP adres můžete použít na hello ingestování. 
      
       Můžete definovat hello IP adresy, které jsou povoleny tooingest video toothis kanál. Povolené IP adresy můžete zadat buď jako jednu IP adresu (např. '10.0.0.1'), rozsah IP adres pomocí IP adresy a masky podsítě s technologií CIDR (např. '10.0.0.1/22') nebo jako rozsah IP adres pomocí IP adresy a masky podsítě zapsané jako čísla oddělená tečkami (např. '10.0.0.1(255.255.252.0)').
      
       Pokud žádné IP adresy nezadáte a nedefinujete žádné pravidlo, nebude povolená žádná IP adresa. tooallow libovolné IP adresy, vytvořte pravidlo a nastavte 0.0.0.0/0.
6. Na hello **Preview** kartě, ve verzi preview hello platí omezení IP adres.
7. Na hello **kódování** zadejte předvolby kódování hello. 
   
    V současné době hello pouze systém je můžete vybrat přednastavení **výchozí 720p**. toospecify vlastní předvolby, otevřete lístek podpory společnosti Microsoft. Potom zadejte název hello hello přednastavených vytvořená. 

> [!NOTE]
> V současné době může spuštění kanálu hello trvat až too30 minut. Resetování kanálu může trvat až too5 minut.
> 
> 

Po vytvoření kanálu hello můžete kliknutím na hello kanál a vybrat **nastavení** které uvidíte konfigurace vašeho kanálů. 

Další informace najdete v tématu [živé streamování využívající Azure Media Services toocreate více přenosovými rychlostmi datové proudy](media-services-manage-live-encoder-enabled-channels.md).

## <a name="get-ingest-urls"></a>Získání ingestovaných adres URL
Po vytvoření kanálu hello lze získat ingestovaných adres URL, které poskytnete kodéru toohello za provozu. Kodér Hello používá tyto adresy URL tooinput živý datový proud.

![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)

## <a name="create-and-manage-events"></a>Vytvoření a správa událostí
### <a name="overview"></a>Přehled
Kanál, který je přidružen k událostem a programům, které umožňují toocontrol hello publikování a ukládání segmentů v živém datovém proudu. Kanály spravují události nebo programy. Hello vztah kanálů a programů se velmi podobné tootraditional médií, kde kanál obsahuje nepřetržitý datový proud obsahu a program je vymezená toosome vypršel časový limit událostí v tomto kanálu.

Můžete zadat hello počet hodin, které chcete obsah hello zaznamenávají tooretain hello události podle nastavení hello **archivačního okna** délka. Tuto hodnotu můžete nastavit v rozmezí od 5 minut tooa maximálně 25 hodin. Délka archivačního okna také určuje maximální množství času, které klienty můžete hledat zpět v čase od aktuální živé pozice hello hello. Události můžete spustit přes hello určenou dobu a obsah, který hello délky okna nevejde je vždy zahozen. Hodnota této vlastnosti také určuje, jak dlouho hello klienta můžou růst manifesty.

Každá událost je přidružena k assetu. toopublish hello událostí, je nutné vytvořit lokátor OnDemand pro hello související prostředek. Tento Lokátor vám umožní toobuild adresu URL pro streamování, kterou potom poskytnete tooyour klientů.

Kanál podporuje až toothree souběžně s události, takže si můžete vytvořit několik archivů hello stejného příchozího datového proudu. To vám umožní toopublish a archivovat různé části události podle potřeby. Například vaše firemní požadavky je tooarchive 6 hodin událostí, ale toobroadcast pouze posledních 10 minut. tooaccomplish, potřebujete, aby toocreate dva souběžně s událostí. Jedna událost je nastavit tooarchive 6 hodin hello události, ale není publikována programu hello. Hello dalších událostí je sada tooarchive 10 minut a tento program budete publikovat.

Existující programy nepoužívejte znovu pro nové události. Místo toho vytvořte a spusťte nový program pro každou jednotlivou událost.

Jakmile jsou připravené toostart Streamovat a archivovat, spusťte událost nebo program. Zastavte hello událost vždy, když chcete toostop streamování a archivaci události hello. 

obsah toodelete archivovat, zastavte a odstranit hello událost a potom odstraňte přidružený asset hello. Asset nemůžete odstranit, pokud je používána hello událostí; Nejprve je třeba odstranit Hello událostí. 

I po zastavení a odstranění události hello, hello uživatelé by byl schopný toostream archivovaný obsah jako video na vyžádání, tak dlouho, dokud neodstraníte hello asset.

Pokud chcete archivovat hello tooretain obsahu, ale není ho mít dostupný pro streamování, odstraňte Lokátor streamování hello.

### <a name="createstartstop-events"></a>Vytvoření, spuštění a zastavení událostí
Jakmile máte hello datový proud plyne do kanálu hello můžete začít hello streamování událostí tak, že vytvoříte Asset, Program a Lokátor streamování. To bude archivu datového proudu hello a nastavte jej k dispozici tooviewers prostřednictvím hello Streaming koncový bod. 

>[!NOTE]
>Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu. toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu. 

Existují dva způsoby toostart událostí: 

1. Z hello **kanál** stiskněte **živá událost** tooadd novou událost.
   
    Zadejte název události, název assetu, archivační okno a možnost šifrování.
   
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
   
    Pokud jste nechali **publikovat tento živé události teď** zaškrtnuto, budou vytvořeny události hello hello adresy URL pro publikování.
   
    Stisknutím klávesy **spustit**, vždy, když jsou připravené toostream hello událostí.
   
    Jakmile začnete hello událostí, můžete stisknout **sledovat** toostart přehrávání obsahu hello.
2. Alternativně můžete použít zástupce a stiskněte klávesu **přejděte Live** na hello tlačítko **kanál** stránky. Tím vytvoříte výchozí asset, program a lokátor streamování.
   
    událost Hello jmenuje **výchozí** a hello archivační okno bude nastaveno too8 hodin.

Můžete sledovat hello publikovaných událostí z hello **živé události** stránky. 

Pokud kliknete na tlačítko **Zrušit streamování**, zastaví se všechny živé události. 

## <a name="watch-hello-event"></a>Sledování událostí hello
toowatch hello událostí, klikněte na tlačítko **sledovat** v hello Azure portal nebo kopírování hello adresu URL streamování a použijte přehrávač dle svého výběru. 

![Vytvořeno](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-play-event.png)

Živé události automaticky převede události tooon vyžádání obsahu při zastavení.

## <a name="clean-up"></a>Vyčištění
Pokud jste dokončili streamování událostí a chcete tooclean si dříve zřízené prostředky hello, postupujte podle hello následující postup.

* Zastavte odesílání hello datového proudu z kodéru hello.
* Zastavte kanál hello. Jakmile hello kanál zastaví narůstání poplatků. Když potřebujete toostart ho znovu, bude mít hello stejnou ingestovanou adresu URL, takže nebude nutné tooreconfigure kodér.
* Koncový bod streamování, můžete zastavit, pokud chcete, aby toocontinue tooprovide hello archivu živé události jako datového proudu na vyžádání. Pokud je hello kanál v zastaveném stavu, nebudou vám narůstat poplatky.

## <a name="view-archived-content"></a>Zobrazení archivovaného obsahu
I po zastavení a odstranění události hello, hello uživatelé by byl schopný toostream archivovaný obsah jako video na vyžádání, tak dlouho, dokud neodstraníte hello asset. Asset nemůžete odstranit, pokud se používá událost; Nejprve je třeba odstranit Hello událostí. 

Vyberte prostředky, toomanage **nastavení** a klikněte na tlačítko **prostředky**.

![Prostředky](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-assets.png)

## <a name="considerations"></a>Požadavky
* V současné době hello maximální doporučená doba živé události je 8 hodin. Pokud potřebujete toorun kanál pro delší časové období, obraťte se na adrese amslived@Microsoft.com.
* Zkontrolujte, zda text hello koncový bod, ze kterého mají být toostream streamování vašeho obsahu je v hello **systémem** stavu.

## <a name="next-step"></a>Další krok
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

