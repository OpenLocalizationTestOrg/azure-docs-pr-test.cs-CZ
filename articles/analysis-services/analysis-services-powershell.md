---
title: "aaaManage Azure Analysis Services pomocí prostředí PowerShell | Microsoft Docs"
description: "Správa Azure Analysis Services pomocí prostředí PowerShell."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: owend
ms.openlocfilehash: bc4250bf77b5a0d86c1049ee57493bcf2a1f0c1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Správa služby Azure Analysis Services pomocí prostředí PowerShell

Tento článek popisuje prostředí PowerShell rutiny používané tooperform Azure Analysis Services server a databáze úlohy správy. 

Úlohy správy serveru jako je například vytváření nebo odstraňování serveru, pozastavení nebo obnovení operací serveru nebo změna úrovně služby hello (vrstvy) použít rutiny Azure Resource Manager (AzureRM). Další úlohy pro správu databází, jako je přidání nebo odebrání členy role, zpracování nebo rozdělení do oddílů pomocí rutiny součástí hello stejný modul SQL Server jako SQL Server Analysis Services.

## <a name="permissions"></a>Oprávnění
Většinu úloh prostředí PowerShell vyžadují, že abyste měli oprávnění správce na serveru služby Analysis Services hello, které spravujete. Naplánované úlohy prostředí PowerShell jsou bezobslužné operace. Hello účet, který spouští hello scheduler musí mít oprávnění správce na serveru služby Analysis Services hello. 

Pro operace serveru pomocí rutin AzureRm, účet nebo hello účet, který spouští scheduler musí patřit také toohello roli vlastníka pro prostředek hello v [řízení řízení přístupu (RBAC)](../active-directory/role-based-access-control-what-is.md). 

## <a name="server-operations"></a>Operace serveru 
Rutiny Azure Analysis Services jsou součástí hello [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) součást modulu. moduly rutin AzureRM tooinstall, najdete v části [rutiny Azure Resource Manager](/powershell/azure/overview) v hello Galerie prostředí PowerShell.

|Rutina|Popis| 
|------------|-----------------| 
|[Export-AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/export-azureanalysisservicesinstancelog)|Export protokolu toofile.| 
|[Get-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|Získá podrobnosti instance serveru.|  
|[Nové AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|Vytvoří instanci serveru.|
|[Odebrat AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|Odebere instanci serveru.|  
|[Pozastavit AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|Pozastaví instanci serveru.| 
|[Resume-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|Obnoví instanci serveru.|  
|[Set-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|Upravuje instanci serveru.|   
|[Test AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|Testy existence hello instance serveru.| 

## <a name="database-operations"></a>Operace databáze

Použití databázových operací Azure Analysis Services hello stejné [SqlServer](https://www.powershellgallery.com/packages/SqlServer) modulu jako SQL Server Analysis Services. Ne všechny rutiny jsou však podporovány pro Azure Analysis Services. 

modul SQL Server Hello poskytuje rutiny pro správu databáze specifických úkolů a taky hello obecné účely Invoke-ASCmd rutiny, která přijímá tabulkový Model skriptovací jazyk (TMSL) dotazu nebo skriptu. Hello následující rutiny v modulu hello SQL Server jsou podporovány pro Azure Analysis Services.

  
|Rutina|Popis|
|------------|-----------------| 
|[Přidat RoleMember](https://msdn.microsoft.com/library/hh510167.aspx)|Přidání role databáze tooa člen.| 
|[Zálohování ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet)|Zálohování databáze služby Analysis Services.|  
|[Odebrat RoleMember](https://msdn.microsoft.com/library/hh510173.aspx)|Odebrání člena z databázové role.|   
|[Vyvolání ASCmd](https://msdn.microsoft.com/library/hh479579.aspx)|Spuštění skriptu TMSL.|
|[Vyvolání ProcessASDatabase](https://msdn.microsoft.com/library/mt651773.aspx)|Zpracování databáze.|  
|[Vyvolání ProcessPartition](https://msdn.microsoft.com/library/hh510164.aspx)|Zpracování oddílu.| 
|[Vyvolání ProcessTable](https://msdn.microsoft.com/library/mt651774.aspx)|Proces tabulku.|  
|[Sloučení oddílů](https://msdn.microsoft.com/library/hh479576.aspx)|Sloučit oddíl.|  
|[Obnovení ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet)|Obnovte databázi služby Analysis Services.| 
  

## <a name="related-information"></a>Související informace

* [Stáhnout modul prostředí PowerShell serveru SQL](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Stažení aplikace SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [Modul SQL Server v galerii prostředí PowerShell](https://www.powershellgallery.com/packages/SqlServer)    
* [Tabulkový Model programování pro 1200 úroveň kompatibility a vyšší](https://msdn.microsoft.com/library/mt712541.aspx)
