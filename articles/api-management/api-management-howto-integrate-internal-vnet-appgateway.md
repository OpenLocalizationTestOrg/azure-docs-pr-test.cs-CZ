---
title: "aaaHow toouse Azure API Management ve virtuální síti s aplikační brány | Microsoft Docs"
description: "Zjistěte, jak toosetup a nakonfigurovat Azure API Management v interní virtuální síť s aplikací brány (firewall webových aplikací) jako front-endu"
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
ms.openlocfilehash: 74303a2ee8a10db633ab1740ec7267728eacb473
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a><span data-ttu-id="c9a01-103">Integrovat správu rozhraní API v interní virtuální síť s aplikační brány</span><span class="sxs-lookup"><span data-stu-id="c9a01-103">Integrate API Management in an internal VNET with Application Gateway</span></span> 

##<span data-ttu-id="c9a01-104"><a name="overview"></a> – Přehled</span><span class="sxs-lookup"><span data-stu-id="c9a01-104"><a name="overview"> </a> Overview</span></span>
 
<span data-ttu-id="c9a01-105">Hello služba API Management můžete nakonfigurovat ve virtuální síti v interní režimu, takže je dostupné pouze v aplikaci hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="c9a01-105">hello API Management service can be configured in a Virtual Network in internal mode which makes it accessible only from within hello Virtual Network.</span></span> <span data-ttu-id="c9a01-106">Služba Azure Application Gateway je služba PAAS, které poskytuje nástroj pro vyrovnávání zatížení vrstvy 7.</span><span class="sxs-lookup"><span data-stu-id="c9a01-106">Azure Application Gateway is a PAAS Service which provides a Layer-7 load balancer.</span></span> <span data-ttu-id="c9a01-107">To funguje jako služba reverznímu proxy serveru a poskytuje mezi jeho nabídky webové aplikace brány Firewall (firewall webových aplikací).</span><span class="sxs-lookup"><span data-stu-id="c9a01-107">It acts as a reverse-proxy service and provides among its offering a Web Application Firewall (WAF).</span></span>

<span data-ttu-id="c9a01-108">Kombinování API Management zřízené v interní virtuální síti VNET s front-endu Application Gateway hello umožňuje hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="c9a01-108">Combining API Management provisioned in an internal VNET with hello Application Gateway frontend enables hello following scenarios:</span></span>

* <span data-ttu-id="c9a01-109">Použití hello stejné rozhraní API správy prostředků pro spotřeba příjemci interní i externí uživatelé.</span><span class="sxs-lookup"><span data-stu-id="c9a01-109">Use hello same API Management resource for consumption by both internal consumers and external consumers.</span></span>
* <span data-ttu-id="c9a01-110">Použít jeden zdroj API Management a mít podmnožinu rozhraní API definované ve službě API Management, které jsou k dispozici pro externí příjemci.</span><span class="sxs-lookup"><span data-stu-id="c9a01-110">Use a single API Management resource and have a subset of APIs defined in API Management available for external consumers.</span></span>
* <span data-ttu-id="c9a01-111">Zadejte způsob klíč tooswitch přístup tooAPI správy z hello veřejného Internetu zapnout a vypnout.</span><span class="sxs-lookup"><span data-stu-id="c9a01-111">Provide a turn-key way tooswitch access tooAPI Management from hello public Internet on and off.</span></span> 

##<span data-ttu-id="c9a01-112"><a name="scenario"></a> Scénář</span><span class="sxs-lookup"><span data-stu-id="c9a01-112"><a name="scenario"> </a> Scenario</span></span>
<span data-ttu-id="c9a01-113">Tento článek se týkají jak toouse jeden rozhraní API správy služby pro interní i externí příjemce a nastavte jej fungovat jako jeden front-end pro obě místní a cloudové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c9a01-113">This article will cover how toouse a single API Management service for both internal and external consumers and make it act as a single frontend for both on-prem and cloud APIs.</span></span> <span data-ttu-id="c9a01-114">Zobrazí se také jak tooexpose pouze podmnožina vašich rozhraní API (v příkladu hello jsou vyznačené na zelená) pro externí spotřebu pomocí funkce PathBasedRouting hello je k dispozici v aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="c9a01-114">You will also see how tooexpose only a subset of your APIs (in hello example they are highlighted in green) for External Consumption using hello PathBasedRouting functionality available in Application Gateway.</span></span>

<span data-ttu-id="c9a01-115">V prvním příkladu instalace hello všechna rozhraní API spravují pouze v rámci virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="c9a01-115">In hello first setup example all your APIs are managed only from within your Virtual Network.</span></span> <span data-ttu-id="c9a01-116">Interní příjemci (zvýraznit v oranžová) mají přístup všechny vaše interní a externí rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c9a01-116">Internal consumers (highlighted in orange) can access all your internal and external APIs.</span></span> <span data-ttu-id="c9a01-117">Provoz se nikdy nedostane mimo tooInternet, vysoký výkon je doručovány prostřednictvím okruhy Expressroute.</span><span class="sxs-lookup"><span data-stu-id="c9a01-117">Traffic never goes out tooInternet a high performance is delivered via Express Route circuits.</span></span>

