---
title: "Vytvoření služby Azure Application Gateway - šablony | Microsoft Docs"
description: "Tato stránka obsahuje pokyny pro vytvoření služby Azure Application Gateway pomocí šablony Azure Resource Manageru."
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
ms.openlocfilehash: 46cca89ccb5bd77d57fabc3e9027fcebd38da8e7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-application-gateway-by-using-the-azure-resource-manager-template"></a><span data-ttu-id="2916a-103">Vytvoření služby Application Gateway pomocí šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="2916a-103">Create an application gateway by using the Azure Resource Manager template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2916a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2916a-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="2916a-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="2916a-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="2916a-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="2916a-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="2916a-107">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="2916a-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="2916a-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2916a-108">Azure CLI</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="2916a-109">Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7.</span><span class="sxs-lookup"><span data-stu-id="2916a-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="2916a-110">Poskytuje převzetí služeb při selhání a směrování výkonu požadavků HTTP mezi různými servery, ať už jsou místní nebo v cloudu.</span><span class="sxs-lookup"><span data-stu-id="2916a-110">It provides failover and performance-routing HTTP requests between different servers, whether they are on the cloud or on-premises.</span></span> <span data-ttu-id="2916a-111">Application Gateway poskytuje mnoho funkcí kontroleru doručování aplikací (ADC), včetně vyrovnávání zatížení protokolu HTTP, spřažení relace na základě souborů cookie, přesměrování zpracování SSL (Secure Sockets Layer), vlastních sond stavu, podpory více webů a mnoha dalších.</span><span class="sxs-lookup"><span data-stu-id="2916a-111">Application Gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, Secure Sockets Layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="2916a-112">Úplný seznam podporovaných funkcích naleznete [přehled Application Gateway](application-gateway-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="2916a-112">To find a complete list of supported features, visit [Application Gateway overview](application-gateway-introduction.md)</span></span>

<span data-ttu-id="2916a-113">Tento článek vás provede stahování a úprava existující šablony Azure Resource Manageru z webu GitHub a nasazení šablony z Githubu, prostředí PowerShell a rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="2916a-113">This article walks you through downloading and modifying an existing Azure Resource Manager template from GitHub and deploying the template from GitHub, PowerShell, and the Azure CLI.</span></span>

<span data-ttu-id="2916a-114">Pokud šablonu Azure Resource Manageru jednoduše nasazujete přímo z GitHubu beze změn, přejděte k části o nasazení šablony z GitHubu.</span><span class="sxs-lookup"><span data-stu-id="2916a-114">If you are simply deploying the Azure Resource Manager template directly from GitHub without any changes, skip to deploy a template from GitHub.</span></span>

## <a name="scenario"></a><span data-ttu-id="2916a-115">Scénář</span><span class="sxs-lookup"><span data-stu-id="2916a-115">Scenario</span></span>

<span data-ttu-id="2916a-116">V tomto scénáři provedete tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="2916a-116">In this scenario you will:</span></span>

* <span data-ttu-id="2916a-117">Vytvoření služby application gateway pomocí brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="2916a-117">Create an application gateway with web application firewall.</span></span>
* <span data-ttu-id="2916a-118">Vytvoříte virtuální síť s názvem VirtualNetwork1 s vyhrazeným blokem CIDR 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="2916a-118">Create a virtual network named VirtualNetwork1 with a reserved CIDR block of 10.0.0.0/16.</span></span>
* <span data-ttu-id="2916a-119">Vytvoříte podsíť s názvem Appgatewaysubnet, která používá blok CIDR 10.0.0.0/28.</span><span class="sxs-lookup"><span data-stu-id="2916a-119">Create a subnet called Appgatewaysubnet that uses 10.0.0.0/28 as its CIDR block.</span></span>
* <span data-ttu-id="2916a-120">Nastavíte dvě dříve nakonfigurované back-end IP adresy pro webové servery, které chcete použít k vyrovnávání zatížení datových přenosů.</span><span class="sxs-lookup"><span data-stu-id="2916a-120">Set up two previously configured back-end IPs for the web servers you want to load balance the traffic.</span></span> <span data-ttu-id="2916a-121">V tomto příkladu šablony se jedná o back-end IP adresy 10.0.1.10 a 10.0.1.11.</span><span class="sxs-lookup"><span data-stu-id="2916a-121">In this template example, the back-end IPs are 10.0.1.10 and 10.0.1.11.</span></span>

