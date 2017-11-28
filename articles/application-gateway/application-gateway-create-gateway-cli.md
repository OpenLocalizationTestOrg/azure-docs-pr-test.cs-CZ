---
title: "aaaCreate služby Azure Application Gateway - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate služby Application Gateway pomocí Azure CLI 2.0 ve službě Správce prostředků"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 952065586cd87d253882438bb779b768d9fd59fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-by-using-hello-azure-cli-20"></a><span data-ttu-id="8442e-103">Vytvoření služby application gateway pomocí Azure CLI 2.0 hello</span><span class="sxs-lookup"><span data-stu-id="8442e-103">Create an application gateway by using hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="8442e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8442e-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="8442e-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="8442e-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="8442e-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="8442e-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="8442e-107">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="8442e-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="8442e-108">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8442e-108">Azure CLI 1.0</span></span>](application-gateway-create-gateway-cli.md)
> * [<span data-ttu-id="8442e-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8442e-109">Azure CLI 2.0</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="8442e-110">Application Gateway je vyhrazené virtuální zařízení poskytující aplikace doručení řadiče (ADC) jako služba nabízí různé vrstvy 7 načíst vyrovnávání možnosti pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8442e-110">Application Gateway is a dedicated virtual appliance that provides application delivery controller (ADC) as a service, offering various layer 7 load balancing capabilities for your application.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="8442e-111">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="8442e-111">CLI versions toocomplete hello task</span></span>

<span data-ttu-id="8442e-112">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="8442e-112">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="8442e-113">[Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) -naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely.</span><span class="sxs-lookup"><span data-stu-id="8442e-113">[Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) - our CLI for hello classic and resource management deployment models.</span></span>
* <span data-ttu-id="8442e-114">[Azure CLI 2.0](application-gateway-create-gateway-cli.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="8442e-114">[Azure CLI 2.0](application-gateway-create-gateway-cli.md) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="prerequisite-install-hello-azure-cli-20"></a><span data-ttu-id="8442e-115">Předpoklad: Instalace hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="8442e-115">Prerequisite: Install hello Azure CLI 2.0</span></span>

<span data-ttu-id="8442e-116">tooperform hello kroky v tomto článku, budete potřebovat příliš[instalace hello rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="8442e-116">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

> [!NOTE]
> <span data-ttu-id="8442e-117">Pokud nemáte účet Azure, budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="8442e-117">If you don't have an Azure account, you need one.</span></span> <span data-ttu-id="8442e-118">Zde si můžete zaregistrovat [bezplatnou zkušební verzi](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="8442e-118">Go sign up for a [free trial here](../active-directory/sign-up-organization.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="8442e-119">Scénář</span><span class="sxs-lookup"><span data-stu-id="8442e-119">Scenario</span></span>

<span data-ttu-id="8442e-120">V tomto scénáři zjistíte, jak hello toocreate brány aplikace pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8442e-120">In this scenario, you learn how toocreate an application gateway using hello Azure portal.</span></span>

<span data-ttu-id="8442e-121">Tento scénář se:</span><span class="sxs-lookup"><span data-stu-id="8442e-121">This scenario will:</span></span>

* <span data-ttu-id="8442e-122">Vytvoření střední application gateway se dvěma instancemi.</span><span class="sxs-lookup"><span data-stu-id="8442e-122">Create a medium application gateway with two instances.</span></span>
* <span data-ttu-id="8442e-123">Vytvořte virtuální síť s názvem AdatumAppGatewayVNET s vyhrazeným blokem CIDR 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="8442e-123">Create a virtual network named AdatumAppGatewayVNET with a reserved CIDR block of 10.0.0.0/16.</span></span>
* <span data-ttu-id="8442e-124">Vytvoříte podsíť s názvem Appgatewaysubnet, která používá blok CIDR 10.0.0.0/28.</span><span class="sxs-lookup"><span data-stu-id="8442e-124">Create a subnet called Appgatewaysubnet that uses 10.0.0.0/28 as its CIDR block.</span></span>

> [!NOTE]
> <span data-ttu-id="8442e-125">Další konfigurace hello aplikační brány, včetně stavu vlastní testy, adresy fondu back-end a dalších pravidlech nakonfigurovány po hello aplikace brána je nakonfigurovaná a ne během počátečního nasazení.</span><span class="sxs-lookup"><span data-stu-id="8442e-125">Additional configuration of hello application gateway, including custom health probes, backend pool addresses, and additional rules are configured after hello application gateway is configured and not during initial deployment.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="8442e-126">Než začnete</span><span class="sxs-lookup"><span data-stu-id="8442e-126">Before you begin</span></span>

<span data-ttu-id="8442e-127">Služba Azure Application Gateway vyžaduje vlastní podsíti.</span><span class="sxs-lookup"><span data-stu-id="8442e-127">Azure Application Gateway requires its own subnet.</span></span> <span data-ttu-id="8442e-128">Při vytváření virtuální sítě, ujistěte se, ponechte dostatek místa toohave adresu více podsítí.</span><span class="sxs-lookup"><span data-stu-id="8442e-128">When creating a virtual network, ensure that you leave enough address space toohave multiple subnets.</span></span> <span data-ttu-id="8442e-129">Jakmile nasadíte tooa podsítě application gateway, jedinými dodatečnými application Gateway se dá přidat toohello podsíť.</span><span class="sxs-lookup"><span data-stu-id="8442e-129">Once you deploy an application gateway tooa subnet, only additional application gateways can be added toohello subnet.</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="8442e-130">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="8442e-130">Log in tooAzure</span></span>

<span data-ttu-id="8442e-131">Otevřete hello **Microsoft Azure příkazového řádku**a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="8442e-131">Open hello **Microsoft Azure Command Prompt**, and log in.</span></span> 

```azurecli-interactive
az login -u "username"
```

> [!NOTE]
> <span data-ttu-id="8442e-132">Můžete také použít `az login` bez hello přepínač pro přihlášení zařízení, která vyžaduje zadání kódu v aka.ms/devicelogin.</span><span class="sxs-lookup"><span data-stu-id="8442e-132">You can also use `az login` without hello switch for device login that requires entering a code at aka.ms/devicelogin.</span></span>

<span data-ttu-id="8442e-133">Jakmile zadáte hello předchozím příkladu, je k dispozici kód.</span><span class="sxs-lookup"><span data-stu-id="8442e-133">Once you type hello preceding example, a code is provided.</span></span> <span data-ttu-id="8442e-134">Přejděte toohttps://aka.ms/devicelogin v procesu přihlášení hello toocontinue prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="8442e-134">Navigate toohttps://aka.ms/devicelogin in a browser toocontinue hello login process.</span></span>

![cmd zobrazující zařízení přihlášení][1]

<span data-ttu-id="8442e-136">V prohlížeči hello zadejte kód hello, kterou jste obdrželi.</span><span class="sxs-lookup"><span data-stu-id="8442e-136">In hello browser, enter hello code you received.</span></span> <span data-ttu-id="8442e-137">Jste přihlašovací stránku přesměrovaného tooa.</span><span class="sxs-lookup"><span data-stu-id="8442e-137">You are redirected tooa sign-in page.</span></span>

![Prohlížeč tooenter kódu][2]

<span data-ttu-id="8442e-139">Jakmile byl zadán kód hello jste přihlášeni, Zavřít hello prohlížeče toocontinue se scénářem hello.</span><span class="sxs-lookup"><span data-stu-id="8442e-139">Once hello code has been entered you are signed in, close hello browser toocontinue on with hello scenario.</span></span>

![Úspěšné přihlášení][3]

## <a name="create-hello-resource-group"></a><span data-ttu-id="8442e-141">Vytvořte skupinu prostředků hello</span><span class="sxs-lookup"><span data-stu-id="8442e-141">Create hello resource group</span></span>

<span data-ttu-id="8442e-142">Před vytvořením hello aplikační bránu, se vytvoří skupinu prostředků toocontain hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="8442e-142">Before creating hello application gateway, a resource group is created toocontain hello application gateway.</span></span> <span data-ttu-id="8442e-143">Následující Hello ukazuje příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="8442e-143">hello following shows hello command.</span></span>

```azurecli-interactive
az group create --name myresourcegroup --location "eastus"
```

## <a name="create-hello-application-gateway"></a><span data-ttu-id="8442e-144">Vytvoření hello application gateway</span><span class="sxs-lookup"><span data-stu-id="8442e-144">Create hello application gateway</span></span>

<span data-ttu-id="8442e-145">Hello adres IP použitých pro back-end hello jsou hello IP adresy pro back-end serveru.</span><span class="sxs-lookup"><span data-stu-id="8442e-145">hello IP addresses used for hello backend are hello IP addresses for your backend server.</span></span> <span data-ttu-id="8442e-146">Tyto hodnoty mohou být buď soukromé IP adresy ve virtuální síti hello, veřejné IP adresy nebo plně kvalifikované názvy domény pro back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="8442e-146">These values can be either private IPs in hello virtual network, public ips, or fully qualified domain names for your backend servers.</span></span> <span data-ttu-id="8442e-147">Hello následující příklad vytvoří aplikační brány s další konfiguraci nastavení pro nastavení http, porty a pravidla.</span><span class="sxs-lookup"><span data-stu-id="8442e-147">hello following example creates an application gateway with additional configuration settings for http settings, ports, and rules.</span></span>

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet" \
--subnet-address-prefix "10.0.0.0/28" \
--servers 10.0.0.4 10.0.0.5 \
--capacity 2 \
--sku Standard_Small \
--http-settings-cookie-based-affinity Enabled \
--http-settings-protocol Http \
--frontend-port 80 \
--routing-rule-type Basic \
--http-settings-port 80 \
--public-ip-address "pip2" \
--public-ip-address-allocation "dynamic" \

```

<span data-ttu-id="8442e-148">Hello ukazuje předchozí příklad mnoho vlastností, které nejsou nutné během hello vytvoření služby application gateway.</span><span class="sxs-lookup"><span data-stu-id="8442e-148">hello preceding example shows many properties that are not required during hello creation of an application gateway.</span></span> <span data-ttu-id="8442e-149">Následující ukázka kódu Hello vytvoří aplikační brány s hello požadované informace.</span><span class="sxs-lookup"><span data-stu-id="8442e-149">hello following code example creates an application gateway with hello required information.</span></span>

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet \
--subnet-address-prefix "10.0.0.0/28" \
--servers "10.0.0.5"  \
--public-ip-address pip
```
 
> [!NOTE]
> <span data-ttu-id="8442e-150">Seznam parametrů, které lze zadat během hello vytvoření spusťte následující příkaz: `az network application-gateway create --help`.</span><span class="sxs-lookup"><span data-stu-id="8442e-150">For a list of parameters that can be provided during creation run hello following command: `az network application-gateway create --help`.</span></span>

<span data-ttu-id="8442e-151">Tento příklad vytvoří základní aplikační brána s výchozím nastavením pro naslouchací proces hello, fond back-end, nastavení http back-end a pravidla.</span><span class="sxs-lookup"><span data-stu-id="8442e-151">This example creates a basic application gateway with default settings for hello listener, backend pool, backend http settings, and rules.</span></span> <span data-ttu-id="8442e-152">Můžete upravit tyto toosuit nastavení nasazení po úspěšné hello zřizování.</span><span class="sxs-lookup"><span data-stu-id="8442e-152">You can modify these settings toosuit your deployment once hello provisioning is successful.</span></span>
<span data-ttu-id="8442e-153">Pokud již máte webovou aplikaci definované s fondem back-end hello v předchozích krocích, po vytvoření, hello Vyrovnávání zatížení začne.</span><span class="sxs-lookup"><span data-stu-id="8442e-153">If you already have your web application defined with hello backend pool in hello preceding steps, once created, load balancing begins.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="8442e-154">Získání názvu DNS služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="8442e-154">Get application gateway DNS name</span></span>

<span data-ttu-id="8442e-155">Po vytvoření brány hello hello dalším krokem je tooconfigure hello front-end pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="8442e-155">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="8442e-156">Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný.</span><span class="sxs-lookup"><span data-stu-id="8442e-156">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="8442e-157">tooensure koncoví uživatelé mohou dosáhl hello aplikační bránu, záznam CNAME lze použít toopoint toohello veřejný koncový bod hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="8442e-157">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="8442e-158">[Konfigurace vlastního názvu domény pro cloudovou službu Azure](../dns/dns-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="8442e-158">[Configuring a custom domain name for in Azure](../dns/dns-custom-domain.md).</span></span> <span data-ttu-id="8442e-159">tooconfigure alias, načíst podrobnosti o hello aplikační brány a svému přidruženému názvu IP a DNS pomocí hello PublicIPAddress element připojené toohello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="8442e-159">tooconfigure an alias, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="8442e-160">název DNS Hello Aplikační brána musí být použité toocreate záznam CNAME, které body hello dva webové aplikace toothis název DNS.</span><span class="sxs-lookup"><span data-stu-id="8442e-160">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="8442e-161">Hello použití záznamů A se nedoporučuje, protože hello VIP může změnit při restartu aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="8442e-161">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>


```azurecli-interactive
az network public-ip show --name "pip" --resource-group "AdatumAppGatewayRG"
```

```
{
  "dnsSettings": {
    "domainNameLabel": null,
    "fqdn": "8c786058-96d4-4f3e-bb41-660860ceae4c.cloudapp.net",
    "reverseFqdn": null
  },
  "etag": "W/\"3b0ac031-01f0-4860-b572-e3c25e0c57ad\"",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/publicIPAddresses/pip2",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "40.121.167.250",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/applicationGateways/AdatumAppGateway2/frontendIPConfigurations/appGatewayFrontendIP",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "AdatumAppGatewayRG",
    "subnet": null
  },
  "location": "eastus",
  "name": "pip2",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "AdatumAppGatewayRG",
  "resourceGuid": "3c30d310-c543-4e9d-9c72-bbacd7fe9b05",
  "tags": {
    "cli[2] owner[administrator]": ""
  },
  "type": "Microsoft.Network/publicIPAddresses"
}
```

## <a name="delete-all-resources"></a><span data-ttu-id="8442e-162">Odstranění všech prostředků</span><span class="sxs-lookup"><span data-stu-id="8442e-162">Delete all resources</span></span>

<span data-ttu-id="8442e-163">toodelete vytvořit všechny prostředky v tomto článku, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8442e-163">toodelete all resources created in this article, complete hello following steps:</span></span>

```azurecli-interactive
az group delete --name AdatumAppGatewayRG
```
 
## <a name="next-steps"></a><span data-ttu-id="8442e-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8442e-164">Next steps</span></span>

<span data-ttu-id="8442e-165">Zjistěte, jak testy toocreate vlastní stavu navštivte stránky [vytvořit vlastní stav testu](application-gateway-create-probe-portal.md)</span><span class="sxs-lookup"><span data-stu-id="8442e-165">Learn how toocreate custom health probes by visiting [Create a custom health probe](application-gateway-create-probe-portal.md)</span></span>

<span data-ttu-id="8442e-166">Zjistěte, jak tooconfigure snižování zátěže protokolu SSL a proveďte hello nákladná SSL dešifrování vypnout navštivte stránky webových serverů [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-arm.md)</span><span class="sxs-lookup"><span data-stu-id="8442e-166">Learn how tooconfigure SSL Offloading and take hello costly SSL decryption off your web servers by visiting [Configure SSL Offload](application-gateway-ssl-arm.md)</span></span>

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
