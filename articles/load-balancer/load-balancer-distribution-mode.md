---
title: "Konfigurace pro vyrovnávání zatížení režim distribuce | Microsoft Docs"
description: "Jak nakonfigurovat režim distribuce nástroje pro vyrovnávání zatížení Azure pro podporu spřažení IP zdroje"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: 7df27a4d-67a8-47d6-b73e-32c0c6206e6e
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 4cb000c8ee1bb2e267dc0813dab23a77a46080ce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-the-distribution-mode-for-load-balancer"></a><span data-ttu-id="0a1e7-103">Nakonfigurujte distribuční režim pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="0a1e7-103">Configure the distribution mode for load balancer</span></span>

## <a name="hash-based-distribution-mode"></a><span data-ttu-id="0a1e7-104">Režim distribuce na základě hodnoty hash</span><span class="sxs-lookup"><span data-stu-id="0a1e7-104">Hash-based distribution mode</span></span>

<span data-ttu-id="0a1e7-105">Výchozí distribuční algoritmus používá 5-řazené kolekce členů (zdrojové IP adresy, zdrojového portu, cílové adresy IP, cílový port, protokol typu) hodnotu hash pro mapování provoz na dostupné servery.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-105">The default distribution algorithm is a 5-tuple (source IP, source port, destination IP, destination port, protocol type) hash to map traffic to available servers.</span></span> <span data-ttu-id="0a1e7-106">Poskytuje věrnosti pouze v rámci relace přenosu.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-106">It provides stickiness only within a transport session.</span></span> <span data-ttu-id="0a1e7-107">Pakety ve stejné relaci se přesměruje na stejnou instanci datacenter IP (DIP) za vyrovnáváním zatížení koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-107">Packets in the same session will be directed to the same datacenter IP (DIP) instance behind the load balanced endpoint.</span></span> <span data-ttu-id="0a1e7-108">Když se klient spustí novou relaci ze stejné zdrojové IP adresy, zdrojového portu změny a způsobí, že provoz přejít k jinému koncovému bodu vyhrazené IP adresy.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-108">When the client starts a new session from the same source IP, the source port changes and causes the traffic to go to a different DIP endpoint.</span></span>

