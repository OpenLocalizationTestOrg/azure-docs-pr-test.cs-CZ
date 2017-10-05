---
title: "Vytvoření služby Azure Application Gateway - rozhraní příkazového řádku Azure 1.0 | Microsoft Docs"
description: "Informace o vytvoření služby Application Gateway pomocí Azure CLI 1.0 ve službě Správce prostředků"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: e7b16e789e0f241aa8ca2292aacb2bccde8777ee
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-application-gateway-by-using-the-azure-cli"></a><span data-ttu-id="c1263-103">Vytvoření služby application gateway pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c1263-103">Create an application gateway by using the Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c1263-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c1263-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="c1263-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="c1263-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="c1263-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="c1263-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="c1263-107">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="c1263-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="c1263-108">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="c1263-108">Azure CLI 1.0</span></span>](application-gateway-create-gateway-cli.md)
> * [<span data-ttu-id="c1263-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c1263-109">Azure CLI 2.0</span></span>](application-gateway-create-gateway-cli.md)
> 
> 

<span data-ttu-id="c1263-110">Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7.</span><span class="sxs-lookup"><span data-stu-id="c1263-110">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="c1263-111">Poskytuje převzetí služeb při selhání, směrování výkonu požadavků HTTP mezi různými servery, ať už jsou místní nebo v cloudu.</span><span class="sxs-lookup"><span data-stu-id="c1263-111">It provides failover, performance-routing HTTP requests between different servers, whether they are on the cloud or on-premises.</span></span> <span data-ttu-id="c1263-112">Application gateway poskytuje následující funkce doručování aplikací: HTTP načíst vlastní stavu sondy vyrovnávání, spřažení relace na základě souborů cookie a přesměrování zpracování Secure Sockets Layer (SSL) a podpora pro více lokalit.</span><span class="sxs-lookup"><span data-stu-id="c1263-112">Application gateway has the following application delivery features: HTTP load balancing, cookie-based session affinity, and Secure Sockets Layer (SSL) offload, custom health probes, and support for multi-site.</span></span>

## <a name="prerequisite-install-the-azure-cli"></a><span data-ttu-id="c1263-113">Předpoklad: Instalace rozhraní příkazového řádku Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c1263-113">Prerequisite: Install the Azure CLI</span></span>

<span data-ttu-id="c1263-114">Chcete-li provést kroky v tomto článku, je potřeba [nainstalovat rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](../xplat-cli-install.md) a potřebujete [Přihlaste se k Azure](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="c1263-114">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](../xplat-cli-install.md) and you need to [log on to Azure](../xplat-cli-connect.md).</span></span> 

> [!NOTE]
> <span data-ttu-id="c1263-115">Pokud nemáte účet Azure, budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="c1263-115">If you don't have an Azure account, you need one.</span></span> <span data-ttu-id="c1263-116">Zde si můžete zaregistrovat [bezplatnou zkušební verzi](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="c1263-116">Go sign up for a [free trial here](../active-directory/sign-up-organization.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="c1263-117">Scénář</span><span class="sxs-lookup"><span data-stu-id="c1263-117">Scenario</span></span>

<span data-ttu-id="c1263-118">V tomto scénáři zjistíte postup vytvoření služby application gateway pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c1263-118">In this scenario, you learn how to create an application gateway using the Azure portal.</span></span>

<span data-ttu-id="c1263-119">Tento scénář se:</span><span class="sxs-lookup"><span data-stu-id="c1263-119">This scenario will:</span></span>

* <span data-ttu-id="c1263-120">Vytvoření střední application gateway se dvěma instancemi.</span><span class="sxs-lookup"><span data-stu-id="c1263-120">Create a medium application gateway with two instances.</span></span>
* <span data-ttu-id="c1263-121">Vytvořte virtuální síť s názvem ContosoVNET s vyhrazeným blokem CIDR 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="c1263-121">Create a virtual network named ContosoVNET with a reserved CIDR block of 10.0.0.0/16.</span></span>
* <span data-ttu-id="c1263-122">Vytvoříte podsíť s názvem subnet01, který používá jako jeho blok CIDR 10.0.0.0/28.</span><span class="sxs-lookup"><span data-stu-id="c1263-122">Create a subnet called subnet01 that uses 10.0.0.0/28 as its CIDR block.</span></span>

> [!NOTE]
> <span data-ttu-id="c1263-123">Další konfigurace aplikační brány, včetně stavu vlastní testy, adresy fondu back-end a dalších pravidlech nakonfigurovány po dokončení konfigurace aplikační brány a ne během počátečního nasazení.</span><span class="sxs-lookup"><span data-stu-id="c1263-123">Additional configuration of the application gateway, including custom health probes, backend pool addresses, and additional rules are configured after the application gateway is configured and not during initial deployment.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c1263-124">Než začnete</span><span class="sxs-lookup"><span data-stu-id="c1263-124">Before you begin</span></span>

