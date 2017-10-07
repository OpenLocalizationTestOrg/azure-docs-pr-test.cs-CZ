---
title: "aaaScenario - vytvořit zákazníka přehledný řídicí panel s bez serveru Azure | Microsoft Docs"
description: "Příklad zákazník toomanage řídicího panelu můžete vytvořit, zpětnou vazbu, sociální data a další s funkcemi Azure a Azure Logic Apps."
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2017
ms.author: jehollan
ms.openlocfilehash: db175e895e37aa795a9c34bf4d65566bf68f8c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-real-time-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a>Vytvořte v reálném čase zákazníka přehledný řídicí panel s funkcemi Azure a Azure Logic Apps

Nástroje Azure bez serveru zadejte výkonné možnosti tooquickly sestavení a aplikacím v cloudu hello bez nutnosti toothink infrastruktury.  V tomto scénáři jsme bude vytvoření tootrigger řídicí panel názorů zákazníků, analýzu zpětnou vazbu pomocí machine learning a publikování Statistika zdroj jako Power BI nebo Azure Data Lake.

## <a name="overview-of-hello-scenario-and-tools-used"></a>Přehled hello scénář a použít nástroje

V pořadí tooimplement toto řešení, jsme využije hello dvě klíčové komponenty aplikace bez serveru v Azure: [Azure Functions](https://azure.microsoft.com/services/functions/) a [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).

Služba Logic Apps je modul bez serveru pracovního postupu v cloudu hello.  Poskytuje orchestration mezi komponentami bez serveru a také připojí tooover 100 služeb a rozhraní API.  V tomto scénáři vytvoříme tootrigger aplikace logiky na zpětnou vazbu od zákazníků.  Mezi hello konektorů, které se podílejí na zpětnou vazbu reagující toocustomer patří Outlook.com, Office 365, opic průzkum, Twitter a požadavek HTTP [z webového formuláře](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).  Pro pracovní postup hello níže jsme monitorovat hashtag na Twitteru.

Funkce zajistit bez serveru výpočetní v cloudu hello.  V tomto scénáři budeme používat Azure Functions tooflag tweetů od zákazníků založené na řadu předem definovaná klíčová slova.

může být celé řešení Hello [sestavení v sadě Visual Studio](logic-apps-deploy-from-vs.md) a [nasazen jako součást šablony prostředků](logic-apps-create-deploy-template.md).  Je také video s návodem hello scénář [na webu Channel 9](http://aka.ms/logicappsdemo).

## <a name="build-hello-logic-app-tootrigger-on-customer-data"></a>Sestavení hello logiku aplikace tootrigger na data zákazníků

Po [vytvoření aplikace logiky](logic-apps-create-a-logic-app.md) v sadě Visual Studio nebo hello portálu Azure:

1. Přidat aktivační událost pro **na nové Tweetů** ze služby Twitter.
2. Nakonfigurujte hello aktivační událost toolisten tootweets na – klíčové slovo nebo hashtag.

   > [!NOTE]
   > Vlastnost opakování Hello na aktivační událost hello určí, jak často se aplikace logiky hello kontrolovat přítomnost nových položek na základě cyklického dotazování aktivační události

   ![Příklad aktivační události služby Twitter.][1]

Tato aplikace bude nyní platit na všechny nové tweetů.  Pak můžeme trvat dat tweet a lépe hello postojích vyjádřené porozumět.  Pro tento používáme hello [kognitivní služby Azure](https://azure.microsoft.com/services/cognitive-services/) toodetect postojích textu.

1. Klikněte na tlačítko **nový krok**
1. Vyberte nebo vyhledejte hello **Analýza textu** konektoru
1. Vyberte hello **zjistit postojích** operace
1. Pokud se zobrazí výzva, zadejte platný klíč kognitivní služby pro hello služba Analýza textu
1. Přidat hello **Tweet Text** jako hello text tooanalyze.

Teď, když máme hello tweet dat a statistika na hello tweet, může být několik dalších konektorů relevantní:
* Power BI - přidat řádky tooStreaming datovou sadu: zobrazení tweetů reálném čase na řídicí panel Power BI.
* Azure Data Lake - připojit soubor: Přidání zákazníka data tooan Azure Data Lake datovou sadu tooinclude v úloh analytics.
* SQL - přidávání řádků: ukládání dat v databázi pro pozdější načtení.
* Slack – poslat zprávu: výstrahy slack kanál na záporné zpětnou vazbu, která vyžaduje akce.

Funkce Azure může být také použít toodo další vlastní výpočetní nad hello data.

## <a name="enriching-hello-data-with-an-azure-function"></a>Rozšíření dat hello s funkcí Azure

Můžeme vytvořit funkci, potřebujeme toohave aplikaci funkce v naše předplatné Azure.  Informace o vytvoření funkce Azure hello portálu můžete [se nachází tady](../azure-functions/functions-create-first-azure-function-azure-portal.md)

Pro volání přímo z aplikace logiky toobe funkce se vyžaduje toohave HTTP aktivovat vazby.  Doporučujeme používat hello **HttpTrigger** šablony.

V tomto scénáři by tělo žádosti hello hello funkce Azure hello tweet text.  V kódu funkce hello jednoduše definujte logiku Pokud hello tweet text obsahuje klíčové slovo nebo frázi.  samotná Funkce Hello může uchovávat jako jednoduché nebo komplexní, podle potřeby pro scénář hello.

Na konci hello hello funkce jednoduše vrátí odpověď aplikace logiky toohello určitými daty.  To může být jednoduchý logickou hodnotu (například `containsKeyword`), nebo komplexní objekt.

![Nakonfigurované krok funkce Azure][2]

> [!TIP]
> Při přístupu k komplexní odpověď z funkce v aplikaci logiky, pomocí akce hello analyzovat JSON.

Po uložení hello funkce mohou být přidány do aplikace logiky hello vytvořili výše.  V aplikaci logiky hello:

1. Klikněte na tlačítko tooadd **nový krok**
1. Vyberte hello **Azure Functions** konektoru
1. Vyberte toochoose existující funkce a procházet toohello funkce vytvořit
1. Odeslat v hello **Tweet Text** pro hello **text žádosti**

## <a name="running-and-monitoring-hello-solution"></a>Spuštění a sledování řešení hello

Jednou z výhod hello vytváření bez serveru orchestrations v Logic Apps je hello bohaté ladění a možnosti monitorování.  Všechny spustit (aktuální nebo historické) lze zobrazit z v sadě Visual Studio, hello portál Azure, nebo prostřednictvím hello REST API a sady SDK.

Jeden z hello nejjednodušší způsoby tootest aplikace logiky používá hello **spustit** tlačítko v Návrháři hello.  Kliknutím na tlačítko **spustit** bude pokračovat toopoll hello aktivace každých 5 sekund, dokud je zjištěna událost a poskytnout za provozu zobrazení průběhu hello spustit.

Předchozí spuštění historií lze zobrazit v okně Přehled hello v hello portál Azure nebo pomocí hello cloudu Průzkumníka Visual Studio.

## <a name="creating-a-deployment-template-for-automated-deployments"></a>Vytvoření šablony nasazení pro automatické nasazení

Jakmile byla vyvinuta řešení, můžete zachytit a nasazují přes tooany šablony nasazení Azure oblast Azure z hello, world.  To je užitečné pro oba změny parametry pro různé verze tento pracovní postup, ale také pro integraci toto řešení v kanálu sestavení a verze.  Informace o vytváření šablony nasazení lze nalézt [v tomto článku](logic-apps-create-deploy-template.md).

Azure Functions můžete také začlení do šablony nasazení hello - tak hello celé řešení s všechny závislosti můžete spravovat jako jednu šablonu.  Příklad funkce šablony nasazení lze nalézt v hello [úložiště šablony Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).

## <a name="next-steps"></a>Další kroky

* [Zobrazit další příklady a scénáře pro Azure Logic Apps](logic-apps-examples-and-scenarios.md)
* [Podívejte se na video s návodem k vytvoření tohoto řešení pro kompletní](http://aka.ms/logicappsdemo)
* [Zjistěte, jak toohandle a catch výjimky v rámci aplikace logiky](logic-apps-exception-handling.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png