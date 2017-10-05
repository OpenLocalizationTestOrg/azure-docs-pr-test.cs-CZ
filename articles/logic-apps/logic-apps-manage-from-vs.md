---
title: "Správa aplikací logiky v sadě Visual Studio – Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: a5bf24de1a7a2b6d4c1ae6416c95d83ef7506da3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="manage-your-logic-apps-with-visual-studio-cloud-explorer"></a><span data-ttu-id="10b81-103">Správa aplikace logiky s cloudu Průzkumníka Visual Studio</span><span class="sxs-lookup"><span data-stu-id="10b81-103">Manage your logic apps with Visual Studio Cloud Explorer</span></span>

<span data-ttu-id="10b81-104">I když [portál Azure](https://portal.azure.com/) nabízí skvělý způsob, jak můžete navrhnout a spravovat Azure Logic Apps, můžete použít Průzkumníka cloudové služby Visual Studio pro správu mnoho prostředků Azure, včetně aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="10b81-104">Although the [Azure portal](https://portal.azure.com/) offers a great way for you to design and manage Azure Logic Apps, you can use Visual Studio Cloud Explorer for managing many Azure assets, including logic apps.</span></span> <span data-ttu-id="10b81-105">Průzkumník cloudu Visual Studio umožňuje procházet, spravovat, upravit, a stažení publikované aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="10b81-105">Visual Studio Cloud Explorer lets you browse, manage, edit, and download published logic apps.</span></span> <span data-ttu-id="10b81-106">Úlohy správy zahrnují povolit, zakázat a spustit zobrazení historie.</span><span class="sxs-lookup"><span data-stu-id="10b81-106">Management tasks include enable, disable, and view run history.</span></span> 

<span data-ttu-id="10b81-107">Než budete moct přístup a Správa aplikace logiky v sadě Visual Studio, nainstalujte a nakonfigurujte tyto nástroje Visual Studio pro Azure Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="10b81-107">Before you can access and manage your logic apps in Visual Studio, install and configure these Visual Studio tools for Azure Logic Apps.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="10b81-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="10b81-108">Prerequisites</span></span>

* [<span data-ttu-id="10b81-109">Visual Studio 2015 nebo Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="10b81-109">Visual Studio 2015 or Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* <span data-ttu-id="10b81-110">[Nejnovější sadu Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 nebo vyšší)</span><span class="sxs-lookup"><span data-stu-id="10b81-110">[Latest Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 or greater)</span></span>
* [<span data-ttu-id="10b81-111">Průzkumník cloudu sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="10b81-111">Visual Studio Cloud Explorer</span></span>](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015)
* <span data-ttu-id="10b81-112">Přístup k webovému při použití návrháře embedded</span><span class="sxs-lookup"><span data-stu-id="10b81-112">Access to the web when using the embedded designer</span></span>

## <a name="install-visual-studio-tools-for-logic-apps"></a><span data-ttu-id="10b81-113">Instalace nástrojů Visual Studio pro Logic Apps</span><span class="sxs-lookup"><span data-stu-id="10b81-113">Install Visual Studio tools for Logic Apps</span></span>

<span data-ttu-id="10b81-114">Po instalaci požadavky, stáhněte a nainstalujte nástroje aplikace logiky Azure pro sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="10b81-114">After you install the prerequisites, download and install the Azure Logic Apps Tools for Visual Studio.</span></span>

1. <span data-ttu-id="10b81-115">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="10b81-115">Open Visual Studio.</span></span> <span data-ttu-id="10b81-116">Na **nástroje** nabídce vyberte možnost **rozšíření a aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="10b81-116">On the **Tools** menu, select **Extensions and Updates**.</span></span>
2. <span data-ttu-id="10b81-117">Rozbalte **Online** kategorie, můžete hledat online v Galerii Visual Studia.</span><span class="sxs-lookup"><span data-stu-id="10b81-117">Expand the **Online** category so you can search online in the Visual Studio Gallery.</span></span>
3. <span data-ttu-id="10b81-118">Procházet nebo Hledat **Logic Apps** vyhledejte **nástroje aplikace logiky Azure pro sadu Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="10b81-118">Browse or search for **Logic Apps** until you find **Azure Logic Apps Tools for Visual Studio**.</span></span>
4. <span data-ttu-id="10b81-119">Chcete-li stáhnout a nainstalovat rozšíření, klikněte na tlačítko **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="10b81-119">To download and install the extension, click **Download**.</span></span>
5. <span data-ttu-id="10b81-120">Po instalaci, restartujte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="10b81-120">Restart Visual Studio after installation.</span></span>

> [!NOTE]
> <span data-ttu-id="10b81-121">Pokud chcete stáhnout nástroje aplikace logiky Azure pro sadu Visual Studio přímo, přejděte na [Visual Studio Marketplace](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9).</span><span class="sxs-lookup"><span data-stu-id="10b81-121">To download the Azure Logic Apps Tools for Visual Studio directly, go to the [Visual Studio Marketplace](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9).</span></span>

## <a name="browse-for-logic-apps-in-cloud-explorer"></a><span data-ttu-id="10b81-122">Procházením vyhledejte aplikace logiky v Průzkumníku cloudu</span><span class="sxs-lookup"><span data-stu-id="10b81-122">Browse for logic apps in Cloud Explorer</span></span>

1.  <span data-ttu-id="10b81-123">Chcete otevřít v Průzkumníku cloudu **zobrazení** nabídce zvolte **Průzkumník cloudu**.</span><span class="sxs-lookup"><span data-stu-id="10b81-123">To open Cloud Explorer, on the **View** menu, choose **Cloud Explorer**.</span></span>
2.  <span data-ttu-id="10b81-124">Procházením vyhledejte aplikaci logiky, skupinu prostředků nebo typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="10b81-124">Browse for your logic app, either by resource group or by resource type.</span></span> 

    * <span data-ttu-id="10b81-125">Pokud podle typů prostředků, vyberte předplatné Azure, rozbalte **Logic Apps** a vyberte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="10b81-125">If you browse by resource type, select your Azure subscription, expand the **Logic Apps** section, and select your logic app.</span></span> 
    * <span data-ttu-id="10b81-126">Pokud ve skupině prostředků, rozbalte skupinu prostředků, která má svou aplikaci logiky a vyberte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="10b81-126">If you browse by resource group, expand the resource group that has your logic app, and select your logic app.</span></span>

    <span data-ttu-id="10b81-127">Pokud chcete zobrazit příkazy pro svou aplikaci logiky, klikněte pravým tlačítkem na svou aplikaci logiky buď, nebo v dolní části Průzkumník cloudu, vyberte z **akce** nabídky.</span><span class="sxs-lookup"><span data-stu-id="10b81-127">To view commands for your logic app, either right-click your logic app, or at the bottom of Cloud Explorer, choose from the **Actions** menu.</span></span>

    ![Procházením vyhledejte aplikaci logiky](./media/logic-apps-manage-from-vs/browse.png)

## <a name="edit-your-logic-app-with-logic-apps-designer"></a><span data-ttu-id="10b81-129">Upravit aplikaci logiky pomocí návrháře aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="10b81-129">Edit your logic app with Logic Apps Designer</span></span>

<span data-ttu-id="10b81-130">V Průzkumníku cloudu můžete otevřít aplikace logiky aktuálně nasazená v Návrháři stejné, které budete používat v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="10b81-130">From Cloud Explorer, you can open a currently deployed logic app in the same designer that you use in the Azure portal.</span></span> 

* <span data-ttu-id="10b81-131">Chcete-li upravit aplikaci logiky v Průzkumníku cloudu, klikněte pravým tlačítkem na svou aplikaci logiky a vyberte **otevřete pomocí editoru aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="10b81-131">To edit your logic app, in Cloud Explorer, right-click your logic app, and select **Open with Logic App Editor**.</span></span> 

* <span data-ttu-id="10b81-132">Chcete-li publikovat aktualizace do cloudu, zvolte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="10b81-132">To publish your updates to the cloud, choose **Publish**.</span></span> 

* <span data-ttu-id="10b81-133">Chcete-li spustit novou spustit, zvolte **spustit aktivační událost**.</span><span class="sxs-lookup"><span data-stu-id="10b81-133">To start a new run, choose **Run Trigger**.</span></span>

![Návrhář aplikace logiky](./media/logic-apps-manage-from-vs/designer.png)

<span data-ttu-id="10b81-135">Z návrháře, můžete také **Stáhnout** aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="10b81-135">From the designer, you can also **Download** a logic app.</span></span> <span data-ttu-id="10b81-136">Tato akce automaticky parameterizes definici aplikace logiky a uloží definici jako šablonu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="10b81-136">This action automatically parameterizes the logic app definition, and saves the definition as an Azure Resource Manager deployment template.</span></span> <span data-ttu-id="10b81-137">Tato šablona nasazení můžete přidat do projektu skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="10b81-137">You can add this deployment template to your Azure Resource Group project.</span></span>

## <a name="browse-your-logic-app-run-history"></a><span data-ttu-id="10b81-138">Procházet aplikace logiky historie spouštění</span><span class="sxs-lookup"><span data-stu-id="10b81-138">Browse your logic app run history</span></span>

<span data-ttu-id="10b81-139">Chcete-li zobrazit historii spouštění aplikace logiky, klikněte pravým tlačítkem na svou aplikaci logiky a vyberte **historie spouštění otevřete**.</span><span class="sxs-lookup"><span data-stu-id="10b81-139">To view the run history for your logic app, right-click your logic app, and select **Open run history**.</span></span> <span data-ttu-id="10b81-140">Chcete-li změnit pořadí historii spuštění na základě některé vlastnosti zobrazené, vyberte na záhlaví sloupce.</span><span class="sxs-lookup"><span data-stu-id="10b81-140">To reorder your run history based on any of the properties shown, select the column header.</span></span>

![Historie spouštění](media/logic-apps-manage-from-vs/runs.png)

<span data-ttu-id="10b81-142">Pokud chcete zobrazit historii spouštění instance, můžete zkontrolovat spuštění výsledky, včetně vstupy a výstupy z každého kroku, dvakrát klikněte na jednu z instancí spuštění.</span><span class="sxs-lookup"><span data-stu-id="10b81-142">To show the run history for an instance so you can review the run results, including the inputs and outputs from each step, double-click one of the run instances.</span></span>

![Výsledky historie spouštění, vstupy a výstupy z kroků](./media/logic-apps-manage-from-vs/history.png)

## <a name="next-steps"></a><span data-ttu-id="10b81-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="10b81-144">Next steps</span></span>

* [<span data-ttu-id="10b81-145">Vytvoření první aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="10b81-145">Create your first logic app</span></span>](logic-apps-create-a-logic-app.md)
* [<span data-ttu-id="10b81-146">Návrh, vytvoření a nasazení aplikace logiky v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="10b81-146">Design, build, and deploy logic apps in Visual Studio</span></span>](logic-apps-deploy-from-vs.md)
* <span data-ttu-id="10b81-147">[Zobrazení běžných příkladů a scénářů](logic-apps-examples-and-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="10b81-147">[View common examples and scenarios](logic-apps-examples-and-scenarios.md)</span></span>
* [<span data-ttu-id="10b81-148">Video: Automatizovat firemní procesy službou Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="10b81-148">Video: Automate business processes with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/T694)
* [<span data-ttu-id="10b81-149">Video: Integrate systémy s Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="10b81-149">Video: Integrate your systems with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/P462)
