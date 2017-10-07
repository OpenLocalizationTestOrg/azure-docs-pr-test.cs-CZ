---
title: "aaaConfigure SSL snižování zátěže prostředí PowerShell – Azure Application Gateway - classic | Microsoft Docs"
description: "Tento článek obsahuje pokyny hello toocreate přesměrování zpracování úloh služby application gateway pomocí protokolu SSL pomocí modelu nasazení Azure classic."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 63f28d96-9c47-410e-97dd-f5ca1ad1b8a4
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: 5cb128015747ed4b71802cf751c80b60634601a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-hello-classic-deployment-model"></a>Konfigurace aplikační brány pro přesměrování zpracování SSL pomocí modelu nasazení classic hello

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure Classic PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Služba Azure Application Gateway může být relace Secure Sockets Layer (SSL) nakonfigurované tooterminate hello v hello brány tooavoid nákladná SSL dešifrování úlohy toohappen v hello webové farmy. Přesměrování zpracování SSL zjednodušuje i nastavení serveru front-end hello a Správa webové aplikace hello.

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello pomocí hello instalačního programu webové platformy. Můžete stáhnout a nainstalovat nejnovější verzi hello z hello **prostředí Windows PowerShell** části hello [položky ke stažení](https://azure.microsoft.com/downloads/).
2. Ověřte, že máte funkční virtuální síť s platnou podsítí. Ujistěte se, že hello podsíť nepoužívají žádné virtuální počítače ani Cloudová nasazení. Hello Aplikační brána musí být sám o sobě v podsíti virtuální sítě.
3. Hello servery nakonfigurovat toouse hello Aplikační brána musí existovat nebo přiřadili své koncové body vytvořené ve virtuální síti hello nebo s veřejnou IP nebo virtuální IP.

tooconfigure SSL přesměrování zpracování úloh na aplikační brány, text hello, proveďte kroky v uvedeném pořadí hello:

1. [Vytvoření služby application gateway](#create-an-application-gateway)
2. [Nahrát certifikáty SSL](#upload-ssl-certificates)
3. [Konfigurace brány hello](#configure-the-gateway)
4. [Konfigurace brány hello sady](#set-the-gateway-configuration)
5. [Spusťte bránu hello](#start-the-gateway)
6. [Ověření stavu brány hello](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

toocreate hello brány, použijte hello `New-AzureApplicationGateway` rutinu a nahraďte hello hodnoty vlastními. Fakturace hello brány se nespustí v tomto okamžiku. Fakturace začíná v pozdější fázi, po úspěšném spuštění brány hello.

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

byl vytvořen toovalidate, který hello brány, můžete použít hello `Get-AzureApplicationGateway` rutiny.

V ukázce hello *popis*, *InstanceCount*, a *GatewaySize* jsou volitelné parametry. Výchozí hodnota pro Hello *InstanceCount* je 2, přičemž maximální hodnota je 10. Výchozí hodnota pro Hello *GatewaySize* je střední. Malých a velkých jsou ostatní dostupné hodnoty. *Rezervovaná* a *DnsName* se zobrazují jako prázdné, protože hello brána ještě nespustila. Tyto hodnoty se vytvoří, jakmile hello brána v běžícím stavu hello.

```powershell
Get-AzureApplicationGateway AppGwTest
```

## <a name="upload-ssl-certificates"></a>Nahrát certifikáty SSL

Použití `Add-AzureApplicationGatewaySslCertificate` tooupload hello certifikátu serveru v *pfx* formát toohello aplikační brány. název certifikátu Hello je název vybrané uživatelem a musí být jedinečný v rámci hello aplikační brány. Tento certifikát je tooby označují název v všechny operace správy certifikátů ve hello application gateway.

Tento následující příklad ukazuje rutinu hello nahraďte hello hodnoty v ukázce hello vlastní.

```powershell
Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path toopfx file>
```

Dále ověřte nahrávání certifikátu hello. Použití hello `Get-AzureApplicationGatewayCertificate` rutiny.

Tento příklad ukazuje rutinu hello na prvním řádku hello, následuje výstup hello.

```powershell
Get-AzureApplicationGatewaySslCertificate AppGwTest
```

```
VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
Name           : SslCert
SubjectName    : CN=gwcert.app.test.contoso.com
Thumbprint     : AF5ADD77E160A01A6......EE48D1A
ThumbprintAlgo : sha1RSA
State..........: Provisioned
```

> [!NOTE]
> heslo certifikátu Hello má toobe mezi 4 too12 znaky, písmena nebo číslice. Speciální znaky nejsou přijata.

## <a name="configure-hello-gateway"></a>Konfigurace brány hello

Konfigurace brány aplikace se skládá z více hodnot. může být vázáno Hello hodnoty společně tooconstruct hello konfigurace.

Hello hodnoty jsou:

* **Fond back-end serverů:** hello seznam IP adres hello back-end serverů. uvedené Hello IP adresy by měly buď patřit toohello podsíť virtuální sítě nebo by měla být veřejné IP Adrese nebo VIP.
* **Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello.
* **Front-end port:** tento port je hello veřejný port, který se otevírá ve hello aplikační brány. Provoz volá Tenhle port a potom získá přesměruje tooone hello back-end serverů.
* **Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, tyto hodnoty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL).
* **Pravidlo:** hello pravidlo váže naslouchací proces hello a hello fond back-end serverů a definuje, jaký provoz hello fond back-end serverů by měla být směrovanou toowhen volání příslušného naslouchacího procesu. V současné době pouze hello *základní* pravidel je podporována. Hello *základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.

**Další poznámky ke konfiguraci**

Pro konfiguraci certifikátů SSL, hello protokol v **HttpListener** by se měl změnit příliš*Https* (malá a velká písmena). Hello **SslCert** prvek přidán příliš**HttpListener** s hello nastavena hodnota toohello stejný název jako použít v odesílání hello předcházející části certifikáty SSL. Hello front-end port musí být aktualizované too443.

**spřažení na základě souborů cookie tooenable**: služby application gateway může být nakonfigurované tooensure, zda je žádost o od klientské relace vždy směrovanou toohello stejný virtuální počítač v hello webové farmy. Tento scénář se provádí injektáží souboru cookie relace, které umožňuje přenos toodirect hello brány správně. Nastavení spřažení na základě souborů cookie tooenable **CookieBasedAffinity** příliš*povoleno* v hello **BackendHttpSettings** element.

Konfiguraci můžete vytvořit buď pomocí vytvoření objektu konfigurace, nebo pomocí konfiguračního souboru XML.
použít konfiguraci pomocí konfigurační soubor XML, tooconstruct hello následující ukázka:

**Ukázkový kód XML konfigurace**

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations />
    <FrontendPorts>
        <FrontendPort>
            <Name>FrontendPort1</Name>
            <Port>443</Port>
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
            <Protocol>Https</Protocol>
            <SslCert>GWCert</SslCert>
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

## <a name="set-hello-gateway-configuration"></a>Konfigurace brány hello sady

Dále nastavte aplikační bránu hello. Můžete použít hello `Set-AzureApplicationGatewayConfig` rutiny buď objekt konfigurace nebo s konfiguračním souborem XML.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

## <a name="start-hello-gateway"></a>Spusťte bránu hello

Jakmile se nakonfiguruje brána hello, použijte hello `Start-AzureApplicationGateway` rutiny toostart hello brány. Fakturace aplikační brány se spustí po úspěšném spuštění brány hello.

> [!NOTE]
> Hello `Start-AzureApplicationGateway` rutiny může trvat až toofinish too15-20 minut.
>
>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-hello-gateway-status"></a>Ověření stavu brány hello

Použití hello `Get-AzureApplicationGateway` rutiny toocheck hello stav hello brány. Pokud `Start-AzureApplicationGateway` úspěšné v předchozím kroku hello *stavu* by měl být spuštěn, a *rezervovaná* a *DnsName* musí obsahovat platné položky.

Tento příklad ukazuje aplikační bránu, která je, spuštěna a je připravený tootake provoz.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
Name          : AppGwTest2
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Running
VirtualIPs    : {23.96.22.241}
DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net
```

## <a name="next-steps"></a>Další kroky

Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:

* [Nástroj pro vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

