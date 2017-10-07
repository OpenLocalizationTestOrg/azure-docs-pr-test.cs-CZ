---
title: "aaaCreate nebo upravit labs automaticky pomocí šablon Azure Resource Manageru pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toouse šablon Azure Resource Manageru pomocí prostředí PowerShell toocreate nebo upravit automaticky v testovacím prostředí DevTest labs"
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
ms.openlocfilehash: 29c8bc67caaec17b1f8926dde4e5d9d314b06600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-or-modify-labs-automatically-using-azure-resource-manager-templates-and-powershell"></a><span data-ttu-id="17bc1-103">Vytvořit nebo upravit labs automaticky pomocí šablon Azure Resource Manageru a prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="17bc1-103">Create or modify labs automatically using Azure Resource Manager templates and PowerShell</span></span>

<span data-ttu-id="17bc1-104">DevTest Labs poskytuje mnoho šablon Azure Resource Manageru a skriptů prostředí PowerShell, které můžete vám pomohou rychle a automaticky vytvořit nové labs nebo upravit existující labs a pak nasadit tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="17bc1-104">DevTest Labs provides many Azure Resource Manager templates and PowerShell scripts that can help you quickly and automatically create new labs or modify existing labs and then deploy these resources.</span></span>

<span data-ttu-id="17bc1-105">Tento článek pomůže vás provede procesem hello pomocí těchto šablon a skripty tooautomate hello vytváření, úpravu a nasazení vaší laboratoře.</span><span class="sxs-lookup"><span data-stu-id="17bc1-105">This article helps guide you through hello process of using these templates and scripts tooautomate hello creation, modification, and deployment of your labs.</span></span> <span data-ttu-id="17bc1-106">Tento článek také ukazuje, kde můžete najít další informace o tom, jak toouse prostředí PowerShell tooperform některé běžné úlohy v DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="17bc1-106">This article also shows you where you can find more information about how toouse PowerShell tooperform some common tasks in DevTest Labs.</span></span>

