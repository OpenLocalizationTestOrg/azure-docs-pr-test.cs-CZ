---
title: "aaaExamples & běžné scénáře - Azure Logic Apps | Microsoft Docs"
description: "Další informace o aplikacích logiky s příklady scénářů a kurzy"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: e06311bc-29eb-49df-9273-1f05bbb2395c
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/9/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 17caa8539ec6a57726b9c6c07a71fb74caa07ccb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="examples-and-common-scenarios-for-azure-logic-apps"></a>Příklady a běžné scénáře pro Azure Logic Apps

toohelp, které další informace o hello mnoho vzory a možnosti v Azure Logic Apps, tady jsou běžných příkladů a scénářů.

## <a name="key-scenarios-for-logic-apps"></a>Hlavní scénáře pro logic apps

Azure Logic Apps poskytuje orchestraci odolný a integrace pro různé služby. Hello služba Logic Apps je "bez serveru", takže nemáte tooworry o škálování nebo instancí – všechny máte toodo definovat hello pracovního postupu (aktivační události a akce). základní platformu Hello zpracovává škálování, dostupnosti a výkonu. Každý scénář, kde je nutné toocoordinate více akcí, zejména mezi několika systémy, je případ použití skvělá pro Azure Logic Apps. Tady jsou některé příklady a vzory.

## <a name="respond-tootriggers-and-extend-actions"></a>Odpověď tootriggers a rozšířit akce

Všechny aplikace logiky začíná aktivační událost. Například pracovní postup můžete spustit v plánu události, ruční volání nebo události z externího systému, jako například hello "Pokud je soubor přidán tooan FTP server" aktivační události. Aplikace logiky Azure aktuálně podporuje více než 100 konektory připravené k použití, od místní SAP tooMicrosoft kognitivní služby. Pro systémy a služby, které nemusí být publikovány konektory můžete také rozšířit aplikace logiky.

* [Vytvořit vlastní aktivační události nebo akce](../logic-apps/logic-apps-create-api-app.md)
* [Nastavit dlouhotrvající akce pro spuštění pracovního postupu](../logic-apps/logic-apps-create-api-app.md)
* [Odpovědět, tooexternal události a akce s webhooky](../logic-apps/logic-apps-create-api-app.md)
* [Volání, aktivaci nebo vnořit pracovních s požadavky tooHTTP synchronní odpovědi](../logic-apps/logic-apps-http-endpoint.md)
* [Kurz: Odpovídají webhooky tooTwilio SMS a odeslání text odpovědi](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)
* [Kurz: Sestavení používá technologii AI sociálních řídicím v minutách s Logic Apps a Power BI](http://aka.ms/logicappsdemo)

## <a name="error-handling-logging-and-control-flow-capabilities"></a>Zpracování chyb, protokolování a funkcí toku řízení

Aplikace logiky patří bohaté možnosti pro pokročilé řízení toku, jako jsou podmínky, přepínače, smyčky a oborů. tooensure odolné řešení, můžete také implementovat chyb a zpracování výjimek v vaše pracovní postupy. Azure Logic Apps pro oznámení a diagnostické protokoly pro pracovní postup spustit stavu, poskytuje také monitorování a výstrahy.

* [Provádění různých akcí s příkazech switch](../logic-apps/logic-apps-switch-case.md)
* [Proces položky v poli a kolekce s smyčky a listy v aplikacích logiky](../logic-apps/logic-apps-loops-and-scopes.md)
* [Chyba autora a zpracování výjimek v pracovním postupu](../logic-apps/logic-apps-exception-handling.md)
* [Zapnout sledování, protokolování a výstrahy pro existující aplikace logiky](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Zapnout sledování a protokolování diagnostiky při vytváření aplikací logiky](../logic-apps/logic-apps-monitor-your-logic-apps-oms.md)
* [Případ použití: jak zdravotní péče společnost používá zpracování pro pracovní postupy HL7 FHIR výjimek aplikace logiky](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)

## <a name="deploy-and-manage-logic-apps"></a>Nasazení a Správa aplikací logiky

Můžete plně vývoj a nasazení aplikací logiky pomocí sady Visual Studio, Visual Studio Team Services, nebo jakékoli jiné zdrojového kódu a automatizované sestavovací nástroje. toosupport nasazení pro pracovní postupy a závislé připojení v šabloně prostředků, aplikace logiky pomocí šablony nasazení prostředků Azure. Nástroje sady Visual Studio automaticky generovat tyto šablony, které můžete zkontrolovat v ovládacím prvku toosource pro správu verzí.

* [Vytvořit šablonu automatického nasazení](../logic-apps/logic-apps-create-deploy-template.md)
* [Vytváření a nasazování aplikací logiky ze sady Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md)
* [Sledování stavu hello logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md)

## <a name="content-types-conversions-and-transformations-within-a-run"></a>Typy obsahu, převody a transformace v rámci spustit

Můžete používat, převod a transformace více typů obsahu pomocí hello mnoho funkcí v hello Azure Logic Apps [jazyk definic workflowů](http://aka.ms/logicappsdocs). Například můžete převést mezi řetězec, JSON a XML s hello `@json()` a `@xml()` výrazy pracovního postupu. modul Logic Apps Hello zachová přenos obsahu toosupport typy obsahu beze ztrát způsobem mezi službami.

* [Zpracování typy obsahu bez JSON](../logic-apps/logic-apps-content-type.md), například `application/xml`, `application/octet-stream`, a`multipart/formdata`
* [Jak fungují výrazy pracovního postupu v aplikace logiky](../logic-apps/logic-apps-author-definitions.md)
* [– Referenční informace: Jazyk definic workflowů funkce Azure Logic Apps](http://aka.ms/logicappsdocs)

## <a name="other-integrations-and-capabilities"></a>Další integrace a funkce

Aplikace logiky také nabízí integraci s mnoha služeb, jako jsou funkce Azure, Azure API Management, Azure App Services a vlastní koncové body protokolu HTTP, například REST a protokolu SOAP.

* [Vytvořte v reálném čase sociálních řídicí bez serveru Azure](../logic-apps/logic-apps-scenario-social-serverless.md)
* [Azure Functions volání z aplikace logiky](../logic-apps/logic-apps-azure-functions.md)
* [Scénář: Aktivační událost logiku aplikace s Azure Functions](../logic-apps/logic-apps-scenario-function-sb-trigger.md)
* [Blog: Volání koncových bodů protokolu SOAP z aplikace logiky](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)

## <a name="end-to-end-scenarios"></a>Kompletní scénáře

* [Dokument White Paper: Enterprise začátku do konce případu vedení integrace se službami Azure, jako jsou aplikace logiky](https://aka.ms/enterprise-integration-e2e-case-management-utilities-logic-apps)

## <a name="next-steps"></a>Další kroky

- [Zpracování chyb a výjimek v aplikacích logiky](../logic-apps/logic-apps-exception-handling.md)
- [Autor definice pracovního postupu s jazyk definic workflowů hello](../logic-apps/logic-apps-author-definitions.md)
- [Odesílat dotazy, komentáře, zpětnou vazbu nebo návrhy pro jak budeme vylepšovat Azure Logic Apps](https://feedback.azure.com/forums/287593-logic-apps)