---
title: "aaaCreate Vyrovnávání zatížení interní - šablony Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate na interní vyrovnávání zátěže pomocí šablony Resource Manageru"
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
ms.openlocfilehash: 3ffa8178b863367cd79e2bc2b7ce4e45b23267e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-using-a-template"></a><span data-ttu-id="5c333-103">Vytvoření interního nástroje pro vyrovnávání zatížení pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="5c333-103">Create an internal load balancer using a template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5c333-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5c333-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="5c333-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5c333-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="5c333-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5c333-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="5c333-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="5c333-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="5c333-108">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5c333-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="5c333-109">Tento článek popisuje použití modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello [modelu nasazení classic](load-balancer-get-started-ilb-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="5c333-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](load-balancer-get-started-ilb-classic-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-hello-template-by-using-click-toodeploy"></a><span data-ttu-id="5c333-110">Nasazení šablony hello pomocí klikněte na tlačítko toodeploy</span><span class="sxs-lookup"><span data-stu-id="5c333-110">Deploy hello template by using click toodeploy</span></span>

<span data-ttu-id="5c333-111">Hello Ukázka šablony k dispozici v úložišti na veřejné hello používá parametr souboru, který obsahuje hello výchozí hodnoty používané toogenerate hello scénář popsaný výše.</span><span class="sxs-lookup"><span data-stu-id="5c333-111">hello sample template available in hello public repository uses a parameter file containing hello default values used toogenerate hello scenario described above.</span></span> <span data-ttu-id="5c333-112">toodeploy pomocí této šablony, klikněte na toodeploy, postupujte podle [tento odkaz](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), klikněte na tlačítko **nasazení tooAzure**, nahraďte hello výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="5c333-112">toodeploy this template using click toodeploy, follow [this link](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>

## <a name="deploy-hello-template-by-using-powershell"></a><span data-ttu-id="5c333-113">Nasazení šablony hello pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="5c333-113">Deploy hello template by using PowerShell</span></span>

<span data-ttu-id="5c333-114">toodeploy hello šablonu, kterou jste stáhli pomocí prostředí PowerShell, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="5c333-114">toodeploy hello template you downloaded by using PowerShell, follow hello steps below.</span></span>

1. <span data-ttu-id="5c333-115">Pokud jste prostředí Azure PowerShell nikdy nepoužívali, projděte si téma [jak tooInstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) a postupujte podle pokynů hello všechny toohello hello způsob ukončení toosign do Azure a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="5c333-115">If you have never used Azure PowerShell, see [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) and follow hello instructions all hello way toohello end toosign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="5c333-116">Stáhněte si hello parametry souboru tooyour místní disk.</span><span class="sxs-lookup"><span data-stu-id="5c333-116">Download hello parameters file tooyour local disk.</span></span>
3. <span data-ttu-id="5c333-117">Upravte soubor hello a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="5c333-117">Edit hello file and save it.</span></span>
4. <span data-ttu-id="5c333-118">Spustit hello **New-AzureRmResourceGroupDeployment** hello rutiny toocreate skupinu prostředků pomocí šablony.</span><span class="sxs-lookup"><span data-stu-id="5c333-118">Run hello **New-AzureRmResourceGroupDeployment** cmdlet toocreate a resource group using hello template.</span></span>

    ```azurecli
    New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
        -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
        -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'
    ```

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a><span data-ttu-id="5c333-119">Nasazení šablony hello pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="5c333-119">Deploy hello template by using hello Azure CLI</span></span>

<span data-ttu-id="5c333-120">šablony hello toodeploy pomocí hello rozhraní příkazového řádku Azure, postupujte podle kroků hello níže.</span><span class="sxs-lookup"><span data-stu-id="5c333-120">toodeploy hello template by using hello Azure CLI, follow hello steps below.</span></span>

1. <span data-ttu-id="5c333-121">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="5c333-121">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="5c333-122">Spustit hello **azure konfigurace režim** příkaz tooswitch tooResource Manager režimu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="5c333-122">Run hello **azure config mode** command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="5c333-123">Tady je hello očekávaný výstup výše hello příkazu:</span><span class="sxs-lookup"><span data-stu-id="5c333-123">Here is hello expected output for hello command above:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="5c333-124">Otevřete soubor parametrů hello, vyberte svůj obsah a uložte ho tooa soubor v počítači.</span><span class="sxs-lookup"><span data-stu-id="5c333-124">Open hello parameter file, select its contents, and save it tooa file in your computer.</span></span> <span data-ttu-id="5c333-125">V tomto příkladu jsme příliš uložili soubor parametrů hello*Parameters.JSON tímto kódem*.</span><span class="sxs-lookup"><span data-stu-id="5c333-125">For this example, we saved hello parameters file too*parameters.json*.</span></span>
4. <span data-ttu-id="5c333-126">Spustit hello **vytvoření skupiny azure nasazení** příkaz toodeploy hello nový nástroj pro vyrovnávání zatížení interní pomocí šablony hello a parametr souborů, které jste stáhli a upravili v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="5c333-126">Run hello **azure group deployment create** command toodeploy hello new internal load balancer by using hello template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="5c333-127">Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.</span><span class="sxs-lookup"><span data-stu-id="5c333-127">hello list shown after hello output explains hello parameters used.</span></span>

    ```azurecli
    azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json
    ```

## <a name="next-steps"></a><span data-ttu-id="5c333-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5c333-128">Next steps</span></span>

[<span data-ttu-id="5c333-129">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení pomocí spřažení se zdrojovou IP adresou</span><span class="sxs-lookup"><span data-stu-id="5c333-129">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="5c333-130">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="5c333-130">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

