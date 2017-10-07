---
title: "aaaRelease poznámky pro službu Application Insights | Microsoft Docs"
description: "Přidání nasazení nebo vytvořit značek tooyour metriky explorer grafů ve službě Application Insights."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 23173e33-d4f2-4528-a730-913a8fd5f02e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: bwren
ms.openlocfilehash: e802d22701cb69e96fd1a6b469ea67454195f77a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Poznámky u metriky grafů ve službě Application Insights
Poznámky na [Průzkumníku metrik](app-insights-metrics-explorer.md) grafy zobrazují, kde jste nasadili nového sestavení nebo jinou významnou událost. Provádění ho snadno toosee zda změny měla vliv na výkon vaší aplikace. Můžete být automaticky vytvoří pomocí hello [Visual Studio Team Services sestavení systému](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs). Můžete také vytvořit poznámky tooflag všechny události, které chcete pomocí [jejich vytváření z prostředí PowerShell](#create-annotations-from-powershell).

![Příklad poznámky s viditelné korelace s doba odezvy serveru](./media/app-insights-annotations/00.png)



## <a name="release-annotations-with-vsts-build"></a>Poznámky k verzi s služby VSTS sestavení

Poznámky k verzi jsou funkce hello cloudového sestavování a verzí služby Visual Studio Team Services. 

### <a name="install-hello-annotations-extension-one-time"></a>Nainstalujte rozšíření poznámky hello (jednou)
toobe možné toocreate verze poznámky, budete potřebovat tooinstall jeden z hello mnoho rozšíření Team služeb k dispozici v hello Visual Studio Marketplace.

1. Přihlaste se tooyour [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) projektu.
2. V sadě Visual Studio Marketplace [získat hello poznámky k verzi rozšíření](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations)a přidejte ji tooyour účtu Team Services.

![Top na pravé straně Team Services webové stránky, otevřete Marketplace. Vyberte Visual Team Services a potom v části sestavení a verze, zvolte najdete v části více.](./media/app-insights-annotations/10.png)

Potřebujete jenom toodo tomto jednou pro váš účet Visual Studio Team Services. Poznámky k verzi se teď dá nakonfigurovat pro každý projekt ve vašem účtu. 

### <a name="configure-release-annotations"></a>Konfigurace verze poznámek

Ke každé šabloně verze služby VSTS musíte klíč tooget samostatné rozhraní API.

1. Přihlaste se toohello [portálu Microsoft Azure](https://portal.azure.com) a otevřete prostředek Application Insights hello, který monitoruje vaše aplikace. (Nebo [vytvořit nový](app-insights-overview.md), pokud jste tak ještě neučinili.)
2. Otevřete **přístup pomocí rozhraní API**, **Application Insights Id**.
   
    ![Na stránce portal.azure.com otevřete prostředek Application Insights a zvolte nastavení. Otevřete přístup pomocí rozhraní API. Zkopírujte hello ID aplikace](./media/app-insights-annotations/20.png)

4. V samostatném okně prohlížeče otevřete (nebo vytvořte) hello verzi šablony, která spravuje vaše nasazení z Visual Studio Team Services. 
   
    Přidat úlohu a vyberte úlohu hello Application Insights verze poznámky z nabídky hello.
   
    Vložení hello **Id aplikace** který jste zkopírovali ze hello okno přístup pomocí rozhraní API.
   
    ![Ve Visual Studio Team Services otevřete verzi, vyberte verzi definice a zvolte Upravit. Klikněte na tlačítko Přidat úloha a vyberte Application Insights verze poznámky. Vložte hello ID Application Insights.](./media/app-insights-annotations/30.png)
4. Sada hello **APIKey** proměnné pole tooa `$(ApiKey)`.

5. Zpět v hello okno Azure vytvořte nový klíč rozhraní API a provést jeho kopii.
   
    ![V okně přístup pomocí rozhraní API v Azure okno hello hello klikněte na tlačítko vytvořit klíč rozhraní API. Zadejte komentář, zkontrolujte poznámky zápisu a klikněte na tlačítko vygenerovat klíč. Zkopírujte nový klíč hello.](./media/app-insights-annotations/40.png)

6. Otevřete kartu Konfigurace hello hello verze šablony.
   
    Vytvoření proměnné definice pro `ApiKey`.
   
    Vložte klíče toohello rozhraní API ApiKey definicí proměnné.
   
    ![V okně hello Team Services vyberte na kartě Konfigurace hello a klikněte na tlačítko Přidat proměnnou. Nastavte název tooApiKey hello do hello hodnotu, vložte hello klíče, který jste právě vygenerovali a klikněte na ikonu zámku hello.](./media/app-insights-annotations/50.png)
7. Nakonec **Uložit** hello verze definice.


## <a name="view-annotations"></a>Zobrazení poznámky
Nyní vždy, když používáte hello verze šablony toodeploy novou verzi, poznámky odešle tooApplication statistiky. Hello poznámky se zobrazí v grafech v Průzkumníku metrik.

Klikněte na žádné poznámky značky tooopen podrobnosti o hello verze, včetně žadatele, zdroj ovládacího prvku větve, definice vydání, prostředí a další.

![Klikněte na možnost všechny značky poznámky verze.](./media/app-insights-annotations/60.png)

## <a name="create-custom-annotations-from-powershell"></a>Vytvořit vlastní poznámky z prostředí PowerShell
Můžete také vytvořit poznámky z jakýkoli proces, který chcete (bez použití VS Team System). 


1. Vytvořit místní kopii hello [skript prostředí Powershell z Githubu](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

2. Získejte hello ID aplikace a vytvořte klíč rozhraní API z hello okno přístup pomocí rozhraní API.

3. Volání skriptu hello takto:

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

Je snadno toomodify hello skriptu, například toocreate poznámky pro posledních hello.

## <a name="next-steps"></a>Další kroky

* [Vytváření pracovních položek](app-insights-diagnostic-search.md#create-work-item)
* [Automatizace v prostředí PowerShell](app-insights-powershell.md)
