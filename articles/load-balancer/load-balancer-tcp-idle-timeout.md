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
# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>Konfigurace nastavení časového limitu nečinnosti TCP pro službu Vyrovnávání zatížení Azure

Ve výchozí konfiguraci Vyrovnávání zatížení Azure má časový limit nečinnosti nastavení 4 minut. Pokud je delší než hodnota časového limitu hello určité době nečinnosti, není zaručeno, že hello TCP nebo HTTP relace zachovaný mezi klientem hello a cloudové služby.

Při hello připojení je ukončeno, klientská aplikace může zobrazit hello následující chybová zpráva: "hello základní připojení bylo ukončeno: připojení, který byl očekáván toobe zachovány zachování připojení bylo ukončeno serverem hello."

Běžnou praxí je toouse udržování připojení TCP. Tento postup zachová připojení hello active delší dobu. Další informace najdete v tématu tyto [.NET příklady](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). S udržování připojení povolena, jsou pakety odesílány době nečinnosti hello připojení. Tyto udržovací pakety zajistěte, aby hodnota časového limitu nečinnosti hello není přístupný a se zachová připojení hello na dlouhou dobu.

Toto nastavení funguje pro příchozí připojení pouze. tooavoid ztráta hello připojení, je nutné nakonfigurovat hello udržování připojení protokolu TCP s intervalem menší než hello časový limit nečinnosti nastavení, nebo zvyšte hello časový limit nečinnosti hodnota. toosupport takových scénářů přidali jsme podporu pro konfigurovat časový limit nečinnosti. Nyní ji můžete nastavit po dobu 4 minuty too30.

Udržování připojení TCP funguje dobře pro scénáře, kdy výdrž baterie není omezení. Není doporučeno pro mobilní aplikace. Používání udržování v mobilní aplikaci TCP můžou vyprázdnit baterie zařízení hello pouze rychlejší.

![Vypršení časového limitu TCP](./media/load-balancer-tcp-idle-timeout/image1.png)

Hello následující části popisují, jak toochange nečinnosti nastavení časového limitu v virtuální počítače a cloudové služby.

## <a name="configure-hello-tcp-timeout-for-your-instance-level-public-ip-too15-minutes"></a>Konfigurace hello TCP vypršení časového limitu pro vaše instance úrovni veřejné IP too15 minut

```powershell
Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15
```

Parametr `IdleTimeoutInMinutes` je volitelný. Pokud není nastaven, hello výchozí časový limit je 4 minuty. rozsah Hello přijatelný časový limit je 4 minuty too30.

## <a name="set-hello-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>Při vytváření koncový bod Azure na virtuálním počítači nastavit časový limit nečinnosti hello

toochange hello časový limit nastavení pro koncový bod, použijte následující hello:

```powershell
Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM
```

tooretrieve konfiguraci časový limit nečinnosti hello použijte následující příkaz:

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

## <a name="set-hello-tcp-timeout-on-a-load-balanced-endpoint-set"></a>Nastavit časový limit TCP hello na sady koncových bodů s vyrovnáváním zatížení

Pokud koncové body jsou součástí sady koncových bodů s vyrovnáváním zatížení, hello TCP vypršení časového limitu musí být nastavena na sady koncových bodů s vyrovnáváním zatížení hello. Například:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15
```

## <a name="change-timeout-settings-for-cloud-services"></a>Změna nastavení časového limitu pro cloudové služby

Tooupdate hello Azure SDK můžete použít cloudové služby. Vytváření nastavení koncového bodu pro cloudové služby v souboru .csdef hello. Aktualizace hello časového limitu TCP pro nasazení cloudové služby vyžaduje nasazení upgradu. Výjimkou je, pokud je časový limit TCP hello zadat jenom pro veřejnou IP adresu. Nastavení veřejné IP adresy jsou v souboru .cscfg hello a můžete je aktualizovat prostřednictvím aktualizací nasazení a upgrade.

Hello .csdef změny pro koncový bod nastavení jsou tyto:

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
    </Endpoints>
</WorkerRole>
```

změny Hello .cscfg pro nastavení limitu hello na veřejné IP adresy jsou tyto:

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

## <a name="rest-api-example"></a>Příklad REST API

Časový limit nečinnosti TCP hello můžete nakonfigurovat pomocí služby hello rozhraní API pro správu. Ujistěte se, že hello `x-ms-version` záhlaví nastavena tooversion `2014-06-01` nebo novější. Aktualizace konfigurace hello hello zadat Vyrovnávání zatížení sítě vstupních koncových bodů na všechny virtuální počítače v nasazení.

### <a name="request"></a>Žádost

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Odpověď

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

## <a name="next-steps"></a>Další kroky

[Přehled nástroje pro vyrovnávání zatížení interní](load-balancer-internal-overview.md)

[Začněte konfiguraci Vyrovnávání zatížení internetového](load-balancer-get-started-internet-arm-ps.md)

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)
