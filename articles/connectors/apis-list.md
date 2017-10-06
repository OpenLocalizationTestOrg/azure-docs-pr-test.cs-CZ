---
title: aaaConnectors pro Azure Logic Apps | Microsoft Docs
description: "Zvolte ze všech dostupných konektorů spravovaných společností Microsoft toobuild hello a vytvoření aplikace logiky"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: f1f1fd50-b7f9-4d13-824a-39678619aa7a
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: mandia; ladocs
ms.openlocfilehash: d681d13d642e6e1512d1f8ab0e1078a194b5da83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connectors-list"></a>Seznam konektorů
> [!TIP]
> Hello [úplný seznam A-Z](#az) (v tomto tématu) obsahuje seznam všech dostupných konektorů hello můžete použít ve vašich Logic Apps. [Podrobnosti o konektoru](/connectors/) uvádí všechny aktivační události a akce definované v hello swagger a taky seznam žádné limity pro každý konektor.

Konektory jsou nedílnou součástí vytváření aplikací logiky. Pomocí těchto konektorů, můžete skutečně rozbalte položku místní a cloudové aplikace toodo různých věcí s daty, který vytvoříte a data, která už máte. Hello konektory jsou k dispozici v hello následujících kategorií: 

* **Standardní konektory:** Jsou automaticky dostupné a zahrnuté při používání aplikací logiky. Mezi příklady patří Service Bus, Power BI, Oracle Database, OneDrive a mnoho dalších.

* **Konektory účtu pro integraci:** Jsou dostupné po zakoupení účtu pro integraci. Pomocí těchto konektorů můžete transformovat a ověřovat XML, zpracovávat zprávy typu business-to-business pomocí AS2, X12 nebo EDIFACT a kódovat a dekódovat ploché soubory. Pokud pracujete s BizTalk serveru, pak tyto konektory jsou že dobrou začlenit tooexpand vaše pracovní postupy BizTalk do Azure.  

    BizTalk Server má také [Logic Apps adaptér](https://msdn.microsoft.com/library/mt787163.aspx) přijímá od aplikace logiky a odesílání tooa aplikace logiky, který obsahuje.

* **Podnikové konektory:** Zahrnují MQ a SAP. Dostupné za další poplatek. 

[Ceny aplikace logiky](https://azure.microsoft.com/pricing/details/logic-apps/) a [cenová modelu](../logic-apps/logic-apps-pricing.md) poskytují další podrobnosti o hello náklady. 

## <a name="popular-connectors"></a>Oblíbené konektory
Pomocí těchto konektorů úspěšně zpracovávají data a informace tisíce aplikací s miliony spuštění. Hello následující tabulka uvádí hello nejoblíbenější a některých oblíbených položek s naši uživatelé:

| |  |  |  |
| --- | --- | --- | --- |
| [![Ikona rozhraní API][AzureBlobStorageicon]<br/>**Azure Blob<br/>Storage**][AzureBlobStoragedoc] | Pokud chcete, tooautomate všechny úlohy se svým účtem úložiště, pak měli podívat, tento konektor. Podporuje operace CRUD (vytvoření, čtení, aktualizace a odstranění). | [![Ikona rozhraní API][Azure-Functionsicon]<br/>**Azure Functions**][azure-functionsdoc] | Vytvořte funkce, které spouští vlastní fragmenty kódu v jazyce C# nebo Node.js, a potom tyto funkce použijte ve svých aplikacích logiky.  |
| [![Ikona rozhraní API][Dynamics-365icon]<br/>**Dynamics 365<br/>CRM Online**][Dynamics-365doc] | Jeden z hello většinu žádali konektory. Má triggery a akce toohelp automatizovat pracovní postupy s zájemců a další. | [![Ikona rozhraní API][Event-hubs-icon]<br/>**Event Hubs**][event-hubs-doc] | Zpracovávejte a publikujte události v centru událostí. Můžete třeba získat výstup z aplikace logiky pomocí služby Event Hubs a potom odešlete poskytovatel hello výstupní tooa analýzu v reálném čase. |
| [![Ikona rozhraní API][FTPicon]<br/>**FTP**][FTPdoc] | Pokud váš server FTP je přístupný z Internetu, hello, pak můžete automatizovat pracovní postupy toowork se souborů a složek. <br/><br/>Pomocí protokolu SFTP je také dostupná v konektoru SFTP hello. | [![Ikona rozhraní API][HTTPicon]<br/>**HTTP**][httpdoc] | Použijte logiku aplikace toocommunicate s žádný koncový bod přes protokol HTTP. |
| [![Ikona rozhraní API][Office-365-Outlookicon]<br/>**Office 365<br/>Outlook**][office365-outlookdoc] | Velké množství aktivační události a mnohem víc akce toouse Office 365 e-mailu a události v rámci vaše pracovní postupy. <br/><br/>Tento konektor zahrnuje *e-mailu schválení* požadavky dovolenou tooapprove akce, výdajů a tak dále. <br/><br/>Uživatelé Office 365 jsou k dispozici s konektorem hello uživatelé služeb Office 365.| [![Ikona rozhraní API][HTTP-Requesticon]<br/>**Žádost a odpověď**][HTTP-Requestdoc] | Tento konektor poskytuje adresu URL HTTPS. Když aplikace logiky hello obdrží požadavek toothis URL, spustí se aplikace logiky hello. |
| [![Ikona rozhraní API][Salesforceicon]<br/>**Salesforce**][salesforcedoc] | Snadno se přihlaste se pomocí vaší služby Salesforce účet tooget přístup tooobjects, jako je například zájemců a další. |  [![Ikona rozhraní API][Service-Busicon]<br/>**Service Bus**][Service-Busdoc] | Hello nejoblíbenější konektor v rámci logic apps, obsahuje aktivační události a zasílání zpráv asynchronní akce toodo a publikování a přihlášení k odběru s fronty, odběry a témata. |
|  [![Ikona rozhraní API][SharePointicon]<br/>**SharePoint<br/>Online**][SharePointdoc] | Pokud SharePoint k ničemu nepoužíváte a mohli byste využít automatizaci, měli byste se podívat na tento konektor. Můžete ho použít s místní službou SharePoint nebo SharePoint Online. | [![Ikona rozhraní API][SQL-Servericon]<br/>**SQL Server**][SQL-Serverdoc] | Jeden z hello nejvíc používat konektory, se může připojit tooan místní systém SQL Server a databáze SQL Azure. | 
| [![Ikona rozhraní API][Twittericon]<br/>**Twitter**][Twitterdoc] | Jednoduše se přihlaste pomocí účtu Twitteru a následně spouštějte pracovní postup, když se publikuje nový tweet. Potom uložte tyto tweetů tooa SQL database nebo seznamu služby SharePoint. | | | 

## <a name="integration-account-connectors"></a>Konektory účtu pro integraci 

Hello Enterprise Integration Pack (EIP) obsahuje konektory, které jsou dobře známé toohello komunity BizTalk Server. Když jste si koupili [integrace účet](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), získáte následující konektory hello: 

|  |  |  |  |
| --- | --- | --- | --- |
| [![Ikona rozhraní API][as2icon]<br/>**Dekódování</br> AS2**][as2decode] | [![Ikona rozhraní API][as2icon]<br/>**Kódování</br> AS2**][as2encode] | [![Ikona rozhraní API][x12icon]<br/>**Dekódování</br> EDIFACT**][EDIFACTdecode] | [![Ikona rozhraní API][x12icon]<br/>**Kódování</br> EDIFACT**][EDIFACTencode] |
[![Ikona rozhraní API][flatfileicon]<br/>**Kódování</br> plochého souboru**][flatfiledoc] | [![Ikona rozhraní API][flatfiledecodeicon]<br/>**Dekódování</br> plochého souboru**][flatfiledecodedoc] | [![Ikona rozhraní API][integrationaccounticon]<br/>**Účet<br/>pro integraci**][integrationaccountdoc] | [![Ikona rozhraní API][xmltransformicon]<br/>**Transformace<br/>XML**][xmltransformdoc] |
| [![Ikona rozhraní API][x12icon]<br/>**Dekódování</br> X12**][x12decode] | [![Ikona rozhraní API][x12icon]<br/>**Kódování</br> X12**][x12encode] | [![Ikona rozhraní API][xmlvalidateicon]<br/>**XML<br/>validace**][xmlvalidatedoc] | |

## <a name="enterprise-connectors"></a>Podnikové konektory

Připojte tooyour podnikové aplikace v rámci logic apps.

|  |  |
| --- | --- |
|[![Ikona rozhraní API][MQicon]<br/>**MQ**][mqdoc]|[![Ikona rozhraní API][SAPicon]<br/>**SAP**][sapconnector]|


## <a name="az"></a>Úplný seznam A-Z

[Podrobnosti o konektoru](/connectors/) seznamu všechny aktivační události a akce definované v hello swagger a taky seznam žádné limity pro každý konektor.

| | | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| [**1**](#1) | [**A**](#a) | [**B**](#b) | [**C**](#c) | [**D**](#d) | [**E**](#e) | [**F**](#f) | [**G**](#g) | [**H**](#h) | [**I**](#i) | [**J**](#j) | [**L**](#l) | [**M**](#m) |
| [**N**](#n) | [**O**](#o) | [**P**](#p) | [**R**](#r) | [**S**](#s) | [**T**](#t) | [**U**](#u) | [**V**](#v) | [**W**](#w) | [**X**](#x) | [**Y**](#y) | [**Z**](#z) | | 

| | |
|---|---|
|<a name="1"></a>Plánování schůzek 10to8<br/><br/><a name="a"></a>Act!<br/>Adobe Creative Cloud<br/>appFigures<br/>[AS2][as2doc]<br/>Asana<br/>Azure Active Directory (AD)<br/>Azure API Management<br/>Azure App Services<br/>Azure Application<br/>Azure Automation<br/>[Azure Blob Storage][azureblobstoragedoc]<br/>Azure Data Lake<br/>Azure DocumentDB (Cosmos DB)<br/>[Azure Functions][azure-functionsdoc]<br/>[Azure Logic Apps][nested-logic-appdoc]<br/>AzureML<br/>Fronty Azure<br/>Azure Resource Manager<br/>[Azure SQL Database][sql-serverdoc]<br/><br/><a name="b"></a>Basecamp 2<br/>Basecamp 3<br/>Batch<br/>Benchmark Email<br/>Vyhledávání pomocí služby Bing<br/>Bitbucket<br/>Bitly<br/>BizTalk Server<br/>Blogger<br/>Box<br/>Buffer<br/><br/><a name="c"></a>Calendly<br/>Campfire<br/>Capsule CRM<br/>Chatter<br/>Cognito Forms<br/>Rozhraní API služeb Cognitive Services pro počítačové zpracování obrazu<br/>Rozhraní API služeb Cognitive Services pro rozpoznávání tváře<br/>Cognitive Services LUIS<br/>Analýza textu služeb Cognitive Services<br/>Common Data Service<br/>Převod obsahu<br/>Control-Terminate<br/>[Vlastní rozhraní API / webové aplikace][api/web-appdoc]<br/><br/><a name="d"></a>Operace s daty<br/>[DB2][db2doc]<br/>Disqus<br/>DocuSign<br/>Do Until<br/>Dropbox<br/>[Dynamics 365 CRM Online][Dynamics-365doc]<br/>Dynamics 365 for Financials<br/>Dynamics 365 for Operations<br/>Dynamics NAV<br/><br/><a name="e"></a>Easy Redmine<br/>EDIFACT<br/>[Event Hubs][event-hubs-doc]<br/>Eventbrite<br/><br/><a name="f"></a>Facebook<br/>[Systém souborů][filesystemdoc]<br/>[Plochý soubor][flatfiledoc]<br/>FreshBooks<br/>Freshdesk<br/>Freshservice<br/>[FTP][ftpdoc]<br/><br/><a name="g"></a>GitHub<br/>Gmail<br/>Kalendář Google<br/>Kontakty Google<br/>Disk Google<br/>Tabulky Google<br/>Google Tasks<br/>GoToMeeting<br/>GoToTraining<br/>GoToWebinar<br/><br/><a name="h"></a>Harvest<br/>HelloSign<br/>HipChat<br/>[HTTP][httpdoc]<br/>[HTTP + Swagger][http-swaggerdoc]<br/>[HTTP Webhook][webhookdoc]<br/><br/><a name="i"></a>[Informix][informixdoc]<br/>Infusionsoft<br/>Inoreader<br/>Insightly<br/>Instagram<br/>Instapaper<br/>Účet pro integraci<br/>Intercom | <a name="j"></a>JotForm<br/>JIRA<br/><br/><a name="l"></a>LeanKit<br/>LiveChat<br/><br/><a name="m"></a>MailChimp<br/>Mandrill<br/>Střednědobé používání<br/>Microsoft Forms<br/>Microsoft Teams<br/>Microsoft Translator<br/>[MQ][mqdoc]<br/>MSN Počasí<br/>Muhimbi PDF<br/>MySQL<br/><br/><a name="n"></a>Nexmo<br/><br/><a name="o"></a>[Office 365 Outlook][office365-outlookdoc]<br/>Uživatelé Office 365<br/>Office 365 Video<br/>OneDrive<br/>OneDrive pro firmy<br/>OneNote (Business)<br/>[Oracle Database][oracle-db-doc]<br/>Outlook Customer Manager<br/>Úkoly v aplikaci Outlook<br/>Outlook.com<br/><br/><a name="p"></a>PagerDuty<br/>Parserr<br/>Paylocity<br/>Pinterest<br/>Pipedrive<br/>Pivotal Tracker<br/>Planner<br/>PostgreSQL<br/>Power BI<br/>Project Online<br/><br/><a name="r"></a>Redmine<br/>[Žádost a odpověď][http-requestdoc]<br/>RSS<br/><br/><a name="s"></a>[Salesforce][salesforcedoc]<br/>[Aplikační server SAP][sapconnector]<br/>[Server zpráv SAP][sapconnector]<br/>[Plán][recurrencedoc]<br/>Rozsah<br/>SendGrid<br/>Odeslání zprávy toobatch<br/>[Service Bus][service-busdoc]<br/>SFTP<br/>[SharePoint Online][sharepointdoc]<br/>[SharePoint Server][sharepointserver]<br/>Slack<br/>Smartsheet<br/>SMTP<br/>SparkPost<br/>[SQL Server][sql-serverdoc]<br/>Stripe<br/>SurveyMonkey<br/>Switch Case<br/><br/><a name="t"></a>Teamwork Projects<br/>Teradata<br/>Todoist<br/>Toodledo<br/>[Transformace XML][xmltransformdoc]<br/>Trello<br/>Twilio<br/>[Twitter][twitterdoc]<br/>Typeform<br/><br/><a name="u"></a>UserVoice<br/><br/><a name="v"></a>Proměnné<br/>Vimeo<br/>Visual Studio Team Services<br/><br/><a name="w"></a>WebMerge<br/>WordPress<br/>Wunderlist<br/><br/><a name="x"></a>[X12][x12doc]<br/>[Ověření XML][xmlvalidatedoc]<br/><br/><a name="y"></a>Yammer<br/>YouTube<br/><br/><a name="z"></a>Zendesk |

> [!TIP]
> tooget začít s Azure Logic Apps, ještě než si zaregistrujete účet Azure přejděte příliš[zkuste Logic Apps](https://tryappservice.azure.com/?appservice=logic). Ihned si můžete vytvořit krátkodobou úvodní aplikaci logiky. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.

## <a name="connectors-as-triggers-and-actions"></a>Konektory jako triggery a akce

**Trigger** spouští instanci aplikace logiky. Některé konektory poskytují triggery, které vaší aplikaci odesílají upozornění na konkrétní události. Konektor hello FTP má například hello `OnUpdatedFile` aktivační událost, která se spouští aplikace logiky při aktualizaci souboru. 

Aplikace logiky patří hello následující typy triggerů:  

* *Dotazování aktivační události*: tyto triggery odesílají službě v zadané četnosti toocheck pro nová data. 

    Pokud je k dispozici nová data, spustí novou instanci aplikace logiky s daty hello jako vstup. 

* *Push aktivační události*: tyto triggery naslouchají pro data v koncovém bodě nebo pro událost toohappen, pak spustí novou instanci aplikace logiky.

* *Trigger opakování:* Tento trigger vytvoří instanci aplikace logiky podle předepsaného plánu.

Konektory také poskytují **akce**, které můžete použít ve vašem pracovním postupu. Vaše aplikace logiky například může vyhledávat data pro pozdější použití. Přesněji řečeno můžete vyhledat zákaznická data z databáze SQL a potom pomocí této toobuild dat zákazníka pracovního postupu. 

> [!TIP]
> Další podrobnosti o triggerech a akcích najdete v tématu [Přehled konektorů](connectors-overview.md). 


## <a name="message-manipulation-actions"></a>Akce pro manipulaci se zprávami

Aplikace logiky zahrnují integrované akce, které můžou měnit datové části nebo s nimi manipulovat. Hello předdefinované **operace dat** konektor zahrnuje hello následující akce: 

| | |
|---|---|
| **Vytvoření** | Sestavení nebo generování hodnoty nebo objekty toouse později nebo jako můžete vytvořit pracovní postup. Můžete například vytvořit objekt JSON s hodnotami z několika kroků nebo vypočítat konstantní tooreference později v aplikace logiky spustit. |
| **Vytvoření tabulky CSV**<br/>**Vytvoření tabulky HTML** | Vytvoří ze sedy výsledků tabulku CSV nebo HTML. Například přidejte akci "Seznamu záznamů" hello CRM a přidejte filtr pro dnešní přidaných záznamů. Pošlete hello výsledky jako tabulka jazyka HTML v e-mailu. |
| **Pole filtru** (dotaz) | Filtrovat položky toohello sady výsledků, které vás zajímají. Například hledat všechny tweetů s `#Azure`, a potom "filtr" hello vrácených tweetů tooonly vráceny výsledky, které jsou `Tweeted_by_followers > 50`. |
| **Spojení** | Spojí pole s použitím nějakého oddělovače. Například hello zjistit frází klíč operace vrátí pole klíče frází. Můžete je „spojit“ s použitím oddělovače `,` nebo podobného. Takže místo `["Some", "Phrase"]` budete mít `"Some, Phrase"`. |
| **Parsování formátu JSON** | Analyzovat a přístup k hodnot z objektu JSON v Návrháři hello. Například pokud vaše Azure funkce vrátí datovou část JSON, pak můžete analyzovat jeho vlastnostech formátu JSON tooaccess hello později v jiném kroku. Akce Hello zároveň ověří, že hello odpovídá hello zadaný schématu JSON za běhu. | 
| **Výběr** | Vybere určité vlastnosti pole pro další zpracování. Pokud provedete výpis záznamů v SQL a vrátí se 15 sloupců, můžete pro další zpracování vybrat jenom některé z těchto sloupců. výstup Hello je pole, které obsahuje pouze hello vlastnosti, které vyberete. |

## <a name="custom-connectors-and-azure-certification"></a>Vlastní konektory a certifikace Azure 

toocall do rozhraní API, která nejsou k dispozici jako konektory nebo spuštění vlastního kódu, můžete rozšířit hello platformy Logic Apps pomocí [vytváření založené na REST API Apps jako vlastní konektory](../logic-apps/logic-apps-create-api-app.md). 

Pokud chcete toomake vaše vlastní aplikace API pro veřejné a k dispozici toouse v Azure, pak odeslat vaše kandidátů toohello [programu Microsoft Azure Certified](https://azure.microsoft.com/marketplace/programs/certified/logic-apps/).

## <a name="get-help"></a>Podpora

tooask otázky, odpovědi na dotazy a zjistit, jaké jiných uživatelů Azure Logic Apps dělají, přejděte toohello [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp zlepšení Azure Logic Apps a konektory, hlasovat o nebo odeslání nápadů v hello [web pro zasílání názorů uživatele Logic Apps](http://aka.ms/logicapps-wish).

Chybí tu nějaké téma věnované konektorům nebo podrobnosti, které považuje za důležité? Pokud ano, pomůže nám přidáním tooour stávajících témat nebo napsat vlastní. Naše dokumentace je typu open source a je hostovaná v GitHubu. Začněte v našem [úložišti GitHub](https://github.com/Microsoft/azure-docs). 

## <a name="next-steps"></a>Další kroky
* [Vytvoření první aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md)
* [Vytvoření vlastních rozhraní API pro aplikace logiky](../logic-apps/logic-apps-create-api-app.md)
* [Monitorování aplikací logiky](../logic-apps/logic-apps-monitor-your-logic-apps.md)

<!--Connectors Documentation-->

[api/web-appdoc]: ../logic-apps/logic-apps-custom-hosted-api.md "Integrace aplikací logiky s funkcí App Service API Apps"
[azureblobstoragedoc]: ./connectors-create-api-azureblobstorage.md "Správa souborů v kontejneru objektů blob pomocí konektoru služby Blob Storage"
[azure-functionsdoc]: ../logic-apps/logic-apps-azure-functions.md "Integrace aplikací logiky se službou Azure Functions"
[db2doc]: ./connectors-create-api-db2.md "Připojte tooIBM DB2 v hello cloudu nebo místně. Aktualizace řádku, získání tabulky a provádění dalších akcí"
[Dynamics-365doc]: ./connectors-create-api-crmonline.md "Připojit tooDynamics CRM Online, abyste mohli pracovat s daty aplikace CRM Online"
[event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Připojte tooAzure Event Hubs. Příjem a odesílání událostí mezi aplikacemi logiky a Event Hubs"
[filesystemdoc]: ../logic-apps/logic-apps-using-file-connector.md "Připojit tooan v místním systému souborů"
[ftpdoc]: ./connectors-create-api-ftp.md "Připojit tooan FTP / FTPS serveru pro úlohy, FTP, jako je odesílání, získávání, odstraňování souborů a další"
[httpdoc]: ./connectors-native-http.md "Volání protokolu HTTP s konektorem HTTP hello"
[http-requestdoc]: ./connectors-native-reqres.md "Přidejte akce pro HTTP požadavky a odpovědi tooyour logic apps"
[http-swaggerdoc]: ./connectors-native-http-swagger.md "Provádění volání HTTP pomocí konektoru HTTP + Swagger"
[informixdoc]: ./connectors-create-api-informix.md "Připojte tooInformix v hello cloudu nebo místně. Přečtěte si řádek, seznamu hello tabulek a další"
[nested-logic-appdoc]: ../logic-apps/logic-apps-http-endpoint.md "Integrace aplikací logiky s vnořenými pracovními postupy"
[office365-outlookdoc]: ./connectors-create-api-office365-outlook.md "Připojte účet tooyour Office 365. Odesílání a příjem e-mailů, správa kalendáře a kontaktů a provádění dalších akcí"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Připojit tooadd databáze Oracle tooan, vložit, odstranit řádky a další"
[mqdoc]: ./connectors-create-api-mq.md "Připojte tooMQ na pracovišti nebo v Azure a odesílat a přijímat zprávy"
[recurrencedoc]:  ./connectors-native-recurrence.md "Aktivace opakujících se akcí pro aplikace logiky"
[salesforcedoc]: ./connectors-create-api-salesforce.md "Připojte tooyour účtu Salesforce. Správa účtů, zájemců, příležitostí a provádění dalších akcí"
[sapconnector]: ../logic-apps/logic-apps-using-sap-connector.md "Připojit tooan v místním SAP systému"
[service-busdoc]: ./connectors-create-api-servicebus.md "Odesílání zpráv z front a témat služby Service Bus a příjem zpráv z front a odběrů služby Service Bus"
[sharepointdoc]: ./connectors-create-api-sharepointonline.md "Připojte tooSharePoint Online. Správa dokumentů, vypisování položek a provádění dalších akcí"
[sharepointserver]: ./connectors-create-api-sharepointserver.md "Připojte tooSharePoint na místním serveru. Správa dokumentů, vypisování položek a provádění dalších akcí"
[sql-serverdoc]: ./connectors-create-api-sqlazure.md "Připojte tooAzure SQL Database nebo SQL server. Vytváření, aktualizace, získávání a odstraňování položek v tabulce databáze SQL."
[twitterdoc]: ./connectors-create-api-twitter.md "Připojte tooTwitter. Získávání časových os, odesílání tweetů a provádění dalších akcí"
[webhookdoc]: ./connectors-native-webhook.md "Přidání aplikací logiky tooyour Webhooku pro akce a aktivační události"

[as2doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Přečtěte si víc o podnikové integraci AS2."
[x12doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Přečtěte si víc o podnikové integraci X12."
[flatfiledoc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Přečtěte si víc o plochém souboru podnikové integrace."
[flatfiledecodedoc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Přečtěte si víc o plochém souboru podnikové integrace."
[xmlvalidatedoc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Přečtěte si víc o validaci XML podnikové integrace."
[xmltransformdoc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Přečtěte si víc o transformacích podnikové integrace."
[as2decode]: ../logic-apps/logic-apps-enterprise-integration-as2-decode.md "Přečtěte si víc o dekódování pro podnikovou integraci AS2."
[as2encode]:../logic-apps/logic-apps-enterprise-integration-as2-encode.md "Přečtěte si víc o kódování pro podnikovou integraci AS2."
[X12decode]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Přečtěte si víc o dekódování pro podnikovou integraci X12."
[X12encode]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Přečtěte si víc o kódování pro podnikovou integraci X12."
[EDIFACTdecode]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Přečtěte si víc o dekódování pro podnikovou integraci EDIFACT."
[EDIFACTencode]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Přečtěte si víc o kódování pro podnikovou integraci EDIFACT."
[integrationaccountdoc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Vyhledání schémat, map, partnerů a dalších v účtu pro integraci"


[boxDoc]: ./connectors-create-api-box.md "Připojte tooBox. Nahrávání, získávání, odstraňování, vypisování souborů a provádění dalších akcí"
[delaydoc]: ./connectors-native-delay.md "Provádění zpožděných akcí"
[dropboxdoc]: ./connectors-create-api-dropbox.md "Připojte tooDropbox. Nahrávání, získávání, odstraňování, vypisování souborů a provádění dalších akcí"
[facebookdoc]: ./connectors-create-api-facebook.md "Připojte tooFacebook. Konečná časová osa tooa, získat kanálů stránek a další"
[githubdoc]: ./connectors-create-api-github.md "Připojení tooGitHub a sledování problémů"
[google-drivedoc]: ./connectors-create-api-googledrive.md "Připojit tooGoogleDrive, abyste mohli pracovat s daty"
[google-sheetsdoc]: ./connectors-create-api-googlesheet.md "Připojte tooGoogle listy, můžete můžete upravit své"
[google-tasksdoc]: ./connectors-create-api-googletasks.md "Připojí tooGoogle úlohy lze tedy spravovat vaše úkoly"
[google-calendardoc]: ./connectors-create-api-googlecalendar.md "Připojí tooGoogle kalendář a kalendář můžete spravovat."
[http-responsedoc]: ./connectors-native-reqres.md "Přidejte akce pro HTTP požadavky a odpovědi tooyour logic apps"
[instagramdoc]: ./connectors-create-api-instagram.md "Připojte tooInstagram. Aktivování nebo reakce na události"
[mailchimpdoc]: ./connectors-create-api-mailchimp.md "Připojte tooyour MailChimp účet. Správa a automatizace e-mailů"
[mandrilldoc]: ./connectors-create-api-mandrill.md "Připojit tooMandrill pro komunikaci"
[microsoft-translatordoc]: ./connectors-create-api-microsofttranslator.md "Připojte tooMicrosoft překladač. Překlad textu, zjišťování jazyků a provádění dalších akcí" 
[office365-usersdoc]: ./connectors-create-api-office365-users.md 
[office365-videodoc]: ./connectors-create-api-office365-video.md "Získání informací o videu, seznamů a kanálů videí a adres URL pro přehrávání videí Office 365"
[onedrivedoc]: ./connectors-create-api-onedrive.md "Připojit tooyour osobní OneDrive společnosti Microsoft. Nahrávání, odstraňování, vypisování souborů a provádění dalších akcí"
[onedrive-for-businessdoc]: ./connectors-create-api-onedriveforbusiness.md "Připojte tooyour obchodní Microsoft OneDrive. Nahrávání, odstraňování, vypisování souborů a provádění dalších akcí"
[outlook.comdoc]: ./connectors-create-api-outlook.md "Připojte poštovní schránce Outlooku tooyour. Správa e-mailů, kalendářů, kontaktů a provádění dalších akcí"
[project-onlinedoc]: ./connectors-create-api-projectonline.md "Připojte tooMicrosoft Projectu Online. Správa projektů, úloh, prostředků a provádění dalších akcí"
[querydoc]: ./connectors-native-query.md "Vyberte a filtrovat pole s akcí dotazu hello"
[rssdoc]: ./connectors-create-api-rss.md "Publikovat a načítat položky informačních kanálů, spouštět operace, když je nová položka publikovaných tooan RSS informačního kanálu."
[sendgriddoc]: ./connectors-create-api-sendgrid.md "Připojte tooSendGrid. Odesílání e-mailů a správa seznamů příjemců"
[sftpdoc]: ./connectors-create-api-sftp.md "Připojte účet pomocí protokolu SFTP tooyour. Nahrávání, získávání, odstraňování souborů a provádění dalších akcí"
[slackdoc]: ./connectors-create-api-slack.md "Připojení tooSlack a odeslání zprávy tooSlack kanály"
[smtpdoc]: ./connectors-create-api-smtp.md "Připojení serveru SMTP tooa a odesílání e-mailů s přílohami"
[sparkpostdoc]: ./connectors-create-api-sparkpost.md "Připojí tooSparkPost pro komunikaci"
[trellodoc]: ./connectors-create-api-trello.md "Připojte tooTrello. Správa projektů a organizace čehokoli s kýmkoli"
[twiliodoc]: ./connectors-create-api-twilio.md "Připojte tooTwilio. Odesílání a získávání zpráv, získávání dostupných čísel, správa příchozích hovorů z telefonních čísel a provádění dalších akcí"
[wunderlistdoc]: ./connectors-create-api-wunderlist.md "Připojte tooWunderlist. Správa úkolů a seznamů úkolů, udržování synchronizace všeho, co potřebujete k životu, a provádění dalších akcí"
[yammerdoc]: ./connectors-create-api-yammer.md "Připojte tooYammer. Odesílání zpráv, získávání nových zpráv a provádění dalších akcí"
[youtubedoc]: ./connectors-create-api-youtube.md "Připojte tooYouTube. Správa videí a kanálů"


<!--Icon references-->
[appFiguresicon]: ./media/apis-list/appfigures.png
[Asanaicon]: ./media/apis-list/asana.png
[Azure-Automation-icon]: ./media/apis-list/azure-automation.png
[AzureBlobStorageicon]: ./media/apis-list/azureblob.png
[Azure-Data-Lake-icon]: ./media/apis-list/azure-data-lake.png
[Azure-DocumentDBicon]: ./media/apis-list/azure-documentdb.png
[Azure-MLicon]: ./media/apis-list/azureml.png
[Azure-Resource-Manager-icon]: ./media/apis-list/azure-resource-manager.png
[Azure-Queues-icon]: ./media/apis-list/azure-queues.png
[Basecamp-3icon]: ./media/apis-list/basecamp.png
[Bitbucket-icon]: ./media/apis-list/bitbucket.png
[Bitlyicon]: ./media/apis-list/bitly.png
[BizTalk-Servericon]: ./media/apis-list/biztalk.png
[Bloggericon]: ./media/apis-list/blogger.png
[Boxicon]: ./media/apis-list/box.png
[Campfireicon]: ./media/apis-list/campfire.png
[Cognitive-Services-Text-Analyticsicon]: ./media/apis-list/cognitiveservicestextanalytics.png
[DB2icon]: ./media/apis-list/db2.png
[Dropboxicon]: ./media/apis-list/dropbox.png
[Dynamics-365icon]: ./media/apis-list/dynamicscrmonline.png
[Dynamics-365-for-Financialsicon]: ./media/apis-list/madeira.png
[Dynamics-365-for-Operationsicon]: ./media/apis-list/dynamicsax.png
[Easy-Redmineicon]: ./media/apis-list/easyredmine.png
[Event-Hubs-icon]: ./media/apis-list/eventhubs.png
[Facebookicon]: ./media/apis-list/facebook.png
[FTPicon]: ./media/apis-list/ftp.png
[GitHubicon]: ./media/apis-list/github.png
[Google-Calendaricon]: ./media/apis-list/googlecalendar.png
[Google-Driveicon]: ./media/apis-list/googledrive.png
[Google-Sheetsicon]: ./media/apis-list/googlesheet.png
[Google-Tasksicon]: ./media/apis-list/googletasks.png
[HideKeyicon]: ./media/apis-list/hidekey.png
[HipChaticon]: ./media/apis-list/hipchat.png
[Informixicon]: ./media/apis-list/informix.png
[Insightlyicon]: ./media/apis-list/insightly.png
[Instagramicon]: ./media/apis-list/instagram.png
[Instapapericon]: ./media/apis-list/instapaper.png
[JIRAicon]: ./media/apis-list/jira.png
[MailChimpicon]: ./media/apis-list/mailchimp.png
[Mandrillicon]: ./media/apis-list/mandrill.png
[Microsoft-Translatoricon]: ./media/apis-list/microsofttranslator.png
[MQicon]: ./media/apis-list/mq.png
[Office-365-Outlookicon]: ./media/apis-list/office365.png
[Office-365-Usersicon]: ./media/apis-list/office365users.png
[Office-365-Videoicon]: ./media/apis-list/office365video.png
[OneDriveicon]: ./media/apis-list/onedrive.png
[OneDrive-for-Businessicon]: ./media/apis-list/onedriveforbusiness.png
[Oracle-DB-icon]: ./media/apis-list/oracle-db.png
[Outlook.comicon]: ./media/apis-list/outlook.png
[PagerDutyicon]: ./media/apis-list/pagerduty.png
[Pinteresticon]: ./media/apis-list/pinterest.png
[Project-Onlineicon]: ./media/apis-list/projectonline.png
[Redmineicon]: ./media/apis-list/redmine.png
[RSSicon]: ./media/apis-list/rss.png
[Common-Data-Serviceicon]: ./media/apis-list/runtimeservice.png
[Salesforceicon]: ./media/apis-list/salesforce.png
[SAPicon]: ./media/apis-list/sap.png
[SendGridicon]: ./media/apis-list/sendgrid.png
[Service-Busicon]: ./media/apis-list/servicebus.png
[SFTPicon]: ./media/apis-list/sftp.png
[SharePointicon]: ./media/apis-list/sharepointonline.png
[Slackicon]: ./media/apis-list/slack.png
[Smartsheeticon]: ./media/apis-list/smartsheet.png
[SMTPicon]: ./media/apis-list/smtp.png
[SparkPosticon]: ./media/apis-list/sparkpost.png
[SQL-Servericon]: ./media/apis-list/sql.png
[Todoisticon]: ./media/apis-list/todoist.png
[Trelloicon]: ./media/apis-list/trello.png
[Twilioicon]: ./media/apis-list/twilio.png
[Twittericon]: ./media/apis-list/twitter.png
[Vimeoicon]: ./media/apis-list/vimeo.png
[Visual-Studio-Team-Servicesicon]: ./media/apis-list/visualstudioteamservices.png
[WordPressicon]: ./media/apis-list/wordpress.png
[Wunderlisticon]: ./media/apis-list/wunderlist.png
[Yammericon]: ./media/apis-list/yammer.png
[YouTubeicon]: ./media/apis-list/youtube.png

<!-- Primitive Icons -->
[API/Web-Appicon]: ./media/apis-list/api.png
[Azure-Functionsicon]: ./media/apis-list/function.png
[Delayicon]: ./media/apis-list/delay.png
[FileSystemIcon]: ./media/apis-list/filesystem.png
[HTTPicon]: ./media/apis-list/http.png
[HTTP-Requesticon]: ./media/apis-list/request.png
[HTTP-Responseicon]: ./media/apis-list/response.png
[HTTP-Swaggericon]: ./media/apis-list/http_swagger.png
[Nested-Logic-Appicon]: ./media/apis-list/workflow.png
[Recurrenceicon]: ./media/apis-list/recurrence.png
[Queryicon]: ./media/apis-list/query.png
[Webhookicon]: ./media/apis-list/webhook.png

<!-- EIP Icons -->
[as2icon]: ./media/apis-list/as2.png
[x12icon]: ./media/apis-list/x12new.png
[flatfileicon]: ./media/apis-list/flatfileencoding.png
[flatfiledecodeicon]: ./media/apis-list/flatfiledecoding.png
[xmlvalidateicon]: ./media/apis-list/xmlvalidation.png
[xmltransformicon]: ./media/apis-list/xsltransform.png
[integrationaccounticon]: ./media/apis-list/integrationaccount.png
