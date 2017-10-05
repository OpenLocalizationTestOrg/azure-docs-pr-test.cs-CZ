---
title: "Vytvoření vlastní image Azure DevTest Labs ze souboru virtuálního pevného disku pomocí prostředí PowerShell | Microsoft Docs"
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
ms.openlocfilehash: a4729f70aae80a13233fbe96a5d8a56c0c9d01d3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-image-from-a-vhd-file-using-powershell"></a><span data-ttu-id="fdc5b-103">Vytvořit vlastní image ze souboru virtuálního pevného disku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="fdc5b-103">Create a custom image from a VHD file using PowerShell</span></span>

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a><span data-ttu-id="fdc5b-104">Podrobné pokyny</span><span class="sxs-lookup"><span data-stu-id="fdc5b-104">Step-by-step instructions</span></span>

<span data-ttu-id="fdc5b-105">Následující postup vás provede procesem vytvoření vlastní image ze souboru virtuálního pevného disku pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="fdc5b-105">The following steps walk you through creating a custom image from a VHD file using PowerShell:</span></span>

1. <span data-ttu-id="fdc5b-106">Na příkazovém řádku prostředí PowerShell, přihlaste se k účtu Azure s následující volání **Login-AzureRmAccount** rutiny.</span><span class="sxs-lookup"><span data-stu-id="fdc5b-106">At a PowerShell prompt, log in to your Azure account with the following call to the **Login-AzureRmAccount** cmdlet.</span></span>  
    
    ```PowerShell
    Login-AzureRmAccount
    ```

1.  <span data-ttu-id="fdc5b-107">Vyberte požadované předplatné Azure voláním **Select-AzureRmSubscription** rutiny.</span><span class="sxs-lookup"><span data-stu-id="fdc5b-107">Select the desired Azure subscription by calling the **Select-AzureRmSubscription** cmdlet.</span></span> <span data-ttu-id="fdc5b-108">Nahraďte následující zástupný symbol pro **$subscriptionId** proměnné s ID platné předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="fdc5b-108">Replace the following placeholder for the **$subscriptionId** variable with a valid Azure subscription ID.</span></span> 

    ```PowerShell
    $subscriptionId = '<Specify your subscription ID here>'
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    ```

1.  <span data-ttu-id="fdc5b-109">Získání testovacího prostředí objektu voláním **Get-AzureRmResource** rutiny.</span><span class="sxs-lookup"><span data-stu-id="fdc5b-109">Get the lab object by calling the **Get-AzureRmResource** cmdlet.</span></span> <span data-ttu-id="fdc5b-110">Nahraďte následující zástupné symboly **$labRg** a **$labName** proměnné s příslušnými hodnotami pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="fdc5b-110">Replace the following placeholders for the **$labRg** and **$labName** variables with the appropriate values for your environment.</span></span> 

    ```PowerShell
    $labRg = '<Specify your lab resource group name here>'
    $labName = '<Specify your lab name here>'
    $lab = Get-AzureRmResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    ```
 
1.  <span data-ttu-id="fdc5b-111">Získáte testovací prostředí úložiště účet a testovacího prostředí úložiště účet klíče hodnoty z objektu testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="fdc5b-111">Get the lab storage account and lab storage account key values from the lab object.</span></span> 

    ```PowerShell
    $labStorageAccount = Get-AzureRmResource -ResourceId $lab.Properties.defaultStorageAccount 
    $labStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value
    ```

1.  <span data-ttu-id="fdc5b-112">Nahraďte následující zástupný symbol pro **$vhdUri** proměnné s identifikátor URI pro nahraný soubor virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="fdc5b-112">Replace the following placeholder for the **$vhdUri** variable with the URI to your uploaded VHD file.</span></span> <span data-ttu-id="fdc5b-113">Identifikátor URI virtuálního pevného disku soubor můžete získat v okně účtu úložiště objektů blob na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fdc5b-113">You can get the VHD file's URI from the storage account's blob blade in the Azure portal.</span></span>

    ```PowerShell
    $vhdUri = '<Specify the VHD URI here>'
    ```

1.  <span data-ttu-id="fdc5b-114">Vytvoření vlastní image pomocí **New-AzureRmResourceGroupDeployment** rutiny.</span><span class="sxs-lookup"><span data-stu-id="fdc5b-114">Create the custom image using the **New-AzureRmResourceGroupDeployment** cmdlet.</span></span> <span data-ttu-id="fdc5b-115">Nahraďte následující zástupné symboly **$customImageName** a **$customImageDescription** proměnné na smysluplný názvy pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="fdc5b-115">Replace the following placeholders for the **$customImageName** and **$customImageDescription** variables to meaningful names for your environment.</span></span>

    ```PowerShell
    $customImageName = '<Specify the custom image name>'
    $customImageDescription = '<Specify the custom image description>'

    $parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

    New-AzureRmResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
    ```

## <a name="powershell-script-to-create-a-custom-image-from-a-vhd-file"></a><span data-ttu-id="fdc5b-116">Skript prostředí PowerShell pro vytvoření vlastní image ze souboru virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="fdc5b-116">PowerShell script to create a custom image from a VHD file</span></span>

<span data-ttu-id="fdc5b-117">Následující skript prostředí PowerShell můžete použít k vytvoření vlastní image z soubor virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="fdc5b-117">The following PowerShell script can be used to create a custom image from a VHD file.</span></span> <span data-ttu-id="fdc5b-118">Nahraďte zástupné symboly (počáteční a koncovou s lomené závorky) na odpovídající hodnoty pro své potřeby.</span><span class="sxs-lookup"><span data-stu-id="fdc5b-118">Replace the placeholders (starting and ending with angle brackets) with the appropriate values for your needs.</span></span> 

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

## <a name="related-blog-posts"></a><span data-ttu-id="fdc5b-119">Příspěvky blogu související</span><span class="sxs-lookup"><span data-stu-id="fdc5b-119">Related blog posts</span></span>

- [<span data-ttu-id="fdc5b-120">Vlastní Image nebo vzorce?</span><span class="sxs-lookup"><span data-stu-id="fdc5b-120">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [<span data-ttu-id="fdc5b-121">Kopírování vlastních bitových kopií mezi Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="fdc5b-121">Copying Custom Images between Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a><span data-ttu-id="fdc5b-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fdc5b-122">Next steps</span></span>

- [<span data-ttu-id="fdc5b-123">Přidejte virtuální počítač do testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="fdc5b-123">Add a VM to your lab</span></span>](./devtest-lab-add-vm-with-artifacts.md)
