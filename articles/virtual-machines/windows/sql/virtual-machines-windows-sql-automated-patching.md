---
title: "aaaAutomated opravy pro virtuální počítače serveru SQL (Resource Manager) | Microsoft Docs"
description: "Vysvětluje funkci hello automatizované opravy pro SQL Server virtuální počítače běžící v Azure pomocí Správce prostředků."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 58232e92-318f-456b-8f0a-2201a541e08d
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 8bb8d0fb265e69d7bbf1fa047f5ceef02e7c56fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automatizované opravy pro SQL Server v Azure Virtual Machines (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-automated-patching.md)
> * [Classic](../classic/sql-automated-patching.md)
> 
> 

Automatizovaných oprav určuje časové období údržby pro virtuální počítač Azure systémem SQL Server. Automatické aktualizace lze nainstalovat pouze během tohoto časového období údržby. Pro systém SQL Server tento rescriction zajistí, že aktualizace systému a všechny přidružené restartuje dojít na nejlepší možný čas hello hello databáze. Automatizovaných oprav závisí na hello [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

tooview hello classic verze tohoto článku, najdete v části [automatizované opravy pro SQL Server v Azure virtuální počítače Classic](../classic/sql-automated-patching.md).

## <a name="prerequisites"></a>Požadavky
toouse automatizované opravy, vezměte v úvahu hello následující požadavky:

**Operační systém**:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**Verze systému SQL Server**:

* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

**Prostředí Azure PowerShell**:

* [Nainstalujte nejnovější příkazy prostředí Azure PowerShell hello](/powershell/azure/overview) Pokud máte v plánu tooconfigure automatizovaných oprav pomocí prostředí PowerShell.

> [!NOTE]
> Automatizovaných oprav spoléhá na hello rozšíření IaaS agenta systému SQL Server. Aktuální SQL bitové kopie virtuálních počítačů Galerie přidejte toto rozšíření ve výchozím nastavení. Další informace najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).
> 
> 

## <a name="settings"></a>Nastavení
Hello následující tabulka popisuje možnosti hello, které mohou být konfigurovány pro automatizované opravy. kroky skutečné konfigurace Hello lišit v závislosti na tom, zda používáte hello portál Azure nebo příkazů prostředí Windows PowerShell pro Azure.

| Nastavení | Možné hodnoty | Popis |
| --- | --- | --- |
| **Automatizované opravy** |Povolí nebo zakáže (zakázáno) |Povolí nebo zakáže automatizované opravy pro virtuální počítač Azure. |
| **Plán údržby.** |Každý den, pondělí, úterý, středu, čtvrtek a pátek, sobota, neděle |Hello plán pro stahování a instalace aktualizací s Windows, SQL Server a Microsoft pro virtuální počítač. |
| **Hodina spouštění údržby** |0-24 |Hello místní počáteční čas tooupdate hello virtuálního počítače. |
| **Doba trvání okna údržby** |30-180 |Hello počet minut povolené toocomplete hello stažení a instalace aktualizací. |
| **Oprava kategorie** |Důležité |Hello kategorie aktualizace toodownload a instalaci. |

## <a name="configuration-in-hello-portal"></a>Konfigurace v hello portálu
Můžete použít hello Azure portálu tooconfigure automatizovaných oprav při zřizování nebo pro existující virtuální počítače.

### <a name="new-vms"></a>Nové virtuální počítače
Použití hello Azure portálu tooconfigure automatizovaných oprav při vytváření nového virtuálního počítače SQL serveru v modelu nasazení Resource Manager hello.

V hello **nastavení systému SQL Server** vyberte **automatizované opravy**. Hello následující Azure portálu snímek obrazovky ukazuje hello **automatizovaných oprav SQL** okno.

![Automatizovaných oprav SQL na portálu Azure](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Kontext, tématu hello dokončení na [zřizování virtuálního počítače s SQL serverem v Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Existující virtuální počítače
Pro existující virtuální počítače systému SQL Server vyberte virtuální počítač systému SQL Server. Potom vyberte hello **konfigurace systému SQL Server** části hello **nastavení** okno.

![Automatické opravy SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

V hello **konfigurace systému SQL Server** okně klikněte na tlačítko hello **upravit** tlačítka na hello automatizovaných oprav části.

![Konfigurace automatizovaných oprav SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

Po dokončení klikněte na tlačítko hello **OK** na konci hello hello tlačítko **konfigurace systému SQL Server** okno toosave změny.

Chcete-li povolit automatizované opravy pro hello poprvé, nakonfiguruje Azure hello IaaS agenta systému SQL Server hello pozadí. Během této doby nemusí zobrazit hello portálu Azure, je nakonfigurován automatizovaných oprav. Počkejte několik minut, než toobe hello agenta nainstalovat, nakonfigurovat. Po této hello Azure portal odráží hello nové nastavení.

> [!NOTE]
> Můžete také nakonfigurovat automatizovaných oprav pomocí šablony. Další informace najdete v tématu [šablony Azure rychlý start pro automatizované opravy](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).
> 
> 

## <a name="configuration-with-powershell"></a>Konfigurace pomocí prostředí PowerShell
Po zřízení virtuálního počítače SQL, pomocí prostředí PowerShell tooconfigure automatizovaných oprav.

V následujícím příkladu hello, prostředí PowerShell je použité tooconfigure automatizovaných oprav na existující virtuální počítač serveru SQL. Hello **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig** příkaz nakonfiguruje nové okno údržby pro automatické aktualizace.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

Podle toho, v tomto příkladu, hello následující tabulka popisuje hello praktické vliv na hello cílový virtuální počítač Azure:

| Parametr | Efekt |
| --- | --- |
| **DayOfWeek** |Každý čtvrtek nainstalovány opravy. |
| **MaintenanceWindowStartingHour** |Začátek aktualizace na 11:00. |
| **MaintenanceWindowsDuration** |Během 120 minut musí být nainstalované opravy. Podle času zahájení hello, musí provést podle 1:00 pm. |
| **PatchCategory** |Hello jedinou možnou nastavení pro tento parametr je **důležité**. |

Ho může trvat několik minut tooinstall a nakonfigurujte hello IaaS agenta systému SQL Server.

toodisable automatizovaných oprav spuštění hello stejný skript bez hello **-povolit** parametr toohello **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig**. Hello absenci hello **-povolit** parametr signály hello příkaz toodisable hello funkce.

## <a name="next-steps"></a>Další kroky
Informace o dalších úlohách, k dispozici automation najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).