<span data-ttu-id="c1263-125">Služba Azure Application Gateway vyžaduje vlastní podsíti.</span><span class="sxs-lookup"><span data-stu-id="c1263-125">Azure Application Gateway requires its own subnet.</span></span> <span data-ttu-id="c1263-126">Při vytváření virtuální sítě, ujistěte se, že necháte dostatek adresní prostor tak, aby měl více podsítí.</span><span class="sxs-lookup"><span data-stu-id="c1263-126">When creating a virtual network, ensure that you leave enough address space to have multiple subnets.</span></span> <span data-ttu-id="c1263-127">Po nasazení služby application gateway k podsíti, jedinými dodatečnými application Gateway je moct přidat do podsítě.</span><span class="sxs-lookup"><span data-stu-id="c1263-127">Once you deploy an application gateway to a subnet, only additional application gateways are able to be added to the subnet.</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="c1263-128">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="c1263-128">Log in to Azure</span></span>

<span data-ttu-id="c1263-129">Otevřete **Microsoft Azure příkazového řádku**a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="c1263-129">Open the **Microsoft Azure Command Prompt**, and log in.</span></span> 

```azurecli-interactive
azure login
```

<span data-ttu-id="c1263-130">Jakmile zadáte v předchozím příkladu, je k dispozici kód.</span><span class="sxs-lookup"><span data-stu-id="c1263-130">Once you type the preceding example, a code is provided.</span></span> <span data-ttu-id="c1263-131">Přejděte do https://aka.ms/devicelogin ve prohlížeči a pokračujte v procesu přihlášení.</span><span class="sxs-lookup"><span data-stu-id="c1263-131">Navigate to https://aka.ms/devicelogin in a browser to continue the login process.</span></span>

![cmd zobrazující zařízení přihlášení][1]

<span data-ttu-id="c1263-133">V prohlížeči zadejte kód, který jste dostali.</span><span class="sxs-lookup"><span data-stu-id="c1263-133">In the browser, enter the code you received.</span></span> <span data-ttu-id="c1263-134">Budete přesměrováni na stránku přihlášení.</span><span class="sxs-lookup"><span data-stu-id="c1263-134">You are redirected to a sign-in page.</span></span>

![prohlížeče k zadání kódu][2]

<span data-ttu-id="c1263-136">Jakmile byl zadán kód jste přihlášeni, zavřete prohlížeč pokračovat na tento scénář.</span><span class="sxs-lookup"><span data-stu-id="c1263-136">Once the code has been entered you are signed in, close the browser to continue on with the scenario.</span></span>

![Úspěšné přihlášení][3]

## <a name="switch-to-resource-manager-mode"></a><span data-ttu-id="c1263-138">Přepnout do režimu Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c1263-138">Switch to Resource Manager Mode</span></span>

```azurecli-interactive
azure config mode arm
```

## <a name="create-the-resource-group"></a><span data-ttu-id="c1263-139">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="c1263-139">Create the resource group</span></span>

<span data-ttu-id="c1263-140">Před vytvořením služby application gateway, se tak, aby obsahovala služby application gateway vytvoří skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c1263-140">Before creating the application gateway, a resource group is created to contain the application gateway.</span></span> <span data-ttu-id="c1263-141">Následuje ukázka příkazu.</span><span class="sxs-lookup"><span data-stu-id="c1263-141">The following shows the command.</span></span>

```azurecli-interactive
azure group create \
--name ContosoRG \
--location eastus
```

## <a name="create-a-virtual-network"></a><span data-ttu-id="c1263-142">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="c1263-142">Create a virtual network</span></span>

<span data-ttu-id="c1263-143">Po vytvoření skupiny prostředků pro službu application gateway se vytvoří virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="c1263-143">Once the resource group is created, a virtual network is created for the application gateway.</span></span>  <span data-ttu-id="c1263-144">V následujícím příkladu se adresní prostor jako 10.0.0.0/16, jak jsou definovány v předchozím scénáři poznámky.</span><span class="sxs-lookup"><span data-stu-id="c1263-144">In the following example, the address space was as 10.0.0.0/16 as defined in the preceding scenario notes.</span></span>

```azurecli-interactive
azure network vnet create \
--name ContosoVNET \
--address-prefixes 10.0.0.0/16 \
--resource-group ContosoRG \
--location eastus
```

## <a name="create-a-subnet"></a><span data-ttu-id="c1263-145">Vytvoření podsítě</span><span class="sxs-lookup"><span data-stu-id="c1263-145">Create a subnet</span></span>

<span data-ttu-id="c1263-146">Po vytvoření virtuální sítě, přidá se podsíť pro aplikační bránu.</span><span class="sxs-lookup"><span data-stu-id="c1263-146">After the virtual network is created, a subnet is added for the application gateway.</span></span>  <span data-ttu-id="c1263-147">Pokud budete chtít aplikační bránu pomocí webové aplikace hostované ve stejné virtuální síti jako službu application gateway, ujistěte se, zda je dostatek místa pro jinou podsíť.</span><span class="sxs-lookup"><span data-stu-id="c1263-147">If you plan to use application gateway with a web app hosted in the same virtual network as the application gateway, be sure to leave enough room for another subnet.</span></span>

