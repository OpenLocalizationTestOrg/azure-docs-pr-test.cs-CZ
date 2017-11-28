---
title: "aaaTechnical požadavky pro vytvoření bitové kopie virtuálního počítače pro hello Azure Marketplace | Microsoft Docs"
description: "Pochopení hello požadavky pro vytváření a nasazování toohello bitové kopie virtuální počítač Azure Marketplace pro ostatní toopurchase."
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
ms.openlocfilehash: fcae4d9e052581e3c1dfe7962e6d50ec31040419
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-hello-azure-marketplace"></a><span data-ttu-id="fe02f-103">Technické požadavky pro vytvoření bitové kopie virtuálního počítače pro hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="fe02f-103">Technical prerequisites for creating a virtual machine image for hello Azure Marketplace</span></span>
<span data-ttu-id="fe02f-104">Přečtěte si hello proces důkladně před zahájením a pochopit, kde a proč se provádí každý krok.</span><span class="sxs-lookup"><span data-stu-id="fe02f-104">Read hello process thoroughly before beginning and understand where and why each step is performed.</span></span> <span data-ttu-id="fe02f-105">Co nejvíce, měli byste Příprava informací o společnosti a další data, stáhněte potřebné nástroje, a vytvořit technické součásti před zahájením procesu vytváření nabídka hello.</span><span class="sxs-lookup"><span data-stu-id="fe02f-105">As much as possible, you should prepare your company information and other data, download necessary tools, and/or create technical components before beginning hello offer creation process.</span></span> <span data-ttu-id="fe02f-106">Tyto položky musí být vymazat z kontrola v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="fe02f-106">These items should be clear from reviewing this article.</span></span>  

## <a name="download-needed-tools--applications"></a><span data-ttu-id="fe02f-107">Stáhněte potřebné nástroje & aplikace</span><span class="sxs-lookup"><span data-stu-id="fe02f-107">Download needed tools & applications</span></span>
<span data-ttu-id="fe02f-108">Měli byste hello následující položky, které jsou připravené před zahájením procesu hello:</span><span class="sxs-lookup"><span data-stu-id="fe02f-108">You should have hello following items ready before beginning hello process:</span></span>

