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
# <a name="create-an-application-gateway-by-using-the-azure-cli-20"></a>Vytvoření služby application gateway pomocí Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Classic PowerShell](application-gateway-create-gateway.md)
> * [Šablona Azure Resource Manageru](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI 1.0](application-gateway-create-gateway-cli.md)
> * [Azure CLI 2.0](application-gateway-create-gateway-cli.md)

Application Gateway je vyhrazené virtuální zařízení poskytující aplikace doručení řadiče (ADC) jako služba nabízí různé vrstvy 7 načíst vyrovnávání možnosti pro vaši aplikaci.

## <a name="cli-versions-to-complete-the-task"></a>Verze rozhraní příkazového řádku pro dokončení úlohy

K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:

* [Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků
* [Azure CLI 2.0](application-gateway-create-gateway-cli.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků

## <a name="prerequisite-install-the-azure-cli-20"></a>Předpoklad: Instalace rozhraní příkazového řádku Azure 2.0

Chcete-li provést kroky v tomto článku, je potřeba [nainstalovat rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

> [!NOTE]
> Pokud nemáte účet Azure, budete potřebovat. Zde si můžete zaregistrovat [bezplatnou zkušební verzi](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Scénář

V tomto scénáři zjistíte postup vytvoření služby application gateway pomocí portálu Azure.

Tento scénář se:

* Vytvoření střední application gateway se dvěma instancemi.
* Vytvořte virtuální síť s názvem AdatumAppGatewayVNET s vyhrazeným blokem CIDR 10.0.0.0/16.
* Vytvoříte podsíť s názvem Appgatewaysubnet, která používá blok CIDR 10.0.0.0/28.

> [!NOTE]
> Další konfigurace aplikační brány, včetně stavu vlastní testy, adresy fondu back-end a dalších pravidlech nakonfigurovány po dokončení konfigurace aplikační brány a ne během počátečního nasazení.

## <a name="before-you-begin"></a>Než začnete

Služba Azure Application Gateway vyžaduje vlastní podsíti. Při vytváření virtuální sítě, ujistěte se, že necháte dostatek adresní prostor tak, aby měl více podsítí. Po nasazení služby application gateway k podsíti, lze přidat pouze další aplikačních bran do podsítě.

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Otevřete **Microsoft Azure příkazového řádku**a přihlaste se. 

```azurecli-interactive
az login -u "username"
```

> [!NOTE]
> Můžete také použít `az login` bez přepínač pro přihlášení zařízení, která vyžaduje zadání kódu v aka.ms/devicelogin.

Jakmile zadáte v předchozím příkladu, je k dispozici kód. Přejděte do https://aka.ms/devicelogin ve prohlížeči a pokračujte v procesu přihlášení.

![cmd zobrazující zařízení přihlášení][1]

V prohlížeči zadejte kód, který jste dostali. Budete přesměrováni na stránku přihlášení.

![prohlížeče k zadání kódu][2]

Jakmile byl zadán kód jste přihlášeni, zavřete prohlížeč pokračovat na tento scénář.

![Úspěšné přihlášení][3]

## <a name="create-the-resource-group"></a>Vytvoření skupiny prostředků

Před vytvořením služby application gateway, se tak, aby obsahovala služby application gateway vytvoří skupinu prostředků. Následuje ukázka příkazu.

```azurecli-interactive
az group create --name myresourcegroup --location "eastus"
```

## <a name="create-the-application-gateway"></a>Vytvoření služby Application Gateway

Použít pro back-end IP adresy jsou IP adresy pro back-end serveru. Tyto hodnoty může být buď soukromé IP adresy ve virtuální síti, veřejné IP adresy nebo plně kvalifikované názvy domény pro back-end serverů. Následující příklad vytvoří aplikační brány s další konfiguraci nastavení pro nastavení http, porty a pravidla.

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

Předchozí příklad ukazuje mnoho vlastností, které nejsou nutné během vytváření služby application gateway. Následující příklad kódu vytvoří aplikační brány s požadované informace.

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
> Seznam parametrů, které lze zadat během vytváření, spusťte následující příkaz: `az network application-gateway create --help`.

Tento příklad vytvoří základní aplikační brána s výchozím nastavením pro naslouchací proces, fond back-end, nastavení http back-end a pravidla. Můžete upravit toto nastavení tak, aby vyhovovala vašemu nasazení po úspěšné přidělení přístupových práv.
Pokud již máte webové aplikace definovaná pomocí fondu back-end v předchozích krocích po vytvoření, Vyrovnávání zatížení začne.

## <a name="get-application-gateway-dns-name"></a>Získání názvu DNS služby Application Gateway

Po vytvoření brány je dalším krokem konfigurace front-endu pro komunikaci. Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný. Pokud chcete zajistit, aby se koncoví uživatelé mohli dostat ke službě Application Gateway, můžete použít záznam CNAME jako odkaz na veřejný koncový bod služby Application Gateway. [Konfigurace vlastního názvu domény pro cloudovou službu Azure](../dns/dns-custom-domain.md). Pokud chcete konfigurovat alias, načíst podrobnosti o aplikační bránu a svému přidruženému názvu IP a DNS pomocí elementu PublicIPAddress připojit k službě application gateway. Název DNS služby Application Gateway byste měli použít k vytvoření záznamu CNAME, který tyto dvě webové aplikace odkazuje na tento název DNS. Použití záznamů A se nedoporučuje z toho důvodu, že virtuální IP adresa se může změnit při restartování služby Application Gateway.


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

## <a name="delete-all-resources"></a>Odstranění všech prostředků

Pokud chcete odstranit všechny prostředky vytvořené v rámci tohoto článku, proveďte následující kroky:

```azurecli-interactive
az group delete --name AdatumAppGatewayRG
```
 
## <a name="next-steps"></a>Další kroky

Naučte se vytvářet vlastní stavu sondy navštivte stránky [vytvořit vlastní stav testu](application-gateway-create-probe-portal.md)

Zjistěte, jak nakonfigurovat snižování zátěže protokolu SSL a trvat nákladná dešifrování SSL vypnout webových serverů, navštivte stránky [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
