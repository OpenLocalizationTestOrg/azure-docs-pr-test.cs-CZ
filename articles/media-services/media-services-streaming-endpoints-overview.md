---
title: "Přehled služby Media Services streamování Endpoint aaaAzure | Microsoft Docs"
description: "Toto téma nabízí přehled Azure Media Services koncové body streamování."
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: cfowler
editor: 
ms.assetid: 097ab5e5-24e1-4e8e-b112-be74172c2701
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: f27f590175dcc945d1d3299fc0cae5a68cfbf4e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="streaming-endpoints-overview"></a>Přehled koncových bodů streamování 

##<a name="overview"></a>Přehled

V Microsoft Azure Media Services (AMS) **koncový bod streamování** představuje streamování služba, která může poskytnout obsahu přímo tooa aplikace přehrávače klienta nebo tooa sítě pro doručování obsahu (CDN) pro další distribuci. Služba Media Services také poskytuje bezproblémovou integraci Azure CDN. výstupní datový proud Hello z StreamingEndpoint služba může být živý datový proud, video na vyžádání nebo progresivní stahování asset ve vašem účtu Media Services. Každý účet Azure Media Services obsahuje výchozí StreamingEndpoint. V rámci účtu hello se dají vytvořit další koncové body streamování. Existují dvě verze koncové body streamování, 1.0 a 2.0. Od ledna 2017 10, budou všechny nově vytvořené účty AMS obsahovat verze 2.0 **výchozí** StreamingEndpoint. Další, že přidáte účet toothis koncové body streamování bude také verze 2.0. Tato změna nemá vliv hello existující účty; stávající koncové body streamování bude verze 1.0 a může být upgradovaný tooversion 2.0. V této změně bude změny chování, fakturace a funkce (Další informace najdete v tématu hello **streamování typy a verze** části popsané dole).

