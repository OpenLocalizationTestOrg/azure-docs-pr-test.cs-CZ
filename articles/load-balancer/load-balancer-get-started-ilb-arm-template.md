---
title: "Vytvoření interního nástroje pro vyrovnávání zatížení – šablona Azure | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvořit interní nástroj pro vyrovnávání zatížení pomocí šablony v Resource Manageru"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: 64150862-6ced-42de-85dc-89d323257d7c
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 5e0278cf5c605298932d6ac55d147a1c43fd9d23
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-internal-load-balancer-using-a-template"></a><span data-ttu-id="65a3e-103">Vytvoření interního nástroje pro vyrovnávání zatížení pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="65a3e-103">Create an internal load balancer using a template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="65a3e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="65a3e-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="65a3e-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="65a3e-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="65a3e-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="65a3e-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="65a3e-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="65a3e-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="65a3e-108">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="65a3e-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="65a3e-109">Tento článek se věnuje modelu nasazení Resource Manager, který Microsoft doporučuje pro většinu nových nasazení namísto [klasického modelu nasazení](load-balancer-get-started-ilb-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="65a3e-109">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the [classic deployment model](load-balancer-get-started-ilb-classic-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a><span data-ttu-id="65a3e-110">Nasazení šablony pomocí metody Click to Deploy</span><span class="sxs-lookup"><span data-stu-id="65a3e-110">Deploy the template by using click to deploy</span></span>

<span data-ttu-id="65a3e-111">Ukázková šablona, která je k dispozici ve veřejném úložišti, používá soubor parametrů obsahující výchozí hodnoty, které se použijí k vygenerování výše popsaného scénáře.</span><span class="sxs-lookup"><span data-stu-id="65a3e-111">The sample template available in the public repository uses a parameter file containing the default values used to generate the scenario described above.</span></span> <span data-ttu-id="65a3e-112">Pokud chcete nasadit tuto šablonu pomocí metody Click to Deploy, pokračujte na [tento odkaz](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), klikněte na **Nasadit do Azure**, v případě potřeby nahraďte výchozí hodnoty parametrů, a pokračujte podle pokynů na portálu.</span><span class="sxs-lookup"><span data-stu-id="65a3e-112">To deploy this template using click to deploy, follow [this link](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), click **Deploy to Azure**, replace the default parameter values if necessary, and follow the instructions in the portal.</span></span>

## <a name="deploy-the-template-by-using-powershell"></a><span data-ttu-id="65a3e-113">Nasazení šablony pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="65a3e-113">Deploy the template by using PowerShell</span></span>

<span data-ttu-id="65a3e-114">Pokud chcete nasadit šablonu, kterou jste stáhli, pomocí prostředí PowerShell, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="65a3e-114">To deploy the template you downloaded by using PowerShell, follow the steps below.</span></span>

1. <span data-ttu-id="65a3e-115">Pokud jste prostředí Azure PowerShell nikdy nepoužívali, přejděte na téma [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) a proveďte všechny pokyny, abyste se mohli přihlásit k Azure a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="65a3e-115">If you have never used Azure PowerShell, see [How to Install and Configure Azure PowerShell](/powershell/azure/overview) and follow the instructions all the way to the end to sign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="65a3e-116">Stáhněte si soubor parametrů na místní disk.</span><span class="sxs-lookup"><span data-stu-id="65a3e-116">Download the parameters file to your local disk.</span></span>
3. <span data-ttu-id="65a3e-117">Upravte soubor a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="65a3e-117">Edit the file and save it.</span></span>
4. <span data-ttu-id="65a3e-118">Spusťte rutinu **New-AzureRmResourceGroupDeployment**, která pomocí šablony vytvoří skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="65a3e-118">Run the **New-AzureRmResourceGroupDeployment** cmdlet to create a resource group using the template.</span></span>

    ```azurecli
    New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
        -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
        -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'
    ```

## <a name="deploy-the-template-by-using-the-azure-cli"></a><span data-ttu-id="65a3e-119">Nasazení šablony pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="65a3e-119">Deploy the template by using the Azure CLI</span></span>

<span data-ttu-id="65a3e-120">Pokud chcete nasadit šablonu pomocí rozhraní příkazového řádku Azure, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="65a3e-120">To deploy the template by using the Azure CLI, follow the steps below.</span></span>

1. <span data-ttu-id="65a3e-121">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="65a3e-121">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="65a3e-122">Spuštěním příkazu **azure config mode** přejděte do režimu Resource Manager, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="65a3e-122">Run the **azure config mode** command to switch to Resource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="65a3e-123">Toto je očekávaný výstup výše uvedeného příkazu:</span><span class="sxs-lookup"><span data-stu-id="65a3e-123">Here is the expected output for the command above:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="65a3e-124">Otevřete soubor parametrů, vyberte jeho obsah a ten uložte do souboru ve svém počítači.</span><span class="sxs-lookup"><span data-stu-id="65a3e-124">Open the parameter file, select its contents, and save it to a file in your computer.</span></span> <span data-ttu-id="65a3e-125">V tomto příkladu jsme uložili soubor parametrů do souboru *parameters.json*.</span><span class="sxs-lookup"><span data-stu-id="65a3e-125">For this example, we saved the parameters file to *parameters.json*.</span></span>
4. <span data-ttu-id="65a3e-126">Spuštěním příkazu **azure group deployment create** nasaďte nový interní nástroj pro vyrovnávání zatížení pomocí šablony a souborů parametrů, které jste stáhli a upravili v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="65a3e-126">Run the **azure group deployment create** command to deploy the new internal load balancer by using the template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="65a3e-127">Seznam uvedený za výstupem vysvětluje použité parametry.</span><span class="sxs-lookup"><span data-stu-id="65a3e-127">The list shown after the output explains the parameters used.</span></span>

    ```azurecli
    azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json
    ```

## <a name="next-steps"></a><span data-ttu-id="65a3e-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="65a3e-128">Next steps</span></span>

[<span data-ttu-id="65a3e-129">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení pomocí spřažení se zdrojovou IP adresou</span><span class="sxs-lookup"><span data-stu-id="65a3e-129">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="65a3e-130">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="65a3e-130">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

