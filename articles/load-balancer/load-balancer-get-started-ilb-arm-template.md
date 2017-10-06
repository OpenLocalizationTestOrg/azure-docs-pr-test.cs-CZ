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
# <a name="create-an-internal-load-balancer-using-a-template"></a>Vytvoření interního nástroje pro vyrovnávání zatížení pomocí šablony

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Šablona](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).  Tento článek popisuje použití modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello [modelu nasazení classic](load-balancer-get-started-ilb-classic-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-hello-template-by-using-click-toodeploy"></a>Nasazení šablony hello pomocí klikněte na tlačítko toodeploy

Hello Ukázka šablony k dispozici v úložišti na veřejné hello používá parametr souboru, který obsahuje hello výchozí hodnoty používané toogenerate hello scénář popsaný výše. toodeploy pomocí této šablony, klikněte na toodeploy, postupujte podle [tento odkaz](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), klikněte na tlačítko **nasazení tooAzure**, nahraďte hello výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů hello hello portálu.

## <a name="deploy-hello-template-by-using-powershell"></a>Nasazení šablony hello pomocí prostředí PowerShell

toodeploy hello šablonu, kterou jste stáhli pomocí prostředí PowerShell, postupujte podle následujících kroků hello.

1. Pokud jste prostředí Azure PowerShell nikdy nepoužívali, projděte si téma [jak tooInstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) a postupujte podle pokynů hello všechny toohello hello způsob ukončení toosign do Azure a vybrat své předplatné.
2. Stáhněte si hello parametry souboru tooyour místní disk.
3. Upravte soubor hello a uložte ho.
4. Spustit hello **New-AzureRmResourceGroupDeployment** hello rutiny toocreate skupinu prostředků pomocí šablony.

    ```azurecli
    New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
        -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
        -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'
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

3. Otevřete soubor parametrů hello, vyberte svůj obsah a uložte ho tooa soubor v počítači. V tomto příkladu jsme příliš uložili soubor parametrů hello*Parameters.JSON tímto kódem*.
4. Spustit hello **vytvoření skupiny azure nasazení** příkaz toodeploy hello nový nástroj pro vyrovnávání zatížení interní pomocí šablony hello a parametr souborů, které jste stáhli a upravili v předchozích krocích. Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.

    ```azurecli
    azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json
    ```

## <a name="next-steps"></a>Další kroky

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení pomocí spřažení se zdrojovou IP adresou](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)

