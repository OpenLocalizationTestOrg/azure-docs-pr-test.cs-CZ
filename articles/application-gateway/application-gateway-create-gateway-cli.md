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
# <a name="create-an-application-gateway-by-using-hello-azure-cli-20"></a>Vytvoření služby application gateway pomocí Azure CLI 2.0 hello

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Classic PowerShell](application-gateway-create-gateway.md)
> * [Šablona Azure Resource Manageru](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI 1.0](application-gateway-create-gateway-cli.md)
> * [Azure CLI 2.0](application-gateway-create-gateway-cli.md)

Application Gateway je vyhrazené virtuální zařízení poskytující aplikace doručení řadiče (ADC) jako služba nabízí různé vrstvy 7 načíst vyrovnávání možnosti pro vaši aplikaci.

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku

Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

* [Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) -naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely.
* [Azure CLI 2.0](application-gateway-create-gateway-cli.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello

## <a name="prerequisite-install-hello-azure-cli-20"></a>Předpoklad: Instalace hello 2.0 rozhraní příkazového řádku Azure

tooperform hello kroky v tomto článku, budete potřebovat příliš[instalace hello rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

> [!NOTE]
> Pokud nemáte účet Azure, budete potřebovat. Zde si můžete zaregistrovat [bezplatnou zkušební verzi](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Scénář

V tomto scénáři zjistíte, jak hello toocreate brány aplikace pomocí portálu Azure.

Tento scénář se:

* Vytvoření střední application gateway se dvěma instancemi.
* Vytvořte virtuální síť s názvem AdatumAppGatewayVNET s vyhrazeným blokem CIDR 10.0.0.0/16.
* Vytvoříte podsíť s názvem Appgatewaysubnet, která používá blok CIDR 10.0.0.0/28.

> [!NOTE]
> Další konfigurace hello aplikační brány, včetně stavu vlastní testy, adresy fondu back-end a dalších pravidlech nakonfigurovány po hello aplikace brána je nakonfigurovaná a ne během počátečního nasazení.

## <a name="before-you-begin"></a>Než začnete

Služba Azure Application Gateway vyžaduje vlastní podsíti. Při vytváření virtuální sítě, ujistěte se, ponechte dostatek místa toohave adresu více podsítí. Jakmile nasadíte tooa podsítě application gateway, jedinými dodatečnými application Gateway se dá přidat toohello podsíť.

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Otevřete hello **Microsoft Azure příkazového řádku**a přihlaste se. 

```azurecli-interactive
az login -u "username"
```

> [!NOTE]
> Můžete také použít `az login` bez hello přepínač pro přihlášení zařízení, která vyžaduje zadání kódu v aka.ms/devicelogin.

Jakmile zadáte hello předchozím příkladu, je k dispozici kód. Přejděte toohttps://aka.ms/devicelogin v procesu přihlášení hello toocontinue prohlížeče.

![cmd zobrazující zařízení přihlášení][1]

V prohlížeči hello zadejte kód hello, kterou jste obdrželi. Jste přihlašovací stránku přesměrovaného tooa.

![Prohlížeč tooenter kódu][2]

Jakmile byl zadán kód hello jste přihlášeni, Zavřít hello prohlížeče toocontinue se scénářem hello.

![Úspěšné přihlášení][3]

## <a name="create-hello-resource-group"></a>Vytvořte skupinu prostředků hello

Před vytvořením hello aplikační bránu, se vytvoří skupinu prostředků toocontain hello aplikační brány. Následující Hello ukazuje příkaz hello.

```azurecli-interactive
az group create --name myresourcegroup --location "eastus"
```

## <a name="create-hello-application-gateway"></a>Vytvoření hello application gateway

Hello adres IP použitých pro back-end hello jsou hello IP adresy pro back-end serveru. Tyto hodnoty mohou být buď soukromé IP adresy ve virtuální síti hello, veřejné IP adresy nebo plně kvalifikované názvy domény pro back-end serverů. Hello následující příklad vytvoří aplikační brány s další konfiguraci nastavení pro nastavení http, porty a pravidla.

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

Hello ukazuje předchozí příklad mnoho vlastností, které nejsou nutné během hello vytvoření služby application gateway. Následující ukázka kódu Hello vytvoří aplikační brány s hello požadované informace.

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
> Seznam parametrů, které lze zadat během hello vytvoření spusťte následující příkaz: `az network application-gateway create --help`.

Tento příklad vytvoří základní aplikační brána s výchozím nastavením pro naslouchací proces hello, fond back-end, nastavení http back-end a pravidla. Můžete upravit tyto toosuit nastavení nasazení po úspěšné hello zřizování.
Pokud již máte webovou aplikaci definované s fondem back-end hello v předchozích krocích, po vytvoření, hello Vyrovnávání zatížení začne.

## <a name="get-application-gateway-dns-name"></a>Získání názvu DNS služby Application Gateway

Po vytvoření brány hello hello dalším krokem je tooconfigure hello front-end pro komunikaci. Při použití veřejné IP adresy služba Application Gateway vyžaduje dynamicky přidělený název DNS, který ale není popisný. tooensure koncoví uživatelé mohou dosáhl hello aplikační bránu, záznam CNAME lze použít toopoint toohello veřejný koncový bod hello aplikační brány. [Konfigurace vlastního názvu domény pro cloudovou službu Azure](../dns/dns-custom-domain.md). tooconfigure alias, načíst podrobnosti o hello aplikační brány a svému přidruženému názvu IP a DNS pomocí hello PublicIPAddress element připojené toohello aplikační brány. název DNS Hello Aplikační brána musí být použité toocreate záznam CNAME, které body hello dva webové aplikace toothis název DNS. Hello použití záznamů A se nedoporučuje, protože hello VIP může změnit při restartu aplikační brány.


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

toodelete vytvořit všechny prostředky v tomto článku, dokončení hello následující kroky:

```azurecli-interactive
az group delete --name AdatumAppGatewayRG
```
 
## <a name="next-steps"></a>Další kroky

Zjistěte, jak testy toocreate vlastní stavu navštivte stránky [vytvořit vlastní stav testu](application-gateway-create-probe-portal.md)

Zjistěte, jak tooconfigure snižování zátěže protokolu SSL a proveďte hello nákladná SSL dešifrování vypnout navštivte stránky webových serverů [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