> [!NOTE]
> <span data-ttu-id="2916a-122">Tato nastavení jsou parametry této šablony.</span><span class="sxs-lookup"><span data-stu-id="2916a-122">Those settings are the parameters for this template.</span></span> <span data-ttu-id="2916a-123">Chcete-li přizpůsobit šablonu, můžete změnit pravidla, naslouchací proces, SSL a další možnosti v souboru azuredeploy.json.</span><span class="sxs-lookup"><span data-stu-id="2916a-123">To customize the template, you can change rules, the listener, SSL, and other options in the azuredeploy.json file.</span></span>

![Scénář](./media/application-gateway-create-gateway-arm-template/scenario.png)

## <a name="download-and-understand-the-azure-resource-manager-template"></a><span data-ttu-id="2916a-125">Stažení a pochopení šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="2916a-125">Download and understand the Azure Resource Manager template</span></span>

<span data-ttu-id="2916a-126">Z webu GitHub si můžete stáhnout existující šablonu Azure Resource Manageru, která umožňuje vytvořit virtuální síť a dvě podsítě, provést v ní jakékoli změny a opakovaně ji používat.</span><span class="sxs-lookup"><span data-stu-id="2916a-126">You can download the existing Azure Resource Manager template to create a virtual network and two subnets from GitHub, make any changes you might want, and reuse it.</span></span> <span data-ttu-id="2916a-127">Chcete-li tak učinit, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2916a-127">To do so, use the following steps:</span></span>

1. <span data-ttu-id="2916a-128">Přejděte na [vytvoření aplikační brány s brány firewall webových aplikací povoleno](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).</span><span class="sxs-lookup"><span data-stu-id="2916a-128">Navigate to [Create Application Gateway with web application firewall enabled](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).</span></span>
1. <span data-ttu-id="2916a-129">Klikněte na **azuredeploy.json** a potom klikněte na **RAW**.</span><span class="sxs-lookup"><span data-stu-id="2916a-129">Click **azuredeploy.json**, and then click **RAW**.</span></span>
1. <span data-ttu-id="2916a-130">Uložte soubor do místní složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="2916a-130">Save the file to a local folder on your computer.</span></span>
1. <span data-ttu-id="2916a-131">Pokud už šablony Azure Resource Manageru znáte, pokračujte krokem 7.</span><span class="sxs-lookup"><span data-stu-id="2916a-131">If you are familiar with Azure Resource Manager templates, skip to step 7.</span></span>
1. <span data-ttu-id="2916a-132">Otevřete soubor, který jste uložili a prohlédněte si obsah v části **parametry** v řádku</span><span class="sxs-lookup"><span data-stu-id="2916a-132">Open the file that you saved and look at the contents under **parameters** in line</span></span>
1. <span data-ttu-id="2916a-133">Parametry šablony Azure Resource Manageru představují zástupce hodnot, které můžete doplnit během nasazování.</span><span class="sxs-lookup"><span data-stu-id="2916a-133">Azure Resource Manager template parameters provide a placeholder for values that can be filled out during deployment.</span></span>

  | <span data-ttu-id="2916a-134">Parametr</span><span class="sxs-lookup"><span data-stu-id="2916a-134">Parameter</span></span> | <span data-ttu-id="2916a-135">Popis</span><span class="sxs-lookup"><span data-stu-id="2916a-135">Description</span></span> |
  | --- | --- |
  | <span data-ttu-id="2916a-136">**subnetPrefix**</span><span class="sxs-lookup"><span data-stu-id="2916a-136">**subnetPrefix**</span></span> |<span data-ttu-id="2916a-137">Blok CIDR podsítě služby application gateway.</span><span class="sxs-lookup"><span data-stu-id="2916a-137">CIDR block for the application gateway subnet.</span></span> |
  | <span data-ttu-id="2916a-138">**applicationGatewaySize**</span><span class="sxs-lookup"><span data-stu-id="2916a-138">**applicationGatewaySize**</span></span> | <span data-ttu-id="2916a-139">Velikost aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="2916a-139">Size of the application gateway.</span></span>  <span data-ttu-id="2916a-140">Firewall webových aplikací umožňuje použití jenom střední a velké.</span><span class="sxs-lookup"><span data-stu-id="2916a-140">WAF only allows medium and large.</span></span> |
  | <span data-ttu-id="2916a-141">**backendIpaddress1**</span><span class="sxs-lookup"><span data-stu-id="2916a-141">**backendIpaddress1**</span></span> |<span data-ttu-id="2916a-142">IP adresa prvního webového serveru.</span><span class="sxs-lookup"><span data-stu-id="2916a-142">IP address of the first web server.</span></span> |
  | <span data-ttu-id="2916a-143">**backendIpaddress2**</span><span class="sxs-lookup"><span data-stu-id="2916a-143">**backendIpaddress2**</span></span> |<span data-ttu-id="2916a-144">IP adresa druhého webového serveru.</span><span class="sxs-lookup"><span data-stu-id="2916a-144">IP address of the second web server.</span></span> |
  | <span data-ttu-id="2916a-145">**wafEnabled**</span><span class="sxs-lookup"><span data-stu-id="2916a-145">**wafEnabled**</span></span> | <span data-ttu-id="2916a-146">Nastavení k určení, zda je povolený firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="2916a-146">Setting to determine if WAF is enabled.</span></span>|
  | <span data-ttu-id="2916a-147">**wafMode**</span><span class="sxs-lookup"><span data-stu-id="2916a-147">**wafMode**</span></span> | <span data-ttu-id="2916a-148">Režim brány firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="2916a-148">Mode of the web application firewall.</span></span>  <span data-ttu-id="2916a-149">Jsou k dispozici možnosti **prevence** nebo **detekce**.</span><span class="sxs-lookup"><span data-stu-id="2916a-149">Available options are **prevention** or **detection**.</span></span>|
  | <span data-ttu-id="2916a-150">**wafRuleSetType**</span><span class="sxs-lookup"><span data-stu-id="2916a-150">**wafRuleSetType**</span></span> | <span data-ttu-id="2916a-151">Sada pravidel pro typ firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="2916a-151">Ruleset type for WAF.</span></span>  <span data-ttu-id="2916a-152">Aktuálně OWASP je jedinou možností podporované.</span><span class="sxs-lookup"><span data-stu-id="2916a-152">Currently OWASP is the only supported option.</span></span> |
  | <span data-ttu-id="2916a-153">**wafRuleSetVersion**</span><span class="sxs-lookup"><span data-stu-id="2916a-153">**wafRuleSetVersion**</span></span> |<span data-ttu-id="2916a-154">Sada pravidel pro verzi.</span><span class="sxs-lookup"><span data-stu-id="2916a-154">Ruleset version.</span></span> <span data-ttu-id="2916a-155">OWASP řádku 2.2.9 a 3.0 jsou aktuálně podporované možnosti.</span><span class="sxs-lookup"><span data-stu-id="2916a-155">OWASP CRS 2.2.9 and 3.0 are currently the supported options.</span></span> |