## <a name="step-1-gather-your-templates-and-scripts"></a><span data-ttu-id="17bc1-107">Krok 1: Shromáždění šablony a skripty</span><span class="sxs-lookup"><span data-stu-id="17bc1-107">Step 1: Gather your templates and scripts</span></span>
<span data-ttu-id="17bc1-108">Můžete najít předem provedené [šablon Azure Resource Manageru](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) a [skriptů prostředí PowerShell](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) v našeho veřejného [úložiště Github](https://github.com/Azure/azure-devtestlab).</span><span class="sxs-lookup"><span data-stu-id="17bc1-108">You can find pre-made [Azure Resource Manager templates](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) and [PowerShell scripts](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) at our public [Github repository](https://github.com/Azure/azure-devtestlab).</span></span> <span data-ttu-id="17bc1-109">Použijte je jako-je, nebo si je přizpůsobit pro vaše potřeby a uložit je do vlastní [privátní úložiště Git](devtest-lab-add-artifact-repo.md).</span><span class="sxs-lookup"><span data-stu-id="17bc1-109">Use them as-is, or customize them for your needs and store them in your own [private Git repo](devtest-lab-add-artifact-repo.md).</span></span> 

## <a name="step-2-modify-your-azure-resource-manager-template"></a><span data-ttu-id="17bc1-110">Krok 2: Upravte šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="17bc1-110">Step 2: Modify your Azure Resource Manager template</span></span>
<span data-ttu-id="17bc1-111">Můžete provést kroky hello v [vytvoření vaší první šablony Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-create-first-template) Pokud jste vytvořili šablonu před nikdy.</span><span class="sxs-lookup"><span data-stu-id="17bc1-111">You can follow hello steps at [Create your first Azure Resource Manager template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-create-first-template) if you have never created a template before.</span></span>

<span data-ttu-id="17bc1-112">Kromě toho [osvědčené postupy pro vytváření šablon Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) nabízí mnoho pokyny a návrhy toohelp vytváření šablon Azure Resource Manageru, které jsou toouse spolehlivé a snadné.</span><span class="sxs-lookup"><span data-stu-id="17bc1-112">In addition, [Best practices for creating Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) offers many guidelines and suggestions toohelp you create Azure Resource Manager templates that are reliable and easy toouse.</span></span> <span data-ttu-id="17bc1-113">Obvykle budete používat jeden z přístupů hello varianta nebo příklady zadat a upravit šablony pro vaše potřeby.</span><span class="sxs-lookup"><span data-stu-id="17bc1-113">Typically, you will use a variation of one of hello approaches or examples provided and modify your template for your needs.</span></span>

## <a name="step-3-deploy-resources-with-powershell"></a><span data-ttu-id="17bc1-114">Krok 3: Nasazení prostředků v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="17bc1-114">Step 3: Deploy resources with PowerShell</span></span>
<span data-ttu-id="17bc1-115">Po přizpůsobení šablony a skripty, postupujte podle hello kroky nezbytné příliš[nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="17bc1-115">After you have customized your templates and scripts, follow hello steps necessary too[deploy resources with Resource Manager templates and Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span></span> <span data-ttu-id="17bc1-116">Hello článek poskytuje obecné informace o použití prostředí Azure PowerShell s toodeploy šablony Azure Resource Manager tooAzure vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="17bc1-116">hello article provides general information about using Azure PowerShell with Azure Resource Manager templates toodeploy your resources tooAzure.</span></span>


## <a name="common-tasks-you-can-perform-in-devtest-labs-using-powershell"></a><span data-ttu-id="17bc1-117">Běžné úlohy, které můžete provádět v DevTest labs pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="17bc1-117">Common tasks you can perform in DevTest labs using PowerShell</span></span>
<span data-ttu-id="17bc1-118">Existuje mnoho dalších běžných úloh můžete automatizovat pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="17bc1-118">There are many other common tasks that you can automate by using PowerShell.</span></span> <span data-ttu-id="17bc1-119">Hello následující oddíly hello dokumentace popisují hello kroky požadované tooperform tyto úlohy.</span><span class="sxs-lookup"><span data-stu-id="17bc1-119">hello following sections of hello documentation outline hello steps required tooperform these tasks.</span></span>

* [<span data-ttu-id="17bc1-120">Vytvořit vlastní image ze souboru virtuálního pevného disku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="17bc1-120">Create a custom image from a VHD file using PowerShell</span></span>](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
* [<span data-ttu-id="17bc1-121">Nahrání virtuálního pevného disku soubor toolab účet úložiště pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="17bc1-121">Upload VHD file toolab's storage account using PowerShell</span></span>](devtest-lab-upload-vhd-using-powershell.md)
* [<span data-ttu-id="17bc1-122">Přidat testovacím tooa externího uživatele pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="17bc1-122">Add an external user tooa lab using PowerShell</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell)
* [<span data-ttu-id="17bc1-123">Vytvoření testovacího prostředí vlastní role pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="17bc1-123">Create a lab custom role using PowerShell</span></span>](devtest-lab-grant-user-permissions-to-specific-lab-policies.md#creating-a-lab-custom-role-using-powershell)

### <a name="next-steps"></a><span data-ttu-id="17bc1-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="17bc1-124">Next steps</span></span>
* <span data-ttu-id="17bc1-125">Zjistěte, jak toocreate [privátní úložiště Git](devtest-lab-add-artifact-repo.md) kde budete ukládat vlastní šablony nebo skripty.</span><span class="sxs-lookup"><span data-stu-id="17bc1-125">Learn how toocreate a [private Git repository](devtest-lab-add-artifact-repo.md) where you will store your customized templates or scripts.</span></span>
* <span data-ttu-id="17bc1-126">Prozkoumejte hello [šablon Azure Resource Manageru z Galerie šablon Azure Quickstart](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="17bc1-126">Explore hello [Azure Resource Manager templates from Azure Quickstart template gallery](https://github.com/Azure/azure-quickstart-templates).</span></span>
