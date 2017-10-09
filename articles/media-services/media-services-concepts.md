---
title: "koncepty služby Media Services aaaAzure | Microsoft Docs"
description: "Toto téma nabízí přehled Azure Media Services konceptů"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: dcefc8bc-e2ea-4b38-a643-9010f4436fb5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: juliako
ms.openlocfilehash: 0a45deff32336dfcf778552f720c1ea21927951b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-concepts"></a>Koncepty Azure Media Services
Toto téma poskytuje přehled konceptů Media Services nejdůležitější hello.

## <a id="assets"></a>Prostředky a úložiště
### <a name="assets"></a>Prostředky
[Asset](https://docs.microsoft.com/rest/api/media/operations/asset) obsahuje digitální soubory (včetně video, zvuk, obrázky, kolekci miniatur, textové stopy a soubory titulků) a hello metadata o těchto souborech. Po hello digitální soubory jsou odeslány do assetu, může být používána hello Media Services kódování a vysílání datového proudu pracovních postupů.

Prostředek je namapované tooa kontejner objektů blob v účtu Azure Storage hello a hello soubory v prostředku hello jsou uloženy jako objekty BLOB bloku v tomto kontejneru. Objekty BLOB stránky nejsou podporovány službou Azure Media Services.

Při rozhodování, jaké média obsahu tooupload a úložiště v prostředek, použít hello následující aspekty:

* Prostředek by měl obsahovat pouze jeden, jedinečné instance mediální obsah. Například jednom upravit nástroje televizního díl, videa nebo oznámení o inzerovaném programu.
* Prostředek nesmí obsahovat více interpretace nebo úpravy audiovizuálních souborů. Příkladem nesprávné použití prostředek by se pokusit toostore více než jeden díl TV, oznámení o inzerovaném programu nebo více úhlů z jediné provozní uvnitř prostředek. Ukládání více interpretace nebo úpravy audiovizuálních souborů v prostředek může mít za následek potíží při odesílání úlohy kódování, streamování a zabezpečení hello doručování asset hello později v pracovním postupu hello.  

### <a name="asset-file"></a>Souboru assetu
[AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) představuje skutečný video nebo zvuk soubor, který je uložen v kontejneru objektů blob. Soubor asset je vždy přidružena k assetu a prostředek může obsahovat jeden nebo mnoho souborů. Hello Media Services Encoder úloh selže, pokud objekt souboru asset není spojen s digitálnímu souboru v kontejneru objektů blob.

Hello **AssetFile** instance a hello samotný mediální soubor jsou dva odlišné objekty. Hello AssetFile instance obsahuje metadata o hello soubor média, zatímco soubor média hello obsahuje hello samotný mediální obsah.

Byste neměli toochange hello obsah kontejnery objektů blob, které byly vygenerovány Media Services bez použití rozhraní API služby Media.

### <a name="asset-encryption-options"></a>Možnosti šifrování Asset
V závislosti na typu hello obsahu, který má tooupload, úložiště a poskytnout, služba Media Services poskytuje různé možnosti šifrování, které lze vybírat.

>[!NOTE]
>Nepoužívá se žádné šifrování. Toto je výchozí hodnota hello. Při použití této možnosti není váš obsah chráněný během přenosu ani umístěná v úložišti.

Pokud máte v plánu toodeliver MP4 pomocí progresivního stahování, použijte tuto možnost tooupload svůj obsah.

**StorageEncrypted** – použít tuto možnost tooencrypt obsahu místně pomocí šifrování AES 256 bitů a nahrajte ho tooAzure úložiště, kde je uložený v zašifrované podobě. Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování a umístit do předchozí tooencoding systému souborů EFS a volitelně znovu zašifrovat předchozí toouploading zpět v podobě nového výstupního prostředku. Hello případem primárního použití šifrování úložiště je, pokud chcete toosecure rest souborů vysoké kvality vstupními médii pomocí silného šifrování na disku. 

Toodeliver order asset šifrované úložiště musíte nakonfigurovat zásady doručení assetu hello tak Media Services vědět, jak chcete toodeliver, obsah. Před Streamovat asset hello streamování šifrování úložiště hello odebere server a datových proudů svůj obsah pomocí hello zadat zásady pro doručení (například AES, PlayReady nebo žádné šifrování). 

**CommonEncryptionProtected** – tuto možnost použijte, pokud chcete tooencrypt (nebo nahrávání už šifrovaný) obsah běžným šifrováním nebo DRM s technologií PlayReady (například technologie Smooth Streaming chráněná pomocí DRM s technologií PlayReady).

**EnvelopeEncryptionProtected** – tuto možnost použijte, pokud chcete tooprotect (nebo odeslání, které jsou již chráněny) HTTP Live Streaming (HLS) zašifrovaná pomocí šifrování AES (Advanced Standard). Pokud odesíláte HLS již šifrováním pomocí standardu AES, se musí být šifrována pomocí Správce transformací.

### <a name="access-policy"></a>Zásady přístupu
[AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy) definuje oprávnění (např. čtení, zápisu a seznamu) a dobu trvání asset tooan přístup. By obvykle úspěšně prošel zpracováním Lokátor tooa AccessPolicy objektu, která by pak použít tooaccess hello soubory obsažené v prostředek.

>[!NOTE]
>Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy). Měli byste použít hello stejné ID zásad, pokud vždy používáte hello stejné dny / přístupová oprávnění, například zásady pro lokátory, které jsou určený tooremain zavedené po dlouhou dobu (bez odeslání zásady). Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.