![Nástroj pro vyrovnávání zatížení na základě hodnoty hash](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

<span data-ttu-id="0a1e7-110">Obrázek 1-5-n-tice distribuce</span><span class="sxs-lookup"><span data-stu-id="0a1e7-110">Figure 1 - 5-tuple distribution</span></span>

## <a name="source-ip-affinity-mode"></a><span data-ttu-id="0a1e7-111">Režim spřažení IP zdroje</span><span class="sxs-lookup"><span data-stu-id="0a1e7-111">Source IP affinity mode</span></span>

<span data-ttu-id="0a1e7-112">Máme jiný režim distribuce se označuje jako zdroj IP spřažení (známou taky jako spřažení relace na spřažení IP klienta).</span><span class="sxs-lookup"><span data-stu-id="0a1e7-112">We have another distribution mode called Source IP Affinity (also known as session affinity or client IP affinity).</span></span> <span data-ttu-id="0a1e7-113">Azure nástroj pro vyrovnávání zatížení lze nakonfigurovat k využívání 2-n-tice (zdrojová adresa IP, cílovou IP adresu) nebo 3-n-tice (zdrojová adresa IP, cílové adresy IP, protokol) pro mapování provoz do dostupných serverů.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-113">Azure Load Balancer can be configured to use a 2-tuple (Source IP, Destination IP) or 3-tuple (Source IP, Destination IP, Protocol) to map traffic to the available servers.</span></span> <span data-ttu-id="0a1e7-114">Pomocí spřažení zdrojové IP adresy připojení inicializována z stejný klientský počítač přejde do stejné koncový bod vyhrazené IP adresy.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-114">By using Source IP affinity, connections initiated from the same client computer goes to the same DIP endpoint.</span></span>

<span data-ttu-id="0a1e7-115">Následující diagram znázorňuje konfiguraci 2 řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-115">The following diagram illustrates a 2-tuple configuration.</span></span> <span data-ttu-id="0a1e7-116">Všimněte si, jak 2-n-tice spustí pomocí nástroje pro vyrovnávání zatížení do virtuálního počítače 1 (VM1) které se potom zálohuje virtuálního počítače 2 a VM3.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-116">Notice how the 2-tuple runs through the load balancer to virtual machine 1 (VM1) which is then backed up by VM2 and VM3.</span></span>

![spřažení relace](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

<span data-ttu-id="0a1e7-118">Obrázek 2 – distribuční 2 řazené kolekce členů</span><span class="sxs-lookup"><span data-stu-id="0a1e7-118">Figure 2 - 2-tuple distribution</span></span>

<span data-ttu-id="0a1e7-119">Zdroj IP spřažení řeší nekompatibilitou nástroj pro vyrovnávání zatížení Azure a brány vzdálené plochy (RD).</span><span class="sxs-lookup"><span data-stu-id="0a1e7-119">Source IP affinity solves an incompatibility between the Azure Load Balancer and Remote Desktop (RD) Gateway.</span></span> <span data-ttu-id="0a1e7-120">Nyní můžete vytvořit farmy služby Brána VP v jednom cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-120">Now you can build an RD gateway farm in a single cloud service.</span></span>

<span data-ttu-id="0a1e7-121">Další scénář případu využití je média nahrávání, kde nahrávání dat se nakonfigurují UDP ale rovině řízení je dosaženo pomocí TCP:</span><span class="sxs-lookup"><span data-stu-id="0a1e7-121">Another use case scenario is media upload where the data upload happens through UDP but the control plane is achieved through TCP:</span></span>

* <span data-ttu-id="0a1e7-122">Klient nejprve zahájí relace TCP na skupinu s vyrovnáváním zatížení veřejnou adresu, získá přesměrován na konkrétní vyhrazené IP adresy v tomto kanálu zůstane aktivní, monitorování stavu připojení</span><span class="sxs-lookup"><span data-stu-id="0a1e7-122">A client first initiates a TCP session to the load balanced public address, gets directed to a specific DIP, this channel is left active to monitor the connection health</span></span>
* <span data-ttu-id="0a1e7-123">Novou relaci UDP ze stejné klientský počítač se zahájí na stejný veřejný koncový bod Vyrovnávání zatížení, zde očekává se, že toto připojení je také k přesměrování stejný koncový bod vyhrazené IP adresy jako bylo předchozí připojení TCP tak, aby média nahrát mohou být provedeny na vysokou propustnost při zachování také řídicí kanál prostřednictvím TCP.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-123">A new UDP session from the same client computer is initiated to the same load balanced public endpoint, the expectation here is that this connection is also directed to the same DIP endpoint as the previous TCP connection so that media upload can be executed at high throughput while also maintaining a control channel through TCP.</span></span>

> [!NOTE]
> <span data-ttu-id="0a1e7-124">Když se změní sady s vyrovnáváním zatížení (odebrání nebo přidání virtuálního počítače), distribuci požadavky klientů je přepočítávány.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-124">When a load-balanced set changes (removing or adding a virtual machine), the distribution of client requests is recomputed.</span></span> <span data-ttu-id="0a1e7-125">Nemůže záviset na nové připojení z existující klienti ukončení na stejném serveru.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-125">You cannot depend on new connections from existing clients ending up at the same server.</span></span> <span data-ttu-id="0a1e7-126">Kromě toho použití zdrojové IP adresy režim distribuce spřažení může způsobit jako nerovné distribučního provozu.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-126">Additionally, using source IP affinity distribution mode may cause an unequal distribution of traffic.</span></span> <span data-ttu-id="0a1e7-127">Klienti se systémem za proxy může považovat za jeden jedinečných klientských aplikací.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-127">Clients running behind proxies may be seen as one unique client application.</span></span>

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a><span data-ttu-id="0a1e7-128">Konfigurace nastavení spřažení zdrojové IP adresy pro službu Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="0a1e7-128">Configuring Source IP affinity settings for load balancer</span></span>

<span data-ttu-id="0a1e7-129">Pro virtuální počítače můžete použít PowerShell Chcete-li změnit nastavení časového limitu:</span><span class="sxs-lookup"><span data-stu-id="0a1e7-129">For virtual machines, you can use PowerShell to change timeout settings:</span></span>

<span data-ttu-id="0a1e7-130">Koncový bod Azure přidejte k virtuálnímu počítači a nastavte režim distribuce nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="0a1e7-130">Add an Azure endpoint to a Virtual Machine and set load balancer distribution mode</span></span>

```powershell
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

<span data-ttu-id="0a1e7-131">LoadBalancerDistribution může být nastaven na sourceIP pro 2-n-tice (zdrojová adresa IP, cílovou IP adresu) služby Vyrovnávání zatížení, sourceIPProtocol pro vyrovnávání zatížení 3-n-tice (zdrojová adresa IP, cílové adresy IP, protokol), nebo žádný Pokud chcete výchozí chování Vyrovnávání zatížení 5 řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-131">LoadBalancerDistribution can be set to sourceIP for 2-tuple (Source IP, Destination IP) load balancing, sourceIPProtocol for 3-tuple (Source IP, Destination IP, protocol) load balancing, or none if you want the default behavior of 5-tuple load balancing.</span></span>

<span data-ttu-id="0a1e7-132">Použijte následující postupy k načtení konfigurace aplikace endpoint zatížení vyrovnávání distribuční režimu:</span><span class="sxs-lookup"><span data-stu-id="0a1e7-132">Use the following to retrieve an endpoint load balancer distribution mode configuration:</span></span>

    PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

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
    LoadBalancerDistribution : sourceIP

<span data-ttu-id="0a1e7-133">Pokud není zadán LoadBalancerDistribution element nástroje pro vyrovnávání zatížení Azure používá výchozí algoritmus 5 řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-133">If the LoadBalancerDistribution element is not present then the Azure Load balancer uses the default 5-tuple algorithm.</span></span>

### <a name="set-the-distribution-mode-on-a-load-balanced-endpoint-set"></a><span data-ttu-id="0a1e7-134">Nastavit režim distribuce na sady koncových bodů s vyrovnáváním zatížení</span><span class="sxs-lookup"><span data-stu-id="0a1e7-134">Set the Distribution mode on a load balanced endpoint set</span></span>

<span data-ttu-id="0a1e7-135">Pokud koncové body jsou součástí sady koncových bodů s vyrovnáváním zatížení, režim distribuce musí být nastavena na sady koncových bodů s vyrovnáváním zatížení:</span><span class="sxs-lookup"><span data-stu-id="0a1e7-135">If endpoints are part of a load balanced endpoint set, the distribution mode must be set on the load balanced endpoint set:</span></span>

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="cloud-service-configuration-to-change-distribution-mode"></a><span data-ttu-id="0a1e7-136">Konfigurace služby, chcete-li změnit režim distribuce v cloudu</span><span class="sxs-lookup"><span data-stu-id="0a1e7-136">Cloud Service configuration to change distribution mode</span></span>

<span data-ttu-id="0a1e7-137">Sada Azure SDK pro .NET 2.5 (pro vydané v listopadu) můžete využít k aktualizaci cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-137">You can leverage the Azure SDK for .NET 2.5 (to be released in November) to update your Cloud Service.</span></span> <span data-ttu-id="0a1e7-138">Nastavení koncového bodu pro cloudové služby se provádí v .csdef.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-138">Endpoint settings for Cloud Services are made in the .csdef.</span></span> <span data-ttu-id="0a1e7-139">Aby bylo možné aktualizovat režim distribuce nástroje pro vyrovnávání zatížení pro nasazení cloudové služby, je požadovaná nasazení upgradu.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-139">In order to update the load balancer distribution mode for a Cloud Services deployment, a deployment upgrade is required.</span></span>
<span data-ttu-id="0a1e7-140">Tady je příklad změny .csdef pro koncový bod nastavení:</span><span class="sxs-lookup"><span data-stu-id="0a1e7-140">Here is an example of .csdef changes for endpoint settings:</span></span>

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
    </Endpoints>
</WorkerRole>
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

## <a name="api-example"></a><span data-ttu-id="0a1e7-141">Příklad rozhraní API</span><span class="sxs-lookup"><span data-stu-id="0a1e7-141">API example</span></span>

<span data-ttu-id="0a1e7-142">Můžete nakonfigurovat distribuci nástroje pro vyrovnávání zatížení, pomocí rozhraní API správy služby.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-142">You can configure the load balancer distribution using the service management API.</span></span> <span data-ttu-id="0a1e7-143">Nezapomeňte přidat `x-ms-version` záhlaví je nastaven na verzi `2014-09-01` nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-143">Make sure to add the `x-ms-version` header is set to version `2014-09-01` or higher.</span></span>

### <a name="update-the-configuration-of-the-specified-load-balanced-set-in-a-deployment"></a><span data-ttu-id="0a1e7-144">Aktualizujte konfiguraci konkrétního nastavení Vyrovnávání zatížení v nasazení</span><span class="sxs-lookup"><span data-stu-id="0a1e7-144">Update the configuration of the specified load-balanced set in a deployment</span></span>

#### <a name="request-example"></a><span data-ttu-id="0a1e7-145">Příklad požadavku</span><span class="sxs-lookup"><span data-stu-id="0a1e7-145">Request example</span></span>

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet   x-ms-version: 2014-09-01
    Content-Type: application/xml

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
          <Port> port-assigned-to-probe </Port>
          <Protocol> probe-protocol </Protocol>
          <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
          <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
      </InputEndpoint>
    </LoadBalancedEndpointList>

<span data-ttu-id="0a1e7-146">Hodnota LoadBalancerDistribution může být sourceIP pro spřažení 2 řazené kolekce členů, sourceIPProtocol pro spřažení 3 řazené kolekce členů nebo hodnotu none (pro bez přidružení.</span><span class="sxs-lookup"><span data-stu-id="0a1e7-146">The value of LoadBalancerDistribution can be sourceIP for 2-tuple affinity, sourceIPProtocol for 3-tuple affinity or none (for no affinity.</span></span> <span data-ttu-id="0a1e7-147">například 5-n-tice)</span><span class="sxs-lookup"><span data-stu-id="0a1e7-147">i.e. 5-tuple)</span></span>

#### <a name="response"></a><span data-ttu-id="0a1e7-148">Odpověď</span><span class="sxs-lookup"><span data-stu-id="0a1e7-148">Response</span></span>

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a><span data-ttu-id="0a1e7-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0a1e7-149">Next Steps</span></span>

[<span data-ttu-id="0a1e7-150">Přehled nástroje pro vyrovnávání zatížení interní</span><span class="sxs-lookup"><span data-stu-id="0a1e7-150">Internal load balancer overview</span></span>](load-balancer-internal-overview.md)

[<span data-ttu-id="0a1e7-151">Začít konfigurovat internetové nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="0a1e7-151">Get started Configuring an Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="0a1e7-152">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="0a1e7-152">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
