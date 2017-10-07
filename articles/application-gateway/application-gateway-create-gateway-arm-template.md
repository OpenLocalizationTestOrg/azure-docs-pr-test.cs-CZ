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
# <a name="create-an-application-gateway-by-using-hello-azure-resource-manager-template"></a><span data-ttu-id="ac35a-103">Vytvoření služby application gateway pomocí šablony Azure Resource Manager hello</span><span class="sxs-lookup"><span data-stu-id="ac35a-103">Create an application gateway by using hello Azure Resource Manager template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ac35a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ac35a-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="ac35a-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac35a-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="ac35a-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac35a-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="ac35a-107">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="ac35a-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="ac35a-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ac35a-108">Azure CLI</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="ac35a-109">Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7.</span><span class="sxs-lookup"><span data-stu-id="ac35a-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="ac35a-110">Poskytuje převzetí služeb při selhání a směrování výkonu požadavků HTTP mezi různými servery, ať už jsou hello cloudu nebo místně.</span><span class="sxs-lookup"><span data-stu-id="ac35a-110">It provides failover and performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="ac35a-111">Application Gateway poskytuje mnoho funkcí kontroleru doručování aplikací (ADC), včetně vyrovnávání zatížení protokolu HTTP, spřažení relace na základě souborů cookie, přesměrování zpracování SSL (Secure Sockets Layer), vlastních sond stavu, podpory více webů a mnoha dalších.</span><span class="sxs-lookup"><span data-stu-id="ac35a-111">Application Gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, Secure Sockets Layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="ac35a-112">Navštivte toofind úplný seznam podporovaných funkcích [přehled Application Gateway](application-gateway-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="ac35a-112">toofind a complete list of supported features, visit [Application Gateway overview](application-gateway-introduction.md)</span></span>

<span data-ttu-id="ac35a-113">Tento článek vás provede stahování a úprava existující šablony Azure Resource Manageru z webu GitHub a nasazení hello šablony z Githubu, prostředí PowerShell a hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="ac35a-113">This article walks you through downloading and modifying an existing Azure Resource Manager template from GitHub and deploying hello template from GitHub, PowerShell, and hello Azure CLI.</span></span>

<span data-ttu-id="ac35a-114">Pokud jednoduše nasazujete šablony Azure Resource Manageru hello přímo z Githubu beze změn, přeskočte toodeploy šablony z Githubu.</span><span class="sxs-lookup"><span data-stu-id="ac35a-114">If you are simply deploying hello Azure Resource Manager template directly from GitHub without any changes, skip toodeploy a template from GitHub.</span></span>

## <a name="scenario"></a><span data-ttu-id="ac35a-115">Scénář</span><span class="sxs-lookup"><span data-stu-id="ac35a-115">Scenario</span></span>

<span data-ttu-id="ac35a-116">V tomto scénáři provedete tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="ac35a-116">In this scenario you will:</span></span>

* <span data-ttu-id="ac35a-117">Vytvoření služby application gateway pomocí brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="ac35a-117">Create an application gateway with web application firewall.</span></span>
* <span data-ttu-id="ac35a-118">Vytvoříte virtuální síť s názvem VirtualNetwork1 s vyhrazeným blokem CIDR 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="ac35a-118">Create a virtual network named VirtualNetwork1 with a reserved CIDR block of 10.0.0.0/16.</span></span>
* <span data-ttu-id="ac35a-119">Vytvoříte podsíť s názvem Appgatewaysubnet, která používá blok CIDR 10.0.0.0/28.</span><span class="sxs-lookup"><span data-stu-id="ac35a-119">Create a subnet called Appgatewaysubnet that uses 10.0.0.0/28 as its CIDR block.</span></span>
* <span data-ttu-id="ac35a-120">Nastavit dvě dříve nakonfigurované back-end IP adresy pro webové servery hello mají tooload vyrovnávání hello přenosů.</span><span class="sxs-lookup"><span data-stu-id="ac35a-120">Set up two previously configured back-end IPs for hello web servers you want tooload balance hello traffic.</span></span> <span data-ttu-id="ac35a-121">V tomto příkladu šablony hello back-end IP adresy jsou 10.0.1.10 a 10.0.1.11.</span><span class="sxs-lookup"><span data-stu-id="ac35a-121">In this template example, hello back-end IPs are 10.0.1.10 and 10.0.1.11.</span></span>

