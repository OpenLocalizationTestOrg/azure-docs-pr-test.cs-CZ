---
title: "aplikace logiky aaaManage v sadě Visual Studio – Azure Logic Apps | Microsoft Docs"
description: "Správa aplikace logiky a dalších prostředků Azure pomocí Průzkumníka cloudové služby Visual Studio"
author: klam
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 12/19/2016
ms.author: LADocs; klam
ms.openlocfilehash: 419f83eb062b56e4ac2642dea4de1a025f747521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-logic-apps-with-visual-studio-cloud-explorer"></a><span data-ttu-id="565a5-103">Správa aplikace logiky s cloudu Průzkumníka Visual Studio</span><span class="sxs-lookup"><span data-stu-id="565a5-103">Manage your logic apps with Visual Studio Cloud Explorer</span></span>

<span data-ttu-id="565a5-104">I když hello [portál Azure](https://portal.azure.com/) nabízí skvělý způsob pro vás toodesign a spravovat Azure Logic Apps, Průzkumník cloudu Visual Studio můžete použít pro správu mnoho prostředků Azure, včetně aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="565a5-104">Although hello [Azure portal](https://portal.azure.com/) offers a great way for you toodesign and manage Azure Logic Apps, you can use Visual Studio Cloud Explorer for managing many Azure assets, including logic apps.</span></span> <span data-ttu-id="565a5-105">Průzkumník cloudu Visual Studio umožňuje procházet, spravovat, upravit, a stažení publikované aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="565a5-105">Visual Studio Cloud Explorer lets you browse, manage, edit, and download published logic apps.</span></span> <span data-ttu-id="565a5-106">Úlohy správy zahrnují povolit, zakázat a spustit zobrazení historie.</span><span class="sxs-lookup"><span data-stu-id="565a5-106">Management tasks include enable, disable, and view run history.</span></span> 

<span data-ttu-id="565a5-107">Než budete moct přístup a Správa aplikace logiky v sadě Visual Studio, nainstalujte a nakonfigurujte tyto nástroje Visual Studio pro Azure Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="565a5-107">Before you can access and manage your logic apps in Visual Studio, install and configure these Visual Studio tools for Azure Logic Apps.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="565a5-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="565a5-108">Prerequisites</span></span>

* [<span data-ttu-id="565a5-109">Visual Studio 2015 nebo Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="565a5-109">Visual Studio 2015 or Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* <span data-ttu-id="565a5-110">[Nejnovější sadu Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 nebo vyšší)</span><span class="sxs-lookup"><span data-stu-id="565a5-110">[Latest Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 or greater)</span></span>
* [<span data-ttu-id="565a5-111">Průzkumník cloudu sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="565a5-111">Visual Studio Cloud Explorer</span></span>](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015)
* <span data-ttu-id="565a5-112">Přístup k toohello webové při použití vložených Návrhář hello</span><span class="sxs-lookup"><span data-stu-id="565a5-112">Access toohello web when using hello embedded designer</span></span>

## <a name="install-visual-studio-tools-for-logic-apps"></a><span data-ttu-id="565a5-113">Instalace nástrojů Visual Studio pro Logic Apps</span><span class="sxs-lookup"><span data-stu-id="565a5-113">Install Visual Studio tools for Logic Apps</span></span>

<span data-ttu-id="565a5-114">Po instalaci hello požadavky, stáhněte a nainstalujte nástroje aplikace logiky hello Azure pro sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="565a5-114">After you install hello prerequisites, download and install hello Azure Logic Apps Tools for Visual Studio.</span></span>

1. <span data-ttu-id="565a5-115">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="565a5-115">Open Visual Studio.</span></span> <span data-ttu-id="565a5-116">Na hello **nástroje** nabídce vyberte možnost **rozšíření a aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="565a5-116">On hello **Tools** menu, select **Extensions and Updates**.</span></span>
2. <span data-ttu-id="565a5-117">Rozbalte hello **Online** kategorie, můžete hledat online v hello Galerie sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="565a5-117">Expand hello **Online** category so you can search online in hello Visual Studio Gallery.</span></span>
3. <span data-ttu-id="565a5-118">Procházet nebo Hledat **Logic Apps** vyhledejte **nástroje aplikace logiky Azure pro sadu Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="565a5-118">Browse or search for **Logic Apps** until you find **Azure Logic Apps Tools for Visual Studio**.</span></span>
4. <span data-ttu-id="565a5-119">toodownload a příponu hello instalace, klikněte na tlačítko **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="565a5-119">toodownload and install hello extension, click **Download**.</span></span>
5. <span data-ttu-id="565a5-120">Po instalaci, restartujte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="565a5-120">Restart Visual Studio after installation.</span></span>

> [!NOTE]
> <span data-ttu-id="565a5-121">hello toodownload nástroje aplikace logiky Azure pro sadu Visual Studio přejděte přímo, toohello [Visual Studio Marketplace](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9).</span><span class="sxs-lookup"><span data-stu-id="565a5-121">toodownload hello Azure Logic Apps Tools for Visual Studio directly, go toohello [Visual Studio Marketplace](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9).</span></span>

## <a name="browse-for-logic-apps-in-cloud-explorer"></a><span data-ttu-id="565a5-122">Procházením vyhledejte aplikace logiky v Průzkumníku cloudu</span><span class="sxs-lookup"><span data-stu-id="565a5-122">Browse for logic apps in Cloud Explorer</span></span>

1.  <span data-ttu-id="565a5-123">tooopen Průzkumník cloudu na hello **zobrazení** nabídce zvolte **Průzkumník cloudu**.</span><span class="sxs-lookup"><span data-stu-id="565a5-123">tooopen Cloud Explorer, on hello **View** menu, choose **Cloud Explorer**.</span></span>
2.  <span data-ttu-id="565a5-124">Procházením vyhledejte aplikaci logiky, skupinu prostředků nebo typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="565a5-124">Browse for your logic app, either by resource group or by resource type.</span></span> 

    * <span data-ttu-id="565a5-125">Pokud podle typů prostředků, vyberte předplatné Azure, rozbalte položku hello **Logic Apps** a vyberte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="565a5-125">If you browse by resource type, select your Azure subscription, expand hello **Logic Apps** section, and select your logic app.</span></span> 
    * <span data-ttu-id="565a5-126">Pokud ve skupině prostředků, rozbalte skupinu prostředků hello, který má svou aplikaci logiky a vyberte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="565a5-126">If you browse by resource group, expand hello resource group that has your logic app, and select your logic app.</span></span>

    <span data-ttu-id="565a5-127">příkazy tooview pro svou aplikaci logiky buď klikněte pravým tlačítkem na svou aplikaci logiky, nebo v hello dolní části Průzkumník cloudu, zvolte z hello **akce** nabídky.</span><span class="sxs-lookup"><span data-stu-id="565a5-127">tooview commands for your logic app, either right-click your logic app, or at hello bottom of Cloud Explorer, choose from hello **Actions** menu.</span></span>

    ![Procházením vyhledejte aplikaci logiky](./media/logic-apps-manage-from-vs/browse.png)

## <a name="edit-your-logic-app-with-logic-apps-designer"></a><span data-ttu-id="565a5-129">Upravit aplikaci logiky pomocí návrháře aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="565a5-129">Edit your logic app with Logic Apps Designer</span></span>

<span data-ttu-id="565a5-130">V Průzkumníku cloudu, můžete otevřít aplikace logiky aktuálně nasazená v hello stejné designer, které budete používat v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="565a5-130">From Cloud Explorer, you can open a currently deployed logic app in hello same designer that you use in hello Azure portal.</span></span> 

* <span data-ttu-id="565a5-131">tooedit aplikace logiky v Průzkumníku cloudu, klikněte pravým tlačítkem na svou aplikaci logiky a vyberte **otevřete pomocí editoru aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="565a5-131">tooedit your logic app, in Cloud Explorer, right-click your logic app, and select **Open with Logic App Editor**.</span></span> 

* <span data-ttu-id="565a5-132">toopublish cloudu vaší toohello aktualizace, zvolte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="565a5-132">toopublish your updates toohello cloud, choose **Publish**.</span></span> 

* <span data-ttu-id="565a5-133">Zvolte toostart spuštění nové **spustit aktivační událost**.</span><span class="sxs-lookup"><span data-stu-id="565a5-133">toostart a new run, choose **Run Trigger**.</span></span>

![Návrhář aplikace logiky](./media/logic-apps-manage-from-vs/designer.png)

<span data-ttu-id="565a5-135">Z Návrháře hello, můžete také **Stáhnout** aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="565a5-135">From hello designer, you can also **Download** a logic app.</span></span> <span data-ttu-id="565a5-136">Tato akce automaticky parameterizes definici aplikace logiky hello a uloží hello definice jako šablonu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="565a5-136">This action automatically parameterizes hello logic app definition, and saves hello definition as an Azure Resource Manager deployment template.</span></span> <span data-ttu-id="565a5-137">Můžete přidat tento projekt skupiny prostředků Azure tooyour šablonu nasazení.</span><span class="sxs-lookup"><span data-stu-id="565a5-137">You can add this deployment template tooyour Azure Resource Group project.</span></span>

## <a name="browse-your-logic-app-run-history"></a><span data-ttu-id="565a5-138">Procházet aplikace logiky historie spouštění</span><span class="sxs-lookup"><span data-stu-id="565a5-138">Browse your logic app run history</span></span>

<span data-ttu-id="565a5-139">Klikněte pravým tlačítkem na svou aplikaci logiky tooview hello historie pro svou aplikaci logiky, spouštění a vyberte **historie spouštění otevřete**.</span><span class="sxs-lookup"><span data-stu-id="565a5-139">tooview hello run history for your logic app, right-click your logic app, and select **Open run history**.</span></span> <span data-ttu-id="565a5-140">na základě vaší historie spouštění tooreorder na žádném z záhlaví sloupce hello uvedené, vyberte vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="565a5-140">tooreorder your run history based on any of hello properties shown, select hello column header.</span></span>

![Historie spouštění](media/logic-apps-manage-from-vs/runs.png)

<span data-ttu-id="565a5-142">hello tooshow historie pro instanci spouštění, můžete zkontrolovat hello spustit výsledky včetně hello vstupy a výstupy z každého kroku, dvakrát klikněte na spustit instance hello.</span><span class="sxs-lookup"><span data-stu-id="565a5-142">tooshow hello run history for an instance so you can review hello run results, including hello inputs and outputs from each step, double-click one of hello run instances.</span></span>

![Výsledky historie spouštění, vstupy a výstupy z kroků](./media/logic-apps-manage-from-vs/history.png)

## <a name="next-steps"></a><span data-ttu-id="565a5-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="565a5-144">Next steps</span></span>

* [<span data-ttu-id="565a5-145">Vytvoření první aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="565a5-145">Create your first logic app</span></span>](logic-apps-create-a-logic-app.md)
* [<span data-ttu-id="565a5-146">Návrh, vytvoření a nasazení aplikace logiky v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="565a5-146">Design, build, and deploy logic apps in Visual Studio</span></span>](logic-apps-deploy-from-vs.md)
* <span data-ttu-id="565a5-147">[Zobrazení běžných příkladů a scénářů](logic-apps-examples-and-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="565a5-147">[View common examples and scenarios](logic-apps-examples-and-scenarios.md)</span></span>
* [<span data-ttu-id="565a5-148">Video: Automatizovat firemní procesy službou Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="565a5-148">Video: Automate business processes with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/T694)
* [<span data-ttu-id="565a5-149">Video: Integrate systémy s Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="565a5-149">Video: Integrate your systems with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/P462)
