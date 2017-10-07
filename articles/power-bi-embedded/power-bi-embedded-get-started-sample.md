---
title: "aaaGet s ukázkovým pracovat"
description: "Power BI Embedded, použijte interaktivní Power BI sestavy do své aplikace business intelligence tooadd SDK"
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
ms.openlocfilehash: 1fef9dd8e0f734b748b930d3f85ad4b517d9661e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-power-bi-embedded-sample"></a>Začínáme s ukázkou Power BI Embedded

S **Microsoft Power BI Embedded**, sestavy Power BI můžete integrovat přímo do vašeho webu nebo mobilní aplikace. V tomto článku budete zavedeme jste toohello **Power BI Embedded** ukázka Začínáme get.

Předtím, než jsme přejít žádné další, budete muset pravděpodobně toosave hello následující prostředky. Budete pomáhají při integraci sestavy Power BI příliš do hello ukázkovou aplikaci a vlastní aplikace.

* [Ukázka prostoru webové aplikace](http://go.microsoft.com/fwlink/?LinkId=761493)
* [Power BI Embedded API – referenční informace](https://msdn.microsoft.com/en-US/library/azure/mt711507.aspx)
* [Power BI Embedded .NET SDK ](http://go.microsoft.com/fwlink/?LinkId=746472) (k dispozici prostřednictvím balíčku NuGet)
* [Ukázka vložení sestavy JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo)

> [!NOTE] 
> Než můžete nakonfigurovat a spustit hello Power BI Embedded získat spuštění ukázkové, je třeba toocreate alespoň jeden **kolekce pracovních prostorů** ve vašem předplatném Azure. toolearn jak toocreate **kolekce pracovních prostorů** v hello portálu Azure najdete v části [Začínáme s Power BI Embedded](power-bi-embedded-get-started.md).

## <a name="configure-hello-sample-app"></a>Konfigurace hello ukázkové aplikace

Projděme nastavení sady Visual Studio vývoj prostředí tooaccess hello součásti potřebné toorun hello ukázkové aplikace.

1. Stáhněte a rozbalte hello [Power BI Embedded - integraci sestavy do webové aplikace](http://go.microsoft.com/fwlink/?LinkId=761493) ukázku na Githubu.
2. Otevřete **PowerBI embedded.sln** v sadě Visual Studio. Může být nutné tooexecute hello **balíček aktualizace** v hello konzoly Správce balíčků NuGET v pořadí tooupdate hello balíčky použité v tomto řešení.
3. Vytvoření řešení hello.
4. Spustit hello **ProvisionSample** konzolovou aplikaci. Ve hello ukázkovou aplikaci konzoly můžete zřídit pracovního prostoru a naimportovat soubor PBIX.
5. tooprovision nový **prostoru**, vyberte možnost 1, **Správa kolekce**a potom vyberte možnost 6, **zřídit nový pracovní prostor**
6. tooimport nový **sestavy**, vyberte možnost 2, **sestav správy**a potom vyberte možnost 3, **plochy soubor PBIX Import souboru do pracovního prostoru**.

7. Zadejte vaše **kolekce pracovních prostorů** název, a **přístupový klíč**. Můžete získat v hello **portálu Azure**. toolearn více o tooget vaše **přístupový klíč**, najdete v části [zobrazit Power BI API přístupové klíče](power-bi-embedded-get-started.md#view-power-bi-api-access-keys) v Začínáme s Microsoft Power BI Embedded.

    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
8. Zkopírujte a uložte nově vytvořený hello **ID pracovního prostoru** toouse později v tomto článku. Po hello **ID pracovního prostoru** je vytvořen, najdete ji hello **portálu Azure**.

    ![](media/powerbi-embedded-get-started-sample/workspace-id.png)
9. tooimport soubor PBIX soubor do vaší **prostoru**, vyberte možnost **6. Soubor PBIX plochy importu do existujícímu pracovnímu prostoru**. Pokud nemáte soubor PBIX souboru užitečný, si můžete stáhnout hello [prodejní analýzy ukázkový soubor PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).
10. Pokud se zobrazí výzva, zadejte popisný název pro vaše **datovou sadu**.

Zobrazí takováto odpověď:

```
Checking import state... Publishing
Checking import state... Succeeded
```

> [!NOTE]
> Pokud váš soubor PBIX obsahuje všechna připojení přímý dotaz, spusťte možnost 7 tooupdate hello připojovací řetězce.

V tuto chvíli máte soubor PBIX Power BI sestavu importovat do vaší **prostoru**. Teď se podíváme, jak toorun hello **Power BI Embedded** získat Začínáme ukázkovou webovou aplikaci.

## <a name="run-hello-sample-web-app"></a>Spustit hello ukázkovou webovou aplikaci
Hello webové aplikace ukázka je ukázkovou aplikaci, která vykreslí sestavy importovat do vaší **prostoru**. Zde je, jak tooconfigure hello ukázkové webové aplikace.

1. V hello **vložených PowerBI** řešení sady Visual Studio, správné, klikněte na tlačítko hello **EmbedSample** webové aplikace a zvolte **nastavit jako spouštěný projekt**.
2. V **web.config**, v hello **EmbedSample** webovou aplikaci, upravit hello **appSettings**: **AccessKey**,  **WorkspaceCollection** název, a **WorkspaceId**.

    ```
    <appSettings>
        <add key="powerbi:AccessKey" value="" />
        <add key="powerbi:ApiUrl" value="https://api.powerbi.com" />
        <add key="powerbi:WorkspaceCollection" value="" />
        <add key="powerbi:WorkspaceId" value="" />
    </appSettings>
    ```
3. Spustit hello **EmbedSample** webové aplikace.

Po spuštění hello **EmbedSample** webové aplikace, musí obsahovat hello levém navigačním panelu **sestavy** nabídky. Sestava hello tooview jste importovali, rozbalte **sestavy**a klikněte na sestavu. Pokud jste importovali hello [prodejní analýzy ukázkový soubor PBIX](http://go.microsoft.com/fwlink/?LinkID=780547), hello ukázkovou webovou aplikaci bude vypadat takto:

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-sample-left-nav.png)

Po kliknutí na tlačítko sestavy, hello **EmbedSample** webová aplikace by měl vypadat přibližně toto:

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="explore-hello-sample-code"></a>Prozkoumejte hello ukázkový kód

Hello **Microsoft Power BI Embedded** ukázka je ukázkové webové aplikace, která ukazuje, jak toointegrate **Power BI** sestavy do vaší aplikace. Používá Model-View-Controller (MVC) návrh vzor toodemonstrate osvědčené postupy. V této části jsou zdůrazněné částí hello ukázkový kód, který můžete prozkoumat v rámci hello **vložených PowerBI** webové aplikace řešení. Hello vzor Model-View-Controller (MVC) odděluje hello modelování hello domény, hello prezentace a hello akce založené na vstup uživatele na tři samostatné třídy: Model, zobrazení a řízení. toolearn Další informace o MVC, najdete v části [Další informace o ASP.NET](http://www.asp.net/mvc).

Hello **Microsoft Power BI Embedded** ukázkový kód je oddělená následujícím způsobem. Každý oddíl obsahuje název souboru hello v hello PowerBI embedded.sln řešení tak, že budete moci snadno najít hello kód v ukázce hello.

> [!NOTE]
> V této části je uveden seznam hello ukázkový kód, který ukazuje, jak byla zapsána hello kódu. tooview hello dokončení ukázkové, načtěte hello PowerBI embedded.sln řešení v sadě Visual Studio.

### <a name="model"></a>Model

Ukázka Hello má **ReportsViewModel** a **ReportViewModel**.

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

Hello připojovací řetězec musí být ve formátu hello:

```
Data Source=tcp:MyServer.database.windows.net,1433;Initial Catalog=MyDatabase
```

Pomocí běžné atributy server a databáze se nezdaří. Příklad: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,

### <a name="view"></a>Zobrazení

Hello **zobrazení** spravuje hello zobrazení Power BI **sestavy** a Power BI **sestavy**.

**Reports.cshtml**: iterace v **Model.Reports** toocreate **ActionLink**. Hello **ActionLink** se skládá následujícím způsobem:

| Část | Popis |
| --- | --- |
| Název |Název hello sestavy. |
| Řetězce dotazu |Odkaz toohello ID sestavy. |

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

Report.cshtml: Nastavte hello **Model.AccessToken**, a hello výrazu Lambda pro **PowerBIReportFor**.

    @model ReportViewModel

    ...

    <div class="side-body padding-top">
        @Html.PowerBIAccessToken(Model.AccessToken)
        @Html.PowerBIReportFor(m => m.Report, new { style = "height:85vh" })
    </div>

### <a name="controller"></a>Řadiče

**DashboardController.cs**: vytvoří předávání PowerBIClient **tokenu aplikace**. Token JSON Web Token (JWT) se generují z hello **podpisový klíč** tooget hello **pověření**. Hello **pověření** jsou použité toocreate instanci **PowerBIClient**. Až budete mít instanci **PowerBIClient**, můžete volat GetReports() a GetReportsAsync().

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

Až budete mít **sestavy**, můžete použít **IFrame** tooembed hello Power BI **sestavy**. Zde je fragment kódu z powerbi.js v hello **Microsoft Power BI Embedded** ukázka.

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-iframe-code.png)

## <a name="filter-reports-embedded-in-your-application"></a>Filtrování zpráv vloží do vaší aplikace

Vloženou sestavu můžete filtrovat pomocí syntaxe adresy URL. toodo, přidáte **$filter** parametr řetězce s dotazu **eq** operátor tooyour iFrame src url s filtrem hello. Tady je hello se syntaxí filtru dotazu:

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-...-9673-ee9655d54a4a&
$filter={tableName/fieldName}%20eq%20'{fieldValue}'
```

> [!NOTE]
> V řetězci {tableName/fieldName} nesmí být mezery ani speciální znaky. Hello {místo položky fieldValue} je možné zadat jednu hodnotu představující kategorii.  

## <a name="see-also"></a>Viz také

[Běžné scénáře Microsoft Power BI Embedded](power-bi-embedded-scenarios.md)  
[Ověřování a autorizace v Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Vložení sestavy](power-bi-embedded-embed-report.md)  
[Vytvoření nové sestavy z datové sady](power-bi-embedded-create-report-from-dataset.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Vložená ukázka JavaScriptu](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
Chcete se ještě na něco zeptat? [Zkuste hello komunitě Power BI](http://community.powerbi.com/)
