---
title: "aaaCreate serveru Azure Analysis Services pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak server toocreate Azure Analysis Services serveru pomocí prostředí PowerShell"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/01/2017
ms.author: owend
ms.custom: mvc
ms.openlocfilehash: 269b78983410f773d47c4cea34d6d353b19f9e91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-analysis-services-server-by-using-powershell"></a>Vytvoření serveru služby Azure Analysis Services pomocí PowerShellu

Tento rychlý start popisuje pomocí serveru Azure Analysis Services v prostředí PowerShell z příkazového řádku toocreate hello [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) ve vašem předplatném Azure.

Tato úloha vyžaduje modul Azure PowerShell verze 4.0 nebo novější. verze hello toofind, spusťte ` Get-Module -ListAvailable AzureRM`. tooinstall nebo aktualizace, najdete v části [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps). 

> [!NOTE]
> Vytvoření serveru může znamenat, že se vám začne fakturovat nová služba. Další, najdete v části toolearn [ceny služby Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="prerequisites"></a>Požadavky
toocomplete tento rychlý start, budete potřebovat:

* **Předplatné Azure**: navštivte [bezplatná zkušební verze Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate účet.
* **Azure Active Directory:** Vaše předplatné musí být přidružené k tenantovi Azure Active Directory a musíte mít účet v tomto adresáři. Další, najdete v části toolearn [ověřování a uživatel oprávnění](analysis-services-manage-users.md).

## <a name="import-azurermanalysisservices-module"></a>Import modulu AzureRm.AnalysisServices
toocreate serveru v rámci vašeho předplatného, použijte hello [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) součást modulu. Načtení modulu AzureRm.AnalysisServices hello do relace prostředí PowerShell.

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="sign-in-tooazure"></a>Přihlaste se tooAzure

Přihlaste se pomocí hello tooyour předplatného Azure [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) příkaz. Postupujte podle hello na obrazovce pokynů.

```powershell
Add-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
 
[Skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) je logický kontejner, ve kterém se nasazují a spravují prostředky Azure jako skupina. Při vytváření serveru je potřeba zadat skupinu prostředků ve vašem předplatném. Pokud již jste skupinu prostředků, můžete vytvořit novou pomocí hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) příkaz. Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v oblasti západní USA hello.

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
```

## <a name="create-a-server"></a>Vytvoření serveru

Vytvoření nového serveru pomocí hello [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) příkaz. Hello následující příklad vytvoří server s názvem myServer ve myResourceGroup, v oblasti západní USA hello na úroveň hello D1 a určuje philipc@adventureworks.com jako správce serveru.

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myServer" -Location West US -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Hello server můžete odebrat ze svého předplatného pomocí hello [odebrat AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) příkaz. Pokud budete pokračovat dalšími rychlými starty a kurzy v této kolekci, server neodebírejte. Hello následující příklad odebere server hello vytvořili v předchozím kroku hello.


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myServer" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>Další kroky
[Správa služby Azure Analysis Services pomocí PowerShellu](analysis-services-powershell.md)   
[Nasazení modelu ze sady SSDT](analysis-services-deploy.md)   
[Vytvoření modelu na webu Azure Portal](analysis-services-create-model-portal.md)