---
title: "aaaCreate klasický vlastní test paměti - Azure Application Gateway - prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate vlastní sběru dat pro službu Application Gateway pomocí prostředí PowerShell v modelu nasazení classic hello"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 338a7be1-835c-48e9-a072-95662dc30f5e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: 68332367c99328bd6456b0c339923765637be986
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Vytvoření vlastní test paměti pro Azure Application Gateway (klasické pomocí prostředí PowerShell)

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Azure Classic PowerShell](application-gateway-create-probe-classic-ps.md)

V tomto článku můžete přidat vlastní test paměti tooan existující aplikace bránu pomocí prostředí PowerShell. Vlastní testy paměti jsou užitečné pro aplikace, které mají na stránce Kontrola konkrétní stav nebo pro aplikace, které neposkytuje úspěšné odpovědi na hello výchozí webové aplikace.

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu Resource Manager hello](application-gateway-create-probe-ps.md).

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Vytvoření služby Application Gateway

toocreate aplikační brány:

1. Vytvořte prostředek aplikační brány.
2. Vytvořte konfigurační soubor XML nebo objekt konfigurace.
3. Potvrďte toohello konfigurace hello nově vytvořeného prostředku aplikační brány.

### <a name="create-an-application-gateway-resource-with-a-custom-probe"></a>Vytvořte prostředek aplikační brány s vlastní test paměti

toocreate hello brány, použijte hello `New-AzureApplicationGateway` rutinu a nahraďte hello hodnoty vlastními. Fakturace hello brány se nespustí v tomto okamžiku. Fakturace začíná v pozdější fázi, po úspěšném spuštění brány hello.

Hello následující příklad vytvoří aplikační bránu pomocí virtuální sítě s názvem "testvnet1" a podsítě s názvem "subnet-1".

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

byl vytvořen toovalidate, který hello brány, můžete použít hello `Get-AzureApplicationGateway` rutiny.

```powershell
Get-AzureApplicationGateway AppGwTest
```

> [!NOTE]
> Výchozí hodnota pro Hello *InstanceCount* je 2, přičemž maximální hodnota je 10. Výchozí hodnota pro Hello *GatewaySize* je střední. Můžete zvolit malá, střední a velké.
> 
> 

*Rezervovaná* a *DnsName* se zobrazují jako prázdné, protože hello brána ještě nespustila. Tyto hodnoty se vytvoří, jakmile hello brána v běžícím stavu hello.

### <a name="configure-an-application-gateway-by-using-xml"></a>Nakonfigurujte aplikační bránu pomocí XML

V následujícím příkladu hello použijte tooconfigure souboru XML všech nastavení aplikační brány a potvrdíte je toohello prostředku aplikační brány.  

Zkopírujte následující text tooNotepad hello.

```xml
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
<FrontendIPConfigurations>
    <FrontendIPConfiguration>
        <Name>fip1</Name>
        <Type>Private</Type>
    </FrontendIPConfiguration>
</FrontendIPConfigurations>
<FrontendPorts>
    <FrontendPort>
        <Name>port1</Name>
        <Port>80</Port>
    </FrontendPort>
</FrontendPorts>
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
    </Probes>
    <BackendAddressPools>
    <BackendAddressPool>
        <Name>pool1</Name>
        <IPAddresses>
            <IPAddress>1.1.1.1</IPAddress>
            <IPAddress>2.2.2.2</IPAddress>
        </IPAddresses>
    </BackendAddressPool>
</BackendAddressPools>
<BackendHttpSettingsList>
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
</BackendHttpSettingsList>
<HttpListeners>
    <HttpListener>
        <Name>listener1</Name>
        <FrontendIP>fip1</FrontendIP>
    <FrontendPort>port1</FrontendPort>
        <Protocol>Http</Protocol>
    </HttpListener>
</HttpListeners>
<HttpLoadBalancingRules>
    <HttpLoadBalancingRule>
        <Name>lbrule1</Name>
        <Type>basic</Type>
        <BackendHttpSettings>setting1</BackendHttpSettings>
        <Listener>listener1</Listener>
        <BackendAddressPool>pool1</BackendAddressPool>
    </HttpLoadBalancingRule>
</HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

Upravte hodnoty hello mezi hello závorkách pro položky konfigurace hello. Uložte hello soubor s příponou .xml.

Hello následující příklad ukazuje, jak toouse konfigurační soubor tooset až tooload brány aplikace hello vyrovnávat přenosy HTTP na veřejném portu 80 a odesílat přenos v síti tooback-end port 80 mezi dvě IP adresy pomocí vlastní test paměti.

> [!IMPORTANT]
> Položka Hello protokolu Http nebo Https rozlišuje velká a malá písmena.

Nová položka konfigurace \<testu\> je přidána tooconfigure vlastní testy paměti.

parametry konfigurace Hello jsou:

|Parametr|Popis|
|---|---|
|**Název** |Název odkazu pro vlastní test paměti. |
* **Protokol** | Protokol použitý (možné hodnoty jsou protokolu HTTP nebo HTTPS).|
| **Hostitele** a **cesta** | Dokončete cestu adresy URL, který je vyvolán hello hello stavu toodetermine brány aplikace hello instance. Například pokud máte na webu http://contoso.com/, pak lze nakonfigurovat vlastní test paměti hello "http://contoso.com/path/custompath.htm" pro test kontroluje toohave úspěšné odpovědi HTTP.|
| **Interval** | Nakonfiguruje hello testu interval kontroly v sekundách.|
| **Časový limit** | Definuje časový limit testu hello pro kontrolu odpovědi HTTP.|
| **UnhealthyThreshold** | Hello počet neúspěšných odpovědí HTTP potřeby tooflag hello back-end instance jako *není v pořádku*.|

Název sondy Hello se odkazuje v hello \<BackendHttpSettings\> tooassign konfigurace, které fond back-end používá nastavení vlastní test paměti.

## <a name="add-a-custom-probe-tooan-existing-application-gateway"></a>Přidat existující aplikace bránu tooan vlastní test paměti

Změny hello aktuální konfigurace služby application gateway vyžaduje tři kroky: získání hello aktuální konfigurační soubor XML, úpravě toohave vlastní test paměti a konfigurovat hello aplikační bránu pomocí nového nastavení XML hello.

1. Získat soubor XML hello pomocí `Get-AzureApplicationGatewayConfig`. Tato rutina exportuje hello konfigurace XML toobe upravit tooadd nastavení testu.

  ```powershell
  Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path toofile>"
  ```

1. V textovém editoru otevřete soubor XML hello. Přidat `<probe>` části po `<frontendport>`.

  ```xml
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
</Probes>
  ```

  V části backendHttpSettings hello hello XML přidejte název sondy hello, jak ukazuje následující příklad hello:

  ```xml
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
  ```

  Uložte soubor XML hello.

1. Konfigurace brány aplikace hello aktualizace s hello nový soubor XML s použitím `Set-AzureApplicationGatewayConfig`. Tato rutina aktualizuje aplikační bránu pomocí hello novou konfiguraci.

```powershell
Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path toofile>"
```

## <a name="next-steps"></a>Další kroky

Pokud chcete tooconfigure přesměrování zpracování Secure Sockets Layer (SSL), najdete v části [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).

Pokud chcete tooconfigure toouse brány aplikací s nástrojem pro vyrovnávání zatížení pro vnitřní, najdete v části [vytvoření aplikační brány s nástrojem pro vyrovnávání interní zatížení (ILB)](application-gateway-ilb.md).

