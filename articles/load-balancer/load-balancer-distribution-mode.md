---
title: "režim distribuce nástroj pro vyrovnávání zatížení aaaConfigure | Microsoft Docs"
description: "Jak tooconfigure Azure zatížení vyrovnávání distribuční režimu toosupport zdrojové IP spřažení"
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
ms.openlocfilehash: e745240b733ffc07928d8ed0ae097785ad4f412e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-distribution-mode-for-load-balancer"></a><span data-ttu-id="1fa98-103">Konfigurovat režim distribuce hello nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1fa98-103">Configure hello distribution mode for load balancer</span></span>

## <a name="hash-based-distribution-mode"></a><span data-ttu-id="1fa98-104">Režim distribuce na základě hodnoty hash</span><span class="sxs-lookup"><span data-stu-id="1fa98-104">Hash-based distribution mode</span></span>

<span data-ttu-id="1fa98-105">Hello výchozí distribuční algoritmus používá 5-řazené kolekce členů (zdrojová adresa IP, zdrojového portu, cílové adresy IP, cílový port, protokol typu) hash toomap provoz tooavailable servery.</span><span class="sxs-lookup"><span data-stu-id="1fa98-105">hello default distribution algorithm is a 5-tuple (source IP, source port, destination IP, destination port, protocol type) hash toomap traffic tooavailable servers.</span></span> <span data-ttu-id="1fa98-106">Poskytuje věrnosti pouze v rámci relace přenosu.</span><span class="sxs-lookup"><span data-stu-id="1fa98-106">It provides stickiness only within a transport session.</span></span> <span data-ttu-id="1fa98-107">Pakety hello stejné relace bude přesměruje toohello instance stejné datacenter IP (DIP) za vyrovnáváním zatížení hello koncový bod.</span><span class="sxs-lookup"><span data-stu-id="1fa98-107">Packets in hello same session will be directed toohello same datacenter IP (DIP) instance behind hello load balanced endpoint.</span></span> <span data-ttu-id="1fa98-108">Při spuštění klienta hello hello novou relaci ze stejné zdrojové IP adresy, zdrojového portu hello změny a způsobí, že hello provoz toogo tooa různých DIP koncový bod.</span><span class="sxs-lookup"><span data-stu-id="1fa98-108">When hello client starts a new session from hello same source IP, hello source port changes and causes hello traffic toogo tooa different DIP endpoint.</span></span>

