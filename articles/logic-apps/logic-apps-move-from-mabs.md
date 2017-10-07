---
title: aplikace aaaMove z tooAzure BizTalk Services Logic Apps | Microsoft Docs
description: "Přesunutí nebo migrovat Azure BizTalk Services MABS tooLogic aplikace"
services: logic-apps
documentationcenter: 
author: jonfancey
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: ladocs; jonfan; mandia
ms.openlocfilehash: b3b065b90a37002f72305b0fc866c24231fb5f9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-from-biztalk-services-toologic-apps"></a>Přesun z BizTalk Services tooLogic aplikace

Microsoft Azure BizTalk Services (MABS) jsou vyřazována z provozu. Pomocí tohoto tématu toomove vaše MABS integrace řešení tooAzure Logic Apps. 

## <a name="overview"></a>Přehled

BizTalk Services se skládá ze dvou dílčí služeb:

1.  Microsoft BizTalk Services hybridní připojení
2.  EAI a EDI na základě most integrace

Pokud se díváte toomove hybridních připojení, potom [Azure App Service hybridní připojení](../app-service/app-service-hybrid-connections.md) popisuje hello změny a funkce této služby. Azure hybridní připojení nahrazuje BizTalk Services hybridní připojení. Azure hybridní připojení se službou Azure App Service k dispozici a je k dispozici v hello portálu Azure. Azure hybridní připojení poskytuje také nové toomanage správce hybridního připojení stávající hybridní připojení služby BizTalk Services a nové hybridní připojení, které vytvoříte v hello portálu. Azure App Service hybridní připojení je všeobecně dostupná (GA).

EAI a EDI na základě most integrace Logic Apps je hello nahrazení. Služba Logic Apps poskytuje že všechny hello stejné schopnosti jako službu BizTalk Services a další. Služba Logic Apps poskytuje cloudového škálovatelného na základě spotřeby pracovního postupu a orchestraci funkce, které umožňují tooquickly a snadno vytváření složitých integrační řešení pomocí prohlížeče, nebo pomocí nástrojů v sadě Visual Studio.

Hello následující tabulka obsahuje mapování funkcí služby BizTalk Services tooLogic aplikace.

| BizTalk Services   | Logic Apps            | Účel                  |
| ------------------ | --------------------- | ---------------------------- |
| konektor          | konektor             | Odesílání a příjmu dat   |
| Most             | Aplikace logiky             | Procesor kanálu           |
| Ověření fáze     | Ověření XML akce      | Ověření dokument XML pro schéma.             |
| Zlepšit komunikaci oddělení fáze       | Tokeny dat      | Povyšte vlastnosti do zprávy nebo pro rozhodnutí o směrování             |
| Fáze transformace    | Transformace akce      | Převod XML zprávy z jednoho formátu tooanother             |
| Dekódovat fáze       | Plochý soubor dekódovat akce      | Převést z tooXML plochý soubor             |
| Kódování fáze       |  Plochý soubor kódování akce      | Převést ze souboru XML tooflat             |
| Zpráva Inspector       |  Azure Functions nebo aplikace API      | Spuštění vlastního kódu v vaše integrace             |
| Akce trasy      |  Podmínka nebo přepínače      | Směrování zprávy tooone Dobrý den zadaný konektory             |

Existuje několik různých typů artefakt ve službě BizTalk Services.

## <a name="connectors"></a>Konektory
Konektory ve službě BizTalk Services povolit mostů toosend a přijímat data, včetně obousměrný mostů, které povolené interakce založené na protokolu HTTP žádosti a odpovědi. V aplikacích logiky hello stejné terminologie se používá. Konektory v aplikacích logiky zpracovat hello stejný účel a také obsahovat více než 140, zda se může připojit tooa širokou škálu technologie a služby, jak místně pomocí hello místní brána dat (nahrazení hello služba BizTalk Adapter Service používá služba BizTalk Services), a cloudové služby SaaS a PaaS, jako je například OneDrive, Office 365, Dynamics CRM a mnoho dalších.

Zdroje ve službě BizTalk Services jsou omezené tooFTP, SFTP a frontou Service Bus nebo odběr tématu.

![](media/logic-apps-move-from-mabs/sources.png)

Každý most má koncový bod HTTP ve výchozím nastavení, která je nakonfigurovaná s hello adresu Runtime a vlastnosti relativní adresy hello mostu hello. tooachieve hello stejné s Logic Apps, použijte hello [žádostí a odpovědí](../connectors/connectors-native-reqres.md) akce.

