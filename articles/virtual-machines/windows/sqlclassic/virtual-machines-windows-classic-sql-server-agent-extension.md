---
title: "úlohy správy aaaAutomate na virtuálních počítačích SQL (klasické) | Microsoft Docs"
description: "Toto téma popisuje, jak toomanage hello přípony agenta systému SQL Server, který automatizuje konkrétní úlohy správy systému SQL Server. Mezi ně patří automatizovaného zálohování, automatizovaných oprav a Azure Key Vault integrace. Toto téma používá režim hello nasazení classic."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a9bda2e7-cdba-427c-bc30-77cde4376f3a
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1dc011e0526845701eaf0c365007938f9ee32ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-hello-sql-server-agent-extension-classic"></a>Automatizaci úloh správy ve virtuálních počítačích Azure s hello rozšíření systému SQL Server Agent (klasické)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md)
> * [Classic](../classic/sql-server-agent-extension.md)
> 
>
 
Hello IaaS agenta rozšíření (SQLIaaSAgent) systému SQL Server spouští na úlohy správy tooautomate virtuální počítače Azure. Toto téma obsahuje přehled služby hello podporované hello rozšíření, jakož i pokyny k instalaci, stavu a odebrání.

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. tooview hello Resource Manager verze tohoto článku, najdete v části [rozšíření agenta systému SQL Server pro SQL Server virtuální počítače Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md).

## <a name="supported-services"></a>Podporované služby
Hello rozšíření IaaS agenta systému SQL Server podporuje hello následující úlohy správy:

| Funkce správy | Popis |
| --- | --- |
| **Automatizované zálohování SQL** |Automatizuje hello plánování zálohování pro všechny databáze hello výchozí instanci systému SQL Server v hello virtuálních počítačů. Další informace najdete v tématu [automatizované zálohování pro SQL Server v Azure Virtual Machines (klasické)](../classic/sql-automated-backup.md). |
| **Automatizované opravy pro SQL** |Konfiguraci časového období údržby, během které aktualizace umístit tooyour, může trvat virtuálních počítačů, tak, aby se můžete vyhnout aktualizace během špiček pro úlohy. Další informace najdete v tématu [automatizované opravy pro SQL Server v Azure Virtual Machines (klasické)](../classic/sql-automated-patching.md). |
| **Integrace se službou Azure Key Vault** |Umožňuje vám tooautomatically instalace a konfigurace Azure Key Vault na virtuální počítač s SQL serverem. Další informace najdete v tématu [nakonfigurovat klíč trezoru integrace se službou Azure pro systém SQL Server na virtuálních počítačích Azure (klasický)](../classic/ps-sql-keyvault.md). |

## <a name="prerequisites"></a>Požadavky
Požadavky na toouse hello rozšíření IaaS agenta systému SQL Server na vašem virtuálním počítači:

### <a name="operating-system"></a>Operační systém:
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

### <a name="sql-server-versions"></a>Verze systému SQL Server:
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

### <a name="azure-powershell"></a>Azure PowerShell:
[Stáhnout a nakonfigurovat Azure PowerShell příkazy nejnovější hello](/powershell/azure/overview).

Spusťte prostředí Windows PowerShell a připojte ho tooyour předplatného Azure s hello **Add-AzureAccount** příkaz.

    Add-AzureAccount

Pokud máte více předplatných, použijte **Select-AzureSubscription** tooselect hello předplatné, které obsahuje cílový classic virtuálních počítačů.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

V tomto okamžiku můžete získat seznam hello klasické virtuální počítače a jejich názvy přidružená služba s hello **Get-AzureVM** příkaz.

    Get-AzureVM

## <a name="installation"></a>Instalace
Pro klasické virtuální počítače musíte použít PowerShell tooinstall hello rozšíření agenta systému SQL Server IaaS a nakonfigurovat jejích přidružených služeb. Použití hello **Set-AzureVMSqlServerExtension** rozšíření hello tooinstall rutiny prostředí PowerShell. Hello následující příkaz například nainstaluje hello rozšíření na virtuálním počítači Windows serveru (klasické) a pojmenuje ji "SQLIaaSExtension".

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

Pokud aktualizujete toohello nejnovější verzi hello rozšíření agenta systému SQL IaaS, je nutné restartovat virtuální počítač po aktualizaci hello rozšíření.

> [!NOTE]
> Klasické virtuální počítače nemají možnost tooinstall a nakonfigurovat hello rozšíření agenta systému SQL IaaS prostřednictvím portálu hello.
> 
> 

## <a name="status"></a>Status
Jedním ze způsobů tooverify nainstalovanou hello rozšíření je stav agenta hello tooview hello portálu Azure. Vyberte virtuální počítač uvedený v okně hello virtuálního počítače a potom klikněte na **rozšíření**. Měli byste vidět hello **SQLIaaSAgent** rozšíření uvedené.

![Rozšíření IaaS agenta systému SQL Server na portálu Azure](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

Můžete taky hello **Get-AzureVMSqlServerExtension** rutiny Azure Powershellu.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Odebrání
V hello portálu Azure, můžete odinstalovat rozšíření hello kliknutím hello třemi tečkami na hello **rozšíření** okno vaší vlastnosti virtuálního počítače. Pak klikněte na tlačítko **odinstalovat**.

![Odinstalujte hello rozšíření IaaS agenta systému SQL Server na portálu Azure](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

Můžete taky hello **odebrat AzureVMSqlServerExtension** rutiny prostředí Powershell.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Další kroky
Začnete používat jednu z hello služby podporované hello rozšíření. Další podrobnosti najdete v tématu hello témata, kterou se odkazuje v hello [podporované služby](#supported-services) tohoto článku.

Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

