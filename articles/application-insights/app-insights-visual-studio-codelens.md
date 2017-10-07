---
title: aaaApplication telemetrii Insights ve funkci CodeLens Visual Studio | Microsoft Docs
description: "Přistupujte rychle k požadavkům Application Insights a telemetrii výjimek pomocí CodeLens v sadě Visual Studio."
services: application-insights
documentationcenter: .net
author: numberbycolors
manager: carmonm
ms.assetid: 93559e44-23cb-4b9d-8425-60f7f0d0a82c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: bwren
ms.openlocfilehash: e812aa48f2a67eea860e7ecde341855763bb8a8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-telemetry-in-visual-studio-codelens"></a>Telemetrie Application Insights ve Visual Studio CodeLens
Metody v hello kódu vaší webové aplikace může být označený poznámkou s telemetrii týkající se výjimky v době běhu a požadovat doby odezvy. Pokud instalujete [Azure Application Insights](app-insights-overview.md) ve vaší aplikaci v sadě Visual Studio se zobrazí hello telemetrie [Codelensu](https://msdn.microsoft.com/library/dn269218.aspx) -hello poznámky hello horní části každé funkce, kde jste zvyklí tooseeing užitečné informace, jako je hello počet míst hello funkce odkazuje nebo hello poslední osobě, která ji upravit.

![CodeLens](./media/app-insights-visual-studio-codelens/codelens-overview.png)

> [!NOTE]
> Application Insights ve funkci CodeLens je k dispozici v sadě Visual Studio 2015 Update 3 a novější nebo s nejnovější verzí hello [Developer Analytics Tools rozšíření](https://visualstudiogallery.msdn.microsoft.com/82367b81-3f97-4de1-bbf1-eaf52ddc635a). Codelensu je k dispozici v hello Enterprise a Professional edice sady Visual Studio.
> 
> 

## <a name="where-toofind-application-insights-data"></a>Kde toofind data Application Insights
Vyhledejte telemetrie Application Insights v hello Codelensu indikátory metod hello veřejné žádost webové aplikace. Indikátory CodeLens jsou uvedené nad metodami a dalšími deklaracemi v kódu C# a Visual Basic. Pokud jsou pro metodu k dispozici data Application Insights, zobrazí se indikátory požadavků a výjimek, například „100 požadavků, 1 % neúspěšných“ nebo „10 výjimek“. Další podrobnosti zobrazíte kliknutím na indikátor CodeLens. 

> [!TIP]
> Žádosti o služby Application Insights a výjimky indikátory může trvat několik sekund, navíc tooload po další indikátory Codelensu zobrazí.
> 
> 

## <a name="exceptions-in-codelens"></a>Výjimky v CodeLens
![Bude doplněno](./media/app-insights-visual-studio-codelens/codelens-exceptions.png)

Hello výjimka Codelensu ukazatele zobrazuje hello počet výjimek, k nimž došlo v hello posledních 24 hodin od hello 15 většina nejčastěji se vyskytující výjimek v aplikaci během této doby, během zpracování požadavku hello poskytovaného metodou hello.

toosee více podrobností, klikněte na indikátor Codelensu hello výjimky:

* Procento Hello Změna počtu výjimky z hello nejnovější 24 hodin relativní toohello předchozích 24 hodin
* Zvolte **přejděte toocode** toonavigate toohello zdrojového kódu pro funkce hello vyvolání výjimky hello
* Zvolte **vyhledávání** tooquery hello všechny instance této výjimky, ke kterým došlo v posledních 24 hodin
* Zvolte **Trend** tooview trend vizualizace pro výskyty této výjimky v hello posledních 24 hodin
* Zvolte **zobrazit všechny výjimky v této aplikaci** tooquery hello všechny výjimky, k nimž došlo v posledních 24 hodin
* Zvolte **prozkoumat trendy výjimka** tooview trend vizualizace pro všechny výjimky, k nimž došlo v hello posledních 24 hodin. 

> [!TIP]
> Pokud najdete v části "0 výjimky" ve funkci CodeLens ale víte, že by měla být výjimky, zkontrolujte, zda je zaškrtnuto vpravo prostředek Application Insights hello ve funkci CodeLens toomake. tooselect jiný prostředek, klikněte pravým tlačítkem na projekt v Průzkumníku řešení hello a zvolte **Application Insights > Vybrat zdroj Telemetrie**. Codelensu je zobrazeno pouze pro hello 15 většina nejčastěji se vyskytující výjimek ve vaší aplikaci v hello posledních 24 hodin, pokud výjimku je často hello 16 většinu nebo méně, uvidíte "0 výjimky." Výjimky z zobrazení ASP.NET se nemůže nacházet na hello řadiče metody, které generovaná těchto zobrazení.
> 
> [!TIP]
> Pokud v CodeLens uvidíte „? výjimky"ve funkci CodeLens, je nutné tooassociate, které mohla vypršet platnost účtu Azure v sadě Visual Studio nebo svoje přihlašovací údaje účtu Azure. V obou případech klikněte na „? výjimky"a zvolte **přidat účet...**  tooenter přihlašovacích údajů.
> 
> 

## <a name="requests-in-codelens"></a>Požadavky v CodeLens
![Bude doplněno](./media/app-insights-visual-studio-codelens/codelens-requests.png)

Hello požadavek zobrazuje indikátor Codelensu hello počet HTTP požadavky, které byly obsluhovány pomocí metody v hello za 24 hodin, plus hello procento tyto požadavky, které se nezdařilo.

Klikněte na další podrobnosti, toosee hello požadavky Codelensu ukazatele:

* Hello absolutní a procento změny v počtu žádostí o, neúspěšných požadavků a průměrné odezvy přes hello za 24 hodin porovnání toohello předchozích 24 hodin
* spolehlivost Hello hello metody, vypočítá se jako procento hello požadavků, které nedošlo k selhání v hello posledních 24 hodin
* Zvolte **vyhledávání** požadavky ani neúspěšných požadavků tooquery všechny hello (neúspěšných) požadavků, které nastaly v hello posledních 24 hodin
* Zvolte **Trend** tooview trend vizualizace pro požadavky, neúspěšné požadavky nebo Průměrná odpovědi krát za hello posledních 24 hodin.
* Vyberte název hello hello prostředek Application Insights v hello levého horního rohu hello Codelensu podrobnosti zobrazení toochange, který prostředek se hello zdroj pro Codelensu data.

## <a name="next"></a>Další kroky
|  |  |
| --- | --- |
| **[Práce s Application Insights v sadě Visual Studio](app-insights-visual-studio.md)**<br/>Hledejte telemetrii, zobrazujte data v CodeLens a konfigurujte Application Insights. Vše v sadě Visual Studio. |![Klikněte pravým tlačítkem na projekt hello a vyberte Application Insights, vyhledávání](./media/app-insights-visual-studio-codelens/34.png) |
| **[Přidání dalších dat](app-insights-asp-net-more.md)**<br/>Sledování využití, dostupnosti, závislostí, výjimek. Integrujte trasování z rozhraní protokolování. Zapisuje vlastní telemetrii. |![Visual Studio](./media/app-insights-visual-studio-codelens/64.png) |
| **[Práce s Application Insights portál hello](app-insights-dashboards.md)**<br/>Řídicí panely, výkonné nástroje pro diagnostiku a analýzy, výstrahy, aktivní mapa závislostí vaší aplikace a export telemetrie. |![Visual Studio](./media/app-insights-visual-studio-codelens/62.png) |