### <a name="blob-container"></a>Kontejner objektů BLOB
Kontejner objektů blob zajišťuje seskupení sady objektů BLOB. Kontejnery objektů blob se ve službě Media Services používají jako hranice bod pro řízení přístupu a lokátory sdíleného přístupového podpisu (SAS) na prostředky. Účet úložiště Azure může obsahovat neomezený počet kontejnerů objektů blob. Kontejner můžete pojmout neomezený počet objektů blob.

>[!NOTE]
> Byste neměli toochange hello obsah kontejnery objektů blob, které byly vygenerovány Media Services bez použití rozhraní API služby Media.
> 
> 

### <a id="locators"></a>Lokátory
[Lokátor](https://docs.microsoft.com/rest/api/media/operations/locator)s zadejte hello vstupní bod tooaccess soubory, které jsou obsaženy v assetu. Zásady přístupu je použité toodefine hello oprávnění a doba trvání, že má klient přístup tooa zadaný prostředek. Lokátory může mít mnoho tooone relaci s zásady přístupu, tak, že jiný lokátory můžete zadat dobu různou počáteční a klientům toodifferent typy připojení při všech pomocí hello stejné oprávnění a nastavení doby trvání; z důvodu omezení zásady sdíleného přístupu nastavit pomocí služby Azure storage, ale nemůže mít více než pět jedinečný lokátory spojené s danou asset najednou. 

Služba Media Services podporuje dva typy lokátorů: OnDemandOrigin lokátory používá toostream média (například MPEG DASH, HLS nebo technologie Smooth Streaming) nebo progresivně stahovat médií a SAS adresa URL, použité tooupload nebo stažení média soubory to\from úložiště Azure. 

>[!NOTE]
>Při vytváření Lokátor OnDemandOrigin, není vhodné používat Hello seznam oprávnění (AccessPermissions.List). 

### <a name="storage-account"></a>Účet úložiště
Všechny tooAzure přístup úložiště se provádí prostřednictvím účtu úložiště. Účet Media Service můžete přidružit jeden nebo více účtů úložiště. Účet může obsahovat neomezený počet kontejnerů, tak dlouho, dokud jejich celková velikost je v části 500TB na účet úložiště.  Služba Media Services poskytuje úroveň SDK tooling tooallow toomanage můžete více účtů úložiště a zatížení vyvážit hello distribuce vaše prostředky během nahrávání toothese účtů na základě metriky nebo náhodné distribuční. Další informace najdete v tématu práci s [Azure Storage](https://msdn.microsoft.com/library/azure/dn767951.aspx). 

## <a name="jobs-and-tasks"></a>Úlohy a úkoly
A [úlohy](https://https://docs.microsoft.com/rest/api/media/operations/job) je obvykle používanými tooprocess (například index nebo kódování) jeden prezentace zvuku a videa. Při zpracování více videa, vytvořte úlohu pro každý video toobe kódování.

Úloha obsahuje metadata o zpracování hello toobe provést. Každá úloha obsahuje jeden nebo více [úloh](https://docs.microsoft.com/rest/api/media/operations/task)s určeným atomic zpracování úloh, jeho vstupní prostředky výstupní prostředky, procesor médií a jeho přidružené nastavení. Úlohy v rámci úlohy dají se propojit, kde hello výstupní asset jeden úlohy, která je zadána jako hello vstupní asset toohello další úlohy. Tímto způsobem může obsahovat jednu úlohu všechny hello zpracování, které jsou nezbytné pro prezentaci média.

## <a id="encoding"></a>Kódování
Azure Media Services poskytuje několik možností hello kódování médií v cloudu hello.

Při spouštění pomocí služby Media Services, je důležité toounderstand hello rozdíl mezi formáty kodeků a souboru.
Kodeky jsou hello software, který implementuje hello kompresi nebo dekompresi algoritmy, zatímco formáty souborů jsou kontejnery, které obsahují hello komprimované video.

Služba Media Services poskytuje dynamické balení, což vám umožní toodeliver vaše s adaptivní přenosovou rychlostí MP4 nebo technologie Smooth Streaming kódování obsahu ve formátech streamování podporovaných službou Media Services (MPEG DASH, HLS, technologie Smooth Streaming) bez nutnosti toore balíček do těchto formátů streamování.

tootake výhod [dynamické balení](media-services-dynamic-packaging-overview.md), potřebujete tooencode váš soubor mezzanine (zdrojový) do sady souborů MP4 nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí a mít alespoň jeden standard nebo premium koncového bodu streamování ve spuštěném stavu.

Služba Media Services podporuje hello následující kodéry na vyžádání, které jsou popsané v tomto článku:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Pracovní postup kodéru Media Encoder Premium](media-services-encode-asset.md#media-encoder-premium-workflow)

Informace o podporovaných kodéry najdete v tématu [kodéry](media-services-encode-asset.md).

## <a name="live-streaming"></a>Živé streamování
Ve službě Azure Media Services kanál, který představuje kanálu pro zpracování obsahu živého streamování. Kanál, který obdrží živé vstupní datové proudy v jednom ze dvou způsobů:

* Místní kodér za provozu odešle více přenosovými rychlostmi RTMP nebo technologie Smooth Streaming (fragmentovaný soubor MP4) toohello kanál. Můžete použít následující kodéry, které výstupu technologie Smooth Streaming více přenosovými rychlostmi hello: MediaExcel, Ateme, představte si komunikace, Envivio, Cisco a Elemental. Hello následující kodéry výstupu RTMP: Adobe Flash kodéru služby Live, Telestream Wirecast, Teradek, Haivision a čase kodérů. Hello ingestované datové proudy prochází kanály bez kódování a překódování žádné další. Po vyžádání Media Services doručí datový proud toocustomers hello.
* Datový proud s jednou přenosovou rychlostí (v jednom z hello následujících formátů: RTP (MPEG-TS)), RTMP nebo technologie Smooth Streaming (fragmentovaný soubor MP4)) je odeslán toohello kanálu, který je povolen tooperform za provozu, kódování pomocí služby Media Services. Hello kanál potom provede kódování v reálném čase z hello příchozí jednou přenosovou rychlostí datového proudu tooa více přenosovými rychlostmi (adaptivní) datový proud videa. Po vyžádání Media Services doručí datový proud toocustomers hello.

### <a name="channel"></a>Kanál
Ve službě Media Services [kanál](https://docs.microsoft.com/rest/api/media/operations/channel)s jsou zodpovědná za zpracování obsahu živého streamování. Kanál, který obsahuje vstupní koncový bod (ingestovanou adresu URL), pak zadejte převaděč tooa za provozu. kanál Hello přijímá živé vstupní datové proudy ze za provozu převaděč hello a zpřístupní se k vysílání datového proudu prostřednictvím jeden nebo více koncové body streamování. Kanály také poskytují preview koncový bod (URL náhledu) použít toopreview a ověřit před další zpracování a doručení datového proudu.

Hello můžete získat ingestované adresy URL a adresy URL preview hello při vytváření kanálu hello. tooget tyto adresy URL hello kanál nemá toobe ve stavu spuštění hello. Pokud jste připravené toostart nabízet data z provozu převaděč do kanálu hello, musí být spuštěná hello kanálu. Jakmile se převaděč hello za provozu spustí příjem dat, můžete zobrazit náhled datového proudu.

Každý účet Media Services může obsahovat více typů komunikačních kanálů, více programů a více koncové body streamování. V závislosti na potřebách hello šířky pásma a zabezpečení může být StreamingEndpoint služby vyhrazené tooone nebo další kanály. Všechny StreamingEndpoint můžete načítat z jakékoli kanálu.

### <a name="program-event"></a>Program (událost)
A [programu (událost)](https://docs.microsoft.com/rest/api/media/operations/program) vám umožní toocontrol hello publikování a ukládání segmentů v živém datovém proudu. Kanály spravují programy (událostí). Hello vztah kanálů a programů je podobné tootraditional médií, kde kanál obsahuje nepřetržitý datový proud obsahu a program je vymezená toosome vypršel časový limit událostí v tomto kanálu.
Můžete zadat hello počet hodin, které chcete obsah hello zaznamenávají tooretain programu hello podle nastavení hello **ArchiveWindowLength** vlastnost. Tuto hodnotu můžete nastavit v rozmezí od 5 minut tooa maximálně 25 hodin.

ArchiveWindowLength také určuje maximální množství času, které klienty můžete hledat zpět v čase od aktuální živé pozice hello hello. Programy můžou běžet po hello určenou dobu a obsah, který hello délky okna nevejde je vždy zahozen. Hodnota této vlastnosti také určuje, jak dlouho hello klienta můžou růst manifesty.

Každý program (událost) je přidružena k Assetu. program hello toopublish musí vytvořit lokátor pro hello související prostředek. Tento Lokátor vám umožní toobuild adresu URL pro streamování, kterou potom poskytnete tooyour klientů.

Kanál podporuje až toothree souběžně spuštěnými programy, takže si můžete vytvořit několik archivů hello stejného příchozího datového proudu. To vám umožní toopublish a archivovat různé části události podle potřeby. Například vaše firemní požadavky je tooarchive 6 hodin programu, ale toobroadcast pouze posledních 10 minut. tooaccomplish, je nutné toocreate dva současně spuštěné programy. Jeden program nastaven tooarchive 6 hodin hello události ale programu hello není publikována. Hello jiný program je sada tooarchive 10 minut a tento program budete publikovat.

Další informace naleznete v tématu:

* [Práce s kanály, že jsou povolené tooPerform živé kódování službou Azure Media Services](media-services-manage-live-encoder-enabled-channels.md)
* [Práce s kanály, které přijímají živé datové proudy s více přenosovými rychlostmi z místních kodérů](media-services-live-streaming-with-onprem-encoders.md)
* [Kvóty a omezení](media-services-quotas-and-limitations.md).

## <a name="protecting-content"></a>Ochrana obsahu
### <a name="dynamic-encryption"></a>Dynamické šifrování
Azure Media Services umožňuje vám toosecure médiu z hello doby, kdy opouští váš počítač prostřednictvím úložiště, zpracování a doručení. Služba Media Services umožňuje toodeliver obsah šifrován dynamicky Standard AES (Advanced Encryption) (s použitím 128bitové šifrování klíče) a běžné šifrování (CENC) pomocí PlayReady nebo Widevine DRM. Služba Media Services také poskytuje službu k doručování klíčů AES a klienty tooauthorized licence PlayReady.

V současné době můžete šifrovat hello následující streamování formáty: HLS, MPEG DASH a technologie Smooth Streaming. Nelze zašifrovat progresivní stahování.

Pokud chcete pro Media Services tooencrypt prostředek, třeba tooassociate šifrovací klíč (CommonEncryption nebo EnvelopeEncryption) s asset a taky nakonfigurovat zásady autorizace pro klíč hello.

Pokud chcete, aby toostream asset šifrované úložiště, musíte nakonfigurovat zásady doručení assetu hello v pořadí toospecify způsob toodeliver asset.

Když přehrávač vyžádá datový proud, služba Media Services použije hello zadané klíče toodynamically šifrování svůj obsah pomocí obálky šifrování (pomocí standardu AES) nebo běžné šifrování (PlayReady nebo Widevine). datový proud hello toodecrypt, hello player bude požadovat hello klíč z hello doručení klíče služby. toodecide, jestli je uživatel hello autorizovaný tooget hello klíč, hello služba vyhodnocuje hello zásady autorizace, které jste zadali pro klíč hello.

### <a name="token-restriction"></a>Omezení s tokenem
Hello zásady autorizace klíče obsahu může mít jeden nebo více omezení autorizace: otevřete, token omezení nebo omezení IP adres. zásady omezení tokenem Hello musí být doplněny tokenem vydaným podle zabezpečení tokenu služby (STS). Služba Media Services podporuje tokeny ve formátu hello jednoduché webové tokeny (SWT) a formátu JSON Web Token (JWT). Služba Media Services neposkytuje zabezpečení tokenu služby. Můžete vytvořit vlastní službu tokenů zabezpečení nebo využívat Microsoft Azure ACS tooissue tokeny. Hello služby tokenů zabezpečení musí být nakonfigurované toocreate token podepsané hello zadaný klíč a vystavování deklarací identity, které jste zadali v konfiguraci omezení s tokenem hello. Hello Media Services, který vrátí doručení klíče služby hello požadovanou klíč (nebo licencí) toohello klienta, když je platný hello token a hello deklarace identity ve hello tokenu shoda ty nakonfigurované pro klíč hello (nebo licencí).

Při konfiguraci zásady omezení tokenem hello, je nutné zadat klíč hello primární ověřování, vystavitele a cílová skupina parametry. Hello ověření primární klíč obsahuje hello klíče, že byl podepsaný hello token, vystavitele hello zabezpečené tokenu služba, která vydá hello token. Cílová skupina Hello (někdy nazývané oboru) popisuje hello záměr hello tokenu nebo token hello prostředku hello povolí přístup k. Hello doručení klíče služby Media Services ověří, že tyto hodnoty v tokenu hello shodují s hodnotami hello v šabloně hello.

Další informace najdete v tématu hello následující články:

[Ochrana obsahu přehled](media-services-content-protection-overview.md)
[chránit pomocí standardu AES-128](media-services-protect-with-aes128.md)
[chránit pomocí DRM](media-services-protect-with-drm.md)

## <a name="delivering"></a>Doručování
### <a id="dynamic_packaging"></a>Dynamické balení
Při práci se službou Media Services, doporučuje se tooencode soubory váš soubor mezzanine do MP4 adaptivní přenosovou rychlostí nastavení a pak převést hello nastavit požadovaný formát toohello pomocí hello [dynamické balení](media-services-dynamic-packaging-overview.md).

### <a name="streaming-endpoint"></a>Koncový bod streamování
Představuje streamování služba, která může poskytnout obsahu přímo tooa aplikace přehrávače klienta nebo tooa sítě pro doručování obsahu (CDN) pro další distribuci (Azure Media Services nyní poskytuje hello Azure CDN integrace.) StreamingEndpoint hello výstupní datový proud z datových proudů koncový bod služby může být živý datový proud nebo Asset videa na vyžádání ve vašem účtu Media Services. Služba Media Services zákazníci rozhodnou, že buď **standardní** streamování koncového bodu nebo jeden nebo více **Premium** koncové body, podle potřeby tootheir streamování. Koncový bod streamování Standard je vhodná pro většinu úloh streamování. 

Koncový bod streamování Standard je vhodný pro většinu streamovacích úloh. Standardní koncové body streamování vašeho obsahu toovirtually nabízejí flexibilitu toodeliver hello každé zařízení prostřednictvím dynamické balení do HLS nastavení MPEG-DASH, technologie Smooth Streaming, jakož i dynamického šifrování Microsoft PlayReady, Google Widevine, Apple Fairplay a algoritmu aes128 za pomoci.  Také škálování z velké cílové skupiny velmi malé toovery s tisíci souběžných divákům prostřednictvím integrace Azure CDN. Pokud máte pokročilé úlohy streamování požadavků na kapacitu nehodí toostandard cíle propustnost koncový bod streamování nebo chcete kapacitu hello toocontrol hello StreamingEndpoint služby toohandle rostoucí pásma musí, se doporučuje tooallocate jednotek škálování (také označované jako premium jednotek streaming).

Je doporučeno toouse dynamické balení a/nebo dynamické šifrování.

>[!NOTE]
>Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu. toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu. 

Další informace najdete v [tomto](media-services-portal-manage-streaming-endpoints.md) tématu.

Ve výchozím nastavení může obsahovat maximálně too2 koncových bodů streamování v účtu Media Services. toorequest vyšší limit, najdete v části [kvóty a omezení](media-services-quotas-and-limitations.md).

Fakturuje se jenom, když vaše StreamingEndpoint je v běžícím stavu.

### <a name="asset-delivery-policy"></a>Zásady doručení assetu
Jeden z hello kroků v hello Media Services je konfigurace pracovního postupu pro doručování obsahu [zásady doručení pro prostředky ](https://docs.microsoft.com/rest/api/media/operations/assetdeliverypolicy), které chcete toobe datového proudu. zásady doručení assetu Hello informuje Media Services, jak chcete použít pro váš asset toobe doručit: do streamování protokol, který by měl váš asset dynamicky zabalené (pro příklad, MPEG DASH, HLS, technologie Smooth Streaming nebo všechny), zda chcete toodynamically váš asset šifrovat a jak (obálky nebo common encryption).

Pokud máte úložiště šifrované prostředek, než Streamovat asset, hello streamování šifrování úložiště hello odebere server a datových proudů svůj obsah pomocí hello zadat zásady pro doručení. Například toodeliver asset šifrován Advanced Encryption (Standard AES) šifrovací klíč, sada hello zásad typ tooDynamicEnvelopeEncryption. šifrování úložiště tooremove a asset hello datový proud v hello jasné, nastavit tooNoDynamicEncryption typ zásad hello.

### <a name="progressive-download"></a>Progresivní stahování
Progresivní stahování můžete toostart přehrávání média, než byl stažen hello celý soubor. Můžete pouze progresivně stáhnout soubor MP4.

>[!NOTE]
>Šifrované prostředky musí dešifrovat, pokud chcete pro ně toobe k dispozici pro progresivní stahování.

Uživatelé tooprovide s progresivního stahování adresy URL, nejprve musíte vytvořit lokátor OnDemandOrigin. Vytvoření lokátoru hello, poskytuje hello asset toohello základní cesta. Pak musíte tooappend hello název souboru MP4. Například:

http://amstest1.Streaming.mediaservices.Windows.NET/3c5fe676-199c-4620-9B03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.MP4

### <a name="streaming-urls"></a>Adresy URL streamování
Streamování vašeho obsahu tooclients. Uživatelé tooprovide s adresami URL streamování, je nejprve nutné vytvořit lokátor OnDemandOrigin. Vytvoření lokátoru hello, poskytuje hello základní cesta toohello asset, který obsahuje hello obsah, který chcete toostream. Však možné toostream toobe tento obsah, je nutné toomodify další tuto cestu. Úplná adresa URL toohello streamování soubor manifestu, musí řetězení hello Lokátor cesty tooconstruct hodnota a hello (filename.ism) název souboru manifestu. Pak připojí /Manifest a cestu Lokátor toohello příslušný formát (v případě potřeby).

Můžete také Streamovat obsah pomocí připojení SSL. toodo, zkontrolujte, že spustíte adresy URL streamování s protokolem HTTPS. V současné době nepodporuje AMS SSL s vlastní domény.  

>[!NOTE]
>Pouze proudy přes protokol SSL Pokud hello koncový bod, ze kterého doručení obsahu streamování byl vytvořen po 10. září 2014. Jsou-li u hello streamování koncové body vytvořené po 10 září adresám URL streamování, adresa URL hello obsahuje "streaming.mediaservices.windows.net" (nový formát hello). Streamování adresy URL, které obsahují "origin.mediaservices.windows.net" (starý formát hello) nepodporují SSL. Pokud je adresa URL v původním formátu hello a chcete mít toostream toobe přes protokol SSL, vytvořte nový koncový bod streamování. Pomocí adresy URL vytvořili podle hello nové streamování toostream koncový bod vašeho obsahu přes protokol SSL.

Hello následující seznam popisuje různých formátech streamování a uvádí příklady:

* Technologie Smooth Streaming

{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest

* MPEG DASH

{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=MPD-Time-CSF)

* Apple HTTP Live Streaming (HLS) V4

{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl)

* Apple HTTP Live Streaming (HLS) V3

{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3)

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

