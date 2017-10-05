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
# <a name="get-started-with-power-bi-embedded-sample"></a><span data-ttu-id="69c92-103">Začínáme s ukázkou Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="69c92-103">Get started with Power BI Embedded sample</span></span>

<span data-ttu-id="69c92-104">S **Microsoft Power BI Embedded**, sestavy Power BI můžete integrovat přímo do vašeho webu nebo mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="69c92-104">With **Microsoft Power BI Embedded**, you can integrate Power BI reports right into your web or mobile applications.</span></span> <span data-ttu-id="69c92-105">V tomto článku budete zavedeme vám **Power BI Embedded** ukázka Začínáme get.</span><span class="sxs-lookup"><span data-stu-id="69c92-105">In this article, we'll introduce you to the **Power BI Embedded** get started sample.</span></span>

<span data-ttu-id="69c92-106">Předtím, než jsme přejít žádné další, budete pravděpodobně chtít uložit v následujících zdrojích informací.</span><span class="sxs-lookup"><span data-stu-id="69c92-106">Before we go any further, you'll probably want to save the following resources.</span></span> <span data-ttu-id="69c92-107">Budete pomáhají při integraci sestavy Power BI příliš do své vlastní aplikace a ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="69c92-107">They'll help you when integrating Power BI reports into the sample app and your own apps too.</span></span>

