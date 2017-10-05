---
title: "Vytvoření, spuštění nebo odstranění aplikační brány | Dokumentace Microsoftu"
description: "Tahle stránka poskytuje pokyny pro vytvoření, konfiguraci, spuštění a odstranění služby Azure application gateway"
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
ms.openlocfilehash: c4932096229b1941e0966e7f3e97de39c6931392
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-start-or-delete-an-application-gateway-with-powershell"></a><span data-ttu-id="2548e-103">Vytvoření, spuštění nebo odstranění aplikační brány pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="2548e-103">Create, start, or delete an application gateway with PowerShell</span></span> 

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2548e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2548e-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="2548e-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="2548e-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="2548e-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="2548e-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="2548e-107">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="2548e-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="2548e-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2548e-108">Azure CLI</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="2548e-109">Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7.</span><span class="sxs-lookup"><span data-stu-id="2548e-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="2548e-110">Poskytuje převzetí služeb při selhání, směrování výkonu požadavků HTTP mezi různými servery, ať už jsou místní nebo v cloudu.</span><span class="sxs-lookup"><span data-stu-id="2548e-110">It provides failover, performance-routing HTTP requests between different servers, whether they are on the cloud or on-premises.</span></span> <span data-ttu-id="2548e-111">Application Gateway poskytuje mnoho funkcí Application Delivery Controlleru (ADC), včetně vyrovnávání zatížení protokolu HTTP, spřažení relace na základě souborů cookie, přesměrování zpracování SSL (Secure Sockets Layer), vlastních testů stavu, podpory více webů a mnoha dalších.</span><span class="sxs-lookup"><span data-stu-id="2548e-111">Application Gateway provides many Application Delivery Controller (ADC) features including HTTP load balancing, cookie-based session affinity, Secure Sockets Layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="2548e-112">Úplný seznam podporovaných funkcí najdete v tématu [Přehled služby Application Gateway](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2548e-112">To find a complete list of supported features, visit [Application Gateway Overview](application-gateway-introduction.md)</span></span>

<span data-ttu-id="2548e-113">Tenhle článek vás provede kroky k vytvoření, konfiguraci, spuštění a odstranění aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="2548e-113">This article walks you through the steps to create, configure, start, and delete an application gateway.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="2548e-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="2548e-114">Before you begin</span></span>

1. <span data-ttu-id="2548e-115">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy.</span><span class="sxs-lookup"><span data-stu-id="2548e-115">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="2548e-116">Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="2548e-116">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="2548e-117">Pokud už máte virtuální síť, vyberte buď existující prázdnou podsíť, nebo vytvořte novou podsíť výhradně pro účely služby Application Gateway v existující virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="2548e-117">If you have an existing virtual network, either select an existing empty subnet or create a new subnet in your existing virtual network solely for use by the application gateway.</span></span> <span data-ttu-id="2548e-118">Službu Application Gateway nelze nasadit do jiné virtuální sítě, než prostředky, které máte v úmyslu nasadit za službou Application Gateway, pokud nepoužijete partnerský vztah virtuálních sítí.</span><span class="sxs-lookup"><span data-stu-id="2548e-118">You cannot deploy the application gateway to a different virtual network than the resources you intend to deploy behind the application gateway unless vnet peering is used.</span></span> <span data-ttu-id="2548e-119">Další informace najdete v tématu [Partnerské vztahy virtuálních sítí](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2548e-119">To learn more visit [Vnet Peering](../virtual-network/virtual-network-peering-overview.md)</span></span>
3. <span data-ttu-id="2548e-120">Ověřte, že máte funkční virtuální síť s platnou podsítí.</span><span class="sxs-lookup"><span data-stu-id="2548e-120">Verify that you have a working virtual network with a valid subnet.</span></span> <span data-ttu-id="2548e-121">Ujistěte se, že žádné virtuální počítače nebo cloudová nasazení nepoužívají podsíť.</span><span class="sxs-lookup"><span data-stu-id="2548e-121">Make sure that no virtual machines or cloud deployments are using the subnet.</span></span> <span data-ttu-id="2548e-122">Služba Application Gateway musí být sama o sobě v podsíti virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="2548e-122">The application gateway must be by itself in a virtual network subnet.</span></span>
4. <span data-ttu-id="2548e-123">Servery, které nakonfigurujete pro použití služby Application Gateway, musí existovat nebo mít své koncové body vytvořené buď ve virtuální síti, nebo s přiřazenou veřejnou IP adresou nebo virtuální IP adresou.</span><span class="sxs-lookup"><span data-stu-id="2548e-123">The servers that you configure to use the application gateway must exist or have their endpoints created either in the virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-to-create-an-application-gateway"></a><span data-ttu-id="2548e-124">Co je potřeba k vytvoření služby Application Gateway?</span><span class="sxs-lookup"><span data-stu-id="2548e-124">What is required to create an application gateway?</span></span>

<span data-ttu-id="2548e-125">Když k vytvoření služby Application Gateway použijete příkaz `New-AzureApplicationGateway`, v tomto bodě se nenastaví žádná konfigurace a nově vytvořený prostředek se konfiguruje buď pomocí XML, nebo objektu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2548e-125">When you use the `New-AzureApplicationGateway` command to create the application gateway, no configuration is set at this point and the newly created resource are configured either by using XML or a configuration object.</span></span>

<span data-ttu-id="2548e-126">Hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="2548e-126">The values are:</span></span>

* <span data-ttu-id="2548e-127">**Fond back-end serverů:** Seznam IP adres back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="2548e-127">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="2548e-128">Uvedené IP adresy by měly buď patřit do podsítě virtuální sítě, nebo by měly být veřejnými nebo virtuálními IP adresami.</span><span class="sxs-lookup"><span data-stu-id="2548e-128">The IP addresses listed should either belong to the virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="2548e-129">**Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="2548e-129">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="2548e-130">Tato nastavení se vážou na fond a používají se na všechny servery v rámci fondu.</span><span class="sxs-lookup"><span data-stu-id="2548e-130">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="2548e-131">**Front-end port:** Toto je veřejný port, který se otevírá ve službě Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="2548e-131">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="2548e-132">Když datový přenos dorazí na tento port, přesměruje se na některý back-end server.</span><span class="sxs-lookup"><span data-stu-id="2548e-132">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="2548e-133">**Naslouchací proces:** Naslouchací proces má front-end port, protokol (Http nebo Https, u těchto hodnot se rozlišují malá a velká písmena) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="2548e-133">**Listener:** The listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="2548e-134">**Pravidlo:** Pravidlo váže naslouchací proces a fond back-end serverů a definuje, ke kterému fondu back-end serverů se má provoz směrovat při volání příslušného naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="2548e-134">**Rule:** The rule binds the listener and the back-end server pool and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="2548e-135">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="2548e-135">Create an application gateway</span></span>

<span data-ttu-id="2548e-136">Pro vytvoření nové aplikační brány:</span><span class="sxs-lookup"><span data-stu-id="2548e-136">To create an application gateway:</span></span>

1. <span data-ttu-id="2548e-137">Vytvořte prostředek aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="2548e-137">Create an application gateway resource.</span></span>
2. <span data-ttu-id="2548e-138">Vytvořte konfigurační soubor XML nebo objekt konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2548e-138">Create a configuration XML file or a configuration object.</span></span>
3. <span data-ttu-id="2548e-139">Potvrďte konfiguraci nově vytvořeného prostředku aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="2548e-139">Commit the configuration to the newly created application gateway resource.</span></span>

> [!NOTE]
> <span data-ttu-id="2548e-140">Když potřebujete nakonfigurovat vlastní test paměti svojí aplikační brány, přečtěte si část [Vytvořit bránu s vlastními testy paměti pomocí prostředí PowerShell](application-gateway-create-probe-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="2548e-140">If you need to configure a custom probe for your application gateway, see [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-classic-ps.md).</span></span> <span data-ttu-id="2548e-141">Další informace najdete v části [vlastní testy paměti a sledování stavu](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2548e-141">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>

![Příklad scénáře][scenario]

### <a name="create-an-application-gateway-resource"></a><span data-ttu-id="2548e-143">Vytvořte prostředek aplikační brány</span><span class="sxs-lookup"><span data-stu-id="2548e-143">Create an application gateway resource</span></span>

<span data-ttu-id="2548e-144">Pokud chcete vytvořit bránu, použijte rutinu `New-AzureApplicationGateway` a zadejte vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2548e-144">To create the gateway, use the `New-AzureApplicationGateway` cmdlet, replacing the values with your own.</span></span> <span data-ttu-id="2548e-145">Fakturace brány se nespustí v tomhle okamžiku.</span><span class="sxs-lookup"><span data-stu-id="2548e-145">Billing for the gateway does not start at this point.</span></span> <span data-ttu-id="2548e-146">Fakturace začíná v pozdější fázi, po úspěšném spuštění brány.</span><span class="sxs-lookup"><span data-stu-id="2548e-146">Billing begins in a later step, when the gateway is successfully started.</span></span>

<span data-ttu-id="2548e-147">Následující příklad vytvoří aplikační bránu pomocí virtuální sítě s názvem testvnet1 a podsítě s názvem subnet-1:</span><span class="sxs-lookup"><span data-stu-id="2548e-147">The following example creates an application gateway by using a virtual network called "testvnet1" and a subnet called "subnet-1":</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

<span data-ttu-id="2548e-148">*Popis*, *InstanceCount* a *GatewaySize* jsou volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="2548e-148">*Description*, *InstanceCount*, and *GatewaySize* are optional parameters.</span></span>

<span data-ttu-id="2548e-149">Pokud chcete ověřit vytvoření brány, můžete použít rutinu `Get-AzureApplicationGateway`.</span><span class="sxs-lookup"><span data-stu-id="2548e-149">To validate that the gateway was created, you can use the `Get-AzureApplicationGateway` cmdlet.</span></span>

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
> <span data-ttu-id="2548e-150">Výchozí hodnota *InstanceCount* je 2, přičemž maximální hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="2548e-150">The default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="2548e-151">Výchozí hodnota *GatewaySize* je Medium (Střední).</span><span class="sxs-lookup"><span data-stu-id="2548e-151">The default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="2548e-152">Můžete vybrat mezi Malá, Střední a Velká.</span><span class="sxs-lookup"><span data-stu-id="2548e-152">You can choose between Small, Medium and Large.</span></span>

<span data-ttu-id="2548e-153">Hodnoty *VirtualIPs* a *DnsName* se zobrazují jako prázdné, protože se brána ještě nespustila.</span><span class="sxs-lookup"><span data-stu-id="2548e-153">*VirtualIPs* and *DnsName* are shown as blank because the gateway has not started yet.</span></span> <span data-ttu-id="2548e-154">Vytvoří se, jakmile bude brána v běžícím stavu.</span><span class="sxs-lookup"><span data-stu-id="2548e-154">These are created once the gateway is in the running state.</span></span>

## <a name="configure-the-application-gateway"></a><span data-ttu-id="2548e-155">Nakonfigurujte aplikační bránu</span><span class="sxs-lookup"><span data-stu-id="2548e-155">Configure the application gateway</span></span>

<span data-ttu-id="2548e-156">Aplikační bránu můžete nakonfigurovat pomocí XML nebo objektu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2548e-156">You can configure the application gateway by using XML or a configuration object.</span></span>

### <a name="configure-the-application-gateway-by-using-xml"></a><span data-ttu-id="2548e-157">Nakonfigurujte aplikační bránu pomocí XML</span><span class="sxs-lookup"><span data-stu-id="2548e-157">Configure the application gateway by using XML</span></span>

<span data-ttu-id="2548e-158">V následujícím příkladu použijete soubor XML k nakonfigurování všech nastavení aplikační brány a potvrdíte je pro prostředek aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="2548e-158">In the following example, you use an XML file to configure all application gateway settings and commit them to the application gateway resource.</span></span>  

#### <a name="step-1"></a><span data-ttu-id="2548e-159">Krok 1</span><span class="sxs-lookup"><span data-stu-id="2548e-159">Step 1</span></span>

<span data-ttu-id="2548e-160">Zkopírujte následující text do Poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="2548e-160">Copy the following text to Notepad.</span></span>

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

<span data-ttu-id="2548e-161">Upravte hodnoty položek konfigurace v závorkách.</span><span class="sxs-lookup"><span data-stu-id="2548e-161">Edit the values between the parentheses for the configuration items.</span></span> <span data-ttu-id="2548e-162">Uložte soubor s příponou .xml.</span><span class="sxs-lookup"><span data-stu-id="2548e-162">Save the file with extension .xml.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2548e-163">Položka protokolu Http nebo Https rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="2548e-163">The protocol item Http or Https is case-sensitive.</span></span>

<span data-ttu-id="2548e-164">Následující příklad ukazuje, jak použít konfigurační soubor k nastavení služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="2548e-164">The following example shows how to use a configuration file to set up the application gateway.</span></span> <span data-ttu-id="2548e-165">V příkladu se vyrovnává zatížení provozu HTTP na veřejném portu 80 a síťový provoz mezi dvěma IP adresami se odesílá na port back-end 80.</span><span class="sxs-lookup"><span data-stu-id="2548e-165">The example load balances HTTP traffic on public port 80 and sends network traffic to back-end port 80 between two IP addresses.</span></span>

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

#### <a name="step-2"></a><span data-ttu-id="2548e-166">Krok 2</span><span class="sxs-lookup"><span data-stu-id="2548e-166">Step 2</span></span>

<span data-ttu-id="2548e-167">Dále nastavte aplikační bránu.</span><span class="sxs-lookup"><span data-stu-id="2548e-167">Next, set the application gateway.</span></span> <span data-ttu-id="2548e-168">Použijte rutinu `Set-AzureApplicationGatewayConfig` s konfiguračním souborem XML.</span><span class="sxs-lookup"><span data-stu-id="2548e-168">Use the `Set-AzureApplicationGatewayConfig` cmdlet with a configuration XML file.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"
```

### <a name="configure-the-application-gateway-by-using-a-configuration-object"></a><span data-ttu-id="2548e-169">Nakonfigurujte aplikační bránu pomocí objektu konfigurace</span><span class="sxs-lookup"><span data-stu-id="2548e-169">Configure the application gateway by using a configuration object</span></span>

<span data-ttu-id="2548e-170">Následující příklad ukazuje, jak se provádí konfigurace aplikační brány pomocí objektu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2548e-170">The following example shows how to configure the application gateway by using configuration objects.</span></span> <span data-ttu-id="2548e-171">Všechny položky konfigurace se musí nakonfigurovat individuálně a potom se musí přidat k objektu konfigurace aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="2548e-171">All configuration items must be configured individually and then added to an application gateway configuration object.</span></span> <span data-ttu-id="2548e-172">Po vytvoření objektu konfigurace použijte příkaz `Set-AzureApplicationGateway` pro potvrzení konfigurace k předem vytvořenému prostředku služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="2548e-172">After creating the configuration object, you use the `Set-AzureApplicationGateway` command to commit the configuration to the previously created application gateway resource.</span></span>

> [!NOTE]
> <span data-ttu-id="2548e-173">Před přiřazením hodnoty každému objektu konfigurace musíte deklarovat, který typ objektu používá prostředí PowerShell pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="2548e-173">Before assigning a value to each configuration object, you need to declare what kind of object PowerShell uses for storage.</span></span> <span data-ttu-id="2548e-174">První řádek vytvoření jednotlivých položek definuje, jaký model `Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(object name)` se použije.</span><span class="sxs-lookup"><span data-stu-id="2548e-174">The first line to create the individual items defines what `Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(object name)` are used.</span></span>

#### <a name="step-1"></a><span data-ttu-id="2548e-175">Krok 1</span><span class="sxs-lookup"><span data-stu-id="2548e-175">Step 1</span></span>

<span data-ttu-id="2548e-176">Vytvořte všechny položky individuální konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2548e-176">Create all individual configuration items.</span></span>

<span data-ttu-id="2548e-177">Vytvořte front-end IP adresu, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="2548e-177">Create the front-end IP as shown in the following example.</span></span>

```powershell
$fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
$fip.Name = "fip1"
$fip.Type = "Private"
$fip.StaticIPAddress = "10.0.0.5"
```

<span data-ttu-id="2548e-178">Vytvořte front-end port, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="2548e-178">Create the front-end port as shown in the following example.</span></span>

```powershell
$fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
$fep.Name = "fep1"
$fep.Port = 80
```

<span data-ttu-id="2548e-179">Vytvořte fond back-end serveru.</span><span class="sxs-lookup"><span data-stu-id="2548e-179">Create the back-end server pool.</span></span>

<span data-ttu-id="2548e-180">Definujte IP adresy, které se přidají do fondu back-end serverů, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="2548e-180">Define the IP addresses that are added to the back-end server pool as shown in the next example.</span></span>

```powershell
$servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
$servers.Add("10.0.0.1")
$servers.Add("10.0.0.2")
```

<span data-ttu-id="2548e-181">Pomocí objektu $server přidejte hodnoty do objektu back-end fondu ($pool).</span><span class="sxs-lookup"><span data-stu-id="2548e-181">Use the $server object to add the values to the back-end pool object ($pool).</span></span>

```powershell
$pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
$pool.BackendServers = $servers
$pool.Name = "pool1"
```

<span data-ttu-id="2548e-182">Vytvořte nastavení fondu back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="2548e-182">Create the back-end server pool setting.</span></span>

```powershell
$setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
$setting.Name = "setting1"
$setting.CookieBasedAffinity = "enabled"
$setting.Port = 80
$setting.Protocol = "http"
```

<span data-ttu-id="2548e-183">Vytvořte naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="2548e-183">Create the listener.</span></span>

```powershell
$listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
$listener.Name = "listener1"
$listener.FrontendPort = "fep1"
$listener.FrontendIP = "fip1"
$listener.Protocol = "http"
$listener.SslCert = ""
```

<span data-ttu-id="2548e-184">Vytvořte pravidlo.</span><span class="sxs-lookup"><span data-stu-id="2548e-184">Create the rule.</span></span>

```powershell
$rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
$rule.Name = "rule1"
$rule.Type = "basic"
$rule.BackendHttpSettings = "setting1"
$rule.Listener = "listener1"
$rule.BackendAddressPool = "pool1"
```

#### <a name="step-2"></a><span data-ttu-id="2548e-185">Krok 2</span><span class="sxs-lookup"><span data-stu-id="2548e-185">Step 2</span></span>

<span data-ttu-id="2548e-186">Přiřaďte všechny položky individuální konfigurace objektu konfigurace aplikační brány ($appgwconfig).</span><span class="sxs-lookup"><span data-stu-id="2548e-186">Assign all individual configuration items to an application gateway configuration object ($appgwconfig).</span></span>

<span data-ttu-id="2548e-187">Přidejte front-end IP adresu ke konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="2548e-187">Add the front-end IP to the configuration.</span></span>

```powershell
$appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
$appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
$appgwconfig.FrontendIPConfigurations.Add($fip)
```

<span data-ttu-id="2548e-188">Přidejte front-end port ke konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="2548e-188">Add the front-end port to the configuration.</span></span>

```powershell
$appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
$appgwconfig.FrontendPorts.Add($fep)
```
<span data-ttu-id="2548e-189">Přidejte fond back-end serverů ke konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="2548e-189">Add the back-end server pool to the configuration.</span></span>

```powershell
$appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
$appgwconfig.BackendAddressPools.Add($pool)
```

<span data-ttu-id="2548e-190">Přidejte nastavení back-end fondu ke konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="2548e-190">Add the back-end pool setting to the configuration.</span></span>

```powershell
$appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
$appgwconfig.BackendHttpSettingsList.Add($setting)
```

<span data-ttu-id="2548e-191">Přidejte naslouchací proces ke konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="2548e-191">Add the listener to the configuration.</span></span>

```powershell
$appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
$appgwconfig.HttpListeners.Add($listener)
```

<span data-ttu-id="2548e-192">Přidejte pravidlo ke konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="2548e-192">Add the rule to the configuration.</span></span>

```powershell
$appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
$appgwconfig.HttpLoadBalancingRules.Add($rule)
```

### <a name="step-3"></a><span data-ttu-id="2548e-193">Krok 3</span><span class="sxs-lookup"><span data-stu-id="2548e-193">Step 3</span></span>
<span data-ttu-id="2548e-194">Potvrďte objekt konfigurace k prostředku služby Application Gateway pomocí rutiny `Set-AzureApplicationGatewayConfig`.</span><span class="sxs-lookup"><span data-stu-id="2548e-194">Commit the configuration object to the application gateway resource by using `Set-AzureApplicationGatewayConfig`.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig
```

## <a name="start-the-gateway"></a><span data-ttu-id="2548e-195">Spusťte bránu</span><span class="sxs-lookup"><span data-stu-id="2548e-195">Start the gateway</span></span>

<span data-ttu-id="2548e-196">Jakmile se brána nakonfiguruje, pomocí rutiny `Start-AzureApplicationGateway` ji spusťte.</span><span class="sxs-lookup"><span data-stu-id="2548e-196">Once the gateway has been configured, use the `Start-AzureApplicationGateway` cmdlet to start the gateway.</span></span> <span data-ttu-id="2548e-197">Fakturace aplikační brány se spustí až po úspěšném spuštění brány.</span><span class="sxs-lookup"><span data-stu-id="2548e-197">Billing for an application gateway begins after the gateway has been successfully started.</span></span>

> [!NOTE]
> <span data-ttu-id="2548e-198">Dokončení rutiny `Start-AzureApplicationGateway` může trvat 15 až 20 minut.</span><span class="sxs-lookup"><span data-stu-id="2548e-198">The `Start-AzureApplicationGateway` cmdlet might take up to 15-20 minutes to finish.</span></span>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-the-gateway-status"></a><span data-ttu-id="2548e-199">Ověřte stav brány.</span><span class="sxs-lookup"><span data-stu-id="2548e-199">Verify the gateway status</span></span>

<span data-ttu-id="2548e-200">Pomocí rutiny `Get-AzureApplicationGateway` zkontrolujte stav brány.</span><span class="sxs-lookup"><span data-stu-id="2548e-200">Use the `Get-AzureApplicationGateway` cmdlet to check the status of the gateway.</span></span> <span data-ttu-id="2548e-201">Pokud se v předcházejícím kroku podařilo úspěšně spustit rutinu `Start-AzureApplicationGateway`, položka *State* (Stav) by měla mít hodnotu Running (Spuštěno) a *Vip* a *DnsName* by měly obsahovat platné položky.</span><span class="sxs-lookup"><span data-stu-id="2548e-201">If `Start-AzureApplicationGateway` succeeded in the previous step, *State* should be Running, and *Vip* and *DnsName* should have valid entries.</span></span>

<span data-ttu-id="2548e-202">Následující příklad ukazuje aplikační bránu, která je aktivní, spuštěná a připravená přijmout provoz určený pro `http://<generated-dns-name>.cloudapp.net`.</span><span class="sxs-lookup"><span data-stu-id="2548e-202">The following example shows an application gateway that is up, running, and ready to take traffic destined for `http://<generated-dns-name>.cloudapp.net`.</span></span>

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

## <a name="delete-the-application-gateway"></a><span data-ttu-id="2548e-203">Odstranění služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="2548e-203">Delete the application gateway</span></span>

<span data-ttu-id="2548e-204">Chcete-li odstranit službu Application Gateway:</span><span class="sxs-lookup"><span data-stu-id="2548e-204">To delete the application gateway:</span></span>

1. <span data-ttu-id="2548e-205">Pomocí rutiny `Stop-AzureApplicationGateway` zastavte bránu.</span><span class="sxs-lookup"><span data-stu-id="2548e-205">Use the `Stop-AzureApplicationGateway` cmdlet to stop the gateway.</span></span>
2. <span data-ttu-id="2548e-206">Pomocí rutiny `Remove-AzureApplicationGateway` bránu odeberte.</span><span class="sxs-lookup"><span data-stu-id="2548e-206">Use the `Remove-AzureApplicationGateway` cmdlet to remove the gateway.</span></span>
3. <span data-ttu-id="2548e-207">Zkontrolujte odstranění brány pomocí rutiny `Get-AzureApplicationGateway`.</span><span class="sxs-lookup"><span data-stu-id="2548e-207">Verify that the gateway has been removed by using the `Get-AzureApplicationGateway` cmdlet.</span></span>

<span data-ttu-id="2548e-208">Následující příklad ukazuje rutinu `Stop-AzureApplicationGateway` na prvním řádku, následovanou výstupem.</span><span class="sxs-lookup"><span data-stu-id="2548e-208">The following example shows the `Stop-AzureApplicationGateway` cmdlet on the first line, followed by the output.</span></span>

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

<span data-ttu-id="2548e-209">Jakmile je služba Application Gateway v zastaveném stavu, pomocí rutiny `Remove-AzureApplicationGateway` službu odstraňte.</span><span class="sxs-lookup"><span data-stu-id="2548e-209">Once the application gateway is in a stopped state, use the `Remove-AzureApplicationGateway` cmdlet to remove the service.</span></span>

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

<span data-ttu-id="2548e-210">Pokud chcete zkontrolovat, že se služba odstranila, můžete použít rutinu `Get-AzureApplicationGateway`.</span><span class="sxs-lookup"><span data-stu-id="2548e-210">To verify that the service has been removed, you can use the `Get-AzureApplicationGateway` cmdlet.</span></span> <span data-ttu-id="2548e-211">Tenhle krok není povinný.</span><span class="sxs-lookup"><span data-stu-id="2548e-211">This step is not required.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
.....
```

## <a name="next-steps"></a><span data-ttu-id="2548e-212">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2548e-212">Next steps</span></span>

<span data-ttu-id="2548e-213">Pokud chcete konfigurovat přesměrování zpracování SSL, přejděte do části [Konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="2548e-213">If you want to configure SSL offload, see [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="2548e-214">Pokud chcete provést konfiguraci aplikační brány pro použití s interním nástrojem pro vyrovnávání zatížení, přečtěte si část [Vytvoření aplikační brány s interním nástrojem pro vyrovnávání zatížení (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="2548e-214">If you want to configure an application gateway to use with an internal load balancer, see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="2548e-215">Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:</span><span class="sxs-lookup"><span data-stu-id="2548e-215">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="2548e-216">Nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="2548e-216">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="2548e-217">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="2548e-217">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png
