---
title: "Vytvoření nástroje pro vyrovnávání zatížení připojeného k internetu – šablony Azure | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvořit internetový nástroj pro vyrovnávání zatížení v Resource Manageru pomocí šablony"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: b24f4729-4559-4458-8527-71009d242647
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: d829000e63515814b192f3f8256e3b8637bb3a34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="creating-an-internet-facing-load-balancer-using-a-template"></a><span data-ttu-id="6cb98-103">Vytvoření internetového nástroje pro vyrovnávání zatížení pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="6cb98-103">Creating an Internet facing load balancer using a template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="6cb98-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6cb98-104">Portal</span></span>](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [<span data-ttu-id="6cb98-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6cb98-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [<span data-ttu-id="6cb98-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6cb98-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [<span data-ttu-id="6cb98-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="6cb98-107">Template</span></span>](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="6cb98-108">Tento článek se týká modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6cb98-108">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="6cb98-109">Případně [zjistěte, jak vytvořit internetový nástroj pro vyrovnávání zatížení pomocí modelu nasazení Classic](load-balancer-get-started-internet-classic-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6cb98-109">You can also [Learn how to create an Internet facing load balancer using classic deployment model](load-balancer-get-started-internet-classic-portal.md)</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a><span data-ttu-id="6cb98-110">Nasazení šablony pomocí metody Click to Deploy</span><span class="sxs-lookup"><span data-stu-id="6cb98-110">Deploy the template by using click to deploy</span></span>

<span data-ttu-id="6cb98-111">Ukázková šablona, která je k dispozici ve veřejném úložišti, používá soubor parametrů obsahující výchozí hodnoty, které se použijí k vygenerování výše popsaného scénáře.</span><span class="sxs-lookup"><span data-stu-id="6cb98-111">The sample template available in the public repository uses a parameter file containing the default values used to generate the scenario described above.</span></span> <span data-ttu-id="6cb98-112">Pokud chcete nasadit tuto šablonu pomocí metody Click to Deploy, pokračujte na [tento odkaz](http://go.microsoft.com/fwlink/?LinkId=544801), klikněte na **Nasadit do Azure**, v případě potřeby nahraďte výchozí hodnoty parametrů, a pokračujte podle pokynů na portálu.</span><span class="sxs-lookup"><span data-stu-id="6cb98-112">To deploy this template using click to deploy, follow [this link](http://go.microsoft.com/fwlink/?LinkId=544801), click **Deploy to Azure**, replace the default parameter values if necessary, and follow the instructions in the portal.</span></span>

## <a name="deploy-the-template-by-using-powershell"></a><span data-ttu-id="6cb98-113">Nasazení šablony pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6cb98-113">Deploy the template by using PowerShell</span></span>

<span data-ttu-id="6cb98-114">Pokud chcete nasadit šablonu, kterou jste stáhli, pomocí prostředí PowerShell, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="6cb98-114">To deploy the template you downloaded by using PowerShell, follow the steps below.</span></span>

1. <span data-ttu-id="6cb98-115">Pokud jste prostředí Azure PowerShell nikdy nepoužívali, přejděte na téma [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) a proveďte všechny pokyny, abyste se mohli přihlásit k Azure a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="6cb98-115">If you have never used Azure PowerShell, see [How to Install and Configure Azure PowerShell](/powershell/azure/overview) and follow the instructions all the way to the end to sign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="6cb98-116">Spusťte rutinu **New-AzureRmResourceGroupDeployment**, která pomocí šablony vytvoří skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6cb98-116">Run the **New-AzureRmResourceGroupDeployment** cmdlet to create a resource group using the template.</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
        -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
        -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'
    ```

## <a name="deploy-the-template-by-using-the-azure-cli"></a><span data-ttu-id="6cb98-117">Nasazení šablony pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="6cb98-117">Deploy the template by using the Azure CLI</span></span>

<span data-ttu-id="6cb98-118">Pokud chcete nasadit šablonu pomocí rozhraní příkazového řádku Azure, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="6cb98-118">To deploy the template by using the Azure CLI, follow the steps below.</span></span>

1. <span data-ttu-id="6cb98-119">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="6cb98-119">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="6cb98-120">Spuštěním příkazu **azure config mode** přejděte do režimu Resource Manager, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="6cb98-120">Run the **azure config mode** command to switch to Resource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="6cb98-121">Toto je očekávaný výstup výše uvedeného příkazu:</span><span class="sxs-lookup"><span data-stu-id="6cb98-121">Here is the expected output for the command above:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="6cb98-122">V prohlížeči přejděte na [Šablonu pro rychlý start](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules), zkopírujte obsah souboru JSON a vložte jej do nového souboru v počítači.</span><span class="sxs-lookup"><span data-stu-id="6cb98-122">From your browser, navigate to [the Quickstart Template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules), copy the contents of the json file and paste into a new file in your computer.</span></span> <span data-ttu-id="6cb98-123">V tomto scénáři byste zkopírovali níže uvedené hodnoty do souboru s názvem **c:\lb\azuredeploy.parameters.json**.</span><span class="sxs-lookup"><span data-stu-id="6cb98-123">For this scenario, you would be copying the values below to a file named **c:\lb\azuredeploy.parameters.json**.</span></span>
4. <span data-ttu-id="6cb98-124">Spuštěním rutiny **azure group deployment create** nasaďte nový nástroj pro vyrovnávání zatížení pomocí šablony a souborů parametrů, které jste stáhli a upravili v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="6cb98-124">Run the **azure group deployment create** cmdlet to deploy the new load balancer by using the template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="6cb98-125">Seznam uvedený za výstupem vysvětluje použité parametry.</span><span class="sxs-lookup"><span data-stu-id="6cb98-125">The list shown after the output explains the parameters used.</span></span>

    ```azurecli
    azure group create --name TestRG --location westus --template-file 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' --parameters-file 'c:\lb\azuredeploy.parameters.json'
    ```

## <a name="next-steps"></a><span data-ttu-id="6cb98-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6cb98-126">Next steps</span></span>

[<span data-ttu-id="6cb98-127">Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="6cb98-127">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="6cb98-128">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="6cb98-128">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="6cb98-129">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="6cb98-129">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
