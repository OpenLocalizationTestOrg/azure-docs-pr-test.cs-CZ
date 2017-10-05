---
title: "Začínáme s pomocí ukázky"
description: "Power BI Embedded – přidejte do své aplikace business intelligence interaktivní sestavy Power BI pomocí sady SDK"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: d8a9ef78-ad4e-4bc7-9711-89172dc5c548
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/02/2017
ms.author: asaxton
ms.openlocfilehash: c3cb1763f807220a4a829f410d7eb77974b25776
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-power-bi-embedded-sample"></a>Začínáme s ukázkou Power BI Embedded

S **Microsoft Power BI Embedded**, sestavy Power BI můžete integrovat přímo do vašeho webu nebo mobilní aplikace. V tomto článku budete zavedeme vám **Power BI Embedded** ukázka Začínáme get.

Předtím, než jsme přejít žádné další, budete pravděpodobně chtít uložit v následujících zdrojích informací. Budete pomáhají při integraci sestavy Power BI příliš do své vlastní aplikace a ukázkové aplikace.

* [Ukázka prostoru webové aplikace](http://go.microsoft.com/fwlink/?LinkId=761493)
* [Power BI Embedded API – referenční informace](https://msdn.microsoft.com/en-US/library/azure/mt711507.aspx)
* [Power BI Embedded .NET SDK ](http://go.microsoft.com/fwlink/?LinkId=746472) (k dispozici prostřednictvím balíčku NuGet)
* [Ukázka vložení sestavy JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo)

> [!NOTE] 
> Než můžete nakonfigurovat a spustit Power BI Embedded získat spuštění ukázkové, musíte vytvořit alespoň jednu **kolekce pracovních prostorů** ve vašem předplatném Azure. Další informace o vytvoření **kolekce pracovních prostorů** najdete v tématu Azure Portal [Začínáme s Power BI Embedded](power-bi-embedded-get-started.md).

## <a name="configure-the-sample-app"></a>Nakonfigurujte ukázkovou aplikaci

Projděme nastavení vývojového prostředí sady Visual Studio pro přístup k součástem potřebné ke spuštění ukázkové aplikace.

1. Stáhněte a rozbalte [Power BI Embedded - integraci sestavy do webové aplikace](http://go.microsoft.com/fwlink/?LinkId=761493) ukázku na Githubu.
2. Otevřete **PowerBI embedded.sln** v sadě Visual Studio. Budete muset provést **balíček aktualizace** příkazu v konzole Správce balíčků NuGET za účelem aktualizace balíčcích používaných v tomto řešení.
3. Sestavte řešení.
4. Spustit **ProvisionSample** konzolovou aplikaci. V konzole ukázková aplikace zřídíte pracovního prostoru a naimportovat soubor PBIX.
5. Zřídit novou **prostoru**, vyberte možnost 1, **Správa kolekce**a potom vyberte možnost 6, **zřídit nový pracovní prostor**
6. Importovat novou **sestavy**, vyberte možnost 2, **sestav správy**a potom vyberte možnost 3, **naimportovat soubor PBIX plochy souboru do pracovního prostoru**.

7. Zadejte vaše **kolekce pracovních prostorů** název, a **přístupový klíč**. To můžete získat **portálu Azure**. Další informace o tom, jak získat vaše **přístupový klíč**, najdete v části [zobrazit Power BI API přístupové klíče](power-bi-embedded-get-started.md#view-power-bi-api-access-keys) v Začínáme s Microsoft Power BI Embedded.

    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
8. Zkopírujte a uložte nově vytvořený **ID pracovního prostoru** používat později v tomto článku. Po **ID pracovního prostoru** je vytvořen, najdete ji **portálu Azure**.

    ![](media/powerbi-embedded-get-started-sample/workspace-id.png)
9. Chcete-li importovat soubor PBIX do vaší **prostoru**, vyberte možnost **6. Soubor PBIX plochy importu do existujícímu pracovnímu prostoru**. Pokud nemáte soubor PBIX souboru užitečný, si můžete stáhnout [prodejní analýzy ukázkový soubor PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).
10. Pokud se zobrazí výzva, zadejte popisný název pro vaše **datovou sadu**.

Zobrazí takováto odpověď:

```
Checking import state... Publishing
Checking import state... Succeeded
```

> [!NOTE]
> Pokud váš soubor PBIX obsahuje všechna připojení přímý dotaz, spusťte možnost 7 aktualizovat připojovací řetězce.

V tuto chvíli máte soubor PBIX Power BI sestavu importovat do vaší **prostoru**. Teď se podíváme, jak spouštět **Power BI Embedded** získat Začínáme ukázkovou webovou aplikaci.

## <a name="run-the-sample-web-app"></a>Spuštění ukázkové webové aplikace
Ukázka aplikace web je ukázkovou aplikaci, která vykreslí sestavy importovat do vaší **prostoru**. Zde je postup konfigurace ukázky webové aplikaci.

1. V **vložených PowerBI** řešení sady Visual Studio, klikněte pravým tlačítkem **EmbedSample** webové aplikace a zvolte **nastavit jako spouštěný projekt**.
2. V **web.config**v **EmbedSample** webovou aplikaci, upravit **appSettings**: **AccessKey**, **WorkspaceCollection** název, a **WorkspaceId**.

    ```
    <appSettings>
        <add key="powerbi:AccessKey" value="" />
        <add key="powerbi:ApiUrl" value="https://api.powerbi.com" />
        <add key="powerbi:WorkspaceCollection" value="" />
        <add key="powerbi:WorkspaceId" value="" />
    </appSettings>
    ```
3. Spustit **EmbedSample** webové aplikace.

Po spuštění **EmbedSample** webové aplikace, musí obsahovat levém navigačním panelu **sestavy** nabídky. Chcete-li zobrazit sestavu jste importovali, rozbalte **sestavy**a klikněte na sestavu. Pokud jste importovali [prodejní analýzy ukázkový soubor PBIX](http://go.microsoft.com/fwlink/?LinkID=780547), ukázkovou webovou aplikaci bude vypadat takto:

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-sample-left-nav.png)

Po kliknutí na tlačítko sestavy, **EmbedSample** webová aplikace by měl vypadat přibližně toto:

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="explore-the-sample-code"></a>Prozkoumejte ukázkový kód

**Microsoft Power BI Embedded** ukázka je ukázkové webové aplikace, která ukazuje, jak integrovat **Power BI** sestavy do vaší aplikace. Návrhový vzor Model-View-Controller (MVC) používá k předvedení osvědčené postupy. V této části jsou zdůrazněné částí ukázkový kód, který můžete prozkoumat v rámci **vložených PowerBI** webové aplikace řešení. Vzor Model-View-Controller (MVC) odděluje modelování domény, prezentace a akce založené na vstup uživatele na tři samostatné třídy: Model, zobrazení a řízení. Další informace o MVC naleznete v tématu [Další informace o ASP.NET](http://www.asp.net/mvc).

**Microsoft Power BI Embedded** ukázkový kód je oddělená následujícím způsobem. Každá část zahrnuje název souboru v řešení PowerBI embedded.sln, takže budete moci snadno najít kód v ukázce.

> [!NOTE]
> V této části je uveden seznam ukázkový kód, který ukazuje, jak byla zapsána kód. Chcete-li zobrazit celý vzorek, načtěte PowerBI embedded.sln řešení v sadě Visual Studio.

### <a name="model"></a>Model

Je vzorku **ReportsViewModel** a **ReportViewModel**.

**ReportsViewModel.cs**: představuje sestavy Power BI.

    public class ReportsViewModel
    {
        public List<Report> Reports { get; set; }
    }

**ReportViewModel.cs**: představuje sestavy Power BI.

    public classReportViewModel
    {
        public IReport Report { get; set; }

        public string AccessToken { get; set; }
    }

### <a name="connection-string"></a>Připojovací řetězec

Připojovací řetězec musí být v následujícím formátu:

```
Data Source=tcp:MyServer.database.windows.net,1433;Initial Catalog=MyDatabase
```

Pomocí běžné atributy server a databáze se nezdaří. Příklad: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,

### <a name="view"></a>Zobrazení

**Zobrazení** spravuje zobrazení Power BI **sestavy** a Power BI **sestavy**.

**Reports.cshtml**: iterace v **Model.Reports** vytvořit **ActionLink**. **ActionLink** se skládá následujícím způsobem:

| Část | Popis |
| --- | --- |
| Název |Název sestavy. |
| Řetězce dotazu |Odkaz na ID sestavy. |

    <div id="reports-nav" class="panel-collapse collapse">
        <div class="panel-body">
            <ul class="nav navbar-nav">
                @foreach (var report in Model.Reports)
                {
                    var reportClass = Request.QueryString["reportId"] == report.Id ? "active" : "";
                    <li class="@reportClass">
                        @Html.ActionLink(report.Name, "Report", new { reportId = report.Id })
                    </li>
                }
            </ul>
        </div>
    </div>

Report.cshtml: Nastavte **Model.AccessToken**a ve výrazu Lambda **PowerBIReportFor**.

    @model ReportViewModel

    ...

    <div class="side-body padding-top">
        @Html.PowerBIAccessToken(Model.AccessToken)
        @Html.PowerBIReportFor(m => m.Report, new { style = "height:85vh" })
    </div>

### <a name="controller"></a>Řadiče

**DashboardController.cs**: vytvoří předávání PowerBIClient **tokenu aplikace**. Token JSON Web Token (JWT) se generují z **podpisový klíč** získat **pověření**. **Pověření** slouží k vytvoření instance **PowerBIClient**. Až budete mít instanci **PowerBIClient**, můžete volat GetReports() a GetReportsAsync().

CreatePowerBIClient()

    private IPowerBIClient CreatePowerBIClient()
    {
        var credentials = new TokenCredentials(accessKey, "AppKey");
        var client = new PowerBIClient(credentials)
        {
            BaseUri = new Uri(apiUrl)
        };

        return client;
    }

ActionResult Reports()

    public ActionResult Reports()
    {
        using (var client = this.CreatePowerBIClient())
        {
            var reportsResponse = client.Reports.GetReports(this.workspaceCollection, this.workspaceId);

            var viewModel = new ReportsViewModel
            {
                Reports = reportsResponse.Value.ToList()
            };

            return PartialView(viewModel);
        }
    }


Úloha<ActionResult> sestavy (reportId řetězec)

    public async Task<ActionResult> Report(string reportId)
    {
        using (var client = this.CreatePowerBIClient())
        {
            var reportsResponse = await client.Reports.GetReportsAsync(this.workspaceCollection, this.workspaceId);
            var report = reportsResponse.Value.FirstOrDefault(r => r.Id == reportId);
            var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

            var viewModel = new ReportViewModel
            {
                Report = report,
                AccessToken = embedToken.Generate(this.accessKey)
            };

            return View(viewModel);
        }
    }

### <a name="integrate-a-report-into-your-app"></a>Integraci sestavy do vaší aplikace

Až budete mít **sestavy**, můžete použít **IFrame** pro vložení Power BI **sestavy**. Zde je fragment kódu z powerbi.js v **Microsoft Power BI Embedded** ukázka.

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-iframe-code.png)

## <a name="filter-reports-embedded-in-your-application"></a>Filtrování zpráv vloží do vaší aplikace

Vloženou sestavu můžete filtrovat pomocí syntaxe adresy URL. Chcete-li to provést, je přidat **$filter** parametr řetězce s dotazu **eq** operátor na adresu url elementu iFrame src filtrem. Tady je syntaxe dotazů filtru:

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-...-9673-ee9655d54a4a&
$filter={tableName/fieldName}%20eq%20'{fieldValue}'
```

> [!NOTE]
> V řetězci {tableName/fieldName} nesmí být mezery ani speciální znaky. Na místo položky {FieldValue} je možné vložit jednu hodnotu představující kategorii.  

## <a name="see-also"></a>Viz také

[Běžné scénáře Microsoft Power BI Embedded](power-bi-embedded-scenarios.md)  
[Ověřování a autorizace v Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Vložení sestavy](power-bi-embedded-embed-report.md)  
[Vytvoření nové sestavy z datové sady](power-bi-embedded-create-report-from-dataset.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Vložená ukázka JavaScriptu](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
Chcete se ještě na něco zeptat? [Vyzkoušejte komunitu Power BI](http://community.powerbi.com/)