> [!NOTE]
> <span data-ttu-id="ac35a-122">Tato nastavení jsou hello parametry pro tuto šablonu.</span><span class="sxs-lookup"><span data-stu-id="ac35a-122">Those settings are hello parameters for this template.</span></span> <span data-ttu-id="ac35a-123">toocustomize hello šablony, můžete změnit pravidla, naslouchací proces hello, SSL a další možnosti v souboru azuredeploy.json hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-123">toocustomize hello template, you can change rules, hello listener, SSL, and other options in hello azuredeploy.json file.</span></span>

![Scénář](./media/application-gateway-create-gateway-arm-template/scenario.png)

## <a name="download-and-understand-hello-azure-resource-manager-template"></a><span data-ttu-id="ac35a-125">Stažení a pochopení šablony Azure Resource Manageru hello</span><span class="sxs-lookup"><span data-stu-id="ac35a-125">Download and understand hello Azure Resource Manager template</span></span>

<span data-ttu-id="ac35a-126">Můžete stáhnout z webu GitHub hello existující Azure Resource Manager šablony toocreate virtuální síť a dvě podsítě, proveďte požadované změny, může být vhodné a opakovaně ji používat.</span><span class="sxs-lookup"><span data-stu-id="ac35a-126">You can download hello existing Azure Resource Manager template toocreate a virtual network and two subnets from GitHub, make any changes you might want, and reuse it.</span></span> <span data-ttu-id="ac35a-127">toodo tedy použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ac35a-127">toodo so, use hello following steps:</span></span>

1. <span data-ttu-id="ac35a-128">Přejděte příliš[vytvoření aplikační brány s brány firewall webových aplikací povoleno](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).</span><span class="sxs-lookup"><span data-stu-id="ac35a-128">Navigate too[Create Application Gateway with web application firewall enabled](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).</span></span>
1. <span data-ttu-id="ac35a-129">Klikněte na **azuredeploy.json** a potom klikněte na **RAW**.</span><span class="sxs-lookup"><span data-stu-id="ac35a-129">Click **azuredeploy.json**, and then click **RAW**.</span></span>
1. <span data-ttu-id="ac35a-130">Uložte hello souboru tooa místní složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="ac35a-130">Save hello file tooa local folder on your computer.</span></span>
1. <span data-ttu-id="ac35a-131">Pokud jste obeznámeni s šablon Azure Resource Manageru, přeskočte toostep 7.</span><span class="sxs-lookup"><span data-stu-id="ac35a-131">If you are familiar with Azure Resource Manager templates, skip toostep 7.</span></span>
1. <span data-ttu-id="ac35a-132">Otevřete hello soubor, který jste uložili a prohlédněte si obsah hello v části **parametry** v řádku</span><span class="sxs-lookup"><span data-stu-id="ac35a-132">Open hello file that you saved and look at hello contents under **parameters** in line</span></span>
1. <span data-ttu-id="ac35a-133">Parametry šablony Azure Resource Manageru představují zástupce hodnot, které můžete doplnit během nasazování.</span><span class="sxs-lookup"><span data-stu-id="ac35a-133">Azure Resource Manager template parameters provide a placeholder for values that can be filled out during deployment.</span></span>

  | <span data-ttu-id="ac35a-134">Parametr</span><span class="sxs-lookup"><span data-stu-id="ac35a-134">Parameter</span></span> | <span data-ttu-id="ac35a-135">Popis</span><span class="sxs-lookup"><span data-stu-id="ac35a-135">Description</span></span> |
  | --- | --- |
  | <span data-ttu-id="ac35a-136">**subnetPrefix**</span><span class="sxs-lookup"><span data-stu-id="ac35a-136">**subnetPrefix**</span></span> |<span data-ttu-id="ac35a-137">Blok CIDR podsítě hello application gateway.</span><span class="sxs-lookup"><span data-stu-id="ac35a-137">CIDR block for hello application gateway subnet.</span></span> |
  | <span data-ttu-id="ac35a-138">**applicationGatewaySize**</span><span class="sxs-lookup"><span data-stu-id="ac35a-138">**applicationGatewaySize**</span></span> | <span data-ttu-id="ac35a-139">Velikost hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="ac35a-139">Size of hello application gateway.</span></span>  <span data-ttu-id="ac35a-140">Firewall webových aplikací umožňuje použití jenom střední a velké.</span><span class="sxs-lookup"><span data-stu-id="ac35a-140">WAF only allows medium and large.</span></span> |
  | <span data-ttu-id="ac35a-141">**backendIpaddress1**</span><span class="sxs-lookup"><span data-stu-id="ac35a-141">**backendIpaddress1**</span></span> |<span data-ttu-id="ac35a-142">IP adresa prvního webového serveru hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-142">IP address of hello first web server.</span></span> |
  | <span data-ttu-id="ac35a-143">**backendIpaddress2**</span><span class="sxs-lookup"><span data-stu-id="ac35a-143">**backendIpaddress2**</span></span> |<span data-ttu-id="ac35a-144">IP adresa druhého webového serveru hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-144">IP address of hello second web server.</span></span> |
  | <span data-ttu-id="ac35a-145">**wafEnabled**</span><span class="sxs-lookup"><span data-stu-id="ac35a-145">**wafEnabled**</span></span> | <span data-ttu-id="ac35a-146">Nastavení toodetermine, pokud je povolen firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="ac35a-146">Setting toodetermine if WAF is enabled.</span></span>|
  | <span data-ttu-id="ac35a-147">**wafMode**</span><span class="sxs-lookup"><span data-stu-id="ac35a-147">**wafMode**</span></span> | <span data-ttu-id="ac35a-148">Režim brány firewall webových aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-148">Mode of hello web application firewall.</span></span>  <span data-ttu-id="ac35a-149">Jsou k dispozici možnosti **prevence** nebo **detekce**.</span><span class="sxs-lookup"><span data-stu-id="ac35a-149">Available options are **prevention** or **detection**.</span></span>|
  | <span data-ttu-id="ac35a-150">**wafRuleSetType**</span><span class="sxs-lookup"><span data-stu-id="ac35a-150">**wafRuleSetType**</span></span> | <span data-ttu-id="ac35a-151">Sada pravidel pro typ firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="ac35a-151">Ruleset type for WAF.</span></span>  <span data-ttu-id="ac35a-152">Aktuálně OWASP je hello pouze podporované možnosti.</span><span class="sxs-lookup"><span data-stu-id="ac35a-152">Currently OWASP is hello only supported option.</span></span> |
  | <span data-ttu-id="ac35a-153">**wafRuleSetVersion**</span><span class="sxs-lookup"><span data-stu-id="ac35a-153">**wafRuleSetVersion**</span></span> |<span data-ttu-id="ac35a-154">Sada pravidel pro verzi.</span><span class="sxs-lookup"><span data-stu-id="ac35a-154">Ruleset version.</span></span> <span data-ttu-id="ac35a-155">OWASP řádku 2.2.9 a 3.0 jsou aktuálně hello podporované možnosti.</span><span class="sxs-lookup"><span data-stu-id="ac35a-155">OWASP CRS 2.2.9 and 3.0 are currently hello supported options.</span></span> |

