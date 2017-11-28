---
title: "aaaValidate hello virtuální síť Azure toouse s Azure Remoteappem | Microsoft Docs"
description: "Zjistěte, jak toomake opravdu virtuální sítě Azure je připravené toouse s Azure Remoteappem"
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
ms.openlocfilehash: 5587556c264356e6ab6039b983a38cb2b95ed268
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="validate-hello-azure-vnet-toouse-with-azure-remoteapp"></a><span data-ttu-id="1fa7c-103">Ověření hello toouse virtuální síť Azure s Azure Remoteappem</span><span class="sxs-lookup"><span data-stu-id="1fa7c-103">Validate hello Azure VNET toouse with Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="1fa7c-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="1fa7c-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="1fa7c-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1fa7c-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="1fa7c-106">Než použijete o virtuální síť Azure s Azure Remoteappem, můžete chtít toovalidate hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="1fa7c-106">Before you use an Azure VNET with Azure RemoteApp, you might want toovalidate hello VNET.</span></span> <span data-ttu-id="1fa7c-107">To pomáhá zabránit problémům s připojením.</span><span class="sxs-lookup"><span data-stu-id="1fa7c-107">This helps prevent issues with connectivity.</span></span>

<span data-ttu-id="1fa7c-108">toovalidate virtuální sítě Azure, hello následující:</span><span class="sxs-lookup"><span data-stu-id="1fa7c-108">toovalidate your Azure VNET, do hello following:</span></span>

1. <span data-ttu-id="1fa7c-109">Vytvořte virtuální počítač Azure uvnitř hello podsíť hello virtuální síť Azure, které chcete toouse s Azure Remoteappem.</span><span class="sxs-lookup"><span data-stu-id="1fa7c-109">Create an Azure virtual machine inside hello subnet of hello Azure VNET you want toouse with Azure RemoteApp.</span></span>
2. <span data-ttu-id="1fa7c-110">Připojit toothat virtuálního počítače pomocí programu hello **Connect** možnost na portálu pro správu hello.</span><span class="sxs-lookup"><span data-stu-id="1fa7c-110">Connect toothat VM by using hello **Connect** option in hello management portal.</span></span>
3. <span data-ttu-id="1fa7c-111">Připojit virtuální počítač toohello hello stejné domény, které chcete toouse s Azure Remoteappem.</span><span class="sxs-lookup"><span data-stu-id="1fa7c-111">Join hello virtual machine toohello same domain that you want toouse with Azure RemoteApp.</span></span> <span data-ttu-id="1fa7c-112">Pokud vytváříte hybridní kolekci, která se připojuje tooyour do místní sítě, připojení k místní doméně tooyour hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1fa7c-112">If you are creating a hybrid collection that connects tooyour on-premises network, join hello virtual machine tooyour local domain.</span></span>

<span data-ttu-id="1fa7c-113">Pokud budou úspěšní, hello virtuální síť Azure je připravené toouse s Remoteappem.</span><span class="sxs-lookup"><span data-stu-id="1fa7c-113">If this is successful, hello Azure VNET is ready toouse with RemoteApp.</span></span>

<span data-ttu-id="1fa7c-114">Další informace o hello začátku do konce hybridní kolekci pracovního postupu najdete v tématu hello následující články:</span><span class="sxs-lookup"><span data-stu-id="1fa7c-114">For more information about hello end-to-end hybrid collection workflow, see hello following articles:</span></span>

* [<span data-ttu-id="1fa7c-115">Jak tooplan virtuální sítě pro Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="1fa7c-115">How tooplan your virtual network for Azure RemoteApp</span></span>](remoteapp-planvnet.md)
* [<span data-ttu-id="1fa7c-116">Vytvoření hybridní kolekce</span><span class="sxs-lookup"><span data-stu-id="1fa7c-116">Create a hybrid collection</span></span>](remoteapp-create-hybrid-deployment.md)
* [<span data-ttu-id="1fa7c-117">Nasadit Azure RemoteApp kolekce tooyour Azure Virtual Network (s podporou pro ExpressRoute)</span><span class="sxs-lookup"><span data-stu-id="1fa7c-117">Deploy Azure RemoteApp collection tooyour Azure Virtual Network (with support for ExpressRoute)</span></span>](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)

