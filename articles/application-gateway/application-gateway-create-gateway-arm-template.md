---
title: "Vytvoření služby Azure Application Gateway - šablony | Microsoft Docs"
description: "Tato stránka obsahuje pokyny pro vytvoření služby Azure Application Gateway pomocí šablony Azure Resource Manageru."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: davidmu
ms.openlocfilehash: 0aa16e9d7472d2d8c3c251e60a506a7f4223ac1d
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="create-an-application-gateway-by-using-the-azure-resource-manager-template"></a>Vytvoření služby Application Gateway pomocí šablony Azure Resource Manageru

> [!div class="op_single_selector"]
> * [portál Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Classic PowerShell](application-gateway-create-gateway.md)
> * [Šablona Azure Resource Manageru](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)

Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7. Poskytuje převzetí služeb při selhání a směrování výkonu požadavků HTTP mezi různými servery, ať už jsou místní nebo v cloudu. Application Gateway poskytuje mnoho funkcí kontroleru doručování aplikací (ADC), včetně vyrovnávání zatížení protokolu HTTP, spřažení relace na základě souborů cookie, přesměrování zpracování SSL (Secure Sockets Layer), vlastních sond stavu, podpory více webů a mnoha dalších. Úplný seznam podporovaných funkcích naleznete [přehled Application Gateway](application-gateway-introduction.md)

Tento článek vás provede stahování a úprava existující [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md) z Githubu a nasazení šablony z Githubu, prostředí PowerShell a rozhraní příkazového řádku Azure.

Pokud nasazujete jednoduše šabloně přímo z Githubu beze změn, přejděte k nasazení šablony z Githubu.

## <a name="scenario"></a>Scénář

V tomto scénáři provedete tyto kroky:

* Vytvoření služby application gateway pomocí brány firewall webových aplikací.
* Vytvoříte virtuální síť s názvem VirtualNetwork1 s vyhrazeným blokem CIDR 10.0.0.0/16.
* Vytvoříte podsíť s názvem Appgatewaysubnet, která používá blok CIDR 10.0.0.0/28.
* Nastavíte dvě dříve nakonfigurované back-end IP adresy pro webové servery, které chcete použít k vyrovnávání zatížení datových přenosů. V tomto příkladu šablony se jedná o back-end IP adresy 10.0.1.10 a 10.0.1.11.

> [!NOTE]
> Tato nastavení jsou parametry této šablony. Chcete-li přizpůsobit šablonu, můžete změnit pravidla, naslouchací proces, SSL a další možnosti v souboru azuredeploy.json.

![Scénář](./media/application-gateway-create-gateway-arm-template/scenario.png)

## <a name="download-and-understand-the-azure-resource-manager-template"></a>Stažení a pochopení šablony Azure Resource Manageru

Z webu GitHub si můžete stáhnout existující šablonu Azure Resource Manageru, která umožňuje vytvořit virtuální síť a dvě podsítě, provést v ní jakékoli změny a opakovaně ji používat. Chcete-li tak učinit, proveďte následující kroky:

1. Přejděte na [vytvoření aplikační brány s brány firewall webových aplikací povoleno](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).
1. Klikněte na **azuredeploy.json** a potom klikněte na **RAW**.
1. Uložte soubor do místní složky v počítači.
1. Pokud už šablony Azure Resource Manageru znáte, pokračujte krokem 7.
1. Otevřete soubor, který jste uložili a prohlédněte si obsah v části **parametry** v řádku
1. Parametry šablony Azure Resource Manageru představují zástupce hodnot, které můžete doplnit během nasazování.

  | Parametr | Popis |
  | --- | --- |
  | **subnetPrefix** |Blok CIDR podsítě služby application gateway. |
  | **applicationGatewaySize** | Velikost aplikační brány.  Firewall webových aplikací umožňuje použití jenom střední a velké. |
  | **backendIpaddress1** |IP adresa prvního webového serveru. |
  | **backendIpaddress2** |IP adresa druhého webového serveru. |
  | **wafEnabled** | Nastavení k určení, zda je povolený firewall webových aplikací.|
  | **wafMode** | Režim brány firewall webových aplikací.  Jsou k dispozici možnosti **prevence** nebo **detekce**.|
  | **wafRuleSetType** | Sada pravidel pro typ firewall webových aplikací.  Aktuálně OWASP je jedinou možností podporované. |
  | **wafRuleSetVersion** |Sada pravidel pro verzi. OWASP řádku 2.2.9 a 3.0 jsou aktuálně podporované možnosti. |

