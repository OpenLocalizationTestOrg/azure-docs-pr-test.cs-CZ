---
title: "Vytvoření služby Azure Application Gateway - rozhraní příkazového řádku Azure 2.0 | Microsoft Docs"
description: "Informace o vytvoření služby Application Gateway pomocí Azure CLI 2.0 ve službě Správce prostředků"
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
ms.openlocfilehash: 052410db8c7619c7990dc319951a55663f2c2ba1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-gateway-by-using-the-azure-cli-20"></a><span data-ttu-id="b8b81-103">Vytvoření služby application gateway pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b8b81-103">Create an application gateway by using the Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b8b81-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b8b81-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="b8b81-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="b8b81-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="b8b81-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="b8b81-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="b8b81-107">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="b8b81-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="b8b81-108">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b8b81-108">Azure CLI 1.0</span></span>](application-gateway-create-gateway-cli.md)
> * [<span data-ttu-id="b8b81-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b8b81-109">Azure CLI 2.0</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="b8b81-110">Application Gateway je vyhrazené virtuální zařízení poskytující aplikace doručení řadiče (ADC) jako služba nabízí různé vrstvy 7 načíst vyrovnávání možnosti pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b8b81-110">Application Gateway is a dedicated virtual appliance that provides application delivery controller (ADC) as a service, offering various layer 7 load balancing capabilities for your application.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="b8b81-111">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="b8b81-111">CLI versions to complete the task</span></span>

<span data-ttu-id="b8b81-112">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="b8b81-112">You can complete the task using one of the following CLI versions:</span></span>

* <span data-ttu-id="b8b81-113">[Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="b8b81-113">[Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) - our CLI for the classic and resource management deployment models.</span></span>
* <span data-ttu-id="b8b81-114">[Azure CLI 2.0](application-gateway-create-gateway-cli.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="b8b81-114">[Azure CLI 2.0](application-gateway-create-gateway-cli.md) - our next generation CLI for the resource management deployment model</span></span>

## <a name="prerequisite-install-the-azure-cli-20"></a><span data-ttu-id="b8b81-115">Předpoklad: Instalace rozhraní příkazového řádku Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="b8b81-115">Prerequisite: Install the Azure CLI 2.0</span></span>

<span data-ttu-id="b8b81-116">Chcete-li provést kroky v tomto článku, je potřeba [nainstalovat rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="b8b81-116">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

> [!NOTE]
> <span data-ttu-id="b8b81-117">Pokud nemáte účet Azure, budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="b8b81-117">If you don't have an Azure account, you need one.</span></span> <span data-ttu-id="b8b81-118">Zde si můžete zaregistrovat [bezplatnou zkušební verzi](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="b8b81-118">Go sign up for a [free trial here](../active-directory/sign-up-organization.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="b8b81-119">Scénář</span><span class="sxs-lookup"><span data-stu-id="b8b81-119">Scenario</span></span>

<span data-ttu-id="b8b81-120">V tomto scénáři zjistíte postup vytvoření služby application gateway pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b8b81-120">In this scenario, you learn how to create an application gateway using the Azure portal.</span></span>

<span data-ttu-id="b8b81-121">Tento scénář se:</span><span class="sxs-lookup"><span data-stu-id="b8b81-121">This scenario will:</span></span>

* <span data-ttu-id="b8b81-122">Vytvoření střední application gateway se dvěma instancemi.</span><span class="sxs-lookup"><span data-stu-id="b8b81-122">Create a medium application gateway with two instances.</span></span>
* <span data-ttu-id="b8b81-123">Vytvořte virtuální síť s názvem AdatumAppGatewayVNET s vyhrazeným blokem CIDR 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="b8b81-123">Create a virtual network named AdatumAppGatewayVNET with a reserved CIDR block of 10.0.0.0/16.</span></span>
* <span data-ttu-id="b8b81-124">Vytvoříte podsíť s názvem Appgatewaysubnet, která používá blok CIDR 10.0.0.0/28.</span><span class="sxs-lookup"><span data-stu-id="b8b81-124">Create a subnet called Appgatewaysubnet that uses 10.0.0.0/28 as its CIDR block.</span></span>

> [!NOTE]
> <span data-ttu-id="b8b81-125">Další konfigurace aplikační brány, včetně stavu vlastní testy, adresy fondu back-end a dalších pravidlech nakonfigurovány po dokončení konfigurace aplikační brány a ne během počátečního nasazení.</span><span class="sxs-lookup"><span data-stu-id="b8b81-125">Additional configuration of the application gateway, including custom health probes, backend pool addresses, and additional rules are configured after the application gateway is configured and not during initial deployment.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b8b81-126">Než začnete</span><span class="sxs-lookup"><span data-stu-id="b8b81-126">Before you begin</span></span>

<span data-ttu-id="b8b81-127">Služba Azure Application Gateway vyžaduje vlastní podsíti.</span><span class="sxs-lookup"><span data-stu-id="b8b81-127">Azure Application Gateway requires its own subnet.</span></span> <span data-ttu-id="b8b81-128">Při vytváření virtuální sítě, ujistěte se, že necháte dostatek adresní prostor tak, aby měl více podsítí.</span><span class="sxs-lookup"><span data-stu-id="b8b81-128">When creating a virtual network, ensure that you leave enough address space to have multiple subnets.</span></span> <span data-ttu-id="b8b81-129">Po nasazení služby application gateway k podsíti, lze přidat pouze další aplikačních bran do podsítě.</span><span class="sxs-lookup"><span data-stu-id="b8b81-129">Once you deploy an application gateway to a subnet, only additional application gateways can be added to the subnet.</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="b8b81-130">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="b8b81-130">Log in to Azure</span></span>

<span data-ttu-id="b8b81-131">Otevřete **Microsoft Azure příkazového řádku**a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="b8b81-131">Open the **Microsoft Azure Command Prompt**, and log in.</span></span> 

```azurecli-interactive
az login -u "username"
```

> [!NOTE]
> <span data-ttu-id="b8b81-132">Můžete také použít `az login` bez přepínač pro přihlášení zařízení, která vyžaduje zadání kódu v aka.ms/devicelogin.</span><span class="sxs-lookup"><span data-stu-id="b8b81-132">You can also use `az login` without the switch for device login that requires entering a code at aka.ms/devicelogin.</span></span>

<span data-ttu-id="b8b81-133">Jakmile zadáte v předchozím příkladu, je k dispozici kód.</span><span class="sxs-lookup"><span data-stu-id="b8b81-133">Once you type the preceding example, a code is provided.</span></span> <span data-ttu-id="b8b81-134">Přejděte do https://aka.ms/devicelogin ve prohlížeči a pokračujte v procesu přihlášení.</span><span class="sxs-lookup"><span data-stu-id="b8b81-134">Navigate to https://aka.ms/devicelogin in a browser to continue the login process.</span></span>

![cmd zobrazující zařízení přihlášení][1]

<span data-ttu-id="b8b81-136">V prohlížeči zadejte kód, který jste dostali.</span><span class="sxs-lookup"><span data-stu-id="b8b81-136">In the browser, enter the code you received.</span></span> <span data-ttu-id="b8b81-137">Budete přesměrováni na stránku přihlášení.</span><span class="sxs-lookup"><span data-stu-id="b8b81-137">You are redirected to a sign-in page.</span></span>

![prohlížeče k zadání kódu][2]

<span data-ttu-id="b8b81-139">Jakmile byl zadán kód jste přihlášeni, zavřete prohlížeč pokračovat na tento scénář.</span><span class="sxs-lookup"><span data-stu-id="b8b81-139">Once the code has been entered you are signed in, close the browser to continue on with the scenario.</span></span>

![Úspěšné přihlášení][3]

## <a name="create-the-resource-group"></a><span data-ttu-id="b8b81-141">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="b8b81-141">Create the resource group</span></span>

<span data-ttu-id="b8b81-142">Před vytvořením služby application gateway, se tak, aby obsahovala služby application gateway vytvoří skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="b8b81-142">Before creating the application gateway, a resource group is created to contain the application gateway.</span></span> <span data-ttu-id="b8b81-143">Následuje ukázka příkazu.</span><span class="sxs-lookup"><span data-stu-id="b8b81-143">The following shows the command.</span></span>

```azurecli-interactive
az group create --name myresourcegroup --location "eastus"
```

## <a name="create-the-application-gateway"></a><span data-ttu-id="b8b81-144">Vytvoření služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="b8b81-144">Create the application gateway</span></span>

<span data-ttu-id="b8b81-145">Použít pro back-end IP adresy jsou IP adresy pro back-end serveru.</span><span class="sxs-lookup"><span data-stu-id="b8b81-145">The IP addresses used for the backend are the IP addresses for your backend server.</span></span> <span data-ttu-id="b8b81-146">Tyto hodnoty může být buď soukromé IP adresy ve virtuální síti, veřejné IP adresy nebo plně kvalifikované názvy domény pro back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="b8b81-146">These values can be either private IPs in the virtual network, public ips, or fully qualified domain names for your backend servers.</span></span> <span data-ttu-id="b8b81-147">Následující příklad vytvoří aplikační brány s další konfiguraci nastavení pro nastavení http, porty a pravidla.</span><span class="sxs-lookup"><span data-stu-id="b8b81-147">The following example creates an application gateway with additional configuration settings for http settings, ports, and rules.</span></span>

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

<span data-ttu-id="b8b81-148">Předchozí příklad ukazuje mnoho vlastností, které nejsou nutné během vytváření služby application gateway.</span><span class="sxs-lookup"><span data-stu-id="b8b81-148">The preceding example shows many properties that are not required during the creation of an application gateway.</span></span> <span data-ttu-id="b8b81-149">Následující příklad kódu vytvoří aplikační brány s požadované informace.</span><span class="sxs-lookup"><span data-stu-id="b8b81-149">The following code example creates an application gateway with the required information.</span></span>

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
> <span data-ttu-id="b8b81-150">Seznam parametrů, které lze zadat během vytváření, spusťte následující příkaz: `az network application-gateway create --help`.</span><span class="sxs-lookup"><span data-stu-id="b8b81-150">For a list of parameters that can be provided during creation run the following command: `az network application-gateway create --help`.</span></span>

<span data-ttu-id="b8b81-151">Tento příklad vytvoří základní aplikační brána s výchozím nastavením pro naslouchací proces, fond back-end, nastavení http back-end a pravidla.</span><span class="sxs-lookup"><span data-stu-id="b8b81-151">This example creates a basic application gateway with default settings for the listener, backend pool, backend http settings, and rules.</span></span> <span data-ttu-id="b8b81-152">Můžete upravit toto nastavení tak, aby vyhovovala vašemu nasazení po úspěšné přidělení přístupových práv.</span><span class="sxs-lookup"><span data-stu-id="b8b81-152">You can modify these settings to suit your deployment once the provisioning is successful.</span></span>
<span data-ttu-id="b8b81-153">Pokud již máte webové aplikace definovaná pomocí fondu back-end v předchozích krocích po vytvoření, Vyrovnávání zatížení začne.</span><span class="sxs-lookup"><span data-stu-id="b8b81-153">If you already have your web application defined with the backend pool in the preceding steps, once created, load balancing begins.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="b8b81-154">Získání názvu DNS služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="b8b81-154">Get application gateway DNS name</span></span>

<span data-ttu-id="b8b81-155">Po vytvoření brány je dalším krokem konfigurace front-endu pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="b8b81-155">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="b8b81-156">Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný.</span><span class="sxs-lookup"><span data-stu-id="b8b81-156">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="b8b81-157">Pokud chcete zajistit, aby se koncoví uživatelé mohli dostat ke službě Application Gateway, můžete použít záznam CNAME jako odkaz na veřejný koncový bod služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="b8b81-157">To ensure end users can hit the application gateway, a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="b8b81-158">[Konfigurace vlastního názvu domény pro cloudovou službu Azure](../dns/dns-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="b8b81-158">[Configuring a custom domain name for in Azure](../dns/dns-custom-domain.md).</span></span> <span data-ttu-id="b8b81-159">Pokud chcete konfigurovat alias, načíst podrobnosti o aplikační bránu a svému přidruženému názvu IP a DNS pomocí elementu PublicIPAddress připojit k službě application gateway.</span><span class="sxs-lookup"><span data-stu-id="b8b81-159">To configure an alias, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="b8b81-160">Název DNS služby Application Gateway byste měli použít k vytvoření záznamu CNAME, který tyto dvě webové aplikace odkazuje na tento název DNS.</span><span class="sxs-lookup"><span data-stu-id="b8b81-160">The application gateway's DNS name should be used to create a CNAME record, which points the two web applications to this DNS name.</span></span> <span data-ttu-id="b8b81-161">Použití záznamů A se nedoporučuje z toho důvodu, že virtuální IP adresa se může změnit při restartování služby Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="b8b81-161">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>


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

## <a name="delete-all-resources"></a><span data-ttu-id="b8b81-162">Odstranění všech prostředků</span><span class="sxs-lookup"><span data-stu-id="b8b81-162">Delete all resources</span></span>

<span data-ttu-id="b8b81-163">Pokud chcete odstranit všechny prostředky vytvořené v rámci tohoto článku, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b8b81-163">To delete all resources created in this article, complete the following steps:</span></span>

```azurecli-interactive
az group delete --name AdatumAppGatewayRG
```
 
## <a name="next-steps"></a><span data-ttu-id="b8b81-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b8b81-164">Next steps</span></span>

<span data-ttu-id="b8b81-165">Naučte se vytvářet vlastní stavu sondy navštivte stránky [vytvořit vlastní stav testu](application-gateway-create-probe-portal.md)</span><span class="sxs-lookup"><span data-stu-id="b8b81-165">Learn how to create custom health probes by visiting [Create a custom health probe](application-gateway-create-probe-portal.md)</span></span>

<span data-ttu-id="b8b81-166">Zjistěte, jak nakonfigurovat snižování zátěže protokolu SSL a trvat nákladná dešifrování SSL vypnout webových serverů, navštivte stránky [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-arm.md)</span><span class="sxs-lookup"><span data-stu-id="b8b81-166">Learn how to configure SSL Offloading and take the costly SSL decryption off your web servers by visiting [Configure SSL Offload](application-gateway-ssl-arm.md)</span></span>

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