* [<span data-ttu-id="69c92-108">Ukázka prostoru webové aplikace</span><span class="sxs-lookup"><span data-stu-id="69c92-108">Sample workspace web app</span></span>](http://go.microsoft.com/fwlink/?LinkId=761493)
* [<span data-ttu-id="69c92-109">Power BI Embedded API – referenční informace</span><span class="sxs-lookup"><span data-stu-id="69c92-109">Power BI Embedded API reference</span></span>](https://msdn.microsoft.com/en-US/library/azure/mt711507.aspx)
* <span data-ttu-id="69c92-110">[Power BI Embedded .NET SDK ](http://go.microsoft.com/fwlink/?LinkId=746472) (k dispozici prostřednictvím balíčku NuGet)</span><span class="sxs-lookup"><span data-stu-id="69c92-110">[Power BI Embedded .NET SDK ](http://go.microsoft.com/fwlink/?LinkId=746472) (available via NuGet)</span></span>
* [<span data-ttu-id="69c92-111">Ukázka vložení sestavy JavaScript</span><span class="sxs-lookup"><span data-stu-id="69c92-111">JavaScript Report Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo)

> [!NOTE] 
> <span data-ttu-id="69c92-112">Než můžete nakonfigurovat a spustit Power BI Embedded získat spuštění ukázkové, musíte vytvořit alespoň jednu **kolekce pracovních prostorů** ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="69c92-112">Before you can configure and run the Power BI Embedded get started sample, you need to create at least one **Workspace Collection** in your Azure subscription.</span></span> <span data-ttu-id="69c92-113">Další informace o vytvoření **kolekce pracovních prostorů** najdete v tématu Azure Portal [Začínáme s Power BI Embedded](power-bi-embedded-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="69c92-113">To learn how to create a **Workspace Collection** in the Azure Portal see [Getting Started with Power BI Embedded](power-bi-embedded-get-started.md).</span></span>

## <a name="configure-the-sample-app"></a><span data-ttu-id="69c92-114">Nakonfigurujte ukázkovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="69c92-114">Configure the sample app</span></span>

<span data-ttu-id="69c92-115">Projděme nastavení vývojového prostředí sady Visual Studio pro přístup k součástem potřebné ke spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="69c92-115">Let's walk through setting up your Visual Studio development environment to access the  components needed to run the sample app.</span></span>

1. <span data-ttu-id="69c92-116">Stáhněte a rozbalte [Power BI Embedded - integraci sestavy do webové aplikace](http://go.microsoft.com/fwlink/?LinkId=761493) ukázku na Githubu.</span><span class="sxs-lookup"><span data-stu-id="69c92-116">Download and unzip the [Power BI Embedded - Integrate a report into a web app](http://go.microsoft.com/fwlink/?LinkId=761493) sample on GitHub.</span></span>
2. <span data-ttu-id="69c92-117">Otevřete **PowerBI embedded.sln** v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="69c92-117">Open **PowerBI-embedded.sln** in Visual Studio.</span></span> <span data-ttu-id="69c92-118">Budete muset provést **balíček aktualizace** příkazu v konzole Správce balíčků NuGET za účelem aktualizace balíčcích používaných v tomto řešení.</span><span class="sxs-lookup"><span data-stu-id="69c92-118">You may need to execute the **Update-Package** command in the NuGET Package Manager Console in order to update the packages used in this solution.</span></span>
3. <span data-ttu-id="69c92-119">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="69c92-119">Build the solution.</span></span>
4. <span data-ttu-id="69c92-120">Spustit **ProvisionSample** konzolovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="69c92-120">Run the **ProvisionSample** console app.</span></span> <span data-ttu-id="69c92-121">V konzole ukázková aplikace zřídíte pracovního prostoru a naimportovat soubor PBIX.</span><span class="sxs-lookup"><span data-stu-id="69c92-121">In the sample console app, you provision a workspace and import a PBIX file.</span></span>
5. <span data-ttu-id="69c92-122">Zřídit novou **prostoru**, vyberte možnost 1, **Správa kolekce**a potom vyberte možnost 6, **zřídit nový pracovní prostor**</span><span class="sxs-lookup"><span data-stu-id="69c92-122">To provision a new **Workspace**, select option 1, **Collection management**, and then select option 6, **Provision a new Workspace**</span></span>
6. <span data-ttu-id="69c92-123">Importovat novou **sestavy**, vyberte možnost 2, **sestav správy**a potom vyberte možnost 3, **naimportovat soubor PBIX plochy souboru do pracovního prostoru**.</span><span class="sxs-lookup"><span data-stu-id="69c92-123">To import a new **Report**, select option 2, **Report management**, and then select option 3, **Import PBIX Desktop file into a workspace**.</span></span>

7. <span data-ttu-id="69c92-124">Zadejte vaše **kolekce pracovních prostorů** název, a **přístupový klíč**.</span><span class="sxs-lookup"><span data-stu-id="69c92-124">Enter your **Workspace Collection** name, and **Access Key**.</span></span> <span data-ttu-id="69c92-125">To můžete získat **portálu Azure**.</span><span class="sxs-lookup"><span data-stu-id="69c92-125">You can get these in the **Azure Portal**.</span></span> <span data-ttu-id="69c92-126">Další informace o tom, jak získat vaše **přístupový klíč**, najdete v části [zobrazit Power BI API přístupové klíče](power-bi-embedded-get-started.md#view-power-bi-api-access-keys) v Začínáme s Microsoft Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="69c92-126">To learn more about how to get your **Access Key**, see [View Power BI API Access Keys](power-bi-embedded-get-started.md#view-power-bi-api-access-keys) in Get started with Microsoft Power BI Embedded.</span></span>

    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
8. <span data-ttu-id="69c92-127">Zkopírujte a uložte nově vytvořený **ID pracovního prostoru** používat později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="69c92-127">Copy and save the newly created **Workspace ID** to use later in this article.</span></span> <span data-ttu-id="69c92-128">Po **ID pracovního prostoru** je vytvořen, najdete ji **portálu Azure**.</span><span class="sxs-lookup"><span data-stu-id="69c92-128">After the **Workspace ID** is created, you can find it the **Azure Portal**.</span></span>

    ![](media/powerbi-embedded-get-started-sample/workspace-id.png)
9. <span data-ttu-id="69c92-129">Chcete-li importovat soubor PBIX do vaší **prostoru**, vyberte možnost **6. Soubor PBIX plochy importu do existujícímu pracovnímu prostoru**.</span><span class="sxs-lookup"><span data-stu-id="69c92-129">To import a PBIX file into your **Workspace**, select option **6. Import PBIX Desktop file into an existing workspace**.</span></span> <span data-ttu-id="69c92-130">Pokud nemáte soubor PBIX souboru užitečný, si můžete stáhnout [prodejní analýzy ukázkový soubor PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).</span><span class="sxs-lookup"><span data-stu-id="69c92-130">If you don't have a PBIX file handy, you can download the [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).</span></span>
10. <span data-ttu-id="69c92-131">Pokud se zobrazí výzva, zadejte popisný název pro vaše **datovou sadu**.</span><span class="sxs-lookup"><span data-stu-id="69c92-131">If prompted, enter a friendly name for your **Dataset**.</span></span>

<span data-ttu-id="69c92-132">Zobrazí takováto odpověď:</span><span class="sxs-lookup"><span data-stu-id="69c92-132">You should see a response like:</span></span>

```
Checking import state... Publishing
Checking import state... Succeeded
```

> [!NOTE]
> <span data-ttu-id="69c92-133">Pokud váš soubor PBIX obsahuje všechna připojení přímý dotaz, spusťte možnost 7 aktualizovat připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="69c92-133">If your PBIX file contains any direct query connections, run option 7 to update the connection strings.</span></span>

<span data-ttu-id="69c92-134">V tuto chvíli máte soubor PBIX Power BI sestavu importovat do vaší **prostoru**.</span><span class="sxs-lookup"><span data-stu-id="69c92-134">At this point, you have a Power BI PBIX report imported into your **Workspace**.</span></span> <span data-ttu-id="69c92-135">Teď se podíváme, jak spouštět **Power BI Embedded** získat Začínáme ukázkovou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="69c92-135">Now, let's look at how to run the **Power BI Embedded** get started sample web app.</span></span>

## <a name="run-the-sample-web-app"></a><span data-ttu-id="69c92-136">Spuštění ukázkové webové aplikace</span><span class="sxs-lookup"><span data-stu-id="69c92-136">Run the sample web app</span></span>
<span data-ttu-id="69c92-137">Ukázka aplikace web je ukázkovou aplikaci, která vykreslí sestavy importovat do vaší **prostoru**.</span><span class="sxs-lookup"><span data-stu-id="69c92-137">The web app sample is a sample application that renders reports imported into your **Workspace**.</span></span> <span data-ttu-id="69c92-138">Zde je postup konfigurace ukázky webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="69c92-138">Here's how to configure the web app sample.</span></span>

1. <span data-ttu-id="69c92-139">V **vložených PowerBI** řešení sady Visual Studio, klikněte pravým tlačítkem **EmbedSample** webové aplikace a zvolte **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="69c92-139">In the **PowerBI-embedded** Visual Studio solution, right click the **EmbedSample** web application, and choose **Set as StartUp project**.</span></span>
2. <span data-ttu-id="69c92-140">V **web.config**v **EmbedSample** webovou aplikaci, upravit **appSettings**: **AccessKey**, **WorkspaceCollection** název, a **WorkspaceId**.</span><span class="sxs-lookup"><span data-stu-id="69c92-140">In **web.config**, in the **EmbedSample** web application, edit the **appSettings**: **AccessKey**, **WorkspaceCollection** name, and **WorkspaceId**.</span></span>

    ```
    <appSettings>
        <add key="powerbi:AccessKey" value="" />
        <add key="powerbi:ApiUrl" value="https://api.powerbi.com" />
        <add key="powerbi:WorkspaceCollection" value="" />
        <add key="powerbi:WorkspaceId" value="" />
    </appSettings>
    ```
3. <span data-ttu-id="69c92-141">Spustit **EmbedSample** webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="69c92-141">Run the **EmbedSample** web application.</span></span>

<span data-ttu-id="69c92-142">Po spuštění **EmbedSample** webové aplikace, musí obsahovat levém navigačním panelu **sestavy** nabídky.</span><span class="sxs-lookup"><span data-stu-id="69c92-142">Once you run the **EmbedSample** web application, the left navigation panel should contain a **Reports** menu.</span></span> <span data-ttu-id="69c92-143">Chcete-li zobrazit sestavu jste importovali, rozbalte **sestavy**a klikněte na sestavu.</span><span class="sxs-lookup"><span data-stu-id="69c92-143">To view the report you imported, expand **Reports**, and click a report.</span></span> <span data-ttu-id="69c92-144">Pokud jste importovali [prodejní analýzy ukázkový soubor PBIX](http://go.microsoft.com/fwlink/?LinkID=780547), ukázkovou webovou aplikaci bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="69c92-144">If you imported the [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547), the sample web app would look like this:</span></span>

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-sample-left-nav.png)

<span data-ttu-id="69c92-145">Po kliknutí na tlačítko sestavy, **EmbedSample** webová aplikace by měl vypadat přibližně toto:</span><span class="sxs-lookup"><span data-stu-id="69c92-145">After you click a report, the **EmbedSample** web application should look something this:</span></span>

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="explore-the-sample-code"></a><span data-ttu-id="69c92-146">Prozkoumejte ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="69c92-146">Explore the sample code</span></span>

<span data-ttu-id="69c92-147">**Microsoft Power BI Embedded** ukázka je ukázkové webové aplikace, která ukazuje, jak integrovat **Power BI** sestavy do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="69c92-147">The **Microsoft Power BI Embedded** sample is an example web app that shows you how to integrate **Power BI** reports into your app.</span></span> <span data-ttu-id="69c92-148">Návrhový vzor Model-View-Controller (MVC) používá k předvedení osvědčené postupy.</span><span class="sxs-lookup"><span data-stu-id="69c92-148">It uses a Model-View-Controller (MVC) design pattern to demonstrate best practices.</span></span> <span data-ttu-id="69c92-149">V této části jsou zdůrazněné částí ukázkový kód, který můžete prozkoumat v rámci **vložených PowerBI** webové aplikace řešení.</span><span class="sxs-lookup"><span data-stu-id="69c92-149">This section highlights parts of the sample code that you can explore within the **PowerBI-embedded** web app solution.</span></span> <span data-ttu-id="69c92-150">Vzor Model-View-Controller (MVC) odděluje modelování domény, prezentace a akce založené na vstup uživatele na tři samostatné třídy: Model, zobrazení a řízení.</span><span class="sxs-lookup"><span data-stu-id="69c92-150">The Model-View-Controller (MVC) pattern separates the modeling of the domain, the presentation, and the actions based on user input into three separate classes: Model, View, and Control.</span></span> <span data-ttu-id="69c92-151">Další informace o MVC naleznete v tématu [Další informace o ASP.NET](http://www.asp.net/mvc).</span><span class="sxs-lookup"><span data-stu-id="69c92-151">To learn more about MVC, see [Learn About ASP.NET](http://www.asp.net/mvc).</span></span>

<span data-ttu-id="69c92-152">**Microsoft Power BI Embedded** ukázkový kód je oddělená následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="69c92-152">The **Microsoft Power BI Embedded** sample code is separated as follows.</span></span> <span data-ttu-id="69c92-153">Každá část zahrnuje název souboru v řešení PowerBI embedded.sln, takže budete moci snadno najít kód v ukázce.</span><span class="sxs-lookup"><span data-stu-id="69c92-153">Each section includes the file name in the PowerBI-embedded.sln solution so that you can easily find the code in the sample.</span></span>

> [!NOTE]
> <span data-ttu-id="69c92-154">V této části je uveden seznam ukázkový kód, který ukazuje, jak byla zapsána kód.</span><span class="sxs-lookup"><span data-stu-id="69c92-154">This section is a summary of the sample code that shows how the code was written.</span></span> <span data-ttu-id="69c92-155">Chcete-li zobrazit celý vzorek, načtěte PowerBI embedded.sln řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="69c92-155">To view the complete sample, please load the PowerBI-embedded.sln solution in Visual Studio.</span></span>

### <a name="model"></a><span data-ttu-id="69c92-156">Model</span><span class="sxs-lookup"><span data-stu-id="69c92-156">Model</span></span>

<span data-ttu-id="69c92-157">Je vzorku **ReportsViewModel** a **ReportViewModel**.</span><span class="sxs-lookup"><span data-stu-id="69c92-157">The sample has a **ReportsViewModel** and **ReportViewModel**.</span></span>

<span data-ttu-id="69c92-158">**ReportsViewModel.cs**: představuje sestavy Power BI.</span><span class="sxs-lookup"><span data-stu-id="69c92-158">**ReportsViewModel.cs**: Represents Power BI Reports.</span></span>

    public class ReportsViewModel
    {
        public List<Report> Reports { get; set; }
    }

<span data-ttu-id="69c92-159">**ReportViewModel.cs**: představuje sestavy Power BI.</span><span class="sxs-lookup"><span data-stu-id="69c92-159">**ReportViewModel.cs**: Represents a Power BI Report.</span></span>

    public classReportViewModel
    {
        public IReport Report { get; set; }

        public string AccessToken { get; set; }
    }

### <a name="connection-string"></a><span data-ttu-id="69c92-160">Připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="69c92-160">Connection string</span></span>

<span data-ttu-id="69c92-161">Připojovací řetězec musí být v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="69c92-161">The connection string must be in the following format:</span></span>

```
Data Source=tcp:MyServer.database.windows.net,1433;Initial Catalog=MyDatabase
```

<span data-ttu-id="69c92-162">Pomocí běžné atributy server a databáze se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="69c92-162">Using common server and database attributes will fail.</span></span> <span data-ttu-id="69c92-163">Příklad: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,</span><span class="sxs-lookup"><span data-stu-id="69c92-163">For example: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,</span></span>

### <a name="view"></a><span data-ttu-id="69c92-164">Zobrazení</span><span class="sxs-lookup"><span data-stu-id="69c92-164">View</span></span>

<span data-ttu-id="69c92-165">**Zobrazení** spravuje zobrazení Power BI **sestavy** a Power BI **sestavy**.</span><span class="sxs-lookup"><span data-stu-id="69c92-165">The **View** manages the display of Power BI **Reports** and a Power BI **Report**.</span></span>

<span data-ttu-id="69c92-166">**Reports.cshtml**: iterace v **Model.Reports** vytvořit **ActionLink**.</span><span class="sxs-lookup"><span data-stu-id="69c92-166">**Reports.cshtml**: Iterate over **Model.Reports** to create an **ActionLink**.</span></span> <span data-ttu-id="69c92-167">**ActionLink** se skládá následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="69c92-167">The **ActionLink** is composed as follows:</span></span>

| <span data-ttu-id="69c92-168">Část</span><span class="sxs-lookup"><span data-stu-id="69c92-168">Part</span></span> | <span data-ttu-id="69c92-169">Popis</span><span class="sxs-lookup"><span data-stu-id="69c92-169">Description</span></span> |
| --- | --- |
| <span data-ttu-id="69c92-170">Název</span><span class="sxs-lookup"><span data-stu-id="69c92-170">Title</span></span> |<span data-ttu-id="69c92-171">Název sestavy.</span><span class="sxs-lookup"><span data-stu-id="69c92-171">Name of the Report.</span></span> |
| <span data-ttu-id="69c92-172">Řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="69c92-172">QueryString</span></span> |<span data-ttu-id="69c92-173">Odkaz na ID sestavy.</span><span class="sxs-lookup"><span data-stu-id="69c92-173">A link to the Report ID.</span></span> |

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

<span data-ttu-id="69c92-174">Report.cshtml: Nastavte **Model.AccessToken**a ve výrazu Lambda **PowerBIReportFor**.</span><span class="sxs-lookup"><span data-stu-id="69c92-174">Report.cshtml: Set the **Model.AccessToken**, and the Lambda expression for **PowerBIReportFor**.</span></span>

    @model ReportViewModel

    ...

    <div class="side-body padding-top">
        @Html.PowerBIAccessToken(Model.AccessToken)
        @Html.PowerBIReportFor(m => m.Report, new { style = "height:85vh" })
    </div>

### <a name="controller"></a><span data-ttu-id="69c92-175">Řadiče</span><span class="sxs-lookup"><span data-stu-id="69c92-175">Controller</span></span>

<span data-ttu-id="69c92-176">**DashboardController.cs**: vytvoří předávání PowerBIClient **tokenu aplikace**.</span><span class="sxs-lookup"><span data-stu-id="69c92-176">**DashboardController.cs**: Creates a PowerBIClient passing an **app token**.</span></span> <span data-ttu-id="69c92-177">Token JSON Web Token (JWT) se generují z **podpisový klíč** získat **pověření**.</span><span class="sxs-lookup"><span data-stu-id="69c92-177">A JSON Web Token (JWT) is generated from the **Signing Key** to get the **Credentials**.</span></span> <span data-ttu-id="69c92-178">**Pověření** slouží k vytvoření instance **PowerBIClient**.</span><span class="sxs-lookup"><span data-stu-id="69c92-178">The **Credentials** are used to create an instance of **PowerBIClient**.</span></span> <span data-ttu-id="69c92-179">Až budete mít instanci **PowerBIClient**, můžete volat GetReports() a GetReportsAsync().</span><span class="sxs-lookup"><span data-stu-id="69c92-179">Once you have an instance of **PowerBIClient**, you can call GetReports() and GetReportsAsync().</span></span>

<span data-ttu-id="69c92-180">CreatePowerBIClient()</span><span class="sxs-lookup"><span data-stu-id="69c92-180">CreatePowerBIClient()</span></span>

    private IPowerBIClient CreatePowerBIClient()
    {
        var credentials = new TokenCredentials(accessKey, "AppKey");
        var client = new PowerBIClient(credentials)
        {
            BaseUri = new Uri(apiUrl)
        };

        return client;
    }

<span data-ttu-id="69c92-181">ActionResult Reports()</span><span class="sxs-lookup"><span data-stu-id="69c92-181">ActionResult Reports()</span></span>

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


<span data-ttu-id="69c92-182">Úloha<ActionResult> sestavy (reportId řetězec)</span><span class="sxs-lookup"><span data-stu-id="69c92-182">Task<ActionResult> Report(string reportId)</span></span>

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

### <a name="integrate-a-report-into-your-app"></a><span data-ttu-id="69c92-183">Integraci sestavy do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="69c92-183">Integrate a report into your app</span></span>

<span data-ttu-id="69c92-184">Až budete mít **sestavy**, můžete použít **IFrame** pro vložení Power BI **sestavy**.</span><span class="sxs-lookup"><span data-stu-id="69c92-184">Once you have a **Report**, you use an **IFrame** to embed the Power BI **Report**.</span></span> <span data-ttu-id="69c92-185">Zde je fragment kódu z powerbi.js v **Microsoft Power BI Embedded** ukázka.</span><span class="sxs-lookup"><span data-stu-id="69c92-185">Here is a code snippet from  powerbi.js in the **Microsoft Power BI Embedded** sample.</span></span>

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-iframe-code.png)

## <a name="filter-reports-embedded-in-your-application"></a><span data-ttu-id="69c92-186">Filtrování zpráv vloží do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="69c92-186">Filter reports embedded in your application</span></span>

<span data-ttu-id="69c92-187">Vloženou sestavu můžete filtrovat pomocí syntaxe adresy URL.</span><span class="sxs-lookup"><span data-stu-id="69c92-187">You can filter an embedded report using a URL syntax.</span></span> <span data-ttu-id="69c92-188">Chcete-li to provést, je přidat **$filter** parametr řetězce s dotazu **eq** operátor na adresu url elementu iFrame src filtrem.</span><span class="sxs-lookup"><span data-stu-id="69c92-188">To do this, you add a **$filter** query string parameter with an **eq** operator to your iFrame src url with the filter specified.</span></span> <span data-ttu-id="69c92-189">Tady je syntaxe dotazů filtru:</span><span class="sxs-lookup"><span data-stu-id="69c92-189">Here is the filter query syntax:</span></span>

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-...-9673-ee9655d54a4a&
$filter={tableName/fieldName}%20eq%20'{fieldValue}'
```

> [!NOTE]
> <span data-ttu-id="69c92-190">V řetězci {tableName/fieldName} nesmí být mezery ani speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="69c92-190">{tableName/fieldName} cannot include spaces or special characters.</span></span> <span data-ttu-id="69c92-191">Na místo položky {FieldValue} je možné vložit jednu hodnotu představující kategorii.</span><span class="sxs-lookup"><span data-stu-id="69c92-191">The {fieldValue} accepts a single categorical value.</span></span>  

## <a name="see-also"></a><span data-ttu-id="69c92-192">Viz také</span><span class="sxs-lookup"><span data-stu-id="69c92-192">See also</span></span>

[<span data-ttu-id="69c92-193">Běžné scénáře Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="69c92-193">Common Microsoft Power BI Embedded scenarios</span></span>](power-bi-embedded-scenarios.md)  
[<span data-ttu-id="69c92-194">Ověřování a autorizace v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="69c92-194">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="69c92-195">Vložení sestavy</span><span class="sxs-lookup"><span data-stu-id="69c92-195">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="69c92-196">Vytvoření nové sestavy z datové sady</span><span class="sxs-lookup"><span data-stu-id="69c92-196">Create a new report from a dataset</span></span>](power-bi-embedded-create-report-from-dataset.md)  
[<span data-ttu-id="69c92-197">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="69c92-197">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="69c92-198">Vložená ukázka JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="69c92-198">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="69c92-199">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="69c92-199">More questions?</span></span> [<span data-ttu-id="69c92-200">Vyzkoušejte komunitu Power BI</span><span class="sxs-lookup"><span data-stu-id="69c92-200">Try the Power BI Community</span></span>](http://community.powerbi.com/)
