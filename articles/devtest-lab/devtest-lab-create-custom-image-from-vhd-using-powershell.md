---
title: "Vytvoření vlastní image Azure DevTest Labs ze souboru virtuálního pevného disku pomocí prostředí PowerShell | Microsoft Docs"
description: "Automatizovat vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí prostředí PowerShell"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: v-craic
ms.openlocfilehash: 0810a67c28224170ccde7e2c347f8704896e69ad
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/02/2018
---
# <a name="create-a-custom-image-from-a-vhd-file-using-powershell"></a>Vytvořit vlastní image ze souboru virtuálního pevného disku pomocí prostředí PowerShell

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Podrobné pokyny

Následující postup vás provede procesem vytvoření vlastní image ze souboru virtuálního pevného disku pomocí prostředí PowerShell:

1. Na příkazovém řádku prostředí PowerShell, přihlaste se k účtu Azure s následující volání **Login-AzureRmAccount** rutiny.  
    
    ```PowerShell
    Login-AzureRmAccount
    ```

1.  Vyberte požadované předplatné Azure voláním **Select-AzureRmSubscription** rutiny. Nahraďte následující zástupný symbol pro **$subscriptionId** proměnné s ID platné předplatné Azure. 

    ```PowerShell
    $subscriptionId = '<Specify your subscription ID here>'
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    ```

1.  Získání testovacího prostředí objektu voláním **Get-AzureRmResource** rutiny. Nahraďte následující zástupné symboly **$labRg** a **$labName** proměnné s příslušnými hodnotami pro vaše prostředí. 

    ```PowerShell
    $labRg = '<Specify your lab resource group name here>'
    $labName = '<Specify your lab name here>'
    $lab = Get-AzureRmResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    ```
 
1.  Získáte testovací prostředí úložiště účet a testovacího prostředí úložiště účet klíče hodnoty z objektu testovacího prostředí. 

    ```PowerShell
    $labStorageAccount = Get-AzureRmResource -ResourceId $lab.Properties.defaultStorageAccount 
    $labStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value
    ```

1.  Nahraďte následující zástupný symbol pro **$vhdUri** proměnné s identifikátor URI pro nahraný soubor virtuálního pevného disku. Identifikátor URI virtuálního pevného disku soubor můžete získat v okně účtu úložiště objektů blob na portálu Azure.

    ```PowerShell
    $vhdUri = '<Specify the VHD URI here>'
    ```

1.  Vytvoření vlastní image pomocí **New-AzureRmResourceGroupDeployment** rutiny. Nahraďte následující zástupné symboly **$customImageName** a **$customImageDescription** proměnné na smysluplný názvy pro vaše prostředí.

    ```PowerShell
    $customImageName = '<Specify the custom image name>'
    $customImageDescription = '<Specify the custom image description>'

    $parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

    New-AzureRmResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
    ```

## <a name="powershell-script-to-create-a-custom-image-from-a-vhd-file"></a>Skript prostředí PowerShell pro vytvoření vlastní image ze souboru virtuálního pevného disku

Následující skript prostředí PowerShell můžete použít k vytvoření vlastní image z soubor virtuálního pevného disku. Nahraďte zástupné symboly (počáteční a koncovou s lomené závorky) na odpovídající hodnoty pro své potřeby. 

```PowerShell
# Log in to your Azure account.  
Login-AzureRmAccount

# Select the desired Azure subscription. 
$subscriptionId = '<Specify your subscription ID here>'
Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Get the lab object.
$labRg = '<Specify your lab resource group name here>'
$labName = '<Specify your lab name here>'
$lab = Get-AzureRmResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)

# Get the lab storage account and lab storage account key values.
$labStorageAccount = Get-AzureRmResource -ResourceId $lab.Properties.defaultStorageAccount 
$labStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value

# Set the URI of the VHD file.  
$vhdUri = '<Specify the VHD URI here>'

# Set the custom image name and description values.
$customImageName = '<Specify the custom image name>'
$customImageDescription = '<Specify the custom image description>'

# Set up the parameters object.
$parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

# Create the custom image. 
New-AzureRmResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
```

## <a name="related-blog-posts"></a>Příspěvky blogu související

- [Vlastní Image nebo vzorce?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopírování vlastních bitových kopií mezi Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Další postup

- [Přidejte virtuální počítač do testovacího prostředí](devtest-lab-add-vm.md)
