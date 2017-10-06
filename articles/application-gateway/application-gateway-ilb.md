---
title: "aaaUsing Azure Application Gateway s interním nástrojem pro vyrovnávání zatížení | Microsoft Docs"
description: "Tato stránka obsahuje pokyny tooconfigure služby Azure Application Gateway s vyrovnáváním zatížení se vnitřní koncový bod"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 7403d28e-909f-46a2-b282-43a8e942f53c
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: 272ef84a02f92a8521c35aad6f1d9f9bf1675718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>Vytvoření brány Application Gateway s interním nástrojem Load Balancer (ILB)

> [!div class="op_single_selector"]
> * [Azure Classic PowerShell](application-gateway-ilb.md)
> * [Azure Resource Manager PowerShell](application-gateway-ilb-arm.md)

Application Gateway se dá nakonfigurovat s internetovým virtuální IP adresy nebo se toohello vnitřní koncový bod, nejsou viditelné internet, taky označovaný jako koncový bod interní nástroj pro vyrovnávání zatížení (ILB). Konfigurace hello brány s ILB je užitečná pro interní-obchodní aplikace, které nejsou viditelné toointernet. Je také užitečné pro/úrovně služeb v rámci vícevrstvé aplikace, který je spuštěn toointernet hranice nejsou viditelné zabezpečení, ale stále vyžadují distribuci zatížení kruhové dotazování, dlouhodobost relace nebo ukončení protokolu SSL. Tento článek vás provede kroky tooconfigure hello aplikační brány s ILB.

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello pomocí hello instalačního programu webové platformy. Můžete stáhnout a nainstalovat nejnovější verzi hello z hello **prostředí Windows PowerShell** části hello [stránky pro stažení](https://azure.microsoft.com/downloads/).
2. Ověřte, zda máte funkční virtuální síť s platnou podsítí.
3. Ověřte, zda máte back-end serverů ve virtuální síti hello nebo s veřejné nebo virtuálními IP Adresami přiřazen.

toocreate aplikační brány, proveďte následující kroky v uvedeném pořadí hello hello. 

1. [Vytvoření služby application gateway](#create-a-new-application-gateway)
2. [Konfigurace brány hello](#configure-the-gateway)
3. [Konfigurace brány hello sady](#set-the-gateway-configuration)
4. [Spusťte bránu hello](#start-the-gateway)
5. [Ověření hello brány](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a>Vytvoření služby application gateway:

**Brána hello toocreate**, použijte hello `New-AzureApplicationGateway` rutinu a nahraďte hello hodnoty vlastními. Všimněte si, že fakturace brány hello se nespustí v tomto okamžiku. Fakturace začíná v pozdější fázi, po úspěšném spuštění brány hello.

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

```
VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399
```

**toovalidate** , byl vytvořen hello brány, můžete použít hello `Get-AzureApplicationGateway` rutiny. 

V ukázce hello *popis*, *InstanceCount*, a *GatewaySize* jsou volitelné parametry. Výchozí hodnota pro Hello *InstanceCount* je 2, přičemž maximální hodnota je 10. Výchozí hodnota pro Hello *GatewaySize* je střední. Malých a velkých jsou ostatní dostupné hodnoty. *VIP* a *DnsName* se zobrazují jako prázdné, protože hello brána ještě nespustila. Tyto soubory jsou vytvořeny po hello brána je v běžícím stavu hello. 

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 4:39:39 PM - Begin Operation:
Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
Operation: Get-AzureApplicationGateway
Name: AppGwTest    
Description: 
VnetName: testvnet1 
Subnets: {Subnet-1} 
InstanceCount: 2 
GatewaySize: Medium 
State: Stopped 
VirtualIPs: 
DnsName:
```

## <a name="configure-hello-gateway"></a>Konfigurace brány hello
Konfigurace brány aplikace se skládá z více hodnot. může být vázáno Hello hodnoty společně tooconstruct hello konfigurace.

Hello hodnoty jsou:

* **Fond back-end serverů:** hello seznam IP adres hello back-end serverů. uvedené Hello IP adresy by měly buď patřit toohello podsíť virtuální sítě, nebo by měla být veřejné IP Adrese nebo VIP. 
* **Nastavení fondu serverů back-end:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie. Tato nastavení jsou vázané tooa fond a jsou použité tooall servery v rámci fondu hello.
* **Front-end Port:** Toto je veřejný port hello otevírá ve hello application gateway. Provoz volá Tenhle port a pak získá přesměrovaného tooone hello back-end serverů.
* **Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, ty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL). 
* **Pravidlo:** hello pravidlo váže naslouchací proces hello a fondu hello back-end serverů a definuje, jaký provoz back-end serveru fondu hello by měla být směrovanou toowhen volání příslušného naslouchacího procesu. V současné době pouze hello *základní* pravidel je podporována. Hello *základní* pravidlo je distribuce zatížení pomocí kruhového dotazování.

Konfiguraci můžete vytvořit buď tak, že vytvoříte objekt konfigurace, nebo pomocí konfiguračního souboru XML. tooconstruct ukázkové konfiguraci pomocí konfiguračního souboru XML, použijte hello níže.

Vezměte na vědomí následující hello:

* Hello *FrontendIPConfigurations* element popisuje podrobnosti ILB hello relevantní pro konfiguraci aplikační brány s ILB. 
* IP front-endu Hello *typ* by mělo být nastavené too'Private.
* Hello *StaticIPAddress* by mělo být nastavené toohello potřeby interních IP, na které hello brány přijímá provoz. Všimněte si, že hello *StaticIPAddress* prvek je volitelný. Pokud není sady, na k dispozici interní IP adresu z podsítě hello nasazení je vybrán. 
* Hello hodnotu hello *název* zadaný v elementu *FrontendIPConfiguration* by měly být použity hello HTTPListener na *FrontendIP* toohello toorefer – element FrontendIPConfiguration.
  
  **Ukázkový kód XML konfigurace**
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name> 
            <Type>Private</Type> 
            <StaticIPAddress>10.0.0.10</StaticIPAddress> 
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>
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
            <FrontendIP>fip1</FrontendIP>
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


## <a name="set-hello-gateway-configuration"></a>Konfigurace brány hello sady
Dále budete nastavte hello aplikační brány. Můžete použít hello `Set-AzureApplicationGatewayConfig` rutiny objekt konfigurace, nebo s konfiguračním souborem XML. 

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

```
VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d
```

## <a name="start-hello-gateway"></a>Spusťte bránu hello

Jakmile se nakonfiguruje brána hello, použijte hello `Start-AzureApplicationGateway` rutiny toostart hello brány. Fakturace aplikační brány se spustí po úspěšném spuštění brány hello. 

> [!NOTE]
> Hello `Start-AzureApplicationGateway` rutiny může trvat až toocomplete too15-20 minut. 
> 
> 

```powershell
Start-AzureApplicationGateway AppGwTest 
```

```
VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b
```

## <a name="verify-hello-gateway-status"></a>Ověření stavu brány hello

Použití hello `Get-AzureApplicationGateway` rutiny toocheck hello stav brány. Pokud `Start-AzureApplicationGateway` byly úspěšné v předchozím kroku hello, by měla být hello stavu *systémem*, hello Vip a DnsName musí obsahovat platné položky. Tento příklad ukazuje rutinu hello na prvním řádku hello, následuje výstup hello. V této ukázce hello brány je spuštěná a připravená tootake přenosy. 

> [!NOTE]
> Hello aplikace brána je nakonfigurovaná tooaccept provoz na hello nakonfigurovaný koncový bod ILB 10.0.0.10 v tomto příkladu.

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
VirtualIPs    : {10.0.0.10}
DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net
```

## <a name="next-steps"></a>Další kroky
Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:

* [Nástroj pro vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