1. Prohlédněte si obsah v části **prostředky** a Všimněte si následujících vlastností:

   * **type**. Typ prostředku vytvořeného šablonou. V takovém případě je typ `Microsoft.Network/applicationGateways`, který představuje službu application gateway.
   * **name**. Název prostředku. Všimněte si použití `[parameters('applicationGatewayName')]`, což znamená, že název je poskytován jako vstup vámi nebo ze souboru parametrů během nasazování.
   * **properties**. Seznam vlastností prostředku. Tato šablona používá při vytváření služby Application Gateway virtuální síť a veřejnou IP adresu.

1. Přejděte zpět na [https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).
1. Klikněte na tlačítko **azuredeploy-Parameters.JSON tímto kódem**a potom klikněte na **RAW**.
1. Uložte soubor do místní složky v počítači.
1. Otevřete soubor, který jste uložili, a upravte hodnoty parametrů. K nasazení služby Application Gateway popsané v tomto scénáři použijte následující hodnoty.

    ```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "addressPrefix": {
            "value": "10.0.0.0/16"
            },
            "subnetPrefix": {
            "value": "10.0.0.0/28"
            },
            "applicationGatewaySize": {
            "value": "WAF_Medium"
            },
            "capacity": {
            "value": 2
            },
            "backendIpAddress1": {
            "value": "10.0.1.10"
            },
            "backendIpAddress2": {
            "value": "10.0.1.11"
            },
            "wafEnabled": {
            "value": true
            },
            "wafMode": {
            "value": "Detection"
            },
            "wafRuleSetType": {
            "value": "OWASP"
            },
            "wafRuleSetVersion": {
            "value": "3.0"
            }
        }
    }
    ```

1. Uložte soubor. Šablonu JSON a šablonu parametrů můžete otestovat pomocí online ověřovacích nástrojů JSON, jako je třeba [JSlint.com](http://www.jslint.com/).

## <a name="deploy-the-azure-resource-manager-template-by-using-powershell"></a>Nasazení šablony Azure Resource Manageru pomocí prostředí PowerShell

Pokud jste prostředí Azure PowerShell nikdy nepoužívali, navštivte: [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) a postupujte podle pokynů a přihlaste se k Azure a vybrat své předplatné.

1. Přihlášení k prostředí PowerShell

    ```powershell
    Login-AzureRmAccount
    ```

1. Zkontrolujte předplatná pro příslušný účet.

    ```powershell
    Get-AzureRmSubscription
    ```

    Zobrazí se výzva k ověření pomocí přihlašovacích údajů.

1. Zvolte předplatné Azure, které chcete použít.

    ```powershell
    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
    ```

1. Pokud je to potřeba, vytvořte pomocí rutiny **New-AzureResourceGroup** skupinu prostředků. V následujícím příkladu vytvoříte skupinu prostředků s názvem AppgatewayRG v umístění Východní USA.

    ```powershell
    New-AzureRmResourceGroup -Name AppgatewayRG -Location "West US"
    ```

1. Spuštěním rutiny **New-AzureRmResourceGroupDeployment** nasadíte novou virtuální síť pomocí šablony a souborů parametrů, které jste stáhli a upravili v krocích výše.
    
    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

## <a name="deploy-the-azure-resource-manager-template-by-using-the-azure-cli"></a>Nasazení šablony Azure Resource Manageru pomocí rozhraní příkazového řádku Azure

Pokud chcete nasadit šablonu Azure Resource Manager, kterou jste stáhli pomocí rozhraní příkazového řádku Azure, postupujte podle následujících kroků:

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.

1. V případě potřeby spustit `az group create` příkazu vytvořte skupinu prostředků, jak je znázorněno v následující fragment kódu. Prohlédněte si výstup příkazu. Seznam uvedený za výstupem vysvětluje použité parametry. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).

    ```azurecli
    az group create --location westus --name appgatewayRG
    ```
    
    **-n (nebo --name)**. Název nové skupiny prostředků. V našem scénáři je to název *appgatewayRG*.
    
    **-l (nebo --location)**. Oblast Azure, ve které je vytvořena nová skupina prostředků. Pro náš scénář má *westus*.

