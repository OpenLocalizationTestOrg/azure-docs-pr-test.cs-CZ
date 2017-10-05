---
title: "Jak používat Azure API Management ve virtuální síti s aplikační brány | Microsoft Docs"
description: "Zjistěte, jak nainstalovat a nakonfigurovat Azure API Management v interní virtuální síť s aplikací brány (firewall webových aplikací) jako front-endu"
services: api-management
documentationcenter: 
author: solankisamir
manager: kjoshi
editor: antonba
ms.assetid: a8c982b2-bca5-4312-9367-4a0bbc1082b1
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2017
ms.author: sasolank
ms.openlocfilehash: 8131ded6b74e9c544bf70b1a4659ed07e5def04d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a><span data-ttu-id="e9458-103">Integrovat správu rozhraní API v interní virtuální síť s aplikační brány</span><span class="sxs-lookup"><span data-stu-id="e9458-103">Integrate API Management in an internal VNET with Application Gateway</span></span> 

##<span data-ttu-id="e9458-104"><a name="overview"></a> – Přehled</span><span class="sxs-lookup"><span data-stu-id="e9458-104"><a name="overview"> </a> Overview</span></span>
 
<span data-ttu-id="e9458-105">Služba API Management můžete nakonfigurovat ve virtuální síti v interní režimu, který je dostupný jenom z virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="e9458-105">The API Management service can be configured in a Virtual Network in internal mode which makes it accessible only from within the Virtual Network.</span></span> <span data-ttu-id="e9458-106">Služba Azure Application Gateway je služba PAAS, které poskytuje nástroj pro vyrovnávání zatížení vrstvy 7.</span><span class="sxs-lookup"><span data-stu-id="e9458-106">Azure Application Gateway is a PAAS Service which provides a Layer-7 load balancer.</span></span> <span data-ttu-id="e9458-107">To funguje jako služba reverznímu proxy serveru a poskytuje mezi jeho nabídky webové aplikace brány Firewall (firewall webových aplikací).</span><span class="sxs-lookup"><span data-stu-id="e9458-107">It acts as a reverse-proxy service and provides among its offering a Web Application Firewall (WAF).</span></span>

<span data-ttu-id="e9458-108">Kombinování API Management zřízené v interní virtuální síti VNET s front-endu Application Gateway umožňuje následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="e9458-108">Combining API Management provisioned in an internal VNET with the Application Gateway frontend enables the following scenarios:</span></span>

* <span data-ttu-id="e9458-109">Použijte stejné rozhraní API správy prostředků pro spotřeba příjemci interní i externí spotřebitelů.</span><span class="sxs-lookup"><span data-stu-id="e9458-109">Use the same API Management resource for consumption by both internal consumers and external consumers.</span></span>
* <span data-ttu-id="e9458-110">Použít jeden zdroj API Management a mít podmnožinu rozhraní API definované ve službě API Management, které jsou k dispozici pro externí příjemci.</span><span class="sxs-lookup"><span data-stu-id="e9458-110">Use a single API Management resource and have a subset of APIs defined in API Management available for external consumers.</span></span>
* <span data-ttu-id="e9458-111">Zadejte klíč způsob, jak zapnout a vypnout přepnout přístup k rozhraní API správy z veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="e9458-111">Provide a turn-key way to switch access to API Management from the public Internet on and off.</span></span> 

##<span data-ttu-id="e9458-112"><a name="scenario"></a> Scénář</span><span class="sxs-lookup"><span data-stu-id="e9458-112"><a name="scenario"> </a> Scenario</span></span>
<span data-ttu-id="e9458-113">Tento článek se zabývá pomocí jedné služby API Management pro interní i externí uživatelé a nastavit jej jako jeden front-end pro obě místní a cloudové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e9458-113">This article will cover how to use a single API Management service for both internal and external consumers and make it act as a single frontend for both on-prem and cloud APIs.</span></span> <span data-ttu-id="e9458-114">Zobrazí se taky, jak vystavit pouze podmnožina vašich rozhraní API (v příkladu, které jsou vyznačené na zelená) pro externí spotřebu pomocí funkce PathBasedRouting v aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="e9458-114">You will also see how to expose only a subset of your APIs (in the example they are highlighted in green) for External Consumption using the PathBasedRouting functionality available in Application Gateway.</span></span>

<span data-ttu-id="e9458-115">V prvním příkladu instalace všechna rozhraní API spravují pouze v rámci virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="e9458-115">In the first setup example all your APIs are managed only from within your Virtual Network.</span></span> <span data-ttu-id="e9458-116">Interní příjemci (zvýraznit v oranžová) mají přístup všechny vaše interní a externí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e9458-116">Internal consumers (highlighted in orange) can access all your internal and external APIs.</span></span> <span data-ttu-id="e9458-117">Provoz nikdy nedostane mimo vysoký výkon doručuje Internet prostřednictvím okruhy Expressroute.</span><span class="sxs-lookup"><span data-stu-id="e9458-117">Traffic never goes out to Internet a high performance is delivered via Express Route circuits.</span></span>

