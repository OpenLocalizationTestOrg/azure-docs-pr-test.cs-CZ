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
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway-with-azure-cli"></a>Konfigurace brány firewall webových aplikací na nový nebo existující Application Gateway pomocí rozhraní příkazového řádku Azure

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Zjistěte, jak toocreate brány firewall webových aplikací povoleno aplikační bránu, nebo přidejte webové aplikace brány firewall tooan existující aplikační brány.

Hello brány firewall webových aplikací (firewall webových aplikací) v Azure Application Gateway chrání webových aplikací z běžných útoky založenými na web jako Injektáž SQL, útoky skriptování mezi weby a hijacks relace.

Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7. Poskytuje převzetí služeb při selhání, směrování výkonu požadavků HTTP mezi různými servery, ať už jsou hello cloudu nebo místně. Aplikační brána poskytuje funkce doručování řadiče (ADC) mnoho aplikací se včetně HTTP Vyrovnávání zatížení, na základě souboru cookie relace spřažení, vrstvu secure Sockets Layer (SSL) snižování zátěže, sondy vlastní stavu, podporu pro více lokalit a mnohé další. toofind úplný seznam podporovaných funkcích, navštivte: [Přehled služby Application Gateway](application-gateway-introduction.md).

Hello následující článek ukazuje, jak příliš[webové aplikace brány firewall tooan existující aplikace brány přidat](#add-web-application-firewall-to-an-existing-application-gateway) a [vytvoření služby application gateway, která používá brány firewall webových aplikací](#create-an-application-gateway-with-web-application-firewall).

![scénář image][scenario]

## <a name="prerequisite-install-hello-azure-cli-20"></a>Předpoklad: Instalace hello 2.0 rozhraní příkazového řádku Azure

tooperform hello kroky v tomto článku, budete potřebovat příliš[instalace hello rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="waf-configuration-differences"></a>Rozdíly konfigurace firewall webových aplikací

Pokud jste si přečetli [vytvoření služby Application Gateway pomocí Azure CLI](application-gateway-create-gateway-cli.md), rozumíte hello SKU nastavení tooconfigure při vytváření služby application gateway. Firewall webových aplikací poskytuje další nastavení toodefine při konfiguraci hello SKU na aplikační brány. Neexistují žádné další změny, které provedete v samotné bráně aplikace hello.

| **Nastavení** | **Podrobnosti**
|---|---|
|**SKU** |Normální aplikační brána bez firewall webových aplikací podporuje **standardní\_malé**, **standardní\_střední**, a **standardní\_velké** velikosti. Se zavedením hello firewall webových aplikací, jsou dva další SKU, **firewall webových aplikací\_střední** a **firewall webových aplikací\_velké**. Malé aplikačních bran firewall webových aplikací nepodporuje.|
|**Režim** | Toto nastavení je režim hello firewall webových aplikací. Povolené hodnoty jsou **detekce** a **prevence**. Pokud je v režimu zjišťování nastavení firewall webových aplikací, všechny hrozby jsou uloženy v souboru protokolu. V režimu prevence události jsou protokolovány ale hello útočník dostane 403 neoprávněným odpověď z hello aplikační brány.|

## <a name="add-web-application-firewall-tooan-existing-application-gateway"></a>Přidání webové aplikace brány firewall tooan existující aplikační brány

Hello použijte příkaz změny existující bránu aplikace s povolenou standardní aplikace brány tooa firewall webových aplikací.

```azurecli-interactive
#!/bin/bash

az network application-gateway waf-config set \
  --enabled true \
  --firewall-mode Prevention \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"
```

Tento příkaz aktualizuje hello aplikační bránu pomocí brány firewall webových aplikací. Navštivte [Application Diagnostics brány](application-gateway-diagnostics.md) toounderstand jak tooview protokoly pro službu application gateway. Z důvodu zabezpečení povaze toohello firewall webových aplikací zkontrolovat toobe třeba protokoly pravidelně toounderstand hello postavení zabezpečení webových aplikací.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Vytvoření služby Application Gateway pomocí brány firewall webových aplikací

Hello následující příkaz vytvoří aplikační brány s brány firewall webových aplikací.

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
> Application Gateway, které jsou vytvořené pomocí konfigurace brány firewall hello základní webové aplikace jsou nakonfigurovány s řádku 3.0 pro ochranu.

## <a name="get-application-gateway-dns-name"></a>Získání názvu DNS služby Application Gateway

Po vytvoření brány hello hello dalším krokem je tooconfigure hello front-end pro komunikaci. Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný. tooensure koncoví uživatelé mohou dosáhl hello aplikační bránu, záznam CNAME lze použít toopoint toohello veřejný koncový bod hello aplikační brány. [Konfigurace vlastního názvu domény pro cloudovou službu Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). tooconfigure záznam CNAME načíst podrobnosti o hello aplikační brány a svému přidruženému názvu IP a DNS pomocí hello PublicIPAddress element připojené toohello aplikační brány. název DNS Hello Aplikační brána musí být použité toocreate záznam CNAME, které body hello dva webové aplikace toothis název DNS. Hello použití záznamů A se nedoporučuje, protože hello VIP může změnit při restartu aplikační brány.

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

## <a name="next-steps"></a>Další kroky

Zjistěte, jak pravidla toocustomize firewall webových aplikací navštivte stránky: [přizpůsobit pravidla brány firewall webových aplikací prostřednictvím hello Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md).

[scenario]: ./media/application-gateway-web-application-firewall-cli/scenario.png