![Adresa URL trasy](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <span data-ttu-id="c9a01-119"><a name="before-you-begin"></a> Před zahájením</span><span class="sxs-lookup"><span data-stu-id="c9a01-119"><a name="before-you-begin"> </a> Before you begin</span></span>

1. <span data-ttu-id="c9a01-120">Nainstalujte nejnovější verzi rutin prostředí Azure PowerShell hello hello pomocí hello instalačního programu webové platformy.</span><span class="sxs-lookup"><span data-stu-id="c9a01-120">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="c9a01-121">Můžete stáhnout a nainstalovat nejnovější verzi hello z hello **prostředí Windows PowerShell** části hello [položky ke stažení](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c9a01-121">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="c9a01-122">Vytvoření virtuální sítě a vytvořte oddělené podsítě pro API Management a aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="c9a01-122">Create a Virtual Network and create separate subnets for API Management and Application Gateway.</span></span> 
3. <span data-ttu-id="c9a01-123">Pokud máte v úmyslu toocreate vlastního serveru DNS pro hello virtuální sítě, můžete tak učiňte před zahájením nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="c9a01-123">If you intend toocreate a custom DNS server for hello Virtual Network, do so before starting hello deployment.</span></span> <span data-ttu-id="c9a01-124">Překontrolujte, který funguje zajištěním virtuální počítač vytvořený v novou podsíť ve hello virtuální sítě můžete vyřešit a přístup všechny koncové body služby Azure.</span><span class="sxs-lookup"><span data-stu-id="c9a01-124">Double check it works by ensuring a virtual machine created in a new subnet in hello Virtual Network can resolve and access all Azure service endpoints.</span></span>

## <a name="what-is-required-toocreate-an-integration-between-api-management-and-application-gateway"></a><span data-ttu-id="c9a01-125">Co je požadovaná toocreate integraci mezi API Management a Application Gateway?</span><span class="sxs-lookup"><span data-stu-id="c9a01-125">What is required toocreate an integration between API Management and Application Gateway?</span></span>

* <span data-ttu-id="c9a01-126">**Fond back-end serverů:** hello interní virtuální IP adresa hello služba API Management.</span><span class="sxs-lookup"><span data-stu-id="c9a01-126">**Back-end server pool:** This is hello internal virtual IP address of hello API Management service.</span></span>
* <span data-ttu-id="c9a01-127">**Nastavení fondu back-end serverů:** Každý fond má nastavení, jako je port, protokol a spřažení na základě souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="c9a01-127">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="c9a01-128">Tato nastavení jsou použité tooall servery v rámci fondu hello.</span><span class="sxs-lookup"><span data-stu-id="c9a01-128">These settings are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="c9a01-129">**Front-end port:** jde hello veřejný port, který se otevírá ve hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="c9a01-129">**Front-end port:** This is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="c9a01-130">Provoz stiskne ho získá přesměrovaného tooone hello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="c9a01-130">Traffic hitting it gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="c9a01-131">**Naslouchací proces:** hello naslouchací proces má front-end port, protokol (Http nebo Https, tyto hodnoty jsou malá a velká písmena) a název certifikátu SSL hello (Pokud se konfiguruje přesměrování zpracování SSL).</span><span class="sxs-lookup"><span data-stu-id="c9a01-131">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="c9a01-132">**Pravidlo:** hello pravidlo váže naslouchací proces fondu tooa back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="c9a01-132">**Rule:** hello rule binds a listener tooa back-end server pool.</span></span>
* <span data-ttu-id="c9a01-133">**Vlastní test stavu:** aplikační bránu, ve výchozím nastavení, použije IP adresy na základě sondy toofigure, na které servery v hello BackendAddressPool jsou aktivní.</span><span class="sxs-lookup"><span data-stu-id="c9a01-133">**Custom Health Probe:** Application Gateway, by default, uses IP address based probes toofigure out which servers in hello BackendAddressPool are active.</span></span> <span data-ttu-id="c9a01-134">Hello služba API Management pouze odpoví toorequests mít hlavička hello správné hostitele, proto hello výchozí sondy nezdaří.</span><span class="sxs-lookup"><span data-stu-id="c9a01-134">hello API Management service only responds toorequests which have hello correct host header, hence hello default probes fail.</span></span> <span data-ttu-id="c9a01-135">Test vlastní stavu musí toobe definované toohelp Aplikační brána určit, že služba hello je aktivní a předávat požadavky.</span><span class="sxs-lookup"><span data-stu-id="c9a01-135">A custom health probe needs toobe defined toohelp application gateway determine that hello service is alive and it should forward requests.</span></span>
* <span data-ttu-id="c9a01-136">**Certifikát vlastní domény:** tooaccess API Management z hello internet, je nutné toocreate mapování CNAME hostname toohello Application Gateway front-end DNS názvu.</span><span class="sxs-lookup"><span data-stu-id="c9a01-136">**Custom domain certificate:** tooaccess API Management from hello internet you need toocreate a CNAME mapping of its hostname toohello Application Gateway front-end DNS name.</span></span> <span data-ttu-id="c9a01-137">To zajišťuje, že hlavičku hello název hostitele a certifikát odeslat tooApplication bránu, která se předají tooAPI správu jednoho APIM rozpoznat jako platný.</span><span class="sxs-lookup"><span data-stu-id="c9a01-137">This ensures that hello hostname header and certificate sent tooApplication Gateway that is forwarded tooAPI Management is one APIM can recognize as valid.</span></span>

## <span data-ttu-id="c9a01-138"><a name="overview-steps"></a> Kroky potřebné k integraci API Management a aplikační brány</span><span class="sxs-lookup"><span data-stu-id="c9a01-138"><a name="overview-steps"> </a> Steps required for integrating API Management and Application Gateway</span></span> 

1. <span data-ttu-id="c9a01-139">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c9a01-139">Create a resource group for Resource Manager.</span></span>
2. <span data-ttu-id="c9a01-140">Vytvořte virtuální síť, podsíť a veřejnou IP adresu pro hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c9a01-140">Create a Virtual Network, subnet, and public IP for hello Application Gateway.</span></span> <span data-ttu-id="c9a01-141">Vytvořte další podsítě pro API Management.</span><span class="sxs-lookup"><span data-stu-id="c9a01-141">Create another subnet for API Management.</span></span>
3. <span data-ttu-id="c9a01-142">Vytvoření služby API Management uvnitř podsíť virtuální sítě hello vytvořili výše a ujistěte se, že používáte interní režim hello.</span><span class="sxs-lookup"><span data-stu-id="c9a01-142">Create an API Management service inside hello VNET subnet created above and ensure you use hello Internal mode.</span></span>
4. <span data-ttu-id="c9a01-143">Instalační program hello vlastní název domény v hello služba API Management.</span><span class="sxs-lookup"><span data-stu-id="c9a01-143">Setup hello custom domain name in hello API Management service.</span></span>
5. <span data-ttu-id="c9a01-144">Vytvoření objektu konfigurace služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c9a01-144">Create an Application Gateway configuration object.</span></span>
6. <span data-ttu-id="c9a01-145">Vytvořte prostředek aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="c9a01-145">Create an Application Gateway resource.</span></span>
7. <span data-ttu-id="c9a01-146">Vytvořte záznam CNAME z hello veřejného názvu DNS hello Application Gateway toohello API Management proxy hostitele.</span><span class="sxs-lookup"><span data-stu-id="c9a01-146">Create a CNAME from hello public DNS name of hello Application Gateway toohello API Management proxy hostname.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="c9a01-147">Vytvoření skupiny prostředků pro Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c9a01-147">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="c9a01-148">Ujistěte se, že používáte nejnovější verzi prostředí Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="c9a01-148">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="c9a01-149">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="c9a01-149">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="c9a01-150">Krok 1</span><span class="sxs-lookup"><span data-stu-id="c9a01-150">Step 1</span></span>

<span data-ttu-id="c9a01-151">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="c9a01-151">Log in tooAzure</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="c9a01-152">Ověření pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="c9a01-152">Authenticate with your credentials.</span></span><BR>

### <a name="step-2"></a><span data-ttu-id="c9a01-153">Krok 2</span><span class="sxs-lookup"><span data-stu-id="c9a01-153">Step 2</span></span>

<span data-ttu-id="c9a01-154">Zkontrolujte předplatná hello pro účet hello a vyberte jej.</span><span class="sxs-lookup"><span data-stu-id="c9a01-154">Check hello subscriptions for hello account and select it.</span></span>

```powershell
Get-AzureRmSubscription -Subscriptionid "GUID of subscription" | Select-AzureRmSubscription
```

### <a name="step-3"></a><span data-ttu-id="c9a01-155">Krok 3</span><span class="sxs-lookup"><span data-stu-id="c9a01-155">Step 3</span></span>

<span data-ttu-id="c9a01-156">Vytvořte skupinu prostředků (pokud používáte některou ze stávajících skupin prostředků, můžete tenhle krok přeskočit).</span><span class="sxs-lookup"><span data-stu-id="c9a01-156">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name "apim-appGw-RG" -Location "West US"
```
<span data-ttu-id="c9a01-157">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="c9a01-157">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="c9a01-158">To slouží jako hello výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="c9a01-158">This is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="c9a01-159">Ujistěte se, že všechny příkazy toocreate hello aplikaci brány použijte stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c9a01-159">Make sure that all commands toocreate an application gateway use hello same resource group.</span></span>

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a><span data-ttu-id="c9a01-160">Vytvoření virtuální sítě a podsítě pro službu hello application gateway</span><span class="sxs-lookup"><span data-stu-id="c9a01-160">Create a Virtual Network and a subnet for hello application gateway</span></span>

<span data-ttu-id="c9a01-161">Hello následující příklad ukazuje, jak hello toocreate virtuální sítě pomocí Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="c9a01-161">hello following example shows how toocreate a Virtual Network using hello resource manager.</span></span>

### <a name="step-1"></a><span data-ttu-id="c9a01-162">Krok 1</span><span class="sxs-lookup"><span data-stu-id="c9a01-162">Step 1</span></span>

<span data-ttu-id="c9a01-163">Přiřaďte hello adresa rozsahu 10.0.0.0/24 toohello podsíť proměnné toobe použit pro službu Application Gateway při vytváření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="c9a01-163">Assign hello address range 10.0.0.0/24 toohello subnet variable toobe used for Application Gateway while creating a Virtual Network.</span></span>

```powershell
$appgatewaysubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a><span data-ttu-id="c9a01-164">Krok 2</span><span class="sxs-lookup"><span data-stu-id="c9a01-164">Step 2</span></span>

<span data-ttu-id="c9a01-165">Přiřaďte hello adresa rozsahu 10.0.1.0/24 toohello podsíť proměnné toobe používá pro správu rozhraní API při vytváření virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="c9a01-165">Assign hello address range 10.0.1.0/24 toohello subnet variable toobe used for API Management while creating a Virtual Network.</span></span>

```powershell
$apimsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a><span data-ttu-id="c9a01-166">Krok 3</span><span class="sxs-lookup"><span data-stu-id="c9a01-166">Step 3</span></span>

<span data-ttu-id="c9a01-167">Vytvořit virtuální síť s názvem **appgwvnet** ve skupině prostředků **apim-appGw-RG** pro oblast západní USA hello pomocí hello předpony 10.0.0.0/16 s podsítí 10.0.0.0/24 a 10.0.1.0/24.</span><span class="sxs-lookup"><span data-stu-id="c9a01-167">Create a Virtual Network named **appgwvnet** in resource group **apim-appGw-RG** for hello West US region using hello prefix 10.0.0.0/16 with subnets 10.0.0.0/24 and 10.0.1.0/24.</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name "appgwvnet" -ResourceGroupName "apim-appGw-RG" -Location "West US" -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a><span data-ttu-id="c9a01-168">Krok 4</span><span class="sxs-lookup"><span data-stu-id="c9a01-168">Step 4</span></span>

<span data-ttu-id="c9a01-169">Přiřaďte proměnnou podsítě pro další kroky hello</span><span class="sxs-lookup"><span data-stu-id="c9a01-169">Assign a subnet variable for hello next steps</span></span>

```powershell
$appgatewaysubnetdata=$vnet.Subnets[0]
$apimsubnetdata=$vnet.Subnets[1]
```
## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a><span data-ttu-id="c9a01-170">Vytvoření služby API Management uvnitř nakonfigurován v režimu interní virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="c9a01-170">Create an API Management service inside a VNET configured in internal mode</span></span>

<span data-ttu-id="c9a01-171">Hello následující příklad ukazuje, jak toocreate služby API Management ve virtuální síti nakonfigurované interní pouze pro přístup.</span><span class="sxs-lookup"><span data-stu-id="c9a01-171">hello following example shows how toocreate an API Management service in a VNET configured for internal access only.</span></span>

### <a name="step-1"></a><span data-ttu-id="c9a01-172">Krok 1</span><span class="sxs-lookup"><span data-stu-id="c9a01-172">Step 1</span></span>
<span data-ttu-id="c9a01-173">Vytvořte virtuální síť pro správu rozhraní API objekt, který používá podsíť hello $apimsubnetdata vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="c9a01-173">Create an API Management Virtual Network object using hello subnet $apimsubnetdata created above.</span></span>

```powershell
$apimVirtualNetwork = New-AzureRmApiManagementVirtualNetwork -Location "West US" -SubnetResourceId $apimsubnetdata.Id
```
### <a name="step-2"></a><span data-ttu-id="c9a01-174">Krok 2</span><span class="sxs-lookup"><span data-stu-id="c9a01-174">Step 2</span></span>
<span data-ttu-id="c9a01-175">Vytvoření služby API Management uvnitř hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="c9a01-175">Create an API Management service inside hello Virtual Network.</span></span>

```powershell
$apimService = New-AzureRmApiManagement -ResourceGroupName "apim-appGw-RG" -Location "West US" -Name "ContosoApi" -Organization "Contoso" -AdminEmail "admin@contoso.com" -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```
<span data-ttu-id="c9a01-176">Po úspěšném provedení hello výše příkaz odkazovat příliš[tooaccess interní virtuální síť rozhraní API správy služby nutná konfigurace DNS](api-management-using-with-internal-vnet.md#apim-dns-configuration) tooaccess ho.</span><span class="sxs-lookup"><span data-stu-id="c9a01-176">After hello above command succeeds refer too[DNS Configuration required tooaccess internal VNET API Management service](api-management-using-with-internal-vnet.md#apim-dns-configuration) tooaccess it.</span></span>

## <a name="set-up-a-custom-domain-name-in-api-management"></a><span data-ttu-id="c9a01-177">Nastavit vlastní název domény ve službě API Management</span><span class="sxs-lookup"><span data-stu-id="c9a01-177">Set-up a custom domain name in API Management</span></span>

### <a name="step-1"></a><span data-ttu-id="c9a01-178">Krok 1</span><span class="sxs-lookup"><span data-stu-id="c9a01-178">Step 1</span></span>
<span data-ttu-id="c9a01-179">Nahrajte hello certifikát s privátním klíčem pro doménu hello.</span><span class="sxs-lookup"><span data-stu-id="c9a01-179">Upload hello certificate with private key for hello domain.</span></span> <span data-ttu-id="c9a01-180">V tomto příkladu bude `*.contoso.net`.</span><span class="sxs-lookup"><span data-stu-id="c9a01-180">For this example it will be `*.contoso.net`.</span></span> 

```powershell
$certUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName "apim-appGw-RG" -Name "ContosoApi" -HostnameType "Proxy" -PfxPath <full path too.pfx file> -PfxPassword <password for certificate file> -PassThru
```

### <a name="step-2"></a><span data-ttu-id="c9a01-181">Krok 2</span><span class="sxs-lookup"><span data-stu-id="c9a01-181">Step 2</span></span>
<span data-ttu-id="c9a01-182">Po nahrání certifikátu hello vytvořit objekt konfigurace název hostitele pro proxy server hello s název hostitele `api.contoso.net`, jak hello příklad certifikát poskytuje autority pro hello `*.contoso.net` domény.</span><span class="sxs-lookup"><span data-stu-id="c9a01-182">Once hello certificate is uploaded, create a hostname configuration object for hello proxy with a hostname of `api.contoso.net`, as hello example certificate provides authority for hello  `*.contoso.net` domain.</span></span> 

```powershell
$proxyHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certUploadResult.Thumbprint -Hostname "api.contoso.net"
$result = Set-AzureRmApiManagementHostnames -Name "ContosoApi" -ResourceGroupName "apim-appGw-RG" -ProxyHostnameConfiguration $proxyHostnameConfig
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="c9a01-183">Vytvoření veřejné IP adresy pro front-end konfiguraci hello</span><span class="sxs-lookup"><span data-stu-id="c9a01-183">Create a public IP address for hello front-end configuration</span></span>

<span data-ttu-id="c9a01-184">Vytvořte prostředek veřejné IP **adresy publicIP01** ve skupině prostředků **apim-appGw-RG** pro oblast západní USA hello.</span><span class="sxs-lookup"><span data-stu-id="c9a01-184">Create a public IP resource **publicIP01** in resource group **apim-appGw-RG** for hello West US region.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -name "publicIP01" -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="c9a01-185">IP adresa je přiřazen toohello aplikační bránu, při spuštění služby hello.</span><span class="sxs-lookup"><span data-stu-id="c9a01-185">An IP address is assigned toohello application gateway when hello service starts.</span></span>

## <a name="create-application-gateway-configuration"></a><span data-ttu-id="c9a01-186">Vytvoření konfigurace brány aplikace</span><span class="sxs-lookup"><span data-stu-id="c9a01-186">Create application gateway configuration</span></span>

<span data-ttu-id="c9a01-187">Všechny položky konfigurace, musí ho nastavit před vytvořením hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="c9a01-187">All configuration items must be set up before creating hello application gateway.</span></span> <span data-ttu-id="c9a01-188">Hello následujících kroků vytvořte hello položky konfigurace, které jsou potřebné pro prostředek aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="c9a01-188">hello following steps create hello configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="c9a01-189">Krok 1</span><span class="sxs-lookup"><span data-stu-id="c9a01-189">Step 1</span></span>

<span data-ttu-id="c9a01-190">Vytvořte konfiguraci IP adresy služby Application Gateway s názvem **gatewayIP01**.</span><span class="sxs-lookup"><span data-stu-id="c9a01-190">Create an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="c9a01-191">Při spuštění služby Application Gateway, vybere IP adresa z nakonfigurované podsítě hello a směrovat síťový provoz toohello IP adresy ve fondu hello back-end IP adres.</span><span class="sxs-lookup"><span data-stu-id="c9a01-191">When Application Gateway starts, it picks up an IP address from hello subnet configured and route network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="c9a01-192">Uvědomte si, že každá instance vyžaduje jednu IP adresu.</span><span class="sxs-lookup"><span data-stu-id="c9a01-192">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a><span data-ttu-id="c9a01-193">Krok 2</span><span class="sxs-lookup"><span data-stu-id="c9a01-193">Step 2</span></span>

<span data-ttu-id="c9a01-194">Nakonfigurujte port front-end IP hello pro hello koncový bod veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="c9a01-194">Configure hello front-end IP port for hello public IP endpoint.</span></span> <span data-ttu-id="c9a01-195">Toto je hello port, který koncoví uživatelé připojit k.</span><span class="sxs-lookup"><span data-stu-id="c9a01-195">This port is hello port that end users connect to.</span></span>

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "port01"  -Port 443
```
### <a name="step-3"></a><span data-ttu-id="c9a01-196">Krok 3</span><span class="sxs-lookup"><span data-stu-id="c9a01-196">Step 3</span></span>

<span data-ttu-id="c9a01-197">Nakonfigurujte veřejný koncový bod IP hello front-end IP adresu.</span><span class="sxs-lookup"><span data-stu-id="c9a01-197">Configure hello front-end IP with public IP endpoint.</span></span>

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a><span data-ttu-id="c9a01-198">Krok 4</span><span class="sxs-lookup"><span data-stu-id="c9a01-198">Step 4</span></span>

<span data-ttu-id="c9a01-199">Nakonfigurujte hello certifikát pro hello Application Gateway používají toodecrypt a znovu zašifrovat přenosy hello procházející.</span><span class="sxs-lookup"><span data-stu-id="c9a01-199">Configure hello certificate for hello Application Gateway, used toodecrypt and re-encrypt hello traffic passing through.</span></span>

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name "cert01" -CertificateFile <full path too.pfx file> -Password <password for certificate file>
```

### <a name="step-5"></a><span data-ttu-id="c9a01-200">Krok 5</span><span class="sxs-lookup"><span data-stu-id="c9a01-200">Step 5</span></span>

<span data-ttu-id="c9a01-201">Vytvořte naslouchací proces protokolu HTTP hello pro hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c9a01-201">Create hello HTTP listener for hello Application Gateway.</span></span> <span data-ttu-id="c9a01-202">Přiřaďte hello front-endové konfigurace protokolu IP, portu a tooit certifikát ssl.</span><span class="sxs-lookup"><span data-stu-id="c9a01-202">Assign hello front-end IP configuration, port, and ssl certificate tooit.</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert
```

### <a name="step-6"></a><span data-ttu-id="c9a01-203">Krok 6</span><span class="sxs-lookup"><span data-stu-id="c9a01-203">Step 6</span></span>

<span data-ttu-id="c9a01-204">Vytvořit vlastní test paměti toohello služba API Management `ContosoApi` koncový bod domény proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="c9a01-204">Create a custom probe toohello API Management service `ContosoApi` proxy domain endpoint.</span></span> <span data-ttu-id="c9a01-205">Cesta Hello `/status-0123456789abcdef` je výchozího koncového bodu stavu hostované na všechny služby API Management hello.</span><span class="sxs-lookup"><span data-stu-id="c9a01-205">hello path `/status-0123456789abcdef` is a default health endpoint hosted on all hello API Management services.</span></span> <span data-ttu-id="c9a01-206">Nastavit `api.contoso.net` jako toosecure název hostitele vlastní test paměti ho s certifikátem SSL.</span><span class="sxs-lookup"><span data-stu-id="c9a01-206">Set `api.contoso.net` as a custom probe hostname toosecure it with SSL certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="c9a01-207">název hostitele Hello `contosoapi.azure-api.net` hello výchozí proxy hostname-li je konfigurována s názvem služby `contosoapi` je vytvořen v veřejný Azure.</span><span class="sxs-lookup"><span data-stu-id="c9a01-207">hello hostname `contosoapi.azure-api.net` is hello default proxy hostname configured when a service named `contosoapi` is created in public Azure.</span></span> 
> 

```powershell
$apimprobe = New-AzureRmApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName "api.contoso.net" -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
```

### <a name="step-7"></a><span data-ttu-id="c9a01-208">Krok 7</span><span class="sxs-lookup"><span data-stu-id="c9a01-208">Step 7</span></span>

<span data-ttu-id="c9a01-209">Nahrajte certifikát hello toobe použít na hello povolen protokol SSL back-end fondu zdrojů.</span><span class="sxs-lookup"><span data-stu-id="c9a01-209">Upload hello certificate toobe used on hello SSL-enabled backend pool resources.</span></span> <span data-ttu-id="c9a01-210">Toto je hello stejný certifikát, který jste zadali v kroku 4 výše.</span><span class="sxs-lookup"><span data-stu-id="c9a01-210">This is hello same certificate which you provided in Step 4 above.</span></span>

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile <full path too.cer file>
```

### <a name="step-8"></a><span data-ttu-id="c9a01-211">Krok 8</span><span class="sxs-lookup"><span data-stu-id="c9a01-211">Step 8</span></span>

<span data-ttu-id="c9a01-212">Nakonfigurujte nastavení HTTP back-end pro hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c9a01-212">Configure HTTP backend settings for hello Application Gateway.</span></span> <span data-ttu-id="c9a01-213">To zahrnuje nastavení časového limitu pro požadavek back-end, po jejímž uplynutí se zrušil.</span><span class="sxs-lookup"><span data-stu-id="c9a01-213">This includes setting a time-out limit for backend request after which they are cancelled.</span></span> <span data-ttu-id="c9a01-214">Tato hodnota se liší od časový limit testu hello.</span><span class="sxs-lookup"><span data-stu-id="c9a01-214">This value is different from hello probe time-out.</span></span>

```powershell
$apimPoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a><span data-ttu-id="c9a01-215">Krok 9</span><span class="sxs-lookup"><span data-stu-id="c9a01-215">Step 9</span></span>

<span data-ttu-id="c9a01-216">Nakonfigurujte fond back-end IP adresy s názvem **apimbackend** s hello interní virtuální IP adresu hello služba API Management vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="c9a01-216">Configure a back-end IP address pool named **apimbackend**  with hello internal virtual IP address of hello API Management service created above.</span></span>

```powershell
$apimProxyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.StaticIPs[0]
```

### <a name="step-10"></a><span data-ttu-id="c9a01-217">Krok 10</span><span class="sxs-lookup"><span data-stu-id="c9a01-217">Step 10</span></span>

<span data-ttu-id="c9a01-218">Vytvořte nastavení pro fiktivní back-end (neexistující).</span><span class="sxs-lookup"><span data-stu-id="c9a01-218">Create settings for a dummy (non-existent) backend.</span></span> <span data-ttu-id="c9a01-219">Cesty tooAPI požadavky, které jsme nechcete tooexpose ze správy rozhraní API prostřednictvím brány aplikace bude dosáhl tento back-end a vrátit 404.</span><span class="sxs-lookup"><span data-stu-id="c9a01-219">Requests tooAPI paths that we do not want tooexpose from API Management via Application Gateway will hit this backend and return 404.</span></span>

<span data-ttu-id="c9a01-220">Nakonfigurujte nastavení protokolu HTTP pro fiktivní back-end hello.</span><span class="sxs-lookup"><span data-stu-id="c9a01-220">Configure HTTP settings for hello dummy backend.</span></span>

```powershell
$dummyBackendSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "dummySetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

<span data-ttu-id="c9a01-221">Konfigurace fiktivní back-end **dummyBackendPool**, která odkazuje tooa plně kvalifikovaný název domény adresy **dummybackend.com**. Tato adresa plně kvalifikovaný název domény ve virtuální síti hello neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c9a01-221">Configure a dummy backend **dummyBackendPool**, which points tooa FQDN address **dummybackend.com**. This FQDN address does not exist in hello virtual network.</span></span>

```powershell
$dummyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "dummyBackendPool" -BackendFqdns "dummybackend.com"
```

<span data-ttu-id="c9a01-222">Vytvoření pravidla nastavení tohoto hello Application Gateway bude používat ve výchozím nastavení, které odkazuje back-end neexistující toohello **dummybackend.com** v hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="c9a01-222">Create a rule setting that hello Application Gateway will use by default which points toohello non-existent backend **dummybackend.com** in hello Virtual Network.</span></span>

```powershell
$dummyPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "nonexistentapis" -Paths "/*" -BackendAddressPool $dummyBackendPool -BackendHttpSettings $dummyBackendSetting
```

### <a name="step-11"></a><span data-ttu-id="c9a01-223">Krok 11</span><span class="sxs-lookup"><span data-stu-id="c9a01-223">Step 11</span></span>

<span data-ttu-id="c9a01-224">Konfigurovat pravidla cesty adresy URL pro hello back endové fondy.</span><span class="sxs-lookup"><span data-stu-id="c9a01-224">Configure URL rule paths for hello back-end pools.</span></span> <span data-ttu-id="c9a01-225">To umožňuje výběr jen některé z hello rozhraní API pro správu rozhraní API pro probíhá zveřejněné toohello veřejné.</span><span class="sxs-lookup"><span data-stu-id="c9a01-225">This enables selecting only some of hello APIs from API Management for being exposed toohello public.</span></span> <span data-ttu-id="c9a01-226">Například, pokud existují `Echo API` (/ echo /), `Calculator API` (/calc/) atd. Zkontrolujte pouze `Echo API` přístupné z Internetu).</span><span class="sxs-lookup"><span data-stu-id="c9a01-226">For example, if there are `Echo API` (/echo/), `Calculator API` (/calc/) etc. make only `Echo API` accessible from Internet).</span></span> 

