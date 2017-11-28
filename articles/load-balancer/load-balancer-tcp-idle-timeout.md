---
title: "časový limit nečinnosti TCP nástroje pro vyrovnávání zatížení aaaConfigure | Microsoft Docs"
description: "Nakonfigurujte časový limit nečinnosti TCP nástroje pro vyrovnávání zatížení"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: 4625c6a8-5725-47ce-81db-4fa3bd055891
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 2bf0704b891f708e0a5bd7aa827441930f51cfaf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a><span data-ttu-id="2204e-103">Konfigurace nastavení časového limitu nečinnosti TCP pro službu Vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="2204e-103">Configure TCP idle timeout settings for Azure Load Balancer</span></span>

<span data-ttu-id="2204e-104">Ve výchozí konfiguraci Vyrovnávání zatížení Azure má časový limit nečinnosti nastavení 4 minut.</span><span class="sxs-lookup"><span data-stu-id="2204e-104">In its default configuration, Azure Load Balancer has an idle timeout setting of 4 minutes.</span></span> <span data-ttu-id="2204e-105">Pokud je delší než hodnota časového limitu hello určité době nečinnosti, není zaručeno, že hello TCP nebo HTTP relace zachovaný mezi klientem hello a cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="2204e-105">If a period of inactivity is longer than hello timeout value, there's no guarantee that hello TCP or HTTP session is maintained between hello client and your cloud service.</span></span>

<span data-ttu-id="2204e-106">Při hello připojení je ukončeno, klientská aplikace může zobrazit hello následující chybová zpráva: "hello základní připojení bylo ukončeno: připojení, který byl očekáván toobe zachovány zachování připojení bylo ukončeno serverem hello."</span><span class="sxs-lookup"><span data-stu-id="2204e-106">When hello connection is closed, your client application may receive hello following error message: "hello underlying connection was closed: A connection that was expected toobe kept alive was closed by hello server."</span></span>

<span data-ttu-id="2204e-107">Běžnou praxí je toouse udržování připojení TCP.</span><span class="sxs-lookup"><span data-stu-id="2204e-107">A common practice is toouse a TCP keep-alive.</span></span> <span data-ttu-id="2204e-108">Tento postup zachová připojení hello active delší dobu.</span><span class="sxs-lookup"><span data-stu-id="2204e-108">This practice keeps hello connection active for a longer period.</span></span> <span data-ttu-id="2204e-109">Další informace najdete v tématu tyto [.NET příklady](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx).</span><span class="sxs-lookup"><span data-stu-id="2204e-109">For more information, see these [.NET examples](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx).</span></span> <span data-ttu-id="2204e-110">S udržování připojení povolena, jsou pakety odesílány době nečinnosti hello připojení.</span><span class="sxs-lookup"><span data-stu-id="2204e-110">With keep-alive enabled, packets are sent during periods of inactivity on hello connection.</span></span> <span data-ttu-id="2204e-111">Tyto udržovací pakety zajistěte, aby hodnota časového limitu nečinnosti hello není přístupný a se zachová připojení hello na dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="2204e-111">These keep-alive packets ensure that hello idle timeout value is never reached and hello connection is maintained for a long period.</span></span>

<span data-ttu-id="2204e-112">Toto nastavení funguje pro příchozí připojení pouze.</span><span class="sxs-lookup"><span data-stu-id="2204e-112">This setting works for inbound connections only.</span></span> <span data-ttu-id="2204e-113">tooavoid ztráta hello připojení, je nutné nakonfigurovat hello udržování připojení protokolu TCP s intervalem menší než hello časový limit nečinnosti nastavení, nebo zvyšte hello časový limit nečinnosti hodnota.</span><span class="sxs-lookup"><span data-stu-id="2204e-113">tooavoid losing hello connection, you must configure hello TCP keep-alive with an interval less than hello idle timeout setting or increase hello idle timeout value.</span></span> <span data-ttu-id="2204e-114">toosupport takových scénářů přidali jsme podporu pro konfigurovat časový limit nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="2204e-114">toosupport such scenarios, we've added support for a configurable idle timeout.</span></span> <span data-ttu-id="2204e-115">Nyní ji můžete nastavit po dobu 4 minuty too30.</span><span class="sxs-lookup"><span data-stu-id="2204e-115">You can now set it for a duration of 4 too30 minutes.</span></span>

