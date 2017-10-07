---
title: "aaaView a použít šablonu virtuálního počítače Azure Resource Manager | Microsoft Docs"
description: "Zjistěte, jak toouse hello šablony Azure Resource Manageru z virtuálního počítače toocreate ostatních virtuálních počítačů"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: a759d9ce-655c-4ac6-8834-cb29dd7d30dd
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: tarcher
ms.openlocfilehash: b79f7eb4171793681a0b654e6e72a83191c76bde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-a-virtual-machines-azure-resource-manager-template"></a>Použít šablonu virtuálního počítače Azure Resource Manager

Když vytváříte virtuální počítač (VM) v DevTest Labs prostřednictvím hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040), můžete zobrazit šablonu Azure Resource Manager hello před uložením hello virtuálních počítačů. Hello šablony lze poté použít jako základ toocreate více virtuálních počítačů testovacího prostředí s hello stejné nastavení.

Tento článek popisuje, jak tooview hello šablony Resource Manageru při vytvoření virtuálního počítače a jak toodeploy ho novější vytvoření tooautomate hello stejného virtuálního počítače.

## <a name="multi-vm-vs-single-vm-resource-manager-templates"></a>Více virtuálních počítačů vs. šablony Resource Manageru single-VM
Existují dva způsoby toocreate virtuální počítače v DevTest Labs pomocí šablony Resource Manageru: zřídit hello Microsoft.DevTestLab/labs/virtualmachines prostředků nebo zřídit hello Microsoft.Commpute/virtualmachines prostředků. Každý se používá v různých scénářích a vyžadují různé oprávnění.

- Šablony Resource Manageru, které používají typ prostředku Microsoft.DevTestLab/labs/virtualmachines (podle údajů v vlastnost "prostředek" hello v šabloně hello) můžou zřizovat virtuální počítače jednotlivých testovacího prostředí. Každý virtuální počítač se pak objeví jako jedna položka v seznamu virtuálních počítačů DevTest Labs hello:

   ![Seznam virtuálních počítačů jako jednotlivé položky v seznamu virtuálních počítačů DevTest Labs hello](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-item.png)

   Tento typ šablony Resource Manageru můžete zřídit prostřednictvím hello prostředí Azure PowerShell příkaz **New-AzureRmResourceGroupDeployment** nebo prostřednictvím hello příkazu příkazového řádku Azure CLI **vytvořit nasazení skupiny az** . Vyžaduje oprávnění správce, takže uživatelé, kteří jsou přiřazeny k roli uživatele DevTest Labs nelze provést nasazení hello. 

- Šablony Resource Manageru, které používají typ prostředku Microsoft.Compute/virtualmachines můžete zřídit několik virtuálních počítačů jako jeden prostředí v seznamu virtuálních počítačů DevTest Labs hello:

   ![Seznam virtuálních počítačů jako jednotlivé položky v seznamu virtuálních počítačů DevTest Labs hello](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-environment.png)

   Virtuální počítače ve stejném prostředí lze spravovat společně hello a sdílenou složku hello stejný životní cyklus. Uživatelé, kteří jsou přiřazeny k roli uživatele DevTest Labs můžete vytvořit prostředí pomocí těchto šablon, dokud hello správce nakonfiguroval hello testovacím tímto způsobem.

Hello zbývající část tohoto článku popisuje šablony Resource Manageru, které používají Mirosoft.DevTestLab/labs/virtualmachines. Ty se používají testovacího prostředí admins tooautomate testovacím vytvoření virtuálního počítače (například vymahatelných virtuální počítače) nebo zlaté image generace (například obrázek factory).

[Osvědčené postupy pro vytváření šablon Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) nabízí mnoho pokyny a návrhy toohelp vytváření šablon Azure Resource Manageru, které jsou toouse spolehlivé a snadné.

## <a name="view-and-save-a-virtual-machines-resource-manager-template"></a>Zobrazení a uložení šablony Resource Manageru virtuálního počítače
1. Postupujte podle kroků hello v [vytvoření vaší první virtuální počítač v testovacím prostředí](devtest-lab-create-first-vm.md) toobegin vytvoření virtuálního počítače.
1. Zadejte informace o hello požadované pro virtuální počítač a přidejte všechny artefakty, které chcete pro tento virtuální počítač.
1. V dolní části hello okna hello se nastavení konfigurace, zvolte **šablony ARM zobrazení**.

   ![Tlačítko šablony ARM zobrazení](./media/devtest-lab-use-arm-template/devtestlab-lab-view-rm-template.png)
1. Zkopírujte a uložte toouse šablony Resource Manageru hello později toocreate jiný virtuální počítač.

   ![Toosave šablony správce prostředků pro pozdější použití](./media/devtest-lab-use-arm-template/devtestlab-lab-copy-rm-template.png)

Po uložení šablony Resource Manageru hello, je nutné aktualizovat oddíl hello parametry šablony hello než budete moci použít. Můžete vytvořit parameter.json, který upravuje právě hello parametry mimo hello skutečné šablony Resource Manageru. 

![Přizpůsobení paramaters pomocí souboru JSON](./media/devtest-lab-use-arm-template/devtestlab-lab-custom-params.png)

## <a name="deploy-a-resource-manager-template-toocreate-a-vm"></a>Nasazení toocreate šablony správce prostředků virtuálního počítače
Po uložení šablony Resource Manageru a přizpůsobené vašim potřebám, ho můžete tooautomate vytvoření virtuálního počítače. [Nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy) popisuje, jak toouse prostředí Azure PowerShell s Resource Manager šablony toodeploy tooAzure vaše prostředky. [Nasazení prostředků pomocí šablony Resource Manageru a rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli) popisuje, jak toouse rozhraní příkazového řádku Azure s toodeploy šablony Resource Manageru tooAzure vaše prostředky.

> [!NOTE]
> Pouze uživatel s oprávněními vlastníka testovacího prostředí můžete vytvořit virtuální počítače ze šablony Resource Manageru pomocí prostředí Azure PowerShell. Pokud chcete, aby tooautomate vytvoření virtuálního počítače pomocí šablony Resource Manageru a máte oprávnění uživatele, můžete použít hello [ **az testovacího prostředí virtuálního počítače vytvořit** v hello rozhraní příkazového řádku](https://docs.microsoft.com/cli/azure/lab/vm#create).

### <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[vytvoření prostředí více virtuálních počítačů pomocí šablony Resource Manageru](devtest-lab-create-environment-from-arm.md).
* Prozkoumejte další šablony Resource Manageru rychlým zahájením pro automatizaci DevTest Labs z hello [veřejného úložiště DevTest Labs GitHub](https://github.com/Azure/azure-quickstart-templates).
