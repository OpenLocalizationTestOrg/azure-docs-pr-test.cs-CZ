---
title: "aaaCreate nový prostředek Azure Application Insights | Microsoft Docs"
description: "Ručně nastavte Application Insights monitorování nové aplikace za provozu."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: bwren
ms.openlocfilehash: 3aba7045e1f8fe43d473f0be01dd52106ab992a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-insights-resource"></a>Vytvořte prostředek Application Insights
Azure Application Insights zobrazí data o vaší aplikaci v Microsoft Azure *prostředků*. Vytvoření nového prostředku, proto je součástí [nastavení Application Insights toomonitor novou aplikaci][start]. V mnoha případech vytvoření prostředku můžete provést automaticky hello IDE. Ale v některých případech můžete ručně vytvořit prostředek – například toohave samostatné prostředky pro vývoj a produkci sestavení aplikace.

Po vytvoření hello prostředků, můžete získat svůj klíč instrumentace a používat tento hello tooconfigure SDK v aplikaci hello. klíče odkazy na zdroje Hello hello telemetrie toohello prostředků.

## <a name="sign-up-toomicrosoft-azure"></a>Zaregistrujte si tooMicrosoft Azure
Pokud jste to ještě získali [Microsoft account, pořiďte si ho](http://live.com). (Pokud používáte služby jako Outlook.com, OneDrive, Windows Phone nebo XBox Live, už máte účet Microsoft.)

Musíte taky předplatné příliš[Microsoft Azure](http://azure.com). Pokud váš tým nebo společnost má předplatné Azure, vlastník hello vás může přidat tooit, pomocí účtu Windows Live ID. Se účtují poplatky se používá. Základní plán Hello výchozí umožňuje určité množství experimentální použití zdarma.

Pokud máte k dispozici přístup tooa předplatného, přihlaste se tooApplication Insights na [http://portal.azure.com](https://portal.azure.com)a používat vaše toologin Live ID.

## <a name="create-an-application-insights-resource"></a>Vytvořte prostředek Application Insights
V hello [portal.azure.com](https://portal.azure.com), přidejte prostředek Application Insights:

![Klikněte na tlačítko Nový, Application Insights](./media/app-insights-create-new-resource/01-new.png)

* **Typ aplikace** ovlivňuje, co se zobrazí v okně Přehled hello a hello vlastnosti, které jsou k dispozici v [metriky explorer][metrics]. Pokud nevidíte vašeho typu aplikace, vyberte Obecné.
* **Předplatné** je váš účet platebních v Azure.
* **Skupina prostředků** je užitečný pro správu vlastnosti jako řízení přístupu. Pokud jste již vytvořili ostatní prostředky služby Azure, můžete vybrat tooput tento nový prostředek v hello stejné skupiny.
* **Umístění** je, kde společnost Microsoft uchovávat data.
* **PIN kód toodashboard** vloží dlaždici rychlý přístup pro prostředek na Azure domovské stránky. Nedoporučuje.

Po vytvoření aplikace, otevře se nové okno. Toto okno je, kde uvidíte data o využití a výkonu o vaší aplikaci. 

tooget back tooit při příštím přihlášení tooAzure, podívejte se na dlaždici úvodní vaší aplikace na hello spustit Tabule (domovskou obrazovku). Nebo klikněte na tlačítko Procházet toofind ho.

## <a name="copy-hello-instrumentation-key"></a>Zkopírovat klíč instrumentace hello
klíč instrumentace Hello identifikuje hello prostředek, který jste vytvořili. Budete ho potřebovat toogive toohello SDK.

![Klikněte na tlačítko Essentials, klikněte na tlačítko hello klíč instrumentace, CTRL + C](./media/app-insights-create-new-resource/02-props.png)

## <a name="install-hello-sdk-in-your-app"></a>Nainstalujte hello SDK v aplikaci
Nainstalujte hello Application Insights SDK ve vaší aplikaci. Tento krok výraznou závisí na typu hello vaší aplikace. 

Použití klíče tooconfigure hello instrumentace [hello SDK, který nainstalujete v aplikaci][start].

Hello SDK zahrnuje standardní moduly, které odesílat telemetrická data bez nutnosti toowrite žádný kód. akce uživatelů tootrack nebo diagnostikovat problémy podrobněji, [pomocí rozhraní API hello] [ api] toosend vlastní telemetrii.

## <a name="monitor"></a>Zobrazit data telemetrie
Zavřít hello rychlé spuštění okna okno tooreturn tooyour aplikací v hello portálu Azure.

Klikněte na tlačítko hello Search dlaždice toosee [diagnostické vyhledávání][diagnostic], kde se zobrazí události první hello. 

Pokud očekáváte více dat, klikněte na tlačítko **aktualizovat** za několik sekund.

## <a name="creating-a-resource-automatically"></a>Vytvoření prostředku automaticky
Můžete napsat [skript prostředí PowerShell](app-insights-powershell.md) toocreate prostředek automaticky.

## <a name="next-steps"></a>Další kroky
* [Vytvoření řídicího panelu](app-insights-dashboards.md)
* [Diagnostické vyhledávání](app-insights-diagnostic-search.md)
* [Zkoumání metrik](app-insights-metrics-explorer.md)
* [Psaní analytických dotazů](app-insights-analytics.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[start]: app-insights-overview.md