![Adresa URL trasy](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <span data-ttu-id="e9458-119"><a name="before-you-begin"></a> Před zahájením</span><span class="sxs-lookup"><span data-stu-id="e9458-119"><a name="before-you-begin"> </a> Before you begin</span></span>

1. <span data-ttu-id="e9458-120">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell pomocí instalační služby webové platformy.</span><span class="sxs-lookup"><span data-stu-id="e9458-120">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="e9458-121">Nejnovější verzi můžete stáhnout a nainstalovat v části **Windows PowerShell** na stránce [Položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="e9458-121">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="e9458-122">Vytvoření virtuální sítě a vytvořte oddělené podsítě pro API Management a aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="e9458-122">Create a Virtual Network and create separate subnets for API Management and Application Gateway.</span></span> 
3. <span data-ttu-id="e9458-123">Pokud máte v úmyslu vytvořit vlastního serveru DNS pro virtuální síť, můžete tak učiňte před zahájením nasazení.</span><span class="sxs-lookup"><span data-stu-id="e9458-123">If you intend to create a custom DNS server for the Virtual Network, do so before starting the deployment.</span></span> <span data-ttu-id="e9458-124">Překontrolujte, který funguje zajištěním virtuální počítač vytvořený v novou podsíť ve virtuální síti můžete vyřešit a získat přístup k všechny koncové body služby Azure.</span><span class="sxs-lookup"><span data-stu-id="e9458-124">Double check it works by ensuring a virtual machine created in a new subnet in the Virtual Network can resolve and access all Azure service endpoints.</span></span>

## <a name="what-is-required-to-create-an-integration-between-api-management-and-application-gateway"></a><span data-ttu-id="e9458-125">Co je potřeba k vytvoření integraci mezi API Management a Application Gateway?</span><span class="sxs-lookup"><span data-stu-id="e9458-125">What is required to create an integration between API Management and Application Gateway?</span></span>

* <span data-ttu-id="e9458-126">**Fond back-end serverů:** Toto je interní virtuální IP adresu služby API Management.</span><span class="sxs-lookup"><span data-stu-id="e9458-126">**Back-end server pool:** This is the internal virtual IP address of the API Management service.</span></span>
* <span data-ttu-id="e9458-127">**Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="e9458-127">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="e9458-128">Tato nastavení se použijí na všechny servery v rámci fondu.</span><span class="sxs-lookup"><span data-stu-id="e9458-128">These settings are applied to all servers within the pool.</span></span>
* <span data-ttu-id="e9458-129">**Front-end port:** Toto je veřejný port, který se otevírá ve službě application gateway.</span><span class="sxs-lookup"><span data-stu-id="e9458-129">**Front-end port:** This is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="e9458-130">Získá provoz stiskne ho přesměruje na jeden z back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="e9458-130">Traffic hitting it gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="e9458-131">**Naslouchací proces:** Naslouchací proces má front-end port, protokol (Http nebo Https, u těchto hodnot se rozlišují malá a velká písmena) a název certifikátu SSL (pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="e9458-131">**Listener:** The listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="e9458-132">**Pravidlo:** pravidlo váže naslouchací proces pro fond back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="e9458-132">**Rule:** The rule binds a listener to a back-end server pool.</span></span>
* <span data-ttu-id="e9458-133">**Vlastní test stavu:** Application Gateway, standardně používá IP adresu, na základě sondy zjistěte, které servery v BackendAddressPool jsou aktivní.</span><span class="sxs-lookup"><span data-stu-id="e9458-133">**Custom Health Probe:** Application Gateway, by default, uses IP address based probes to figure out which servers in the BackendAddressPool are active.</span></span> <span data-ttu-id="e9458-134">Rozhraní API správy, které služba pouze reaguje na požadavky, které mají hlavičku hostitele správná, proto sondy výchozí nezdaří.</span><span class="sxs-lookup"><span data-stu-id="e9458-134">The API Management service only responds to requests which have the correct host header, hence the default probes fail.</span></span> <span data-ttu-id="e9458-135">Test vlastní stavu musí být definován pomohou určit, že služba není aktivní a předávat požadavky aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="e9458-135">A custom health probe needs to be defined to help application gateway determine that the service is alive and it should forward requests.</span></span>
* <span data-ttu-id="e9458-136">**Certifikát vlastní domény:** pro přístup k rozhraní API správy z Internetu, je potřeba vytvořit mapování CNAME z jeho názvu hostitele k názvu DNS front-end pro aplikační bránu.</span><span class="sxs-lookup"><span data-stu-id="e9458-136">**Custom domain certificate:** To access API Management from the internet you need to create a CNAME mapping of its hostname to the Application Gateway front-end DNS name.</span></span> <span data-ttu-id="e9458-137">To zajišťuje, že název hostitele záhlaví a certifikát odeslat aplikační bránu, která se předají do rozhraní API správy jednoho APIM rozpoznat jako platný.</span><span class="sxs-lookup"><span data-stu-id="e9458-137">This ensures that the hostname header and certificate sent to Application Gateway that is forwarded to API Management is one APIM can recognize as valid.</span></span>

## <span data-ttu-id="e9458-138"><a name="overview-steps"></a> Kroky potřebné k integraci API Management a aplikační brány</span><span class="sxs-lookup"><span data-stu-id="e9458-138"><a name="overview-steps"> </a> Steps required for integrating API Management and Application Gateway</span></span> 

1. <span data-ttu-id="e9458-139">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e9458-139">Create a resource group for Resource Manager.</span></span>
2. <span data-ttu-id="e9458-140">Vytvořte virtuální síť, podsíť a veřejnou IP adresu pro aplikační bránu.</span><span class="sxs-lookup"><span data-stu-id="e9458-140">Create a Virtual Network, subnet, and public IP for the Application Gateway.</span></span> <span data-ttu-id="e9458-141">Vytvořte další podsítě pro API Management.</span><span class="sxs-lookup"><span data-stu-id="e9458-141">Create another subnet for API Management.</span></span>
3. <span data-ttu-id="e9458-142">Vytvoření služby API Management uvnitř vytvořili výše podsíť virtuální sítě a ujistěte se, že používáte interní režim.</span><span class="sxs-lookup"><span data-stu-id="e9458-142">Create an API Management service inside the VNET subnet created above and ensure you use the Internal mode.</span></span>
4. <span data-ttu-id="e9458-143">Nastavte vlastní název domény ve službě API Management.</span><span class="sxs-lookup"><span data-stu-id="e9458-143">Setup the custom domain name in the API Management service.</span></span>
5. <span data-ttu-id="e9458-144">Vytvoření objektu konfigurace služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="e9458-144">Create an Application Gateway configuration object.</span></span>
6. <span data-ttu-id="e9458-145">Vytvořte prostředek aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="e9458-145">Create an Application Gateway resource.</span></span>
7. <span data-ttu-id="e9458-146">Vytvořte záznam CNAME z veřejného názvu DNS aplikační brány pro název hostitele proxy API Management.</span><span class="sxs-lookup"><span data-stu-id="e9458-146">Create a CNAME from the public DNS name of the Application Gateway to the API Management proxy hostname.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="e9458-147">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e9458-147">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="e9458-148">Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e9458-148">Make sure that you are using the latest version of Azure PowerShell.</span></span> <span data-ttu-id="e9458-149">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="e9458-149">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="e9458-150">Krok 1</span><span class="sxs-lookup"><span data-stu-id="e9458-150">Step 1</span></span>

<span data-ttu-id="e9458-151">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="e9458-151">Log in to Azure</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="e9458-152">Ověření pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="e9458-152">Authenticate with your credentials.</span></span><BR>

### <a name="step-2"></a><span data-ttu-id="e9458-153">Krok 2</span><span class="sxs-lookup"><span data-stu-id="e9458-153">Step 2</span></span>

<span data-ttu-id="e9458-154">Zkontrolujte předplatná pro příslušný účet a vyberte jej.</span><span class="sxs-lookup"><span data-stu-id="e9458-154">Check the subscriptions for the account and select it.</span></span>

```powershell
Get-AzureRmSubscription -Subscriptionid "GUID of subscription" | Select-AzureRmSubscription
```

### <a name="step-3"></a><span data-ttu-id="e9458-155">Krok 3</span><span class="sxs-lookup"><span data-stu-id="e9458-155">Step 3</span></span>

<span data-ttu-id="e9458-156">Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).</span><span class="sxs-lookup"><span data-stu-id="e9458-156">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name "apim-appGw-RG" -Location "West US"
```
<span data-ttu-id="e9458-157">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="e9458-157">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="e9458-158">To slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="e9458-158">This is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="e9458-159">Ujistěte se, že všechny příkazy k vytvoření služby application gateway používají stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="e9458-159">Make sure that all commands to create an application gateway use the same resource group.</span></span>

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a><span data-ttu-id="e9458-160">Vytvořte virtuální síť a podsíť pro aplikační bránu</span><span class="sxs-lookup"><span data-stu-id="e9458-160">Create a Virtual Network and a subnet for the application gateway</span></span>

<span data-ttu-id="e9458-161">Následující příklad ukazuje, jak vytvořit virtuální síť pomocí prostředek správci.</span><span class="sxs-lookup"><span data-stu-id="e9458-161">The following example shows how to create a Virtual Network using the resource manager.</span></span>

### <a name="step-1"></a><span data-ttu-id="e9458-162">Krok 1</span><span class="sxs-lookup"><span data-stu-id="e9458-162">Step 1</span></span>

<span data-ttu-id="e9458-163">Rozsah adres 10.0.0.0/24 přiřadí proměnné podsítě, který má být použit pro službu Application Gateway při vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="e9458-163">Assign the address range 10.0.0.0/24 to the subnet variable to be used for Application Gateway while creating a Virtual Network.</span></span>

```powershell
$appgatewaysubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a><span data-ttu-id="e9458-164">Krok 2</span><span class="sxs-lookup"><span data-stu-id="e9458-164">Step 2</span></span>

<span data-ttu-id="e9458-165">10.0.1.0/24 rozsah adres přiřadí proměnné podsítě, který má být použit pro API Management při vytvoření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="e9458-165">Assign the address range 10.0.1.0/24 to the subnet variable to be used for API Management while creating a Virtual Network.</span></span>

```powershell
$apimsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a><span data-ttu-id="e9458-166">Krok 3</span><span class="sxs-lookup"><span data-stu-id="e9458-166">Step 3</span></span>

<span data-ttu-id="e9458-167">Vytvořit virtuální síť s názvem **appgwvnet** ve skupině prostředků **apim-appGw-RG** pro oblast západní USA pomocí předpony 10.0.0.0/16 s podsítí 10.0.0.0/24 a 10.0.1.0/24.</span><span class="sxs-lookup"><span data-stu-id="e9458-167">Create a Virtual Network named **appgwvnet** in resource group **apim-appGw-RG** for the West US region using the prefix 10.0.0.0/16 with subnets 10.0.0.0/24 and 10.0.1.0/24.</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name "appgwvnet" -ResourceGroupName "apim-appGw-RG" -Location "West US" -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a><span data-ttu-id="e9458-168">Krok 4</span><span class="sxs-lookup"><span data-stu-id="e9458-168">Step 4</span></span>

<span data-ttu-id="e9458-169">Přiřaďte proměnnou podsítě pro další kroky</span><span class="sxs-lookup"><span data-stu-id="e9458-169">Assign a subnet variable for the next steps</span></span>

```powershell
$appgatewaysubnetdata=$vnet.Subnets[0]
$apimsubnetdata=$vnet.Subnets[1]
```
## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a><span data-ttu-id="e9458-170">Vytvoření služby API Management uvnitř nakonfigurován v režimu interní virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="e9458-170">Create an API Management service inside a VNET configured in internal mode</span></span>

<span data-ttu-id="e9458-171">Následující příklad ukazuje postup vytvoření služby API Management ve virtuální síti nakonfigurován pro interní přístup jenom.</span><span class="sxs-lookup"><span data-stu-id="e9458-171">The following example shows how to create an API Management service in a VNET configured for internal access only.</span></span>

### <a name="step-1"></a><span data-ttu-id="e9458-172">Krok 1</span><span class="sxs-lookup"><span data-stu-id="e9458-172">Step 1</span></span>
<span data-ttu-id="e9458-173">Vytvořte virtuální síť pro správu rozhraní API objekt, který používá podsíť $apimsubnetdata vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="e9458-173">Create an API Management Virtual Network object using the subnet $apimsubnetdata created above.</span></span>

```powershell
$apimVirtualNetwork = New-AzureRmApiManagementVirtualNetwork -Location "West US" -SubnetResourceId $apimsubnetdata.Id
```
### <a name="step-2"></a><span data-ttu-id="e9458-174">Krok 2</span><span class="sxs-lookup"><span data-stu-id="e9458-174">Step 2</span></span>
<span data-ttu-id="e9458-175">Vytvoření služby API Management ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="e9458-175">Create an API Management service inside the Virtual Network.</span></span>

```powershell
$apimService = New-AzureRmApiManagement -ResourceGroupName "apim-appGw-RG" -Location "West US" -Name "ContosoApi" -Organization "Contoso" -AdminEmail "admin@contoso.com" -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```
<span data-ttu-id="e9458-176">Po úspěšném provedení výše uvedeném příkazu odkazovat na [DNS konfigurace požadovaná pro přístup k interní virtuální síť rozhraní API správy služby](api-management-using-with-internal-vnet.md#apim-dns-configuration) k přístupu.</span><span class="sxs-lookup"><span data-stu-id="e9458-176">After the above command succeeds refer to [DNS Configuration required to access internal VNET API Management service](api-management-using-with-internal-vnet.md#apim-dns-configuration) to access it.</span></span>

## <a name="set-up-a-custom-domain-name-in-api-management"></a><span data-ttu-id="e9458-177">Nastavit vlastní název domény ve službě API Management</span><span class="sxs-lookup"><span data-stu-id="e9458-177">Set-up a custom domain name in API Management</span></span>

### <a name="step-1"></a><span data-ttu-id="e9458-178">Krok 1</span><span class="sxs-lookup"><span data-stu-id="e9458-178">Step 1</span></span>
<span data-ttu-id="e9458-179">Nahrajte certifikát s privátním klíčem pro doménu.</span><span class="sxs-lookup"><span data-stu-id="e9458-179">Upload the certificate with private key for the domain.</span></span> <span data-ttu-id="e9458-180">V tomto příkladu bude `*.contoso.net`.</span><span class="sxs-lookup"><span data-stu-id="e9458-180">For this example it will be `*.contoso.net`.</span></span> 

```powershell
$certUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName "apim-appGw-RG" -Name "ContosoApi" -HostnameType "Proxy" -PfxPath <full path to .pfx file> -PfxPassword <password for certificate file> -PassThru
```

### <a name="step-2"></a><span data-ttu-id="e9458-181">Krok 2</span><span class="sxs-lookup"><span data-stu-id="e9458-181">Step 2</span></span>
<span data-ttu-id="e9458-182">Po nahrání certifikátu vytvořit objekt konfigurace název hostitele pro proxy server s název hostitele `api.contoso.net`, jako například certifikát poskytuje autority pro `*.contoso.net` domény.</span><span class="sxs-lookup"><span data-stu-id="e9458-182">Once the certificate is uploaded, create a hostname configuration object for the proxy with a hostname of `api.contoso.net`, as the example certificate provides authority for the  `*.contoso.net` domain.</span></span> 

```powershell
$proxyHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certUploadResult.Thumbprint -Hostname "api.contoso.net"
$result = Set-AzureRmApiManagementHostnames -Name "ContosoApi" -ResourceGroupName "apim-appGw-RG" -ProxyHostnameConfiguration $proxyHostnameConfig
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a><span data-ttu-id="e9458-183">Vytvoření veřejné IP adresy pro front-end konfiguraci</span><span class="sxs-lookup"><span data-stu-id="e9458-183">Create a public IP address for the front-end configuration</span></span>

<span data-ttu-id="e9458-184">Vytvořte prostředek veřejné IP **adresy publicIP01** ve skupině prostředků **apim-appGw-RG** pro oblast západní USA.</span><span class="sxs-lookup"><span data-stu-id="e9458-184">Create a public IP resource **publicIP01** in resource group **apim-appGw-RG** for the West US region.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -name "publicIP01" -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="e9458-185">IP adresa je ke službě Application Gateway přiřazena při spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="e9458-185">An IP address is assigned to the application gateway when the service starts.</span></span>

## <a name="create-application-gateway-configuration"></a><span data-ttu-id="e9458-186">Vytvoření konfigurace brány aplikace</span><span class="sxs-lookup"><span data-stu-id="e9458-186">Create application gateway configuration</span></span>

<span data-ttu-id="e9458-187">Před vytvořením služby Application Gateway musí být nastaveny všechny položky konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e9458-187">All configuration items must be set up before creating the application gateway.</span></span> <span data-ttu-id="e9458-188">Následující kroky slouží k vytvoření položek konfigurace potřebné pro prostředek služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="e9458-188">The following steps create the configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="e9458-189">Krok 1</span><span class="sxs-lookup"><span data-stu-id="e9458-189">Step 1</span></span>

<span data-ttu-id="e9458-190">Vytvořte konfiguraci IP adresy služby Application Gateway s názvem **gatewayIP01**.</span><span class="sxs-lookup"><span data-stu-id="e9458-190">Create an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="e9458-191">Při spuštění služby Application Gateway se předá IP adresa z nakonfigurované podsítě a síťový provoz se bude směrovat na IP adresy ve fondu back-end IP adres.</span><span class="sxs-lookup"><span data-stu-id="e9458-191">When Application Gateway starts, it picks up an IP address from the subnet configured and route network traffic to the IP addresses in the back-end IP pool.</span></span> <span data-ttu-id="e9458-192">Uvědomte si, že každá instance vyžaduje jednu IP adresu.</span><span class="sxs-lookup"><span data-stu-id="e9458-192">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a><span data-ttu-id="e9458-193">Krok 2</span><span class="sxs-lookup"><span data-stu-id="e9458-193">Step 2</span></span>

<span data-ttu-id="e9458-194">Nakonfigurujte port front-end IP pro koncový bod veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="e9458-194">Configure the front-end IP port for the public IP endpoint.</span></span> <span data-ttu-id="e9458-195">Tento port je port, který koncoví uživatelé připojit k.</span><span class="sxs-lookup"><span data-stu-id="e9458-195">This port is the port that end users connect to.</span></span>

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "port01"  -Port 443
```
### <a name="step-3"></a><span data-ttu-id="e9458-196">Krok 3</span><span class="sxs-lookup"><span data-stu-id="e9458-196">Step 3</span></span>

<span data-ttu-id="e9458-197">Nakonfigurujte IP adresu front-endu s koncovým bodem s veřejnou IP adresou.</span><span class="sxs-lookup"><span data-stu-id="e9458-197">Configure the front-end IP with public IP endpoint.</span></span>

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a><span data-ttu-id="e9458-198">Krok 4</span><span class="sxs-lookup"><span data-stu-id="e9458-198">Step 4</span></span>

<span data-ttu-id="e9458-199">Nakonfigurujte certifikát pro službu Application Gateway, používá k dešifrování a znovu zašifrovat přenosy procházející.</span><span class="sxs-lookup"><span data-stu-id="e9458-199">Configure the certificate for the Application Gateway, used to decrypt and re-encrypt the traffic passing through.</span></span>

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name "cert01" -CertificateFile <full path to .pfx file> -Password <password for certificate file>
```

### <a name="step-5"></a><span data-ttu-id="e9458-200">Krok 5</span><span class="sxs-lookup"><span data-stu-id="e9458-200">Step 5</span></span>

<span data-ttu-id="e9458-201">Vytvořte naslouchací proces protokolu HTTP pro službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="e9458-201">Create the HTTP listener for the Application Gateway.</span></span> <span data-ttu-id="e9458-202">Jí přiřadíte front-end IP certifikát konfigurace, port a protokol ssl.</span><span class="sxs-lookup"><span data-stu-id="e9458-202">Assign the front-end IP configuration, port, and ssl certificate to it.</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert
```

### <a name="step-6"></a><span data-ttu-id="e9458-203">Krok 6</span><span class="sxs-lookup"><span data-stu-id="e9458-203">Step 6</span></span>

<span data-ttu-id="e9458-204">Vytvořit vlastní test paměti do služby API Management `ContosoApi` koncový bod domény proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="e9458-204">Create a custom probe to the API Management service `ContosoApi` proxy domain endpoint.</span></span> <span data-ttu-id="e9458-205">Cesta `/status-0123456789abcdef` je výchozího koncového bodu stavu hostované na všechny služby API Management.</span><span class="sxs-lookup"><span data-stu-id="e9458-205">The path `/status-0123456789abcdef` is a default health endpoint hosted on all the API Management services.</span></span> <span data-ttu-id="e9458-206">Nastavit `api.contoso.net` jako název hostitele vlastní test paměti k zabezpečení s certifikátem SSL.</span><span class="sxs-lookup"><span data-stu-id="e9458-206">Set `api.contoso.net` as a custom probe hostname to secure it with SSL certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="e9458-207">Název hostitele `contosoapi.azure-api.net` je název hostitele proxy výchozí nakonfigurované služby s názvem `contosoapi` je vytvořen v veřejný Azure.</span><span class="sxs-lookup"><span data-stu-id="e9458-207">The hostname `contosoapi.azure-api.net` is the default proxy hostname configured when a service named `contosoapi` is created in public Azure.</span></span> 
> 

```powershell
$apimprobe = New-AzureRmApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName "api.contoso.net" -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
```

### <a name="step-7"></a><span data-ttu-id="e9458-208">Krok 7</span><span class="sxs-lookup"><span data-stu-id="e9458-208">Step 7</span></span>

<span data-ttu-id="e9458-209">Nahrajte certifikát, který chcete použít pro protokol SSL povolen back-end fondu prostředků.</span><span class="sxs-lookup"><span data-stu-id="e9458-209">Upload the certificate to be used on the SSL-enabled backend pool resources.</span></span> <span data-ttu-id="e9458-210">Toto je stejný certifikát, který jste zadali v kroku 4 výše.</span><span class="sxs-lookup"><span data-stu-id="e9458-210">This is the same certificate which you provided in Step 4 above.</span></span>

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile <full path to .cer file>
```

### <a name="step-8"></a><span data-ttu-id="e9458-211">Krok 8</span><span class="sxs-lookup"><span data-stu-id="e9458-211">Step 8</span></span>

<span data-ttu-id="e9458-212">Nakonfigurujte nastavení HTTP back-end pro službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="e9458-212">Configure HTTP backend settings for the Application Gateway.</span></span> <span data-ttu-id="e9458-213">To zahrnuje nastavení časového limitu pro požadavek back-end, po jejímž uplynutí se zrušil.</span><span class="sxs-lookup"><span data-stu-id="e9458-213">This includes setting a time-out limit for backend request after which they are cancelled.</span></span> <span data-ttu-id="e9458-214">Tato hodnota se liší od časový limit testu.</span><span class="sxs-lookup"><span data-stu-id="e9458-214">This value is different from the probe time-out.</span></span>

```powershell
$apimPoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a><span data-ttu-id="e9458-215">Krok 9</span><span class="sxs-lookup"><span data-stu-id="e9458-215">Step 9</span></span>

<span data-ttu-id="e9458-216">Nakonfigurujte fond back-end IP adresy s názvem **apimbackend** s interní virtuální IP adresu služby API Management vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="e9458-216">Configure a back-end IP address pool named **apimbackend**  with the internal virtual IP address of the API Management service created above.</span></span>

```powershell
$apimProxyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.StaticIPs[0]
```

### <a name="step-10"></a><span data-ttu-id="e9458-217">Krok 10</span><span class="sxs-lookup"><span data-stu-id="e9458-217">Step 10</span></span>

<span data-ttu-id="e9458-218">Vytvořte nastavení pro fiktivní back-end (neexistující).</span><span class="sxs-lookup"><span data-stu-id="e9458-218">Create settings for a dummy (non-existent) backend.</span></span> <span data-ttu-id="e9458-219">Požadavky na rozhraní API cesty, které jsme nechcete vystavit ze správy rozhraní API prostřednictvím brány aplikace bude dosáhl tento back-end a vrátí 404.</span><span class="sxs-lookup"><span data-stu-id="e9458-219">Requests to API paths that we do not want to expose from API Management via Application Gateway will hit this backend and return 404.</span></span>

<span data-ttu-id="e9458-220">Nakonfigurujte nastavení protokolu HTTP pro fiktivní back-end.</span><span class="sxs-lookup"><span data-stu-id="e9458-220">Configure HTTP settings for the dummy backend.</span></span>

```powershell
$dummyBackendSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "dummySetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

<span data-ttu-id="e9458-221">Konfigurace fiktivní back-end **dummyBackendPool**, která odkazuje na adresu plně kvalifikovaný název domény **dummybackend.com**. Tato adresa plně kvalifikovaný název domény ve virtuální síti neexistuje.</span><span class="sxs-lookup"><span data-stu-id="e9458-221">Configure a dummy backend **dummyBackendPool**, which points to a FQDN address **dummybackend.com**. This FQDN address does not exist in the virtual network.</span></span>

```powershell
$dummyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "dummyBackendPool" -BackendFqdns "dummybackend.com"
```

<span data-ttu-id="e9458-222">Vytvořit pravidlo nastavení, které aplikační brány použije ve výchozím nastavení, která odkazuje na neexistující back-end **dummybackend.com** ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="e9458-222">Create a rule setting that the Application Gateway will use by default which points to the non-existent backend **dummybackend.com** in the Virtual Network.</span></span>

```powershell
$dummyPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "nonexistentapis" -Paths "/*" -BackendAddressPool $dummyBackendPool -BackendHttpSettings $dummyBackendSetting
```

### <a name="step-11"></a><span data-ttu-id="e9458-223">Krok 11</span><span class="sxs-lookup"><span data-stu-id="e9458-223">Step 11</span></span>

<span data-ttu-id="e9458-224">Konfigurovat pravidla cesty adresy URL pro back endové fondy.</span><span class="sxs-lookup"><span data-stu-id="e9458-224">Configure URL rule paths for the back-end pools.</span></span> <span data-ttu-id="e9458-225">To umožňuje výběr jenom některé z rozhraní API ze správy rozhraní API pro vystavení veřejnosti.</span><span class="sxs-lookup"><span data-stu-id="e9458-225">This enables selecting only some of the APIs from API Management for being exposed to the public.</span></span> <span data-ttu-id="e9458-226">Například, pokud existují `Echo API` (/ echo /), `Calculator API` (/calc/) atd. Zkontrolujte pouze `Echo API` přístupné z Internetu).</span><span class="sxs-lookup"><span data-stu-id="e9458-226">For example, if there are `Echo API` (/echo/), `Calculator API` (/calc/) etc. make only `Echo API` accessible from Internet).</span></span> 

