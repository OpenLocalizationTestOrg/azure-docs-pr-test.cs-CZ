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
# <a name="create-a-custom-image-from-a-vhd-file-using-powershell"></a><span data-ttu-id="c8955-103">Vytvořit vlastní image ze souboru virtuálního pevného disku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c8955-103">Create a custom image from a VHD file using PowerShell</span></span>

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a><span data-ttu-id="c8955-104">Podrobné pokyny</span><span class="sxs-lookup"><span data-stu-id="c8955-104">Step-by-step instructions</span></span>

<span data-ttu-id="c8955-105">Hello následující postup vás provede procesem vytvoření vlastní image ze souboru virtuálního pevného disku pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="c8955-105">hello following steps walk you through creating a custom image from a VHD file using PowerShell:</span></span>

1. <span data-ttu-id="c8955-106">V řádku prostředí PowerShell přihlásit tooyour účet Azure s hello následující volání toohello **Login-AzureRmAccount** rutiny.</span><span class="sxs-lookup"><span data-stu-id="c8955-106">At a PowerShell prompt, log in tooyour Azure account with hello following call toohello **Login-AzureRmAccount** cmdlet.</span></span>  
    
    ```PowerShell
    Login-AzureRmAccount
    ```

1.  <span data-ttu-id="c8955-107">Vyberte hello potřeby předplatného Azure ve volání hello **Select-AzureRmSubscription** rutiny.</span><span class="sxs-lookup"><span data-stu-id="c8955-107">Select hello desired Azure subscription by calling hello **Select-AzureRmSubscription** cmdlet.</span></span> <span data-ttu-id="c8955-108">Nahraďte zástupný symbol pro hello následující hello **$subscriptionId** proměnné s ID platné předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="c8955-108">Replace hello following placeholder for hello **$subscriptionId** variable with a valid Azure subscription ID.</span></span> 

    ```PowerShell
    $subscriptionId = '<Specify your subscription ID here>'
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    ```

1.  <span data-ttu-id="c8955-109">Získat objekt testovacím hello volání hello **Get-AzureRmResource** rutiny.</span><span class="sxs-lookup"><span data-stu-id="c8955-109">Get hello lab object by calling hello **Get-AzureRmResource** cmdlet.</span></span> <span data-ttu-id="c8955-110">Nahraďte zástupné symboly hello následující hello **$labRg** a **$labName** proměnné s hello příslušné hodnoty pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8955-110">Replace hello following placeholders for hello **$labRg** and **$labName** variables with hello appropriate values for your environment.</span></span> 

    ```PowerShell
    $labRg = '<Specify your lab resource group name here>'
    $labName = '<Specify your lab name here>'
    $lab = Get-AzureRmResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    ```
 
1.  <span data-ttu-id="c8955-111">Získáte účet úložiště hello testovacího prostředí a účet úložiště testovacího prostředí hodnoty klíče z objektu hello testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8955-111">Get hello lab storage account and lab storage account key values from hello lab object.</span></span> 

    ```PowerShell
    $labStorageAccount = Get-AzureRmResource -ResourceId $lab.Properties.defaultStorageAccount 
    $labStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value
    ```

1.  <span data-ttu-id="c8955-112">Nahraďte zástupný symbol pro hello následující hello **$vhdUri** proměnné s hello URI tooyour nahrát soubor virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="c8955-112">Replace hello following placeholder for hello **$vhdUri** variable with hello URI tooyour uploaded VHD file.</span></span> <span data-ttu-id="c8955-113">Identifikátor URI souboru virtuálního pevného disku hello můžete získat v okně účtu hello úložiště objektů blob v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c8955-113">You can get hello VHD file's URI from hello storage account's blob blade in hello Azure portal.</span></span>

    ```PowerShell
    $vhdUri = '<Specify hello VHD URI here>'
    ```

1.  <span data-ttu-id="c8955-114">Vytvoření vlastní image hello pomocí hello **New-AzureRmResourceGroupDeployment** rutiny.</span><span class="sxs-lookup"><span data-stu-id="c8955-114">Create hello custom image using hello **New-AzureRmResourceGroupDeployment** cmdlet.</span></span> <span data-ttu-id="c8955-115">Nahraďte zástupné symboly hello následující hello **$customImageName** a **$customImageDescription** toomeaningful názvy proměnných pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8955-115">Replace hello following placeholders for hello **$customImageName** and **$customImageDescription** variables toomeaningful names for your environment.</span></span>

    ```PowerShell
    $customImageName = '<Specify hello custom image name>'
    $customImageDescription = '<Specify hello custom image description>'

    $parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

    New-AzureRmResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
    ```

## <a name="powershell-script-toocreate-a-custom-image-from-a-vhd-file"></a><span data-ttu-id="c8955-116">Toocreate skript prostředí PowerShell vlastní image ze souboru virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="c8955-116">PowerShell script toocreate a custom image from a VHD file</span></span>

<span data-ttu-id="c8955-117">Následující skript prostředí PowerShell Hello lze použít toocreate vlastní image ze souboru virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="c8955-117">hello following PowerShell script can be used toocreate a custom image from a VHD file.</span></span> <span data-ttu-id="c8955-118">Nahraďte zástupné symboly hello (počáteční a koncovou s lomené závorky) hello odpovídající hodnoty pro své potřeby.</span><span class="sxs-lookup"><span data-stu-id="c8955-118">Replace hello placeholders (starting and ending with angle brackets) with hello appropriate values for your needs.</span></span> 

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

## <a name="related-blog-posts"></a><span data-ttu-id="c8955-119">Příspěvky blogu související</span><span class="sxs-lookup"><span data-stu-id="c8955-119">Related blog posts</span></span>

- [<span data-ttu-id="c8955-120">Vlastní Image nebo vzorce?</span><span class="sxs-lookup"><span data-stu-id="c8955-120">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [<span data-ttu-id="c8955-121">Kopírování vlastních bitových kopií mezi Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="c8955-121">Copying Custom Images between Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a><span data-ttu-id="c8955-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c8955-122">Next steps</span></span>

- [<span data-ttu-id="c8955-123">Přidání testovacího prostředí tooyour virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c8955-123">Add a VM tooyour lab</span></span>](./devtest-lab-add-vm-with-artifacts.md)
