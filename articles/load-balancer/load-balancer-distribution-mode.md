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
# <a name="configure-the-distribution-mode-for-load-balancer"></a>Nakonfigurujte distribuční režim pro vyrovnávání zatížení

## <a name="hash-based-distribution-mode"></a>Režim distribuce na základě hodnoty hash

Výchozí distribuční algoritmus používá 5-řazené kolekce členů (zdrojové IP adresy, zdrojového portu, cílové adresy IP, cílový port, protokol typu) hodnotu hash pro mapování provoz na dostupné servery. Poskytuje věrnosti pouze v rámci relace přenosu. Pakety ve stejné relaci se přesměruje na stejnou instanci datacenter IP (DIP) za vyrovnáváním zatížení koncového bodu. Když se klient spustí novou relaci ze stejné zdrojové IP adresy, zdrojového portu změny a způsobí, že provoz přejít k jinému koncovému bodu vyhrazené IP adresy.

![Nástroj pro vyrovnávání zatížení na základě hodnoty hash](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

Obrázek 1-5-n-tice distribuce

## <a name="source-ip-affinity-mode"></a>Režim spřažení IP zdroje

Máme jiný režim distribuce se označuje jako zdroj IP spřažení (známou taky jako spřažení relace na spřažení IP klienta). Azure nástroj pro vyrovnávání zatížení lze nakonfigurovat k využívání 2-n-tice (zdrojová adresa IP, cílovou IP adresu) nebo 3-n-tice (zdrojová adresa IP, cílové adresy IP, protokol) pro mapování provoz do dostupných serverů. Pomocí spřažení zdrojové IP adresy připojení inicializována z stejný klientský počítač přejde do stejné koncový bod vyhrazené IP adresy.

Následující diagram znázorňuje konfiguraci 2 řazené kolekce členů. Všimněte si, jak 2-n-tice spustí pomocí nástroje pro vyrovnávání zatížení do virtuálního počítače 1 (VM1) které se potom zálohuje virtuálního počítače 2 a VM3.

![spřažení relace](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Obrázek 2 – distribuční 2 řazené kolekce členů

Zdroj IP spřažení řeší nekompatibilitou nástroj pro vyrovnávání zatížení Azure a brány vzdálené plochy (RD). Nyní můžete vytvořit farmy služby Brána VP v jednom cloudové služby.

Další scénář případu využití je média nahrávání, kde nahrávání dat se nakonfigurují UDP ale rovině řízení je dosaženo pomocí TCP:

* Klient nejprve zahájí relace TCP na skupinu s vyrovnáváním zatížení veřejnou adresu, získá přesměrován na konkrétní vyhrazené IP adresy v tomto kanálu zůstane aktivní, monitorování stavu připojení
* Novou relaci UDP ze stejné klientský počítač se zahájí na stejný veřejný koncový bod Vyrovnávání zatížení, zde očekává se, že toto připojení je také k přesměrování stejný koncový bod vyhrazené IP adresy jako bylo předchozí připojení TCP tak, aby média nahrát mohou být provedeny na vysokou propustnost při zachování také řídicí kanál prostřednictvím TCP.

> [!NOTE]
> Když se změní sady s vyrovnáváním zatížení (odebrání nebo přidání virtuálního počítače), distribuci požadavky klientů je přepočítávány. Nemůže záviset na nové připojení z existující klienti ukončení na stejném serveru. Kromě toho použití zdrojové IP adresy režim distribuce spřažení může způsobit jako nerovné distribučního provozu. Klienti se systémem za proxy může považovat za jeden jedinečných klientských aplikací.

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a>Konfigurace nastavení spřažení zdrojové IP adresy pro službu Vyrovnávání zatížení

Pro virtuální počítače můžete použít PowerShell Chcete-li změnit nastavení časového limitu:

Koncový bod Azure přidejte k virtuálnímu počítači a nastavte režim distribuce nástroje pro vyrovnávání zatížení

```powershell
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

LoadBalancerDistribution může být nastaven na sourceIP pro 2-n-tice (zdrojová adresa IP, cílovou IP adresu) služby Vyrovnávání zatížení, sourceIPProtocol pro vyrovnávání zatížení 3-n-tice (zdrojová adresa IP, cílové adresy IP, protokol), nebo žádný Pokud chcete výchozí chování Vyrovnávání zatížení 5 řazené kolekce členů.

Použijte následující postupy k načtení konfigurace aplikace endpoint zatížení vyrovnávání distribuční režimu:

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

Pokud není zadán LoadBalancerDistribution element nástroje pro vyrovnávání zatížení Azure používá výchozí algoritmus 5 řazené kolekce členů.

### <a name="set-the-distribution-mode-on-a-load-balanced-endpoint-set"></a>Nastavit režim distribuce na sady koncových bodů s vyrovnáváním zatížení

Pokud koncové body jsou součástí sady koncových bodů s vyrovnáváním zatížení, režim distribuce musí být nastavena na sady koncových bodů s vyrovnáváním zatížení:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="cloud-service-configuration-to-change-distribution-mode"></a>Konfigurace služby, chcete-li změnit režim distribuce v cloudu

Sada Azure SDK pro .NET 2.5 (pro vydané v listopadu) můžete využít k aktualizaci cloudové služby. Nastavení koncového bodu pro cloudové služby se provádí v .csdef. Aby bylo možné aktualizovat režim distribuce nástroje pro vyrovnávání zatížení pro nasazení cloudové služby, je požadovaná nasazení upgradu.
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

Můžete nakonfigurovat distribuci nástroje pro vyrovnávání zatížení, pomocí rozhraní API správy služby. Nezapomeňte přidat `x-ms-version` záhlaví je nastaven na verzi `2014-09-01` nebo vyšší.

### <a name="update-the-configuration-of-the-specified-load-balanced-set-in-a-deployment"></a>Aktualizujte konfiguraci konkrétního nastavení Vyrovnávání zatížení v nasazení

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

Hodnota LoadBalancerDistribution může být sourceIP pro spřažení 2 řazené kolekce členů, sourceIPProtocol pro spřažení 3 řazené kolekce členů nebo hodnotu none (pro bez přidružení. například 5-n-tice)

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