## <a name="xml-processing-and-bridges"></a>Zpracování kódu XML a přemostění
Most ve službě BizTalk Services je obdobou tooa zpracování kanálu. Most může trvat data přijatá z konektoru a provést některé pracovat s daty hello a potom ji odešlete tooanother systému. Díky podpoře hello stejné interakce na základě kanálu vzory jako službu BizTalk Services a také poskytuje řadu dalších integrace vzorech hello stejné logiky aplikace. Hello [most požadavek-odpověď XML](https://msdn.microsoft.com/library/azure/hh689781.aspx) ve službě BizTalk Services se označuje jako VETER kanálu, který se skládá z fází, které můžou:

* (V) ověření
* (E) zlepšit komunikaci
* (T) transformace
* (E) zlepšit komunikaci
* (R) trasy

Jak je vidět v hello následující bitové kopie, zpracování hello je rozdělená mezi dotazů a odpovědí a umožňuje řídit hello požadavku a hello odpovědi cesty samostatně (například pomocí různých mapy pro každou):

![](media/logic-apps-move-from-mabs/xml-request-reply.png)

Kromě toho XML jednosměrný most přidá dekódování a kódovat fázích na hello začátek a konec zpracování a hello průchozí most obsahuje jeden úsek Enrich.

### <a name="message-processing-and-decodingencoding"></a>Zpracování a dekódování kódování zpráv
Ve službě BizTalk Services přijímat zprávy XML různých typů a určit hello odpovídající schéma pro přijata zpráva hello. To se provádí v hello **typy zpráv** fáze hello přijímat zpracování kanálu. Potom hello dekódování fázi používá hello zjištěna zpráva typu toodecode pomocí hello zadané schéma. Pokud schéma hello flatfile schéma, převede příchozí tooXML flatfile hello. 

Služba Logic Apps poskytuje podobné funkce. Můžete přijímat flatfile přes velkého množství různé protokoly, pomocí různých konektor aktivace hello (systém souborů, FTP, HTTP a tak dále) a pomocí hello [plochý soubor dekódovat](../logic-apps/logic-apps-enterprise-integration-flatfile.md) akce tooconvert hello příchozí data tooXML. Vaše existující schémata plochý soubor můžete přejít přímo toologic aplikace bez nutnosti žádné změny a pak nahrajte schémata tooyour integrace účtu.

### <a name="validation"></a>Ověření
Po hello příchozích dat převedený tooXML (nebo pokud XML byl přijat formát zprávy hello), spustí ověření toodetermine v případě, že dodržuje schématu XSD tooyour uvítací zprávu. toodo v Logic Apps, použijte hello [ověření XML](../logic-apps/logic-apps-enterprise-integration-xml-validation.md) akce. Znovu, můžete použít hello stejné schémat ze služby BizTalk Services beze změn.

### <a name="transform-messages"></a>Transformace zprávy
Ve službě BizTalk Services fáze transformace hello převede jeden tooanother formátu zprávu formátu XML. K tomu je potřeba použití mapu, pomocí hello na základě TRFM mapper. V Logic Apps je podobný procesu hello. Hello transformace akce provede mapu z vašeho účtu integrace. Hello hlavní rozdíl je, že map v Logic Apps jsou ve formátu XSLT. XSLT zahrnuje tooreuse možnost hello existující XSLT je již, včetně map vytvořených pro BizTalk serveru, které obsahují functoids. 

### <a name="routing-rules"></a>Pravidla směrování
BizTalk Services vytváří na koncový bod/konektor toosend příchozí zprávy nebo datových směrování rozhodnutí. možnost tooselect Hello, jedním z několika předem nakonfigurované koncové body je možné pomocí směrování možnost filter hello:

![](media/logic-apps-move-from-mabs/route-filter.png)

Služba Logic Apps poskytuje možnosti složitější logiku [podmínku](../logic-apps/logic-apps-use-logic-app-features.md) a [přepínač](../logic-apps/logic-apps-switch-case.md), povolení pokročilé řízení toku a směrování. Převádění filtrech směrování ve službě BizTalk Services nejlépe dosáhnete pomocí **podmínku** *Pokud* pouze dvě možnosti. Pokud je větší než dvě, použijte **přepínač**.

### <a name="enrich"></a>Zlepšit komunikaci oddělení
Hello Enrich fáze zpracování služby BizTalk Services přidá vlastnosti toohello zpráva kontextu přidruženého hello data přijatá. Například povýšení toouse vlastnost pro směrování (viz následující popis) z vyhledávání v databázi nebo extrahováním hodnotu pomocí výrazu XPath. Služba Logic Apps poskytuje přístup tooall kontextuální data výstupy z předchozích akce, takže je přehledné tooreplicate hello stejné chování. Například pomocí hello `Get Row` SQL připojení akce, můžete vrátit data z databáze systému SQL Server a použití hello data v rámci akce rozhodnutí pro směrování. Podobně vlastnosti sběrnice příchozích zpráv zařazených do fronty aktivační procedura jsou adresovatelné, jakož i pomocí hello xpath pracovní postup definice jazyka výrazu XPath.

### <a name="use-custom-code"></a>Použít vlastní kód
Služba BizTalk Services nabízí možnost hello příliš[spuštění vlastního kódu](https://msdn.microsoft.com/library/azure/dn232389.aspx) nahrát vlastní sestavení. Tato možnost je implementovaná pomocí hello [IMessageInspector](https://msdn.microsoft.com/library/microsoft.biztalk.services.imessageinspector.aspx) rozhraní. Každá fáze v hello most zahrnuje dvě vlastnosti (na zadejte Inspector a na konec Inspector), které poskytují typ formátu .net hello, kterou jste vytvořili a který toto rozhraní implementuje. Vlastní kód vám umožní tooperform složitější zpracování na hello dat, jakož i opakované použití existujícího kódu v sestavení, které provádějí běžné obchodní logiku. 

Služba Logic Apps nabízí dva způsoby, primární vlastní kód tooexecute: Azure Functions a aplikace API. Azure Functions můžete vytvořit a volání z aplikace logiky. V tématu [přidat a spuštění vlastní kód pro logic apps prostřednictvím Azure Functions](../logic-apps/logic-apps-azure-functions.md). Pomocí aplikace API, součástí Azure App Service, toocreate vlastní triggery a akce. Další informace o [vytváření vlastní toouse rozhraní API s Logic Apps](../logic-apps/logic-apps-create-api-app.md). 

Pokud máte vlastní kód v assmeblies, kterou je možné volat z BizTalk Services, můžete buď přesunout to code tooAzure funkce, nebo vytvořte vlastní rozhraní API s API Apps; v závislosti na tom, že implementace. Například pokud máte kód, který zabalí jiné službě, že Logic Apps nemá konektor, pak vytvořit aplikaci API a používejte hello akce, které vaše aplikace API poskytuje v rámci aplikace logiky. Pokud máte podpůrné funkce nebo knihovny, Azure Functions je pravděpodobně hello nejlepší.

### <a name="edi-processing-and-trading-partner-management"></a>EDI zpracování a správa obchodních partnerů
BizTalk Services zahrnuje zpracování EDI a B2B s podporou pro AS2 (použitelnosti příkaz 2), X12 a EDIFACT. Proto nemá Logic Apps. Ve službě BizTalk Services vaší vytvoření mostů EDI a vytvořit a spravovat obchodními partnery a smlouvy hello vyhrazené sledování a Správa portálu.

Tato funkce v Logic Apps je součástí hello [Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md). Tento postup se skládá z hello integrace účet a B2B akcí pro zpracování EDI a B2B. Hello [integrace účet](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) je použité toocreate a spravovat [obchodních partnerů](../logic-apps/logic-apps-enterprise-integration-partners.md) a [smlouvy](../logic-apps/logic-apps-enterprise-integration-agreements.md). Jakmile vytvoříte účet integrace, můžete přidružit jeden nebo více účtů toohello aplikace logiky. Jakmile spojen, můžete použít hello B2B akce tooaccess obchodování informace o partnerovi v rámci aplikace logiky. jsou k dispozici Hello následující akce:

* Kódování AS2
* Dekódovat AS2
* X12 kódování
* Dekódovat X12
* EDIFACT kódování
* Dekódovat EDIFACT

Na rozdíl od služby BizTalk tyto akce jsou odpojené od hello přenosové protokoly. Při vytváření aplikace logiky, takže máte větší flexibilitu, na které konektory pomocí toosend a přijímat data. Například ho je možné tooreceive X12 soubory jako přílohy e-mailu a pak zpracování těchto souborů v aplikaci logiky. 

## <a name="manage-and-monitor"></a>Správa a sledování
A správa obchodních partnerů, hello vyhrazené portál služby BizTalk Services zadaný toomonitor možnosti sledování a řešení problémů. 

Služba Logic Apps poskytuje bohatší sledování a monitorování funkcí v hello [portál Azure](../logic-apps/logic-apps-monitor-your-logic-apps.md)a s hello [Operations Management Suite B2B řešení](../logic-apps/logic-apps-monitor-b2b-message.md), včetně mobilní aplikaci pro sledování věcí Když jste na hello přesunete.

## <a name="high-availability"></a>Vysoká dostupnost
tooachieve vysokou dostupnost (HA) ve službě BizTalk Services, se používá více než jedna instance danou oblast tooshare hello zpracování zatížení. S logic apps v oblasti HA je integrovaný a dodává bez dalších poplatků. Pro zotavení po havárii na více systémů oblast pro zpracování B2B ve službě BizTalk Services se vyžaduje procesu zálohování a obnovení. V Logic Apps, mezi oblastmi aktivní/pasivní [možnost zotavení po Havárii](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md) je poskytována – což umožňuje hello synchronizace dat B2B mezi účty pro integraci v různých oblastech pro kontinuitu podnikových procesů.

## <a name="next"></a>Další
* [Co je služba Logic Apps?](logic-apps-what-are-logic-apps.md)
* [Vytvořte první aplikaci logiky](logic-apps-create-a-logic-app.md) nebo můžete rychle začít pomocí [předem připravené šablony](logic-apps-use-logic-app-templates.md).  
* [Zobrazit všechny dostupné konektory hello](../connectors/apis-list.md) můžete použít v aplikaci logiky