<span data-ttu-id="e9458-227">Následující příklad vytvoří jednoduché pravidlo pro "/ echo /" cesty směrování provozu, který back-end "apimProxyBackendPool".</span><span class="sxs-lookup"><span data-stu-id="e9458-227">The following example creates a simple rule for the "/echo/" path routing traffic to the back-end "apimProxyBackendPool".</span></span>

```powershell
$echoapiRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "externalapis" -Paths "/echo/*" -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
```

<span data-ttu-id="e9458-228">Pokud cesta neodpovídá pravidla cesty chceme, aby ze správy rozhraní API, konfigurace pravidla cesty mapy nakonfiguruje taky výchozího fondu adres back-end s názvem **dummyBackendPool**.</span><span class="sxs-lookup"><span data-stu-id="e9458-228">If the path doesn't match the path rules we want to enable from API Management, the rule path map configuration also configures a default back-end address pool named **dummyBackendPool**.</span></span> <span data-ttu-id="e9458-229">Například http://api.contoso.net/calc/ * přejde k **dummyBackendPool** je definovaný jako výchozí fond pro zrušení odpovídající provoz.</span><span class="sxs-lookup"><span data-stu-id="e9458-229">For example, http://api.contoso.net/calc/* goes to **dummyBackendPool** as it is defined as the default pool for un-matched traffic.</span></span>

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $echoapiRule, $dummyPathRule -DefaultBackendAddressPool $dummyBackendPool -DefaultBackendHttpSettings $dummyBackendSetting
```

<span data-ttu-id="e9458-230">Předchozí krok zajistí, který pouze požadavky pro cestu "/ odezvu" jsou povoleny prostřednictvím služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="e9458-230">The above step ensures that only requests for the path "/echo" are allowed through the Application Gateway.</span></span> <span data-ttu-id="e9458-231">Požadavky pro jiná rozhraní API nakonfigurovaný ve službě API Management vyvolá výjimku chyby 404 ze Application Gateway při přístupu z Internetu.</span><span class="sxs-lookup"><span data-stu-id="e9458-231">Requests to other APIs configured in API Management will throw 404 errors from Application Gateway when accessed from the Internet.</span></span> 

### <a name="step-12"></a><span data-ttu-id="e9458-232">Krok 12</span><span class="sxs-lookup"><span data-stu-id="e9458-232">Step 12</span></span>

<span data-ttu-id="e9458-233">Vytvoření pravidla nastavení pro službu Application Gateway používat na základě cestu směrování adres URL.</span><span class="sxs-lookup"><span data-stu-id="e9458-233">Create a rule setting for the Application Gateway to use URL path-based routing.</span></span>

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-13"></a><span data-ttu-id="e9458-234">Krok 13</span><span class="sxs-lookup"><span data-stu-id="e9458-234">Step 13</span></span>

<span data-ttu-id="e9458-235">Nakonfigurujte počet instancí a velikost pro službu Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="e9458-235">Configure the number of instances and size for the Application Gateway.</span></span> <span data-ttu-id="e9458-236">Tady se používá [firewall webových aplikací SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) pro zvýšení zabezpečení prostředků API Management.</span><span class="sxs-lookup"><span data-stu-id="e9458-236">Here we are using the [WAF SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) for increased security of the API Management resource.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-14"></a><span data-ttu-id="e9458-237">Krok 14</span><span class="sxs-lookup"><span data-stu-id="e9458-237">Step 14</span></span>

<span data-ttu-id="e9458-238">Nakonfigurujte firewall webových aplikací v režimu "Prevence".</span><span class="sxs-lookup"><span data-stu-id="e9458-238">Configure WAF to be in "Prevention" mode.</span></span>
```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a><span data-ttu-id="e9458-239">Vytvořte aplikační bránu</span><span class="sxs-lookup"><span data-stu-id="e9458-239">Create Application Gateway</span></span>

