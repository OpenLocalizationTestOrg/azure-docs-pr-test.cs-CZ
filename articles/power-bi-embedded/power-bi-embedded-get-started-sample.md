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
# <a name="get-started-with-power-bi-embedded-sample"></a><span data-ttu-id="1bba1-103">Začínáme s ukázkou Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="1bba1-103">Get started with Power BI Embedded sample</span></span>

<span data-ttu-id="1bba1-104">S **Microsoft Power BI Embedded**, sestavy Power BI můžete integrovat přímo do vašeho webu nebo mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="1bba1-104">With **Microsoft Power BI Embedded**, you can integrate Power BI reports right into your web or mobile applications.</span></span> <span data-ttu-id="1bba1-105">V tomto článku budete zavedeme jste toohello **Power BI Embedded** ukázka Začínáme get.</span><span class="sxs-lookup"><span data-stu-id="1bba1-105">In this article, we'll introduce you toohello **Power BI Embedded** get started sample.</span></span>

<span data-ttu-id="1bba1-106">Předtím, než jsme přejít žádné další, budete muset pravděpodobně toosave hello následující prostředky.</span><span class="sxs-lookup"><span data-stu-id="1bba1-106">Before we go any further, you'll probably want toosave hello following resources.</span></span> <span data-ttu-id="1bba1-107">Budete pomáhají při integraci sestavy Power BI příliš do hello ukázkovou aplikaci a vlastní aplikace.</span><span class="sxs-lookup"><span data-stu-id="1bba1-107">They'll help you when integrating Power BI reports into hello sample app and your own apps too.</span></span>