<span data-ttu-id="c9a01-227">Hello následující příklad vytvoří jednoduché pravidlo pro hello "/ echo /" cesty směrování provozu toohello back-end "apimProxyBackendPool".</span><span class="sxs-lookup"><span data-stu-id="c9a01-227">hello following example creates a simple rule for hello "/echo/" path routing traffic toohello back-end "apimProxyBackendPool".</span></span>

```powershell
$echoapiRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "externalapis" -Paths "/echo/*" -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
```

<span data-ttu-id="c9a01-228">Pokud cesta hello neodpovídá hello cesta pravidla chceme tooenable ze správy rozhraní API, hello pravidlo cesty mapy konfigurace nakonfiguruje taky výchozího fondu adres back-end s názvem **dummyBackendPool**.</span><span class="sxs-lookup"><span data-stu-id="c9a01-228">If hello path doesn't match hello path rules we want tooenable from API Management, hello rule path map configuration also configures a default back-end address pool named **dummyBackendPool**.</span></span> <span data-ttu-id="c9a01-229">Například http://api.contoso.net/calc/ * přejde příliš**dummyBackendPool** definovaným jako hello výchozí fond pro zrušení odpovídající provoz.</span><span class="sxs-lookup"><span data-stu-id="c9a01-229">For example, http://api.contoso.net/calc/* goes too**dummyBackendPool** as it is defined as hello default pool for un-matched traffic.</span></span>

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $echoapiRule, $dummyPathRule -DefaultBackendAddressPool $dummyBackendPool -DefaultBackendHttpSettings $dummyBackendSetting
```

<span data-ttu-id="c9a01-230">Hello výše krok zajistí, který pouze požadavky pro cestu hello "/ odezvu" jsou povoleny prostřednictvím hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c9a01-230">hello above step ensures that only requests for hello path "/echo" are allowed through hello Application Gateway.</span></span> <span data-ttu-id="c9a01-231">Tooother požadavky, které rozhraní API nakonfigurovaný ve službě API Management vyvolá výjimku chyby 404 ze Application Gateway při přístupu z Internetu hello.</span><span class="sxs-lookup"><span data-stu-id="c9a01-231">Requests tooother APIs configured in API Management will throw 404 errors from Application Gateway when accessed from hello Internet.</span></span> 

### <a name="step-12"></a><span data-ttu-id="c9a01-232">Krok 12</span><span class="sxs-lookup"><span data-stu-id="c9a01-232">Step 12</span></span>

<span data-ttu-id="c9a01-233">Vytvoření pravidla nastavení pro hello Application Gateway toouse směrování adres URL na základě cesty.</span><span class="sxs-lookup"><span data-stu-id="c9a01-233">Create a rule setting for hello Application Gateway toouse URL path-based routing.</span></span>

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-13"></a><span data-ttu-id="c9a01-234">Krok 13</span><span class="sxs-lookup"><span data-stu-id="c9a01-234">Step 13</span></span>

<span data-ttu-id="c9a01-235">Nakonfigurujte hello počet instancí a velikost hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c9a01-235">Configure hello number of instances and size for hello Application Gateway.</span></span> <span data-ttu-id="c9a01-236">Tady používáme hello [firewall webových aplikací SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) pro zvýšení zabezpečení hello prostředků API Management.</span><span class="sxs-lookup"><span data-stu-id="c9a01-236">Here we are using hello [WAF SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) for increased security of hello API Management resource.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-14"></a><span data-ttu-id="c9a01-237">Krok 14</span><span class="sxs-lookup"><span data-stu-id="c9a01-237">Step 14</span></span>

<span data-ttu-id="c9a01-238">Nakonfigurujte toobe firewall webových aplikací v režimu "Prevence".</span><span class="sxs-lookup"><span data-stu-id="c9a01-238">Configure WAF toobe in "Prevention" mode.</span></span>
```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a><span data-ttu-id="c9a01-239">Vytvořte aplikační bránu</span><span class="sxs-lookup"><span data-stu-id="c9a01-239">Create Application Gateway</span></span>

