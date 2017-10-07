---
title: "aaaList porty a adresy URL toowhitelist pro Azure RemoteApp nasazený ve virtuální síti zákazníka | Microsoft Docs"
description: "Další informace, které porty a adresy URL tooconfigure budete potřebovat pro komunikaci prostřednictvím Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: mghosh1616
manager: mbaldwin
ms.assetid: 5a001ff7-14c9-47fa-9b39-78fd5a5f0250
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 039866f7b64ac763ca833d66031ade3def1d3543
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="list-of-ports-and-urls-toopermit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a><span data-ttu-id="d9f64-103">Seznam porty a adresy URL toopermit přístupu pro Azure RemoteApp nasazení u zákazníka virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="d9f64-103">List of Ports and URLs toopermit access for Azure RemoteApp Deployed in customer Virtual Network</span></span>
> [!IMPORTANT]
> <span data-ttu-id="d9f64-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="d9f64-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="d9f64-105">Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="d9f64-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="d9f64-106">Pokud nasazujete kolekci Azure Remoteappu Cloudová nebo hybridní ve virtuální síti (VNET), přečtěte si následující informace o portu hello.</span><span class="sxs-lookup"><span data-stu-id="d9f64-106">If you are deploying an Azure RemoteApp cloud or hybrid collection in a virtual network (VNET), review hello following port information.</span></span> <span data-ttu-id="d9f64-107">Další informace o virtuálních sítí, přečtěte si [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d9f64-107">For more information on virtual networks, read [Virtual Network Overview](../virtual-network/virtual-networks-overview.md).</span></span> <span data-ttu-id="d9f64-108">Pokud jste vytvořili skupinu zabezpečení sítě (NSG) omezení provozu toohello virtuální síťové prostředky v kolekci, ujistěte se, že hello následující porty jsou dostupné a povolené prostřednictvím zásad zabezpečení hello ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="d9f64-108">If you have created a network security group (NSG) restricting traffic toohello virtual network resources in your collection, make sure hello following ports are accessible and allowed through hello security policies on hello virtual network.</span></span> <span data-ttu-id="d9f64-109">Další informace o skupinách zabezpečení sítě najdete v tématu [co je skupina zabezpečení sítě? (NSG) ](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="d9f64-109">For more information on network security groups, read [What is a Network Security Group? (NSG)](../virtual-network/virtual-networks-nsg.md).</span></span>

## <a name="azure-remoteapp-subnet-needs-access-toothese-endpoints-and-urls"></a><span data-ttu-id="d9f64-110">Azure RemoteApp podsíť musí mít přístup toothese koncových bodů a adresy URL:</span><span class="sxs-lookup"><span data-stu-id="d9f64-110">Azure RemoteApp subnet needs access toothese endpoints and URLs:</span></span>
* <span data-ttu-id="d9f64-111">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="d9f64-111">*.servicebus.windows.net</span></span>
* <span data-ttu-id="d9f64-112">*. servicebus.net</span><span class="sxs-lookup"><span data-stu-id="d9f64-112">*.servicebus.net</span></span>
* <span data-ttu-id="d9f64-113">https://*.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="d9f64-113">https://*.remoteapp.windowsazure.com</span></span>  
* <span data-ttu-id="d9f64-114">https://www.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="d9f64-114">https://www.remoteapp.windowsazure.com</span></span> 
* <span data-ttu-id="d9f64-115">https://*RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="d9f64-115">https://*remoteapp.windowsazure.com</span></span>  
* <span data-ttu-id="d9f64-116">https://*.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="d9f64-116">https://*.core.windows.net</span></span>  
* <span data-ttu-id="d9f64-117">Odchozí: TCP: TCP: 443, 9351, 9352, 10175 10101-Document</span><span class="sxs-lookup"><span data-stu-id="d9f64-117">Outbound: TCP: TCP: 443, 9351, 9352, 10101-10175</span></span> 
* <span data-ttu-id="d9f64-118">Volitelné – UDP: 10201 10275</span><span class="sxs-lookup"><span data-stu-id="d9f64-118">Optional – UDP: 10201-10275</span></span>  

## <a name="azure-remoteapp-clients-need-access-toothese-endpoints-and-urls"></a><span data-ttu-id="d9f64-119">Azure RemoteApp klienti potřebovat přístup k toothese koncových bodů a adresy URL:</span><span class="sxs-lookup"><span data-stu-id="d9f64-119">Azure RemoteApp clients need access toothese endpoints and URLs:</span></span>
<span data-ttu-id="d9f64-120">Klienti, které I znamenat hello stolních počítačů, zařízení atd který lidé použití tooconnect toohello aplikace nasazené v hello kolekci Azure Remoteappu.</span><span class="sxs-lookup"><span data-stu-id="d9f64-120">By clients I mean hello desktops, devices etc. that people use tooconnect toohello apps deployed in hello Azure RemoteApp collection.</span></span>

* <span data-ttu-id="d9f64-121">https://telemetry.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="d9f64-121">https://telemetry.remoteapp.windowsazure.com</span></span>  
* <span data-ttu-id="d9f64-122">https://*.RemoteApp.windowsazure.com (volitelné porty UDP hello jsou pro tuto adresu)</span><span class="sxs-lookup"><span data-stu-id="d9f64-122">https://*.remoteapp.windowsazure.com (hello optional UDP ports are for this address)</span></span> 
* <span data-ttu-id="d9f64-123">https://login.windows.net</span><span class="sxs-lookup"><span data-stu-id="d9f64-123">https://login.windows.net</span></span>  
* <span data-ttu-id="d9f64-124">https://login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="d9f64-124">https://login.microsoftonline.com</span></span>  
* <span data-ttu-id="d9f64-125">https://www.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="d9f64-125">https://www.remoteapp.windowsazure.com</span></span> 
* <span data-ttu-id="d9f64-126">https://*.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="d9f64-126">https://*.core.windows.net</span></span>  
* <span data-ttu-id="d9f64-127">Odchozí: TCP: 443</span><span class="sxs-lookup"><span data-stu-id="d9f64-127">Outbound: TCP: 443</span></span>  
* <span data-ttu-id="d9f64-128">Volitelné - UDP: 3391</span><span class="sxs-lookup"><span data-stu-id="d9f64-128">Optional - UDP: 3391</span></span> 

