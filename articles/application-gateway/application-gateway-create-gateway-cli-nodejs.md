---
title: "aaaCreate služby Azure Application Gateway - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate služby Application Gateway pomocí Azure CLI 1.0 ve službě Správce prostředků"
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
ms.openlocfilehash: 3c0d2d96b6be404d0372d00f0deb2a32959ca419
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-by-using-hello-azure-cli"></a><span data-ttu-id="2c79a-103">Vytvoření služby application gateway pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="2c79a-103">Create an application gateway by using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2c79a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2c79a-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="2c79a-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="2c79a-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="2c79a-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="2c79a-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="2c79a-107">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="2c79a-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="2c79a-108">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="2c79a-108">Azure CLI 1.0</span></span>](application-gateway-create-gateway-cli.md)
> * [<span data-ttu-id="2c79a-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="2c79a-109">Azure CLI 2.0</span></span>](application-gateway-create-gateway-cli.md)
> 
> 

<span data-ttu-id="2c79a-110">Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7.</span><span class="sxs-lookup"><span data-stu-id="2c79a-110">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="2c79a-111">Poskytuje převzetí služeb při selhání, směrování výkonu požadavků HTTP mezi různými servery, ať už jsou hello cloudu nebo místně.</span><span class="sxs-lookup"><span data-stu-id="2c79a-111">It provides failover, performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="2c79a-112">Application gateway poskytuje následující funkce doručování aplikací hello: HTTP načíst vlastní stavu sondy vyrovnávání, spřažení relace na základě souborů cookie a přesměrování zpracování Secure Sockets Layer (SSL) a podpora pro více lokalit.</span><span class="sxs-lookup"><span data-stu-id="2c79a-112">Application gateway has hello following application delivery features: HTTP load balancing, cookie-based session affinity, and Secure Sockets Layer (SSL) offload, custom health probes, and support for multi-site.</span></span>

## <a name="prerequisite-install-hello-azure-cli"></a><span data-ttu-id="2c79a-113">Předpoklad: Instalace hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="2c79a-113">Prerequisite: Install hello Azure CLI</span></span>

<span data-ttu-id="2c79a-114">tooperform hello kroky v tomto článku, budete potřebovat příliš[instalace hello rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](../xplat-cli-install.md) a potřebujete příliš[přihlásit tooAzure](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="2c79a-114">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](../xplat-cli-install.md) and you need too[log on tooAzure](../xplat-cli-connect.md).</span></span> 

> [!NOTE]
> <span data-ttu-id="2c79a-115">Pokud nemáte účet Azure, budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="2c79a-115">If you don't have an Azure account, you need one.</span></span> <span data-ttu-id="2c79a-116">Zde si můžete zaregistrovat [bezplatnou zkušební verzi](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="2c79a-116">Go sign up for a [free trial here](../active-directory/sign-up-organization.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="2c79a-117">Scénář</span><span class="sxs-lookup"><span data-stu-id="2c79a-117">Scenario</span></span>

<span data-ttu-id="2c79a-118">V tomto scénáři zjistíte, jak hello toocreate brány aplikace pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2c79a-118">In this scenario, you learn how toocreate an application gateway using hello Azure portal.</span></span>

<span data-ttu-id="2c79a-119">Tento scénář se:</span><span class="sxs-lookup"><span data-stu-id="2c79a-119">This scenario will:</span></span>

* <span data-ttu-id="2c79a-120">Vytvoření střední application gateway se dvěma instancemi.</span><span class="sxs-lookup"><span data-stu-id="2c79a-120">Create a medium application gateway with two instances.</span></span>
* <span data-ttu-id="2c79a-121">Vytvořte virtuální síť s názvem ContosoVNET s vyhrazeným blokem CIDR 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="2c79a-121">Create a virtual network named ContosoVNET with a reserved CIDR block of 10.0.0.0/16.</span></span>
* <span data-ttu-id="2c79a-122">Vytvoříte podsíť s názvem subnet01, který používá jako jeho blok CIDR 10.0.0.0/28.</span><span class="sxs-lookup"><span data-stu-id="2c79a-122">Create a subnet called subnet01 that uses 10.0.0.0/28 as its CIDR block.</span></span>

> [!NOTE]
> <span data-ttu-id="2c79a-123">Další konfigurace hello aplikační brány, včetně stavu vlastní testy, adresy fondu back-end a dalších pravidlech nakonfigurovány po hello aplikace brána je nakonfigurovaná a ne během počátečního nasazení.</span><span class="sxs-lookup"><span data-stu-id="2c79a-123">Additional configuration of hello application gateway, including custom health probes, backend pool addresses, and additional rules are configured after hello application gateway is configured and not during initial deployment.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="2c79a-124">Než začnete</span><span class="sxs-lookup"><span data-stu-id="2c79a-124">Before you begin</span></span>

<span data-ttu-id="2c79a-125">Služba Azure Application Gateway vyžaduje vlastní podsíti.</span><span class="sxs-lookup"><span data-stu-id="2c79a-125">Azure Application Gateway requires its own subnet.</span></span> <span data-ttu-id="2c79a-126">Při vytváření virtuální sítě, ujistěte se, ponechte dostatek místa toohave adresu více podsítí.</span><span class="sxs-lookup"><span data-stu-id="2c79a-126">When creating a virtual network, ensure that you leave enough address space toohave multiple subnets.</span></span> <span data-ttu-id="2c79a-127">Jakmile nasadíte tooa podsítě application gateway, jsou pouze další aplikaci brány možné toobe přidat toohello podsítě.</span><span class="sxs-lookup"><span data-stu-id="2c79a-127">Once you deploy an application gateway tooa subnet, only additional application gateways are able toobe added toohello subnet.</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="2c79a-128">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="2c79a-128">Log in tooAzure</span></span>

<span data-ttu-id="2c79a-129">Otevřete hello **Microsoft Azure příkazového řádku**a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="2c79a-129">Open hello **Microsoft Azure Command Prompt**, and log in.</span></span> 

```azurecli-interactive
azure login
```

<span data-ttu-id="2c79a-130">Jakmile zadáte hello předchozím příkladu, je k dispozici kód.</span><span class="sxs-lookup"><span data-stu-id="2c79a-130">Once you type hello preceding example, a code is provided.</span></span> <span data-ttu-id="2c79a-131">Přejděte toohttps://aka.ms/devicelogin v procesu přihlášení hello toocontinue prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="2c79a-131">Navigate toohttps://aka.ms/devicelogin in a browser toocontinue hello login process.</span></span>

![cmd zobrazující zařízení přihlášení][1]

<span data-ttu-id="2c79a-133">V prohlížeči hello zadejte kód hello, kterou jste obdrželi.</span><span class="sxs-lookup"><span data-stu-id="2c79a-133">In hello browser, enter hello code you received.</span></span> <span data-ttu-id="2c79a-134">Jste přihlašovací stránku přesměrovaného tooa.</span><span class="sxs-lookup"><span data-stu-id="2c79a-134">You are redirected tooa sign-in page.</span></span>

![Prohlížeč tooenter kódu][2]

<span data-ttu-id="2c79a-136">Jakmile byl zadán kód hello jste přihlášeni, Zavřít hello prohlížeče toocontinue se scénářem hello.</span><span class="sxs-lookup"><span data-stu-id="2c79a-136">Once hello code has been entered you are signed in, close hello browser toocontinue on with hello scenario.</span></span>

![Úspěšné přihlášení][3]

## <a name="switch-tooresource-manager-mode"></a><span data-ttu-id="2c79a-138">Přepínač tooResource režimu Manager</span><span class="sxs-lookup"><span data-stu-id="2c79a-138">Switch tooResource Manager Mode</span></span>

```azurecli-interactive
azure config mode arm
```

## <a name="create-hello-resource-group"></a><span data-ttu-id="2c79a-139">Vytvořte skupinu prostředků hello</span><span class="sxs-lookup"><span data-stu-id="2c79a-139">Create hello resource group</span></span>

<span data-ttu-id="2c79a-140">Před vytvořením hello aplikační bránu, se vytvoří skupinu prostředků toocontain hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="2c79a-140">Before creating hello application gateway, a resource group is created toocontain hello application gateway.</span></span> <span data-ttu-id="2c79a-141">Následující Hello ukazuje příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="2c79a-141">hello following shows hello command.</span></span>

```azurecli-interactive
azure group create \
--name ContosoRG \
--location eastus
```

## <a name="create-a-virtual-network"></a><span data-ttu-id="2c79a-142">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="2c79a-142">Create a virtual network</span></span>

<span data-ttu-id="2c79a-143">Po vytvoření skupiny prostředků hello vytvoření virtuální sítě pro službu hello application gateway.</span><span class="sxs-lookup"><span data-stu-id="2c79a-143">Once hello resource group is created, a virtual network is created for hello application gateway.</span></span>  <span data-ttu-id="2c79a-144">V následujícím příkladu hello hello adresní prostor se jako 10.0.0.0/16 jak je definována v předchozím scénáři poznámky hello.</span><span class="sxs-lookup"><span data-stu-id="2c79a-144">In hello following example, hello address space was as 10.0.0.0/16 as defined in hello preceding scenario notes.</span></span>

```azurecli-interactive
azure network vnet create \
--name ContosoVNET \
--address-prefixes 10.0.0.0/16 \
--resource-group ContosoRG \
--location eastus
```

## <a name="create-a-subnet"></a><span data-ttu-id="2c79a-145">Vytvoření podsítě</span><span class="sxs-lookup"><span data-stu-id="2c79a-145">Create a subnet</span></span>

<span data-ttu-id="2c79a-146">Po vytvoření virtuální sítě hello je hello aplikační brány přidat podsíť.</span><span class="sxs-lookup"><span data-stu-id="2c79a-146">After hello virtual network is created, a subnet is added for hello application gateway.</span></span>  <span data-ttu-id="2c79a-147">Pokud máte v plánu toouse aplikační bránu s webové aplikace hostované v hello stejné virtuální síť jako hello aplikační brány, nebude se tooleave dostatek místa pro jinou podsíť.</span><span class="sxs-lookup"><span data-stu-id="2c79a-147">If you plan toouse application gateway with a web app hosted in hello same virtual network as hello application gateway, be sure tooleave enough room for another subnet.</span></span>

```azurecli-interactive
azure network vnet subnet create \
--resource-group ContosoRG \
--name subnet01 \
--vnet-name ContosoVNET \
--address-prefix 10.0.0.0/28 
```

## <a name="create-hello-application-gateway"></a><span data-ttu-id="2c79a-148">Vytvoření hello application gateway</span><span class="sxs-lookup"><span data-stu-id="2c79a-148">Create hello application gateway</span></span>

<span data-ttu-id="2c79a-149">Po vytvoření hello virtuální síť a podsíť hello předpoklady pro službu hello application gateway jsou dokončeny.</span><span class="sxs-lookup"><span data-stu-id="2c79a-149">Once hello virtual network and subnet are created, hello pre-requisites for hello application gateway are complete.</span></span> <span data-ttu-id="2c79a-150">Kromě toho jsou požadovány pro hello následující krok .pfx dříve exportovaný certifikát a hello heslo pro certifikát hello: hello adres IP použitých pro back-end hello jsou hello IP adresy pro back-end serveru.</span><span class="sxs-lookup"><span data-stu-id="2c79a-150">Additionally a previously exported .pfx certificate and hello password for hello certificate are required for hello following step: hello IP addresses used for hello backend are hello IP addresses for your backend server.</span></span> <span data-ttu-id="2c79a-151">Tyto hodnoty mohou být buď soukromé IP adresy ve virtuální síti hello, veřejné IP adresy nebo plně kvalifikované názvy domény pro back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="2c79a-151">These values can be either private IPs in hello virtual network, public ips, or fully qualified domain names for your backend servers.</span></span>

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
> <span data-ttu-id="2c79a-152">Seznam parametrů, které lze zadat během vytváření, spusťte následující příkaz hello: **síť azure aplikace gateway vytvořit – pomáhají**.</span><span class="sxs-lookup"><span data-stu-id="2c79a-152">For a list of parameters that can be provided during creation run hello following command: **azure network application-gateway create --help**.</span></span>

<span data-ttu-id="2c79a-153">Tento příklad vytvoří základní aplikační brána s výchozím nastavením pro naslouchací proces hello, fond back-end, nastavení http back-end a pravidla.</span><span class="sxs-lookup"><span data-stu-id="2c79a-153">This example creates a basic application gateway with default settings for hello listener, backend pool, backend http settings, and rules.</span></span> <span data-ttu-id="2c79a-154">Můžete upravit tyto toosuit nastavení nasazení po úspěšné hello zřizování.</span><span class="sxs-lookup"><span data-stu-id="2c79a-154">You can modify these settings toosuit your deployment once hello provisioning is successful.</span></span>
<span data-ttu-id="2c79a-155">Pokud již máte webovou aplikaci definované s fondem back-end hello v předchozích krocích, po vytvoření, hello Vyrovnávání zatížení začne.</span><span class="sxs-lookup"><span data-stu-id="2c79a-155">If you already have your web application defined with hello backend pool in hello preceding steps, once created, load balancing begins.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c79a-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c79a-156">Next steps</span></span>

<span data-ttu-id="2c79a-157">Zjistěte, jak testy toocreate vlastní stavu navštivte stránky [vytvořit vlastní stav testu](application-gateway-create-probe-portal.md)</span><span class="sxs-lookup"><span data-stu-id="2c79a-157">Learn how toocreate custom health probes by visiting [Create a custom health probe](application-gateway-create-probe-portal.md)</span></span>

<span data-ttu-id="2c79a-158">Zjistěte, jak tooconfigure snižování zátěže protokolu SSL a proveďte hello nákladná SSL dešifrování vypnout navštivte stránky webových serverů [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-arm.md)</span><span class="sxs-lookup"><span data-stu-id="2c79a-158">Learn how tooconfigure SSL Offloading and take hello costly SSL decryption off your web servers by visiting [Configure SSL Offload](application-gateway-ssl-arm.md)</span></span>

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli-nodejs/scenario.png
[1]: ./media/application-gateway-create-gateway-cli-nodejs/figure1.png
[2]: ./media/application-gateway-create-gateway-cli-nodejs/figure2.png
[3]: ./media/application-gateway-create-gateway-cli-nodejs/figure3.png
