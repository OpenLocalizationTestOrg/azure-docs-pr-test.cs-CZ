---
title: "aaaViews v řešení pro správu Operations Management Suite (OMS) | Microsoft Docs"
description: "Řešení pro správu v Operations Management Suite (OMS) by měl obvykle zahrnovat jeden nebo více toovisualize dat zobrazení.  Tento článek popisuje, jak tooexport zobrazení vytvořené hello Návrhář zobrazení a její zahrnutí do řešení pro správu. "
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 570b278c-2d47-4e5a-9828-7f01f31ddf8c
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: 303861465014a27289f831332b3d95925c0ae66d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a><span data-ttu-id="5b2f2-104">Zobrazení v řešení pro správu Operations Management Suite (OMS) (Preview)</span><span class="sxs-lookup"><span data-stu-id="5b2f2-104">Views in Operations Management Suite (OMS) management solutions (Preview)</span></span>
> [!NOTE]
> <span data-ttu-id="5b2f2-105">Toto je předběžná dokumentace pro vytváření řešení pro správu v OMS, které jsou aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-105">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="5b2f2-106">Žádné schéma níže popsané je toochange subjektu.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-106">Any schema described below is subject toochange.</span></span>    
>
>

<span data-ttu-id="5b2f2-107">[Řešení pro správu v Operations Management Suite (OMS)](operations-management-suite-solutions.md) by měl obvykle zahrnovat jeden nebo více toovisualize dat zobrazení.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-107">[Management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions.md) will typically include one or more views toovisualize data.</span></span>  <span data-ttu-id="5b2f2-108">Tento článek popisuje, jak tooexport zobrazení vytvořené hello [Návrhář zobrazení](../log-analytics/log-analytics-view-designer.md) a její zahrnutí do řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-108">This article describes how tooexport a view created by hello [View Designer](../log-analytics/log-analytics-view-designer.md) and include it in a management solution.</span></span>  