<span data-ttu-id="e9458-240">Vytvoření služby Application Gateway se všemi objekty konfigurace z předchozích kroků.</span><span class="sxs-lookup"><span data-stu-id="e9458-240">Create an Application Gateway with all the configuration objects from the preceding steps.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name $applicationGatewayName -ResourceGroupName $resourceGroupName  -Location $location -BackendAddressPools $apimProxyBackendPool, $dummyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $dummyBackendSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert -Probes $apimprobe
```

## <a name="cname-the-api-management-proxy-hostname-to-the-public-dns-name-of-the-application-gateway-resource"></a><span data-ttu-id="e9458-241">Název hostitele proxy API Management prostředku aplikační brány jako veřejný název DNS CNAME</span><span class="sxs-lookup"><span data-stu-id="e9458-241">CNAME the API Management proxy hostname to the public DNS name of the Application Gateway resource</span></span>

<span data-ttu-id="e9458-242">Po vytvoření brány je dalším krokem konfigurace front-endu pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="e9458-242">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="e9458-243">Pokud používáte veřejnou IP adresu, aplikační brána vyžaduje dynamicky přiřazené název DNS, který nemusí být nejjednodušší.</span><span class="sxs-lookup"><span data-stu-id="e9458-243">When using a public IP, Application Gateway requires a dynamically assigned DNS name, which may not be easy to use.</span></span> 

<span data-ttu-id="e9458-244">Název DNS služby Application Gateway by měl použít k vytvoření záznam CNAME, který ukazuje název APIM proxy hostitele (například `api.contoso.net` ve výše uvedených příkladech) k tomuto názvu DNS.</span><span class="sxs-lookup"><span data-stu-id="e9458-244">The Application Gateway's DNS name should be used to create a CNAME record which points the APIM proxy host name (e.g. `api.contoso.net` in the examples above) to this DNS name.</span></span> <span data-ttu-id="e9458-245">Pokud chcete nakonfigurovat záznam IP CNAME front-endu, načtení podrobností o aplikační bránu a svému přidruženému názvu IP a DNS pomocí elementu PublicIPAddress.</span><span class="sxs-lookup"><span data-stu-id="e9458-245">To configure the frontend IP CNAME record, retrieve the details of the Application Gateway and its associated IP/DNS name using the PublicIPAddress element.</span></span> <span data-ttu-id="e9458-246">Použití záznamů A se nedoporučuje, protože VIP může změnit při restartu brány.</span><span class="sxs-lookup"><span data-stu-id="e9458-246">The use of A-records is not recommended since the VIP may change on restart of gateway.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -Name "publicIP01"
```