<span data-ttu-id="c9a01-240">Vytvoření služby Application Gateway se všemi objekty konfigurace hello z předchozích kroků hello.</span><span class="sxs-lookup"><span data-stu-id="c9a01-240">Create an Application Gateway with all hello configuration objects from hello preceding steps.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name $applicationGatewayName -ResourceGroupName $resourceGroupName  -Location $location -BackendAddressPools $apimProxyBackendPool, $dummyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $dummyBackendSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert -Probes $apimprobe
```

## <a name="cname-hello-api-management-proxy-hostname-toohello-public-dns-name-of-hello-application-gateway-resource"></a><span data-ttu-id="c9a01-241">CNAME hello API Management proxy hostitele toohello veřejného názvu DNS hello prostředku aplikační brány</span><span class="sxs-lookup"><span data-stu-id="c9a01-241">CNAME hello API Management proxy hostname toohello public DNS name of hello Application Gateway resource</span></span>

<span data-ttu-id="c9a01-242">Po vytvoření brány hello hello dalším krokem je tooconfigure hello front-end pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="c9a01-242">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="c9a01-243">Pokud používáte veřejnou IP adresu, aplikační brána vyžaduje dynamicky přiřazené název DNS, který nemusí být snadno toouse.</span><span class="sxs-lookup"><span data-stu-id="c9a01-243">When using a public IP, Application Gateway requires a dynamically assigned DNS name, which may not be easy toouse.</span></span> 

<span data-ttu-id="c9a01-244">Hello Application Gateway název DNS by měl být použité toocreate záznam CNAME, který ukazuje název hostitele proxy APIM hello (například `api.contoso.net` ve výše uvedených příkladech hello) toothis název DNS.</span><span class="sxs-lookup"><span data-stu-id="c9a01-244">hello Application Gateway's DNS name should be used toocreate a CNAME record which points hello APIM proxy host name (e.g. `api.contoso.net` in hello examples above) toothis DNS name.</span></span> <span data-ttu-id="c9a01-245">tooconfigure hello front-endu záznam IP CNAME načíst hello podrobnosti o hello aplikační brány a svému přidruženému názvu IP a DNS pomocí elementu PublicIPAddress hello.</span><span class="sxs-lookup"><span data-stu-id="c9a01-245">tooconfigure hello frontend IP CNAME record, retrieve hello details of hello Application Gateway and its associated IP/DNS name using hello PublicIPAddress element.</span></span> <span data-ttu-id="c9a01-246">použití Hello záznamů A nedoporučuje, protože hello VIP může změnit při restartu brány.</span><span class="sxs-lookup"><span data-stu-id="c9a01-246">hello use of A-records is not recommended since hello VIP may change on restart of gateway.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -Name "publicIP01"
```

