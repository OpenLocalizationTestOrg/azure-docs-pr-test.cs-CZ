---
title: "Vytvoření internetového nástroje pro vyrovnávání zatížení – Azure Classic Portal | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvořit internetový nástroj pro vyrovnávání zatížení v modelu nasazení Classic pomocí portálu Azure Classic"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: fa3e93c0-968a-472d-a17c-65665c050db2
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: a022154f5eca6de2d2dbfc1b9aa30d2ea0a7d650
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a><span data-ttu-id="9099c-103">Začínáme vytvářet internetový nástroj pro vyrovnávání zatížení (Classic) na portálu Azure Classic</span><span class="sxs-lookup"><span data-stu-id="9099c-103">Get started creating an Internet facing load balancer (classic) in the Azure classic portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9099c-104">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="9099c-104">Azure classic portal</span></span>](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [<span data-ttu-id="9099c-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9099c-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [<span data-ttu-id="9099c-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9099c-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [<span data-ttu-id="9099c-107">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="9099c-107">Azure Cloud Services</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="9099c-108">Než začnete pracovat s prostředky Azure, je potřeba si uvědomit, že Azure má v současné době dva modely nasazení: Azure Resource Manager a klasický.</span><span class="sxs-lookup"><span data-stu-id="9099c-108">Before you work with Azure resources, it's important to understand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="9099c-109">Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že rozumíte [modelům nasazení a příslušným nástrojům](../azure-classic-rm.md).</span><span class="sxs-lookup"><span data-stu-id="9099c-109">Make sure you understand [deployment models and tools](../azure-classic-rm.md) before you work with any Azure resource.</span></span> <span data-ttu-id="9099c-110">Dokumentaci k různým nástrojům můžete zobrazit kliknutím na karty v horní části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="9099c-110">You can view the documentation for different tools by clicking the tabs at the top of this article.</span></span> <span data-ttu-id="9099c-111">Tento článek se týká modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="9099c-111">This article covers the classic deployment model.</span></span> <span data-ttu-id="9099c-112">Případně [zjistěte, jak vytvořit internetový nástroj pro vyrovnávání zatížení pomocí Azure Resource Manageru](load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="9099c-112">You can also [Learn how to create an Internet facing load balancer using Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a><span data-ttu-id="9099c-113">Nastavení internetového nástroje pro vyrovnávání zatížení pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="9099c-113">Set up an Internet-facing load balancer for virtual machines</span></span>

<span data-ttu-id="9099c-114">Abyste mohli vyrovnávat zatížení síťového provozu z internetu napříč virtuálními počítači cloudové služby, musíte vytvořit sadu s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="9099c-114">In order to load balance network traffic from the Internet across the virtual machines of a cloud service, you must create a load-balanced set.</span></span> <span data-ttu-id="9099c-115">Tento postup předpokládá, že již máte vytvořené virtuální počítače, a že všechny jsou v rámci stejné cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="9099c-115">This procedure assumes that you have already created the virtual machines and that they are all within the same cloud service.</span></span>

<span data-ttu-id="9099c-116">**Konfigurace sady s vyrovnáváním zatížení pro virtuální počítače**</span><span class="sxs-lookup"><span data-stu-id="9099c-116">**To configure a load-balanced set for virtual machines**</span></span>

1. <span data-ttu-id="9099c-117">Na portálu Azure Classic klikněte na **Virtuální počítače** a potom klikněte na název virtuálního počítače v sadě s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="9099c-117">In the Azure classic portal, click **Virtual Machines**, and then click the name of a virtual machine in the load-balanced set.</span></span>
2. <span data-ttu-id="9099c-118">Klikněte na **Koncové body** a potom na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="9099c-118">Click **Endpoints**, and then click **Add**.</span></span>
3. <span data-ttu-id="9099c-119">Na stránce **Přidat koncový bod do virtuálního počítače** klikněte na šipku doprava.</span><span class="sxs-lookup"><span data-stu-id="9099c-119">On the **Add an endpoint to a virtual machine** page, click the right arrow.</span></span>
4. <span data-ttu-id="9099c-120">Na stránce **Zadat podrobnosti o koncovém bodu**:</span><span class="sxs-lookup"><span data-stu-id="9099c-120">On the **Specify the details of the endpoint** page:</span></span>

   * <span data-ttu-id="9099c-121">V části **Název** zadejte název koncového bodu nebo vyberte název ze seznamu předdefinovaných koncových bodů pro běžné protokoly.</span><span class="sxs-lookup"><span data-stu-id="9099c-121">In **Name**, type a name for the endpoint or select the name from the list of predefined endpoints for common protocols.</span></span>
   * <span data-ttu-id="9099c-122">V části **Protokol** vyberte podle potřeby protokol vyžadovaný typem koncového bodu, buď TCP, nebo UDP.</span><span class="sxs-lookup"><span data-stu-id="9099c-122">In **Protocol**, select the protocol required by the type of endpoint, either TCP or UDP, as needed.</span></span>
   * <span data-ttu-id="9099c-123">V části **Veřejný port a privátní port** zadejte podle potřeby čísla portů, které chcete, aby virtuální počítač používat.</span><span class="sxs-lookup"><span data-stu-id="9099c-123">In **Public Port and Private Port**, type the port numbers that you want the virtual machine to use, as needed.</span></span> <span data-ttu-id="9099c-124">Můžete použít privátní port a pravidla brány firewall na virtuálním počítači k přesměrování provozu ve směru vhodném pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9099c-124">You can use the private port and firewall rules on the virtual machine to redirect traffic in a way that is appropriate for your application.</span></span> <span data-ttu-id="9099c-125">Privátní port může být stejný jako veřejný port.</span><span class="sxs-lookup"><span data-stu-id="9099c-125">The private port can be the same as the public port.</span></span> <span data-ttu-id="9099c-126">Například koncovému bodu pro webový provoz (HTTP) můžete přiřadit port 80 jako veřejný i privátní port.</span><span class="sxs-lookup"><span data-stu-id="9099c-126">For example, for an endpoint for web (HTTP) traffic, you could assign port 80 to both the public and private port.</span></span>

5. <span data-ttu-id="9099c-127">Vyberte možnost **Vytvořit sadu s vyrovnáváním zatížení** a potom klikněte na šipku doprava.</span><span class="sxs-lookup"><span data-stu-id="9099c-127">Select **Create a load-balanced set**, and then click the right arrow.</span></span>
6. <span data-ttu-id="9099c-128">Na stránce **Konfigurovat sadu s vyrovnáváním zatížení** zadejte název sady s vyrovnáváním zatížení a potom přiřaďte hodnoty pro chování testů služby Azure Load Balancer.</span><span class="sxs-lookup"><span data-stu-id="9099c-128">On the **Configure the load-balanced set** page, type a name for the load-balanced set, and then assign the values for probe behavior of the Azure Load Balancer.</span></span> <span data-ttu-id="9099c-129">Služba Load Balancer pomocí testů určuje, jestli jsou virtuální počítače v sadě s vyrovnáváním zatížení dostupné k přijímání příchozího provozu.</span><span class="sxs-lookup"><span data-stu-id="9099c-129">The Load Balancer uses probes to determine if the virtual machines in the load-balanced set are available to receive incoming traffic.</span></span>
7. <span data-ttu-id="9099c-130">Kliknutím na značku zaškrtnutí vytvořte koncový bod s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="9099c-130">Click the check mark to create the load-balanced endpoint.</span></span> <span data-ttu-id="9099c-131">Na stránce **Koncové body virtuálního počítače** uvidíte ve sloupci **Název sady s vyrovnáváním zatížení** hodnotu **Ano**.</span><span class="sxs-lookup"><span data-stu-id="9099c-131">You will see **Yes** in the **Load-balanced set name** column of the **Endpoints** page for the virtual machine.</span></span>
8. <span data-ttu-id="9099c-132">Na portálu klikněte na **Virtuální počítače**, klikněte na název dalšího virtuálního počítače v sadě s vyrovnáváním zatížení, klikněte na **Koncové body** a potom na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="9099c-132">In the portal, click **Virtual Machines**, click the name of an additional virtual machine in the load-balanced set, click **Endpoints**, and then click **Add**.</span></span>
9. <span data-ttu-id="9099c-133">Na stránce **Přidat koncový bod do virtuálního počítače** klikněte na **Přidat koncový bod do existující sady s vyrovnáváním zatížení**, vyberte název sady s vyrovnáváním zatížení a potom klikněte na šipku doprava.</span><span class="sxs-lookup"><span data-stu-id="9099c-133">On the **Add an endpoint to a virtual machine** page, click **Add endpoint to an existing load-balanced set**, select the name of the load-balanced set, and then click the right arrow.</span></span>
10. <span data-ttu-id="9099c-134">Na stránce **Zadat podrobnosti o koncovém bodu** zadejte název koncového bodu a potom klikněte na značku zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="9099c-134">On the **Specify the details of the endpoint** page, type a name for the endpoint, and then click the check mark.</span></span>

<span data-ttu-id="9099c-135">U dalších virtuálních počítačů v sadě s vyrovnáváním zatížení opakujte kroky 8-10.</span><span class="sxs-lookup"><span data-stu-id="9099c-135">For the additional virtual machines in the load-balanced set, repeat steps 8-10.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9099c-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9099c-136">Next steps</span></span>

[<span data-ttu-id="9099c-137">Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="9099c-137">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="9099c-138">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="9099c-138">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="9099c-139">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="9099c-139">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
