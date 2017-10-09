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
# <a name="creating-an-internet-facing-load-balancer-using-a-template"></a>Vytvoření internetového nástroje pro vyrovnávání zatížení pomocí šablony

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Šablona](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení Resource Manager hello. Můžete také [zjistěte, jak toocreate přístupem Internetu pro vyrovnávání zátěže pomocí modelu nasazení classic](load-balancer-get-started-internet-classic-portal.md)

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploy-hello-template-by-using-click-toodeploy"></a>Nasazení šablony hello pomocí klikněte na tlačítko toodeploy

Hello Ukázka šablony k dispozici v úložišti na veřejné hello používá parametr souboru, který obsahuje hello výchozí hodnoty používané toogenerate hello scénář popsaný výše. toodeploy pomocí této šablony, klikněte na toodeploy, postupujte podle [tento odkaz](http://go.microsoft.com/fwlink/?LinkId=544801), klikněte na tlačítko **nasazení tooAzure**, nahraďte hello výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů hello hello portálu.

## <a name="deploy-hello-template-by-using-powershell"></a>Nasazení šablony hello pomocí prostředí PowerShell

toodeploy hello šablonu, kterou jste stáhli pomocí prostředí PowerShell, postupujte podle následujících kroků hello.

1. Pokud jste prostředí Azure PowerShell nikdy nepoužívali, projděte si téma [jak tooInstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) a postupujte podle pokynů hello všechny toohello hello způsob ukončení toosign do Azure a vybrat své předplatné.
2. Spustit hello **New-AzureRmResourceGroupDeployment** hello rutiny toocreate skupinu prostředků pomocí šablony.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
        -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
        -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'
    ```

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a>Nasazení šablony hello pomocí hello rozhraní příkazového řádku Azure

šablony hello toodeploy pomocí hello rozhraní příkazového řádku Azure, postupujte podle kroků hello níže.

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.
2. Spustit hello **azure konfigurace režim** příkaz tooswitch tooResource Manager režimu, jak je uvedeno níže.

    ```azurecli
    azure config mode arm
    ```

    Tady je hello očekávaný výstup výše hello příkazu:

        info:    New mode is arm

3. V prohlížeči přejděte příliš[hello šablony rychlý Start](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules)hello obsah souboru json hello zkopírujte a vložte do nového souboru v počítači. V tomto scénáři by kopírování hello hodnoty menší než tooa soubor s názvem **c:\lb\azuredeploy.parameters.json**.
4. Spustit hello **vytvoření skupiny azure nasazení** rutiny toodeploy hello nový nástroj pro vyrovnávání zatížení pomocí šablony hello a parametr souborů, které jste stáhli a upravili v předchozích krocích. Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.

    ```azurecli
    azure group create --name TestRG --location westus --template-file 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' --parameters-file 'c:\lb\azuredeploy.parameters.json'
    ```

## <a name="next-steps"></a>Další kroky

[Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