```azurecli-interactive
azure network vnet subnet create \
--resource-group ContosoRG \
--name subnet01 \
--vnet-name ContosoVNET \
--address-prefix 10.0.0.0/28 
```

## <a name="create-the-application-gateway"></a><span data-ttu-id="c1263-148">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="c1263-148">Create the application gateway</span></span>

<span data-ttu-id="c1263-149">Po vytvoření virtuální sítě a podsítě jsou dokončeny předpoklady pro službu application gateway.</span><span class="sxs-lookup"><span data-stu-id="c1263-149">Once the virtual network and subnet are created, the pre-requisites for the application gateway are complete.</span></span> <span data-ttu-id="c1263-150">Kromě toho jsou požadovány pro následující krok .pfx dříve exportovaný certifikát a heslo pro certifikát: IP adresy pro back-end jsou IP adresy pro back-end serveru.</span><span class="sxs-lookup"><span data-stu-id="c1263-150">Additionally a previously exported .pfx certificate and the password for the certificate are required for the following step: The IP addresses used for the backend are the IP addresses for your backend server.</span></span> <span data-ttu-id="c1263-151">Tyto hodnoty může být buď soukromé IP adresy ve virtuální síti, veřejné IP adresy nebo plně kvalifikované názvy domény pro back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="c1263-151">These values can be either private IPs in the virtual network, public ips, or fully qualified domain names for your backend servers.</span></span>

```azurecli-interactive
azure network application-gateway create \
--name AdatumAppGateway \
--location eastus \
--resource-group ContosoRG \
--vnet-name ContosoVNET \
--subnet-name subnet01 \
--servers 134.170.185.46,134.170.188.221,134.170.185.50 \
--capacity 2 \
--sku-tier Standard \
--routing-rule-type Basic \
--frontend-port 80 \
--http-settings-cookie-based-affinity Enabled \
--http-settings-port 80 \
--http-settings-protocol http \
--frontend-port http \
--sku-name Standard_Medium
```

> [!NOTE]
> <span data-ttu-id="c1263-152">Seznam parametrů, které lze zadat během vytváření, spusťte následující příkaz: **síť azure aplikace gateway vytvořit – pomáhají**.</span><span class="sxs-lookup"><span data-stu-id="c1263-152">For a list of parameters that can be provided during creation run the following command: **azure network application-gateway create --help**.</span></span>

<span data-ttu-id="c1263-153">Tento příklad vytvoří základní aplikační brána s výchozím nastavením pro naslouchací proces, fond back-end, nastavení http back-end a pravidla.</span><span class="sxs-lookup"><span data-stu-id="c1263-153">This example creates a basic application gateway with default settings for the listener, backend pool, backend http settings, and rules.</span></span> <span data-ttu-id="c1263-154">Můžete upravit toto nastavení tak, aby vyhovovala vašemu nasazení po úspěšné přidělení přístupových práv.</span><span class="sxs-lookup"><span data-stu-id="c1263-154">You can modify these settings to suit your deployment once the provisioning is successful.</span></span>
<span data-ttu-id="c1263-155">Pokud již máte webové aplikace definovaná pomocí fondu back-end v předchozích krocích po vytvoření, Vyrovnávání zatížení začne.</span><span class="sxs-lookup"><span data-stu-id="c1263-155">If you already have your web application defined with the backend pool in the preceding steps, once created, load balancing begins.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c1263-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c1263-156">Next steps</span></span>

<span data-ttu-id="c1263-157">Naučte se vytvářet vlastní stavu sondy navštivte stránky [vytvořit vlastní stav testu](application-gateway-create-probe-portal.md)</span><span class="sxs-lookup"><span data-stu-id="c1263-157">Learn how to create custom health probes by visiting [Create a custom health probe](application-gateway-create-probe-portal.md)</span></span>

<span data-ttu-id="c1263-158">Zjistěte, jak nakonfigurovat snižování zátěže protokolu SSL a trvat nákladná dešifrování SSL vypnout webových serverů, navštivte stránky [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-arm.md)</span><span class="sxs-lookup"><span data-stu-id="c1263-158">Learn how to configure SSL Offloading and take the costly SSL decryption off your web servers by visiting [Configure SSL Offload](application-gateway-ssl-arm.md)</span></span>

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli-nodejs/scenario.png
[1]: ./media/application-gateway-create-gateway-cli-nodejs/figure1.png
[2]: ./media/application-gateway-create-gateway-cli-nodejs/figure2.png
[3]: ./media/application-gateway-create-gateway-cli-nodejs/figure3.png
