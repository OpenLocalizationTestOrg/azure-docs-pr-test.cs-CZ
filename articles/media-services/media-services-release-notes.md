---
title: "Poznámky k verzi služby aaaMedia | Microsoft Docs"
description: "Poznámky k verzi služby Media Services"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3ca2d7af-1cf0-45fa-9585-3b73f3ee057d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: media
ms.devlang: dotnet
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: c365b1133987267173ec858298c4c6de62744946
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-release-notes"></a>Poznámky k verzi Azure Media Services
Tyto poznámky k verzi shrnout změny z předchozích verzí a známé problémy.

> [!NOTE]
> Chcete toohear od našich zákazníků jsme soustředit na řešení problémů, které vám. tooreport problém nebo klást otázky, položit v hello [fóru služby Azure Media Services MSDN].
> 
> 

## <a id="issues"></a>Aktuálně známé problémy
### <a id="general_issues"></a>Obecné problémy služby Media Services
| Problém | Popis |
| --- | --- |
| Několik běžných hlaviček protokolu HTTP nejsou k dispozici v hello REST API. |Pokud vyvíjíte aplikace Media Services pomocí hello REST API, zjistíte, že některé běžné pole hlavičky protokolu HTTP (včetně CLIENT-REQUEST-ID, ID žádosti a vrátit-CLIENT-REQUEST-ID) nejsou podporovány. hlavičky Hello bude přidána v budoucí aktualizaci. |
| Kódování v procentech není povoleno. |Služba Media Services použije hello hodnotu hello IAssetFile.Name vlastnost při sestavování adresy URL pro hello streamování obsahu (například http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tohoto důvodu není povoleno kódování v procentech. Hello hodnotu hello **název** vlastnost nemůže mít žádné z následujících hello [procent kódování vyhrazené znaky](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Navíc může existovat pouze jedna '. " pro hello příponu názvu souboru. |
| Hello ListBlobs metoda, která je součástí verze SDK úložiště Azure hello 3.x selže. |Služba Media Services vygeneruje SAS adresy URL založené na hello [2012-02-12](https://docs.microsoft.com/rest/api/storageservices/Version-2012-02-12) verze. Pokud chcete toouse sada SDK úložiště Azure toolist objektů BLOB v kontejneru objektů blob, použijte hello [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) metoda, která je součástí verze sady SDK úložiště Azure 2.x. Hello ListBlobs metoda, která je součástí verze SDK úložiště Azure hello 3.x selže. |
| Služba Media Services omezení mechanismus omezuje hello využití prostředků pro aplikace, které služby toohello nadměrné požadavku. Služba Hello může vrátit hello kód stavu HTTP služba není dostupná (503). |Další informace najdete v tématu hello popis stavového kódu protokolu HTTP hello 503 v hello [Azure Media Services chybové kódy](media-services-encoding-error-codes.md) tématu. |
| Při dotazování entity, existuje omezení 1000 entit vrátí najednou, protože veřejné v2 REST omezí výsledky too1000 výsledky dotazu. |Budete potřebovat toouse **přeskočit** a **trvat** (.NET) nebo **horní** (REST), jak je popsáno v [v tomto příkladu .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) a [toto rozhraní API REST Příklad](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). |
| Někteří klienti můžete setkat při opakovaném značky problém v manifestu technologie Smooth Streaming hello. |Další informace najdete v [tomto](media-services-deliver-content-overview.md#known-issues) oddílu. |
| Azure Media Services .NET SDK objektů nelze serializovat a v důsledku nefungují s ukládáním do mezipaměti Azure. |Pokud se pokusíte tooserialize hello SDK AssetCollection objekt tooadd ho tooAzure je vyvolána ukládání do mezipaměti, výjimku. |
| Kódování úlohy nezdaří řetězcem zpráva "fáze: DownloadFile. Kód: System.NullReferenceException ". |Typické kódování pracovní postup Hello je tooupload vstupní video soubory tooan vstupu prostředku a odeslání jeden nebo více úloh kódování pro tento vstupní Asset, bez další úpravy, že vstupní Asset. Pokud upravíte však hello vstupní Asset (například o přidání/odstranění/přejmenování souborů v rámci hello Asset) a potom následné úlohy se pravděpodobně nezdaří s chybou DownloadFile. alternativní řešení Hello je toodelete hello vstupní Asset a znovu odeslat vstupní soubory tooa nový prostředek. |

## <a id="rest_version_history"></a>Historie verzí rozhraní API REST
Informace o hello historie verzí Media Services REST API najdete v tématu [Azure Media Services REST API – referenční informace].

## <a name="june-2017-release"></a>2017 června verze

Služba Media Services nyní podporuje [Azure Active Directory (Azure AD)-ověřování na základě](media-services-use-aad-auth-to-access-ams-api.md).

> [!IMPORTANT]
> V současné době Media Services podporuje model ověřování služby Řízení přístupu Azure hello. Řízení přístupu autorizace však bude na 1. června 2018 zastaralá. Doporučujeme, abyste co nejdříve migraci model ověřování toohello Azure AD.

## <a name="march-2017-release"></a>2017 března verze

Teď můžete použít Azure Media Standard příliš[automaticky generovat žebříku přenosovou rychlostí](media-services-autogen-bitrate-ladder-with-mes.md) zadáním hello "Adaptivní datové proudy" přednastavení řetězec při vytvoření úlohy kódování. Pokud chcete tooencode video pro streamování pomocí služby Media Services, je "Adaptivní Streaming" doporučené přednastavených hello. Pokud potřebujete toocustomize přednastavení kódování pro konkrétní scénář, můžete začít s [tyto](media-services-mes-presets-overview.md) přednastavení.

Teď můžete použít Azure Media Standard nebo Media Encoder Premium pracovního postupu příliš[vytvořit kódování úkol, který generuje fMP4 bloky](media-services-generate-fmp4-chunks.md). 


## <a name="febuary-2017-release"></a>Febuary 2017 verze

Od 1. dubna 2017 záznam všechny úlohy ve vašem účtu, který je starší než 90 dní se automaticky odstraní, společně s jeho přidružené záznamy úloh i v případě, že hello celkový počet záznamů je nižší než maximální kvóty hello. Pokud potřebujete tooarchive hello úloh informací, můžete použít kód hello popsané [zde](media-services-dotnet-manage-entities.md).

## <a name="january-2017-release"></a>Ledna 2017 verze

V Microsoft Azure Media Services (AMS) **koncový bod streamování** představuje streamování služba, která může poskytnout obsahu přímo tooa aplikace přehrávače klienta nebo tooa sítě pro doručování obsahu (CDN) pro další distribuci. Služba Media Services také poskytuje bezproblémovou integraci Azure CDN. výstupní datový proud Hello z StreamingEndpoint služba může být živý datový proud, video na vyžádání nebo progresivní stahování asset ve vašem účtu Media Services. Každý účet Azure Media Services obsahuje výchozí StreamingEndpoint. V rámci účtu hello se dají vytvořit další koncové body streamování. Existují dvě verze koncové body streamování, 1.0 a 2.0. Od ledna 2017 10, budou všechny nově vytvořené účty AMS obsahovat verze 2.0 **výchozí** StreamingEndpoint. Další, že přidáte účet toothis koncové body streamování bude také verze 2.0. Tato změna nemá vliv hello existující účty; stávající koncové body streamování bude verze 1.0 a může být upgradovaný tooversion 2.0. V této změně bude změny chování, fakturace a funkce (Další informace najdete v tématu [to](media-services-streaming-endpoints-overview.md) tématu).

Kromě toho počínaje verzí hello 2,15, Azure Media Services přidat následující vlastnosti toohello koncový bod streamování entity hello: **CdnProvider**, **CdnProfile**, ** FreeTrialEndTime**, **StreamingEndpointVersion**. Podrobný přehled o těchto vlastností najdete v části [to](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

## <a name="december-2016-release"></a>Verze prosinec 2016

Azure Media Services nyní umožňuje tooaccess data telemetrie/metriky pro jeho služby. aktuální verze Hello AMS umožňuje shromažďovat telemetrická data pro kanál za provozu, StreamingEndpoint, a za provozu archivu entity. Další informace najdete v [tomto](media-services-telemetry-overview.md) tématu.

## <a id="july_changes16"></a>Verze července 2016
### <a name="updates-toomanifest-file-ism-generated-by-encoding-tasks"></a>Aktualizace toomanifest souboru (*. ISM) generované úlohy kódování
Odeslaná tooMedia Azure Media Encoder nebo standardu pro kodér po kódování úlohy kódování úloh hello generuje [streamování souboru manifestu](media-services-deliver-content-overview.md) (* .ism) souboru v hello výstupní Asset. S hello nejnovější verze služby je aktualizovaná syntaxe hello tento streamování souboru manifestu.

> [!NOTE]
> Syntaxe Hello hello vysílání datového proudu souboru manifestu (.ism) je vyhrazená pro interní použití a je toochange subjektu v budoucích verzích. Prosím neměňte, ani manipulaci s hello obsah tohoto souboru.
> 
> 

### <a name="a-new-client-manifest-ismc-file-is-generated-in-hello-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Nový klient manifest (*. Soubor ISMC) se generuje ve hello výstupní Asset při kódování úloh výstupy jeden nebo více souborů MP4
Počínaje hello nejnovější verze služby, po dokončení hello kódování úlohy, které generuje jeden další soubory MP4, výstupu hello že Asset bude také obsahovat streamovaná soubor manifestu (*.ismc) klienta. soubor .ismc Hello pomáhá zlepšit výkon hello dynamické streamování. 

> [!NOTE]
> Syntaxe Hello soubor manifestu (.ismc) klienta hello je vyhrazená pro interní použití a je toochange subjektu v budoucích verzích. Prosím neměňte, ani manipulaci s hello obsah tohoto souboru.
> 
> 

Další informace najdete v tématu [to](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/) blogu.

### <a name="known-issues"></a>Známé problémy
Někteří klienti můžete setkat při opakovaném značky problém v manifestu technologie Smooth Streaming hello. Další informace najdete v [tomto](media-services-deliver-content-overview.md#known-issues) oddílu.

## <a id="apr_changes16"></a>Verze. dubna 2016
### <a name="azure-media-analytics"></a>Azure Media Analytics
Vyhledávací služba Azure Media zavedli Azure Media Analytics výkonné video intelligence. Podrobné informace najdete v tématu [přehled Azure Media Services Analytics](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (Preview)
Azure Media Services nyní umožňuje toodynamically můžete šifrovat vaší HTTP Live Streaming (HLS) obsahu se Apple FairPlay. Můžete také použít AMS licence doručení služby toodeliver FairPlay licence tooclients. Podrobnější informace najdete v části [pomocí Azure Media Services tooStream HLS obsah chráněný Apple FairPlay ](media-services-protect-hls-with-fairplay.md).

## <a id="feb_changes16"></a>Verze. února 2016
Hello nejnovější verzi Azure Media Services SDK pro platformu .NET (3.5.3) obsahuje Widevine související oprava chyb. byl problém Hello: AssetDeliveryPolicy nelze znovu použít pro více prostředků, které jsou šifrované pomocí Widevine. Jako součást tato oprava chyby hello následující vlastnost byla přidána toohello SDK: **WidevineBaseLicenseAcquisitionUrl**.

    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},

    };

## <a id="jan_changes_16"></a>Verze leden 2016
Jednotky rezervované pro kódování přejmenovat tooreduce záměně se kodér názvy.

Hello Basic, Standard a Premium kódování jednotek rezervovaných jsou přejmenovat tooS1 S2, a jednotky rezervované pro S3, v uvedeném pořadí.  Zákazníci používající základní kódování RUs dnes uvidí S1 hello popisek na portálu Azure (a v faktury hello) při Standard a Premium se zobrazí popisky hello S2 a S3 v uvedeném pořadí. 

## <a id="dec_changes_15"></a>Verze prosince 2015

### <a name="azure-media-encoder-deprecation-announcement"></a>Azure Media Encoder vyřazení oznámení

Azure Media Encoder přestanou počínaje přibližně po dobu 12 měsíců od hello verze nástroje Media Encoder Standard.

### <a name="azure-sdk-for-php"></a>Sada Azure SDK for PHP
tým Azure SDK Hello publikovaná novou verzi hello [Azure SDK pro jazyk PHP](http://github.com/Azure/azure-sdk-for-php) balíček, který obsahuje aktualizací a nových funkcí pro Microsoft Azure Media Services. Konkrétně hello Azure Media Services SDK pro jazyk PHP teď podporuje hello nejnovější [obsahu ochrany](media-services-content-protection-overview.md) funkce: dynamické šifrování AES a DRM (PlayReady a Widevine) a bez omezení Token. Mimoto podporuje i škálování [kódování jednotky](media-services-dotnet-encoding-units.md).

Další informace naleznete v tématu:

* Hello [Microsoft Azure Media Services SDK pro jazyk PHP](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) blogu.
* Následující Hello [ukázky kódu](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) toohelp mohli rychle začít:
  * **vodworkflow_aes.php**: Toto je soubor PHP, který ukazuje, jak toouse dynamické šifrování AES-128 a služba pro přenos klíče. Je založen na ukázkové .NET hello podrobně [to](media-services-protect-with-aes128.md) článku.
  * **vodworkflow_aes.php**: Toto je soubor PHP, který ukazuje, jak toouse dynamického šifrování PlayReady a službu doručování licencí. Je založen na ukázkové .NET hello podrobně [to](media-services-protect-with-drm.md) článku.
  * **scale_encoding_units.php**: Toto je soubor PHP, který ukazuje, jak tooscale kódování vyhrazené jednotky.

## <a id="nov_changes_15"></a>Verze. listopadu 2015.
Azure Media Services nyní nabízí službu doručování licence Google Widevine v cloudu hello. Další podrobnosti najdete v části příliš[tomto blogu oznámení](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Další informace naleznete v [v tomto kurzu](media-services-protect-with-drm.md) a [úložiště GitHub](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Všimněte si, že služeb doručování licence Widevine poskytovaných služeb Azure Media je ve verzi preview. Další informace najdete v části [tomto blogu](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

## <a id="oct_changes_15"></a>Verze říjen 2015
Azure Media Services (AMS) je nyní za provozu v hello následujících datových centrech: Brazílie – Jih, Indie – Západ, Indie – jih a Indie – střed. Teď můžete použít hello portál Azure příliš[vytvoření účtů Media Service](media-services-portal-create-account.md) a provádění různých úloh popsaných [zde](https://azure.microsoft.com/documentation/services/media-services/). Funkce Live Encoding ale v těchto datových center není povolená. Kromě toho nejsou v těchto datových centrech dostupné všechny typy jednotek rezervovaných pro kódování.

* Brazílie – jih: Dostupné jsou jenom jednotky rezervované pro kódování typu Standard a Basic.
* Indie – západ, Indie – jih, Indie – střed: Dostupné jsou jenom jednotky rezervované pro kódování typu Basic

## <a id="september_changes_15"></a>Verze září 2015
* Teď nabízí hello možnost tooprotect AMS na vyžádání Video-On-Demand (VOD) a živé datové proudy s technologií Widevine DRM modulární. Můžete použít následující toohelp partnery služeb doručování doručování licence na Widevine hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). Další informace najdete v tématu [tomto blogu](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).
  
    Můžete použít [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (počínaje hello verzí 3.5.1) nebo REST API tooconfigure vaše AssetDeliveryConfiguration toouse Widevine.  
* AMS přidala se podpora pro Apple ProRes videa. Teď můžete nahrát QuickTime zdrojových souborech videa využívající Apple ProRes nebo jiných kodeky. Další informace najdete v tématu [tomto blogu](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).
* Teď můžete použít Media Encoder Standard toodo dílčí výstřižek a za provozu extrakci archivu. Další informace najdete v tématu [tomto blogu](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).
* byly provedeny následující filtrování aktualizací Hello: 
  
  * Teď můžete použít formát Apple HTTP Live Streaming (HLS) s filtrem jen zvukovém souboru. Tato aktualizace umožňuje sledovat pouze tooremove zadáním (pouze = false) v adrese URL hello.
  * Při definování filtrů pro vaše prostředky, máte nyní možnost toocombine více (aktuálním too3) filtry v jednu adresu URL.
    
    Další informace najdete v části [to](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.
* AMS teď podporuje I rámce v HLS v4. Podpora I rámce optimalizuje operace rychlé převíjení vpřed a zpět. Ve výchozím nastavení všechny výstupy v4 HLS obsahovat I rámce seznam stop (EXT-X-I-FRAME-STREAM-INF).
  
    Další informace najdete v části [to](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.

## <a id="august_changes_15"></a>Verze srpen 2015
* Azure Media Services SDK pro verze Java V0.8.0 a nové ukázky jsou nyní k dispozici. Další informace naleznete v tématu:
  
  * [Příspěvek blogu](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
  * [Java ukázky úložiště](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
* Azure Media Player aktualizace s podporou více zvukový stream. Další informace naleznete v tématu:
  * [Příspěvek blogu](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)

## <a id="july_changes_15"></a>Verze července 2015
* Uvedení hello obecné dostupnosti Media Encoder Standard. Další informace najdete v tématu [tomto příspěvku na blogu](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).
  
    Media Encoder Standard používá přednastavení popsané v [to](http://go.microsoft.com/fwlink/?LinkId=618336) části. Všimněte si, že při použití přednastavení pro kóduje 4k, měli byste obdržet hello **Premium** vyhrazený typ jednotky. Další informace najdete v tématu [jak tooScale kódování](media-services-scale-media-processing-overview.md).
* Za provozu v reálném čase titulky s Azure Media Services a přehrávač. Další informace najdete v tématu [tento příspěvek blogu](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/)

### <a name="media-services-net-sdk-updates"></a>Aktualizace .NET SDK služby Media Services
Azure Media Services .NET SDK je nyní verze 3.4.0.0. v této verzi byl přidán Hello následující funkce:  

* Implementovaná podpora živé archivu. Všimněte si, že nelze stáhnout asset, který obsahuje za provozu archivu.
* Implementovaná podpory pro dynamické filtry.
* Implementovaná funkce, které umožňuje uživatelům kontejner úložiště tookeep při odstraňování prostředku.
* Opravy chyb souvisejících s tooretry zásad v kanály.
* Povolit **Media Encoder Premium pracovního postupu**.

## <a id="june_changes_15"></a>Verze červen 2015
### <a name="media-services-net-sdk-updates"></a>Aktualizace .NET SDK služby Media Services
Azure Media Services .NET SDK je nyní verze 3.3.0.0. v této verzi byl přidán Hello následující funkce:  

* Podpora pro specifikaci OpenId Connect zjišťování
* Podpora pro zpracování výměny klíče na straně zprostředkovatele identity. 

Pokud používáte poskytovatele identity, která zpřístupňuje OpenID Connect zjišťování dokumentu (jako hello udělat následující: Azure Active Directory, Google, Salesforce), můžete určit, aby podpisových klíčů pro ověření tokenu JWT z tooobtain Azure Media Services OpenID connect specifikace zjišťování. 

Další informace najdete v tématu [pomocí Json webové klíče z OpenID Connect specifikace toowork zjišťování s JWT tokenu ověřování ve službě Azure Media Services](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).

## <a id="may_changes_15"></a>Verze květen 2015
Uvedení hello následující nové funkce:

* [Náhled Live Encoding pomocí služby Media Services](media-services-manage-live-encoder-enabled-channels.md)
* [Dynamické manifestu](media-services-dynamic-manifest-overview.md)
* [Náhled procesor médií Azure Media Hyperlapse](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

## <a id="april_changes_15"></a>Vydání duben 2015
### <a name="general-media-services-updates"></a>Aktualizace služby Obecné Media Services
* [Uvedení přehrávač médií Azure](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/).
* Počínaje Media Services REST 2.10, kanály, které jsou nakonfigurované tooingest protokol RTMP jsou vytvořeny pomocí primární a sekundární ingestovaných adres URL. Další informace najdete v tématu [kanál ingestování konfigurace](media-services-live-streaming-with-onprem-encoders.md#channel_input)
* Azure Media Indexer aktualizace
* Podpora pro španělštinu
* Nový formát xml konfigurace

Další informace najdete v části [tomto blogu](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

### <a name="media-services-net-sdk-updates"></a>Aktualizace .NET SDK služby Media Services
Azure Media Services .NET SDK je nyní verze 3.2.0.0.

Hello Toto jsou některé hello zákazníků, kterým čelí aktualizace:

* **Narušující změny**: změnit **TokenRestrictionTemplate.Issuer** a **TokenRestrictionTemplate.Audience** toobe typu řetězec.
* Aktualizace související toocreating opakování vlastní zásady.
* Opravy chyb souvisejících s toouploading nebo stahování souborů.
* Hello **MediaServicesCredentials** třída nyní přijímá primární a sekundární přístup řízení koncový bod tooauthenticate proti.

## <a id="march_changes_15"></a>Verze března 2015
### <a name="general-media-services-updates"></a>Aktualizace služby Obecné Media Services
* Služba Media Services nyní poskytuje integrace Azure CDN. toosupport hello integrace hello **CdnEnabled** vlastnost byla přidána příliš**StreamingEndpoint**.  **CdnEnabled** lze použít s rozhraní REST API počínaje verzí 2.9 (Další informace najdete v tématu [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint)).  **CdnEnabled** je možné pomocí .NET SDK počínaje verzí 3.1.0.2 (Další informace najdete v tématu [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint\(v=azure.10\).aspx)).
* Oznámení o **Media Encoder Premium pracovního postupu**. Další informace najdete v tématu [Představení služby Azure Media Services kódování Premium](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).

## <a id="february_changes_15"></a>Verze únor 2015
### <a name="general-media-services-updates"></a>Aktualizace služby Obecné Media Services
Media Services REST API je nyní verze 2.9. Od této verze, můžete povolit hello Azure CDN integrace s koncových bodů streamování. Další informace najdete v tématu [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

## <a id="january_changes_15"></a>Verze leden 2015
### <a name="general-media-services-updates"></a>Aktualizace služby Obecné Media Services
Oznámení o obecné dostupnosti (GA) obsahu ochrany v případě dynamického šifrování. Další informace najdete v tématu [Azure Media Services zvyšuje streamování zabezpečení s technologií obecné dostupnosti DRM](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

### <a name="media-services-net-sdk-updates"></a>Aktualizace .NET SDK služby Media Services
Azure Media Services .NET SDK je nyní verze 3.1.0.1.

Tato verze označena hello výchozí konstruktor Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate jako zastaralé. new – konstruktor Hello trvá TokenType jako argument.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


## <a id="december_changes_14"></a>Verze z prosince 2014
### <a name="general-media-services-updates"></a>Aktualizace služby Obecné Media Services
* Některé aktualizace a nové funkce byly přidány toohello Azure Indexer Media procesoru. Další informace najdete v tématu [poznámky k verzi Azure Media Indexer verze 1.1.6.7](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
* Přidat nové rozhraní API REST, která vám umožní tooupdate jednotky rezervované pro kódování: [EncodingReservedUnitType se zbytkem](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype).
* Podpora přidání CORS pro doručení klíče služby.
* Vylepšení výkonu dotazů na možnosti zásad autorizace se provádí.
* V Číně datové středisko, o hello [adresa URL doručení klíčů](https://docs.microsoft.com/rest/api/media/operations/contentkey#get_delivery_service_url) je nyní za zákazníků (stejně jako v jiných datových centrech).
* Trvání cíl automaticky přidané HLS. Při provádění živé vysílání datového proudu, HLS je vždy zabalené dynamicky. Ve výchozím nastavení služba Media Services automaticky vypočítá HLS poměr balení segmentu (FragmentsPerSegment) podle hello @keyframe, které určuje interval (KeyFrameInterval), nazývaná také jen tooas skupiny obrázky – GOP, získaná z kodéru hello za provozu. Další informace najdete v tématu [práci s Azure Media Services živým streamováním].

### <a name="media-services-net-sdk-updates"></a>Aktualizace .NET SDK služby Media Services
* [Azure Media Services .NET SDK](http://www.nuget.org/packages/windowsazure.mediaservices/) je nyní verze 3.1.0.0.
* Upgradovat hello .net SDK závislostí too.NET 4.5 Framework.
* Přidat nové rozhraní API, která umožňuje tooupdate jednotky rezervované pro kódování. Další informace najdete v tématu [aktualizace vyhrazený typ jednotky a zvýšení kódování RUs pomocí rozhraní .NET](media-services-dotnet-encoding-units.md).
* Přidání JWT (JSON Web Token) podporu pro ověření tokenu. Další informace najdete v tématu [ověření pomocí tokenu JWT v Azure Media Services a dynamickým šifrováním](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
* Přidání relativní posunutí pro BeginDate a Datumvypršení platnosti v šabloně licence PlayReady hello.

## <a id="november_changes_14"></a>Verze v listopadu 2014
* Služba Media Services nyní umožňuje tooingest funkce live Smooth Streaming (FMP4) obsahu pomocí připojení SSL. tooingest přes protokol SSL, ujistěte se, že hello tooupdate ingestování tooHTTPS adresy URL.  Všimněte si, že v současné době AMS nepodporuje SSL s vlastní domény.  Další informace o živé streamování najdete v tématu [práci s Azure Media Services živým streamováním].
* V současné době nelze ingestování RTMP živý datový proud připojení přes protokol SSL.
* Pouze proudy přes protokol SSL Pokud hello koncový bod, ze kterého doručení obsahu streamování byl vytvořen po 10. září 2014. Jsou-li u hello streamování koncové body vytvořené po 10 září adresám URL streamování, adresa URL hello obsahuje "streaming.mediaservices.windows.net" (nový formát hello). Streamování adresy URL, které obsahují "origin.mediaservices.windows.net" (starý formát hello) nepodporují SSL. Pokud je adresa URL v původním formátu hello a chcete mít toostream toobe přes protokol SSL, [vytvořit nový koncový bod streamování](media-services-portal-manage-streaming-endpoints.md). Pomocí adresy URL vytvořili podle hello nové streamování toostream koncový bod vašeho obsahu přes protokol SSL.

## <a id="october_changes_14"></a>Verze říjen 2014
### <a id="new_encoder_release"></a>Kodér verze služby Media Services
Uvedení hello novou verzi kodér médií Azure Media Services. S hello nejnovější Azure Media Encoder se účtují poplatky pro výstup GB, ale jinak nové kodér hello je funkce, které jsou kompatibilní s předchozí kodér hello. Další informace [podrobnosti o cenách na Media Services]).

### <a id="oct_sdk"></a>Služba Media Services .NET SDK
Media Services SDK pro .NET rozšíření je nyní verze 2.0.0.3.

Media Services SDK pro .NET je nyní verze 3.0.0.8.

byly provedeny následující změny Hello:

* Refaktoring tříd zásady opakování.
* Přidání uživatele agenta řetězec toohttp hlavičky žádosti.
* Přidání kroku sestavení obnovení nuget.
* Oprava scénář testy toouse x509 certifikát z úložiště.
* Ověření nastavení při aktualizaci kanálu a datových proudů end.

### <a name="new-github-repository-toohost-media-services-samples"></a>Ukázky toohost Media Services nové Githubu úložiště
Ukázky jsou umístěné v [úložiště GitHub ukázky Azure Media Services](https://github.com/Azure/Azure-Media-Services-Samples).

## <a id="september_changes_14"></a>Verze září 2014
Metadata Media Services REST je nyní verze 2.7. Další informace o nejnovější aktualizace REST hello najdete v tématu [Azure Media Services REST API – referenční informace].

Media Services SDK pro .NET je nyní verze 3.0.0.7

### <a id="sept_14_breaking_changes"></a>Nejnovější změny
* **Původ** přejmenovala příliš[StreamingEndpoint].
* Změnu hello výchozí chování při použití hello **portál Azure** tooencode a pak publikovat soubory MP4.

Dříve, při použití portálu Azure Classic toopublish hello asset videa jednoho souboru MP4 adresu URL typu SAS by se vytvořily (adresy URL SAS umožňují toodownload hello videa z úložiště objektů blob). V současné době když používáte portál Azure Classic tooencode hello a pak publikujte asset videa jednoho souboru MP4, hello generuje adresu URL body tooan Azure Media Services koncový bod streamování.  Tato změna nemá vliv videa MP4, které jsou přímo nahrané tooMedia služby a publikovány bez zakódování službou Azure Media Services.

V současné době máte hello následující dvě možnosti toosolve hello problém.

* Povolit jednotek streamování a použijte dynamické balení toostream hello .mp4 asset jako technologie smooth streaming prezentace.
* Vytvořit toodownload SAS adresa url (nebo progresivně přehrání) hello MP4. Další informace o tom, najdete v části toocreate lokátoru SAS, [doručování obsahu].

### <a id="sept_14_GA_changes"></a>Nové funkce nebo scénáře, které jsou součástí verze GA
* **Procesor médií indexer**. Další informace najdete v části [indexování mediálních souborů pomocí Azure Media Indexer].
* Hello [StreamingEndpoint] entity teď umožňuje tooadd názvy vlastních domén (hostitel).
  
    Pro toobe název vlastní domény, který používá jako název koncového bodu streamování hello Media Services budete potřebovat tooadd vlastní hostitel názvy tooyour koncový bod streamování. Pomocí názvů vlastního hostitele tooadd hello pro Media Services REST API nebo .NET SDK.
  
    použít Hello následující aspekty:
  
  * Musíte mít hello vlastnictví hello vlastní název domény.
  * Hello vlastnictví hello název domény musí být ověřený službou Azure Media Services. toovalidate hello domény, vytvořte záznam CName, který se mapuje <MediaServicesAccountId>.<parent domain> tooverifydns. < mediaservices zónu dns >. 
  * Musíte vytvořit jiný CName, který mapuje název hostitele hello vlastní hostitel název (například sports.contoso.com) tooyour Media Services StreamingEndpont společnosti (například amstest.streaming.mediaservices.windows.net).

    Další informace najdete v tématu hello **CustomHostNames** vlastnost hello [StreamingEndpoint] tématu.

### <a id="sept_14_preview_changes"></a>Nové funkce nebo scénáře, které jsou součástí verze public preview hello
* Živé streamování Preview. Další informace najdete v tématu [práci s Azure Media Services živým streamováním].
* Služba doručení klíče. Další informace najdete v tématu [pomocí dynamického šifrování AES-128 a služba pro přenos klíče].
* Dynamické šifrování AES. Další informace najdete v tématu [pomocí dynamického šifrování AES-128 a služba pro přenos klíče].
* Službu doručování licencí PlayReady. Další informace najdete v tématu [pomocí dynamického šifrování PlayReady a službu doručování licencí].
* Šifrování PlayReady dynamické. Další informace najdete v tématu [pomocí dynamického šifrování PlayReady a službu doručování licencí].
* Mediální služby šablona licence PlayReady. Další informace najdete v tématu [Media Services PlayReady licence šablony přehled].
* Streamování úložiště šifrované prostředky. Další informace najdete v tématu [obsahu šifrované streamování úložiště].

## <a id="august_changes_14"></a>Verze 2014 srpen
Při kódování prostředek, prostředek výstup vytváří po dokončení úlohy kódování hello. Dokud nebude tato verze Azure Media Services Encoder vytváří metadata o prostředcích výstup. Od této verze hello kodér také vytvoří metadata o vstupní prostředky. Další informace najdete v tématu hello [vstupu Metadata] a [výstup metadat] témata.

## <a id="july_changes_14"></a>Verze červenec 2014
Hello následující opravy chyb byly provedeny pro hello Azure Media Services Balíčkovač a modul pro šifrování:

* Pouze zvuk hraje zpět při převedení tooHTTP asset za provozu archivu živé streamování – tento vyřešený a teď se přehrávají audio a video.
* Když balení asset tooHTTP živé streamování a AES 128 bitů obálky šifrování, datové proudy hello zabalené nejsou přehrávány na zařízeních s Androidem – tato chyba byla opravena a zabalené datového proudu hello přehrává na zařízeních s Androidem, které podporují protokol HTTP Live Streaming.

## <a id="may_changes_14"></a>Verze může 2014
### <a id="may_14_changes"></a>Aktualizace služby Obecné Media Services
Teď můžete použít [dynamické balení] toostream HTTP Live Streaming (HLS) v3. toostream HLS verze 3, přidejte následující cesty ke zdroji Lokátor formátu toohello hello: * .ism/manifest(format=m3u8-aapl-v3). Další informace najdete v tématu [Nick Drouin Blog].

Dynamické balení teď také podporuje doručování HLS (v3 a v4) šifrovat pomocí PlayReady podle staticky šifrovat pomocí PlayReady technologie Smooth Streaming. Informace o tom najdete v části tooencrypt technologie Smooth Streaming pomocí technologie PlayReady, [datový proud Smooth Protecting s technologií PlayReady].

### <a name="may_14_donnet_changes"></a>Aktualizace .NET SDK služby Media Services
hello sady Media Services .NET SDK 3.0.0.5 verze jsou součástí Hello následující vylepšení:

* Vyšší rychlost a odolnosti pro nahrávání nebo stahování média prostředky.
* Vylepšení v opakování logiku a přechodná výjimek: 
  
  * Pro výjimky, které jsou způsobeny dotazování, ukládají se změny, nahrávání nebo stahování souborů byly vylepšené logiku přechodná chyba zjišťování a zkuste to znovu. 
  * Při získávání webové výjimky (například během požadavek tokenu služby ACS), si všimnete, že závažné chyby selhávají rychlejší teď.

Další informace najdete v tématu [opakujte logiku hello sady Media Services SDK pro .NET].

## <a id="april_changes_14"></a>Kodér vydání duben 2014
### <a name="april_14_enocer_changes"></a>Kodér aktualizace služby Media Services
* Přidaná podpora pro příjem souborů AVI vytvořené pomocí hello tráva Valley EDIUS nelineární editor, kde je lehce hello video komprimována pomocí tráva Valley Ústředí/HQX kodek. Další informace najdete v tématu [hello tráva Valley ohlášen EDIUS 7 vysílání datového proudu prostřednictvím cloudu].
* Byla přidána podpora pro zadání hello zásady vytváření názvů pro soubory hello vyprodukované hello kodér médií. Další informace najdete v tématu [řízení Media Service kodér výstup názvy souborů].
* Přidaná podpora pro video nebo zvuk překryvy. Další informace najdete v tématu [vytváření překryvy].
* Přidaná podpora pro ve hřbetu dohromady víc video segmentů. Další informace najdete v tématu [ve hřbetu segmenty Video].
* Chyby související s pevnou tootranscoding soubory MP4 s rychlostmi kde hello zvuk zakódování vrstva zvuk MPEG-1 (neboli MP3 3).

## <a id="jan_feb_changes_14"></a>Verze leden/února 2014
### <a name="jan_fab_14_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.1, 3.0.0.2 a 3.0.0.3
Hello změny v 3.0.0.1 a 3.0.0.2 patří:

* Opravené problémy související s toousage dotazů LINQ s příkazy OrderBy.
* Rozdělení testovací řešení v [Githubu] do jednotkové testování a testy na základě scénáře.

Další informace o změnách hello najdete v tématu: [uvolní Azure Media Services .NET SDK 3.0.0.1 a 3.0.0.2].

Hello následující změny byly provedeny v 3.0.0.3:

* Úložiště Azure závislosti toouse verze 3.0.3.0 upgradovat. 
* Vyřešený problém zpětné kompatibility 3.0. *.* uvolní. 

## <a id="december_changes_13"></a>Verze prosinci 2013
### <a name="dec_13_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.0
> [!NOTE]
> verze 3.0.x.x nejsou zpětně kompatibilní s verzemi 2.4.x.x.
> 
> 

Hello nejnovější verzi hello sada SDK služby Media Services je nyní 3.0.0.0. Můžete stáhnout nejnovější balíček hello z Nuget nebo získat hello bits z [Githubu].

Počínaje hello sady Media Services SDK verze 3.0.0.0, můžete opakovaně použít hello [Active Directory řízení přístupu Azure Service (ACS)] tokeny. Další informace najdete v tématu hello "opakovaného použití řízení služby tokeny přístupu" kapitoly hello [připojení služby tooMedia s hello sady Media Services SDK pro .NET] tématu.

### <a name="dec_13_donnet_ext_changes"></a>Rozšíření sady SDK pro .NET 2.0.0.0 služby Azure Media Services
Hello rozšíření Azure Media Services .NET SDK je sada metod rozšíření a pomocných funkcí, které se zjednoduší kódování a nastavit jej jako jednodušší toodevelop službou Azure Media Services. Můžete získat nejnovější bits hello z [rozšíření Azure Media Services .NET SDK].

## <a id="november_changes_13"></a>Verze v listopadu 2013
### <a name="nov_13_donnet_changes"></a>Azure Media Services .NET SDK změny
Od této verze, hello sady Media Services SDK pro .NET zpracovává přechodná chyba chyby, které mohou nastat při provádění volání toohello Media Services vrstvu rozhraní API REST.

## <a id="august_changes_13"></a>Verze srpen 2013
### <a name="aug_13_powershell_changes"></a>Rutiny prostředí PowerShell služby média součástí nástroje Azure Sdk
Hello následující rutiny prostředí PowerShell Media Services jsou teď součástí [nástroje azure sdk].

* Get-AzureMediaServices 
  
    Například, `Get-AzureMediaServicesAccount`.
* New-AzureMediaServicesAccount 
  
    Například, `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`.
* New-AzureMediaServicesKey 
  
    Například, `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`.
* Remove-AzureMediaServicesAccount 
  
    Například, `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`.

## <a id="june_changes_13"></a>Červen 2013 verze
### <a name="june_13_general_changes"></a>Změní Azure Media Services
změny Hello uvedených v této části jsou aktualizace součástí hello uvolní červen 2013 Media Services.

* Možnost toolink více úložiště účtů tooa účet Media Service. 
  
    StorageAccount
  
    Asset.StorageAccountName a Asset.StorageAccount
* Možnost tooupdate Job.Priority. 
* Entit a vlastnosti související s oznámení: 
  
    JobNotificationSubscription
  
    NotificationEndPoint
  
    Úloha
* Asset.Uri 
* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Změní sadu Azure Media Services .NET SDK
Hello následující změny jsou zahrnuty v červen 2013 uvolní sada SDK služby Media Services. Hello nejnovější sady Media Services SDK je dostupná na Githubu.

* Počínaje verzí hello 2.3.0.0 hello sada SDK služby Media Services podporuje propojování více tooa účty úložiště účtu Media Services. Hello následující rozhraní API tuto funkci podporovat:
  
    Hello IStorageAccount typu.
  
    Hello Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts vlastnost.
  
    Hello StorageAccount vlastnost.
  
    Hello StorageAccountName vlastnost.
  
    Další informace najdete v tématu [Správa Media Services prostředky napříč více účtů úložiště].
* Rozhraní API související s oznámení. Počínaje verzí hello 2.2.0.0 máte hello možnost toolisten tooAzure fronty úložiště oznámení. Další informace najdete v tématu [zpracování Media Services úlohy oznámení].
  
    Hello Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions vlastnost.
  
    Hello Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint typu.
  
    Hello Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription typu.
  
    Hello Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection typu.
  
    Hello Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType typu.
  
    Hello Microsoft.WindowsAzure.MediaServices.Client.NotificationJobState typu.
* Závislost na hello Azure úložiště klienta SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll).
* Závislost na protokolu OData 5.5 (Microsoft.Data.OData.dll).

## <a id="december_changes_12"></a>Verze 2012 prosinec
### <a name="dec_12_dotnet_changes"></a>Změní sadu Azure Media Services .NET SDK
* IntelliSense: Přidat chybějící dokumentace technologie Intellisense pro mnoho typů.
* Microsoft.Practices.TransientFaultHandling.Core: Opraven problém, kde hello SDK stále měl staré verze závislosti tooan tohoto sestavení. Hello teď odkazy verze 5.1.1209.1 toto sestavení sady SDK.

Řeší problémy v hello nalezen listopad 2012 SDK:

* IAsset.Locators.Count: Tento počet je nyní správně hlášené na nové rozhraní IAsset po odstranění všech lokátory.
* IAssetFile.ContentFileSize: Tato hodnota je nyní správně nastavená po nahrávaný podle IAssetFile.Upload(filepath).
* IAssetFile.ContentFileSize: Tuto vlastnost lze nyní nastavit při vytváření souboru asset. Bylo dříve jen pro čtení.
* IAssetFile.Upload(filepath): Opraven problém, kde byla tato metoda synchronní nahrávání vyvolání hello následující chybě při nahrávání asset toohello více souborů. Hello chyba "serveru se nezdařilo tooauthenticate hello požadavku. Ujistěte se, že hodnota hello autorizační hlavičky je vytvořen. správně včetně hello podpis. "
* IAssetFile.UploadAsync: Opraven problém, kde může být víc než 5 soubory současně odeslány.
* IAssetFile.UploadProgressChanged: Tato událost se teď poskytují prostřednictvím hello SDK.
* IAssetFile.DownloadAsync (string, BlobTransferClient, ILocator, CancellationToken): Toto přetížení metody je nyní k dispozici.
* IAssetFile.DownloadAsync: Opraven problém, kde může být současně staženy víc než 5 soubory.
* IAssetFile.Delete(): Opraven problém, kde volání delete může vyvolat výjimku, pokud nebyl nahrán žádný soubor pro hello IAssetFile.
* Úlohy: Byl opraven problém kde řetězení "MP4 tooSmooth datové proudy úlohy" "PlayReady ochrany úkolů" pomocí šablony úlohy by vytvořit všechny úlohy vůbec.
* EncryptionUtils.GetCertificateFromStore(): Tato metoda vyvolá už výjimka odkazu s hodnotou null z důvodu selhání tooa hledání hello certifikát založený na problémy s konfigurací certifikátu.

## <a id="november_changes_12"></a>Verze listopad 2012
Hello změny uvedených v této části byly aktualizace součástí hello listopad 2012 (verze 2.0.0.0) sady SDK. Tyto změny můžou vyžadovat žádné kódu napsaného pro hello červen 2012 preview SDK verze toobe upravené nebo přepsán.

* Prostředky
  
    IAsset.Create(assetName) je hello pouze asset vytvoření funkce. IAsset.Create už ukládání souborů jako součást volání metody hello. Použijte IAssetFile pro odesílání.
  
    Metoda IAsset.Publish Hello a hodnota výčtu AssetState.Publish hello byly odebrány z hello služby SDK. Kód, který závisí na této hodnotě musí být znovu napsané.
* FileInfo
  
    Tato třída je odebrána a nahrazena IAssetFile.
  
    IAssetFiles
  
    IAssetFile nahrazuje FileInfo a s jiným chováním. toouse, vytvoření instance objektu IAssetFiles hello, za nímž následuje nahrání souboru buď pomocí hello sady Media Services SDK nebo hello sada SDK úložiště Azure. Hello následující IAssetFile.Upload přetížení se dají použít:
  
  * IAssetFile.Upload(filePath): Synchronní metoda, která blokuje hello přístup z více vláken a se doporučuje jenom v případě, že odesílání jeden soubor.
  * IAssetFile.UploadAsync (cesta k souboru, blobTransferClient, Lokátor, cancellationToken): asynchronní metodu. Toto je hello upřednostňovaný způsob odeslání. 
    
    Známého problému: pomocí hello cancellationToken skutečně zruší hello nahrávání; Stav hello zrušení úlohy hello však může být z mnoha stavů. Musíte správně catch a zpracování výjimek.
* Lokátory
  
    byly odebrány Hello specifické pro původní verze. Hello SAS konkrétní kontext. Zastaralé, nebo budou odebrány podle Všeobecné budou označeny Locators.CreateSasLocator (asset, accessPolicy) Lokátory hello najdete v části v rámci nové funkce pro aktualizovaný chování.

## <a id="june_changes_12"></a>Verze Preview června 2012
Následující funkce Hello se nové v listopadu hello verzi hello SDK.

* Odstraňování entit
  
    IAsset, IAssetFile, ILocator, IAccessPolicy, IContentKey objekty jsou nyní odstraněny úrovni objektu hello, tj. IObject.Delete(), místo aby odstranění v kolekci, ve kterých je cloudMediaContext.ObjCollection.Delete(objInstance) hello.
* Lokátory
  
    Lokátory je teď vytvořit pomocí metody CreateLocator hello a použít hodnoty výčtu LocatorType.SAS nebo LocatorType.OnDemandOrigin hello jako argument pro konkrétní typ hello lokátoru chcete toocreate.
  
    Byly přidány nové vlastnosti tooLocators toomake je snazší tooobtain použitelné identifikátory URI pro obsah. Tato změna návrhu lokátorů byla určena tooprovide větší flexibilitu pro budoucí rozšíření třetích stran a zvýšit snadné použití pro média klientské aplikace.
* Podpora asynchronní metody
  
    Byla přidána podpora asynchronní metody tooall.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Media Services fórum MSDN]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Referenční dokumentace rozhraní API REST služby Azure Media Services]: https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference
[Podrobnosti o cenách služby Media Services]: http://azure.microsoft.com/pricing/details/media-services/
[Vstupní metadat]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[Výstup metadat]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[Doručování obsahu]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[Indexování mediálních souborů pomocí Azure Media Indexer]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[Práce s Azure Media Services živé datové proudy]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[Použití dynamické šifrování AES-128 a doručení klíče služby]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[Šifrování PlayReady dynamické a službu doručování licencí]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Přehled šablon licencování Media Services PlayReady]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[Streamování úložiště šifrovat obsah]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure portal]: https://manage.windowsazure.com
[Dynamické balení]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Blog NICK Drouin]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Ochrana datový proud Smooth s technologií PlayReady]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[Logika opakovaných pokusů v hello sady Media Services SDK pro .NET]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[Hello tráva Valley ohlášen EDIUS 7 vysílání datového proudu prostřednictvím cloudu]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[Řízení média služby kodér výstupních názvů souborů.]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[Vytváření překryvy]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[Více segmenty Video]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[Azure Media Services .NET SDK 3.0.0.1 a 3.0.0.2 verze]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Služba řízení přístupu Azure Active Directory (ACS)]: http://msdn.microsoft.com/library/hh147631.aspx
[Připojení služby tooMedia s hello sady Media Services SDK pro .NET]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Azure Media Services rozšíření sady SDK pro .NET]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[nástroje Azure sdk]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Správa Media Services prostředky napříč více účtů úložiště]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[Zpracování Media Services oznámení úlohy]: http://msdn.microsoft.com/library/azure/dn261241.aspx