* [<span data-ttu-id="1bba1-108">Ukázka prostoru webové aplikace</span><span class="sxs-lookup"><span data-stu-id="1bba1-108">Sample workspace web app</span></span>](http://go.microsoft.com/fwlink/?LinkId=761493)
* [<span data-ttu-id="1bba1-109">Power BI Embedded API – referenční informace</span><span class="sxs-lookup"><span data-stu-id="1bba1-109">Power BI Embedded API reference</span></span>](https://msdn.microsoft.com/en-US/library/azure/mt711507.aspx)
* <span data-ttu-id="1bba1-110">[Power BI Embedded .NET SDK ](http://go.microsoft.com/fwlink/?LinkId=746472) (k dispozici prostřednictvím balíčku NuGet)</span><span class="sxs-lookup"><span data-stu-id="1bba1-110">[Power BI Embedded .NET SDK ](http://go.microsoft.com/fwlink/?LinkId=746472) (available via NuGet)</span></span>
* [<span data-ttu-id="1bba1-111">Ukázka vložení sestavy JavaScript</span><span class="sxs-lookup"><span data-stu-id="1bba1-111">JavaScript Report Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo)

> [!NOTE] 
> <span data-ttu-id="1bba1-112">Než můžete nakonfigurovat a spustit hello Power BI Embedded získat spuštění ukázkové, je třeba toocreate alespoň jeden **kolekce pracovních prostorů** ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="1bba1-112">Before you can configure and run hello Power BI Embedded get started sample, you need toocreate at least one **Workspace Collection** in your Azure subscription.</span></span> <span data-ttu-id="1bba1-113">toolearn jak toocreate **kolekce pracovních prostorů** v hello portálu Azure najdete v části [Začínáme s Power BI Embedded](power-bi-embedded-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1bba1-113">toolearn how toocreate a **Workspace Collection** in hello Azure Portal see [Getting Started with Power BI Embedded](power-bi-embedded-get-started.md).</span></span>

## <a name="configure-hello-sample-app"></a><span data-ttu-id="1bba1-114">Konfigurace hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="1bba1-114">Configure hello sample app</span></span>

<span data-ttu-id="1bba1-115">Projděme nastavení sady Visual Studio vývoj prostředí tooaccess hello součásti potřebné toorun hello ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1bba1-115">Let's walk through setting up your Visual Studio development environment tooaccess hello  components needed toorun hello sample app.</span></span>

1. <span data-ttu-id="1bba1-116">Stáhněte a rozbalte hello [Power BI Embedded - integraci sestavy do webové aplikace](http://go.microsoft.com/fwlink/?LinkId=761493) ukázku na Githubu.</span><span class="sxs-lookup"><span data-stu-id="1bba1-116">Download and unzip hello [Power BI Embedded - Integrate a report into a web app](http://go.microsoft.com/fwlink/?LinkId=761493) sample on GitHub.</span></span>
2. <span data-ttu-id="1bba1-117">Otevřete **PowerBI embedded.sln** v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1bba1-117">Open **PowerBI-embedded.sln** in Visual Studio.</span></span> <span data-ttu-id="1bba1-118">Může být nutné tooexecute hello **balíček aktualizace** v hello konzoly Správce balíčků NuGET v pořadí tooupdate hello balíčky použité v tomto řešení.</span><span class="sxs-lookup"><span data-stu-id="1bba1-118">You may need tooexecute hello **Update-Package** command in hello NuGET Package Manager Console in order tooupdate hello packages used in this solution.</span></span>
3. <span data-ttu-id="1bba1-119">Vytvoření řešení hello.</span><span class="sxs-lookup"><span data-stu-id="1bba1-119">Build hello solution.</span></span>
4. <span data-ttu-id="1bba1-120">Spustit hello **ProvisionSample** konzolovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1bba1-120">Run hello **ProvisionSample** console app.</span></span> <span data-ttu-id="1bba1-121">Ve hello ukázkovou aplikaci konzoly můžete zřídit pracovního prostoru a naimportovat soubor PBIX.</span><span class="sxs-lookup"><span data-stu-id="1bba1-121">In hello sample console app, you provision a workspace and import a PBIX file.</span></span>
5. <span data-ttu-id="1bba1-122">tooprovision nový **prostoru**, vyberte možnost 1, **Správa kolekce**a potom vyberte možnost 6, **zřídit nový pracovní prostor**</span><span class="sxs-lookup"><span data-stu-id="1bba1-122">tooprovision a new **Workspace**, select option 1, **Collection management**, and then select option 6, **Provision a new Workspace**</span></span>
6. <span data-ttu-id="1bba1-123">tooimport nový **sestavy**, vyberte možnost 2, **sestav správy**a potom vyberte možnost 3, **plochy soubor PBIX Import souboru do pracovního prostoru**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-123">tooimport a new **Report**, select option 2, **Report management**, and then select option 3, **Import PBIX Desktop file into a workspace**.</span></span>

7. <span data-ttu-id="1bba1-124">Zadejte vaše **kolekce pracovních prostorů** název, a **přístupový klíč**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-124">Enter your **Workspace Collection** name, and **Access Key**.</span></span> <span data-ttu-id="1bba1-125">Můžete získat v hello **portálu Azure**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-125">You can get these in hello **Azure Portal**.</span></span> <span data-ttu-id="1bba1-126">toolearn více o tooget vaše **přístupový klíč**, najdete v části [zobrazit Power BI API přístupové klíče](power-bi-embedded-get-started.md#view-power-bi-api-access-keys) v Začínáme s Microsoft Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="1bba1-126">toolearn more about how tooget your **Access Key**, see [View Power BI API Access Keys](power-bi-embedded-get-started.md#view-power-bi-api-access-keys) in Get started with Microsoft Power BI Embedded.</span></span>

    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
8. <span data-ttu-id="1bba1-127">Zkopírujte a uložte nově vytvořený hello **ID pracovního prostoru** toouse později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="1bba1-127">Copy and save hello newly created **Workspace ID** toouse later in this article.</span></span> <span data-ttu-id="1bba1-128">Po hello **ID pracovního prostoru** je vytvořen, najdete ji hello **portálu Azure**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-128">After hello **Workspace ID** is created, you can find it hello **Azure Portal**.</span></span>

    ![](media/powerbi-embedded-get-started-sample/workspace-id.png)
9. <span data-ttu-id="1bba1-129">tooimport soubor PBIX soubor do vaší **prostoru**, vyberte možnost **6. Soubor PBIX plochy importu do existujícímu pracovnímu prostoru**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-129">tooimport a PBIX file into your **Workspace**, select option **6. Import PBIX Desktop file into an existing workspace**.</span></span> <span data-ttu-id="1bba1-130">Pokud nemáte soubor PBIX souboru užitečný, si můžete stáhnout hello [prodejní analýzy ukázkový soubor PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).</span><span class="sxs-lookup"><span data-stu-id="1bba1-130">If you don't have a PBIX file handy, you can download hello [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).</span></span>
10. <span data-ttu-id="1bba1-131">Pokud se zobrazí výzva, zadejte popisný název pro vaše **datovou sadu**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-131">If prompted, enter a friendly name for your **Dataset**.</span></span>

<span data-ttu-id="1bba1-132">Zobrazí takováto odpověď:</span><span class="sxs-lookup"><span data-stu-id="1bba1-132">You should see a response like:</span></span>

```
Checking import state... Publishing
Checking import state... Succeeded
```

> [!NOTE]
> <span data-ttu-id="1bba1-133">Pokud váš soubor PBIX obsahuje všechna připojení přímý dotaz, spusťte možnost 7 tooupdate hello připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="1bba1-133">If your PBIX file contains any direct query connections, run option 7 tooupdate hello connection strings.</span></span>

<span data-ttu-id="1bba1-134">V tuto chvíli máte soubor PBIX Power BI sestavu importovat do vaší **prostoru**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-134">At this point, you have a Power BI PBIX report imported into your **Workspace**.</span></span> <span data-ttu-id="1bba1-135">Teď se podíváme, jak toorun hello **Power BI Embedded** získat Začínáme ukázkovou webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1bba1-135">Now, let's look at how toorun hello **Power BI Embedded** get started sample web app.</span></span>

## <a name="run-hello-sample-web-app"></a><span data-ttu-id="1bba1-136">Spustit hello ukázkovou webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="1bba1-136">Run hello sample web app</span></span>
<span data-ttu-id="1bba1-137">Hello webové aplikace ukázka je ukázkovou aplikaci, která vykreslí sestavy importovat do vaší **prostoru**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-137">hello web app sample is a sample application that renders reports imported into your **Workspace**.</span></span> <span data-ttu-id="1bba1-138">Zde je, jak tooconfigure hello ukázkové webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1bba1-138">Here's how tooconfigure hello web app sample.</span></span>

1. <span data-ttu-id="1bba1-139">V hello **vložených PowerBI** řešení sady Visual Studio, správné, klikněte na tlačítko hello **EmbedSample** webové aplikace a zvolte **nastavit jako spouštěný projekt**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-139">In hello **PowerBI-embedded** Visual Studio solution, right click hello **EmbedSample** web application, and choose **Set as StartUp project**.</span></span>
2. <span data-ttu-id="1bba1-140">V **web.config**, v hello **EmbedSample** webovou aplikaci, upravit hello **appSettings**: **AccessKey**,  **WorkspaceCollection** název, a **WorkspaceId**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-140">In **web.config**, in hello **EmbedSample** web application, edit hello **appSettings**: **AccessKey**, **WorkspaceCollection** name, and **WorkspaceId**.</span></span>

    ```
    <appSettings>
        <add key="powerbi:AccessKey" value="" />
        <add key="powerbi:ApiUrl" value="https://api.powerbi.com" />
        <add key="powerbi:WorkspaceCollection" value="" />
        <add key="powerbi:WorkspaceId" value="" />
    </appSettings>
    ```
3. <span data-ttu-id="1bba1-141">Spustit hello **EmbedSample** webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1bba1-141">Run hello **EmbedSample** web application.</span></span>

<span data-ttu-id="1bba1-142">Po spuštění hello **EmbedSample** webové aplikace, musí obsahovat hello levém navigačním panelu **sestavy** nabídky.</span><span class="sxs-lookup"><span data-stu-id="1bba1-142">Once you run hello **EmbedSample** web application, hello left navigation panel should contain a **Reports** menu.</span></span> <span data-ttu-id="1bba1-143">Sestava hello tooview jste importovali, rozbalte **sestavy**a klikněte na sestavu.</span><span class="sxs-lookup"><span data-stu-id="1bba1-143">tooview hello report you imported, expand **Reports**, and click a report.</span></span> <span data-ttu-id="1bba1-144">Pokud jste importovali hello [prodejní analýzy ukázkový soubor PBIX](http://go.microsoft.com/fwlink/?LinkID=780547), hello ukázkovou webovou aplikaci bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="1bba1-144">If you imported hello [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547), hello sample web app would look like this:</span></span>

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-sample-left-nav.png)

<span data-ttu-id="1bba1-145">Po kliknutí na tlačítko sestavy, hello **EmbedSample** webová aplikace by měl vypadat přibližně toto:</span><span class="sxs-lookup"><span data-stu-id="1bba1-145">After you click a report, hello **EmbedSample** web application should look something this:</span></span>

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="explore-hello-sample-code"></a><span data-ttu-id="1bba1-146">Prozkoumejte hello ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="1bba1-146">Explore hello sample code</span></span>

<span data-ttu-id="1bba1-147">Hello **Microsoft Power BI Embedded** ukázka je ukázkové webové aplikace, která ukazuje, jak toointegrate **Power BI** sestavy do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1bba1-147">hello **Microsoft Power BI Embedded** sample is an example web app that shows you how toointegrate **Power BI** reports into your app.</span></span> <span data-ttu-id="1bba1-148">Používá Model-View-Controller (MVC) návrh vzor toodemonstrate osvědčené postupy.</span><span class="sxs-lookup"><span data-stu-id="1bba1-148">It uses a Model-View-Controller (MVC) design pattern toodemonstrate best practices.</span></span> <span data-ttu-id="1bba1-149">V této části jsou zdůrazněné částí hello ukázkový kód, který můžete prozkoumat v rámci hello **vložených PowerBI** webové aplikace řešení.</span><span class="sxs-lookup"><span data-stu-id="1bba1-149">This section highlights parts of hello sample code that you can explore within hello **PowerBI-embedded** web app solution.</span></span> <span data-ttu-id="1bba1-150">Hello vzor Model-View-Controller (MVC) odděluje hello modelování hello domény, hello prezentace a hello akce založené na vstup uživatele na tři samostatné třídy: Model, zobrazení a řízení.</span><span class="sxs-lookup"><span data-stu-id="1bba1-150">hello Model-View-Controller (MVC) pattern separates hello modeling of hello domain, hello presentation, and hello actions based on user input into three separate classes: Model, View, and Control.</span></span> <span data-ttu-id="1bba1-151">toolearn Další informace o MVC, najdete v části [Další informace o ASP.NET](http://www.asp.net/mvc).</span><span class="sxs-lookup"><span data-stu-id="1bba1-151">toolearn more about MVC, see [Learn About ASP.NET](http://www.asp.net/mvc).</span></span>

<span data-ttu-id="1bba1-152">Hello **Microsoft Power BI Embedded** ukázkový kód je oddělená následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="1bba1-152">hello **Microsoft Power BI Embedded** sample code is separated as follows.</span></span> <span data-ttu-id="1bba1-153">Každý oddíl obsahuje název souboru hello v hello PowerBI embedded.sln řešení tak, že budete moci snadno najít hello kód v ukázce hello.</span><span class="sxs-lookup"><span data-stu-id="1bba1-153">Each section includes hello file name in hello PowerBI-embedded.sln solution so that you can easily find hello code in hello sample.</span></span>

> [!NOTE]
> <span data-ttu-id="1bba1-154">V této části je uveden seznam hello ukázkový kód, který ukazuje, jak byla zapsána hello kódu.</span><span class="sxs-lookup"><span data-stu-id="1bba1-154">This section is a summary of hello sample code that shows how hello code was written.</span></span> <span data-ttu-id="1bba1-155">tooview hello dokončení ukázkové, načtěte hello PowerBI embedded.sln řešení v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1bba1-155">tooview hello complete sample, please load hello PowerBI-embedded.sln solution in Visual Studio.</span></span>

### <a name="model"></a><span data-ttu-id="1bba1-156">Model</span><span class="sxs-lookup"><span data-stu-id="1bba1-156">Model</span></span>

<span data-ttu-id="1bba1-157">Ukázka Hello má **ReportsViewModel** a **ReportViewModel**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-157">hello sample has a **ReportsViewModel** and **ReportViewModel**.</span></span>

<span data-ttu-id="1bba1-158">**ReportsViewModel.cs**: představuje sestavy Power BI.</span><span class="sxs-lookup"><span data-stu-id="1bba1-158">**ReportsViewModel.cs**: Represents Power BI Reports.</span></span>

    public class ReportsViewModel
    {
        public List<Report> Reports { get; set; }
    }

<span data-ttu-id="1bba1-159">**ReportViewModel.cs**: představuje sestavy Power BI.</span><span class="sxs-lookup"><span data-stu-id="1bba1-159">**ReportViewModel.cs**: Represents a Power BI Report.</span></span>

    public classReportViewModel
    {
        public IReport Report { get; set; }

        public string AccessToken { get; set; }
    }

### <a name="connection-string"></a><span data-ttu-id="1bba1-160">Připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="1bba1-160">Connection string</span></span>

<span data-ttu-id="1bba1-161">Hello připojovací řetězec musí být ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="1bba1-161">hello connection string must be in hello following format:</span></span>

```
Data Source=tcp:MyServer.database.windows.net,1433;Initial Catalog=MyDatabase
```

<span data-ttu-id="1bba1-162">Pomocí běžné atributy server a databáze se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="1bba1-162">Using common server and database attributes will fail.</span></span> <span data-ttu-id="1bba1-163">Příklad: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,</span><span class="sxs-lookup"><span data-stu-id="1bba1-163">For example: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,</span></span>

### <a name="view"></a><span data-ttu-id="1bba1-164">Zobrazení</span><span class="sxs-lookup"><span data-stu-id="1bba1-164">View</span></span>

<span data-ttu-id="1bba1-165">Hello **zobrazení** spravuje hello zobrazení Power BI **sestavy** a Power BI **sestavy**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-165">hello **View** manages hello display of Power BI **Reports** and a Power BI **Report**.</span></span>

<span data-ttu-id="1bba1-166">**Reports.cshtml**: iterace v **Model.Reports** toocreate **ActionLink**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-166">**Reports.cshtml**: Iterate over **Model.Reports** toocreate an **ActionLink**.</span></span> <span data-ttu-id="1bba1-167">Hello **ActionLink** se skládá následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1bba1-167">hello **ActionLink** is composed as follows:</span></span>

| <span data-ttu-id="1bba1-168">Část</span><span class="sxs-lookup"><span data-stu-id="1bba1-168">Part</span></span> | <span data-ttu-id="1bba1-169">Popis</span><span class="sxs-lookup"><span data-stu-id="1bba1-169">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1bba1-170">Název</span><span class="sxs-lookup"><span data-stu-id="1bba1-170">Title</span></span> |<span data-ttu-id="1bba1-171">Název hello sestavy.</span><span class="sxs-lookup"><span data-stu-id="1bba1-171">Name of hello Report.</span></span> |
| <span data-ttu-id="1bba1-172">Řetězce dotazu</span><span class="sxs-lookup"><span data-stu-id="1bba1-172">QueryString</span></span> |<span data-ttu-id="1bba1-173">Odkaz toohello ID sestavy.</span><span class="sxs-lookup"><span data-stu-id="1bba1-173">A link toohello Report ID.</span></span> |

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

<span data-ttu-id="1bba1-174">Report.cshtml: Nastavte hello **Model.AccessToken**, a hello výrazu Lambda pro **PowerBIReportFor**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-174">Report.cshtml: Set hello **Model.AccessToken**, and hello Lambda expression for **PowerBIReportFor**.</span></span>

    @model ReportViewModel

    ...

    <div class="side-body padding-top">
        @Html.PowerBIAccessToken(Model.AccessToken)
        @Html.PowerBIReportFor(m => m.Report, new { style = "height:85vh" })
    </div>

### <a name="controller"></a><span data-ttu-id="1bba1-175">Řadiče</span><span class="sxs-lookup"><span data-stu-id="1bba1-175">Controller</span></span>

<span data-ttu-id="1bba1-176">**DashboardController.cs**: vytvoří předávání PowerBIClient **tokenu aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-176">**DashboardController.cs**: Creates a PowerBIClient passing an **app token**.</span></span> <span data-ttu-id="1bba1-177">Token JSON Web Token (JWT) se generují z hello **podpisový klíč** tooget hello **pověření**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-177">A JSON Web Token (JWT) is generated from hello **Signing Key** tooget hello **Credentials**.</span></span> <span data-ttu-id="1bba1-178">Hello **pověření** jsou použité toocreate instanci **PowerBIClient**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-178">hello **Credentials** are used toocreate an instance of **PowerBIClient**.</span></span> <span data-ttu-id="1bba1-179">Až budete mít instanci **PowerBIClient**, můžete volat GetReports() a GetReportsAsync().</span><span class="sxs-lookup"><span data-stu-id="1bba1-179">Once you have an instance of **PowerBIClient**, you can call GetReports() and GetReportsAsync().</span></span>

<span data-ttu-id="1bba1-180">CreatePowerBIClient()</span><span class="sxs-lookup"><span data-stu-id="1bba1-180">CreatePowerBIClient()</span></span>

    private IPowerBIClient CreatePowerBIClient()
    {
        var credentials = new TokenCredentials(accessKey, "AppKey");
        var client = new PowerBIClient(credentials)
        {
            BaseUri = new Uri(apiUrl)
        };

        return client;
    }

<span data-ttu-id="1bba1-181">ActionResult Reports()</span><span class="sxs-lookup"><span data-stu-id="1bba1-181">ActionResult Reports()</span></span>

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


<span data-ttu-id="1bba1-182">Úloha<ActionResult> sestavy (reportId řetězec)</span><span class="sxs-lookup"><span data-stu-id="1bba1-182">Task<ActionResult> Report(string reportId)</span></span>

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

### <a name="integrate-a-report-into-your-app"></a><span data-ttu-id="1bba1-183">Integraci sestavy do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="1bba1-183">Integrate a report into your app</span></span>

<span data-ttu-id="1bba1-184">Až budete mít **sestavy**, můžete použít **IFrame** tooembed hello Power BI **sestavy**.</span><span class="sxs-lookup"><span data-stu-id="1bba1-184">Once you have a **Report**, you use an **IFrame** tooembed hello Power BI **Report**.</span></span> <span data-ttu-id="1bba1-185">Zde je fragment kódu z powerbi.js v hello **Microsoft Power BI Embedded** ukázka.</span><span class="sxs-lookup"><span data-stu-id="1bba1-185">Here is a code snippet from  powerbi.js in hello **Microsoft Power BI Embedded** sample.</span></span>

![](media/powerbi-embedded-get-started-sample/power-bi-embedded-iframe-code.png)

## <a name="filter-reports-embedded-in-your-application"></a><span data-ttu-id="1bba1-186">Filtrování zpráv vloží do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="1bba1-186">Filter reports embedded in your application</span></span>

<span data-ttu-id="1bba1-187">Vloženou sestavu můžete filtrovat pomocí syntaxe adresy URL.</span><span class="sxs-lookup"><span data-stu-id="1bba1-187">You can filter an embedded report using a URL syntax.</span></span> <span data-ttu-id="1bba1-188">toodo, přidáte **$filter** parametr řetězce s dotazu **eq** operátor tooyour iFrame src url s filtrem hello.</span><span class="sxs-lookup"><span data-stu-id="1bba1-188">toodo this, you add a **$filter** query string parameter with an **eq** operator tooyour iFrame src url with hello filter specified.</span></span> <span data-ttu-id="1bba1-189">Tady je hello se syntaxí filtru dotazu:</span><span class="sxs-lookup"><span data-stu-id="1bba1-189">Here is hello filter query syntax:</span></span>

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-...-9673-ee9655d54a4a&
$filter={tableName/fieldName}%20eq%20'{fieldValue}'
```

> [!NOTE]
> <span data-ttu-id="1bba1-190">V řetězci {tableName/fieldName} nesmí být mezery ani speciální znaky.</span><span class="sxs-lookup"><span data-stu-id="1bba1-190">{tableName/fieldName} cannot include spaces or special characters.</span></span> <span data-ttu-id="1bba1-191">Hello {místo položky fieldValue} je možné zadat jednu hodnotu představující kategorii.</span><span class="sxs-lookup"><span data-stu-id="1bba1-191">hello {fieldValue} accepts a single categorical value.</span></span>  

## <a name="see-also"></a><span data-ttu-id="1bba1-192">Viz také</span><span class="sxs-lookup"><span data-stu-id="1bba1-192">See also</span></span>

[<span data-ttu-id="1bba1-193">Běžné scénáře Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="1bba1-193">Common Microsoft Power BI Embedded scenarios</span></span>](power-bi-embedded-scenarios.md)  
[<span data-ttu-id="1bba1-194">Ověřování a autorizace v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="1bba1-194">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="1bba1-195">Vložení sestavy</span><span class="sxs-lookup"><span data-stu-id="1bba1-195">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="1bba1-196">Vytvoření nové sestavy z datové sady</span><span class="sxs-lookup"><span data-stu-id="1bba1-196">Create a new report from a dataset</span></span>](power-bi-embedded-create-report-from-dataset.md)  
[<span data-ttu-id="1bba1-197">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="1bba1-197">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="1bba1-198">Vložená ukázka JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="1bba1-198">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="1bba1-199">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="1bba1-199">More questions?</span></span> [<span data-ttu-id="1bba1-200">Zkuste hello komunitě Power BI</span><span class="sxs-lookup"><span data-stu-id="1bba1-200">Try hello Power BI Community</span></span>](http://community.powerbi.com/)
