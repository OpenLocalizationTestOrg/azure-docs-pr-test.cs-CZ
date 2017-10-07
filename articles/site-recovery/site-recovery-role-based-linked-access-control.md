---
title: "toomanage aaaUsing řízení přístupu na základě Role Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak tooapply a použít na základě rolí řízení přístupu (RBAC) toomanage nasazeních Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: manayar
ms.openlocfilehash: 7b721090351e561b28317ccdcf0ff283e0b146ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-role-based-access-control-toomanage-azure-site-recovery-deployments"></a>Použít nasazení s Azure Site Recovery toomanage řízení přístupu na základě Role

Řízení přístupu na základě role v Azure umožňuje přesnou správu přístupu. Pomocí RBAC, můžete v rámci týmu oddělit odpovědnosti a udělit pouze konkrétní přístup oprávnění toousers jako potřebné tooperform určité úlohy.

Azure Site Recovery poskytuje 3 předdefinované role toocontrol Site Recovery management operace. Další informace na [předdefinované role Azure RBAC](../active-directory/role-based-access-built-in-roles.md)

* [Lokality obnovení Přispěvatel](../active-directory/role-based-access-built-in-roles.md#site-recovery-contributor) – tato role má všechny operace Azure Site Recovery požadované toomanage oprávnění v trezoru služeb zotavení. S touto rolí uživatele však nelze vytvořit nebo odstranit trezor služeb zotavení nebo uživatelé s právy tooother přiřadit přístup. Tato role je nejvhodnější pro správce obnovení po havárii, kteří můžete povolit a spravovat zotavení po havárii aplikace nebo celé organizace, jako může být případ hello.
* [Operátor obnovení lokality](../active-directory/role-based-access-built-in-roles.md#site-recovery-operator) – tato role nemá oprávnění správce a tooexecute převzetí služeb při selhání a navrácení služeb po obnovení operace. Uživatel k této roli nelze povolit nebo zakázat replikaci, vytvořit nebo odstranit trezory, zaregistrujte novou infrastrukturu nebo přiřadit přístup uživatelé s právy tooother. Tato role je nejvhodnější pro operátor obnovení po havárii, kdo může převzetí služeb při selhání virtuálního počítače nebo přejít k podrobnostem aplikace při pokyn vlastníci aplikace a správci IT v situacích skutečná nebo simulované po havárii, jako je například zotavení po Havárii. POST rozlišení hello po havárii, operátor hello zotavení po Havárii můžete znovu nastavit ochranu a navrácení služeb po obnovení hello virtuálních počítačů.
* [Lokality obnovení čtečky](../active-directory/role-based-access-built-in-roles.md#site-recovery-reader) – tato role nemá oprávnění tooview všechny operace správy Site Recovery. Tato role je nejvhodnější pro monitorování vedení IT, který můžete sledovat hello aktuální stav ochrany a zvýšení lístky žádostí o podporu, pokud je to nutné.

Pokud se díváte toodefine vlastní role pro ještě větší kontrolu, najdete v části Jak příliš[vytvářet vlastní role](../active-directory/role-based-access-control-custom-roles.md) v Azure.

## <a name="permissions-required-tooenable-replication-for-new-virtual-machines"></a>TooEnable replikace požadovaná oprávnění pro nové virtuální počítače
Replikované tooAzure pomocí Azure Site Recovery po nový virtuální počítač úrovně přístupu hello přidruženého uživatele jsou ověřené tooensure, který hello uživatele hello vyžaduje oprávnění toouse hello prostředků Azure poskytuje tooSite obnovení.

tooenable replikace pro nový virtuální počítač, musí mít uživatel:
* Oprávnění toocreate virtuálního počítače v hello vybranou skupinou prostředků.
* Oprávnění toocreate virtuálního počítače v hello vybranou virtuální síť.
* Oprávnění toowrite toohello vybraný účet úložiště

Uživatel potřebuje hello následující oprávnění toocomplete replikaci nového virtuálního počítače.

> [!IMPORTANT]
>Zajistěte, aby odpovídající oprávnění jsou přidány na modelu nasazení hello (Resource Manager / Classic) používá pro nasazení prostředků.

| **Typ prostředku** | **Model nasazení** | **Oprávnění** |
| --- | --- | --- |
| Compute | Resource Manager | Microsoft.Compute/availabilitySets/read |
|  |  | Microsoft.Compute/virtualMachines/read |
|  |  | Microsoft.Compute/virtualMachines/write |
|  |  | Microsoft.Compute/virtualMachines/delete |
|  | Classic | Microsoft.ClassicCompute/domainNames/read |
|  |  | Microsoft.ClassicCompute/domainNames/write |
|  |  | Microsoft.ClassicCompute/domainNames/delete |
|  |  | Microsoft.ClassicCompute/virtualMachines/read |
|  |  | Microsoft.ClassicCompute/virtualMachines/write |
|  |  | Microsoft.ClassicCompute/virtualMachines/delete |
| Síť | Resource Manager | Microsoft.Network/networkInterfaces/read |
|  |  | Microsoft.Network/networkInterfaces/write |
|  |  | Microsoft.Network/networkInterfaces/delete |
|  |  | Microsoft.Network/networkInterfaces/join/action |
|  |  | Microsoft.Network/virtualNetworks/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/join/action |
|  | Classic | Microsoft.ClassicNetwork/virtualNetworks/read |
|  |  | Microsoft.ClassicNetwork/virtualNetworks/join/action |
| Úložiště | Resource Manager | Microsoft.Storage/storageAccounts/read |
|  |  | Microsoft.Storage/storageAccounts/listkeys/action |
|  | Classic | Microsoft.ClassicStorage/storageAccounts/read |
|  |  | Microsoft.ClassicStorage/storageAccounts/listKeys/action |
| Skupina prostředků | Resource Manager | Microsoft.Resources/deployments/* |
|  |  | Microsoft.Resources/subscriptions/resourceGroups/read |

Zvažte použití hello 'Přispěvatel virtuálních počítačů a Přispěvatel Classic virtuálních počítačů [předdefinované role](../active-directory/role-based-access-built-in-roles.md) pro nasazení Resource Manager a klasický modelů v uvedeném pořadí.

## <a name="next-steps"></a>Další kroky
* [Řízení přístupu na základě role](../active-directory/role-based-access-control-configure.md): Začínáme s RBAC v hello portálu Azure.
* Zjistěte, jak přistupovat k toomanage se:
  * [PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md)
  * [REST API](../active-directory/role-based-access-control-manage-access-rest.md)
* [Na základě rolí řešení potíží s řízení přístupu](../active-directory/role-based-access-control-troubleshooting.md): umožňuje získat návrhy pro řešení běžných problémů.
