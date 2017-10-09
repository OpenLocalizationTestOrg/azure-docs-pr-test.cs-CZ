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
# <a name="configure-hello-distribution-mode-for-load-balancer"></a>Konfigurovat režim distribuce hello nástroje pro vyrovnávání zatížení

## <a name="hash-based-distribution-mode"></a>Režim distribuce na základě hodnoty hash

Hello výchozí distribuční algoritmus používá 5-řazené kolekce členů (zdrojová adresa IP, zdrojového portu, cílové adresy IP, cílový port, protokol typu) hash toomap provoz tooavailable servery. Poskytuje věrnosti pouze v rámci relace přenosu. Pakety hello stejné relace bude přesměruje toohello instance stejné datacenter IP (DIP) za vyrovnáváním zatížení hello koncový bod. Při spuštění klienta hello hello novou relaci ze stejné zdrojové IP adresy, zdrojového portu hello změny a způsobí, že hello provoz toogo tooa různých DIP koncový bod.

![Nástroj pro vyrovnávání zatížení na základě hodnoty hash](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

Obrázek 1-5-n-tice distribuce

## <a name="source-ip-affinity-mode"></a>Režim spřažení IP zdroje

Máme jiný režim distribuce se označuje jako zdroj IP spřažení (známou taky jako spřažení relace na spřažení IP klienta). Azure nástroj pro vyrovnávání zatížení může být nakonfigurované toouse 2-n-tice (zdrojová adresa IP, cílovou IP adresu) nebo 3-n-tice (zdrojová adresa IP, cílové adresy IP, protokol) toomap provozu toohello dostupné servery. Pomocí zdrojové IP adresy spřažení připojení inicializována z hello stejný klientský počítač přejde toohello stejný koncový bod vyhrazené IP adresy.

Hello následující diagram znázorňuje konfiguraci 2 řazené kolekce členů. Všimněte si, jak hello 2-n-tice spouští prostřednictvím hello zatížení vyrovnávání toovirtual počítač 1 (VM1) které se potom zálohuje virtuálního počítače 2 a VM3.

![spřažení relace](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Obrázek 2 – distribuční 2 řazené kolekce členů

Zdroj IP spřažení řeší nekompatibilitou hello nástroj pro vyrovnávání zatížení Azure a brány vzdálené plochy (RD). Nyní můžete vytvořit farmy služby Brána VP v jednom cloudové služby.

Další scénář případu využití je média nahrávání, kde nahrání dat hello se nakonfigurují UDP ale rovině řízení hello je dosaženo pomocí TCP:

* Klient nejprve zahájí veřejnou adresu s vyrovnáváním zatížení toohello relace TCP, získá směrovanou tooa konkrétní vyhrazené IP adresy v tomto kanálu je stav připojení levém active toomonitor hello
* Novou relaci UDP z hello stejný klientský počítač je zahájena toohello veřejný koncový bod s vyrovnáváním zatížení stejné, zde hello očekává se, aby toto připojení je také směrovanou toohello může být stejný koncový bod vyhrazené IP adresy jako hello předchozí připojení TCP tak, aby média nahrát provedená Vysoká propustnost při zachování také řídicí kanál prostřednictvím TCP.

> [!NOTE]
> Když se změní sady s vyrovnáváním zatížení (odebrání nebo přidání virtuálního počítače), je přepočítávány hello distribuce požadavky klientů. Nemůže záviset na nové připojení z existující klienti ukončení v hello stejný server. Kromě toho použití zdrojové IP adresy režim distribuce spřažení může způsobit jako nerovné distribučního provozu. Klienti se systémem za proxy může považovat za jeden jedinečných klientských aplikací.

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a>Konfigurace nastavení spřažení zdrojové IP adresy pro službu Vyrovnávání zatížení

Pro virtuální počítače můžete použít nastavení vypršení časového limitu toochange prostředí PowerShell:

Přidejte tooa Azure koncový bod virtuálního počítače a nastavte režim distribuce nástroje pro vyrovnávání zatížení

```powershell
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

LoadBalancerDistribution lze nastavit toosourceIP pro 2-n-tice (zdrojová adresa IP, cílovou IP adresu) služby Vyrovnávání zatížení, sourceIPProtocol pro vyrovnávání zatížení 3-n-tice (zdrojová adresa IP, cílové adresy IP, protokol), nebo žádný Pokud chcete, aby hello výchozí chování Vyrovnávání zatížení 5 řazené kolekce členů.

Použijte následující tooretrieve režimu konfigurace koncového bodu zatížení vyrovnávání distribuční hello:

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

Pokud není zadán hello LoadBalancerDistribution element používá nástroj pro vyrovnávání zatížení Azure hello hello výchozí 5 řazené kolekce členů algoritmus.

### <a name="set-hello-distribution-mode-on-a-load-balanced-endpoint-set"></a>Nastavit režim distribuce hello na sady koncových bodů s vyrovnáváním zatížení

Pokud koncové body jsou součástí sady koncových bodů s vyrovnáváním zatížení, režim distribuce hello musí být nastavena na sady koncových bodů s vyrovnáváním zatížení hello:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="cloud-service-configuration-toochange-distribution-mode"></a>Cloudové služby konfigurace toochange distribuční režimu

Můžete využít hello Azure SDK pro .NET 2.5 (toobe vydané v listopadu) tooupdate cloudové služby. Nastavení koncového bodu pro cloudové služby se provádí v hello .csdef. Nasazení upgradu v pořadí tooupdate hello vyrovnávání režim distribuce zatížení pro nasazení cloudové služby, není zapotřebí.
Tady je příklad změny .csdef pro koncový bod nastavení:

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

## <a name="api-example"></a>Příklad rozhraní API

Můžete nakonfigurovat distribuce nástroje pro vyrovnávání zatížení hello pomocí služby hello rozhraní API pro správu. Ujistěte se, zda text hello tooadd `x-ms-version` záhlaví nastavena tooversion `2014-09-01` nebo vyšší.

### <a name="update-hello-configuration-of-hello-specified-load-balanced-set-in-a-deployment"></a>Aktualizace konfigurace hello hello zadat sady s vyrovnáváním zatížení v nasazení

#### <a name="request-example"></a>Příklad požadavku

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

Hodnota Hello LoadBalancerDistribution může být sourceIP pro spřažení 2 řazené kolekce členů, sourceIPProtocol pro spřažení 3 řazené kolekce členů nebo hodnotu none (pro bez přidružení. například 5-n-tice)

#### <a name="response"></a>Odpověď

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Další kroky

[Přehled nástroje pro vyrovnávání zatížení interní](load-balancer-internal-overview.md)

[Začít konfigurovat internetové nástroj pro vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