Kromě toho od verze hello 2,15 (vydané v ledna 2017), Azure Media Services přidat následující vlastnosti toohello koncový bod streamování entity hello: **CdnProvider**, **CdnProfile**,  **FreeTrialEndTime**, **StreamingEndpointVersion**. Podrobný přehled o těchto vlastností najdete v části [to](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

Při vytváření účtu Azure Media Services výchozí standardní koncový bod streamování je vytvořena v hello **Zastaveno** stavu. Nelze odstranit hello výchozího koncového bodu streamování. V závislosti na hello Azure CDN dostupnosti v hello cílové oblasti, ve výchozím nastavení nově vytvořen výchozí koncový bod streamování také zahrnuje "StandardVerizon" CDN integrace zprostředkovatele. 

>[!NOTE]
>Integrace se službou Azure CDN je možné zakázat před zahájením hello koncový bod streamování.

Toto téma poskytuje přehled hello hlavní funkce, které jsou poskytovány koncové body streamování.

## <a name="streaming-types-and-versions"></a>Streamování typy a verze

### <a name="standardpremium-types-version-20"></a>Typy Standard nebo Premium (verze 2.0)

Od verze ledna 2017 hello služby Media Services, máte dva typy streamování: **standardní** a **Premium**. Tyto typy jsou součástí hello Streaming koncový bod verze "2.0".

Typ|Popis
---|---
**Standard**|Toto je výchozí možnost hello, který bude fungovat pro většinu hello scénářů hello.<br/>Pomocí této možnosti získat/omezené SLA, prvních 15 dnů po spuštění hello koncový bod streamování je zdarma.<br/>Pokud vytvoříte více než jeden koncové body streamování, pouze hello nejdřív jednu je zdarma hello prvních 15 dnů hello, které ostatní fakturují, jakmile je spuštění. <br/>Všimněte si, že bezplatné zkušební verze aplikuje jenom toonewly vytvoření účtů media services a výchozí koncový bod streamování. Stávající koncové body streamování a kromě vytvořený koncových bodů streamování není zahrnuje bezplatnou zkušební verzi období jsou i upgradovat tooversion 2.0 nebo jsou vytvořené jako verze 2.0.
**Premium**|Tato možnost je vhodná pro odborníky v oblasti scénáře, které vyžadují vyšší škálování nebo ovládací prvek.<br/>Proměnné SLA založený na premium streamování zakoupili kapacitu jednotky (SU), vyhrazené koncových bodů streamování za provozu v izolovaném prostředí a není pokouší pro prostředky.

Podrobnější informace najdete v tématu hello **porovnat streamování typy** následující části.

### <a name="classic-type-version-10"></a>Klasického typu (verze 1.0)

Uživatelé, kteří vytvořili účty AMS předchozí toohello ledna 2017 10 verze, je nutné **Classic** typ koncového bodu streamování. Tento typ je součástí hello streamování verze koncový bod "1.0".

Pokud vaše **"1.0" verzi** koncový bod streamování má > = 1 premium streamování jednotky (SU), ji budou koncový bod streamování premium a bude poskytovat všechny funkce AMS (stejně jako hello **Standard nebo Premium** typu) bez žádné další kroky konfigurace.

>[!NOTE]
>**Classic** koncové body streamování (verze "1.0" a 0 SU), poskytuje omezené funkce a neobsahuje smlouvy o úrovni služeb. Doporučujeme toomigrate příliš**standardní** zadejte tooget lepší prostředí a toouse funkce, jako jsou dynamické balení nebo šifrování a další funkce, které jsou součástí hello **standardní** typu. toomigrate toohello **standardní** typem, přejděte toohello [portál Azure](https://portal.azure.com/) a vyberte **Opt-in tooStandard**. Další informace o migraci najdete v tématu hello [migrace](#migration-between-types) části.
>
>Mějte na paměti, že tuto operaci nelze vrátit zpět a má cenovou vliv.
>
 
## <a name="comparing-streaming-types"></a>Porovnávání datových proudů typy

### <a name="versions"></a>Verze

|Typ|StreamingEndpointVersion|ScaleUnits|CDN|Fakturace|SLA| 
|--------------|----------|-----------------|-----------------|-----------------|-----------------|    
|Classic|1.0|0|Není k dispozici|Free|Není k dispozici|
|Koncový bod streamování Standard|2.0|0|Ano|Placené|Ano|
|Jednotky streamování Premium|1.0|>0|Ano|Placené|Ano|
|Jednotky streamování Premium|2.0|>0|Ano|Placené|Ano|

### <a name="features"></a>Funkce

Funkce|Standard|Premium
---|---|---
Volné prvních 15 dnů| Ano |Ne
Propustnost |Až too600 MB/s při Azure CDN se nepoužívá. Dál škáluje s CDN.|200 MB/s na jednotku (SU) pro streaming. Dál škáluje s CDN.
SLA | 99.9|99,9 (200 MB/s na SU).
CDN|Azure CDN, třetích stran CDN nebo žádné CDN.|Azure CDN, třetích stran CDN nebo žádné CDN.
Fakturace je účtovány poměrnou částí.| Denně|Denně
Dynamické šifrování|Ano|Ano
Dynamické balení|Ano|Ano
Měřítko|Automatické škálování toohello cílem propustnost.|Další jednotky streamování
IP filtrování nebo G20 nebo vlastního hostitele|Ano|Ano
Progresivní stahování|Ano|Ano
Doporučené využití |Doporučuje hello valná většina streamování scénáře.|Profesionální využití.<br/>Pokud se domníváte, že můžete mít potřeby nad rámec Standard. Pokud očekáváte souběžných cílovou skupinu velikost větší než 50 000 prohlížeče, kontaktujte nás (amsstreaming na webu společnosti Microsoft).


## <a name="migration-between-types"></a>Migrace mezi typy

Z | příliš| Akce
---|---|---
Classic|Standard|Třeba tooopt v
Classic|Premium| Škálování (Další jednotky streamování)
Standard nebo Premium|Classic|Není k dispozici (Pokud je streamování koncový bod verze 1.0. Je povolená toochange tooclassic s nastavení scaleunits příliš "0")
Standard (s/bez CDN)|Premium s hello stejné konfigurace|V hello povoleny **spuštění** stavu. (prostřednictvím portálu Azure)
Premium (s/bez CDN)|Standardní s hello stejné konfigurace|V hello povoleny **spuštění** stavu (prostřednictvím portálu Azure)
Standard (s/bez CDN)|Premium s různých konfiguračním|V hello povoleny **zastavena** stavu (prostřednictvím portálu Azure). Není povoleno v běžícím stavu hello.
Premium (s/bez CDN)|Standard s různých konfiguračním|V hello povoleny **zastavena** stavu (prostřednictvím portálu Azure). Není povoleno v běžícím stavu hello.
Verze 1.0 s SU > = 1 s CDN|Standard nebo Premium s žádné CDN|V hello povoleny **zastavena** stavu. Není povoleno v hello **spuštění** stavu.
Verze 1.0 s SU > = 1 s CDN|Standard s/bez CDN|V hello povoleny **zastavena** stavu. Není povoleno v hello **spuštění** stavu. Verze 1.0 CDN bude odstraněna a nové jeden vytvořit a spustit.
Verze 1.0 s SU > = 1 s CDN|Premium/bez CDN|V hello povoleny **zastavena** stavu. Není povoleno v hello **spuštění** stavu. Classic CDN bude odstraněna a nové jeden vytvořit a spustit.

## <a name="next-steps"></a>Další kroky
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

