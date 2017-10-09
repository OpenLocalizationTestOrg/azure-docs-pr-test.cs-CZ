---
title: "Vyrovnávání zatížení aaaCreate internetové - šablony Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate přístupem Internetu pro vyrovnávání zátěže ve Správci prostředků pomocí šablony"
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
ms.openlocfilehash: 2bce8cb87303838f3bc732d51228ab46d8015552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-internet-facing-load-balancer-using-a-template"></a><span data-ttu-id="700df-103">Vytvoření internetového nástroje pro vyrovnávání zatížení pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="700df-103">Creating an Internet facing load balancer using a template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="700df-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="700df-104">Portal</span></span>](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [<span data-ttu-id="700df-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="700df-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [<span data-ttu-id="700df-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="700df-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [<span data-ttu-id="700df-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="700df-107">Template</span></span>](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="700df-108">Tento článek se týká modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="700df-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="700df-109">Můžete také [zjistěte, jak toocreate přístupem Internetu pro vyrovnávání zátěže pomocí modelu nasazení classic](load-balancer-get-started-internet-classic-portal.md)</span><span class="sxs-lookup"><span data-stu-id="700df-109">You can also [Learn how toocreate an Internet facing load balancer using classic deployment model](load-balancer-get-started-internet-classic-portal.md)</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploy-hello-template-by-using-click-toodeploy"></a><span data-ttu-id="700df-110">Nasazení šablony hello pomocí klikněte na tlačítko toodeploy</span><span class="sxs-lookup"><span data-stu-id="700df-110">Deploy hello template by using click toodeploy</span></span>

<span data-ttu-id="700df-111">Hello Ukázka šablony k dispozici v úložišti na veřejné hello používá parametr souboru, který obsahuje hello výchozí hodnoty používané toogenerate hello scénář popsaný výše.</span><span class="sxs-lookup"><span data-stu-id="700df-111">hello sample template available in hello public repository uses a parameter file containing hello default values used toogenerate hello scenario described above.</span></span> <span data-ttu-id="700df-112">toodeploy pomocí této šablony, klikněte na toodeploy, postupujte podle [tento odkaz](http://go.microsoft.com/fwlink/?LinkId=544801), klikněte na tlačítko **nasazení tooAzure**, nahraďte hello výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="700df-112">toodeploy this template using click toodeploy, follow [this link](http://go.microsoft.com/fwlink/?LinkId=544801), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>

## <a name="deploy-hello-template-by-using-powershell"></a><span data-ttu-id="700df-113">Nasazení šablony hello pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="700df-113">Deploy hello template by using PowerShell</span></span>

<span data-ttu-id="700df-114">toodeploy hello šablonu, kterou jste stáhli pomocí prostředí PowerShell, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="700df-114">toodeploy hello template you downloaded by using PowerShell, follow hello steps below.</span></span>

1. <span data-ttu-id="700df-115">Pokud jste prostředí Azure PowerShell nikdy nepoužívali, projděte si téma [jak tooInstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) a postupujte podle pokynů hello všechny toohello hello způsob ukončení toosign do Azure a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="700df-115">If you have never used Azure PowerShell, see [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) and follow hello instructions all hello way toohello end toosign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="700df-116">Spustit hello **New-AzureRmResourceGroupDeployment** hello rutiny toocreate skupinu prostředků pomocí šablony.</span><span class="sxs-lookup"><span data-stu-id="700df-116">Run hello **New-AzureRmResourceGroupDeployment** cmdlet toocreate a resource group using hello template.</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
        -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
        -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'
    ```

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a><span data-ttu-id="700df-117">Nasazení šablony hello pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="700df-117">Deploy hello template by using hello Azure CLI</span></span>

<span data-ttu-id="700df-118">šablony hello toodeploy pomocí hello rozhraní příkazového řádku Azure, postupujte podle kroků hello níže.</span><span class="sxs-lookup"><span data-stu-id="700df-118">toodeploy hello template by using hello Azure CLI, follow hello steps below.</span></span>

1. <span data-ttu-id="700df-119">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="700df-119">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="700df-120">Spustit hello **azure konfigurace režim** příkaz tooswitch tooResource Manager režimu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="700df-120">Run hello **azure config mode** command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="700df-121">Tady je hello očekávaný výstup výše hello příkazu:</span><span class="sxs-lookup"><span data-stu-id="700df-121">Here is hello expected output for hello command above:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="700df-122">V prohlížeči přejděte příliš[hello šablony rychlý Start](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules)hello obsah souboru json hello zkopírujte a vložte do nového souboru v počítači.</span><span class="sxs-lookup"><span data-stu-id="700df-122">From your browser, navigate too[hello Quickstart Template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules), copy hello contents of hello json file and paste into a new file in your computer.</span></span> <span data-ttu-id="700df-123">V tomto scénáři by kopírování hello hodnoty menší než tooa soubor s názvem **c:\lb\azuredeploy.parameters.json**.</span><span class="sxs-lookup"><span data-stu-id="700df-123">For this scenario, you would be copying hello values below tooa file named **c:\lb\azuredeploy.parameters.json**.</span></span>
4. <span data-ttu-id="700df-124">Spustit hello **vytvoření skupiny azure nasazení** rutiny toodeploy hello nový nástroj pro vyrovnávání zatížení pomocí šablony hello a parametr souborů, které jste stáhli a upravili v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="700df-124">Run hello **azure group deployment create** cmdlet toodeploy hello new load balancer by using hello template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="700df-125">Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.</span><span class="sxs-lookup"><span data-stu-id="700df-125">hello list shown after hello output explains hello parameters used.</span></span>

    ```azurecli
    azure group create --name TestRG --location westus --template-file 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' --parameters-file 'c:\lb\azuredeploy.parameters.json'
    ```

## <a name="next-steps"></a><span data-ttu-id="700df-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="700df-126">Next steps</span></span>

[<span data-ttu-id="700df-127">Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="700df-127">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="700df-128">Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="700df-128">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="700df-129">Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="700df-129">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