1. <span data-ttu-id="ac35a-156">Zkontrolujte obsah hello v části **prostředky** a Všimněte si hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="ac35a-156">Check hello content under **resources** and notice hello following properties:</span></span>

   * <span data-ttu-id="ac35a-157">**type**.</span><span class="sxs-lookup"><span data-stu-id="ac35a-157">**type**.</span></span> <span data-ttu-id="ac35a-158">Typ prostředku vytvořeného šablonou hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-158">Type of resource being created by hello template.</span></span> <span data-ttu-id="ac35a-159">V takovém případě je typ hello `Microsoft.Network/applicationGateways`, který představuje službu application gateway.</span><span class="sxs-lookup"><span data-stu-id="ac35a-159">In this case, hello type is `Microsoft.Network/applicationGateways`, which represents an application gateway.</span></span>
   * <span data-ttu-id="ac35a-160">**name**.</span><span class="sxs-lookup"><span data-stu-id="ac35a-160">**name**.</span></span> <span data-ttu-id="ac35a-161">Název prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-161">Name for hello resource.</span></span> <span data-ttu-id="ac35a-162">Všimněte si použití hello `[parameters('applicationGatewayName')]`, což znamená, že název hello je poskytován jako vstup je nebo soubor parametru během nasazení.</span><span class="sxs-lookup"><span data-stu-id="ac35a-162">Notice hello use of `[parameters('applicationGatewayName')]`, which means that hello name is provided as input by you or by a parameter file during deployment.</span></span>
   * <span data-ttu-id="ac35a-163">**properties**.</span><span class="sxs-lookup"><span data-stu-id="ac35a-163">**properties**.</span></span> <span data-ttu-id="ac35a-164">Seznam vlastností pro prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-164">List of properties for hello resource.</span></span> <span data-ttu-id="ac35a-165">Tato šablona používá během vytváření služby application gateway hello virtuální síť a veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="ac35a-165">This template uses hello virtual network and public IP address during application gateway creation.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ac35a-166">Další informace o šablonách najdete v článku: [referenční informace k šablonám Resource Manager](/templates/)</span><span class="sxs-lookup"><span data-stu-id="ac35a-166">For more information on templates visit: [Resource Manager templates reference](/templates/)</span></span>

