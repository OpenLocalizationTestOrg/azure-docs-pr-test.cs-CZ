---
title: "Připojení z virtuální sítě do Azure Data Lake Store | Microsoft Docs"
description: "Připojení k Azure Data Lake Store z virtuálních sítí Azure"
services: data-lake-store,data-catalog
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 683fcfdc-cf93-46c3-b2d2-5cb79f5e9ea5
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: ff7d28d7b53e872b804788647b1e672fafcf6995
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="access-azure-data-lake-store-from-vms-within-an-azure-vnet"></a><span data-ttu-id="d7f61-103">Přístup k Azure Data Lake Store z virtuálních počítačů v rámci síť Azure</span><span class="sxs-lookup"><span data-stu-id="d7f61-103">Access Azure Data Lake Store from VMs within an Azure VNET</span></span>
<span data-ttu-id="d7f61-104">Azure Data Lake Store je PaaS služba, která běží na veřejné internetové IP adresy.</span><span class="sxs-lookup"><span data-stu-id="d7f61-104">Azure Data Lake Store is a PaaS service that runs on public Internet IP addresses.</span></span> <span data-ttu-id="d7f61-105">Jakýkoli server, který se může připojit k veřejnému Internetu může obvykle připojit k Azure Data Lake Store také koncové body.</span><span class="sxs-lookup"><span data-stu-id="d7f61-105">Any server that can connect to the public Internet can typically connect to Azure Data Lake Store endpoints as well.</span></span> <span data-ttu-id="d7f61-106">Ve výchozím nastavení všechny virtuální počítače, které jsou v sítě Azure Vnet můžete přístup k Internetu a proto můžete přístup k Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="d7f61-106">By default, all VMs that are in Azure VNETs can access the Internet and hence can access Azure Data Lake Store.</span></span> <span data-ttu-id="d7f61-107">Nicméně je možné nakonfigurovat virtuální počítače ve virtuální síti není mít přístup k Internetu.</span><span class="sxs-lookup"><span data-stu-id="d7f61-107">However, it is possible to configure VMs in a VNET to not have access to the Internet.</span></span> <span data-ttu-id="d7f61-108">Pro tyto virtuální počítače přístup k Azure Data Lake Store je omezený také.</span><span class="sxs-lookup"><span data-stu-id="d7f61-108">For such VMs, access to Azure Data Lake Store is restricted as well.</span></span> <span data-ttu-id="d7f61-109">Blokování veřejný přístup k Internetu pro virtuální počítače ve virtuálních sítí Azure lze provést pomocí kteréhokoli následující postup.</span><span class="sxs-lookup"><span data-stu-id="d7f61-109">Blocking public Internet access for VMs in Azure VNETs can be done using any of the following approach.</span></span>

* <span data-ttu-id="d7f61-110">Podle konfigurace skupin zabezpečení sítě (NSG)</span><span class="sxs-lookup"><span data-stu-id="d7f61-110">By configuring Network Security Groups (NSG)</span></span>
* <span data-ttu-id="d7f61-111">Nakonfigurováním uživatele definované trasy (UDR)</span><span class="sxs-lookup"><span data-stu-id="d7f61-111">By configuring User Defined Routes (UDR)</span></span>
* <span data-ttu-id="d7f61-112">Při výměně tras přes protokol BGP (odvětví standardní protokol dynamického směrování) ExpressRoute je při použití tohoto bloku přístup k Internetu</span><span class="sxs-lookup"><span data-stu-id="d7f61-112">By exchanging routes via BGP (industry standard dynamic routing protocol) when ExpressRoute is used that block access to the Internet</span></span>

<span data-ttu-id="d7f61-113">V tomto článku se dozvíte, jak povolit přístup k Azure Data Lake Store z virtuálních počítačů Azure, které byly omezený přístup k prostředkům pomocí jedné ze tří metod uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="d7f61-113">In this article, you will learn how to enable access to the Azure Data Lake Store from Azure VMs which have been restricted to access resources using one of the three methods listed above.</span></span>

## <a name="enabling-connectivity-to-azure-data-lake-store-from-vms-with-restricted-connectivity"></a><span data-ttu-id="d7f61-114">Povolení připojení k Azure Data Lake Store z virtuálních počítačů s připojením s omezeným přístupem</span><span class="sxs-lookup"><span data-stu-id="d7f61-114">Enabling connectivity to Azure Data Lake Store from VMs with restricted connectivity</span></span>
<span data-ttu-id="d7f61-115">Pro přístup k Azure Data Lake Store z těchto virtuálních počítačů, je nutné nakonfigurovat jejich přístupu k IP adresu, kde je k dispozici účet Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="d7f61-115">To access Azure Data Lake Store from such VMs, you must configure them to access the IP address where the Azure Data Lake Store account is available.</span></span> <span data-ttu-id="d7f61-116">Poznáte IP adresy pro vaše účty Data Lake Store pomocí překladu názvů DNS vaše účty (`<account>.azuredatalakestore.net`).</span><span class="sxs-lookup"><span data-stu-id="d7f61-116">You can identify the IP addresses for your Data Lake Store accounts by resolving the DNS names of your accounts (`<account>.azuredatalakestore.net`).</span></span> <span data-ttu-id="d7f61-117">To můžete použít nástroje, jako **nslookup**.</span><span class="sxs-lookup"><span data-stu-id="d7f61-117">For this you can use tools such as **nslookup**.</span></span> <span data-ttu-id="d7f61-118">Otevřete příkazový řádek ve vašem počítači a spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="d7f61-118">Open a command prompt on your computer and run the following command.</span></span>

    nslookup mydatastore.azuredatalakestore.net