1. <span data-ttu-id="2916a-156">Prohlédněte si obsah v části **prostředky** a Všimněte si následujících vlastností:</span><span class="sxs-lookup"><span data-stu-id="2916a-156">Check the content under **resources** and notice the following properties:</span></span>

   * <span data-ttu-id="2916a-157">**type**.</span><span class="sxs-lookup"><span data-stu-id="2916a-157">**type**.</span></span> <span data-ttu-id="2916a-158">Typ prostředku vytvořeného šablonou.</span><span class="sxs-lookup"><span data-stu-id="2916a-158">Type of resource being created by the template.</span></span> <span data-ttu-id="2916a-159">V takovém případě je typ `Microsoft.Network/applicationGateways`, který představuje službu application gateway.</span><span class="sxs-lookup"><span data-stu-id="2916a-159">In this case, the type is `Microsoft.Network/applicationGateways`, which represents an application gateway.</span></span>
   * <span data-ttu-id="2916a-160">**name**.</span><span class="sxs-lookup"><span data-stu-id="2916a-160">**name**.</span></span> <span data-ttu-id="2916a-161">Název prostředku.</span><span class="sxs-lookup"><span data-stu-id="2916a-161">Name for the resource.</span></span> <span data-ttu-id="2916a-162">Všimněte si použití `[parameters('applicationGatewayName')]`, což znamená, že název je poskytován jako vstup vámi nebo ze souboru parametrů během nasazování.</span><span class="sxs-lookup"><span data-stu-id="2916a-162">Notice the use of `[parameters('applicationGatewayName')]`, which means that the name is provided as input by you or by a parameter file during deployment.</span></span>
   * <span data-ttu-id="2916a-163">**properties**.</span><span class="sxs-lookup"><span data-stu-id="2916a-163">**properties**.</span></span> <span data-ttu-id="2916a-164">Seznam vlastností prostředku.</span><span class="sxs-lookup"><span data-stu-id="2916a-164">List of properties for the resource.</span></span> <span data-ttu-id="2916a-165">Tato šablona používá při vytváření služby Application Gateway virtuální síť a veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="2916a-165">This template uses the virtual network and public IP address during application gateway creation.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2916a-166">Další informace o šablonách najdete v článku: [referenční informace k šablonám Resource Manager](/templates/)</span><span class="sxs-lookup"><span data-stu-id="2916a-166">For more information on templates visit: [Resource Manager templates reference](/templates/)</span></span>

