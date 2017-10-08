---
title: "úlohy správy aaaAutomate na virtuálních počítačích SQL (Resource Manager) | Microsoft Docs"
description: "Toto téma popisuje, jak toomanage hello přípony agenta systému SQL Server, který automatizuje konkrétní úlohy správy systému SQL Server. Mezi ně patří automatizovaného zálohování, automatizovaných oprav a Azure Key Vault integrace. Toto téma používá hello režimu nasazení Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ae917612c4af59f12c0b083440673bdc555e9d56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-hello-sql-server-agent-extension-resource-manager"></a>Automatizaci úloh správy ve virtuálních počítačích Azure s hello rozšíření systému SQL Server Agent (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Classic](../classic/sql-server-agent-extension.md)
> 
> 

Hello IaaS agenta rozšíření (SQLIaaSExtension) systému SQL Server spouští na úlohy správy tooautomate virtuální počítače Azure. Toto téma obsahuje přehled služby hello podporované hello rozšíření, jakož i pokyny k instalaci, stavu a odebrání.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

tooview hello classic verze tohoto článku, najdete v části [rozšíření agenta systému SQL Server pro SQL Server virtuální počítače Classic](../classic/sql-server-agent-extension.md).

## <a name="supported-services"></a>Podporované služby
Hello rozšíření IaaS agenta systému SQL Server podporuje hello následující úlohy správy:

| Funkce správy | Popis |
| --- | --- |
| **Automatizované zálohování SQL** |Automatizuje hello plánování zálohování pro všechny databáze hello výchozí instanci systému SQL Server v hello virtuálních počítačů. Další informace najdete v tématu [automatizované zálohování pro SQL Server v Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md). |
| **Automatizované opravy pro SQL** |Konfiguraci časového období údržby, během které aktualizace umístit tooyour, může trvat virtuálních počítačů, tak, aby se můžete vyhnout aktualizace během špiček pro úlohy. Další informace najdete v tématu [automatizované opravy pro SQL Server v Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md). |
| **Integrace se službou Azure Key Vault** |Umožňuje vám tooautomatically instalace a konfigurace Azure Key Vault na virtuální počítač s SQL serverem. Další informace najdete v tématu [nakonfigurovat klíč trezoru integrace se službou Azure pro systém SQL Server na virtuálních počítačích Azure (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md). |

Po dokončení instalace a spuštění, hello rozšíření agenta systému SQL Server IaaS zpřístupní tyto funkce pro správu na panelu systému SQL Server hello hello virtuálního počítače v hello portálu Azure a pomocí prostředí Azure PowerShell pro bitové kopie systému SQL Server marketplace a prostřednictvím Chcete-li prostředí Azure PowerShell pro ruční instalací rozšíření hello. 

## <a name="prerequisites"></a>Požadavky
Požadavky na toouse hello rozšíření IaaS agenta systému SQL Server na vašem virtuálním počítači:

**Operační systém**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**Verze systému SQL Server**:

* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

**Prostředí Azure PowerShell**:

* [Stáhnout a nakonfigurovat Azure PowerShell příkazy nejnovější hello](/powershell/azure/overview)

## <a name="installation"></a>Instalace
Hello rozšíření IaaS agenta systému SQL Server se automaticky nainstaluje při zřizování hello bitové kopie Galerie virtuálních počítačů systému SQL Server. Pokud potřebujete tooreinstall hello rozšíření na jednu z těchto virtuálních počítačích SQL Server ručně, použijte následující příkaz prostředí PowerShell hello:

```powershell
Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2" -Location "East US 2"
```

Je také možné tooinstall hello rozšíření IaaS agenta systému SQL Server na virtuálním počítači jen operačního systému Windows Server. Je podporováno pouze pokud jste nainstalovali také ručně SQL Server na tomto počítači. Pak instalace rozšíření hello ručně pomocí hello stejné **Set-AzureVMSqlServerExtension** rutiny prostředí PowerShell.

> [!NOTE]
> Pokud nainstalujete ručně hello rozšíření IaaS agenta systému SQL Server na jen operačního systému Windows Server virtuální počítač, se nedají spravovat nastavení konfigurace systému SQL Server hello prostřednictvím hello portálu Azure. V tomto scénáři je nutné provést všechny změny v prostředí PowerShell.

## <a name="status"></a>Status
Jedním ze způsobů tooverify nainstalovanou hello rozšíření je stav agenta hello tooview hello portálu Azure. Vyberte **všechna nastavení** v hello okno virtuálního počítače a potom klikněte na **rozšíření**. Měli byste vidět hello **SQLIaaSExtension** rozšíření uvedené.

![Rozšíření IaaS agenta systému SQL Server na portálu Azure](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Můžete taky hello **Get-AzureVMSqlServerExtension** rutiny Azure Powershellu.

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

předchozí příkaz Hello potvrdí hello agenta je nainstalován a poskytuje obecné informace stavu. Také můžete získat informace o automatizované zálohování a oprav konkrétní stavu s hello následující příkazy.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Odebrání
V hello portálu Azure, můžete odinstalovat rozšíření hello kliknutím hello třemi tečkami na hello **rozšíření** okno vaší vlastnosti virtuálního počítače. Pak klikněte na tlačítko **odstranit**.

![Odinstalujte hello rozšíření IaaS agenta systému SQL Server na portálu Azure](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Můžete taky hello **odebrat AzureRmVMSqlServerExtension** rutiny prostředí Powershell.

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Další kroky
Začnete používat jednu z hello služby podporované hello rozšíření. Další podrobnosti najdete v tématu hello témata, kterou se odkazuje v hello [podporované služby](#supported-services) tohoto článku.

Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).

