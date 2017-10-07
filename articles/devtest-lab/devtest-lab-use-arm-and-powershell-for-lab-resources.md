---
title: "aaaCreate nebo upravit labs automaticky pomocí šablon Azure Resource Manageru pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toouse šablon Azure Resource Manageru pomocí prostředí PowerShell toocreate nebo upravit automaticky v testovacím prostředí DevTest labs"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: dad9944c-0b20-48be-ba80-8f4aa0950903
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: tarcher
ms.openlocfilehash: 29c8bc67caaec17b1f8926dde4e5d9d314b06600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-or-modify-labs-automatically-using-azure-resource-manager-templates-and-powershell"></a>Vytvořit nebo upravit labs automaticky pomocí šablon Azure Resource Manageru a prostředí PowerShell

DevTest Labs poskytuje mnoho šablon Azure Resource Manageru a skriptů prostředí PowerShell, které můžete vám pomohou rychle a automaticky vytvořit nové labs nebo upravit existující labs a pak nasadit tyto prostředky.

Tento článek pomůže vás provede procesem hello pomocí těchto šablon a skripty tooautomate hello vytváření, úpravu a nasazení vaší laboratoře. Tento článek také ukazuje, kde můžete najít další informace o tom, jak toouse prostředí PowerShell tooperform některé běžné úlohy v DevTest Labs.

## <a name="step-1-gather-your-templates-and-scripts"></a>Krok 1: Shromáždění šablony a skripty
Můžete najít předem provedené [šablon Azure Resource Manageru](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) a [skriptů prostředí PowerShell](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) v našeho veřejného [úložiště Github](https://github.com/Azure/azure-devtestlab). Použijte je jako-je, nebo si je přizpůsobit pro vaše potřeby a uložit je do vlastní [privátní úložiště Git](devtest-lab-add-artifact-repo.md). 

## <a name="step-2-modify-your-azure-resource-manager-template"></a>Krok 2: Upravte šablony Azure Resource Manager
Můžete provést kroky hello v [vytvoření vaší první šablony Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-create-first-template) Pokud jste vytvořili šablonu před nikdy.

Kromě toho [osvědčené postupy pro vytváření šablon Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) nabízí mnoho pokyny a návrhy toohelp vytváření šablon Azure Resource Manageru, které jsou toouse spolehlivé a snadné. Obvykle budete používat jeden z přístupů hello varianta nebo příklady zadat a upravit šablony pro vaše potřeby.

## <a name="step-3-deploy-resources-with-powershell"></a>Krok 3: Nasazení prostředků v prostředí PowerShell
Po přizpůsobení šablony a skripty, postupujte podle hello kroky nezbytné příliš[nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Hello článek poskytuje obecné informace o použití prostředí Azure PowerShell s toodeploy šablony Azure Resource Manager tooAzure vaše prostředky.


## <a name="common-tasks-you-can-perform-in-devtest-labs-using-powershell"></a>Běžné úlohy, které můžete provádět v DevTest labs pomocí prostředí PowerShell
Existuje mnoho dalších běžných úloh můžete automatizovat pomocí prostředí PowerShell. Hello následující oddíly hello dokumentace popisují hello kroky požadované tooperform tyto úlohy.

* [Vytvořit vlastní image ze souboru virtuálního pevného disku pomocí prostředí PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
* [Nahrání virtuálního pevného disku soubor toolab účet úložiště pomocí prostředí PowerShell](devtest-lab-upload-vhd-using-powershell.md)
* [Přidat testovacím tooa externího uživatele pomocí prostředí PowerShell](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell)
* [Vytvoření testovacího prostředí vlastní role pomocí prostředí PowerShell](devtest-lab-grant-user-permissions-to-specific-lab-policies.md#creating-a-lab-custom-role-using-powershell)

### <a name="next-steps"></a>Další kroky
* Zjistěte, jak toocreate [privátní úložiště Git](devtest-lab-add-artifact-repo.md) kde budete ukládat vlastní šablony nebo skripty.
* Prozkoumejte hello [šablon Azure Resource Manageru z Galerie šablon Azure Quickstart](https://github.com/Azure/azure-quickstart-templates).