1. <span data-ttu-id="ac35a-167">Vraťte se zpět příliš[https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).</span><span class="sxs-lookup"><span data-stu-id="ac35a-167">Navigate back too[https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).</span></span>
1. <span data-ttu-id="ac35a-168">Klikněte na tlačítko **azuredeploy-Parameters.JSON tímto kódem**a potom klikněte na **RAW**.</span><span class="sxs-lookup"><span data-stu-id="ac35a-168">Click **azuredeploy-parameters.json**, and then click **RAW**.</span></span>
1. <span data-ttu-id="ac35a-169">Uložte hello souboru tooa místní složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="ac35a-169">Save hello file tooa local folder on your computer.</span></span>
1. <span data-ttu-id="ac35a-170">Otevřete soubor hello, který jste uložili a upravte hello hodnoty parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-170">Open hello file that you saved and edit hello values for hello parameters.</span></span> <span data-ttu-id="ac35a-171">Použijte následující hodnoty toodeploy hello application gateway popsané v tomto scénáři hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-171">Use hello following values toodeploy hello application gateway described in our scenario.</span></span>

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

1. <span data-ttu-id="ac35a-172">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-172">Save hello file.</span></span> <span data-ttu-id="ac35a-173">Hello šablonu JSON a šablonu parametrů můžete otestovat pomocí online ověřovacích nástrojů JSON jako [JSlint.com](http://www.jslint.com/).</span><span class="sxs-lookup"><span data-stu-id="ac35a-173">You can test hello JSON template and parameter template by using online JSON validation tools like [JSlint.com](http://www.jslint.com/).</span></span>

## <a name="deploy-hello-azure-resource-manager-template-by-using-powershell"></a><span data-ttu-id="ac35a-174">Nasazení šablony Azure Resource Manager hello pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac35a-174">Deploy hello Azure Resource Manager template by using PowerShell</span></span>

<span data-ttu-id="ac35a-175">Pokud jste prostředí Azure PowerShell nikdy nepoužívali, navštivte: [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) a postupujte podle pokynů toosign hello do Azure a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="ac35a-175">If you have never used Azure PowerShell, visit: [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) and follow hello instructions toosign into Azure and select your subscription.</span></span>

1. <span data-ttu-id="ac35a-176">TooPowerShell přihlášení</span><span class="sxs-lookup"><span data-stu-id="ac35a-176">Login tooPowerShell</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

1. <span data-ttu-id="ac35a-177">Zkontrolujte předplatná hello pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-177">Check hello subscriptions for hello account.</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

    <span data-ttu-id="ac35a-178">Jste výzvami tooauthenticate pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="ac35a-178">You are prompted tooauthenticate with your credentials.</span></span>

1. <span data-ttu-id="ac35a-179">Zvolte, které vaše toouse předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="ac35a-179">Choose which of your Azure subscriptions toouse.</span></span>

    ```powershell
    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
    ```

