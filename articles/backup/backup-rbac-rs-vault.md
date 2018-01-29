---
title: "Spravovat zálohy pomocí řízení přístupu na základě Role Azure | Microsoft Docs"
description: "Pomocí řízení přístupu na základě Role můžete spravovat přístup k zálohování management operace v trezoru služeb zotavení."
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 3bd46b97-4b29-47a5-b5ac-ac174dd36760
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/22/2017
ms.author: trinadhk;markgal
ms.openlocfilehash: b6e4c6761e1bd5c17c9c3428491113042d3b1d31
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-role-based-access-control-to-manage-azure-backup-recovery-points"></a>Pomocí řízení přístupu na základě Role můžete spravovat body obnovení Azure Backup
Řízení přístupu na základě role v Azure umožňuje přesnou správu přístupu. Pomocí RBAC, můžete v rámci týmu oddělit povinností a poskytnout pouze takovou úroveň přístupu pro uživatele, kteří potřebují k provádění svých úloh.

> [!IMPORTANT]
> Role, které poskytuje Azure Backup jsou omezeny na akce, které lze provést na portálu Azure nebo rutin prostředí PowerShell trezor služeb zotavení. Akce prováděné v Azure zálohování uživatelské rozhraní agenta klienta nebo System center Data Protection Manager uživatelského rozhraní nebo uživatelské rozhraní serveru služby zálohování Azure jsou mimo kontrolu nad tyto role.

Azure Backup poskytuje 3 předdefinovaných rolí k řízení operace zálohování správy. Další informace na [předdefinované role Azure RBAC](../active-directory/role-based-access-built-in-roles.md)

* [Zálohování Přispěvatel](../active-directory/role-based-access-built-in-roles.md#backup-contributor) – tato role má všechna oprávnění k vytváření a správě zálohování, vyjma případu vytvoření trezoru služeb zotavení a udělíte přístup k ostatním. Představte si tuto roli jako správce zálohování správy, který můžete provést každé operace zálohování správy.
* [Operátor zálohování](../active-directory/role-based-access-built-in-roles.md#backup-operator) – tato role má oprávnění ke všem souborům Přispěvatel s výjimkou odebrání zásady zálohování pro zálohování a správa. Tato role je stejná jako Přispěvatel, s výjimkou destruktivní operace ji nemůžete provést, například při zastavení zálohování s odstranit data nebo odebrat registraci místních prostředků.
* [Zálohování čtečky](../active-directory/role-based-access-built-in-roles.md#backup-reader) – tato role má oprávnění k zobrazení všechny operace zálohování správy. Představte si této role, která bude monitorování osoby.

Pokud hledáte, můžete definovat vlastní role pro ještě větší kontrolu, přečtěte si postup [vytvářet vlastní role](../active-directory/role-based-access-control-custom-roles.md) v Azure RBAC.



## <a name="mapping-backup-built-in-roles-to-backup-management-actions"></a>Mapování zálohování předdefinované role pro správu záloh akce
V následující tabulce zaznamená akce správy zálohování a odpovídající minimální role RBAC nutné k provedení této operace.

| Operace správy | Vyžaduje minimální role RBAC |
| --- | --- |
| Vytvoření trezoru služeb zotavení | Přispěvatel na skupinu prostředků úložiště |
| Povolení zálohování virtuálních počítačů Azure | Operátor zálohování na trezor, Přispěvatel virtuálních počítačů na virtuálních počítačích |
| Zálohování na vyžádání virtuálního počítače | Operátor zálohování |
| Obnovení virtuálního počítače | Operátor zálohování, Přispěvatel skupiny prostředků, ve kterém se bude virtuální počítač a virtuální sítě na nasadí |
| Obnovení disky, jednotlivé soubory ze zálohy virtuálního počítače | Operátor zálohování, Přispěvatel virtuálních počítačů na virtuálních počítačích |
| Vytvořit zásady zálohování pro zálohování virtuálních počítačů Azure | Zálohování přispěvatele |
| Upravit zásady zálohování zálohy virtuálního počítače Azure | Zálohování přispěvatele |
| Odstranit zásady zálohování zálohy virtuálního počítače Azure | Zálohování přispěvatele |
| Zastavení zálohování (při zachování dat nebo odstranit data) na zálohování virtuálních počítačů | Zálohování přispěvatele |
| Zaregistrovat místní Windows serveru nebo klienta nebo SCDPM nebo Azure Backup Server | Operátor zálohování |
| Odstranit zaregistrovaná místně Windows serveru nebo klienta nebo SCDPM nebo Azure Backup Server | Zálohování přispěvatele |

## <a name="next-steps"></a>Další kroky
* [Řízení přístupu na základě role](../active-directory/role-based-access-control-configure.md): Začínáme s RBAC na portálu Azure.
* Zjistěte, jak spravovat přístup pomocí:
  * [PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md)
  * [REST API](../active-directory/role-based-access-control-manage-access-rest.md)
* [Na základě rolí řešení potíží s řízení přístupu](../active-directory/role-based-access-control-troubleshooting.md): umožňuje získat návrhy pro řešení běžných problémů.
