---
title: "aaaCreate interní nástroj pro Azure Cloud Services | Microsoft Docs"
description: "Zjistěte, jak toocreate na interní nástroj pro vyrovnávání pomocí prostředí PowerShell v modelu nasazení classic hello zatížení"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 57966056-0f46-4f95-a295-483ca1ad135d
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: fe7975bca7bec3248626b0ad0fad6823e278ade2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a><span data-ttu-id="b3b58-103">Začínáme vytvářet interní nástroj pro vyrovnávání zatížení (Classic) pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="b3b58-103">Get started creating an internal load balancer (classic) for cloud services</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b3b58-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b3b58-104">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [<span data-ttu-id="b3b58-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b3b58-105">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [<span data-ttu-id="b3b58-106">Cloudové služby</span><span class="sxs-lookup"><span data-stu-id="b3b58-106">Cloud services</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

> [!IMPORTANT]
> <span data-ttu-id="b3b58-107">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b3b58-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="b3b58-108">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="b3b58-108">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="b3b58-109">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="b3b58-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="b3b58-110">Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu Resource Manager hello](load-balancer-get-started-ilb-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b3b58-110">Learn how too[perform these steps using hello Resource Manager model](load-balancer-get-started-ilb-arm-ps.md).</span></span>

## <a name="configure-internal-load-balancer-for-cloud-services"></a><span data-ttu-id="b3b58-111">Konfigurace interního nástroje pro vyrovnávání zatížení pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="b3b58-111">Configure internal load balancer for cloud services</span></span>

<span data-ttu-id="b3b58-112">Interní nástroj pro vyrovnávání zatížení je podporován pro virtuální počítače i cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b3b58-112">Internal load balancer is supported for both virtual machines and cloud services.</span></span> <span data-ttu-id="b3b58-113">Koncový bod vyrovnávání interní služby load vytvořené v Cloudová služba, která je mimo regionální virtuální síť, budou přístupné pouze v rámci hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b3b58-113">An internal load balancer endpoint created in a cloud service that is outside a regional virtual network will be accessible only within hello cloud service.</span></span>

<span data-ttu-id="b3b58-114">Konfigurace služby Vyrovnávání zatížení pro vnitřní Hello má toobe nastavit při vytváření hello hello první nasazení v hello cloudové služby, jak je znázorněno v ukázce hello níže.</span><span class="sxs-lookup"><span data-stu-id="b3b58-114">hello internal load balancer configuration has toobe set during hello creation of hello first deployment in hello cloud service, as shown in hello sample below.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b3b58-115">Požadovaných toorun hello postup je toohave virtuální síti již vytvořené pro nasazení cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="b3b58-115">A prerequisite toorun hello steps below is toohave a virtual network already created for hello cloud deployment.</span></span> <span data-ttu-id="b3b58-116">Budete potřebovat hello virtuální sítě názvem a podsíť název toocreate hello interní Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="b3b58-116">You will need hello virtual network name and subnet name toocreate hello Internal Load Balancing.</span></span>

### <a name="step-1"></a><span data-ttu-id="b3b58-117">Krok 1</span><span class="sxs-lookup"><span data-stu-id="b3b58-117">Step 1</span></span>

<span data-ttu-id="b3b58-118">Otevřete konfigurační soubor služby hello (.cscfg) pro vaše nasazení cloudu v sadě Visual Studio a přidejte následující části toocreate hello interní Vyrovnávání zatížení v rámci hello poslední hello "`</Role>`" položku pro konfiguraci sítě hello.</span><span class="sxs-lookup"><span data-stu-id="b3b58-118">Open hello service configuration file (.cscfg) for your cloud deployment in Visual Studio and add hello following section toocreate hello Internal Load Balancing under hello last "`</Role>`" item for hello network configuration.</span></span>

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="name of hello load balancer">
        <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

<span data-ttu-id="b3b58-119">Přidejme hello hodnoty pro hello sítě konfigurační soubor tooshow jak bude vypadat.</span><span class="sxs-lookup"><span data-stu-id="b3b58-119">Let's add hello values for hello network configuration file tooshow how it will look.</span></span> <span data-ttu-id="b3b58-120">V příkladu hello předpokládá, že jste vytvořili virtuální síť s podsítí 10.0.0.0/24 názvem test_subnet a statická IP adresa 10.0.0.4 názvem "test_vnet".</span><span class="sxs-lookup"><span data-stu-id="b3b58-120">In hello example, assume you created a VNet called "test_vnet" with a subnet 10.0.0.0/24 called test_subnet and a static IP 10.0.0.4.</span></span> <span data-ttu-id="b3b58-121">Nástroj pro vyrovnávání zatížení Hello budou pojmenované testLB.</span><span class="sxs-lookup"><span data-stu-id="b3b58-121">hello load balancer will be named testLB.</span></span>

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="testLB">
        <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

<span data-ttu-id="b3b58-122">Další informace o schématu pro vyrovnávání zatížení hello najdete v tématu [nástroj pro vyrovnávání zatížení přidat](https://msdn.microsoft.com/library/azure/dn722411.aspx).</span><span class="sxs-lookup"><span data-stu-id="b3b58-122">For more information about hello load balancer schema, see [Add load balancer](https://msdn.microsoft.com/library/azure/dn722411.aspx).</span></span>

### <a name="step-2"></a><span data-ttu-id="b3b58-123">Krok 2</span><span class="sxs-lookup"><span data-stu-id="b3b58-123">Step 2</span></span>

<span data-ttu-id="b3b58-124">Změňte hello služby definice (.csdef) souboru tooadd koncové body toohello interní Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="b3b58-124">Change hello service definition (.csdef) file tooadd endpoints toohello Internal Load Balancing.</span></span> <span data-ttu-id="b3b58-125">Hello okamžiku se vytvoří instanci role, souboru definice služby hello přidá toohello instancí role hello interní Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="b3b58-125">hello moment a role instance is created, hello service definition file will add hello role instances toohello Internal Load Balancing.</span></span>

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
    </Endpoints>
</WorkerRole>
```

<span data-ttu-id="b3b58-126">Následující hello stejné hodnoty z hello příkladu výše přidejme souboru definice služby toohello hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="b3b58-126">Following hello same values from hello example above, let's add hello values toohello service definition file.</span></span>

```xml
<WorkerRole name="WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
    </Endpoints>
</WorkerRole>
```

<span data-ttu-id="b3b58-127">Hello síťových přenosů bude Vyrovnávané pomocí vyrovnávání zatížení testLB hello používá port 80 pro příchozí požadavky a odesílání tooworker instance rolí také na portu 80.</span><span class="sxs-lookup"><span data-stu-id="b3b58-127">hello network traffic will be load balanced using hello testLB load balancer using port 80 for incoming requests, sending tooworker role instances also on port 80.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b3b58-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b3b58-128">Next steps</span></span>

[<span data-ttu-id="b3b58-129">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení pomocí spřažení se zdrojovou IP adresou</span><span class="sxs-lookup"><span data-stu-id="b3b58-129">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="b3b58-130">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="b3b58-130">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