![Nástroj pro vyrovnávání zatížení na základě hodnoty hash](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

<span data-ttu-id="1fa98-110">Obrázek 1-5-n-tice distribuce</span><span class="sxs-lookup"><span data-stu-id="1fa98-110">Figure 1 - 5-tuple distribution</span></span>

## <a name="source-ip-affinity-mode"></a><span data-ttu-id="1fa98-111">Režim spřažení IP zdroje</span><span class="sxs-lookup"><span data-stu-id="1fa98-111">Source IP affinity mode</span></span>

<span data-ttu-id="1fa98-112">Máme jiný režim distribuce se označuje jako zdroj IP spřažení (známou taky jako spřažení relace na spřažení IP klienta).</span><span class="sxs-lookup"><span data-stu-id="1fa98-112">We have another distribution mode called Source IP Affinity (also known as session affinity or client IP affinity).</span></span> <span data-ttu-id="1fa98-113">Azure nástroj pro vyrovnávání zatížení může být nakonfigurované toouse 2-n-tice (zdrojová adresa IP, cílovou IP adresu) nebo 3-n-tice (zdrojová adresa IP, cílové adresy IP, protokol) toomap provozu toohello dostupné servery.</span><span class="sxs-lookup"><span data-stu-id="1fa98-113">Azure Load Balancer can be configured toouse a 2-tuple (Source IP, Destination IP) or 3-tuple (Source IP, Destination IP, Protocol) toomap traffic toohello available servers.</span></span> <span data-ttu-id="1fa98-114">Pomocí zdrojové IP adresy spřažení připojení inicializována z hello stejný klientský počítač přejde toohello stejný koncový bod vyhrazené IP adresy.</span><span class="sxs-lookup"><span data-stu-id="1fa98-114">By using Source IP affinity, connections initiated from hello same client computer goes toohello same DIP endpoint.</span></span>

<span data-ttu-id="1fa98-115">Hello následující diagram znázorňuje konfiguraci 2 řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="1fa98-115">hello following diagram illustrates a 2-tuple configuration.</span></span> <span data-ttu-id="1fa98-116">Všimněte si, jak hello 2-n-tice spouští prostřednictvím hello zatížení vyrovnávání toovirtual počítač 1 (VM1) které se potom zálohuje virtuálního počítače 2 a VM3.</span><span class="sxs-lookup"><span data-stu-id="1fa98-116">Notice how hello 2-tuple runs through hello load balancer toovirtual machine 1 (VM1) which is then backed up by VM2 and VM3.</span></span>

![spřažení relace](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

<span data-ttu-id="1fa98-118">Obrázek 2 – distribuční 2 řazené kolekce členů</span><span class="sxs-lookup"><span data-stu-id="1fa98-118">Figure 2 - 2-tuple distribution</span></span>

<span data-ttu-id="1fa98-119">Zdroj IP spřažení řeší nekompatibilitou hello nástroj pro vyrovnávání zatížení Azure a brány vzdálené plochy (RD).</span><span class="sxs-lookup"><span data-stu-id="1fa98-119">Source IP affinity solves an incompatibility between hello Azure Load Balancer and Remote Desktop (RD) Gateway.</span></span> <span data-ttu-id="1fa98-120">Nyní můžete vytvořit farmy služby Brána VP v jednom cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="1fa98-120">Now you can build an RD gateway farm in a single cloud service.</span></span>

<span data-ttu-id="1fa98-121">Další scénář případu využití je média nahrávání, kde nahrání dat hello se nakonfigurují UDP ale rovině řízení hello je dosaženo pomocí TCP:</span><span class="sxs-lookup"><span data-stu-id="1fa98-121">Another use case scenario is media upload where hello data upload happens through UDP but hello control plane is achieved through TCP:</span></span>

* <span data-ttu-id="1fa98-122">Klient nejprve zahájí veřejnou adresu s vyrovnáváním zatížení toohello relace TCP, získá směrovanou tooa konkrétní vyhrazené IP adresy v tomto kanálu je stav připojení levém active toomonitor hello</span><span class="sxs-lookup"><span data-stu-id="1fa98-122">A client first initiates a TCP session toohello load balanced public address, gets directed tooa specific DIP, this channel is left active toomonitor hello connection health</span></span>
* <span data-ttu-id="1fa98-123">Novou relaci UDP z hello stejný klientský počítač je zahájena toohello veřejný koncový bod s vyrovnáváním zatížení stejné, zde hello očekává se, aby toto připojení je také směrovanou toohello může být stejný koncový bod vyhrazené IP adresy jako hello předchozí připojení TCP tak, aby média nahrát provedená Vysoká propustnost při zachování také řídicí kanál prostřednictvím TCP.</span><span class="sxs-lookup"><span data-stu-id="1fa98-123">A new UDP session from hello same client computer is initiated toohello same load balanced public endpoint, hello expectation here is that this connection is also directed toohello same DIP endpoint as hello previous TCP connection so that media upload can be executed at high throughput while also maintaining a control channel through TCP.</span></span>

> [!NOTE]
> <span data-ttu-id="1fa98-124">Když se změní sady s vyrovnáváním zatížení (odebrání nebo přidání virtuálního počítače), je přepočítávány hello distribuce požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="1fa98-124">When a load-balanced set changes (removing or adding a virtual machine), hello distribution of client requests is recomputed.</span></span> <span data-ttu-id="1fa98-125">Nemůže záviset na nové připojení z existující klienti ukončení v hello stejný server.</span><span class="sxs-lookup"><span data-stu-id="1fa98-125">You cannot depend on new connections from existing clients ending up at hello same server.</span></span> <span data-ttu-id="1fa98-126">Kromě toho použití zdrojové IP adresy režim distribuce spřažení může způsobit jako nerovné distribučního provozu.</span><span class="sxs-lookup"><span data-stu-id="1fa98-126">Additionally, using source IP affinity distribution mode may cause an unequal distribution of traffic.</span></span> <span data-ttu-id="1fa98-127">Klienti se systémem za proxy může považovat za jeden jedinečných klientských aplikací.</span><span class="sxs-lookup"><span data-stu-id="1fa98-127">Clients running behind proxies may be seen as one unique client application.</span></span>

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a><span data-ttu-id="1fa98-128">Konfigurace nastavení spřažení zdrojové IP adresy pro službu Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1fa98-128">Configuring Source IP affinity settings for load balancer</span></span>

<span data-ttu-id="1fa98-129">Pro virtuální počítače můžete použít nastavení vypršení časového limitu toochange prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="1fa98-129">For virtual machines, you can use PowerShell toochange timeout settings:</span></span>

<span data-ttu-id="1fa98-130">Přidejte tooa Azure koncový bod virtuálního počítače a nastavte režim distribuce nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1fa98-130">Add an Azure endpoint tooa Virtual Machine and set load balancer distribution mode</span></span>

```powershell
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

<span data-ttu-id="1fa98-131">LoadBalancerDistribution lze nastavit toosourceIP pro 2-n-tice (zdrojová adresa IP, cílovou IP adresu) služby Vyrovnávání zatížení, sourceIPProtocol pro vyrovnávání zatížení 3-n-tice (zdrojová adresa IP, cílové adresy IP, protokol), nebo žádný Pokud chcete, aby hello výchozí chování Vyrovnávání zatížení 5 řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="1fa98-131">LoadBalancerDistribution can be set toosourceIP for 2-tuple (Source IP, Destination IP) load balancing, sourceIPProtocol for 3-tuple (Source IP, Destination IP, protocol) load balancing, or none if you want hello default behavior of 5-tuple load balancing.</span></span>

<span data-ttu-id="1fa98-132">Použijte následující tooretrieve režimu konfigurace koncového bodu zatížení vyrovnávání distribuční hello:</span><span class="sxs-lookup"><span data-stu-id="1fa98-132">Use hello following tooretrieve an endpoint load balancer distribution mode configuration:</span></span>

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

<span data-ttu-id="1fa98-133">Pokud není zadán hello LoadBalancerDistribution element používá nástroj pro vyrovnávání zatížení Azure hello hello výchozí 5 řazené kolekce členů algoritmus.</span><span class="sxs-lookup"><span data-stu-id="1fa98-133">If hello LoadBalancerDistribution element is not present then hello Azure Load balancer uses hello default 5-tuple algorithm.</span></span>

### <a name="set-hello-distribution-mode-on-a-load-balanced-endpoint-set"></a><span data-ttu-id="1fa98-134">Nastavit režim distribuce hello na sady koncových bodů s vyrovnáváním zatížení</span><span class="sxs-lookup"><span data-stu-id="1fa98-134">Set hello Distribution mode on a load balanced endpoint set</span></span>

<span data-ttu-id="1fa98-135">Pokud koncové body jsou součástí sady koncových bodů s vyrovnáváním zatížení, režim distribuce hello musí být nastavena na sady koncových bodů s vyrovnáváním zatížení hello:</span><span class="sxs-lookup"><span data-stu-id="1fa98-135">If endpoints are part of a load balanced endpoint set, hello distribution mode must be set on hello load balanced endpoint set:</span></span>

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="cloud-service-configuration-toochange-distribution-mode"></a><span data-ttu-id="1fa98-136">Cloudové služby konfigurace toochange distribuční režimu</span><span class="sxs-lookup"><span data-stu-id="1fa98-136">Cloud Service configuration toochange distribution mode</span></span>

<span data-ttu-id="1fa98-137">Můžete využít hello Azure SDK pro .NET 2.5 (toobe vydané v listopadu) tooupdate cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="1fa98-137">You can leverage hello Azure SDK for .NET 2.5 (toobe released in November) tooupdate your Cloud Service.</span></span> <span data-ttu-id="1fa98-138">Nastavení koncového bodu pro cloudové služby se provádí v hello .csdef.</span><span class="sxs-lookup"><span data-stu-id="1fa98-138">Endpoint settings for Cloud Services are made in hello .csdef.</span></span> <span data-ttu-id="1fa98-139">Nasazení upgradu v pořadí tooupdate hello vyrovnávání režim distribuce zatížení pro nasazení cloudové služby, není zapotřebí.</span><span class="sxs-lookup"><span data-stu-id="1fa98-139">In order tooupdate hello load balancer distribution mode for a Cloud Services deployment, a deployment upgrade is required.</span></span>
<span data-ttu-id="1fa98-140">Tady je příklad změny .csdef pro koncový bod nastavení:</span><span class="sxs-lookup"><span data-stu-id="1fa98-140">Here is an example of .csdef changes for endpoint settings:</span></span>

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

## <a name="api-example"></a><span data-ttu-id="1fa98-141">Příklad rozhraní API</span><span class="sxs-lookup"><span data-stu-id="1fa98-141">API example</span></span>

<span data-ttu-id="1fa98-142">Můžete nakonfigurovat distribuce nástroje pro vyrovnávání zatížení hello pomocí služby hello rozhraní API pro správu.</span><span class="sxs-lookup"><span data-stu-id="1fa98-142">You can configure hello load balancer distribution using hello service management API.</span></span> <span data-ttu-id="1fa98-143">Ujistěte se, zda text hello tooadd `x-ms-version` záhlaví nastavena tooversion `2014-09-01` nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="1fa98-143">Make sure tooadd hello `x-ms-version` header is set tooversion `2014-09-01` or higher.</span></span>

### <a name="update-hello-configuration-of-hello-specified-load-balanced-set-in-a-deployment"></a><span data-ttu-id="1fa98-144">Aktualizace konfigurace hello hello zadat sady s vyrovnáváním zatížení v nasazení</span><span class="sxs-lookup"><span data-stu-id="1fa98-144">Update hello configuration of hello specified load-balanced set in a deployment</span></span>

#### <a name="request-example"></a><span data-ttu-id="1fa98-145">Příklad požadavku</span><span class="sxs-lookup"><span data-stu-id="1fa98-145">Request example</span></span>

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

<span data-ttu-id="1fa98-146">Hodnota Hello LoadBalancerDistribution může být sourceIP pro spřažení 2 řazené kolekce členů, sourceIPProtocol pro spřažení 3 řazené kolekce členů nebo hodnotu none (pro bez přidružení.</span><span class="sxs-lookup"><span data-stu-id="1fa98-146">hello value of LoadBalancerDistribution can be sourceIP for 2-tuple affinity, sourceIPProtocol for 3-tuple affinity or none (for no affinity.</span></span> <span data-ttu-id="1fa98-147">například 5-n-tice)</span><span class="sxs-lookup"><span data-stu-id="1fa98-147">i.e. 5-tuple)</span></span>

#### <a name="response"></a><span data-ttu-id="1fa98-148">Odpověď</span><span class="sxs-lookup"><span data-stu-id="1fa98-148">Response</span></span>

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a><span data-ttu-id="1fa98-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1fa98-149">Next Steps</span></span>

[<span data-ttu-id="1fa98-150">Přehled nástroje pro vyrovnávání zatížení interní</span><span class="sxs-lookup"><span data-stu-id="1fa98-150">Internal load balancer overview</span></span>](load-balancer-internal-overview.md)

[<span data-ttu-id="1fa98-151">Začít konfigurovat internetové nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1fa98-151">Get started Configuring an Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="1fa98-152">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1fa98-152">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