<span data-ttu-id="2204e-116">Udržování připojení TCP funguje dobře pro scénáře, kdy výdrž baterie není omezení.</span><span class="sxs-lookup"><span data-stu-id="2204e-116">TCP keep-alive works well for scenarios where battery life is not a constraint.</span></span> <span data-ttu-id="2204e-117">Není doporučeno pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="2204e-117">It is not recommended for mobile applications.</span></span> <span data-ttu-id="2204e-118">Používání udržování v mobilní aplikaci TCP můžou vyprázdnit baterie zařízení hello pouze rychlejší.</span><span class="sxs-lookup"><span data-stu-id="2204e-118">Using a TCP keep-alive in a mobile application can drain hello device battery faster.</span></span>

![Vypršení časového limitu TCP](./media/load-balancer-tcp-idle-timeout/image1.png)

<span data-ttu-id="2204e-120">Hello následující části popisují, jak toochange nečinnosti nastavení časového limitu v virtuální počítače a cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="2204e-120">hello following sections describe how toochange idle timeout settings in virtual machines and cloud services.</span></span>

## <a name="configure-hello-tcp-timeout-for-your-instance-level-public-ip-too15-minutes"></a><span data-ttu-id="2204e-121">Konfigurace hello TCP vypršení časového limitu pro vaše instance úrovni veřejné IP too15 minut</span><span class="sxs-lookup"><span data-stu-id="2204e-121">Configure hello TCP timeout for your instance-level public IP too15 minutes</span></span>

```powershell
Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15
```

<span data-ttu-id="2204e-122">Parametr `IdleTimeoutInMinutes` je volitelný.</span><span class="sxs-lookup"><span data-stu-id="2204e-122">`IdleTimeoutInMinutes` is optional.</span></span> <span data-ttu-id="2204e-123">Pokud není nastaven, hello výchozí časový limit je 4 minuty.</span><span class="sxs-lookup"><span data-stu-id="2204e-123">If it is not set, hello default timeout is 4 minutes.</span></span> <span data-ttu-id="2204e-124">rozsah Hello přijatelný časový limit je 4 minuty too30.</span><span class="sxs-lookup"><span data-stu-id="2204e-124">hello acceptable timeout range is 4 too30 minutes.</span></span>

## <a name="set-hello-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a><span data-ttu-id="2204e-125">Při vytváření koncový bod Azure na virtuálním počítači nastavit časový limit nečinnosti hello</span><span class="sxs-lookup"><span data-stu-id="2204e-125">Set hello idle timeout when creating an Azure endpoint on a virtual machine</span></span>

<span data-ttu-id="2204e-126">toochange hello časový limit nastavení pro koncový bod, použijte následující hello:</span><span class="sxs-lookup"><span data-stu-id="2204e-126">toochange hello timeout setting for an endpoint, use hello following:</span></span>

```powershell
Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM
```

<span data-ttu-id="2204e-127">tooretrieve konfiguraci časový limit nečinnosti hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2204e-127">tooretrieve your idle timeout configuration, use hello following command:</span></span>

    PS C:\> Get-AzureVM -ServiceName "MyService" -Name "MyVM" | Get-AzureEndpoint
    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15

## <a name="set-hello-tcp-timeout-on-a-load-balanced-endpoint-set"></a><span data-ttu-id="2204e-128">Nastavit časový limit TCP hello na sady koncových bodů s vyrovnáváním zatížení</span><span class="sxs-lookup"><span data-stu-id="2204e-128">Set hello TCP timeout on a load-balanced endpoint set</span></span>

<span data-ttu-id="2204e-129">Pokud koncové body jsou součástí sady koncových bodů s vyrovnáváním zatížení, hello TCP vypršení časového limitu musí být nastavena na sady koncových bodů s vyrovnáváním zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="2204e-129">If endpoints are part of a load-balanced endpoint set, hello TCP timeout must be set on hello load-balanced endpoint set.</span></span> <span data-ttu-id="2204e-130">Například:</span><span class="sxs-lookup"><span data-stu-id="2204e-130">For example:</span></span>

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15
```

## <a name="change-timeout-settings-for-cloud-services"></a><span data-ttu-id="2204e-131">Změna nastavení časového limitu pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="2204e-131">Change timeout settings for cloud services</span></span>

<span data-ttu-id="2204e-132">Tooupdate hello Azure SDK můžete použít cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="2204e-132">You can use hello Azure SDK tooupdate your cloud service.</span></span> <span data-ttu-id="2204e-133">Vytváření nastavení koncového bodu pro cloudové služby v souboru .csdef hello.</span><span class="sxs-lookup"><span data-stu-id="2204e-133">You make endpoint settings for cloud services in hello .csdef file.</span></span> <span data-ttu-id="2204e-134">Aktualizace hello časového limitu TCP pro nasazení cloudové služby vyžaduje nasazení upgradu.</span><span class="sxs-lookup"><span data-stu-id="2204e-134">Updating hello TCP timeout for deployment of a cloud service requires a deployment upgrade.</span></span> <span data-ttu-id="2204e-135">Výjimkou je, pokud je časový limit TCP hello zadat jenom pro veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="2204e-135">An exception is if hello TCP timeout is specified only for a public IP.</span></span> <span data-ttu-id="2204e-136">Nastavení veřejné IP adresy jsou v souboru .cscfg hello a můžete je aktualizovat prostřednictvím aktualizací nasazení a upgrade.</span><span class="sxs-lookup"><span data-stu-id="2204e-136">Public IP settings are in hello .cscfg file, and you can update them through deployment update and upgrade.</span></span>

<span data-ttu-id="2204e-137">Hello .csdef změny pro koncový bod nastavení jsou tyto:</span><span class="sxs-lookup"><span data-stu-id="2204e-137">hello .csdef changes for endpoint settings are:</span></span>

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
    </Endpoints>
</WorkerRole>
```

