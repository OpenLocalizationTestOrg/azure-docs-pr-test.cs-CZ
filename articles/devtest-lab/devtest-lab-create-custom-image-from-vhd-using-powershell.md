---
title: "aaaCreate Azure DevTest Labs vlastní obrázek ze souboru virtuálního pevného disku pomocí prostředí PowerShell | Microsoft Docs"
description: "Automatizovat vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí prostředí PowerShell"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 39b4005fa46cdf86cf0800ca376128134bcfb650
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-from-a-vhd-file-using-powershell"></a>Vytvořit vlastní image ze souboru virtuálního pevného disku pomocí prostředí PowerShell

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Podrobné pokyny

Hello následující postup vás provede procesem vytvoření vlastní image ze souboru virtuálního pevného disku pomocí prostředí PowerShell:

1. V řádku prostředí PowerShell přihlásit tooyour účet Azure s hello následující volání toohello **Login-AzureRmAccount** rutiny.  
    
    ```PowerShell
    Login-AzureRmAccount
    ```

1.  Vyberte hello potřeby předplatného Azure ve volání hello **Select-AzureRmSubscription** rutiny. Nahraďte zástupný symbol pro hello následující hello **$subscriptionId** proměnné s ID platné předplatné Azure. 

    ```PowerShell
    $subscriptionId = '<Specify your subscription ID here>'
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    ```

1.  Získat objekt testovacím hello volání hello **Get-AzureRmResource** rutiny. Nahraďte zástupné symboly hello následující hello **$labRg** a **$labName** proměnné s hello příslušné hodnoty pro vaše prostředí. 

    ```PowerShell
    $labRg = '<Specify your lab resource group name here>'
    $labName = '<Specify your lab name here>'
    $lab = Get-AzureRmResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    ```
 
1.  Získáte účet úložiště hello testovacího prostředí a účet úložiště testovacího prostředí hodnoty klíče z objektu hello testovacího prostředí. 

    ```PowerShell
    $labStorageAccount = Get-AzureRmResource -ResourceId $lab.Properties.defaultStorageAccount 
    $labStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value
    ```

1.  Nahraďte zástupný symbol pro hello následující hello **$vhdUri** proměnné s hello URI tooyour nahrát soubor virtuálního pevného disku. Identifikátor URI souboru virtuálního pevného disku hello můžete získat v okně účtu hello úložiště objektů blob v hello portálu Azure.

    ```PowerShell
    $vhdUri = '<Specify hello VHD URI here>'
    ```

1.  Vytvoření vlastní image hello pomocí hello **New-AzureRmResourceGroupDeployment** rutiny. Nahraďte zástupné symboly hello následující hello **$customImageName** a **$customImageDescription** toomeaningful názvy proměnných pro vaše prostředí.

    ```PowerShell
    $customImageName = '<Specify hello custom image name>'
    $customImageDescription = '<Specify hello custom image description>'

    $parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

    New-AzureRmResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
    ```

## <a name="powershell-script-toocreate-a-custom-image-from-a-vhd-file"></a>Toocreate skript prostředí PowerShell vlastní image ze souboru virtuálního pevného disku

Následující skript prostředí PowerShell Hello lze použít toocreate vlastní image ze souboru virtuálního pevného disku. Nahraďte zástupné symboly hello (počáteční a koncovou s lomené závorky) hello odpovídající hodnoty pro své potřeby. 

```PowerShell
# Log in tooyour Azure account.  
Login-AzureRmAccount

# Select hello desired Azure subscription. 
$subscriptionId = '<Specify your subscription ID here>'
Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Get hello lab object.
$labRg = '<Specify your lab resource group name here>'
$labName = '<Specify your lab name here>'
$lab = Get-AzureRmResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)

# Get hello lab storage account and lab storage account key values.
$labStorageAccount = Get-AzureRmResource -ResourceId $lab.Properties.defaultStorageAccount 
$labStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value

# Set hello URI of hello VHD file.  
$vhdUri = '<Specify hello VHD URI here>'

# Set hello custom image name and description values.
$customImageName = '<Specify hello custom image name>'
$customImageDescription = '<Specify hello custom image description>'

# Set up hello parameters object.
$parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

# Create hello custom image. 
New-AzureRmResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
```

## <a name="related-blog-posts"></a>Příspěvky blogu související

- [Vlastní Image nebo vzorce?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopírování vlastních bitových kopií mezi Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Další kroky

- [Přidání testovacího prostředí tooyour virtuálních počítačů](./devtest-lab-add-vm-with-artifacts.md)