1. <span data-ttu-id="2916a-167">Přejděte zpět na [https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).</span><span class="sxs-lookup"><span data-stu-id="2916a-167">Navigate back to [https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).</span></span>
1. <span data-ttu-id="2916a-168">Klikněte na tlačítko **azuredeploy-Parameters.JSON tímto kódem**a potom klikněte na **RAW**.</span><span class="sxs-lookup"><span data-stu-id="2916a-168">Click **azuredeploy-parameters.json**, and then click **RAW**.</span></span>
1. <span data-ttu-id="2916a-169">Uložte soubor do místní složky v počítači.</span><span class="sxs-lookup"><span data-stu-id="2916a-169">Save the file to a local folder on your computer.</span></span>
1. <span data-ttu-id="2916a-170">Otevřete soubor, který jste uložili, a upravte hodnoty parametrů.</span><span class="sxs-lookup"><span data-stu-id="2916a-170">Open the file that you saved and edit the values for the parameters.</span></span> <span data-ttu-id="2916a-171">K nasazení služby Application Gateway popsané v tomto scénáři použijte následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2916a-171">Use the following values to deploy the application gateway described in our scenario.</span></span>

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

1. <span data-ttu-id="2916a-172">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="2916a-172">Save the file.</span></span> <span data-ttu-id="2916a-173">Šablonu JSON a šablonu parametrů můžete otestovat pomocí online ověřovacích nástrojů JSON, jako je třeba [JSlint.com](http://www.jslint.com/).</span><span class="sxs-lookup"><span data-stu-id="2916a-173">You can test the JSON template and parameter template by using online JSON validation tools like [JSlint.com](http://www.jslint.com/).</span></span>

## <a name="deploy-the-azure-resource-manager-template-by-using-powershell"></a><span data-ttu-id="2916a-174">Nasazení šablony Azure Resource Manageru pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2916a-174">Deploy the Azure Resource Manager template by using PowerShell</span></span>

<span data-ttu-id="2916a-175">Pokud jste prostředí Azure PowerShell nikdy nepoužívali, navštivte: [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) a postupujte podle pokynů a přihlaste se k Azure a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="2916a-175">If you have never used Azure PowerShell, visit: [How to install and configure Azure PowerShell](/powershell/azure/overview) and follow the instructions to sign into Azure and select your subscription.</span></span>

1. <span data-ttu-id="2916a-176">Přihlášení k prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2916a-176">Login to PowerShell</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

1. <span data-ttu-id="2916a-177">Zkontrolujte předplatná pro příslušný účet.</span><span class="sxs-lookup"><span data-stu-id="2916a-177">Check the subscriptions for the account.</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

    <span data-ttu-id="2916a-178">Zobrazí se výzva k ověření pomocí přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="2916a-178">You are prompted to authenticate with your credentials.</span></span>

1. <span data-ttu-id="2916a-179">Zvolte předplatné Azure, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="2916a-179">Choose which of your Azure subscriptions to use.</span></span>

    ```powershell
    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
    ```

1. <span data-ttu-id="2916a-180">Pokud je to potřeba, vytvořte pomocí rutiny **New-AzureResourceGroup** skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="2916a-180">If needed, create a resource group by using the **New-AzureResourceGroup** cmdlet.</span></span> <span data-ttu-id="2916a-181">V následujícím příkladu vytvoříte skupinu prostředků s názvem AppgatewayRG v umístění Východní USA.</span><span class="sxs-lookup"><span data-stu-id="2916a-181">In the following example, you create a resource group called AppgatewayRG in East US location.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name AppgatewayRG -Location "West US"
    ```

1. <span data-ttu-id="2916a-182">Spuštěním rutiny **New-AzureRmResourceGroupDeployment** nasadíte novou virtuální síť pomocí šablony a souborů parametrů, které jste stáhli a upravili v krocích výše.</span><span class="sxs-lookup"><span data-stu-id="2916a-182">Run the **New-AzureRmResourceGroupDeployment** cmdlet to deploy the new virtual network by using the preceding template and parameter files you downloaded and modified.</span></span>
    
    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

## <a name="deploy-the-azure-resource-manager-template-by-using-the-azure-cli"></a><span data-ttu-id="2916a-183">Nasazení šablony Azure Resource Manageru pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="2916a-183">Deploy the Azure Resource Manager template by using the Azure CLI</span></span>

<span data-ttu-id="2916a-184">Pokud chcete nasadit šablonu Azure Resource Manager, kterou jste stáhli pomocí rozhraní příkazového řádku Azure, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="2916a-184">To deploy the Azure Resource Manager template you downloaded by using Azure CLI, follow the following steps:</span></span>

1. <span data-ttu-id="2916a-185">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="2916a-185">If you have never used Azure CLI, see [Install and configure the Azure CLI](/cli/azure/install-azure-cli) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>

1. <span data-ttu-id="2916a-186">V případě potřeby spustit `az group create` příkazu vytvořte skupinu prostředků, jak je znázorněno v následující fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="2916a-186">If necessary, run the `az group create` command to create a resource group, as shown in the following code snippet.</span></span> <span data-ttu-id="2916a-187">Prohlédněte si výstup příkazu.</span><span class="sxs-lookup"><span data-stu-id="2916a-187">Notice the output of the command.</span></span> <span data-ttu-id="2916a-188">Seznam uvedený za výstupem vysvětluje použité parametry.</span><span class="sxs-lookup"><span data-stu-id="2916a-188">The list shown after the output explains the parameters used.</span></span> <span data-ttu-id="2916a-189">Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2916a-189">For more information about resource groups, visit [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

    ```azurecli
    az group create --location westus --name appgatewayRG
    ```
    
    <span data-ttu-id="2916a-190">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="2916a-190">**-n (or --name)**.</span></span> <span data-ttu-id="2916a-191">Název nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="2916a-191">Name for the new resource group.</span></span> <span data-ttu-id="2916a-192">V našem scénáři je to název *appgatewayRG*.</span><span class="sxs-lookup"><span data-stu-id="2916a-192">For our scenario, it's *appgatewayRG*.</span></span>
    
    <span data-ttu-id="2916a-193">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="2916a-193">**-l (or --location)**.</span></span> <span data-ttu-id="2916a-194">Oblast Azure, ve které je vytvořena nová skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="2916a-194">Azure region where the new resource group is created.</span></span> <span data-ttu-id="2916a-195">Pro náš scénář má *westus*.</span><span class="sxs-lookup"><span data-stu-id="2916a-195">For our scenario, it's *westus*.</span></span>

1. <span data-ttu-id="2916a-196">Spustit `az group deployment create` nasadíte novou virtuální síť pomocí šablony a parametr souborů, které jste stáhli a upravili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="2916a-196">Run the `az group deployment create` cmdlet to deploy the new virtual network by using the template and parameter files you downloaded and modified in the preceding step.</span></span> <span data-ttu-id="2916a-197">Seznam uvedený za výstupem vysvětluje použité parametry.</span><span class="sxs-lookup"><span data-stu-id="2916a-197">The list shown after the output explains the parameters used.</span></span>

    ```azurecli
    az group deployment create --resource-group appgatewayRG --name TestAppgatewayDeployment --template-file azuredeploy.json --parameters @azuredeploy-parameters.json
    ```

## <a name="deploy-the-azure-resource-manager-template-by-using-click-to-deploy"></a><span data-ttu-id="2916a-198">Nasazení šablony Azure Resource Manageru pomocí metody Click to Deploy</span><span class="sxs-lookup"><span data-stu-id="2916a-198">Deploy the Azure Resource Manager template by using click-to-deploy</span></span>

<span data-ttu-id="2916a-199">Metoda Click to Deploy je další způsob použití šablon Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="2916a-199">Click-to-deploy is another way to use Azure Resource Manager templates.</span></span> <span data-ttu-id="2916a-200">Je to snadný způsob, jak používat šablony na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="2916a-200">It's an easy way to use templates with the Azure portal.</span></span>

1. <span data-ttu-id="2916a-201">Přejděte na [vytvoření služby application gateway pomocí brány firewall webových aplikací](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).</span><span class="sxs-lookup"><span data-stu-id="2916a-201">Go to [Create an application gateway with web application firewall](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).</span></span>

1. <span data-ttu-id="2916a-202">Klikněte na **Deploy to Azure** (Nasadit do Azure).</span><span class="sxs-lookup"><span data-stu-id="2916a-202">Click **Deploy to Azure**.</span></span>

    ![Nasazení do Azure](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)
    
1. <span data-ttu-id="2916a-204">Zadejte na portálu parametry šablony nasazení a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="2916a-204">Fill out the parameters for the deployment template on the portal and click **OK**.</span></span>

    ![Parametry](./media/application-gateway-create-gateway-arm-template/ibiza1.png)
    
1. <span data-ttu-id="2916a-206">Vyberte **souhlasím s podmínkami a ujednáními výše uvedených** a klikněte na tlačítko **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="2916a-206">Select **I agree to the terms and conditions stated above** and click **Purchase**.</span></span>

1. <span data-ttu-id="2916a-207">V okně Vlastní nasazení klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2916a-207">On the Custom deployment blade, click **Create**.</span></span>

## <a name="providing-certificate-data-to-resource-manager-templates"></a><span data-ttu-id="2916a-208">Poskytuje data certifikátu k šablonám Resource Manager</span><span class="sxs-lookup"><span data-stu-id="2916a-208">Providing certificate data to Resource Manager templates</span></span>

<span data-ttu-id="2916a-209">Při použití protokolu SSL s využitím šablony certifikátu musí být zadané ve řetězec base64 místo odesílání.</span><span class="sxs-lookup"><span data-stu-id="2916a-209">When using SSL with a template, the certificate needs to be provided in a base64 string instead of being uploaded.</span></span> <span data-ttu-id="2916a-210">Převést .pfx nebo .cer na řetězec ve formátu base64 použijte jednu z následujících příkazů.</span><span class="sxs-lookup"><span data-stu-id="2916a-210">To convert a .pfx or .cer to a base64 string use one of the following commands.</span></span> <span data-ttu-id="2916a-211">Následující příkazy převést na řetězec ve formátu base64, které lze zadat do šablony certifikátu.</span><span class="sxs-lookup"><span data-stu-id="2916a-211">The following commands convert the certificate to a base64 string, which can be provided to the template.</span></span> <span data-ttu-id="2916a-212">Očekávaný výstup je řetězec, který může být uložené v proměnné a vložení v šabloně.</span><span class="sxs-lookup"><span data-stu-id="2916a-212">The expected output is a string that can be stored in a variable and pasted in the template.</span></span>

### <a name="macos"></a><span data-ttu-id="2916a-213">macOS</span><span class="sxs-lookup"><span data-stu-id="2916a-213">macOS</span></span>
```bash
cert=$( base64 <certificate path and name>.pfx )
echo $cert
```

### <a name="windows"></a><span data-ttu-id="2916a-214">Windows</span><span class="sxs-lookup"><span data-stu-id="2916a-214">Windows</span></span>
```powershell
[System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("<certificate path and name>.pfx"))
```

## <a name="delete-all-resources"></a><span data-ttu-id="2916a-215">Odstranění všech prostředků</span><span class="sxs-lookup"><span data-stu-id="2916a-215">Delete all resources</span></span>

<span data-ttu-id="2916a-216">Chcete-li odstranit všechny prostředky, které jsou vytvořené v tomto článku, proveďte jednu z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="2916a-216">To delete all resources created in this article, complete one of the following steps:</span></span>

### <a name="powershell"></a><span data-ttu-id="2916a-217">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2916a-217">PowerShell</span></span>

```powershell
Remove-AzureRmResourceGroup -Name appgatewayRG
```

### <a name="azure-cli"></a><span data-ttu-id="2916a-218">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2916a-218">Azure CLI</span></span>

```azurecli
az group delete --name appgatewayRG
```

## <a name="next-steps"></a><span data-ttu-id="2916a-219">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2916a-219">Next steps</span></span>

<span data-ttu-id="2916a-220">Pokud chcete konfigurovat přesměrování zpracování SSL, přečtěte si článek [Konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="2916a-220">If you want to configure SSL offload, visit: [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="2916a-221">Pokud chcete službu Application Gateway nakonfigurovat pro použití s interním nástrojem pro vyrovnávání zatížení, přečtěte si článek [Vytvoření aplikační brány s interním nástrojem pro vyrovnávání zatížení (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="2916a-221">If you want to configure an application gateway to use with an internal load balancer, visit: [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="2916a-222">Pokud chcete získat další informace o obecných možnostech vyrovnávání zatížení, přečtěte si článek:</span><span class="sxs-lookup"><span data-stu-id="2916a-222">If you want more information about load balancing options in general, visit:</span></span>

* [<span data-ttu-id="2916a-223">Nástroj pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="2916a-223">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="2916a-224">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="2916a-224">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