> [!NOTE]
> <span data-ttu-id="5b2f2-109">Použijte parametry a proměnné, které jsou buď požadované, nebo běžné toomanagement řešení a popsané v zprostředkovatele Hello ukázky v tomto článku [vytváření řešení pro správu v Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span><span class="sxs-lookup"><span data-stu-id="5b2f2-109">hello samples in this article use parameters and variables that are either required or common toomanagement solutions  and described in [Creating management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="5b2f2-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5b2f2-110">Prerequisites</span></span>
<span data-ttu-id="5b2f2-111">Tento článek předpokládá, že jste již obeznámeni s postupy příliš[vytvoření řešení správy](operations-management-suite-solutions-creating.md) a struktuře hello soubor řešení.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-111">This article assumes that you're already familiar with how too[create a management solution](operations-management-suite-solutions-creating.md) and hello structure of a solution file.</span></span>

## <a name="overview"></a><span data-ttu-id="5b2f2-112">Přehled</span><span class="sxs-lookup"><span data-stu-id="5b2f2-112">Overview</span></span>
<span data-ttu-id="5b2f2-113">tooinclude zobrazení v rámci řešení pro správu, vytvoříte **prostředků** pro něj v hello [soubor řešení](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="5b2f2-113">tooinclude a view in a management solution, you create a **resource** for it in hello [solution file](operations-management-suite-solutions-creating.md).</span></span>  <span data-ttu-id="5b2f2-114">Hello formátu JSON, který popisuje konfiguraci podrobné zobrazení hello je obvykle komplexní ale a není něco že autor typické řešení by bylo možné toocreate ručně.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-114">hello JSON that describes hello view's detailed configuration is typically complex though and not something that a typical solution author would be able toocreate manually.</span></span>  <span data-ttu-id="5b2f2-115">Hello nejběžnější metodou je toocreate hello zobrazení pomocí hello [Návrhář zobrazení](../log-analytics/log-analytics-view-designer.md), ho exportovat a poté přidejte jeho řešení toohello podrobnou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-115">hello most common method is toocreate hello view using hello [View Designer](../log-analytics/log-analytics-view-designer.md), export it, and then add its detailed configuration toohello solution.</span></span>

<span data-ttu-id="5b2f2-116">základní kroky tooadd Hello řešení tooa zobrazení jsou následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-116">hello basic steps tooadd a view tooa solution are as follows.</span></span>  <span data-ttu-id="5b2f2-117">Každý krok je podrobně popsán dále v následujících částech hello.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-117">Each step is described in further detail in hello sections below.</span></span>

1. <span data-ttu-id="5b2f2-118">Exportujte soubor tooa zobrazení hello.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-118">Export hello view tooa file.</span></span>
2. <span data-ttu-id="5b2f2-119">Vytvořte prostředek hello zobrazení v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-119">Create hello view resource in hello solution.</span></span>
3. <span data-ttu-id="5b2f2-120">Přidejte hello zobrazit podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-120">Add hello view details.</span></span>

## <a name="export-hello-view-tooa-file"></a><span data-ttu-id="5b2f2-121">Exportovat soubor tooa zobrazení hello</span><span class="sxs-lookup"><span data-stu-id="5b2f2-121">Export hello view tooa file</span></span>
<span data-ttu-id="5b2f2-122">Postupujte podle pokynů hello [Návrhář zobrazení Log Analytics](../log-analytics/log-analytics-view-designer.md) tooexport soubor tooa zobrazení.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-122">Follow hello instructions at [Log Analytics View Designer](../log-analytics/log-analytics-view-designer.md) tooexport a view tooa file.</span></span>  <span data-ttu-id="5b2f2-123">Hello exportovaný soubor bude mít ve formátu JSON s hello stejné [elementy jako soubor řešení hello](operations-management-suite-solutions-solution-file.md).</span><span class="sxs-lookup"><span data-stu-id="5b2f2-123">hello exported file will be in JSON format with hello same [elements as hello solution file](operations-management-suite-solutions-solution-file.md).</span></span>  

<span data-ttu-id="5b2f2-124">Hello **prostředky** prvek hello zobrazení souboru bude mít prostředek s typem **Microsoft.OperationalInsights/workspaces** , představuje hello pracovním prostorem OMS.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-124">hello **resources** element of hello view file will have a resource with a type of **Microsoft.OperationalInsights/workspaces** that represents hello OMS workspace.</span></span>  <span data-ttu-id="5b2f2-125">Tento prvek bude mít dílčí prvek s typem **zobrazení** , představuje hello zobrazení a obsahuje podrobné konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-125">This element will have a subelement with a type of **views** that represents hello view and contains its detailed configuration.</span></span>  <span data-ttu-id="5b2f2-126">Bude kopírovat hello podrobnosti tohoto elementu a zkopírujte jej do řešení.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-126">You will copy hello details of this element and then copy it into your solution.</span></span>

## <a name="create-hello-view-resource-in-hello-solution"></a><span data-ttu-id="5b2f2-127">Vytvořit prostředek hello zobrazení v řešení hello</span><span class="sxs-lookup"><span data-stu-id="5b2f2-127">Create hello view resource in hello solution</span></span>
<span data-ttu-id="5b2f2-128">Přidejte následující zobrazení prostředků toohello hello **prostředky** element souboru řešení.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-128">Add hello following view resource toohello **resources** element of your solution file.</span></span>  <span data-ttu-id="5b2f2-129">Tato služba využívá proměnné, které jsou popsané níže, musíte taky přidat.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-129">This uses variables that are described below that you must also add.</span></span>  <span data-ttu-id="5b2f2-130">Všimněte si, že hello **řídicí panel** a **OverviewTile** vlastnosti jsou zástupné symboly, které bude přepsat hello odpovídající vlastnosti z hello exportovaný soubor zobrazení.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-130">Note that hello **Dashboard** and **OverviewTile** properties are placeholders that you will overwrite with hello corresponding properties from hello exported view file.</span></span>

    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        "dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile":
        }
    }

<span data-ttu-id="5b2f2-131">Přidejte následující proměnné toohello proměnné element soubor řešení hello hello a nahraďte hello toothose hodnoty pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-131">Add hello following variables toohello variables element of hello solution file and replace hello values toothose for your solution.</span></span>

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of hello view."
    "ViewName": "Provide a name for hello view here."


<span data-ttu-id="5b2f2-132">Všimněte si, že může kopírovat hello celého zobrazení prostředek z exportovaného zobrazení souboru, ale potřebovali byste toomake hello následující změny pro něj toowork ve vašem řešení.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-132">Note that you could copy hello entire view resource from your exported view file, but you would need toomake hello following changes for it toowork in your solution.</span></span>  

* <span data-ttu-id="5b2f2-133">Hello **typ** pro zobrazení hello prostředků musí toobe se změnil z **zobrazení** příliš**Microsoft.OperationalInsights/workspaces**.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-133">hello **type** for hello view resource needs toobe changed from **views** too**Microsoft.OperationalInsights/workspaces**.</span></span>
* <span data-ttu-id="5b2f2-134">Hello **název** vlastnost pro prostředek zobrazení hello musí název pracovního prostoru hello tooinclude toobe změnit.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-134">hello **name** property for hello view resource needs toobe changed tooinclude hello workspace name.</span></span>
* <span data-ttu-id="5b2f2-135">Hello závislost na hello prostoru musí toobe odebrat, protože hello prostoru prostředek není definován v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-135">hello dependency on hello workspace needs toobe removed since hello workspace resource isn't defined in hello solution.</span></span>
* <span data-ttu-id="5b2f2-136">**DisplayName** toobe vlastnost potřeby přidat toohello zobrazení.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-136">**DisplayName** property needs toobe added toohello view.</span></span>  <span data-ttu-id="5b2f2-137">Hello **Id**, **název**, a **DisplayName** musí všechny shodovat.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-137">hello **Id**, **Name**, and **DisplayName** must all match.</span></span>
* <span data-ttu-id="5b2f2-138">Názvy parametrů musí být změněno toomatch hello vyžaduje sadu parametrů.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-138">Parameter names must be changed toomatch hello required set of parameters.</span></span>
* <span data-ttu-id="5b2f2-139">Proměnné by měl definovaný v hello řešení a použít v příslušných vlastností hello.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-139">Variables should be defined in hello solution and used in hello appropriate properties.</span></span>

## <a name="add-hello-view-details"></a><span data-ttu-id="5b2f2-140">Přidat hello zobrazit podrobnosti</span><span class="sxs-lookup"><span data-stu-id="5b2f2-140">Add hello view details</span></span>
<span data-ttu-id="5b2f2-141">zobrazení prostředků Hello v hello exportovat soubor bude obsahovat dva elementy v hello zobrazení **vlastnosti** element s názvem **řídicí panel** a **OverviewTile** které obsahují hello podrobnou konfiguraci hello zobrazení.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-141">hello view resource in hello exported view file will contain two elements in hello **properties** element named **Dashboard** and **OverviewTile** which contain hello detailed configuration of hello view.</span></span>  <span data-ttu-id="5b2f2-142">Zkopírujte tyto dva prvky a jejich obsah do hello **vlastnosti** element hello zobrazení prostředků v souboru řešení.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-142">Copy these two elements and their contents into hello **properties** element of hello view resource in your solution file.</span></span>

## <a name="example"></a><span data-ttu-id="5b2f2-143">Příklad</span><span class="sxs-lookup"><span data-stu-id="5b2f2-143">Example</span></span>
<span data-ttu-id="5b2f2-144">Například následující ukázkový text hello ukazuje soubor simple řešení se zobrazením.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-144">For example, hello following sample shows a simple solution file with a view.</span></span>  <span data-ttu-id="5b2f2-145">Výpustky (...) se zobrazují pro hello **řídicí panel** a **OverviewTile** obsah z důvodů místa.</span><span class="sxs-lookup"><span data-stu-id="5b2f2-145">Ellipses (...) are shown for hello **Dashboard** and **OverviewTile** contents for space reasons.</span></span>

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
          ]
    }




## <a name="next-steps"></a><span data-ttu-id="5b2f2-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5b2f2-146">Next steps</span></span>
* <span data-ttu-id="5b2f2-147">Další podrobné informace o vytváření [řešení pro správu](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="5b2f2-147">Learn complete details of creating [management solutions](operations-management-suite-solutions-creating.md).</span></span>
* <span data-ttu-id="5b2f2-148">Zahrnout [runbooků služeb automatizace ve vašem řešení správy](operations-management-suite-solutions-resources-automation.md).</span><span class="sxs-lookup"><span data-stu-id="5b2f2-148">Include [Automation runbooks in your management solution](operations-management-suite-solutions-resources-automation.md).</span></span>