<span data-ttu-id="d7f61-119">Výstup vypadá přibližně takto.</span><span class="sxs-lookup"><span data-stu-id="d7f61-119">The output resembles the following.</span></span> <span data-ttu-id="d7f61-120">Hodnota proti **adresu** vlastnost je IP adresa spojená s vaším účtem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="d7f61-120">The value against **Address** property is the IP address associated with your Data Lake Store account.</span></span>

    Non-authoritative answer:
    Name:    1434ceb1-3a4b-4bc0-9c69-a0823fd69bba-mydatastore.projectcabostore.net
    Address:  104.44.88.112
    Aliases:  mydatastore.azuredatalakestore.net


### <a name="enabling-connectivity-from-vms-restricted-by-using-nsg"></a><span data-ttu-id="d7f61-121">Povolení připojení z virtuálních počítačů omezený pomocí NSG</span><span class="sxs-lookup"><span data-stu-id="d7f61-121">Enabling connectivity from VMs restricted by using NSG</span></span>
<span data-ttu-id="d7f61-122">Pokud pravidla NSG se používá k blokování přístupu k Internetu, můžete vytvořit další NSG, která umožňuje přístup k IP adrese Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="d7f61-122">When a NSG rule is used to block access to the Internet, then you can create another NSG that allows access to the Data Lake Store IP Address.</span></span> <span data-ttu-id="d7f61-123">Další informace o pravidla NSG je k dispozici na [co je skupina zabezpečení sítě?](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="d7f61-123">More information on NSG rules is available at [What is a Network Security Group?](../virtual-network/virtual-networks-nsg.md).</span></span> <span data-ttu-id="d7f61-124">Pokyny o tom, jak vytvářet skupiny Nsg najdete v tématu [Správa skupin Nsg pomocí portálu Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="d7f61-124">For instructions on how to create NSGs see [How to manage NSGs using the Azure portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).</span></span>

### <a name="enabling-connectivity-from-vms-restricted-by-using-udr-or-expressroute"></a><span data-ttu-id="d7f61-125">Povolení připojení z virtuálních počítačů omezený pomocí UDR nebo ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="d7f61-125">Enabling connectivity from VMs restricted by using UDR or ExpressRoute</span></span>
<span data-ttu-id="d7f61-126">Pokud trasy, udr nebo trasy protokolu BGP vyměňují se používá k blokování přístupu k Internetu, je potřeba nakonfigurovat tak, aby virtuální počítače v těchto podsítích přístup koncových bodů Data Lake Store speciální trasy.</span><span class="sxs-lookup"><span data-stu-id="d7f61-126">When routes, either UDRs or BGP-exchanged routes, are used to block access to the Internet, a special route needs to be configured so that VMs in such subnets can access Data Lake Store endpoints.</span></span> <span data-ttu-id="d7f61-127">Další informace najdete v tématu [co jsou trasy definované uživatelem?](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d7f61-127">For more information, see [What are User Defined Routes?](../virtual-network/virtual-networks-udr-overview.md).</span></span> <span data-ttu-id="d7f61-128">Pokyny pro vytvoření udr, najdete v části [udr vytvořit ve službě Správce prostředků](../virtual-network/virtual-network-create-udr-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="d7f61-128">For instructions on creating UDRs, see [Create UDRs in Resource Manager](../virtual-network/virtual-network-create-udr-arm-ps.md).</span></span>

### <a name="enabling-connectivity-from-vms-restricted-by-using-expressroute"></a><span data-ttu-id="d7f61-129">Povolení připojení z virtuálních počítačů omezený pomocí ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="d7f61-129">Enabling connectivity from VMs restricted by using ExpressRoute</span></span>
<span data-ttu-id="d7f61-130">Když je nakonfigurovaná okruh ExpressRoute, místní servery přistupovat k Data Lake Store prostřednictvím veřejného partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="d7f61-130">When an ExpressRoute circuit is configured, the on-premises servers can access Data Lake Store through public peering.</span></span> <span data-ttu-id="d7f61-131">Další informace o konfiguraci ExpressRoute pro veřejný partnerský vztah je k dispozici na [nejčastější dotazy k ExpressRoute](../expressroute/expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="d7f61-131">More details on configuring ExpressRoute for public peering is available at [ExpressRoute FAQs](../expressroute/expressroute-faqs.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="d7f61-132">Viz také</span><span class="sxs-lookup"><span data-stu-id="d7f61-132">See also</span></span>
* [<span data-ttu-id="d7f61-133">Přehled Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="d7f61-133">Overview of Azure Data Lake Store</span></span>](data-lake-store-overview.md)
* [<span data-ttu-id="d7f61-134">Zabezpečení dat uložených v Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="d7f61-134">Securing data stored in Azure Data Lake Store</span></span>](data-lake-store-security-overview.md)

