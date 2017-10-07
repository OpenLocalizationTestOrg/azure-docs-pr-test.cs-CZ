---
title: "aaaGet začít s Microsoft Power BI Embedded"
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
ms.openlocfilehash: ebb550cb4eba761dde3c23e4dd0314fc885817e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-microsoft-power-bi-embedded"></a><span data-ttu-id="79ef0-103">Začínáme s Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="79ef0-103">Get started with Microsoft Power BI Embedded</span></span>

<span data-ttu-id="79ef0-104">**Power BI Embedded** je služba Azure, že umožňuje tooadd vývojáři aplikace interaktivní Power BI sestavy do svých vlastních aplikacích.</span><span class="sxs-lookup"><span data-stu-id="79ef0-104">**Power BI Embedded** is an Azure service that enables application developers tooadd interactive Power BI reports into their own applications.</span></span> <span data-ttu-id="79ef0-105">**Power BI Embedded** funguje s stávající aplikace bez nutnosti změna nebo změna hello způsobem uživatelé přihlásit.</span><span class="sxs-lookup"><span data-stu-id="79ef0-105">**Power BI Embedded** works with existing applications without needing redesign or changing hello way users sign in.</span></span>

<span data-ttu-id="79ef0-106">Prostředky pro **Microsoft Power BI Embedded** jsou zřízené prostřednictvím hello [rozhraní API Azure ARM](https://msdn.microsoft.com/library/mt712306.aspx).</span><span class="sxs-lookup"><span data-stu-id="79ef0-106">Resources for **Microsoft Power BI Embedded** are provisioned through hello [Azure ARM APIs](https://msdn.microsoft.com/library/mt712306.aspx).</span></span> <span data-ttu-id="79ef0-107">V takovém případě je prostředek hello zřídíte **kolekce pracovních prostorů Power BI**.</span><span class="sxs-lookup"><span data-stu-id="79ef0-107">In this case, hello resource you provision is a **Power BI Workspace Collection**.</span></span>

![](media/power-bi-embedded-get-started/introduction.png)

## <a name="create-a-workspace-collection"></a><span data-ttu-id="79ef0-108">Vytvoření kolekce pracovních prostorů</span><span class="sxs-lookup"><span data-stu-id="79ef0-108">Create a workspace collection</span></span>

<span data-ttu-id="79ef0-109">A **kolekce pracovních prostorů** je prostředek Azure nejvyšší úrovně hello a kontejner pro hello obsah, který se vloží do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="79ef0-109">A **Workspace Collection** is hello top-level Azure resource and a container for hello content that will be embedded in your application.</span></span> <span data-ttu-id="79ef0-110">**Kolekci pracovních prostorů** lze vytvořit dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="79ef0-110">A **Workspace Collection** can be created in two ways::</span></span>

* <span data-ttu-id="79ef0-111">Ručně pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="79ef0-111">Manually using hello Azure Portal</span></span>
* <span data-ttu-id="79ef0-112">Programově pomocí rozhraní API Správce Azure Resource arm hello</span><span class="sxs-lookup"><span data-stu-id="79ef0-112">Programmatically using hello Azure Resource Manager(ARM) APIs</span></span>

<span data-ttu-id="79ef0-113">Projděme hello kroky toobuild **kolekce pracovních prostorů** pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="79ef0-113">Let's walk through hello steps toobuild a **Workspace Collection** using hello Azure Portal.</span></span>

1. <span data-ttu-id="79ef0-114">Otevřete a přihlaste se k **webu Azure Portal**: [http://portal.azure.com](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="79ef0-114">Open and sign into **Azure Portal**: [http://portal.azure.com](http://portal.azure.com).</span></span>
2. <span data-ttu-id="79ef0-115">Klikněte na tlačítko **+ nový** na horním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="79ef0-115">Click **+ New** on hello top panel.</span></span>
   
   ![](media/power-bi-embedded-get-started/create-workspace-1.png)
3. <span data-ttu-id="79ef0-116">V části **Data + analýzy** klikněte na **Power BI Embedded**.</span><span class="sxs-lookup"><span data-stu-id="79ef0-116">Under **Data + Analytics** click **Power BI Embedded**.</span></span>
4. <span data-ttu-id="79ef0-117">Na hello **okno kolekce pracovních prostorů**, zadejte hello požadované informace.</span><span class="sxs-lookup"><span data-stu-id="79ef0-117">On hello **Workspace Collection Blade**, enter hello required information.</span></span> <span data-ttu-id="79ef0-118">Pokud jde o **cenu**, nahlédněte do [cen za Power BI Embedded](http://go.microsoft.com/fwlink/?LinkID=760527).</span><span class="sxs-lookup"><span data-stu-id="79ef0-118">For **Pricing**, see [Power BI Embedded pricing](http://go.microsoft.com/fwlink/?LinkID=760527).</span></span>
   
   ![](media/power-bi-embedded-get-started/create-workspace-2.png)
5. <span data-ttu-id="79ef0-119">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="79ef0-119">Click **Create**.</span></span>

<span data-ttu-id="79ef0-120">Hello **kolekce pracovních prostorů** bude trvat několik tooprovision situacích.</span><span class="sxs-lookup"><span data-stu-id="79ef0-120">hello **Workspace Collection** will take a few moments tooprovision.</span></span> <span data-ttu-id="79ef0-121">Po dokončení ověření budete přesměrováni toohello **okno kolekce pracovních prostorů**.</span><span class="sxs-lookup"><span data-stu-id="79ef0-121">When completed, you'll be taken toohello **Workspace Collection Blade**.</span></span>

   ![](media/power-bi-embedded-get-started/create-workspace-3.png)

<span data-ttu-id="79ef0-122">Hello **okno pro vytvoření** obsahuje hello informace, které potřebujete toocall hello rozhraní API, která slouží k vytváření pracovních prostorů a nasazování obsahu toothem.</span><span class="sxs-lookup"><span data-stu-id="79ef0-122">hello **Creation Blade** contains hello information you need toocall hello APIs that create workspaces and deploy content toothem.</span></span>

<a name="view-access-keys"/>

## <a name="view-power-bi-api-access-keys"></a><span data-ttu-id="79ef0-123">Zobrazení přístupových klíčů k rozhraním API pro Power BI</span><span class="sxs-lookup"><span data-stu-id="79ef0-123">View Power BI API access keys</span></span>

<span data-ttu-id="79ef0-124">Jedním z nejdůležitějších kusy informace potřebné toocall hello Power BI REST API hello jsou hello **přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="79ef0-124">One of hello most important pieces of information needed toocall hello Power BI REST APIs are hello **Access Keys**.</span></span> <span data-ttu-id="79ef0-125">Jedná se o hello použité toogenerate **tokeny aplikací** , které jsou používané tooauthenticate vaší žádostí o rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="79ef0-125">These are used toogenerate hello **app tokens** that are used tooauthenticate your API requests.</span></span> <span data-ttu-id="79ef0-126">tooview vaše **přístupové klíče**, klikněte na tlačítko **přístupové klíče** na hello **okno nastavení**.</span><span class="sxs-lookup"><span data-stu-id="79ef0-126">tooview your **Access Keys**, click **Access Keys** on hello **Settings Blade**.</span></span> <span data-ttu-id="79ef0-127">Další informace o **tokenech aplikace** naleznete v části [Ověřování a autorizace pomocí Power BI Embedded](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="79ef0-127">For more about **app tokens**, see [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span>

   ![](media/power-bi-embedded-get-started/access-keys.png)

<span data-ttu-id="79ef0-128">Jak si všimnete, máte dva klíče.</span><span class="sxs-lookup"><span data-stu-id="79ef0-128">You'll'notice that you have two keys.</span></span>

   ![](media/power-bi-embedded-get-started/access-keys-2.png)

<span data-ttu-id="79ef0-129">Zkopírujte tyto klíče a zabezpečeně je uložte do své aplikace.</span><span class="sxs-lookup"><span data-stu-id="79ef0-129">Copy these keys and store them securely in your application.</span></span> <span data-ttu-id="79ef0-130">To je velmi důležité, že jste s těmito klíči zacházeli jako heslo, protože budete poskytují přístup k obsahu hello tooall v vaše **kolekce pracovních prostorů**.</span><span class="sxs-lookup"><span data-stu-id="79ef0-130">It's very important that you treat these keys as you would a password, because they'll provide access tooall hello content in your **Workspace Collection**.</span></span>

<span data-ttu-id="79ef0-131">Ačkoli jsou uvedeny dva klíče, najednou je zapotřebí pouze jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="79ef0-131">While two keys are listed, only one key is needed at a particular time.</span></span> <span data-ttu-id="79ef0-132">druhý klíč Hello je k dispozici, mohli klíče pravidelně obnovovat bez přerušení služby toohello přístup.</span><span class="sxs-lookup"><span data-stu-id="79ef0-132">hello second key is provided so you can periodically regenerate keys without interrupting access toohello service.</span></span>

<span data-ttu-id="79ef0-133">Nyní když máte instanci Power BI pro vaši aplikaci a **přístupové klíče**, můžete do vlastní aplikace naimportovat sestavu.</span><span class="sxs-lookup"><span data-stu-id="79ef0-133">Now that you have an instance of Power BI for your application, and **Access Keys**, you can import a report into your own app.</span></span> <span data-ttu-id="79ef0-134">Před zjistíte, jak tooimport a sestavy, hello další část popisuje vytváření tooembed datové sady a sestavy Power BI do aplikace.</span><span class="sxs-lookup"><span data-stu-id="79ef0-134">Before you learn how tooimport a report, hello next section describes creating Power BI datasets and reports tooembed into an app.</span></span>

## <a name="working-with-workspaces"></a><span data-ttu-id="79ef0-135">Práce s pracovními prostory</span><span class="sxs-lookup"><span data-stu-id="79ef0-135">Working with workspaces</span></span>

<span data-ttu-id="79ef0-136">Po vytvoření vaší kolekce pracovních prostorů, budete potřebovat toocreate pracovní prostor, který bude obsahovat sestavy a datové sady.</span><span class="sxs-lookup"><span data-stu-id="79ef0-136">After you have created your workspace collection, you will need toocreate a workspace that will house your reports and datasets.</span></span> <span data-ttu-id="79ef0-137">toocreate pracovního prostoru, budete potřebovat toouse hello [Post Worksapce REST API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span><span class="sxs-lookup"><span data-stu-id="79ef0-137">toocreate a workspace, you will need toouse hello [Post Worksapce REST API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span></span>

## <a name="create-power-bi-datasets-and-reports-tooembed-into-an-app-using-power-bi-desktop"></a><span data-ttu-id="79ef0-138">Vytvoření tooembed datové sady a sestavy Power BI do aplikace pomocí Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="79ef0-138">Create Power BI datasets and reports tooembed into an app using Power BI Desktop</span></span>

<span data-ttu-id="79ef0-139">Teď, když jste vytvořili instanci Power BI pro vaši aplikaci a mít **přístupové klíče**, budete potřebovat toocreate hello Power BI datové sady a sestavy, které chcete tooembed.</span><span class="sxs-lookup"><span data-stu-id="79ef0-139">Now that you have created an instance of Power BI for your application, and have **Access Keys**, you will need toocreate hello Power BI datasets and reports that you want tooembed.</span></span> <span data-ttu-id="79ef0-140">Datové sady a sestavy lze vytvořit pomocí **Power BI Desktopu**.</span><span class="sxs-lookup"><span data-stu-id="79ef0-140">Datasets and reports can be created by using **Power BI Desktop**.</span></span> <span data-ttu-id="79ef0-141">[Power BI Desktop si můžete stáhnout zdarma](https://go.microsoft.com/fwlink/?LinkId=521662).</span><span class="sxs-lookup"><span data-stu-id="79ef0-141">You can download [Power BI Desktop for free](https://go.microsoft.com/fwlink/?LinkId=521662).</span></span> <span data-ttu-id="79ef0-142">Nebo, začněte tooquickly, si můžete stáhnout hello [prodejní analýzy ukázkový soubor PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).</span><span class="sxs-lookup"><span data-stu-id="79ef0-142">Or, tooquickly get started, you can download hello [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).</span></span>

> [!NOTE]
> <span data-ttu-id="79ef0-143">Další informace o tom toolearn toouse **Power BI Desktop**, najdete v části [Začínáme s Power BI Desktop](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).</span><span class="sxs-lookup"><span data-stu-id="79ef0-143">toolearn more about how toouse **Power BI Desktop**, see [Getting Started with Power BI Desktop](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).</span></span>

<span data-ttu-id="79ef0-144">S **Power BI Desktop**, tooyour zdroj dat připojíte importem kopie dat hello do **Power BI Desktop** nebo připojení přímo toohello datového zdroje pomocí **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="79ef0-144">With **Power BI Desktop**, you connect tooyour data source by importing a copy of hello data into **Power BI Desktop** or connecting directly toohello data source using **DirectQuery**.</span></span>

<span data-ttu-id="79ef0-145">Tady jsou hello rozdíly mezi použitím **Import** a **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="79ef0-145">Here are hello differences between using **Import** and **DirectQuery**.</span></span>

| <span data-ttu-id="79ef0-146">Import</span><span class="sxs-lookup"><span data-stu-id="79ef0-146">Import</span></span> | <span data-ttu-id="79ef0-147">DirectQuery</span><span class="sxs-lookup"><span data-stu-id="79ef0-147">DirectQuery</span></span> |
| --- | --- |
| <span data-ttu-id="79ef0-148">Tabulky, sloupce *a data* se naimportují, tedy zkopírují do **Power BI Desktop**.</span><span class="sxs-lookup"><span data-stu-id="79ef0-148">Tables, columns, *and data* are imported or copied into **Power BI Desktop**.</span></span> <span data-ttu-id="79ef0-149">Při práci s vizualizacemi si **Power BI Desktop** dotazem vyžádá kopii dat hello.</span><span class="sxs-lookup"><span data-stu-id="79ef0-149">As you work with visualizations, **Power BI Desktop** queries a copy of hello data.</span></span> <span data-ttu-id="79ef0-150">toosee žádné změny k chybě toohello základní data, musíte aktualizovat, nebo importovat, celou aktuální datovou sadu znovu.</span><span class="sxs-lookup"><span data-stu-id="79ef0-150">toosee any changes that occurred toohello underlying data, you must refresh, or import, a complete, current dataset again.</span></span> |<span data-ttu-id="79ef0-151">Do **Power BI Desktop** se naimportují, tedy zkopírují pouze *tabulky a sloupce*.</span><span class="sxs-lookup"><span data-stu-id="79ef0-151">Only *tables and columns* are imported or copied into **Power BI Desktop**.</span></span> <span data-ttu-id="79ef0-152">Při práci s vizualizacemi si **Power BI Desktop** dotazy hello základní zdroj dat, což znamená, vám vždy zobrazují aktuální data.</span><span class="sxs-lookup"><span data-stu-id="79ef0-152">As you work with visualizations, **Power BI Desktop** queries hello underlying data source, which means you're always viewing current data.</span></span> |

<span data-ttu-id="79ef0-153">Další informace o připojování zdroje dat tooa najdete v tématu [zdroj dat připojit tooa](power-bi-embedded-connect-datasource.md).</span><span class="sxs-lookup"><span data-stu-id="79ef0-153">For more about connecting tooa data source, see [Connect tooa data source](power-bi-embedded-connect-datasource.md).</span></span>

<span data-ttu-id="79ef0-154">Po uložení práce v **Power BI Desktop** se vytvoří soubor PBIX.</span><span class="sxs-lookup"><span data-stu-id="79ef0-154">After you save your work in **Power BI Desktop**, a PBIX file is created.</span></span> <span data-ttu-id="79ef0-155">Tento soubor obsahuje vaši sestavu.</span><span class="sxs-lookup"><span data-stu-id="79ef0-155">This file contains your report.</span></span> <span data-ttu-id="79ef0-156">Kromě toho hello při importu dat hello soubor PBIX obsahovat úplnou datovou sadu, nebo pokud používáte **DirectQuery**, hello soubor PBIX obsahuje jen schéma datové sady.</span><span class="sxs-lookup"><span data-stu-id="79ef0-156">In addition, if you import data hello PBIX contains hello complete dataset, or if you use **DirectQuery**, hello PBIX contains just a dataset schema.</span></span> <span data-ttu-id="79ef0-157">Hello soubor PBIX programově nasadíte do pracovního prostoru pomocí hello [rozhraní API pro Power BI Import](https://msdn.microsoft.com/library/mt711504.aspx).</span><span class="sxs-lookup"><span data-stu-id="79ef0-157">You programmatically deploy hello PBIX into your workspace using hello [Power BI Import API](https://msdn.microsoft.com/library/mt711504.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="79ef0-158">**Power BI Embedded** má další rozhraní API toochange hello server a databáze, datová sada odkazuje tooand sadu pověření účtu služby, který hello datová sada bude používat tooconnect tooyour databáze.</span><span class="sxs-lookup"><span data-stu-id="79ef0-158">**Power BI Embedded** has additional APIs toochange hello server and database that your dataset is pointing tooand set a service account credential that hello dataset will use tooconnect tooyour database.</span></span> <span data-ttu-id="79ef0-159">Viz [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) a [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).</span><span class="sxs-lookup"><span data-stu-id="79ef0-159">See [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) and [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).</span></span>

## <a name="create-power-bi-datasets-and-reports-using-apis"></a><span data-ttu-id="79ef0-160">Vytváření datových sad a sestav Power BI pomocí rozhraní API</span><span class="sxs-lookup"><span data-stu-id="79ef0-160">Create Power BI datasets and reports using APIs</span></span>

### <a name="datsets"></a><span data-ttu-id="79ef0-161">Datové sady</span><span class="sxs-lookup"><span data-stu-id="79ef0-161">Datsets</span></span>

<span data-ttu-id="79ef0-162">Můžete vytvořit datové sady v rámci Power BI Embedded pomocí hello REST API.</span><span class="sxs-lookup"><span data-stu-id="79ef0-162">You can create datasets within Power BI Embedded using hello REST API.</span></span> <span data-ttu-id="79ef0-163">Potom můžete datovou sadu naplnit daty.</span><span class="sxs-lookup"><span data-stu-id="79ef0-163">You can then push data into your dataset.</span></span> <span data-ttu-id="79ef0-164">To vám umožní toowork s daty bez nutnosti hello z Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="79ef0-164">This allows you toowork with data without hello need of Power BI Desktop.</span></span> <span data-ttu-id="79ef0-165">Další informace najdete v tématu [Post Datasets](https://msdn.microsoft.com/library/azure/mt778875.aspx).</span><span class="sxs-lookup"><span data-stu-id="79ef0-165">For more information, see [Post Datasets](https://msdn.microsoft.com/library/azure/mt778875.aspx).</span></span>

### <a name="reports"></a><span data-ttu-id="79ef0-166">Reports</span><span class="sxs-lookup"><span data-stu-id="79ef0-166">Reports</span></span>

<span data-ttu-id="79ef0-167">Můžete vytvořit sestavy z datové sady přímo do vaší aplikace pomocí hello rozhraní API jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="79ef0-167">You can create a report from a dataset directly in your application using hello JavaScript API.</span></span> <span data-ttu-id="79ef0-168">Další informace najdete v tématu s popisem [vytvoření nové sestavy z datové sady v Power BI Embedded](power-bi-embedded-create-report-from-dataset.md).</span><span class="sxs-lookup"><span data-stu-id="79ef0-168">For more information, see [Create a new report from a dataset in Power BI Embedded](power-bi-embedded-create-report-from-dataset.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="79ef0-169">Viz také</span><span class="sxs-lookup"><span data-stu-id="79ef0-169">See Also</span></span>

[<span data-ttu-id="79ef0-170">Začínáme s ukázkou</span><span class="sxs-lookup"><span data-stu-id="79ef0-170">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="79ef0-171">Ověřování a autorizace v Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="79ef0-171">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="79ef0-172">Vložení sestavy</span><span class="sxs-lookup"><span data-stu-id="79ef0-172">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
<span data-ttu-id="79ef0-173">[Vytvoření nové sestavy z datové sady v Power BI Embedded](power-bi-embedded-create-report-from-dataset.md)
[Ukládání sestav](power-bi-embedded-save-reports.md)</span><span class="sxs-lookup"><span data-stu-id="79ef0-173">[Create a new report from a dataset in Power BI Embedded](power-bi-embedded-create-report-from-dataset.md)
[Save reports](power-bi-embedded-save-reports.md)</span></span>  
[<span data-ttu-id="79ef0-174">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="79ef0-174">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="79ef0-175">Vložená ukázka JavaScriptu</span><span class="sxs-lookup"><span data-stu-id="79ef0-175">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="79ef0-176">Chcete se ještě na něco zeptat?</span><span class="sxs-lookup"><span data-stu-id="79ef0-176">More questions?</span></span> [<span data-ttu-id="79ef0-177">Zkuste hello komunitě Power BI</span><span class="sxs-lookup"><span data-stu-id="79ef0-177">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

