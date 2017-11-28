---
title: "Vyrovnávání zatížení aaaCreate směřujících Internetu pro cloudové služby Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate přístupem Internetu pro vyrovnávání zátěže v modelu nasazení classic pro cloudové služby"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 0bb16f96-56a6-429f-88f5-0de2d0136756
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: d93cf76d417cbfc744cf07ba48c43a63cc14df69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a><span data-ttu-id="02b78-103">Začínáme vytvářet internetový nástroj pro vyrovnávání zatížení pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="02b78-103">Get started creating an Internet facing load balancer for cloud services</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="02b78-104">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="02b78-104">Azure classic portal</span></span>](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [<span data-ttu-id="02b78-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="02b78-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [<span data-ttu-id="02b78-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="02b78-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [<span data-ttu-id="02b78-107">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="02b78-107">Azure Cloud Services</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="02b78-108">Než začnete pracovat s prostředky Azure, je důležité toounderstand Azure aktuálně má dva modely nasazení: Azure Resource Manager a Klasický model.</span><span class="sxs-lookup"><span data-stu-id="02b78-108">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="02b78-109">Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že rozumíte [modelům nasazení a příslušným nástrojům](../azure-classic-rm.md).</span><span class="sxs-lookup"><span data-stu-id="02b78-109">Make sure you understand [deployment models and tools](../azure-classic-rm.md) before you work with any Azure resource.</span></span> <span data-ttu-id="02b78-110">Hello dokumentaci k různým nástrojům můžete zobrazit kliknutím na karty hello hello horní části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="02b78-110">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span> <span data-ttu-id="02b78-111">Tento článek se týká modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="02b78-111">This article covers hello classic deployment model.</span></span> <span data-ttu-id="02b78-112">Můžete také [zjistěte, jak toocreate přístupem Internetu pro vyrovnávání zátěže pomocí Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="02b78-112">You can also [Learn how toocreate an Internet facing load balancer using Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span></span>

<span data-ttu-id="02b78-113">Cloudové služby se automaticky nakonfigurují pomocí služby Vyrovnávání zatížení a lze přizpůsobit prostřednictvím modelu služby hello.</span><span class="sxs-lookup"><span data-stu-id="02b78-113">Cloud services are automatically configured with a load balancer and can be customized via hello service model.</span></span>

## <a name="create-a-load-balancer-using-hello-service-definition-file"></a><span data-ttu-id="02b78-114">Vytvořit nástroj pro vyrovnávání zatížení, pomocí souboru definice služby hello</span><span class="sxs-lookup"><span data-stu-id="02b78-114">Create a load balancer using hello service definition file</span></span>

<span data-ttu-id="02b78-115">Hello Azure SDK pro .NET 2.5 tooupdate mohou využívat cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="02b78-115">You can leverage hello Azure SDK for .NET 2.5 tooupdate your cloud service.</span></span> <span data-ttu-id="02b78-116">Nastavení pro koncový bod pro cloudové služby se provádí v hello [služby definice](https://msdn.microsoft.com/library/azure/gg557553.aspx) souboru .csdef.</span><span class="sxs-lookup"><span data-stu-id="02b78-116">Endpoint settings for cloud services are made in hello [service definition](https://msdn.microsoft.com/library/azure/gg557553.aspx) .csdef file.</span></span>

<span data-ttu-id="02b78-117">Hello následující příklad ukazuje, jak jsou nakonfigurované soubor servicedefinition.csdef pro nasazení cloudu:</span><span class="sxs-lookup"><span data-stu-id="02b78-117">hello following example shows how a servicedefinition.csdef file for a cloud deployment is configured:</span></span>

<span data-ttu-id="02b78-118">Kontrola hello fragment souboru .csdef hello generované nasazení cloudu, uvidíte hello externí koncovým bodem nakonfigurovaným toouse porty HTTP na portu 10000, 10001 a 10002.</span><span class="sxs-lookup"><span data-stu-id="02b78-118">Checking hello snippet for hello .csdef file generated by a cloud deployment, you can see hello external endpoint configured toouse ports HTTP on port 10000, 10001, and 10002.</span></span>

```xml
<ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
<Endpoints>
    <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
    <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
    <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

    <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

    <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
        <AllocatePublicPortFrom>
            <FixedPortRange min=“10110” max=“10120“  />
        </AllocatePublicPortFrom>
    </InstanceInputEndpoint>
    <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
</Endpoints>
    </WorkerRole>
</ServiceDefinition>
```

## <a name="check-load-balancer-health-status-for-cloud-services"></a><span data-ttu-id="02b78-119">Kontrola stavu nástroje pro vyrovnávání zatížení pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="02b78-119">Check load balancer health status for cloud services</span></span>

<span data-ttu-id="02b78-120">Hello následuje příklad test stavu:</span><span class="sxs-lookup"><span data-stu-id="02b78-120">hello following is an example of a health probe:</span></span>

```xml
<LoadBalancerProbes>
    <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
</LoadBalancerProbes>
```

<span data-ttu-id="02b78-121">Hello nástroj pro vyrovnávání zatížení kombinuje hello koncového bodu hello hello informací a toocreate hello test adresy URL ve formě hello `http://{DIP of VM}:80/Probe.aspx` který lze použít tooquery hello stavu služby hello.</span><span class="sxs-lookup"><span data-stu-id="02b78-121">hello load balancer combines hello information of hello endpoint and hello information of hello probe toocreate a URL in hello form of `http://{DIP of VM}:80/Probe.aspx` that can be used tooquery hello health of hello service.</span></span>

<span data-ttu-id="02b78-122">Hello služby rozpozná pravidelné sondy z hello stejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="02b78-122">hello service detects periodic probes from hello same IP address.</span></span> <span data-ttu-id="02b78-123">Toto je hello stavu zkušebního požadavku pocházející od hostitele hello hello uzlu, kde je spuštěný virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="02b78-123">This is hello health probe request coming from hello host of hello node where hello virtual machine is running.</span></span> <span data-ttu-id="02b78-124">Služba Hello má toorespond s stavový kód HTTP 200 pro tooassume nástroje pro vyrovnávání zatížení hello, že služba hello je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="02b78-124">hello service has toorespond with a HTTP 200 status code for hello load balancer tooassume that hello service is healthy.</span></span> <span data-ttu-id="02b78-125">Další stav protokolu HTTP kódu (například 503) přímo trvá hello virtuální počítač ze otočení.</span><span class="sxs-lookup"><span data-stu-id="02b78-125">Any other HTTP status code (for example 503) directly takes hello virtual machine out of rotation.</span></span>

<span data-ttu-id="02b78-126">definice testu Hello také řídí hello četnost kontroly hello.</span><span class="sxs-lookup"><span data-stu-id="02b78-126">hello probe definition also controls hello frequency of hello probe.</span></span> <span data-ttu-id="02b78-127">V našem případě výše je nástroj pro vyrovnávání zatížení hello zjišťování hello koncového bodu každých 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="02b78-127">In our case above, hello load balancer is probing hello endpoint every 5 secs.</span></span> <span data-ttu-id="02b78-128">Pokud není žádná kladná odpověď pro 10 sekund (dva intervaly testu), test hello se předpokládá, že dolů a hello virtuálního počítače se provede mimo otočení.</span><span class="sxs-lookup"><span data-stu-id="02b78-128">If no positive answer is received for 10 secs (two probe intervals), hello probe is assumed down, and hello virtual machine is taken out of rotation.</span></span> <span data-ttu-id="02b78-129">Podobně pokud je služba hello mimo otočení a přijetí kladná odpověď, hello služby je vrátit zpět toorotation hned.</span><span class="sxs-lookup"><span data-stu-id="02b78-129">Similarly, if hello service is out of rotation and a positive answer is received, hello service is put back toorotation right away.</span></span> <span data-ttu-id="02b78-130">Pokud služba hello je kolísal mezi v pořádku a není v pořádku, nástroj pro vyrovnávání zatížení hello můžete rozhodnout toodelay hello umístění nových hello služby back toorotation, dokud byla pro počet sondy v pořádku.</span><span class="sxs-lookup"><span data-stu-id="02b78-130">If hello service is fluctuating between healthy and unhealthy, hello load balancer can decide toodelay hello re-introduction of hello service back toorotation until it has been healthy for a number of probes.</span></span>

<span data-ttu-id="02b78-131">Vyhledejte hello služby definice schématu hello [test stavu](https://msdn.microsoft.com/library/azure/jj151530.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="02b78-131">Check hello service definition schema for hello [health probe](https://msdn.microsoft.com/library/azure/jj151530.aspx) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="02b78-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="02b78-132">Next steps</span></span>

[<span data-ttu-id="02b78-133">Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="02b78-133">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="02b78-134">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="02b78-134">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="02b78-135">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="02b78-135">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

