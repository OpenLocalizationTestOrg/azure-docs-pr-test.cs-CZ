---
title: "aaaOverview systému bez serveru Azure | Microsoft Docs"
description: "Vytvořte výkonné řešení v cloudu hello bez nutnosti toothink infrastruktury."
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
ms.date: 03/30/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 7c9c09d96e472edd1631892982ac60aae97342a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-serverless-with-functions-and-logic-apps"></a>Přehled služby Azure bez serveru s funkcí a Logic Apps

Aplikace bez serveru nabízejí výhod zvýšení rychlosti vývoj, snížení kódu a jednoduchost s měřítkem.  Tento článek přejde do hello různé atributy bez serveru řešení a Azure bez serveru nabídky.

## <a name="what-is-serverless"></a>Co je bez serveru?

Bez serveru neznamená nejsou žádné servery – stačí znamená to, že vývojář hello nemá tooworry o serverech.  Velkou část vývoj tradiční aplikací je odpovědi na otázky ohledně škálování, hostování a monitorování řešení toomeet hello požadavky aplikace hello.  S bez serveru tyto otázky se postarat jako součást řešení hello.  Kromě toho se účtují bez serveru aplikace na plán na základě spotřeby.  Pokud je aplikace hello nikdy nepoužívali, započítány jsou nikdy zpoplatněny.  Tyto funkce umožňují vývojářům toofocus výhradně na obchodní logiku hello hello řešení.

Hello základní služby v Azure kolem bez serveru jsou [Azure Functions](https://azure.microsoft.com/services/functions/) a [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).  Obě tato řešení řídí zásadami hello výše a umožňují vývojářům toobuild robustní cloudové aplikace s minimálním kódem.

## <a name="what-are-azure-functions"></a>Co je Azure Functions?

Azure Functions je řešení umožňující snadno spouštět malé části kódu, nebo "funkce" v cloudu hello. Můžete napsat právě hello kód, které že budete potřebovat pro hello problém, bez obav o celou aplikaci nebo hello toorun infrastruktury. Funkce provádět vývoj i zvýšit produktivitu a můžete použít svůj vývoj jazyk podle vlastní volby, například C#, F #, Node.js, Python nebo PHP. Platí se pouze za hello v době běhu kódu a Azure škáluje podle potřeby.

Pokud chcete toojump práva a začít pracovat s Azure Functions, začněte s [vytvoření první funkce Azure](../azure-functions/functions-create-first-azure-function.md). Pokud hledáte další odborné informace o funkcích, přečtěte si téma hello [referenční informace pro vývojáře](../azure-functions/functions-reference.md).

## <a name="what-are-azure-logic-apps"></a>Jaké jsou Azure Logic Apps?

Azure Logic Apps poskytuje způsob toosimplify a implementovat škálovatelné integrace a pracovních postupů v cloudu hello. Poskytuje visual návrháře toomodel a automatizaci jako řadu kroků volat pracovní postup.  Existují [mnoho konektory](../connectors/apis-list.md) napříč službami pro cloudové a místní tooquickly připojit bez serveru aplikace tooother rozhraní API.  Aplikace logiky začíná aktivační událost (jako "při přidání účtu tooDynamics CRM') a po pálení můžete začít mnoho kombinace akce, převody a logiku podmínku.  Služba Logic Apps je je služba skvělou volbou při různých Azure Functions v procesu - Orchestrace, zejména v případě, že proces hello vyžaduje interakci s externím systému nebo rozhraní API.

tooget začít s Logic Apps, začínat [vytvoření první aplikace logiky](logic-apps-create-a-logic-app.md).  Pokud hledáte další odborné informace o Logic Apps, přečtěte si téma hello [referenční informace pro vývojáře](logic-apps-workflow-actions-triggers.md).

## <a name="how-can-i-build-and-deploy-serverless-applications-in-azure"></a>Jak můžete vytvořit a nasadit bez serveru aplikace v Azure?

Azure poskytuje bohatou sadu nástrojů pro vývoj, nasazení a správu aplikací bez serveru.  Aplikace se dají vytvářet přímo v hello portál Azure nebo s [tooling ze sady Visual Studio](logic-apps-serverless-get-started-vs.md).  Jakmile aplikace byla vytvořena, může být [nasadit okamžitě](logic-apps-create-deploy-template.md).  Azure poskytuje také monitorování pro aplikace, bez serveru.  Toto monitorování je přístupná z hello portál Azure, prostřednictvím rozhraní API hello nebo sady SDK nebo s tooOMS integrované nástrojů a Application Insights.

## <a name="next-steps"></a>Další kroky

* [Začínáme sestavení bez serveru aplikace v sadě Visual Studio](logic-apps-serverless-get-started-vs.md)
* [Vytvořte zákazníka přehledný řídicí panel s bez serveru](logic-apps-scenario-social-serverless.md)
* [Vytvoření šablony nasazení pro aplikaci logiky](logic-apps-create-deploy-template.md)