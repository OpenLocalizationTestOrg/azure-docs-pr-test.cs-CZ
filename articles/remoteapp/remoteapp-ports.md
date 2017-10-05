---
title: "Seznam portů a adres URL do seznamu povolených IP adres pro Azure RemoteApp nasazený ve virtuální síti zákazníka | Microsoft Docs"
description: "Další informace, které porty a adresy URL budete muset nakonfigurovat pro komunikaci prostřednictvím Azure RemoteApp."
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
ms.openlocfilehash: c17ff8d5441ca92f7b893edb541a1e9730c2a847
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="list-of-ports-and-urls-to-permit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a><span data-ttu-id="1b20a-103">Seznam portů a adres URL, k povolení přístupu pro Azure RemoteApp nasazení u zákazníka virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="1b20a-103">List of Ports and URLs to permit access for Azure RemoteApp Deployed in customer Virtual Network</span></span>
> [!IMPORTANT]
> <span data-ttu-id="1b20a-104">Azure RemoteApp se přestává používat dne 31. srpna 2017.</span><span class="sxs-lookup"><span data-stu-id="1b20a-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="1b20a-105">Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="1b20a-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="1b20a-106">Pokud nasazujete kolekci Azure Remoteappu Cloudová nebo hybridní ve virtuální síti (VNET), přečtěte si následující informace o portu.</span><span class="sxs-lookup"><span data-stu-id="1b20a-106">If you are deploying an Azure RemoteApp cloud or hybrid collection in a virtual network (VNET), review the following port information.</span></span> <span data-ttu-id="1b20a-107">Další informace o virtuálních sítí, přečtěte si [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1b20a-107">For more information on virtual networks, read [Virtual Network Overview](../virtual-network/virtual-networks-overview.md).</span></span> <span data-ttu-id="1b20a-108">Pokud jste vytvořili skupinu zabezpečení sítě (NSG) provoz směřující do virtuální síťové prostředky v kolekci, ujistěte se, že následující porty jsou dostupné a povolené prostřednictvím zásad zabezpečení ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="1b20a-108">If you have created a network security group (NSG) restricting traffic to the virtual network resources in your collection, make sure the following ports are accessible and allowed through the security policies on the virtual network.</span></span> <span data-ttu-id="1b20a-109">Další informace o skupinách zabezpečení sítě najdete v tématu [co je skupina zabezpečení sítě? (NSG) ](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="1b20a-109">For more information on network security groups, read [What is a Network Security Group? (NSG)](../virtual-network/virtual-networks-nsg.md).</span></span>

## <a name="azure-remoteapp-subnet-needs-access-to-these-endpoints-and-urls"></a><span data-ttu-id="1b20a-110">Azure RemoteApp podsíť potřebuje přístup k těchto koncových bodů a adresy URL:</span><span class="sxs-lookup"><span data-stu-id="1b20a-110">Azure RemoteApp subnet needs access to these endpoints and URLs:</span></span>
* <span data-ttu-id="1b20a-111">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="1b20a-111">*.servicebus.windows.net</span></span>
* <span data-ttu-id="1b20a-112">*. servicebus.net</span><span class="sxs-lookup"><span data-stu-id="1b20a-112">*.servicebus.net</span></span>
* <span data-ttu-id="1b20a-113">https://*.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="1b20a-113">https://*.remoteapp.windowsazure.com</span></span>  
* <span data-ttu-id="1b20a-114">https://www.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="1b20a-114">https://www.remoteapp.windowsazure.com</span></span> 
* <span data-ttu-id="1b20a-115">https://*RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="1b20a-115">https://*remoteapp.windowsazure.com</span></span>  
* <span data-ttu-id="1b20a-116">https://*.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="1b20a-116">https://*.core.windows.net</span></span>  
* <span data-ttu-id="1b20a-117">Odchozí: TCP: TCP: 443, 9351, 9352, 10175 10101-Document</span><span class="sxs-lookup"><span data-stu-id="1b20a-117">Outbound: TCP: TCP: 443, 9351, 9352, 10101-10175</span></span> 
* <span data-ttu-id="1b20a-118">Volitelné – UDP: 10201 10275</span><span class="sxs-lookup"><span data-stu-id="1b20a-118">Optional – UDP: 10201-10275</span></span>  

## <a name="azure-remoteapp-clients-need-access-to-these-endpoints-and-urls"></a><span data-ttu-id="1b20a-119">Azure RemoteApp klienti potřebovat přístup ke těchto koncových bodů a adresy URL:</span><span class="sxs-lookup"><span data-stu-id="1b20a-119">Azure RemoteApp clients need access to these endpoints and URLs:</span></span>
<span data-ttu-id="1b20a-120">Klienti I rozumí stolní počítače, zařízení atd který lidé použít pro připojení k aplikace nasazené v kolekci Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="1b20a-120">By clients I mean the desktops, devices etc. that people use to connect to the apps deployed in the Azure RemoteApp collection.</span></span>

* <span data-ttu-id="1b20a-121">https://telemetry.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="1b20a-121">https://telemetry.remoteapp.windowsazure.com</span></span>  
* <span data-ttu-id="1b20a-122">https://*.RemoteApp.windowsazure.com (volitelné porty UDP jsou pro tuto adresu)</span><span class="sxs-lookup"><span data-stu-id="1b20a-122">https://*.remoteapp.windowsazure.com (the optional UDP ports are for this address)</span></span> 
* <span data-ttu-id="1b20a-123">https://login.windows.net</span><span class="sxs-lookup"><span data-stu-id="1b20a-123">https://login.windows.net</span></span>  
* <span data-ttu-id="1b20a-124">https://login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="1b20a-124">https://login.microsoftonline.com</span></span>  
* <span data-ttu-id="1b20a-125">https://www.RemoteApp.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="1b20a-125">https://www.remoteapp.windowsazure.com</span></span> 
* <span data-ttu-id="1b20a-126">https://*.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="1b20a-126">https://*.core.windows.net</span></span>  
* <span data-ttu-id="1b20a-127">Odchozí: TCP: 443</span><span class="sxs-lookup"><span data-stu-id="1b20a-127">Outbound: TCP: 443</span></span>  
* <span data-ttu-id="1b20a-128">Volitelné - UDP: 3391</span><span class="sxs-lookup"><span data-stu-id="1b20a-128">Optional - UDP: 3391</span></span> 

