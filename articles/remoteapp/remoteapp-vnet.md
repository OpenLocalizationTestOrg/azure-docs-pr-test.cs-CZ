---
title: "Ověření virtuální síť Azure pro použití s Azure Remoteappem | Microsoft Docs"
description: "Naučte se ujistěte se, že virtuální sítě Azure je připravené k použití s Azure Remoteappem"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b573ba02-4587-4be5-9821-27bd891a73b2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 05c8a0ff04293947cec391b6467cc4adddb6a7b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="validate-the-azure-vnet-to-use-with-azure-remoteapp"></a><span data-ttu-id="4f028-103">Ověření virtuální síť Azure pro použití s Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="4f028-103">Validate the Azure VNET to use with Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="4f028-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="4f028-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="4f028-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="4f028-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="4f028-106">Než použijete o virtuální síť Azure s Azure Remoteappem, můžete ověřit virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4f028-106">Before you use an Azure VNET with Azure RemoteApp, you might want to validate the VNET.</span></span> <span data-ttu-id="4f028-107">To pomáhá zabránit problémům s připojením.</span><span class="sxs-lookup"><span data-stu-id="4f028-107">This helps prevent issues with connectivity.</span></span>

<span data-ttu-id="4f028-108">Ověření virtuální sítě Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="4f028-108">To validate your Azure VNET, do the following:</span></span>

1. <span data-ttu-id="4f028-109">Vytvořte virtuální počítač Azure uvnitř podsíť virtuální sítě Azure, kterou chcete použít s Azure Remoteappem.</span><span class="sxs-lookup"><span data-stu-id="4f028-109">Create an Azure virtual machine inside the subnet of the Azure VNET you want to use with Azure RemoteApp.</span></span>
2. <span data-ttu-id="4f028-110">Připojit k tohoto virtuálního počítače pomocí **Connect** možnost v portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="4f028-110">Connect to that VM by using the **Connect** option in the management portal.</span></span>
3. <span data-ttu-id="4f028-111">Připojení virtuálního počítače do stejné domény, který chcete použít s Azure Remoteappem.</span><span class="sxs-lookup"><span data-stu-id="4f028-111">Join the virtual machine to the same domain that you want to use with Azure RemoteApp.</span></span> <span data-ttu-id="4f028-112">Pokud vytváříte hybridní kolekci, která se připojuje k vaší místní síti, připojení k místní doméně virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4f028-112">If you are creating a hybrid collection that connects to your on-premises network, join the virtual machine to your local domain.</span></span>

<span data-ttu-id="4f028-113">Pokud budou úspěšní, virtuální síť Azure je připravené k použití s Remoteappem.</span><span class="sxs-lookup"><span data-stu-id="4f028-113">If this is successful, the Azure VNET is ready to use with RemoteApp.</span></span>

<span data-ttu-id="4f028-114">Další informace o pracovním postupu začátku do konce hybridní kolekci najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="4f028-114">For more information about the end-to-end hybrid collection workflow, see the following articles:</span></span>

* [<span data-ttu-id="4f028-115">Postup plánování virtuální sítě Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="4f028-115">How to plan your virtual network for Azure RemoteApp</span></span>](remoteapp-planvnet.md)
* [<span data-ttu-id="4f028-116">Vytvoření hybridní kolekce</span><span class="sxs-lookup"><span data-stu-id="4f028-116">Create a hybrid collection</span></span>](remoteapp-create-hybrid-deployment.md)
* [<span data-ttu-id="4f028-117">Nasaďte kolekci Azure Remoteappu k virtuální síti Azure (s podporou pro ExpressRoute)</span><span class="sxs-lookup"><span data-stu-id="4f028-117">Deploy Azure RemoteApp collection to your Azure Virtual Network (with support for ExpressRoute)</span></span>](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)