1. Spustit `az group deployment create` nasadíte novou virtuální síť pomocí šablony a parametr souborů, které jste stáhli a upravili v předchozím kroku. Seznam uvedený za výstupem vysvětluje použité parametry.

    ```azurecli
    az group deployment create --resource-group appgatewayRG --name TestAppgatewayDeployment --template-file azuredeploy.json --parameters @azuredeploy-parameters.json
    ```

## <a name="deploy-the-azure-resource-manager-template-by-using-click-to-deploy"></a>Nasazení šablony Azure Resource Manageru pomocí metody Click to Deploy

Metoda Click to Deploy je další způsob použití šablon Azure Resource Manageru. Je to snadný způsob, jak používat šablony na webu Azure Portal.

1. Přejděte na [vytvoření služby application gateway pomocí brány firewall webových aplikací](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).

1. Klikněte na **Deploy to Azure** (Nasadit do Azure).

    ![Nasazení do Azure](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)
    
1. Zadejte na portálu parametry šablony nasazení a klikněte na **OK**.

    ![Parametry](./media/application-gateway-create-gateway-arm-template/ibiza1.png)
    
1. Vyberte **souhlasím s podmínkami a ujednáními výše uvedených** a klikněte na tlačítko **nákupu**.

1. V okně Vlastní nasazení klikněte na **Vytvořit**.

## <a name="providing-certificate-data-to-resource-manager-templates"></a>Poskytuje data certifikátu k šablonám Resource Manager

Při použití protokolu SSL s využitím šablony certifikátu musí být zadané ve řetězec base64 místo odesílání. Převést .pfx nebo .cer na řetězec ve formátu base64 použijte jednu z následujících příkazů. Následující příkazy převést na řetězec ve formátu base64, které lze zadat do šablony certifikátu. Očekávaný výstup je řetězec, který může být uložené v proměnné a vložení v šabloně.

### <a name="macos"></a>macOS
```bash
cert=$( base64 <certificate path and name>.pfx )
echo $cert
```

### <a name="windows"></a>Windows
```powershell
[System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("<certificate path and name>.pfx"))
```

## <a name="delete-all-resources"></a>Odstranění všech prostředků

Chcete-li odstranit všechny prostředky, které jsou vytvořené v tomto článku, proveďte jednu z následujících kroků:

### <a name="powershell"></a>PowerShell

```powershell
Remove-AzureRmResourceGroup -Name appgatewayRG
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az group delete --name appgatewayRG
```

## <a name="next-steps"></a>Další kroky

Pokud chcete konfigurovat přesměrování zpracování SSL, přečtěte si článek [Konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).

Pokud chcete službu Application Gateway nakonfigurovat pro použití s interním nástrojem pro vyrovnávání zatížení, přečtěte si článek [Vytvoření aplikační brány s interním nástrojem pro vyrovnávání zatížení (ILB)](application-gateway-ilb.md).

Pokud chcete získat další informace o obecných možnostech vyrovnávání zatížení, přečtěte si článek:

* [Nástroj pro vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

