---
title: "Konfigurace protokolu SSL snižování zátěže - Azure Application Gateway - PowerShell classic | Microsoft Docs"
description: "Tento článek poskytuje pokyny pro vytvoření služby application gateway pomocí protokolu SSL, přesměrování zpracování úloh pomocí modelu nasazení Azure classic."
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
ms.openlocfilehash: 2eba6fb24c11add12ac16d04d3445e19a3486216
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a>Konfigurace aplikační brány pro přesměrování zpracování SSL pomocí modelu nasazení classic

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure Classic PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Služba Azure Application Gateway se dá nakonfigurovat k ukončení relace Secure Sockets Layer (SSL) v bráně, vyhnete se tak nákladným úlohám dešifrování SSL na webové serverové farmě. Přesměrování zpracování SSL zjednodušuje i nastavení a správu front-end serverů webových aplikací.

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy. Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).
2. Ověřte, že máte funkční virtuální síť s platnou podsítí. Ujistěte se, že žádné virtuální počítače nebo cloudová nasazení nepoužívají podsíť. Služba Application Gateway musí být sama o sobě v podsíti virtuální sítě.
3. Servery, které nakonfigurujete pro použití služby Application Gateway, musí existovat nebo mít své koncové body vytvořené buď ve virtuální síti, nebo s přiřazenou veřejnou IP adresou nebo virtuální IP adresou.

Pokud chcete konfigurovat přesměrování zpracování SSL na aplikační brány, proveďte následující kroky v uvedeném pořadí:

1. [Vytvoření služby application gateway](#create-an-application-gateway)
2. [Nahrát certifikáty SSL](#upload-ssl-certificates)
3. [Konfigurace brány](#configure-the-gateway)
4. [Nastavení konfigurace brány](#set-the-gateway-configuration)
5. [Spusťte bránu](#start-the-gateway)
6. [Ověřte stav brány.](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

Pokud chcete vytvořit bránu, použijte rutinu `New-AzureApplicationGateway` a zadejte vlastní hodnoty. Fakturace brány se nespustí v tomhle okamžiku. Fakturace začíná v pozdější fázi, po úspěšném spuštění brány.

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

Pokud chcete ověřit vytvoření brány, můžete použít rutinu `Get-AzureApplicationGateway`.

V ukázce *popis*, *InstanceCount*, a *GatewaySize* jsou volitelné parametry. Výchozí hodnota *InstanceCount* je 2, přičemž maximální hodnota je 10. Výchozí hodnota *GatewaySize* je Medium (Střední). Malých a velkých jsou ostatní dostupné hodnoty. Hodnoty *VirtualIPs* a *DnsName* se zobrazují jako prázdné, protože se brána ještě nespustila. Tyto hodnoty se vytvoří, jakmile je brána v běžícím stavu.

```powershell
Get-AzureApplicationGateway AppGwTest
```

## <a name="upload-ssl-certificates"></a>Nahrát certifikáty SSL

Použití `Add-AzureApplicationGatewaySslCertificate` nahrát certifikát serveru ve *pfx* formátu aplikační brány. Název certifikátu je název vybrané uživatelem a musí být jedinečný v rámci služby application gateway. Tento certifikát se označuje s tímto názvem v všechny operace správy certifikátů ve službě application gateway.

Tento následující příklad ukazuje rutinu, nahraďte hodnotami v ukázce vlastní.

```powershell
Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>
```

Dále ověřte nahrávání certifikátu. Použití `Get-AzureApplicationGatewayCertificate` rutiny.

Tento příklad ukazuje rutinu na prvním řádku, následovanou výstupem.

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
> Heslo certifikátu musí být mezi 4 až 12 znaků, písmen nebo číslic. Speciální znaky nejsou přijata.

## <a name="configure-the-gateway"></a>Konfigurace brány

Konfigurace brány aplikace se skládá z více hodnot. Může být vázáno hodnoty společně k vytvoření konfigurace.

Hodnoty jsou:

* **Fond back-end serverů:** Seznam IP adres back-end serverů. Uvedené IP adresy by měly buď patřit do podsítě virtuální sítě, nebo by měly být veřejnými nebo virtuálními IP adresami.
* **Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.
* **Front-end port:** Toto je veřejný port, který se otevírá ve službě Application Gateway. Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.
* **Naslouchací proces:** Naslouchací proces má front-end port, protokol (Http nebo Https, u těchto hodnot se rozlišují malá a velká písmena) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL).
* **Pravidlo:** Pravidlo váže naslouchací proces a fond back-end serverů a definuje, ke kterému fondu back-end serverů se má provoz směrovat při volání příslušného naslouchacího procesu. V tuhle chvíli se podporuje jenom *základní* pravidlo. *Základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.

**Další poznámky ke konfiguraci**

Pro konfiguraci certifikátů SSL by se měl změnit protokol v **HttpListener** na *Https* (rozlišování velkých a malých písmen). **SslCert** prvek přidán na **HttpListener** hodnota nastavena na stejný název jako použít v odesílání předcházející SSL certifikáty části. Front-end port se musí aktualizovat na hodnotu 443.

**Když chcete povolit spřažení na základě souborů cookie**: aplikační brána se může nakonfigurovat tak, aby se žádost od klientské relace vždy směrovala na stejný virtuální počítač v prostředí webové serverové farmy. Takový scénář se provádí injektáží souboru cookie relace, který bráně umožňuje řídit provoz odpovídajícím způsobem. Když chcete povolit spřažení na základě souboru cookie, nastavte **CookieBasedAffinity** na *Povoleno* v elementu **BackendHttpSettings**.

Konfiguraci můžete vytvořit buď pomocí vytvoření objektu konfigurace, nebo pomocí konfiguračního souboru XML.
Chcete-li vytvořit konfiguraci pomocí konfiguračního souboru XML, použijte následující ukázka:

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

## <a name="set-the-gateway-configuration"></a>Nastavení konfigurace brány

Dále nastavte aplikační bránu. Můžete použít `Set-AzureApplicationGatewayConfig` rutiny buď objekt konfigurace nebo s konfiguračním souborem XML.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

## <a name="start-the-gateway"></a>Spusťte bránu

Jakmile se brána nakonfiguruje, pomocí rutiny `Start-AzureApplicationGateway` ji spusťte. Fakturace aplikační brány se spustí až po úspěšném spuštění brány.

> [!NOTE]
> Dokončení rutiny `Start-AzureApplicationGateway` může trvat 15 až 20 minut.
>
>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-the-gateway-status"></a>Ověřte stav brány.

Pomocí rutiny `Get-AzureApplicationGateway` zkontrolujte stav brány. Pokud `Start-AzureApplicationGateway` úspěšné v předchozím kroku, *stavu* by měl být spuštěn, a *rezervovaná* a *DnsName* musí obsahovat platné položky.

Tento příklad ukazuje aplikační bránu, která je v provozu, spuštěna a je připraven přijmout provoz.

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

