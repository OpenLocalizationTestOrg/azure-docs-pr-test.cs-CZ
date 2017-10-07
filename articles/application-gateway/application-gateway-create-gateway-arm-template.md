---
title: "aaaCreate služby Azure Application Gateway - šablony | Microsoft Docs"
description: "Tato stránka obsahuje pokyny toocreate služby Azure application gateway pomocí šablony Azure Resource Manager hello"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: fc18e553852551326d6a302abe2c7f8a08c2eb6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-by-using-hello-azure-resource-manager-template"></a>Vytvoření služby application gateway pomocí šablony Azure Resource Manager hello

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure Classic PowerShell](application-gateway-create-gateway.md)
> * [Šablona Azure Resource Manageru](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)

Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7. Poskytuje převzetí služeb při selhání a směrování výkonu požadavků HTTP mezi různými servery, ať už jsou hello cloudu nebo místně. Application Gateway poskytuje mnoho funkcí kontroleru doručování aplikací (ADC), včetně vyrovnávání zatížení protokolu HTTP, spřažení relace na základě souborů cookie, přesměrování zpracování SSL (Secure Sockets Layer), vlastních sond stavu, podpory více webů a mnoha dalších. Navštivte toofind úplný seznam podporovaných funkcích [přehled Application Gateway](application-gateway-introduction.md)

Tento článek vás provede stahování a úprava existující šablony Azure Resource Manageru z webu GitHub a nasazení hello šablony z Githubu, prostředí PowerShell a hello rozhraní příkazového řádku Azure.

Pokud jednoduše nasazujete šablony Azure Resource Manageru hello přímo z Githubu beze změn, přeskočte toodeploy šablony z Githubu.

## <a name="scenario"></a>Scénář

V tomto scénáři provedete tyto kroky:

* Vytvoření služby application gateway pomocí brány firewall webových aplikací.
* Vytvoříte virtuální síť s názvem VirtualNetwork1 s vyhrazeným blokem CIDR 10.0.0.0/16.
* Vytvoříte podsíť s názvem Appgatewaysubnet, která používá blok CIDR 10.0.0.0/28.
* Nastavit dvě dříve nakonfigurované back-end IP adresy pro webové servery hello mají tooload vyrovnávání hello přenosů. V tomto příkladu šablony hello back-end IP adresy jsou 10.0.1.10 a 10.0.1.11.

> [!NOTE]
> Tato nastavení jsou hello parametry pro tuto šablonu. toocustomize hello šablony, můžete změnit pravidla, naslouchací proces hello, SSL a další možnosti v souboru azuredeploy.json hello.

![Scénář](./media/application-gateway-create-gateway-arm-template/scenario.png)

## <a name="download-and-understand-hello-azure-resource-manager-template"></a>Stažení a pochopení šablony Azure Resource Manageru hello

Můžete stáhnout z webu GitHub hello existující Azure Resource Manager šablony toocreate virtuální síť a dvě podsítě, proveďte požadované změny, může být vhodné a opakovaně ji používat. toodo tedy použijte hello následující kroky:

1. Přejděte příliš[vytvoření aplikační brány s brány firewall webových aplikací povoleno](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).
1. Klikněte na **azuredeploy.json** a potom klikněte na **RAW**.
1. Uložte hello souboru tooa místní složky v počítači.
1. Pokud jste obeznámeni s šablon Azure Resource Manageru, přeskočte toostep 7.
1. Otevřete hello soubor, který jste uložili a prohlédněte si obsah hello v části **parametry** v řádku
1. Parametry šablony Azure Resource Manageru představují zástupce hodnot, které můžete doplnit během nasazování.

  | Parametr | Popis |
  | --- | --- |
  | **subnetPrefix** |Blok CIDR podsítě hello application gateway. |
  | **applicationGatewaySize** | Velikost hello aplikační brány.  Firewall webových aplikací umožňuje použití jenom střední a velké. |
  | **backendIpaddress1** |IP adresa prvního webového serveru hello. |
  | **backendIpaddress2** |IP adresa druhého webového serveru hello. |
  | **wafEnabled** | Nastavení toodetermine, pokud je povolen firewall webových aplikací.|
  | **wafMode** | Režim brány firewall webových aplikací hello.  Jsou k dispozici možnosti **prevence** nebo **detekce**.|
  | **wafRuleSetType** | Sada pravidel pro typ firewall webových aplikací.  Aktuálně OWASP je hello pouze podporované možnosti. |
  | **wafRuleSetVersion** |Sada pravidel pro verzi. OWASP řádku 2.2.9 a 3.0 jsou aktuálně hello podporované možnosti. |

1. Zkontrolujte obsah hello v části **prostředky** a Všimněte si hello následující vlastnosti:

   * **type**. Typ prostředku vytvořeného šablonou hello. V takovém případě je typ hello `Microsoft.Network/applicationGateways`, který představuje službu application gateway.
   * **name**. Název prostředku hello. Všimněte si použití hello `[parameters('applicationGatewayName')]`, což znamená, že název hello je poskytován jako vstup je nebo soubor parametru během nasazení.
   * **properties**. Seznam vlastností pro prostředek hello. Tato šablona používá během vytváření služby application gateway hello virtuální síť a veřejnou IP adresu.

   > [!NOTE]
   > Další informace o šablonách najdete v článku: [referenční informace k šablonám Resource Manager](/templates/)

