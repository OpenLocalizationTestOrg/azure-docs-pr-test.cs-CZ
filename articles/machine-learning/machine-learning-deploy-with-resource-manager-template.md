---
title: "Nasazení pracovní prostor Machine Learning s Azure Resource Manager | Microsoft Docs"
description: "Postup nasazení pracovního prostoru pro Azure Machine Learning pomocí šablony Azure Resource Manageru"
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
ms.openlocfilehash: 9e37780428b0867da63987ec4f7f843a8abeb907
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a><span data-ttu-id="2965e-103">Nasazení pracovního prostoru Machine Learning pomocí Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="2965e-103">Deploy Machine Learning Workspace Using Azure Resource Manager</span></span>
## <a name="introduction"></a><span data-ttu-id="2965e-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="2965e-104">Introduction</span></span>
<span data-ttu-id="2965e-105">Pomocí Azure Resource Manager šablonu nasazení šetří čas je tím, že jste škálovatelné způsob, jak nasadit vzájemně propojena k součástem s ověřování a opakujte mechanismus.</span><span class="sxs-lookup"><span data-stu-id="2965e-105">Using an Azure Resource Manager deployment template saves you time by giving you a scalable way to deploy interconnected components with a validation and retry mechanism.</span></span> <span data-ttu-id="2965e-106">Abyste nastavili Azure Machine Learning pracovní prostory, například musíte nejdřív nakonfigurovat účet úložiště Azure a pak nasadit pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="2965e-106">To set up Azure Machine Learning Workspaces, for example, you need to first configure an Azure storage account and then deploy your workspace.</span></span> <span data-ttu-id="2965e-107">Představte si to ručně ve velkém množství pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="2965e-107">Imagine doing this manually for hundreds of workspaces.</span></span> <span data-ttu-id="2965e-108">Jednodušší alternativou je pomocí šablony Azure Resource Manager k nasazení pracovní prostor služby Azure Machine Learning a všechny jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="2965e-108">An easier alternative is to use an Azure Resource Manager template to deploy an Azure Machine Learning Workspace and all its dependencies.</span></span> <span data-ttu-id="2965e-109">Tento článek vás provede krok za krokem tohoto procesu.</span><span class="sxs-lookup"><span data-stu-id="2965e-109">This article takes you through this process step-by-step.</span></span> <span data-ttu-id="2965e-110">Skvělý Přehled služby Správce prostředků Azure, najdete v části [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2965e-110">For a great overview of Azure Resource Manager, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="step-by-step-create-a-machine-learning-workspace"></a><span data-ttu-id="2965e-111">Postup: vytvoření pracovního prostoru Machine Learning</span><span class="sxs-lookup"><span data-stu-id="2965e-111">Step-by-step: create a Machine Learning Workspace</span></span>
<span data-ttu-id="2965e-112">Jsme se vytvořit skupinu prostředků Azure a pak nasaďte nový účet úložiště Azure a nový Azure Machine Learning pracovní prostor pomocí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="2965e-112">We will create an Azure resource group, then deploy a new Azure storage account and a new Azure Machine Learning Workspace using a Resource Manager template.</span></span> <span data-ttu-id="2965e-113">Po dokončení nasazení jsme se vytiskněte důležité informace o pracovních prostorech, které byly vytvořeny (primární klíč, ID pracovního prostoru a adresu URL do pracovního prostoru).</span><span class="sxs-lookup"><span data-stu-id="2965e-113">Once the deployment is complete, we will print out important information about the workspaces that were created (the primary key, the workspaceID, and the URL to the workspace).</span></span>

### <a name="create-an-azure-resource-manager-template"></a><span data-ttu-id="2965e-114">Vytvořit šablonu Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="2965e-114">Create an Azure Resource Manager template</span></span>
<span data-ttu-id="2965e-115">Pracovní prostor Machine Learning vyžaduje účet úložiště Azure pro uložení datové sady na sebe napojenou.</span><span class="sxs-lookup"><span data-stu-id="2965e-115">A Machine Learning Workspace requires an Azure storage account to store the dataset linked to it.</span></span>
<span data-ttu-id="2965e-116">Následující šablona používá název skupiny prostředků můžete vygenerovat název účtu úložiště a název pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="2965e-116">The following template uses the name of the resource group to generate the storage account name and the workspace name.</span></span>  <span data-ttu-id="2965e-117">Také používá název účtu úložiště jako vlastnost při vytváření pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="2965e-117">It also uses the storage account name as a property when creating the workspace.</span></span>

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
<span data-ttu-id="2965e-118">Uložte jako soubor mlworkspace.json pod c:\temp\ této šablony.</span><span class="sxs-lookup"><span data-stu-id="2965e-118">Save this template as mlworkspace.json file under c:\temp\.</span></span>

### <a name="deploy-the-resource-group-based-on-the-template"></a><span data-ttu-id="2965e-119">Nasazení skupiny prostředků, na základě šablony</span><span class="sxs-lookup"><span data-stu-id="2965e-119">Deploy the resource group, based on the template</span></span>
* <span data-ttu-id="2965e-120">Otevřete prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2965e-120">Open PowerShell</span></span>
* <span data-ttu-id="2965e-121">Instalace modulů pro správu služby Azure a Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="2965e-121">Install modules for Azure Resource Manager and Azure Service Management</span></span>  

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   <span data-ttu-id="2965e-122">Tyto kroky stáhnout a nainstalovat moduly, které jsou potřebné k dokončení zbývající kroky.</span><span class="sxs-lookup"><span data-stu-id="2965e-122">These steps download and install the modules necessary to complete the remaining steps.</span></span> <span data-ttu-id="2965e-123">Stačí, když to se provádí jednou v prostředí, kde jsou prováděny příkazy prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2965e-123">This only needs to be done once in the environment where you are executing the PowerShell commands.</span></span>   

* <span data-ttu-id="2965e-124">Ověřování v Azure</span><span class="sxs-lookup"><span data-stu-id="2965e-124">Authenticate to Azure</span></span>  

```
# Authenticate (enter your credentials in the pop-up window)
Add-AzureRmAccount
```
<span data-ttu-id="2965e-125">Tento krok je potřeba zopakovat pro každou relaci.</span><span class="sxs-lookup"><span data-stu-id="2965e-125">This step needs to be repeated for each session.</span></span> <span data-ttu-id="2965e-126">Po ověření, má být zobrazena informace o vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="2965e-126">Once authenticated, your subscription information should be displayed.</span></span>

![Účet Azure][1]

<span data-ttu-id="2965e-128">Teď, když máme přístup k Azure, můžeme vytvořit skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="2965e-128">Now that we have access to Azure, we can create the resource group.</span></span>

* <span data-ttu-id="2965e-129">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="2965e-129">Create a resource group</span></span>

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

<span data-ttu-id="2965e-130">Ověřte, že je správně zřízený skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="2965e-130">Verify that the resource group is correctly provisioned.</span></span> <span data-ttu-id="2965e-131">**Stav zřizování** by měl být "byla úspěšná."</span><span class="sxs-lookup"><span data-stu-id="2965e-131">**ProvisioningState** should be “Succeeded.”</span></span>
<span data-ttu-id="2965e-132">Název skupiny prostředků se šablonou používá ke generování název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2965e-132">The resource group name is used by the template to generate the storage account name.</span></span> <span data-ttu-id="2965e-133">Název účtu úložiště musí být v rozmezí 3 až 24 znaků a používat jenom číslice a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="2965e-133">The storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.</span></span>

![Skupina prostředků][2]

* <span data-ttu-id="2965e-135">Pomocí nasazení skupiny prostředků, nasaďte nový pracovní prostor Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="2965e-135">Using the resource group deployment, deploy a new Machine Learning Workspace.</span></span>

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

<span data-ttu-id="2965e-136">Po dokončení nasazení je snadný přístup k vlastnosti pracovního prostoru, které jste nasadili.</span><span class="sxs-lookup"><span data-stu-id="2965e-136">Once the deployment is completed, it is straightforward to access properties of the workspace you deployed.</span></span> <span data-ttu-id="2965e-137">Například můžete přistupovat tokenu primární klíč.</span><span class="sxs-lookup"><span data-stu-id="2965e-137">For example, you can access the Primary Key Token.</span></span>

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

<span data-ttu-id="2965e-138">Použití příkazu Invoke-AzureRmResourceAction je jiný způsob, jak načíst tokeny existujícímu pracovnímu prostoru.</span><span class="sxs-lookup"><span data-stu-id="2965e-138">Another way to retrieve tokens of existing workspace is to use the Invoke-AzureRmResourceAction command.</span></span> <span data-ttu-id="2965e-139">Například můžete vytvořit seznam tokenů primární a sekundární všech pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="2965e-139">For example, you can list the primary and secondary tokens of all workspaces.</span></span>

```  
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
<span data-ttu-id="2965e-140">Po zřízení pracovním prostoru můžete také automatizovat celou řadu úloh Azure Machine Learning Studio pomocí [modul prostředí PowerShell pro Azure Machine Learning](http://aka.ms/amlps).</span><span class="sxs-lookup"><span data-stu-id="2965e-140">After the workspace is provisioned, you can also automate many Azure Machine Learning Studio tasks using the [PowerShell Module for Azure Machine Learning](http://aka.ms/amlps).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2965e-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2965e-141">Next Steps</span></span>
* <span data-ttu-id="2965e-142">Další informace o [vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="2965e-142">Learn more about [authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> 
* <span data-ttu-id="2965e-143">Podívejte se na [úložiště šablony Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="2965e-143">Have a look at the [Azure Quickstart Templates Repository](https://github.com/Azure/azure-quickstart-templates).</span></span> 
* <span data-ttu-id="2965e-144">Přehrát toto video o [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39).</span><span class="sxs-lookup"><span data-stu-id="2965e-144">Watch this video about [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39).</span></span> 

<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
