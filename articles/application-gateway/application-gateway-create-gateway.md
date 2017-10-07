---
title: "aaaCreate, spuštění nebo odstranění aplikační brány | Microsoft Docs"
description: "Tato stránka obsahuje pokyny toocreate, konfiguraci, spuštění a odstranění služby Azure application gateway"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 577054ca-8368-4fbf-8d53-a813f29dc3bc
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 3efef5b49880c9efdafad8b88d4bce5b749b82af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-start-or-delete-an-application-gateway-with-powershell"></a>Vytvoření, spuštění nebo odstranění aplikační brány pomocí PowerShellu 

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Classic PowerShell](application-gateway-create-gateway.md)
> * [Šablona Azure Resource Manageru](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)

Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7. Poskytuje převzetí služeb při selhání, směrování výkonu požadavků HTTP mezi různými servery, ať už jsou hello cloudu nebo místně. Application Gateway poskytuje mnoho funkcí Application Delivery Controlleru (ADC), včetně vyrovnávání zatížení protokolu HTTP, spřažení relace na základě souborů cookie, přesměrování zpracování SSL (Secure Sockets Layer), vlastních testů stavu, podpory více webů a mnoha dalších. Navštivte toofind úplný seznam podporovaných funkcích [brány aplikací – přehled](application-gateway-introduction.md)

Tento článek vás provede kroky toocreate hello, konfiguraci, spuštění a odstranění služby application gateway.

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello pomocí hello instalačního programu webové platformy. Můžete stáhnout a nainstalovat nejnovější verzi hello z hello **prostředí Windows PowerShell** části hello [položky ke stažení](https://azure.microsoft.com/downloads/).
2. Pokud máte existující virtuální síť, vyberte existující prázdný podsítí nebo vytvořit novou podsíť ve stávající virtuální síti výhradně pro účely hello aplikační brány. Nelze nasadit hello aplikace brány tooa v jinou virtuální síť než hello prostředků, který chcete toodeploy za hello Aplikační brána Pokud partnerský vztah virtuální síť se používá. toolearn více navštivte [partnerský vztah virtuální sítě](../virtual-network/virtual-network-peering-overview.md)
3. Ověřte, že máte funkční virtuální síť s platnou podsítí. Ujistěte se, že hello podsíť nepoužívají žádné virtuální počítače ani Cloudová nasazení. Hello Aplikační brána musí být sám o sobě v podsíti virtuální sítě.
4. Hello servery nakonfigurovat toouse hello Aplikační brána musí existovat nebo přiřadili své koncové body vytvořené ve virtuální síti hello nebo s veřejnou IP nebo virtuální IP.

## <a name="what-is-required-toocreate-an-application-gateway"></a>Co je požadovaná toocreate služby application gateway?

Při použití hello `New-AzureApplicationGateway` příkaz toocreate hello Aplikační brána v tomto bodě se nenastaví žádná konfigurace a jsou nakonfigurovány hello nově vytvořený prostředek buď pomocí XML nebo objektu konfigurace.

Hello hodnoty jsou:

* **Fond back-end serverů:** hello seznam IP adres hello back-end serverů. uvedené Hello IP adresy by měly buď patřit toohello podsíť virtuální sítě nebo by měla být veřejné IP Adrese nebo VIP.
* **Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello.
* **Front-end port:** tento port je hello veřejný port, který se otevírá ve hello aplikační brány. Provoz volá Tenhle port a potom získá přesměruje tooone hello back-end serverů.
* **Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, tyto hodnoty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL).
* **Pravidlo:** hello pravidlo váže naslouchací proces hello a hello fond back-end serverů a definuje, jaký provoz hello fond back-end serverů by měla být směrovanou toowhen volání příslušného naslouchacího procesu.

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

toocreate aplikační brány:

1. Vytvořte prostředek aplikační brány.
2. Vytvořte konfigurační soubor XML nebo objekt konfigurace.
3. Potvrďte toohello konfigurace hello nově vytvořeného prostředku aplikační brány.

> [!NOTE]
> Pokud potřebujete tooconfigure vlastní test paměti svojí aplikační brány, přečtěte si [vytvoření služby application gateway s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-classic-ps.md). Další informace najdete v části [vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md).

![Příklad scénáře][scenario]

### <a name="create-an-application-gateway-resource"></a>Vytvořte prostředek aplikační brány

toocreate hello brány, použijte hello `New-AzureApplicationGateway` rutinu a nahraďte hello hodnoty vlastními. Fakturace hello brány se nespustí v tomto okamžiku. Fakturace začíná v pozdější fázi, po úspěšném spuštění brány hello.

