---
title: "aaaAutomated opravy pro virtuální počítače serveru SQL (klasické) | Microsoft Docs"
description: "Vysvětluje funkci hello automatizované opravy pro SQL Server virtuální počítače běžící v Azure pomocí režimu hello nasazení classic."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 737b2f65-08b9-4f54-b867-e987730265a8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 2ef06b95705fc457605d6eb2fbc0afd490321843
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Automatizované opravy pro SQL Server na virtuálních počítačích Azure (klasický)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-automated-patching.md)
> * [Classic](../classic/sql-automated-patching.md)
> 
> 

Automatizovaných oprav určuje časové období údržby pro virtuální počítač Azure systémem SQL Server. Automatické aktualizace lze nainstalovat pouze během tohoto časového období údržby. Pro systém SQL Server tím se zajistí, že aktualizace systému a všechny přidružené restartuje dojít na nejlepší možný čas hello hello databáze. Automatizovaných oprav závisí na hello [rozšíření agenta systému SQL Server IaaS](../classic/sql-server-agent-extension.md).

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. tooview hello Resource Manager verze tohoto článku, najdete v části [automatizované opravy pro SQL Server ve službě Správce prostředků virtuálních počítačů Azure](../sql/virtual-machines-windows-sql-automated-patching.md).

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

* [Nainstalujte nejnovější příkazy prostředí Azure PowerShell hello](/powershell/azure/overview).

**Rozšíření systému SQL Server IaaS**:

* [Nainstalujte hello rozšíření systému SQL Server IaaS](../classic/sql-server-agent-extension.md).

## <a name="settings"></a>Nastavení
Hello následující tabulka popisuje možnosti hello, které mohou být konfigurovány pro automatizované opravy. Pro klasické virtuální počítače je nutné použít PowerShell tooconfigure těchto nastavení.

| Nastavení | Možné hodnoty | Popis |
| --- | --- | --- |
| **Automatizované opravy** |Povolí nebo zakáže (zakázáno) |Povolí nebo zakáže automatizované opravy pro virtuální počítač Azure. |
| **Plán údržby.** |Každý den, pondělí, úterý, středu, čtvrtek a pátek, sobota, neděle |Hello plán pro stahování a instalace aktualizací s Windows, SQL Server a Microsoft pro virtuální počítač. |
| **Hodina spouštění údržby** |0-24 |Hello místní počáteční čas tooupdate hello virtuálního počítače. |
| **Doba trvání okna údržby** |30-180 |Hello počet minut povolené toocomplete hello stažení a instalace aktualizací. |
| **Oprava kategorie** |Důležité |Hello kategorie aktualizace toodownload a instalaci. |

## <a name="configuration-with-powershell"></a>Konfigurace pomocí prostředí PowerShell
V následujícím příkladu hello, prostředí PowerShell je použité tooconfigure automatizovaných oprav na existující virtuální počítač serveru SQL. Hello **New-AzureVMSqlServerAutoPatchingConfig** příkaz nakonfiguruje nové okno údržby pro automatické aktualizace.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

Podle toho, v tomto příkladu, hello následující tabulka popisuje hello praktické vliv na hello cílový virtuální počítač Azure:

| Parametr | Efekt |
| --- | --- |
| **DayOfWeek** |Každý čtvrtek nainstalovány opravy. |
| **MaintenanceWindowStartingHour** |Začátek aktualizace na 11:00. |
| **MaintenanceWindowsDuration** |Během 120 minut musí být nainstalované opravy. Podle času zahájení hello, musí provést podle 1:00 pm. |
| **PatchCategory** |Hello pouze možné nastavení pro tento parametr je "Důležité". |

Ho může trvat několik minut tooinstall a nakonfigurujte hello IaaS agenta systému SQL Server.

toodisable automatizovaných oprav spuštění hello stejný skript bez hello - povolit parametr toohello AzureVMSqlServerAutoPatchingConfig nový. Jak instalaci, může trvat několik minut toodisable automatizovaných oprav.

## <a name="next-steps"></a>Další kroky
Informace o dalších úlohách, k dispozici automation najdete v tématu [rozšíření agenta systému SQL Server IaaS](../classic/sql-server-agent-extension.md).

Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

