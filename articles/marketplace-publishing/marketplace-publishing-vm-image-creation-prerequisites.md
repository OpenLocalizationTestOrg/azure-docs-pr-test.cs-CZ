---
title: "Technické požadavky pro vytvoření bitové kopie virtuálního počítače pro Azure Marketplace | Microsoft Docs"
description: "Pochopili požadavky na vytvoření a nasazení bitové kopie virtuálního počítače v Azure Marketplace pro ostatní k nákupu."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 63c16966-0304-4b17-a715-368a0a5ccb2c
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: af3e2ad623d8d7bfafe676411f9ae3fbee78aab8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a><span data-ttu-id="62ccc-103">Technické požadavky pro vytvoření bitové kopie virtuálního počítače pro Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="62ccc-103">Technical prerequisites for creating a virtual machine image for the Azure Marketplace</span></span>
<span data-ttu-id="62ccc-104">Přečtěte si důkladně před zahájením procesu a pochopit, kde a proč se provádí každý krok.</span><span class="sxs-lookup"><span data-stu-id="62ccc-104">Read the process thoroughly before beginning and understand where and why each step is performed.</span></span> <span data-ttu-id="62ccc-105">Co nejvíce, měli byste Příprava informací o společnosti a další data, stáhněte potřebné nástroje, a vytvořit technické součásti před zahájením procesu vytvoření nabídky.</span><span class="sxs-lookup"><span data-stu-id="62ccc-105">As much as possible, you should prepare your company information and other data, download necessary tools, and/or create technical components before beginning the offer creation process.</span></span> <span data-ttu-id="62ccc-106">Tyto položky musí být vymazat z kontrola v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="62ccc-106">These items should be clear from reviewing this article.</span></span>  

## <a name="download-needed-tools--applications"></a><span data-ttu-id="62ccc-107">Stáhněte potřebné nástroje & aplikace</span><span class="sxs-lookup"><span data-stu-id="62ccc-107">Download needed tools & applications</span></span>
<span data-ttu-id="62ccc-108">Musí mít připravené před zahájením procesu následující položky:</span><span class="sxs-lookup"><span data-stu-id="62ccc-108">You should have the following items ready before beginning the process:</span></span>