1. Vraťte se zpět příliš[https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).
1. Klikněte na tlačítko **azuredeploy-Parameters.JSON tímto kódem**a potom klikněte na **RAW**.
1. Uložte hello souboru tooa místní složky v počítači.
1. Otevřete soubor hello, který jste uložili a upravte hello hodnoty parametrů hello. Použijte následující hodnoty toodeploy hello application gateway popsané v tomto scénáři hello.

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

1. Uložte soubor hello. Hello šablonu JSON a šablonu parametrů můžete otestovat pomocí online ověřovacích nástrojů JSON jako [JSlint.com](http://www.jslint.com/).

## <a name="deploy-hello-azure-resource-manager-template-by-using-powershell"></a>Nasazení šablony Azure Resource Manager hello pomocí prostředí PowerShell

Pokud jste prostředí Azure PowerShell nikdy nepoužívali, navštivte: [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) a postupujte podle pokynů toosign hello do Azure a vybrat své předplatné.

1. TooPowerShell přihlášení

    ```powershell
    Login-AzureRmAccount
    ```

1. Zkontrolujte předplatná hello pro účet hello.

    ```powershell
    Get-AzureRmSubscription
    ```

    Jste výzvami tooauthenticate pomocí svých přihlašovacích údajů.

1. Zvolte, které vaše toouse předplatných Azure.

    ```powershell
    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
    ```

1. V případě potřeby vytvořte skupinu prostředků s použitím hello **New-AzureResourceGroup** rutiny. V následujícím příkladu hello vytvoříte skupinu prostředků s názvem AppgatewayRG v umístění východní USA.

    ```powershell
    New-AzureRmResourceGroup -Name AppgatewayRG -Location "West US"
    ```

1. Spustit hello **New-AzureRmResourceGroupDeployment** rutiny toodeploy hello nové virtuální sítě pomocí šablony a parametr soubory stáhli a upravili v předchozích hello.
    
    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

## <a name="deploy-hello-azure-resource-manager-template-by-using-hello-azure-cli"></a>Nasazení šablony Azure Resource Manager hello pomocí hello rozhraní příkazového řádku Azure

šablony Azure Resource Manageru hello toodeploy, které jste stáhli pomocí rozhraní příkazového řádku Azure, postupujte podle hello následující kroky:

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](/cli/azure/install-azure-cli) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.

1. Pokud je nezbytné, spusťte hello `az group create` příkaz toocreate skupinu prostředků, jak je znázorněno v následujícím fragmentu kódu hello. Všimněte si výstup hello hello příkazu. Hello seznam uvedený za výstup hello vysvětluje použité parametry hello. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).

    ```azurecli
    az group create --location westus --name appgatewayRG
    ```
    
    **-n (nebo --name)**. Název pro novou skupinu prostředků hello. V našem scénáři je to název *appgatewayRG*.
    
    **-l (nebo --location)**. Oblast Azure, kde se má vytvořit novou skupinu prostředků hello. Pro náš scénář má *westus*.

1. Spustit hello `az group deployment create` rutiny toodeploy hello nové virtuální sítě pomocí šablony hello a parametr souborů, které jste stáhli a upravili v předchozím kroku hello. Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.

    ```azurecli
    az group deployment create --resource-group appgatewayRG --name TestAppgatewayDeployment --template-file azuredeploy.json --parameters @azuredeploy-parameters.json
    ```

## <a name="deploy-hello-azure-resource-manager-template-by-using-click-to-deploy"></a>Nasazení šablony Azure Resource Manager hello pomocí, klikněte na tlačítko nasadit

Klikněte na tlačítko nasazení je jiný způsob toouse šablon Azure Resource Manageru. Jde snadný způsob toouse šablon s hello portálu Azure.

1. Přejděte příliš[vytvoření služby application gateway pomocí brány firewall webových aplikací](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).

1. Klikněte na tlačítko **nasazení tooAzure**.

    ![Nasazení tooAzure](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)
    
1. Vyplňte hello parametry pro šablonu nasazení hello na portálu hello a klikněte na tlačítko **OK**.

    ![Parametry](./media/application-gateway-create-gateway-arm-template/ibiza1.png)
    
1. Vyberte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše** a klikněte na tlačítko **nákupu**.

1. V okně Vlastní nasazení hello klikněte na tlačítko **vytvořit**.

## <a name="providing-certificate-data-tooresource-manager-templates"></a>Poskytnutí certifikátu data tooResource Manager šablony

Při použití protokolu SSL s využitím šablony certifikátu hello musí toobe řetězec base64 místo odesílání. řetězec tooconvert tooa base64 .pfx nebo .cer použijte jeden z hello následující příkazy. Hello následující příkazy převést hello certifikát tooa base64 řetězce, který lze zadat toohello šablony. Hello očekává, že výstupem je řetězec, který může být uložené v proměnné a vložení v šabloně hello.

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

toodelete vytvořit všechny prostředky v tomto článku, dokončení jedné z hello následující kroky:

### <a name="powershell"></a>PowerShell

```powershell
Remove-AzureRmResourceGroup -Name appgatewayRG
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az group delete --name appgatewayRG
```

## <a name="next-steps"></a>Další kroky

Pokud chcete, aby tooconfigure přesměrování zpracování SSL, navštivte: [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).

Pokud chcete, aby tooconfigure toouse brány aplikací s nástrojem pro vyrovnávání zatížení pro vnitřní, navštivte: [vytvoření aplikační brány s nástrojem pro vyrovnávání interní zatížení (ILB)](application-gateway-ilb.md).

Pokud chcete získat další informace o obecných možnostech vyrovnávání zatížení, přečtěte si článek:

* [Nástroj pro vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