Hello následující příklad vytvoří aplikační bránu pomocí virtuální sítě s názvem "testvnet1" a podsítě s názvem "subnet-1":

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

*Popis*, *InstanceCount* a *GatewaySize* jsou volitelné parametry.

byl vytvořen toovalidate, který hello brány, můžete použít hello `Get-AzureApplicationGateway` rutiny.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Stopped
VirtualIPs    : {}
DnsName       :
```

> [!NOTE]
> Výchozí hodnota pro Hello *InstanceCount* je 2, přičemž maximální hodnota je 10. Výchozí hodnota pro Hello *GatewaySize* je střední. Můžete vybrat mezi Malá, Střední a Velká.

*Rezervovaná* a *DnsName* se zobrazují jako prázdné, protože hello brána ještě nespustila. Tyto soubory jsou vytvořeny po hello brána je v běžícím stavu hello.

## <a name="configure-hello-application-gateway"></a>Konfigurace brány aplikace hello

Hello aplikační bránu můžete nakonfigurovat pomocí XML nebo objektu konfigurace.

### <a name="configure-hello-application-gateway-by-using-xml"></a>Konfigurace hello aplikační bránu pomocí XML

V následujícím příkladu hello použijte tooconfigure souboru XML všech nastavení aplikační brány a potvrdíte je toohello prostředku aplikační brány.  

#### <a name="step-1"></a>Krok 1

Zkopírujte následující text tooNotepad hello.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendPorts>
        <FrontendPort>
            <Name>(name-of-your-frontend-port)</Name>
            <Port>(port number)</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>(name-of-your-backend-pool)</Name>
            <IPAddresses>
                <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>(backend-setting-name-to-configure-rule)</Name>
            <Port>80</Port>
            <Protocol>[Http|Https]</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>(name-of-the-listener)</Name>
            <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
            <Protocol>[Http|Https]</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>(name-of-load-balancing-rule)</Name>
            <Type>basic</Type>
            <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
            <Listener>(name-of-the-listener)</Listener>
            <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

Upravte hodnoty hello mezi hello závorkách pro položky konfigurace hello. Uložte hello soubor s příponou .xml.

> [!IMPORTANT]
> Položka Hello protokolu Http nebo Https rozlišuje velká a malá písmena.

Hello následující příklad ukazuje, jak toouse konfigurační soubor tooset bránu aplikace hello. Příklad zatížení Hello vyrovnává provozu HTTP na veřejném portu 80 a odešle síťový provoz tooback-end port 80 mezi dvěma IP adresami.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendPorts>
        <FrontendPort>
            <Name>FrontendPort1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>BackendPool1</Name>
            <IPAddresses>
                <IPAddress>10.0.0.1</IPAddress>
                <IPAddress>10.0.0.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>BackendSetting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>HTTPListener1</Name>
            <FrontendPort>FrontendPort1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>HttpLBRule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
            <Listener>HTTPListener1</Listener>
            <BackendAddressPool>BackendPool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

#### <a name="step-2"></a>Krok 2

Dále nastavte aplikační bránu hello. Použití hello `Set-AzureApplicationGatewayConfig` rutiny s konfiguračním souborem XML.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"
```

### <a name="configure-hello-application-gateway-by-using-a-configuration-object"></a>Konfigurace hello aplikační bránu pomocí objektu konfigurace

Hello následující příklad ukazuje, jak tooconfigure hello aplikační bránu pomocí objektu konfigurace. Všechny položky konfigurace musíte musí nakonfigurovat individuálně a potom přidat objektu konfigurace tooan application gateway. Po vytvoření objektu konfigurace hello, použijete hello `Set-AzureApplicationGateway` příkaz toocommit hello konfigurace toohello vytvořili prostředku aplikační brány.

> [!NOTE]
> Před přiřazením objekt hodnoty tooeach konfigurace, je nutné toodeclare jaký druh objektu používá prostředí PowerShell pro úložiště. Hello první řádek toocreate hello jednotlivých položek definuje, co `Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(object name)` se používají.

#### <a name="step-1"></a>Krok 1

Vytvořte všechny položky individuální konfigurace.

Vytvořte front-end IP adresu hello, jak je znázorněno v následující ukázka hello.

```powershell
$fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
$fip.Name = "fip1"
$fip.Type = "Private"
$fip.StaticIPAddress = "10.0.0.5"
```

Vytvořte hello front-end port, jak ukazuje následující příklad hello.

```powershell
$fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
$fep.Name = "fep1"
$fep.Port = 80
```

Vytvořte fond back-end serverů hello.

Definujte hello IP adresy, které jsou přidány toohello fondu back-end serverů, jak je znázorněno v následujícím příkladu hello.