* <span data-ttu-id="62ccc-109">V závislosti na tom, jaký operační systém cílení, nainstalujte [rutin prostředí Azure PowerShell](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) nebo [Linux rozhraní příkazového řádku nástroje](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) z [Azure stáhne](https://azure.microsoft.com/downloads/) stránky.</span><span class="sxs-lookup"><span data-stu-id="62ccc-109">Depending on which operating system you are targeting, install the [Azure PowerShell cmdlets](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) or [Linux command-line interface tool](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) from the [Azure Downloads](https://azure.microsoft.com/downloads/) page.</span></span>
* <span data-ttu-id="62ccc-110">Nainstalujte Azure Storage Explorer na webu CodePlex.</span><span class="sxs-lookup"><span data-stu-id="62ccc-110">Install Azure Storage Explorer from CodePlex.</span></span>
* <span data-ttu-id="62ccc-111">Stáhněte a nainstalujte nástroj pro testování certifikační pro certifikaci Azure:</span><span class="sxs-lookup"><span data-stu-id="62ccc-111">Download and install the Certification Test Tool for Azure Certified:</span></span>
  * <span data-ttu-id="62ccc-112">[http://go.microsoft.com/fwlink/?LinkId=526913](http://go.microsoft.com/fwlink/?LinkID=526913).</span><span class="sxs-lookup"><span data-stu-id="62ccc-112">[http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913).</span></span> <span data-ttu-id="62ccc-113">Je nutné počítače se systémem Windows spusťte nástroj certifikace.</span><span class="sxs-lookup"><span data-stu-id="62ccc-113">You need a Windows-based computer to run the certification tool.</span></span> <span data-ttu-id="62ccc-114">Pokud jste počítači se systémem Windows k dispozici, můžete spustit nástroj pomocí systému Windows virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="62ccc-114">If you do not have a Windows-based computer available, you can run the tool using a Windows-based VM in Azure.</span></span>

## <a name="platforms-supported"></a><span data-ttu-id="62ccc-115">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="62ccc-115">Platforms supported</span></span>
<span data-ttu-id="62ccc-116">Můžete vyvíjet založené na Azure virtuální počítače v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="62ccc-116">You can develop Azure-based VMs on Windows or Linux.</span></span> <span data-ttu-id="62ccc-117">Některé prvky proces publikování – například vytváření služby Azure kompatibilní virtuální pevný disk (VHD) – pomocí různých nástrojů a kroky v závislosti na tom, jaký operační systém, kterou používáte:</span><span class="sxs-lookup"><span data-stu-id="62ccc-117">Some elements of the publishing process--such as creating an Azure-compatible virtual hard disk (VHD)--use different tools and steps depending on which operating system you are using:</span></span>  

* <span data-ttu-id="62ccc-118">Pokud používáte Linux, naleznete v části "Vytvoření služby Azure kompatibilní virtuální pevný disk (systémem Linux)" z [Průvodce publikování bitové kopie virtuálních počítačů](marketplace-publishing-vm-image-creation.md).</span><span class="sxs-lookup"><span data-stu-id="62ccc-118">If you are using Linux, refer to the “Create an Azure-compatible VHD (Linux-based)” section of the [Virtual machine image publishing guide](marketplace-publishing-vm-image-creation.md).</span></span>
* <span data-ttu-id="62ccc-119">Pokud používáte systém Windows, naleznete v části "Vytvoření služby Azure kompatibilní virtuální pevný disk (založené na Windows)" z [Průvodce publikování bitové kopie virtuálních počítačů](marketplace-publishing-vm-image-creation.md).</span><span class="sxs-lookup"><span data-stu-id="62ccc-119">If you are using Windows, refer to the “Create an Azure-compatible VHD (Windows-based)” section of the [Virtual machine image publishing guide](marketplace-publishing-vm-image-creation.md).</span></span>

> [!NOTE]
> <span data-ttu-id="62ccc-120">Potřebujete přístup pro počítač systému Windows:</span><span class="sxs-lookup"><span data-stu-id="62ccc-120">You need access to a Windows-based machine to:</span></span>
> 
> * <span data-ttu-id="62ccc-121">Spusťte nástroj Certifikační ověření.</span><span class="sxs-lookup"><span data-stu-id="62ccc-121">Run the certification validation tool.</span></span>
> * <span data-ttu-id="62ccc-122">Vytvořte adresu URL pro podpis sdíleného virtuálního pevného disku přístup pro odeslání certifikační virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="62ccc-122">Create the VHD shared access signature URL for the VHD certification submission.</span></span>
> 
> 

## <a name="develop-your-vhd"></a><span data-ttu-id="62ccc-123">Vytvořte svůj disk VHD</span><span class="sxs-lookup"><span data-stu-id="62ccc-123">Develop your VHD</span></span>
<span data-ttu-id="62ccc-124">Můžete vyvíjet virtuální pevné disky Azure v cloudu nebo místně:</span><span class="sxs-lookup"><span data-stu-id="62ccc-124">You can develop Azure VHDs in the cloud or on-premises:</span></span>

* <span data-ttu-id="62ccc-125">Vývoj v cloudu znamená, že všechny kroky vývoj se vzdáleně na virtuální pevný disk uložené v Azure.</span><span class="sxs-lookup"><span data-stu-id="62ccc-125">Cloud-based development means all development steps are performed remotely on a VHD resident on Azure.</span></span>
* <span data-ttu-id="62ccc-126">Místní vývoj vyžaduje stahování virtuální pevný disk a vývoj pomocí místní infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="62ccc-126">On-premises development requires downloading a VHD and developing it using on-premises infrastructure.</span></span> <span data-ttu-id="62ccc-127">I když je to možné, nedoporučujeme ji.</span><span class="sxs-lookup"><span data-stu-id="62ccc-127">Although this is possible, we do not recommend it.</span></span> <span data-ttu-id="62ccc-128">Nezapomeňte, že vývoj pro Windows nebo SQL místní vyžaduje abyste měli kódy VLK relevantní místní.</span><span class="sxs-lookup"><span data-stu-id="62ccc-128">Note that developing for Windows or SQL on-premises requires you to have the relevant on-premises license keys.</span></span> <span data-ttu-id="62ccc-129">Nelze zahrnout nebo po vytvoření virtuálního počítače nainstalovat systém SQL Server.</span><span class="sxs-lookup"><span data-stu-id="62ccc-129">You cannot include or install SQL Server after creating a VM.</span></span> <span data-ttu-id="62ccc-130">Vaši nabídku musí také založit na bitovou kopii schválené SQL z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="62ccc-130">You must also base your offer on an approved SQL image from the Azure portal.</span></span> <span data-ttu-id="62ccc-131">Pokud se rozhodnete vyvíjet místně, je třeba provést některé kroky jinak než pokud byly vývoj v cloudu.</span><span class="sxs-lookup"><span data-stu-id="62ccc-131">If you decide to develop on-premises, you must perform some steps differently than if you were developing in the cloud.</span></span> <span data-ttu-id="62ccc-132">Může hledání příslušných informací v [vytvořit bitovou kopii virtuálního počítače místní](marketplace-publishing-vm-image-creation-on-premise.md).</span><span class="sxs-lookup"><span data-stu-id="62ccc-132">You can find relevant information in [Create an on-premises VM image](marketplace-publishing-vm-image-creation-on-premise.md).</span></span>

[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