1. <span data-ttu-id="ac35a-180">V případě potřeby vytvořte skupinu prostředků s použitím hello **New-AzureResourceGroup** rutiny.</span><span class="sxs-lookup"><span data-stu-id="ac35a-180">If needed, create a resource group by using hello **New-AzureResourceGroup** cmdlet.</span></span> <span data-ttu-id="ac35a-181">V následujícím příkladu hello vytvoříte skupinu prostředků s názvem AppgatewayRG v umístění východní USA.</span><span class="sxs-lookup"><span data-stu-id="ac35a-181">In hello following example, you create a resource group called AppgatewayRG in East US location.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name AppgatewayRG -Location "West US"
    ```

1. <span data-ttu-id="ac35a-182">Spustit hello **New-AzureRmResourceGroupDeployment** rutiny toodeploy hello nové virtuální sítě pomocí šablony a parametr soubory stáhli a upravili v předchozích hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-182">Run hello **New-AzureRmResourceGroupDeployment** cmdlet toodeploy hello new virtual network by using hello preceding template and parameter files you downloaded and modified.</span></span>
    
    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

## <a name="deploy-hello-azure-resource-manager-template-by-using-hello-azure-cli"></a><span data-ttu-id="ac35a-183">Nasazení šablony Azure Resource Manager hello pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="ac35a-183">Deploy hello Azure Resource Manager template by using hello Azure CLI</span></span>

<span data-ttu-id="ac35a-184">šablony Azure Resource Manageru hello toodeploy, které jste stáhli pomocí rozhraní příkazového řádku Azure, postupujte podle hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ac35a-184">toodeploy hello Azure Resource Manager template you downloaded by using Azure CLI, follow hello following steps:</span></span>

1. <span data-ttu-id="ac35a-185">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](/cli/azure/install-azure-cli) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="ac35a-185">If you have never used Azure CLI, see [Install and configure hello Azure CLI](/cli/azure/install-azure-cli) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>

1. <span data-ttu-id="ac35a-186">Pokud je nezbytné, spusťte hello `az group create` příkaz toocreate skupinu prostředků, jak je znázorněno v následujícím fragmentu kódu hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-186">If necessary, run hello `az group create` command toocreate a resource group, as shown in hello following code snippet.</span></span> <span data-ttu-id="ac35a-187">Všimněte si výstup hello hello příkazu.</span><span class="sxs-lookup"><span data-stu-id="ac35a-187">Notice hello output of hello command.</span></span> <span data-ttu-id="ac35a-188">Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-188">hello list shown after hello output explains hello parameters used.</span></span> <span data-ttu-id="ac35a-189">Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ac35a-189">For more information about resource groups, visit [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

    ```azurecli
    az group create --location westus --name appgatewayRG
    ```
    
    <span data-ttu-id="ac35a-190">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="ac35a-190">**-n (or --name)**.</span></span> <span data-ttu-id="ac35a-191">Název pro novou skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-191">Name for hello new resource group.</span></span> <span data-ttu-id="ac35a-192">V našem scénáři je to název *appgatewayRG*.</span><span class="sxs-lookup"><span data-stu-id="ac35a-192">For our scenario, it's *appgatewayRG*.</span></span>
    
    <span data-ttu-id="ac35a-193">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="ac35a-193">**-l (or --location)**.</span></span> <span data-ttu-id="ac35a-194">Oblast Azure, kde se má vytvořit novou skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-194">Azure region where hello new resource group is created.</span></span> <span data-ttu-id="ac35a-195">Pro náš scénář má *westus*.</span><span class="sxs-lookup"><span data-stu-id="ac35a-195">For our scenario, it's *westus*.</span></span>

1. <span data-ttu-id="ac35a-196">Spustit hello `az group deployment create` rutiny toodeploy hello nové virtuální sítě pomocí šablony hello a parametr souborů, které jste stáhli a upravili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-196">Run hello `az group deployment create` cmdlet toodeploy hello new virtual network by using hello template and parameter files you downloaded and modified in hello preceding step.</span></span> <span data-ttu-id="ac35a-197">Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-197">hello list shown after hello output explains hello parameters used.</span></span>

    ```azurecli
    az group deployment create --resource-group appgatewayRG --name TestAppgatewayDeployment --template-file azuredeploy.json --parameters @azuredeploy-parameters.json
    ```

## <a name="deploy-hello-azure-resource-manager-template-by-using-click-to-deploy"></a><span data-ttu-id="ac35a-198">Nasazení šablony Azure Resource Manager hello pomocí, klikněte na tlačítko nasadit</span><span class="sxs-lookup"><span data-stu-id="ac35a-198">Deploy hello Azure Resource Manager template by using click-to-deploy</span></span>

<span data-ttu-id="ac35a-199">Klikněte na tlačítko nasazení je jiný způsob toouse šablon Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="ac35a-199">Click-to-deploy is another way toouse Azure Resource Manager templates.</span></span> <span data-ttu-id="ac35a-200">Jde snadný způsob toouse šablon s hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ac35a-200">It's an easy way toouse templates with hello Azure portal.</span></span>

1. <span data-ttu-id="ac35a-201">Přejděte příliš[vytvoření služby application gateway pomocí brány firewall webových aplikací](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).</span><span class="sxs-lookup"><span data-stu-id="ac35a-201">Go too[Create an application gateway with web application firewall](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).</span></span>

1. <span data-ttu-id="ac35a-202">Klikněte na tlačítko **nasazení tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="ac35a-202">Click **Deploy tooAzure**.</span></span>

    ![Nasazení tooAzure](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)
    
1. <span data-ttu-id="ac35a-204">Vyplňte hello parametry pro šablonu nasazení hello na portálu hello a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac35a-204">Fill out hello parameters for hello deployment template on hello portal and click **OK**.</span></span>

    ![Parametry](./media/application-gateway-create-gateway-arm-template/ibiza1.png)
    
1. <span data-ttu-id="ac35a-206">Vyberte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše** a klikněte na tlačítko **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="ac35a-206">Select **I agree toohello terms and conditions stated above** and click **Purchase**.</span></span>

1. <span data-ttu-id="ac35a-207">V okně Vlastní nasazení hello klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ac35a-207">On hello Custom deployment blade, click **Create**.</span></span>

## <a name="providing-certificate-data-tooresource-manager-templates"></a><span data-ttu-id="ac35a-208">Poskytnutí certifikátu data tooResource Manager šablony</span><span class="sxs-lookup"><span data-stu-id="ac35a-208">Providing certificate data tooResource Manager templates</span></span>

<span data-ttu-id="ac35a-209">Při použití protokolu SSL s využitím šablony certifikátu hello musí toobe řetězec base64 místo odesílání.</span><span class="sxs-lookup"><span data-stu-id="ac35a-209">When using SSL with a template, hello certificate needs toobe provided in a base64 string instead of being uploaded.</span></span> <span data-ttu-id="ac35a-210">řetězec tooconvert tooa base64 .pfx nebo .cer použijte jeden z hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="ac35a-210">tooconvert a .pfx or .cer tooa base64 string use one of hello following commands.</span></span> <span data-ttu-id="ac35a-211">Hello následující příkazy převést hello certifikát tooa base64 řetězce, který lze zadat toohello šablony.</span><span class="sxs-lookup"><span data-stu-id="ac35a-211">hello following commands convert hello certificate tooa base64 string, which can be provided toohello template.</span></span> <span data-ttu-id="ac35a-212">Hello očekává, že výstupem je řetězec, který může být uložené v proměnné a vložení v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="ac35a-212">hello expected output is a string that can be stored in a variable and pasted in hello template.</span></span>

### <a name="macos"></a><span data-ttu-id="ac35a-213">macOS</span><span class="sxs-lookup"><span data-stu-id="ac35a-213">macOS</span></span>
```bash
cert=$( base64 <certificate path and name>.pfx )
echo $cert
```

### <a name="windows"></a><span data-ttu-id="ac35a-214">Windows</span><span class="sxs-lookup"><span data-stu-id="ac35a-214">Windows</span></span>
```powershell
[System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("<certificate path and name>.pfx"))
```

## <a name="delete-all-resources"></a><span data-ttu-id="ac35a-215">Odstranění všech prostředků</span><span class="sxs-lookup"><span data-stu-id="ac35a-215">Delete all resources</span></span>

<span data-ttu-id="ac35a-216">toodelete vytvořit všechny prostředky v tomto článku, dokončení jedné z hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ac35a-216">toodelete all resources created in this article, complete one of hello following steps:</span></span>

### <a name="powershell"></a><span data-ttu-id="ac35a-217">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac35a-217">PowerShell</span></span>

```powershell
Remove-AzureRmResourceGroup -Name appgatewayRG
```

### <a name="azure-cli"></a><span data-ttu-id="ac35a-218">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ac35a-218">Azure CLI</span></span>

```azurecli
az group delete --name appgatewayRG
```

## <a name="next-steps"></a><span data-ttu-id="ac35a-219">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ac35a-219">Next steps</span></span>

<span data-ttu-id="ac35a-220">Pokud chcete, aby tooconfigure přesměrování zpracování SSL, navštivte: [konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="ac35a-220">If you want tooconfigure SSL offload, visit: [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="ac35a-221">Pokud chcete, aby tooconfigure toouse brány aplikací s nástrojem pro vyrovnávání zatížení pro vnitřní, navštivte: [vytvoření aplikační brány s nástrojem pro vyrovnávání interní zatížení (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="ac35a-221">If you want tooconfigure an application gateway toouse with an internal load balancer, visit: [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="ac35a-222">Pokud chcete získat další informace o obecných možnostech vyrovnávání zatížení, přečtěte si článek:</span><span class="sxs-lookup"><span data-stu-id="ac35a-222">If you want more information about load balancing options in general, visit:</span></span>

* [<span data-ttu-id="ac35a-223">Nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="ac35a-223">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="ac35a-224">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="ac35a-224">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

