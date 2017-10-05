---
title: "Začínáme s Microsoft Power BI Embedded"
description: "Power BI Embedded – přidejte do své aplikace business intelligence interaktivní sestavy Power BI."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 4787cf44-5d1c-4bc3-b3fd-bf396e5c1176
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: 4afa8d2c7f8ec1942521ba5fa131967dfd581c91
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-microsoft-power-bi-embedded"></a><span data-ttu-id="34805-103">Začínáme s Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="34805-103">Get started with Microsoft Power BI Embedded</span></span>

<span data-ttu-id="34805-104">**Power BI Embedded** je služba Azure, který vývojářům aplikací umožňuje přidávání interaktivních sestav Power BI do jejich vlastních aplikací.</span><span class="sxs-lookup"><span data-stu-id="34805-104">**Power BI Embedded** is an Azure service that enables application developers to add interactive Power BI reports into their own applications.</span></span> <span data-ttu-id="34805-105">**Power BI Embedded** funguje se stávajícími aplikacemi bez nutnosti přepracování nebo změny způsobu, jakým se uživatelé přihlašují.</span><span class="sxs-lookup"><span data-stu-id="34805-105">**Power BI Embedded** works with existing applications without needing redesign or changing the way users sign in.</span></span>

<span data-ttu-id="34805-106">Prostředky pro **Microsoft Power BI Embedded** se zřizují prostřednictvím [rozhraní API Azure ARM](https://msdn.microsoft.com/library/mt712306.aspx).</span><span class="sxs-lookup"><span data-stu-id="34805-106">Resources for **Microsoft Power BI Embedded** are provisioned through the [Azure ARM APIs](https://msdn.microsoft.com/library/mt712306.aspx).</span></span> <span data-ttu-id="34805-107">V tomto případě je prostředkem, který zřídíte, **kolekce Pracovních prostorů Power BI**.</span><span class="sxs-lookup"><span data-stu-id="34805-107">In this case, the resource you provision is a **Power BI Workspace Collection**.</span></span>

![](media/power-bi-embedded-get-started/introduction.png)

## <a name="create-a-workspace-collection"></a><span data-ttu-id="34805-108">Vytvoření kolekce pracovních prostorů</span><span class="sxs-lookup"><span data-stu-id="34805-108">Create a workspace collection</span></span>

<span data-ttu-id="34805-109">**Kolekce pracovních prostorů** je prostředek Azure nejvyšší úrovně a kontejner pro obsah, který se vloží do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="34805-109">A **Workspace Collection** is the top-level Azure resource and a container for the content that will be embedded in your application.</span></span> <span data-ttu-id="34805-110">**Kolekci pracovních prostorů** lze vytvořit dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="34805-110">A **Workspace Collection** can be created in two ways::</span></span>

* <span data-ttu-id="34805-111">Ručně na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="34805-111">Manually using the Azure Portal</span></span>
* <span data-ttu-id="34805-112">Programově pomocí rozhraní API Azure ARM (Azure Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="34805-112">Programmatically using the Azure Resource Manager(ARM) APIs</span></span>

<span data-ttu-id="34805-113">Projděme si postup pro sestavení **kolekce pracovních prostorů** na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="34805-113">Let's walk through the steps to build a **Workspace Collection** using the Azure Portal.</span></span>

1. <span data-ttu-id="34805-114">Otevřete a přihlaste se k **webu Azure Portal**: [http://portal.azure.com](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="34805-114">Open and sign into **Azure Portal**: [http://portal.azure.com](http://portal.azure.com).</span></span>
2. <span data-ttu-id="34805-115">Klikněte na **+ Nový** na horním panelu.</span><span class="sxs-lookup"><span data-stu-id="34805-115">Click **+ New** on the top panel.</span></span>
   
   ![](media/power-bi-embedded-get-started/create-workspace-1.png)
3. <span data-ttu-id="34805-116">V části **Data + analýzy** klikněte na **Power BI Embedded**.</span><span class="sxs-lookup"><span data-stu-id="34805-116">Under **Data + Analytics** click **Power BI Embedded**.</span></span>
4. <span data-ttu-id="34805-117">V okně **Kolekce pracovních prostorů** zadejte požadované informace.</span><span class="sxs-lookup"><span data-stu-id="34805-117">On the **Workspace Collection Blade**, enter the required information.</span></span> <span data-ttu-id="34805-118">Pokud jde o **cenu**, nahlédněte do [cen za Power BI Embedded](http://go.microsoft.com/fwlink/?LinkID=760527).</span><span class="sxs-lookup"><span data-stu-id="34805-118">For **Pricing**, see [Power BI Embedded pricing](http://go.microsoft.com/fwlink/?LinkID=760527).</span></span>
   
   ![](media/power-bi-embedded-get-started/create-workspace-2.png)
5. <span data-ttu-id="34805-119">Klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="34805-119">Click **Create**.</span></span>

<span data-ttu-id="34805-120">Zřizování **kolekce pracovních prostorů** bude chvíli trvat.</span><span class="sxs-lookup"><span data-stu-id="34805-120">The **Workspace Collection** will take a few moments to provision.</span></span> <span data-ttu-id="34805-121">Po dokončení se otevře **okno kolekce pracovních prostorů**.</span><span class="sxs-lookup"><span data-stu-id="34805-121">When completed, you'll be taken to the **Workspace Collection Blade**.</span></span>

   ![](media/power-bi-embedded-get-started/create-workspace-3.png)

<span data-ttu-id="34805-122">Toto **okno pro vytvoření** obsahuje informace potřebné k volání rozhraní API, která slouží k vytváření pracovních prostorů a nasazování obsahu do nich.</span><span class="sxs-lookup"><span data-stu-id="34805-122">The **Creation Blade** contains the information you need to call the APIs that create workspaces and deploy content to them.</span></span>

<a name="view-access-keys"/>

## <a name="view-power-bi-api-access-keys"></a><span data-ttu-id="34805-123">Zobrazení přístupových klíčů k rozhraním API pro Power BI</span><span class="sxs-lookup"><span data-stu-id="34805-123">View Power BI API access keys</span></span>

<span data-ttu-id="34805-124">Jednou z nejdůležitějších informací potřebných k volání rozhraní REST API pro Power BI jsou **přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="34805-124">One of the most important pieces of information needed to call the Power BI REST APIs are the **Access Keys**.</span></span> <span data-ttu-id="34805-125">Na jejich základě se generují **tokeny aplikací** používané k ověřování vašich žádostí o rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="34805-125">These are used to generate the **app tokens** that are used to authenticate your API requests.</span></span> <span data-ttu-id="34805-126">Pokud si chcete **přístupové klíče** zobrazit, klikněte na **Přístupové klíče** v **okně Nastavení**.</span><span class="sxs-lookup"><span data-stu-id="34805-126">To view your **Access Keys**, click **Access Keys** on the **Settings Blade**.</span></span> <span data-ttu-id="34805-127">Další informace o **tokenech aplikace** naleznete v části [Ověřování a autorizace pomocí Power BI Embedded](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="34805-127">For more about **app tokens**, see [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span>

   ![](media/power-bi-embedded-get-started/access-keys.png)

<span data-ttu-id="34805-128">Jak si všimnete, máte dva klíče.</span><span class="sxs-lookup"><span data-stu-id="34805-128">You'll'notice that you have two keys.</span></span>

   ![](media/power-bi-embedded-get-started/access-keys-2.png)

<span data-ttu-id="34805-129">Zkopírujte tyto klíče a zabezpečeně je uložte do své aplikace.</span><span class="sxs-lookup"><span data-stu-id="34805-129">Copy these keys and store them securely in your application.</span></span> <span data-ttu-id="34805-130">Je velmi důležité, abyste s těmito klíči zacházeli jako s heslem, protože poskytují přístup k veškerému obsahu ve vaší **kolekci pracovních prostorů**.</span><span class="sxs-lookup"><span data-stu-id="34805-130">It's very important that you treat these keys as you would a password, because they'll provide access to all the content in your **Workspace Collection**.</span></span>

<span data-ttu-id="34805-131">Ačkoli jsou uvedeny dva klíče, najednou je zapotřebí pouze jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="34805-131">While two keys are listed, only one key is needed at a particular time.</span></span> <span data-ttu-id="34805-132">Druhý klíč je k dispozici proto, abyste mohli klíče pravidelně obnovovat, aniž by to ovlivnilo přístup ke službě.</span><span class="sxs-lookup"><span data-stu-id="34805-132">The second key is provided so you can periodically regenerate keys without interrupting access to the service.</span></span>

<span data-ttu-id="34805-133">Nyní když máte instanci Power BI pro vaši aplikaci a **přístupové klíče**, můžete do vlastní aplikace naimportovat sestavu.</span><span class="sxs-lookup"><span data-stu-id="34805-133">Now that you have an instance of Power BI for your application, and **Access Keys**, you can import a report into your own app.</span></span> <span data-ttu-id="34805-134">Než si řekneme, jak sestavu importovat, dozvíte se v další části, jak vytvářet datové sady a sestavy Power BI určené pro vložení do aplikace.</span><span class="sxs-lookup"><span data-stu-id="34805-134">Before you learn how to import a report, the next section describes creating Power BI datasets and reports to embed into an app.</span></span>

## <a name="working-with-workspaces"></a><span data-ttu-id="34805-135">Práce s pracovními prostory</span><span class="sxs-lookup"><span data-stu-id="34805-135">Working with workspaces</span></span>

<span data-ttu-id="34805-136">Po vytvoření vaší kolekce pracovních prostorů musíte vytvořit pracovní prostor, který bude obsahovat vaše sestavy a datové sady.</span><span class="sxs-lookup"><span data-stu-id="34805-136">After you have created your workspace collection, you will need to create a workspace that will house your reports and datasets.</span></span> <span data-ttu-id="34805-137">Pokud chcete vytvořit pracovní prostor, budete muset použít rozhraní [Post Worksapce REST API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span><span class="sxs-lookup"><span data-stu-id="34805-137">To create a workspace, you will need to use the [Post Worksapce REST API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span></span>

## <a name="create-power-bi-datasets-and-reports-to-embed-into-an-app-using-power-bi-desktop"></a><span data-ttu-id="34805-138">Vytváření datových sad a sestav Power BI určených pro vložení do aplikace pomocí Power BI Desktopu</span><span class="sxs-lookup"><span data-stu-id="34805-138">Create Power BI datasets and reports to embed into an app using Power BI Desktop</span></span>

<span data-ttu-id="34805-139">Nyní když jste si pro svou aplikaci vytvořili instanci Power BI a máte **přístupové klíče**, musíte si vytvořit datové sady a sestavy Power BI, které do ní chcete vložit.</span><span class="sxs-lookup"><span data-stu-id="34805-139">Now that you have created an instance of Power BI for your application, and have **Access Keys**, you will need to create the Power BI datasets and reports that you want to embed.</span></span> <span data-ttu-id="34805-140">Datové sady a sestavy lze vytvořit pomocí **Power BI Desktopu**.</span><span class="sxs-lookup"><span data-stu-id="34805-140">Datasets and reports can be created by using **Power BI Desktop**.</span></span> <span data-ttu-id="34805-141">[Power BI Desktop si můžete stáhnout zdarma](https://go.microsoft.com/fwlink/?LinkId=521662).</span><span class="sxs-lookup"><span data-stu-id="34805-141">You can download [Power BI Desktop for free](https://go.microsoft.com/fwlink/?LinkId=521662).</span></span> <span data-ttu-id="34805-142">Nebo pokud chcete rychle začít, můžete si stáhnout [ukázkový soubor PBIX prodejní analýzy](http://go.microsoft.com/fwlink/?LinkID=780547).</span><span class="sxs-lookup"><span data-stu-id="34805-142">Or, to quickly get started, you can download the [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).</span></span>

> [!NOTE]
> <span data-ttu-id="34805-143">Další informace o použití **Power BI Desktop** najdete v části [Začínáme s Power BI Desktop](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).</span><span class="sxs-lookup"><span data-stu-id="34805-143">To learn more about how to use **Power BI Desktop**, see [Getting Started with Power BI Desktop](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).</span></span>

<span data-ttu-id="34805-144">Se službou **Power BI Desktop** se ke zdroji dat připojíte importem kopie dat do **Power BI Desktop** nebo přímým připojením zdroje dat pomocí **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="34805-144">With **Power BI Desktop**, you connect to your data source by importing a copy of the data into **Power BI Desktop** or connecting directly to the data source using **DirectQuery**.</span></span>

<span data-ttu-id="34805-145">Zde jsou rozdíly mezi **importem** a použitím **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="34805-145">Here are the differences between using **Import** and **DirectQuery**.</span></span>

| <span data-ttu-id="34805-146">Import</span><span class="sxs-lookup"><span data-stu-id="34805-146">Import</span></span> | <span data-ttu-id="34805-147">DirectQuery</span><span class="sxs-lookup"><span data-stu-id="34805-147">DirectQuery</span></span> |
| --- | --- |
| <span data-ttu-id="34805-148">Tabulky, sloupce *a data* se naimportují, tedy zkopírují do **Power BI Desktop**.</span><span class="sxs-lookup"><span data-stu-id="34805-148">Tables, columns, *and data* are imported or copied into **Power BI Desktop**.</span></span> <span data-ttu-id="34805-149">Při práci s vizualizacemi si **Power BI Desktop** dotazem vyžádá kopii dat.</span><span class="sxs-lookup"><span data-stu-id="34805-149">As you work with visualizations, **Power BI Desktop** queries a copy of the data.</span></span> <span data-ttu-id="34805-150">Pokud se mají projevit změny, ke kterým došlo v základních datech, musíte provést aktualizaci, tzn. znovu naimportovat celou aktuální datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="34805-150">To see any changes that occurred to the underlying data, you must refresh, or import, a complete, current dataset again.</span></span> |<span data-ttu-id="34805-151">Do **Power BI Desktop** se naimportují, tedy zkopírují pouze *tabulky a sloupce*.</span><span class="sxs-lookup"><span data-stu-id="34805-151">Only *tables and columns* are imported or copied into **Power BI Desktop**.</span></span> <span data-ttu-id="34805-152">Při práci s vizualizacemi si **Power BI Desktop** dotazem vyžádá základní zdroj dat, což znamená, že se vám vždy zobrazují aktuální data.</span><span class="sxs-lookup"><span data-stu-id="34805-152">As you work with visualizations, **Power BI Desktop** queries the underlying data source, which means you're always viewing current data.</span></span> |

<span data-ttu-id="34805-153">Další informace o připojení ke zdroji dat naleznete v tématu [Připojení ke zdroji dat](power-bi-embedded-connect-datasource.md).</span><span class="sxs-lookup"><span data-stu-id="34805-153">For more about connecting to a data source, see [Connect to a data source](power-bi-embedded-connect-datasource.md).</span></span>

<span data-ttu-id="34805-154">Po uložení práce v **Power BI Desktop** se vytvoří soubor PBIX.</span><span class="sxs-lookup"><span data-stu-id="34805-154">After you save your work in **Power BI Desktop**, a PBIX file is created.</span></span> <span data-ttu-id="34805-155">Tento soubor obsahuje vaši sestavu.</span><span class="sxs-lookup"><span data-stu-id="34805-155">This file contains your report.</span></span> <span data-ttu-id="34805-156">Kromě toho bude při importu dat soubor PBIX obsahovat úplnou datovou sadu. A pokud použijete **DirectQuery**, soubor PBIX bude obsahovat jen schéma datové sady.</span><span class="sxs-lookup"><span data-stu-id="34805-156">In addition, if you import data the PBIX contains the complete dataset, or if you use **DirectQuery**, the PBIX contains just a dataset schema.</span></span> <span data-ttu-id="34805-157">Soubor PBIX programově nasadíte do pracovního prostoru pomocí [rozhraní API pro import v Power BI](https://msdn.microsoft.com/library/mt711504.aspx).</span><span class="sxs-lookup"><span data-stu-id="34805-157">You programmatically deploy the PBIX into your workspace using the [Power BI Import API](https://msdn.microsoft.com/library/mt711504.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="34805-158">**Power BI Embedded** má další rozhraní API pro změnu serveru a databáze, na které datová sada odkazuje, a nastavení přihlašovacích údajů pro účet služby, který datová sada bude používat pro připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="34805-158">**Power BI Embedded** has additional APIs to change the server and database that your dataset is pointing to and set a service account credential that the dataset will use to connect to your database.</span></span> <span data-ttu-id="34805-159">Viz [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) a [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).</span><span class="sxs-lookup"><span data-stu-id="34805-159">See [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) and [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).</span></span>

## <a name="create-power-bi-datasets-and-reports-using-apis"></a><span data-ttu-id="34805-160">Vytváření datových sad a sestav Power BI pomocí rozhraní API</span><span class="sxs-lookup"><span data-stu-id="34805-160">Create Power BI datasets and reports using APIs</span></span>

### <a name="datsets"></a><span data-ttu-id="34805-161">Datové sady</span><span class="sxs-lookup"><span data-stu-id="34805-161">Datsets</span></span>

<span data-ttu-id="34805-162">Můžete vytvořit datové sady v rámci Power BI Embedded pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="34805-162">You can create datasets within Power BI Embedded using the REST API.</span></span> <span data-ttu-id="34805-163">Potom můžete datovou sadu naplnit daty.</span><span class="sxs-lookup"><span data-stu-id="34805-163">You can then push data into your dataset.</span></span> <span data-ttu-id="34805-164">To umožňuje pracovat s daty bez potřeby mít Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="34805-164">This allows you to work with data without the need of Power BI Desktop.</span></span> <span data-ttu-id="34805-165">Další informace najdete v tématu [Post Datasets](https://msdn.microsoft.com/library/azure/mt778875.aspx).</span><span class="sxs-lookup"><span data-stu-id="34805-165">For more information, see [Post Datasets](https://msdn.microsoft.com/library/azure/mt778875.aspx).</span></span>

### <a name="reports"></a><span data-ttu-id="34805-166">Reports</span><span class="sxs-lookup"><span data-stu-id="34805-166">Reports</span></span>

<span data-ttu-id="34805-167">Můžete ve své aplikaci vytvořit sestavu přímo z datové sady pomocí rozhraní API jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="34805-167">You can create a report from a dataset directly in your application using the JavaScript API.</span></span> <span data-ttu-id="34805-168">Další informace najdete v tématu s popisem [vytvoření nové sestavy z datové sady v Power BI Embedded](power-bi-embedded-create-report-from-dataset.md).</span><span class="sxs-lookup"><span data-stu-id="34805-168">For more information, see [Create a new report from a dataset in Power BI Embedded](power-bi-embedded-create-report-from-dataset.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="34805-169">Viz také</span><span class="sxs-lookup"><span data-stu-id="34805-169">See Also</span></span>

[<span data-ttu-id="34805-170">Začínáme s ukázkou</span><span class="sxs-lookup"><span data-stu-id="34805-170">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="34805-171">Ověřování a autorizace v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="34805-171">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="34805-172">Vložení sestavy</span><span class="sxs-lookup"><span data-stu-id="34805-172">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
<span data-ttu-id="34805-173">[Vytvoření nové sestavy z datové sady v Power BI Embedded](power-bi-embedded-create-report-from-dataset.md)
[Ukládání sestav](power-bi-embedded-save-reports.md)</span><span class="sxs-lookup"><span data-stu-id="34805-173">[Create a new report from a dataset in Power BI Embedded](power-bi-embedded-create-report-from-dataset.md)
[Save reports](power-bi-embedded-save-reports.md)</span></span>  
[<span data-ttu-id="34805-174">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="34805-174">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="34805-175">Vložená ukázka JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="34805-175">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="34805-176">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="34805-176">More questions?</span></span> [<span data-ttu-id="34805-177">Vyzkoušejte komunitu Power BI</span><span class="sxs-lookup"><span data-stu-id="34805-177">Try the Power BI Community</span></span>](http://community.powerbi.com/)