##<span data-ttu-id="e9458-247"><a name="summary"></a> Souhrn</span><span class="sxs-lookup"><span data-stu-id="e9458-247"><a name="summary"> </a> Summary</span></span>
<span data-ttu-id="e9458-248">Nakonfigurovat ve virtuální síti Azure API Management poskytuje rozhraní jedna brána pro všechny nakonfigurované rozhraní API, ať už jsou hostované v místní nebo v cloudu.</span><span class="sxs-lookup"><span data-stu-id="e9458-248">Azure API Management configured in a VNET provides a single gateway interface for all configured APIs, whether they are hosted on-prem or in the cloud.</span></span> <span data-ttu-id="e9458-249">Integrace Application Gateway s API Management nabízí flexibilitu selektivně povolení konkrétní rozhraní API bude přístupný na Internetu, jakož i poskytnutí brány Firewall webových aplikací jako front-end pro vaše instance služby API Management.</span><span class="sxs-lookup"><span data-stu-id="e9458-249">Integrating Application Gateway with API Management provides the flexibility of selectively enabling particular APIs to be accessible on the Internet, as well as providing a Web Application Firewall as a frontend to your API Management instance.</span></span>

##<span data-ttu-id="e9458-250"><a name="next-steps"></a> Další kroky</span><span class="sxs-lookup"><span data-stu-id="e9458-250"><a name="next-steps"> </a> Next steps</span></span>
* <span data-ttu-id="e9458-251">Další informace o Azure Application Gateway</span><span class="sxs-lookup"><span data-stu-id="e9458-251">Learn more about Azure Application Gateway</span></span>
  * [<span data-ttu-id="e9458-252">Přehled brány aplikace</span><span class="sxs-lookup"><span data-stu-id="e9458-252">Application Gateway Overview</span></span>](../application-gateway/application-gateway-introduction.md)
  * [<span data-ttu-id="e9458-253">Brány Firewall webových aplikací Application Gateway</span><span class="sxs-lookup"><span data-stu-id="e9458-253">Application Gateway Web Application Firewall</span></span>](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [<span data-ttu-id="e9458-254">Aplikační bránu pomocí směrování na základě cesty</span><span class="sxs-lookup"><span data-stu-id="e9458-254">Application Gateway using Path-based Routing</span></span>](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* <span data-ttu-id="e9458-255">Další informace o rozhraní API správy a virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="e9458-255">Learn more about API Management and VNETs</span></span>
  * [<span data-ttu-id="e9458-256">Použití služby API Management, které jsou k dispozici pouze v rámci virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="e9458-256">Using API Management available only within the VNET</span></span>](api-management-using-with-internal-vnet.md)
  * [<span data-ttu-id="e9458-257">Použití služby API Management ve virtuální síti</span><span class="sxs-lookup"><span data-stu-id="e9458-257">Using API Management in VNET</span></span>](api-management-using-with-vnet.md)