---
title: "Vytvořit nebo upravit labs automaticky pomocí šablon Azure Resource Manageru pomocí prostředí PowerShell | Microsoft Docs"
description: "Další informace o použití šablon Azure Resource Manageru pomocí prostředí PowerShell k vytvoření nebo úpravě automaticky v testovacím prostředí DevTest labs"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: dad9944c-0b20-48be-ba80-8f4aa0950903
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: tarcher
ms.openlocfilehash: cea4531175df2cc39790497dc049d27e23ffa0c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-or-modify-labs-automatically-using-azure-resource-manager-templates-and-powershell"></a><span data-ttu-id="2f445-103">Vytvořit nebo upravit labs automaticky pomocí šablon Azure Resource Manageru a prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2f445-103">Create or modify labs automatically using Azure Resource Manager templates and PowerShell</span></span>

<span data-ttu-id="2f445-104">DevTest Labs poskytuje mnoho šablon Azure Resource Manageru a skriptů prostředí PowerShell, které můžete vám pomohou rychle a automaticky vytvořit nové labs nebo upravit existující labs a pak nasadit tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="2f445-104">DevTest Labs provides many Azure Resource Manager templates and PowerShell scripts that can help you quickly and automatically create new labs or modify existing labs and then deploy these resources.</span></span>

<span data-ttu-id="2f445-105">Tento článek pomůže vás provede procesem pomocí těchto šablon a skripty pro automatizaci vytváření, úpravu a nasazení vaší laboratoře.</span><span class="sxs-lookup"><span data-stu-id="2f445-105">This article helps guide you through the process of using these templates and scripts to automate the creation, modification, and deployment of your labs.</span></span> <span data-ttu-id="2f445-106">Tento článek také ukazuje, kde můžete najít další informace o tom, jak pomocí prostředí PowerShell v DevTest Labs provádět některé běžné úlohy.</span><span class="sxs-lookup"><span data-stu-id="2f445-106">This article also shows you where you can find more information about how to use PowerShell to perform some common tasks in DevTest Labs.</span></span>

## <a name="step-1-gather-your-templates-and-scripts"></a><span data-ttu-id="2f445-107">Krok 1: Shromáždění šablony a skripty</span><span class="sxs-lookup"><span data-stu-id="2f445-107">Step 1: Gather your templates and scripts</span></span>
<span data-ttu-id="2f445-108">Můžete najít předem provedené [šablon Azure Resource Manageru](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) a [skriptů prostředí PowerShell](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) v našeho veřejného [úložiště Github](https://github.com/Azure/azure-devtestlab).</span><span class="sxs-lookup"><span data-stu-id="2f445-108">You can find pre-made [Azure Resource Manager templates](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) and [PowerShell scripts](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) at our public [Github repository](https://github.com/Azure/azure-devtestlab).</span></span> <span data-ttu-id="2f445-109">Použijte je jako-je, nebo si je přizpůsobit pro vaše potřeby a uložit je do vlastní [privátní úložiště Git](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="2f445-109">Use them as-is, or customize them for your needs and store them in your own [private Git repo](devtest-lab-add-artifact-repo.md).</span></span> 

## <a name="step-2-modify-your-azure-resource-manager-template"></a><span data-ttu-id="2f445-110">Krok 2: Upravte šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="2f445-110">Step 2: Modify your Azure Resource Manager template</span></span>
<span data-ttu-id="2f445-111">Můžete provést kroky v [vytvoření vaší první šablony Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-create-first-template) Pokud jste vytvořili šablonu před nikdy.</span><span class="sxs-lookup"><span data-stu-id="2f445-111">You can follow the steps at [Create your first Azure Resource Manager template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-create-first-template) if you have never created a template before.</span></span>

<span data-ttu-id="2f445-112">Kromě toho [osvědčené postupy pro vytváření šablon Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) nabízí mnoho pokyny a doporučení k vytváření šablon Azure Resource Manageru, která jsou spolehlivé a snadno se používá.</span><span class="sxs-lookup"><span data-stu-id="2f445-112">In addition, [Best practices for creating Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) offers many guidelines and suggestions to help you create Azure Resource Manager templates that are reliable and easy to use.</span></span> <span data-ttu-id="2f445-113">Obvykle budete používat jeden z přístupů nebo ukázky poskytnuté varianta a upravovat šablony podle svých potřeb.</span><span class="sxs-lookup"><span data-stu-id="2f445-113">Typically, you will use a variation of one of the approaches or examples provided and modify your template for your needs.</span></span>

## <a name="step-3-deploy-resources-with-powershell"></a><span data-ttu-id="2f445-114">Krok 3: Nasazení prostředků v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2f445-114">Step 3: Deploy resources with PowerShell</span></span>
<span data-ttu-id="2f445-115">Po přizpůsobení šablony a skripty, postupujte podle kroků nutné [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="2f445-115">After you have customized your templates and scripts, follow the steps necessary to [deploy resources with Resource Manager templates and Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span></span> <span data-ttu-id="2f445-116">Tento článek poskytuje obecné informace o použití prostředí Azure PowerShell k nasazení vašich prostředků Azure pomocí šablon Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="2f445-116">The article provides general information about using Azure PowerShell with Azure Resource Manager templates to deploy your resources to Azure.</span></span>


## <a name="common-tasks-you-can-perform-in-devtest-labs-using-powershell"></a><span data-ttu-id="2f445-117">Běžné úlohy, které můžete provádět v DevTest labs pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2f445-117">Common tasks you can perform in DevTest labs using PowerShell</span></span>
<span data-ttu-id="2f445-118">Existuje mnoho dalších běžných úloh můžete automatizovat pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2f445-118">There are many other common tasks that you can automate by using PowerShell.</span></span> <span data-ttu-id="2f445-119">Dokumentace následující oddíly popisují kroky potřebné k provedení těchto úloh.</span><span class="sxs-lookup"><span data-stu-id="2f445-119">The following sections of the documentation outline the steps required to perform these tasks.</span></span>

* [<span data-ttu-id="2f445-120">Vytvořit vlastní image ze souboru virtuálního pevného disku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2f445-120">Create a custom image from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
* [<span data-ttu-id="2f445-121">Nahrání souboru virtuálního pevného disku do testovacího prostředí na účet úložiště pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2f445-121">Upload VHD file to lab's storage account using PowerShell</span></span>](devtest-lab-upload-vhd-using-powershell.md)
* [<span data-ttu-id="2f445-122">Přidat externího uživatele k testovacím prostředí pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2f445-122">Add an external user to a lab using PowerShell</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell)
* [<span data-ttu-id="2f445-123">Vytvoření testovacího prostředí vlastní role pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2f445-123">Create a lab custom role using PowerShell</span></span>](devtest-lab-grant-user-permissions-to-specific-lab-policies.md#creating-a-lab-custom-role-using-powershell)

### <a name="next-steps"></a><span data-ttu-id="2f445-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2f445-124">Next steps</span></span>
* <span data-ttu-id="2f445-125">Naučte se vytvářet [privátní úložiště Git](devtest-lab-add-artifact-repo.md) kde budete ukládat vlastní šablony nebo skripty.</span><span class="sxs-lookup"><span data-stu-id="2f445-125">Learn how to create a [private Git repository](devtest-lab-add-artifact-repo.md) where you will store your customized templates or scripts.</span></span>
* <span data-ttu-id="2f445-126">Prozkoumejte [šablon Azure Resource Manageru z Galerie šablon Azure Quickstart](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="2f445-126">Explore the [Azure Resource Manager templates from Azure Quickstart template gallery](https://github.com/Azure/azure-quickstart-templates).</span></span>