* <span data-ttu-id="fe02f-109">V závislosti na tom, jaký operační systém cílení, instalace hello [rutin prostředí Azure PowerShell](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) nebo [Linux rozhraní příkazového řádku nástroje](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) z hello [Azure stáhne](https://azure.microsoft.com/downloads/) stránka.</span><span class="sxs-lookup"><span data-stu-id="fe02f-109">Depending on which operating system you are targeting, install hello [Azure PowerShell cmdlets](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) or [Linux command-line interface tool](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) from hello [Azure Downloads](https://azure.microsoft.com/downloads/) page.</span></span>
* <span data-ttu-id="fe02f-110">Nainstalujte Azure Storage Explorer na webu CodePlex.</span><span class="sxs-lookup"><span data-stu-id="fe02f-110">Install Azure Storage Explorer from CodePlex.</span></span>
* <span data-ttu-id="fe02f-111">Stáhněte a nainstalujte hello nástroj pro testování certifikační pro certifikaci Azure:</span><span class="sxs-lookup"><span data-stu-id="fe02f-111">Download and install hello Certification Test Tool for Azure Certified:</span></span>
  * <span data-ttu-id="fe02f-112">[http://go.microsoft.com/fwlink/?LinkId=526913](http://go.microsoft.com/fwlink/?LinkID=526913).</span><span class="sxs-lookup"><span data-stu-id="fe02f-112">[http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913).</span></span> <span data-ttu-id="fe02f-113">Je nutné nástroj Certifikační hello toorun počítače se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="fe02f-113">You need a Windows-based computer toorun hello certification tool.</span></span> <span data-ttu-id="fe02f-114">Pokud jste počítači se systémem Windows k dispozici, můžete spustit nástroj hello pomocí systému Windows virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="fe02f-114">If you do not have a Windows-based computer available, you can run hello tool using a Windows-based VM in Azure.</span></span>

## <a name="platforms-supported"></a><span data-ttu-id="fe02f-115">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="fe02f-115">Platforms supported</span></span>
<span data-ttu-id="fe02f-116">Můžete vyvíjet založené na Azure virtuální počítače v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="fe02f-116">You can develop Azure-based VMs on Windows or Linux.</span></span> <span data-ttu-id="fe02f-117">Některé prvky hello publikování proces – například vytváření služby Azure kompatibilní virtuální pevný disk (VHD) – pomocí různých nástrojů a kroky v závislosti na tom, jaký operační systém, kterou používáte:</span><span class="sxs-lookup"><span data-stu-id="fe02f-117">Some elements of hello publishing process--such as creating an Azure-compatible virtual hard disk (VHD)--use different tools and steps depending on which operating system you are using:</span></span>  

* <span data-ttu-id="fe02f-118">Pokud používáte Linux, najdete v části "Vytvoření služby Azure kompatibilní virtuální pevný disk (systémem Linux)" toohello Dobrý den [Průvodce publikování bitové kopie virtuálních počítačů](marketplace-publishing-vm-image-creation.md).</span><span class="sxs-lookup"><span data-stu-id="fe02f-118">If you are using Linux, refer toohello “Create an Azure-compatible VHD (Linux-based)” section of hello [Virtual machine image publishing guide](marketplace-publishing-vm-image-creation.md).</span></span>
* <span data-ttu-id="fe02f-119">Pokud používáte systém Windows, naleznete v části "Vytvoření služby Azure kompatibilní virtuální pevný disk (založené na Windows)" toohello Dobrý den [Průvodce publikování bitové kopie virtuálních počítačů](marketplace-publishing-vm-image-creation.md).</span><span class="sxs-lookup"><span data-stu-id="fe02f-119">If you are using Windows, refer toohello “Create an Azure-compatible VHD (Windows-based)” section of hello [Virtual machine image publishing guide](marketplace-publishing-vm-image-creation.md).</span></span>

> [!NOTE]
> <span data-ttu-id="fe02f-120">Potřebujete přístup tooa založené na Windows počítače:</span><span class="sxs-lookup"><span data-stu-id="fe02f-120">You need access tooa Windows-based machine to:</span></span>
> 
> * <span data-ttu-id="fe02f-121">Spusťte nástroj ověření certifikační hello.</span><span class="sxs-lookup"><span data-stu-id="fe02f-121">Run hello certification validation tool.</span></span>
> * <span data-ttu-id="fe02f-122">Vytvořte adresu URL pro hello sdíleného virtuálního pevného disku přístup podpis pro hello odesílání virtuálního pevného disku certifikační.</span><span class="sxs-lookup"><span data-stu-id="fe02f-122">Create hello VHD shared access signature URL for hello VHD certification submission.</span></span>
> 
> 

## <a name="develop-your-vhd"></a><span data-ttu-id="fe02f-123">Vytvořte svůj disk VHD</span><span class="sxs-lookup"><span data-stu-id="fe02f-123">Develop your VHD</span></span>
<span data-ttu-id="fe02f-124">Můžete vyvíjet virtuální pevné disky Azure v cloudu hello nebo místní:</span><span class="sxs-lookup"><span data-stu-id="fe02f-124">You can develop Azure VHDs in hello cloud or on-premises:</span></span>

* <span data-ttu-id="fe02f-125">Vývoj v cloudu znamená, že všechny kroky vývoj se vzdáleně na virtuální pevný disk uložené v Azure.</span><span class="sxs-lookup"><span data-stu-id="fe02f-125">Cloud-based development means all development steps are performed remotely on a VHD resident on Azure.</span></span>
* <span data-ttu-id="fe02f-126">Místní vývoj vyžaduje stahování virtuální pevný disk a vývoj pomocí místní infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="fe02f-126">On-premises development requires downloading a VHD and developing it using on-premises infrastructure.</span></span> <span data-ttu-id="fe02f-127">I když je to možné, nedoporučujeme ji.</span><span class="sxs-lookup"><span data-stu-id="fe02f-127">Although this is possible, we do not recommend it.</span></span> <span data-ttu-id="fe02f-128">Nezapomeňte, že vývoj pro Windows nebo SQL místní vyžaduje můžete kódy VLK relevantní místní toohave hello.</span><span class="sxs-lookup"><span data-stu-id="fe02f-128">Note that developing for Windows or SQL on-premises requires you toohave hello relevant on-premises license keys.</span></span> <span data-ttu-id="fe02f-129">Nelze zahrnout nebo po vytvoření virtuálního počítače nainstalovat systém SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fe02f-129">You cannot include or install SQL Server after creating a VM.</span></span> <span data-ttu-id="fe02f-130">Vaši nabídku musí také založit na bitovou kopii schválené SQL hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fe02f-130">You must also base your offer on an approved SQL image from hello Azure portal.</span></span> <span data-ttu-id="fe02f-131">Pokud se rozhodnete toodevelop místně, je nutné provést některé kroky jinak než pokud byly vývoj v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="fe02f-131">If you decide toodevelop on-premises, you must perform some steps differently than if you were developing in hello cloud.</span></span> <span data-ttu-id="fe02f-132">Může hledání příslušných informací v [vytvořit bitovou kopii virtuálního počítače místní](marketplace-publishing-vm-image-creation-on-premise.md).</span><span class="sxs-lookup"><span data-stu-id="fe02f-132">You can find relevant information in [Create an on-premises VM image](marketplace-publishing-vm-image-creation-on-premise.md).</span></span>

[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