<span data-ttu-id="2204e-138">změny Hello .cscfg pro nastavení limitu hello na veřejné IP adresy jsou tyto:</span><span class="sxs-lookup"><span data-stu-id="2204e-138">hello .cscfg changes for hello timeout setting on public IPs are:</span></span>

```xml
<NetworkConfiguration>
    <VirtualNetworkSite name="VNet"/>
    <AddressAssignments>
    <InstanceAddress roleName="VMRolePersisted">
    <PublicIPs>
        <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
    </PublicIPs>
    </InstanceAddress>
    </AddressAssignments>
</NetworkConfiguration>
```

## <a name="rest-api-example"></a><span data-ttu-id="2204e-139">Příklad REST API</span><span class="sxs-lookup"><span data-stu-id="2204e-139">REST API example</span></span>

<span data-ttu-id="2204e-140">Časový limit nečinnosti TCP hello můžete nakonfigurovat pomocí služby hello rozhraní API pro správu.</span><span class="sxs-lookup"><span data-stu-id="2204e-140">You can configure hello TCP idle timeout by using hello service management API.</span></span> <span data-ttu-id="2204e-141">Ujistěte se, že hello `x-ms-version` záhlaví nastavena tooversion `2014-06-01` nebo novější.</span><span class="sxs-lookup"><span data-stu-id="2204e-141">Make sure that hello `x-ms-version` header is set tooversion `2014-06-01` or later.</span></span> <span data-ttu-id="2204e-142">Aktualizace konfigurace hello hello zadat Vyrovnávání zatížení sítě vstupních koncových bodů na všechny virtuální počítače v nasazení.</span><span class="sxs-lookup"><span data-stu-id="2204e-142">Update hello configuration of hello specified load-balanced input endpoints on all virtual machines in a deployment.</span></span>

### <a name="request"></a><span data-ttu-id="2204e-143">Žádost</span><span class="sxs-lookup"><span data-stu-id="2204e-143">Request</span></span>

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a><span data-ttu-id="2204e-144">Odpověď</span><span class="sxs-lookup"><span data-stu-id="2204e-144">Response</span></span>

```xml
<LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
    <InputEndpoint>
    <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
    <LocalPort>local-port-number</LocalPort>
    <Port>external-port-number</Port>
    <LoadBalancerProbe>
        <Path>path-of-probe</Path>
        <Port>port-assigned-to-probe</Port>
        <Protocol>probe-protocol</Protocol>
        <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
        <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
    </LoadBalancerProbe>
    <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
    <Protocol>endpoint-protocol</Protocol>
    <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
    <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
    <EndpointACL>
        <Rules>
        <Rule>
            <Order>priority-of-the-rule</Order>
            <Action>permit-rule</Action>
            <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
            <Description>description-of-the-rule</Description>
        </Rule>
        </Rules>
    </EndpointACL>
    </InputEndpoint>
</LoadBalancedEndpointList>
```

## <a name="next-steps"></a><span data-ttu-id="2204e-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2204e-145">Next steps</span></span>

[<span data-ttu-id="2204e-146">Přehled nástroje pro vyrovnávání zatížení interní</span><span class="sxs-lookup"><span data-stu-id="2204e-146">Internal load balancer overview</span></span>](load-balancer-internal-overview.md)

[<span data-ttu-id="2204e-147">Začněte konfiguraci Vyrovnávání zatížení internetového</span><span class="sxs-lookup"><span data-stu-id="2204e-147">Get started configuring an Internet-facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="2204e-148">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="2204e-148">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)
