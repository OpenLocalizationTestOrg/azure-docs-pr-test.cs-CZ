---
title: "Vyrovnávání zatížení aaaCreate směřujících Internetu – portál Azure classic | Microsoft Docs"
description: "Zjistěte, jak toocreate k Internetu přístupných pro vyrovnávání zatížení pomocí modelu nasazení classic hello portál Azure classic"
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
ms.openlocfilehash: 27b0d5af6e7b493fa94a9dfbfa260483ae95a2fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-hello-azure-classic-portal"></a><span data-ttu-id="b7f57-103">Začínáte s vytvářením internetovým Vyrovnávání zatížení (klasické) v hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="b7f57-103">Get started creating an Internet facing load balancer (classic) in hello Azure classic portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b7f57-104">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="b7f57-104">Azure classic portal</span></span>](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [<span data-ttu-id="b7f57-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b7f57-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [<span data-ttu-id="b7f57-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b7f57-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [<span data-ttu-id="b7f57-107">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="b7f57-107">Azure Cloud Services</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="b7f57-108">Než začnete pracovat s prostředky Azure, je důležité toounderstand Azure aktuálně má dva modely nasazení: Azure Resource Manager a Klasický model.</span><span class="sxs-lookup"><span data-stu-id="b7f57-108">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="b7f57-109">Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že rozumíte [modelům nasazení a příslušným nástrojům](../azure-classic-rm.md).</span><span class="sxs-lookup"><span data-stu-id="b7f57-109">Make sure you understand [deployment models and tools](../azure-classic-rm.md) before you work with any Azure resource.</span></span> <span data-ttu-id="b7f57-110">Hello dokumentaci k různým nástrojům můžete zobrazit kliknutím na karty hello hello horní části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="b7f57-110">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span> <span data-ttu-id="b7f57-111">Tento článek se týká modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="b7f57-111">This article covers hello classic deployment model.</span></span> <span data-ttu-id="b7f57-112">Můžete také [zjistěte, jak toocreate přístupem Internetu pro vyrovnávání zátěže pomocí Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b7f57-112">You can also [Learn how toocreate an Internet facing load balancer using Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a><span data-ttu-id="b7f57-113">Nastavení internetového nástroje pro vyrovnávání zatížení pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="b7f57-113">Set up an Internet-facing load balancer for virtual machines</span></span>

<span data-ttu-id="b7f57-114">V pořadí tooload vyrovnávání síťové přenosy z Internetu hello napříč hello virtuální počítače cloudové služby je nutné vytvořit sadu s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="b7f57-114">In order tooload balance network traffic from hello Internet across hello virtual machines of a cloud service, you must create a load-balanced set.</span></span> <span data-ttu-id="b7f57-115">Tento postup předpokládá, že jste již vytvořili hello virtuálních počítačů a že jsou všechny v rámci hello stejné cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b7f57-115">This procedure assumes that you have already created hello virtual machines and that they are all within hello same cloud service.</span></span>

<span data-ttu-id="b7f57-116">**tooconfigure sadu Vyrovnávání zatížení sítě pro virtuální počítače**</span><span class="sxs-lookup"><span data-stu-id="b7f57-116">**tooconfigure a load-balanced set for virtual machines**</span></span>

1. <span data-ttu-id="b7f57-117">V hello portál Azure classic, klikněte na **virtuální počítače**a potom klikněte na název hello virtuálního počítače v sadě hello Vyrovnávání zatížení sítě.</span><span class="sxs-lookup"><span data-stu-id="b7f57-117">In hello Azure classic portal, click **Virtual Machines**, and then click hello name of a virtual machine in hello load-balanced set.</span></span>
2. <span data-ttu-id="b7f57-118">Klikněte na **Koncové body** a potom na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="b7f57-118">Click **Endpoints**, and then click **Add**.</span></span>
3. <span data-ttu-id="b7f57-119">Na hello **přidat se koncový bod tooa virtuální počítač** klikněte na šipku vpravo hello.</span><span class="sxs-lookup"><span data-stu-id="b7f57-119">On hello **Add an endpoint tooa virtual machine** page, click hello right arrow.</span></span>
4. <span data-ttu-id="b7f57-120">Na hello **zadejte podrobnosti hello koncového bodu hello** stránky:</span><span class="sxs-lookup"><span data-stu-id="b7f57-120">On hello **Specify hello details of hello endpoint** page:</span></span>

   * <span data-ttu-id="b7f57-121">V **název**, zadejte název pro koncový bod hello nebo vyberte název hello ze seznamu předdefinovaných koncové body pro běžné protokoly hello.</span><span class="sxs-lookup"><span data-stu-id="b7f57-121">In **Name**, type a name for hello endpoint or select hello name from hello list of predefined endpoints for common protocols.</span></span>
   * <span data-ttu-id="b7f57-122">V **protokol**, vyberte protokol hello vyžadované hello typu koncového bodu, TCP nebo UDP, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="b7f57-122">In **Protocol**, select hello protocol required by hello type of endpoint, either TCP or UDP, as needed.</span></span>
   * <span data-ttu-id="b7f57-123">V **Port veřejné a privátní Port**, zadejte hello čísla portů, která chcete hello toouse virtuální počítač podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="b7f57-123">In **Public Port and Private Port**, type hello port numbers that you want hello virtual machine toouse, as needed.</span></span> <span data-ttu-id="b7f57-124">Můžete vytvořit privátní port hello a pravidel brány firewall na přenosy dat virtuálních počítačů tooredirect hello způsobem, který je vhodný pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b7f57-124">You can use hello private port and firewall rules on hello virtual machine tooredirect traffic in a way that is appropriate for your application.</span></span> <span data-ttu-id="b7f57-125">stejné jako veřejný port hello můžete hello Hello privátní port.</span><span class="sxs-lookup"><span data-stu-id="b7f57-125">hello private port can be hello same as hello public port.</span></span> <span data-ttu-id="b7f57-126">Například pro koncový bod pro provoz webu (HTTP), může přiřadit port 80 tooboth hello veřejné a privátní port.</span><span class="sxs-lookup"><span data-stu-id="b7f57-126">For example, for an endpoint for web (HTTP) traffic, you could assign port 80 tooboth hello public and private port.</span></span>

5. <span data-ttu-id="b7f57-127">Vyberte **vytvořit sadu s vyrovnáváním zatížení**a potom klikněte na šipku vpravo hello.</span><span class="sxs-lookup"><span data-stu-id="b7f57-127">Select **Create a load-balanced set**, and then click hello right arrow.</span></span>
6. <span data-ttu-id="b7f57-128">Na hello **konfigurace sady vyrovnáváním zatížení hello** stránky, zadejte název pro sadu hello Vyrovnávání zatížení sítě a pak mu přiřaďte hodnoty hello pro chování testu hello Vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="b7f57-128">On hello **Configure hello load-balanced set** page, type a name for hello load-balanced set, and then assign hello values for probe behavior of hello Azure Load Balancer.</span></span> <span data-ttu-id="b7f57-129">Hello Vyrovnávání zatížení používá sondy toodetermine, pokud jsou k dispozici tooreceive příchozí provoz hello virtuální počítače ve sady hello vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="b7f57-129">hello Load Balancer uses probes toodetermine if hello virtual machines in hello load-balanced set are available tooreceive incoming traffic.</span></span>
7. <span data-ttu-id="b7f57-130">Klikněte na tlačítko hello zaškrtnutí toocreate hello Vyrovnávání zatížení sítě endpoint.</span><span class="sxs-lookup"><span data-stu-id="b7f57-130">Click hello check mark toocreate hello load-balanced endpoint.</span></span> <span data-ttu-id="b7f57-131">Zobrazí se **Ano** v hello **název skupiny s vyrovnáváním zatížení** sloupec hello **koncové body** stránku hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b7f57-131">You will see **Yes** in hello **Load-balanced set name** column of hello **Endpoints** page for hello virtual machine.</span></span>
8. <span data-ttu-id="b7f57-132">Hello portálu, klikněte na tlačítko **virtuální počítače**, klikněte na název hello další virtuálního počítače v sadě hello Vyrovnávání zatížení sítě, klikněte na **koncové body**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="b7f57-132">In hello portal, click **Virtual Machines**, click hello name of an additional virtual machine in hello load-balanced set, click **Endpoints**, and then click **Add**.</span></span>
9. <span data-ttu-id="b7f57-133">Na hello **přidat se koncový bod tooa virtuální počítač** klikněte na tlačítko **přidat koncový bod tooan existující Vyrovnávání zatížení sítě sady**, vyberte název hello sady hello Vyrovnávání zatížení sítě a potom klikněte na šipku vpravo hello.</span><span class="sxs-lookup"><span data-stu-id="b7f57-133">On hello **Add an endpoint tooa virtual machine** page, click **Add endpoint tooan existing load-balanced set**, select hello name of hello load-balanced set, and then click hello right arrow.</span></span>
10. <span data-ttu-id="b7f57-134">Na hello **zadejte podrobnosti hello koncového bodu hello** stránky, zadejte název pro koncový bod hello a pak klikněte na tlačítko zaškrtnutí hello.</span><span class="sxs-lookup"><span data-stu-id="b7f57-134">On hello **Specify hello details of hello endpoint** page, type a name for hello endpoint, and then click hello check mark.</span></span>

<span data-ttu-id="b7f57-135">Pro hello dalších virtuálních počítačů v sadě hello Vyrovnávání zatížení sítě opakujte kroky 8-10.</span><span class="sxs-lookup"><span data-stu-id="b7f57-135">For hello additional virtual machines in hello load-balanced set, repeat steps 8-10.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7f57-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b7f57-136">Next steps</span></span>

[<span data-ttu-id="b7f57-137">Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="b7f57-137">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="b7f57-138">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="b7f57-138">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="b7f57-139">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="b7f57-139">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