```powershell
$servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
$servers.Add("10.0.0.1")
$servers.Add("10.0.0.2")
```

Pomocí hello $server objekt tooadd hello hodnoty toohello fond back-end objektu ($pool).

```powershell
$pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
$pool.BackendServers = $servers
$pool.Name = "pool1"
```

Vytvořte nastavení fondu back-end serverů hello.

```powershell
$setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
$setting.Name = "setting1"
$setting.CookieBasedAffinity = "enabled"
$setting.Port = 80
$setting.Protocol = "http"
```

Vytvořte naslouchací proces hello.

```powershell
$listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
$listener.Name = "listener1"
$listener.FrontendPort = "fep1"
$listener.FrontendIP = "fip1"
$listener.Protocol = "http"
$listener.SslCert = ""
```

Vytvořte pravidlo hello.

```powershell
$rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
$rule.Name = "rule1"
$rule.Type = "basic"
$rule.BackendHttpSettings = "setting1"
$rule.Listener = "listener1"
$rule.BackendAddressPool = "pool1"
```

#### <a name="step-2"></a>Krok 2

Přiřaďte všechny individuální konfigurace položky tooan objektu konfigurace application gateway ($appgwconfig).

Přidejte toohello konfiguraci front-end IP adresy hello.

```powershell
$appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
$appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
$appgwconfig.FrontendIPConfigurations.Add($fip)
```

Přidáte konfiguraci toohello front-end port hello.

```powershell
$appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
$appgwconfig.FrontendPorts.Add($fep)
```
Přidáte konfiguraci toohello fondu back-end serverů hello.

```powershell
$appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
$appgwconfig.BackendAddressPools.Add($pool)
```

Přidáte konfiguraci toohello nastavení fondu back-end hello.

```powershell
$appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
$appgwconfig.BackendHttpSettingsList.Add($setting)
```

Přidáte konfiguraci toohello hello naslouchacího procesu.

```powershell
$appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
$appgwconfig.HttpListeners.Add($listener)
```

Přidáte konfiguraci toohello pravidlo hello.

```powershell
$appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
$appgwconfig.HttpLoadBalancingRules.Add($rule)
```

### <a name="step-3"></a>Krok 3
Potvrdit hello konfigurace objektu toohello prostředku aplikační brány pomocí `Set-AzureApplicationGatewayConfig`.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig
```

## <a name="start-hello-gateway"></a>Spusťte bránu hello

Jakmile se nakonfiguruje brána hello, použijte hello `Start-AzureApplicationGateway` rutiny toostart hello brány. Fakturace aplikační brány se spustí po úspěšném spuštění brány hello.

> [!NOTE]
> Hello `Start-AzureApplicationGateway` rutiny může trvat až toofinish too15-20 minut.

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-hello-gateway-status"></a>Ověření stavu brány hello

Použití hello `Get-AzureApplicationGateway` rutiny toocheck hello stav hello brány. Pokud `Start-AzureApplicationGateway` úspěšné v předchozím kroku hello *stavu* by měl být spuštěn, a *Vip* a *DnsName* musí obsahovat platné položky.

Hello následující příklad ukazuje aplikační bránu, která je aktivní, spuštěná, a je připravený tootake provoz určený pro `http://<generated-dns-name>.cloudapp.net`.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Running
Vip           : 138.91.170.26
DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net
```

## <a name="delete-hello-application-gateway"></a>Odstranění hello application gateway

toodelete hello aplikační brány:

1. Použití hello `Stop-AzureApplicationGateway` rutiny toostop hello brány.
2. Použití hello `Remove-AzureApplicationGateway` rutiny tooremove hello brány.
3. Ověřte, že hello brány byla odebrána pomocí hello `Get-AzureApplicationGateway` rutiny.

Hello následující příklad ukazuje hello `Stop-AzureApplicationGateway` na hello prvním řádku, následovanou výstup hello.

```powershell
Stop-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

Jakmile hello Aplikační brána v zastaveném stavu, použijte hello `Remove-AzureApplicationGateway` rutiny tooremove hello služby.

```powershell
Remove-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

Odebrali jsme tooverify, který hello služby, můžete použít hello `Get-AzureApplicationGateway` rutiny. Tenhle krok není povinný.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: hello gateway does not exist.
.....
```

## <a name="next-steps"></a>Další kroky

Pokud chcete tooconfigure přesměrování zpracování SSL, najdete v části [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).

Pokud chcete tooconfigure toouse brány aplikací s nástrojem pro vyrovnávání zatížení pro vnitřní, najdete v části [vytvoření aplikační brány s nástrojem pro vyrovnávání interní zatížení (ILB)](application-gateway-ilb.md).

Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:

* [Nástroj pro vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png