##<span data-ttu-id="c9a01-247"><a name="summary"></a> Souhrn</span><span class="sxs-lookup"><span data-stu-id="c9a01-247"><a name="summary"> </a> Summary</span></span>
<span data-ttu-id="c9a01-248">Nakonfigurovat ve virtuální síti Azure API Management poskytuje rozhraní jedna brána pro všechny nakonfigurované rozhraní API, ať už jsou hostované v místní nebo v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="c9a01-248">Azure API Management configured in a VNET provides a single gateway interface for all configured APIs, whether they are hosted on-prem or in hello cloud.</span></span> <span data-ttu-id="c9a01-249">Integrace Application Gateway s API Management nabízí flexibilitu hello selektivně povolení konkrétní rozhraní API toobe dostupné na hello Internet, jakož i poskytnutí brány Firewall webových aplikací jako instance API Management tooyour front-endu.</span><span class="sxs-lookup"><span data-stu-id="c9a01-249">Integrating Application Gateway with API Management provides hello flexibility of selectively enabling particular APIs toobe accessible on hello Internet, as well as providing a Web Application Firewall as a frontend tooyour API Management instance.</span></span>

##<span data-ttu-id="c9a01-250"><a name="next-steps"></a> Další kroky</span><span class="sxs-lookup"><span data-stu-id="c9a01-250"><a name="next-steps"> </a> Next steps</span></span>
* <span data-ttu-id="c9a01-251">Další informace o Azure Application Gateway</span><span class="sxs-lookup"><span data-stu-id="c9a01-251">Learn more about Azure Application Gateway</span></span>
  * [<span data-ttu-id="c9a01-252">Přehled brány aplikace</span><span class="sxs-lookup"><span data-stu-id="c9a01-252">Application Gateway Overview</span></span>](../application-gateway/application-gateway-introduction.md)
  * [<span data-ttu-id="c9a01-253">Brány Firewall webových aplikací Application Gateway</span><span class="sxs-lookup"><span data-stu-id="c9a01-253">Application Gateway Web Application Firewall</span></span>](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [<span data-ttu-id="c9a01-254">Aplikační bránu pomocí směrování na základě cesty</span><span class="sxs-lookup"><span data-stu-id="c9a01-254">Application Gateway using Path-based Routing</span></span>](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* <span data-ttu-id="c9a01-255">Další informace o rozhraní API správy a virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="c9a01-255">Learn more about API Management and VNETs</span></span>
  * [<span data-ttu-id="c9a01-256">Použití služby API Management, které jsou k dispozici pouze v rámci hello virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="c9a01-256">Using API Management available only within hello VNET</span></span>](api-management-using-with-internal-vnet.md)
  * [<span data-ttu-id="c9a01-257">Použití služby API Management ve virtuální síti</span><span class="sxs-lookup"><span data-stu-id="c9a01-257">Using API Management in VNET</span></span>](api-management-using-with-vnet.md)
