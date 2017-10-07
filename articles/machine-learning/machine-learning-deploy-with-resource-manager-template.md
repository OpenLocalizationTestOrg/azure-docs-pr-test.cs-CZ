---
title: "aaaDeploy pracovní prostor Machine Learning s Azure Resource Manager | Microsoft Docs"
description: "Jak toodeploy pracovního prostoru pro Azure Machine Learning pomocí šablony Azure Resource Manageru"
services: machine-learning
documentationcenter: 
author: ahgyger
manager: haining
editor: garye
ms.assetid: 4955ac4d-ff99-4908-aa27-69b6bfcc8e85
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/15/2017
ms.author: ahgyger
ms.openlocfilehash: 308959825bcbd670f6ce9b6dc381be767f172357
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a><span data-ttu-id="d59f3-103">Nasazení pracovního prostoru Machine Learning pomocí Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="d59f3-103">Deploy Machine Learning Workspace Using Azure Resource Manager</span></span>
## <a name="introduction"></a><span data-ttu-id="d59f3-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="d59f3-104">Introduction</span></span>
<span data-ttu-id="d59f3-105">Pomocí Azure Resource Manager uloží šablonu nasazení, je čas tak, že jste toodeploy škálovatelné způsob vzájemně propojeny součásti mechanismus ověřování a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="d59f3-105">Using an Azure Resource Manager deployment template saves you time by giving you a scalable way toodeploy interconnected components with a validation and retry mechanism.</span></span> <span data-ttu-id="d59f3-106">tooset si Azure Machine Learning pracovní prostory, například musíte toofirst nakonfigurujte účet úložiště Azure a pak nasaďte pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="d59f3-106">tooset up Azure Machine Learning Workspaces, for example, you need toofirst configure an Azure storage account and then deploy your workspace.</span></span> <span data-ttu-id="d59f3-107">Představte si to ručně ve velkém množství pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="d59f3-107">Imagine doing this manually for hundreds of workspaces.</span></span> <span data-ttu-id="d59f3-108">Jednodušší alternativou je toouse toodeploy šablony Azure Resource Manager pracovní prostor služby Azure Machine Learning a všechny jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="d59f3-108">An easier alternative is toouse an Azure Resource Manager template toodeploy an Azure Machine Learning Workspace and all its dependencies.</span></span> <span data-ttu-id="d59f3-109">Tento článek vás provede krok za krokem tohoto procesu.</span><span class="sxs-lookup"><span data-stu-id="d59f3-109">This article takes you through this process step-by-step.</span></span> <span data-ttu-id="d59f3-110">Skvělý Přehled služby Správce prostředků Azure, najdete v části [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d59f3-110">For a great overview of Azure Resource Manager, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="step-by-step-create-a-machine-learning-workspace"></a><span data-ttu-id="d59f3-111">Postup: vytvoření pracovního prostoru Machine Learning</span><span class="sxs-lookup"><span data-stu-id="d59f3-111">Step-by-step: create a Machine Learning Workspace</span></span>
<span data-ttu-id="d59f3-112">Jsme se vytvořit skupinu prostředků Azure a pak nasaďte nový účet úložiště Azure a nový Azure Machine Learning pracovní prostor pomocí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="d59f3-112">We will create an Azure resource group, then deploy a new Azure storage account and a new Azure Machine Learning Workspace using a Resource Manager template.</span></span> <span data-ttu-id="d59f3-113">Po dokončení nasazení hello jsme se vytiskněte důležité informace o pracovních prostorech hello, které byly vytvořeny (hello primární klíč, hello workspaceID a hello URL toohello prostoru).</span><span class="sxs-lookup"><span data-stu-id="d59f3-113">Once hello deployment is complete, we will print out important information about hello workspaces that were created (hello primary key, hello workspaceID, and hello URL toohello workspace).</span></span>

### <a name="create-an-azure-resource-manager-template"></a><span data-ttu-id="d59f3-114">Vytvořit šablonu Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="d59f3-114">Create an Azure Resource Manager template</span></span>
<span data-ttu-id="d59f3-115">Pracovní prostor Machine Learning vyžaduje tooit úložiště Azure účet toostore hello propojená datová sada.</span><span class="sxs-lookup"><span data-stu-id="d59f3-115">A Machine Learning Workspace requires an Azure storage account toostore hello dataset linked tooit.</span></span>
<span data-ttu-id="d59f3-116">Hello následující šablona používá název hello hello název skupiny prostředků toogenerate hello úložiště účet a název pracovního prostoru hello.</span><span class="sxs-lookup"><span data-stu-id="d59f3-116">hello following template uses hello name of hello resource group toogenerate hello storage account name and hello workspace name.</span></span>  <span data-ttu-id="d59f3-117">Také používá název účtu úložiště hello jako vlastnost při vytváření pracovního prostoru hello.</span><span class="sxs-lookup"><span data-stu-id="d59f3-117">It also uses hello storage account name as a property when creating hello workspace.</span></span>

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
<span data-ttu-id="d59f3-118">Uložte jako soubor mlworkspace.json pod c:\temp\ této šablony.</span><span class="sxs-lookup"><span data-stu-id="d59f3-118">Save this template as mlworkspace.json file under c:\temp\.</span></span>

### <a name="deploy-hello-resource-group-based-on-hello-template"></a><span data-ttu-id="d59f3-119">Nasazení hello skupinu prostředků, na základě šablony hello</span><span class="sxs-lookup"><span data-stu-id="d59f3-119">Deploy hello resource group, based on hello template</span></span>
* <span data-ttu-id="d59f3-120">Otevřete prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d59f3-120">Open PowerShell</span></span>
* <span data-ttu-id="d59f3-121">Instalace modulů pro správu služby Azure a Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="d59f3-121">Install modules for Azure Resource Manager and Azure Service Management</span></span>  

```
# Install hello Azure Resource Manager modules from hello PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install hello Azure Service Management modules from hello PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   <span data-ttu-id="d59f3-122">Tyto kroky stáhnout a nainstalovat hello moduly nezbytné toocomplete hello zbývající kroky.</span><span class="sxs-lookup"><span data-stu-id="d59f3-122">These steps download and install hello modules necessary toocomplete hello remaining steps.</span></span> <span data-ttu-id="d59f3-123">Stačí to toobe provádí jednou v hello prostředí, kde jsou prováděny hello příkazy prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d59f3-123">This only needs toobe done once in hello environment where you are executing hello PowerShell commands.</span></span>   

* <span data-ttu-id="d59f3-124">Ověření tooAzure</span><span class="sxs-lookup"><span data-stu-id="d59f3-124">Authenticate tooAzure</span></span>  

```
# Authenticate (enter your credentials in hello pop-up window)
Add-AzureRmAccount
```
<span data-ttu-id="d59f3-125">Tento krok musí toobe opakuje pro každou relaci.</span><span class="sxs-lookup"><span data-stu-id="d59f3-125">This step needs toobe repeated for each session.</span></span> <span data-ttu-id="d59f3-126">Po ověření, má být zobrazena informace o vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="d59f3-126">Once authenticated, your subscription information should be displayed.</span></span>

![Účet Azure][1]

<span data-ttu-id="d59f3-128">Teď, když máme tooAzure přístup, můžeme vytvořit skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="d59f3-128">Now that we have access tooAzure, we can create hello resource group.</span></span>

* <span data-ttu-id="d59f3-129">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="d59f3-129">Create a resource group</span></span>

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

<span data-ttu-id="d59f3-130">Ověřte, zda že je správně zřízený příslušné skupině prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="d59f3-130">Verify that hello resource group is correctly provisioned.</span></span> <span data-ttu-id="d59f3-131">**Stav zřizování** by měl být "byla úspěšná."</span><span class="sxs-lookup"><span data-stu-id="d59f3-131">**ProvisioningState** should be “Succeeded.”</span></span>
<span data-ttu-id="d59f3-132">Název skupiny prostředků Hello se používá název účtu úložiště hello toogenerate hello šablony.</span><span class="sxs-lookup"><span data-stu-id="d59f3-132">hello resource group name is used by hello template toogenerate hello storage account name.</span></span> <span data-ttu-id="d59f3-133">název účtu úložiště Hello musí být v rozmezí 3 až 24 znaků a používat jenom číslice a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="d59f3-133">hello storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.</span></span>

![Skupina prostředků][2]

* <span data-ttu-id="d59f3-135">Pomocí nasazení skupiny prostředků hello, nasaďte nový pracovní prostor Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="d59f3-135">Using hello resource group deployment, deploy a new Machine Learning Workspace.</span></span>

```
# Create a Resource Group, TemplateFile is hello location of hello JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

<span data-ttu-id="d59f3-136">Po dokončení nasazení hello je přehledné tooaccess vlastnosti hello pracovního prostoru, které jste nasadili.</span><span class="sxs-lookup"><span data-stu-id="d59f3-136">Once hello deployment is completed, it is straightforward tooaccess properties of hello workspace you deployed.</span></span> <span data-ttu-id="d59f3-137">Například můžete přistupovat hello primární klíč tokenu.</span><span class="sxs-lookup"><span data-stu-id="d59f3-137">For example, you can access hello Primary Key Token.</span></span>

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

<span data-ttu-id="d59f3-138">Jiný způsob tooretrieve tokeny existující pracovního prostoru je toouse hello příkaz Invoke-AzureRmResourceAction.</span><span class="sxs-lookup"><span data-stu-id="d59f3-138">Another way tooretrieve tokens of existing workspace is toouse hello Invoke-AzureRmResourceAction command.</span></span> <span data-ttu-id="d59f3-139">Například můžete vytvořit seznam hello primární a sekundární tokeny všech pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="d59f3-139">For example, you can list hello primary and secondary tokens of all workspaces.</span></span>

```  
# List hello primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
<span data-ttu-id="d59f3-140">Po zřízení hello prostoru můžete také automatizovat celou řadu úloh Azure Machine Learning Studio pomocí hello [modul prostředí PowerShell pro Azure Machine Learning](http://aka.ms/amlps).</span><span class="sxs-lookup"><span data-stu-id="d59f3-140">After hello workspace is provisioned, you can also automate many Azure Machine Learning Studio tasks using hello [PowerShell Module for Azure Machine Learning](http://aka.ms/amlps).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d59f3-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d59f3-141">Next Steps</span></span>
* <span data-ttu-id="d59f3-142">Další informace o [vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="d59f3-142">Learn more about [authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> 
* <span data-ttu-id="d59f3-143">Podívejte se na hello [úložiště šablony Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="d59f3-143">Have a look at hello [Azure Quickstart Templates Repository](https://github.com/Azure/azure-quickstart-templates).</span></span> 
* <span data-ttu-id="d59f3-144">Přehrát toto video o [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39).</span><span class="sxs-lookup"><span data-stu-id="d59f3-144">Watch this video about [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39).</span></span> 

<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
