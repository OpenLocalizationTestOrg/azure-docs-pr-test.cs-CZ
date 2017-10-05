---
title: "Nakonfigurujte časový limit nečinnosti TCP nástroje pro vyrovnávání zatížení | Microsoft Docs"
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
ms.openlocfilehash: d040fe6580b8ae777aecc9dd385ed33861530c38
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a><span data-ttu-id="b9744-103">Konfigurace nastavení časového limitu nečinnosti TCP pro službu Vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="b9744-103">Configure TCP idle timeout settings for Azure Load Balancer</span></span>

<span data-ttu-id="b9744-104">Ve výchozí konfiguraci Vyrovnávání zatížení Azure má časový limit nečinnosti nastavení 4 minut.</span><span class="sxs-lookup"><span data-stu-id="b9744-104">In its default configuration, Azure Load Balancer has an idle timeout setting of 4 minutes.</span></span> <span data-ttu-id="b9744-105">Je-li určité době nečinnosti delší než hodnota časového limitu, neexistuje žádná záruka, který se spravuje relace TCP nebo HTTP mezi klientem a cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b9744-105">If a period of inactivity is longer than the timeout value, there's no guarantee that the TCP or HTTP session is maintained between the client and your cloud service.</span></span>

<span data-ttu-id="b9744-106">Když se připojení uzavře, klientské aplikace může zobrazit následující chybová zpráva: "základní připojení bylo ukončeno: byl očekáván být zachováno připojení bylo ukončeno serverem."</span><span class="sxs-lookup"><span data-stu-id="b9744-106">When the connection is closed, your client application may receive the following error message: "The underlying connection was closed: A connection that was expected to be kept alive was closed by the server."</span></span>

