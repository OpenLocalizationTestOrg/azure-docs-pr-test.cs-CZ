---
title: "brány firewall v protokolu aaaConfigure webových aplikací – Azure Application Gateway | Microsoft Docs"
description: "Tento článek obsahuje pokyny k jak toostart pomocí webové aplikace brány firewall na existující nebo nové aplikační brány."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 670b9732-874b-43e6-843b-d2585c160982
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: gwallace
ms.openlocfilehash: d5354984760ceab12ed49efa9e18836e9f1d3c96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway-with-azure-cli"></a><span data-ttu-id="49bcf-103">Konfigurace brány firewall webových aplikací na nový nebo existující Application Gateway pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="49bcf-103">Configure web application firewall on a new or existing Application Gateway with Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="49bcf-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="49bcf-104">Azure portal</span></span>](application-gateway-web-application-firewall-portal.md)
> * [<span data-ttu-id="49bcf-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="49bcf-105">PowerShell</span></span>](application-gateway-web-application-firewall-powershell.md)
> * [<span data-ttu-id="49bcf-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="49bcf-106">Azure CLI</span></span>](application-gateway-web-application-firewall-cli.md)

<span data-ttu-id="49bcf-107">Zjistěte, jak toocreate brány firewall webových aplikací povoleno aplikační bránu, nebo přidejte webové aplikace brány firewall tooan existující aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="49bcf-107">Learn how toocreate a web application firewall enabled application gateway or add web application firewall tooan existing application gateway.</span></span>

<span data-ttu-id="49bcf-108">Hello brány firewall webových aplikací (firewall webových aplikací) v Azure Application Gateway chrání webových aplikací z běžných útoky založenými na web jako Injektáž SQL, útoky skriptování mezi weby a hijacks relace.</span><span class="sxs-lookup"><span data-stu-id="49bcf-108">hello web application firewall (WAF) in Azure Application Gateway protects web applications from common web-based attacks like SQL injection, cross-site scripting attacks, and session hijacks.</span></span>

<span data-ttu-id="49bcf-109">Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7.</span><span class="sxs-lookup"><span data-stu-id="49bcf-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="49bcf-110">Poskytuje převzetí služeb při selhání, směrování výkonu požadavků HTTP mezi různými servery, ať už jsou hello cloudu nebo místně.</span><span class="sxs-lookup"><span data-stu-id="49bcf-110">It provides failover, performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="49bcf-111">Aplikační brána poskytuje funkce doručování řadiče (ADC) mnoho aplikací se včetně HTTP Vyrovnávání zatížení, na základě souboru cookie relace spřažení, vrstvu secure Sockets Layer (SSL) snižování zátěže, sondy vlastní stavu, podporu pro více lokalit a mnohé další.</span><span class="sxs-lookup"><span data-stu-id="49bcf-111">Application gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, secure sockets layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="49bcf-112">toofind úplný seznam podporovaných funkcích, navštivte: [Přehled služby Application Gateway](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="49bcf-112">toofind a complete list of supported features, visit: [Overview of Application Gateway](application-gateway-introduction.md).</span></span>

<span data-ttu-id="49bcf-113">Hello následující článek ukazuje, jak příliš[webové aplikace brány firewall tooan existující aplikace brány přidat](#add-web-application-firewall-to-an-existing-application-gateway) a [vytvoření služby application gateway, která používá brány firewall webových aplikací](#create-an-application-gateway-with-web-application-firewall).</span><span class="sxs-lookup"><span data-stu-id="49bcf-113">hello following article shows how too[add web application firewall tooan existing application gateway](#add-web-application-firewall-to-an-existing-application-gateway) and [create an application gateway that uses web application firewall](#create-an-application-gateway-with-web-application-firewall).</span></span>

![scénář image][scenario]

## <a name="prerequisite-install-hello-azure-cli-20"></a><span data-ttu-id="49bcf-115">Předpoklad: Instalace hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="49bcf-115">Prerequisite: Install hello Azure CLI 2.0</span></span>

<span data-ttu-id="49bcf-116">tooperform hello kroky v tomto článku, budete potřebovat příliš[instalace hello rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="49bcf-116">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="waf-configuration-differences"></a><span data-ttu-id="49bcf-117">Rozdíly konfigurace firewall webových aplikací</span><span class="sxs-lookup"><span data-stu-id="49bcf-117">WAF configuration differences</span></span>

<span data-ttu-id="49bcf-118">Pokud jste si přečetli [vytvoření služby Application Gateway pomocí Azure CLI](application-gateway-create-gateway-cli.md), rozumíte hello SKU nastavení tooconfigure při vytváření služby application gateway.</span><span class="sxs-lookup"><span data-stu-id="49bcf-118">If you have read [Create an Application Gateway with Azure CLI](application-gateway-create-gateway-cli.md), you understand hello SKU settings tooconfigure when creating an application gateway.</span></span> <span data-ttu-id="49bcf-119">Firewall webových aplikací poskytuje další nastavení toodefine při konfiguraci hello SKU na aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="49bcf-119">WAF provides additional settings toodefine when configuring hello SKU on an application gateway.</span></span> <span data-ttu-id="49bcf-120">Neexistují žádné další změny, které provedete v samotné bráně aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="49bcf-120">There are no additional changes that you make on hello application gateway itself.</span></span>

| <span data-ttu-id="49bcf-121">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="49bcf-121">**Setting**</span></span> | <span data-ttu-id="49bcf-122">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="49bcf-122">**Details**</span></span>
|---|---|
|<span data-ttu-id="49bcf-123">**SKU**</span><span class="sxs-lookup"><span data-stu-id="49bcf-123">**SKU**</span></span> |<span data-ttu-id="49bcf-124">Normální aplikační brána bez firewall webových aplikací podporuje **standardní\_malé**, **standardní\_střední**, a **standardní\_velké** velikosti.</span><span class="sxs-lookup"><span data-stu-id="49bcf-124">A normal application gateway without WAF supports **Standard\_Small**, **Standard\_Medium**, and **Standard\_Large** sizes.</span></span> <span data-ttu-id="49bcf-125">Se zavedením hello firewall webových aplikací, jsou dva další SKU, **firewall webových aplikací\_střední** a **firewall webových aplikací\_velké**.</span><span class="sxs-lookup"><span data-stu-id="49bcf-125">With hello introduction of WAF, there are two additional SKUs, **WAF\_Medium** and **WAF\_Large**.</span></span> <span data-ttu-id="49bcf-126">Malé aplikačních bran firewall webových aplikací nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="49bcf-126">WAF is not supported on small application gateways.</span></span>|
|<span data-ttu-id="49bcf-127">**Režim**</span><span class="sxs-lookup"><span data-stu-id="49bcf-127">**Mode**</span></span> | <span data-ttu-id="49bcf-128">Toto nastavení je režim hello firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="49bcf-128">This setting is hello mode of WAF.</span></span> <span data-ttu-id="49bcf-129">Povolené hodnoty jsou **detekce** a **prevence**.</span><span class="sxs-lookup"><span data-stu-id="49bcf-129">allowed values are **Detection** and **Prevention**.</span></span> <span data-ttu-id="49bcf-130">Pokud je v režimu zjišťování nastavení firewall webových aplikací, všechny hrozby jsou uloženy v souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="49bcf-130">When WAF is set up in detection mode, all threats are stored in a log file.</span></span> <span data-ttu-id="49bcf-131">V režimu prevence události jsou protokolovány ale hello útočník dostane 403 neoprávněným odpověď z hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="49bcf-131">In prevention mode, events are still logged but hello attacker receives a 403 unauthorized response from hello application gateway.</span></span>|

## <a name="add-web-application-firewall-tooan-existing-application-gateway"></a><span data-ttu-id="49bcf-132">Přidání webové aplikace brány firewall tooan existující aplikační brány</span><span class="sxs-lookup"><span data-stu-id="49bcf-132">Add web application firewall tooan existing application gateway</span></span>

<span data-ttu-id="49bcf-133">Hello použijte příkaz změny existující bránu aplikace s povolenou standardní aplikace brány tooa firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="49bcf-133">hello follow command changes an existing standard application gateway tooa WAF enabled application gateway.</span></span>

```azurecli-interactive
#!/bin/bash

az network application-gateway waf-config set \
  --enabled true \
  --firewall-mode Prevention \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"
```

<span data-ttu-id="49bcf-134">Tento příkaz aktualizuje hello aplikační bránu pomocí brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="49bcf-134">This command updates hello application gateway with web application firewall.</span></span> <span data-ttu-id="49bcf-135">Navštivte [Application Diagnostics brány](application-gateway-diagnostics.md) toounderstand jak tooview protokoly pro službu application gateway.</span><span class="sxs-lookup"><span data-stu-id="49bcf-135">Visit [Application Gateway Diagnostics](application-gateway-diagnostics.md) toounderstand how tooview logs for your application gateway.</span></span> <span data-ttu-id="49bcf-136">Z důvodu zabezpečení povaze toohello firewall webových aplikací zkontrolovat toobe třeba protokoly pravidelně toounderstand hello postavení zabezpečení webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="49bcf-136">Due toohello security nature of WAF, logs need toobe reviewed regularly toounderstand hello security posture of your web applications.</span></span>

## <a name="create-an-application-gateway-with-web-application-firewall"></a><span data-ttu-id="49bcf-137">Vytvoření služby Application Gateway pomocí brány firewall webových aplikací</span><span class="sxs-lookup"><span data-stu-id="49bcf-137">Create an Application Gateway with web application firewall</span></span>

<span data-ttu-id="49bcf-138">Hello následující příkaz vytvoří aplikační brány s brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="49bcf-138">hello following command creates an Application Gateway with web application firewall.</span></span>

```azurecli-interactive
#!/bin/bash

az network application-gateway create \
  --name "AdatumAppGateway2" \
  --location "eastus" \
  --resource-group "AdatumAppGatewayRG" \
  --vnet-name "AdatumAppGatewayVNET2" \
  --vnet-address-prefix "10.0.0.0/16" \
  --subnet "Appgatewaysubnet2" \
  --subnet-address-prefix "10.0.0.0/28" \
 --servers "10.0.0.5 10.0.0.4" \
  --capacity 2 
  --sku "WAF_Medium" \
  --http-settings-cookie-based-affinity "Enabled" \
  --http-settings-protocol "Http" \
  --frontend-port "80" \
  --routing-rule-type "Basic" \
  --http-settings-port "80" \
  --public-ip-address "pip2" \
  --public-ip-address-allocation "dynamic" \
  --tags "cli[2] owner[administrator]"
```

> [!NOTE]
> <span data-ttu-id="49bcf-139">Application Gateway, které jsou vytvořené pomocí konfigurace brány firewall hello základní webové aplikace jsou nakonfigurovány s řádku 3.0 pro ochranu.</span><span class="sxs-lookup"><span data-stu-id="49bcf-139">Application gateways created with hello basic web application firewall configuration are configured with CRS 3.0 for protections.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="49bcf-140">Získání názvu DNS služby Application Gateway</span><span class="sxs-lookup"><span data-stu-id="49bcf-140">Get application gateway DNS name</span></span>

<span data-ttu-id="49bcf-141">Po vytvoření brány hello hello dalším krokem je tooconfigure hello front-end pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="49bcf-141">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="49bcf-142">Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný.</span><span class="sxs-lookup"><span data-stu-id="49bcf-142">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="49bcf-143">tooensure koncoví uživatelé mohou dosáhl hello aplikační bránu, záznam CNAME lze použít toopoint toohello veřejný koncový bod hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="49bcf-143">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="49bcf-144">[Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="49bcf-144">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="49bcf-145">tooconfigure záznam CNAME načíst podrobnosti o hello aplikační brány a svému přidruženému názvu IP a DNS pomocí hello PublicIPAddress element připojené toohello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="49bcf-145">tooconfigure a CNAME record, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="49bcf-146">název DNS Hello Aplikační brána musí být použité toocreate záznam CNAME, které body hello dva webové aplikace toothis název DNS.</span><span class="sxs-lookup"><span data-stu-id="49bcf-146">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="49bcf-147">Hello použití záznamů A se nedoporučuje, protože hello VIP může změnit při restartu aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="49bcf-147">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

```azurecli-interactive
#!/bin/bash

az network public-ip show \
  --name pip2 \
  --resource-group "AdatumAppGatewayRG"
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

## <a name="next-steps"></a><span data-ttu-id="49bcf-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="49bcf-148">Next steps</span></span>

<span data-ttu-id="49bcf-149">Zjistěte, jak pravidla toocustomize firewall webových aplikací navštivte stránky: [přizpůsobit pravidla brány firewall webových aplikací prostřednictvím hello Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md).</span><span class="sxs-lookup"><span data-stu-id="49bcf-149">Learn how toocustomize WAF rules by visiting: [Customize web application firewall rules through hello Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md).</span></span>

[scenario]: ./media/application-gateway-web-application-firewall-cli/scenario.png
