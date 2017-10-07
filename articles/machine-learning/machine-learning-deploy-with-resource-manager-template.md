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
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a>Nasazení pracovního prostoru Machine Learning pomocí Azure Resource Manageru
## <a name="introduction"></a>Úvod
Pomocí Azure Resource Manager uloží šablonu nasazení, je čas tak, že jste toodeploy škálovatelné způsob vzájemně propojeny součásti mechanismus ověřování a zkuste to znovu. tooset si Azure Machine Learning pracovní prostory, například musíte toofirst nakonfigurujte účet úložiště Azure a pak nasaďte pracovního prostoru. Představte si to ručně ve velkém množství pracovních prostorů. Jednodušší alternativou je toouse toodeploy šablony Azure Resource Manager pracovní prostor služby Azure Machine Learning a všechny jeho závislé součásti. Tento článek vás provede krok za krokem tohoto procesu. Skvělý Přehled služby Správce prostředků Azure, najdete v části [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Postup: vytvoření pracovního prostoru Machine Learning
Jsme se vytvořit skupinu prostředků Azure a pak nasaďte nový účet úložiště Azure a nový Azure Machine Learning pracovní prostor pomocí šablony Resource Manageru. Po dokončení nasazení hello jsme se vytiskněte důležité informace o pracovních prostorech hello, které byly vytvořeny (hello primární klíč, hello workspaceID a hello URL toohello prostoru).

### <a name="create-an-azure-resource-manager-template"></a>Vytvořit šablonu Azure Resource Manager
Pracovní prostor Machine Learning vyžaduje tooit úložiště Azure účet toostore hello propojená datová sada.
Hello následující šablona používá název hello hello název skupiny prostředků toogenerate hello úložiště účet a název pracovního prostoru hello.  Také používá název účtu úložiště hello jako vlastnost při vytváření pracovního prostoru hello.

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
Uložte jako soubor mlworkspace.json pod c:\temp\ této šablony.

### <a name="deploy-hello-resource-group-based-on-hello-template"></a>Nasazení hello skupinu prostředků, na základě šablony hello
* Otevřete prostředí PowerShell
* Instalace modulů pro správu služby Azure a Azure Resource Manager  

```
# Install hello Azure Resource Manager modules from hello PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install hello Azure Service Management modules from hello PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Tyto kroky stáhnout a nainstalovat hello moduly nezbytné toocomplete hello zbývající kroky. Stačí to toobe provádí jednou v hello prostředí, kde jsou prováděny hello příkazy prostředí PowerShell.   

* Ověření tooAzure  

```
# Authenticate (enter your credentials in hello pop-up window)
Add-AzureRmAccount
```
Tento krok musí toobe opakuje pro každou relaci. Po ověření, má být zobrazena informace o vašem předplatném.

![Účet Azure][1]

Teď, když máme tooAzure přístup, můžeme vytvořit skupiny prostředků hello.

* Vytvoření skupiny prostředků

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Ověřte, zda že je správně zřízený příslušné skupině prostředků hello. **Stav zřizování** by měl být "byla úspěšná."
Název skupiny prostředků Hello se používá název účtu úložiště hello toogenerate hello šablony. název účtu úložiště Hello musí být v rozmezí 3 až 24 znaků a používat jenom číslice a malá písmena.

![Skupina prostředků][2]

* Pomocí nasazení skupiny prostředků hello, nasaďte nový pracovní prostor Machine Learning.

```
# Create a Resource Group, TemplateFile is hello location of hello JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Po dokončení nasazení hello je přehledné tooaccess vlastnosti hello pracovního prostoru, které jste nasadili. Například můžete přistupovat hello primární klíč tokenu.

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Jiný způsob tooretrieve tokeny existující pracovního prostoru je toouse hello příkaz Invoke-AzureRmResourceAction. Například můžete vytvořit seznam hello primární a sekundární tokeny všech pracovních prostorů.

```  
# List hello primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
Po zřízení hello prostoru můžete také automatizovat celou řadu úloh Azure Machine Learning Studio pomocí hello [modul prostředí PowerShell pro Azure Machine Learning](http://aka.ms/amlps).

## <a name="next-steps"></a>Další kroky
* Další informace o [vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md). 
* Podívejte se na hello [úložiště šablony Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates). 
* Přehrát toto video o [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39). 

<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