<span data-ttu-id="b9744-107">Běžnou praxí je použít udržování připojení TCP.</span><span class="sxs-lookup"><span data-stu-id="b9744-107">A common practice is to use a TCP keep-alive.</span></span> <span data-ttu-id="b9744-108">Tento postup zachová připojení active delší dobu.</span><span class="sxs-lookup"><span data-stu-id="b9744-108">This practice keeps the connection active for a longer period.</span></span> <span data-ttu-id="b9744-109">Další informace najdete v tématu tyto [.NET příklady](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9744-109">For more information, see these [.NET examples](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx).</span></span> <span data-ttu-id="b9744-110">S udržování připojení povolena, jsou pakety odesílány době nečinnosti na připojení.</span><span class="sxs-lookup"><span data-stu-id="b9744-110">With keep-alive enabled, packets are sent during periods of inactivity on the connection.</span></span> <span data-ttu-id="b9744-111">Tyto udržovací pakety zajistěte, aby hodnota časového limitu nečinnosti není přístupný a se zachová připojení na dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="b9744-111">These keep-alive packets ensure that the idle timeout value is never reached and the connection is maintained for a long period.</span></span>

<span data-ttu-id="b9744-112">Toto nastavení funguje pro příchozí připojení pouze.</span><span class="sxs-lookup"><span data-stu-id="b9744-112">This setting works for inbound connections only.</span></span> <span data-ttu-id="b9744-113">Aby nedošlo ke ztrátě připojení, musíte nakonfigurovat interval menší než nastavení pro časový limit nečinnosti udržování připojení TCP nebo zvyšte hodnotu časového limitu nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="b9744-113">To avoid losing the connection, you must configure the TCP keep-alive with an interval less than the idle timeout setting or increase the idle timeout value.</span></span> <span data-ttu-id="b9744-114">Pro podporu takových scénářů, přidali jsme podporu pro konfigurovat časový limit nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="b9744-114">To support such scenarios, we've added support for a configurable idle timeout.</span></span> <span data-ttu-id="b9744-115">Nyní ji můžete nastavit po dobu 4 až 30 minut.</span><span class="sxs-lookup"><span data-stu-id="b9744-115">You can now set it for a duration of 4 to 30 minutes.</span></span>

<span data-ttu-id="b9744-116">Udržování připojení TCP funguje dobře pro scénáře, kdy výdrž baterie není omezení.</span><span class="sxs-lookup"><span data-stu-id="b9744-116">TCP keep-alive works well for scenarios where battery life is not a constraint.</span></span> <span data-ttu-id="b9744-117">Není doporučeno pro mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="b9744-117">It is not recommended for mobile applications.</span></span> <span data-ttu-id="b9744-118">Používání udržování v mobilní aplikaci TCP můžou vyprázdnit baterie zařízení pouze rychlejší.</span><span class="sxs-lookup"><span data-stu-id="b9744-118">Using a TCP keep-alive in a mobile application can drain the device battery faster.</span></span>

![Vypršení časového limitu TCP](./media/load-balancer-tcp-idle-timeout/image1.png)

<span data-ttu-id="b9744-120">Následující části popisují, jak změnit nastavení časového limitu nečinnosti v virtuální počítače a cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b9744-120">The following sections describe how to change idle timeout settings in virtual machines and cloud services.</span></span>

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a><span data-ttu-id="b9744-121">Konfigurace časového limitu TCP pro vaše instance úrovni veřejné IP adresy na 15 minut</span><span class="sxs-lookup"><span data-stu-id="b9744-121">Configure the TCP timeout for your instance-level public IP to 15 minutes</span></span>

```powershell
Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15
```

<span data-ttu-id="b9744-122">Parametr `IdleTimeoutInMinutes` je volitelný.</span><span class="sxs-lookup"><span data-stu-id="b9744-122">`IdleTimeoutInMinutes` is optional.</span></span> <span data-ttu-id="b9744-123">Pokud není nastaven, je výchozí hodnota časového limitu 4 minuty.</span><span class="sxs-lookup"><span data-stu-id="b9744-123">If it is not set, the default timeout is 4 minutes.</span></span> <span data-ttu-id="b9744-124">Rozsah přijatelný časový limit je 4 až 30 minut.</span><span class="sxs-lookup"><span data-stu-id="b9744-124">The acceptable timeout range is 4 to 30 minutes.</span></span>

## <a name="set-the-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a><span data-ttu-id="b9744-125">Při vytváření koncový bod Azure na virtuálním počítači nastavit časový limit nečinnosti</span><span class="sxs-lookup"><span data-stu-id="b9744-125">Set the idle timeout when creating an Azure endpoint on a virtual machine</span></span>

<span data-ttu-id="b9744-126">Chcete-li změnit nastavení časového limitu pro koncový bod, použijte následující:</span><span class="sxs-lookup"><span data-stu-id="b9744-126">To change the timeout setting for an endpoint, use the following:</span></span>

```powershell
Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM
```

<span data-ttu-id="b9744-127">Pokud chcete načíst konfiguraci časového limitu nečinnosti, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b9744-127">To retrieve your idle timeout configuration, use the following command:</span></span>

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

## <a name="set-the-tcp-timeout-on-a-load-balanced-endpoint-set"></a><span data-ttu-id="b9744-128">Nastavit časový limit TCP na sady koncových bodů s vyrovnáváním zatížení</span><span class="sxs-lookup"><span data-stu-id="b9744-128">Set the TCP timeout on a load-balanced endpoint set</span></span>

<span data-ttu-id="b9744-129">Pokud koncové body jsou součástí sady koncových bodů s vyrovnáváním zatížení, vypršení časového limitu TCP musí být nastavena na sady koncových bodů s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="b9744-129">If endpoints are part of a load-balanced endpoint set, the TCP timeout must be set on the load-balanced endpoint set.</span></span> <span data-ttu-id="b9744-130">Například:</span><span class="sxs-lookup"><span data-stu-id="b9744-130">For example:</span></span>

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15
```

## <a name="change-timeout-settings-for-cloud-services"></a><span data-ttu-id="b9744-131">Změna nastavení časového limitu pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="b9744-131">Change timeout settings for cloud services</span></span>

<span data-ttu-id="b9744-132">Azure SDK můžete použít k aktualizaci cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="b9744-132">You can use the Azure SDK to update your cloud service.</span></span> <span data-ttu-id="b9744-133">Vytváření nastavení koncového bodu pro cloudové služby v souboru .csdef.</span><span class="sxs-lookup"><span data-stu-id="b9744-133">You make endpoint settings for cloud services in the .csdef file.</span></span> <span data-ttu-id="b9744-134">Aktualizace časového limitu TCP pro nasazení cloudové služby vyžaduje nasazení upgradu.</span><span class="sxs-lookup"><span data-stu-id="b9744-134">Updating the TCP timeout for deployment of a cloud service requires a deployment upgrade.</span></span> <span data-ttu-id="b9744-135">Výjimkou je, pokud je časový limit TCP zadat jenom pro veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="b9744-135">An exception is if the TCP timeout is specified only for a public IP.</span></span> <span data-ttu-id="b9744-136">Nastavení veřejné IP adresy jsou v souboru .cscfg a můžete je aktualizovat prostřednictvím aktualizací nasazení a upgrade.</span><span class="sxs-lookup"><span data-stu-id="b9744-136">Public IP settings are in the .cscfg file, and you can update them through deployment update and upgrade.</span></span>

<span data-ttu-id="b9744-137">Změny .csdef pro koncový bod nastavení jsou tyto:</span><span class="sxs-lookup"><span data-stu-id="b9744-137">The .csdef changes for endpoint settings are:</span></span>

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
    </Endpoints>
</WorkerRole>
```

<span data-ttu-id="b9744-138">Změny .cscfg pro nastavení limitu na veřejné IP adresy jsou tyto:</span><span class="sxs-lookup"><span data-stu-id="b9744-138">The .cscfg changes for the timeout setting on public IPs are:</span></span>

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

## <a name="rest-api-example"></a><span data-ttu-id="b9744-139">Příklad REST API</span><span class="sxs-lookup"><span data-stu-id="b9744-139">REST API example</span></span>

<span data-ttu-id="b9744-140">Časový limit nečinnosti TCP můžete nakonfigurovat pomocí rozhraní API správy služby.</span><span class="sxs-lookup"><span data-stu-id="b9744-140">You can configure the TCP idle timeout by using the service management API.</span></span> <span data-ttu-id="b9744-141">Ujistěte se, že `x-ms-version` záhlaví je nastaven na verzi `2014-06-01` nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b9744-141">Make sure that the `x-ms-version` header is set to version `2014-06-01` or later.</span></span> <span data-ttu-id="b9744-142">Aktualizujte konfiguraci zadané Vyrovnávání zatížení sítě vstupní koncové body na všechny virtuální počítače v nasazení.</span><span class="sxs-lookup"><span data-stu-id="b9744-142">Update the configuration of the specified load-balanced input endpoints on all virtual machines in a deployment.</span></span>

### <a name="request"></a><span data-ttu-id="b9744-143">Žádost</span><span class="sxs-lookup"><span data-stu-id="b9744-143">Request</span></span>

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a><span data-ttu-id="b9744-144">Odpověď</span><span class="sxs-lookup"><span data-stu-id="b9744-144">Response</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="b9744-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b9744-145">Next steps</span></span>

[<span data-ttu-id="b9744-146">Přehled nástroje pro vyrovnávání zatížení interní</span><span class="sxs-lookup"><span data-stu-id="b9744-146">Internal load balancer overview</span></span>](load-balancer-internal-overview.md)

[<span data-ttu-id="b9744-147">Začněte konfiguraci Vyrovnávání zatížení internetového</span><span class="sxs-lookup"><span data-stu-id="b9744-147">Get started configuring an Internet-facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="b9744-148">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="b9744-148">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)
