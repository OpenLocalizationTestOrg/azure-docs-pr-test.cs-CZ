---
title: "Spravovat zálohy pomocí řízení přístupu na základě Role Azure | Microsoft Docs"
description: "Pomocí operace správy toobackup přístup toomanage řízení přístupu na základě Role v trezoru služeb zotavení."
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
ms.openlocfilehash: 26d034d152f9b77fc6d5b2ffd5ef2648b1797f46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-role-based-access-control-toomanage-azure-backup-recovery-points"></a>Použít body obnovení Azure Backup toomanage řízení přístupu na základě Role
Řízení přístupu na základě role v Azure umožňuje přesnou správu přístupu. Pomocí RBAC, můžete v rámci týmu oddělit povinností a udělit pouze hello množství toousers přístup, že potřebují tooperform svou práci.

> [!IMPORTANT]
> Role, které poskytuje Azure Backup jsou omezené tooactions, které lze provést na portálu Azure nebo rutin prostředí PowerShell trezor služeb zotavení. Akce prováděné v Azure zálohování uživatelské rozhraní agenta klienta nebo System center Data Protection Manager uživatelského rozhraní nebo uživatelské rozhraní serveru služby zálohování Azure jsou mimo kontrolu nad tyto role.

Azure Backup poskytuje 3 předdefinované role toocontrol operace zálohování správy. Další informace na [předdefinované role Azure RBAC](../active-directory/role-based-access-built-in-roles.md)

* [Zálohování Přispěvatel](../active-directory/role-based-access-built-in-roles.md#backup-contributor) – tato role má všechny toocreate oprávnění a Správa zálohování kromě vytvoření trezoru služeb zotavení a poskytnete tooothers přístup. Představte si tuto roli jako správce zálohování správy, který můžete provést každé operace zálohování správy.
* [Operátor zálohování](../active-directory/role-based-access-built-in-roles.md#backup-operator) – tato role nemá oprávnění tooeverything Přispěvatel s výjimkou odebrání zálohování a Správa zásady zálohování. Tato role je ekvivalentní toocontributor, s výjimkou nemůže provádět destruktivní operace, jako je třeba zastavit zálohování s odstranit data nebo odebrat registraci místních prostředků.
* [Zálohování čtečky](../active-directory/role-based-access-built-in-roles.md#backup-reader) – tato role nemá oprávnění tooview všechny operace zálohování správy. Představte si tuto roli toobe monitorování osoby.

Pokud se díváte toodefine vlastní role pro ještě větší kontrolu, najdete v části Jak příliš[vytvářet vlastní role](../active-directory/role-based-access-control-custom-roles.md) v Azure RBAC.



## <a name="mapping-backup-built-in-roles-toobackup-management-actions"></a>Mapování akce správy toobackup předdefinované role zálohování
Následující tabulka Hello zaznamená akce správy zálohování hello a odpovídající minimální role RBAC požadované tooperform tuto operaci.

| Operace správy | Vyžaduje minimální role RBAC |
| --- | --- |
| Vytvoření trezoru služeb zotavení | Přispěvatel na skupinu prostředků úložiště |
| Povolení zálohování virtuálních počítačů Azure | Operátor zálohování na trezor, Přispěvatel virtuálních počítačů na virtuálních počítačích |
| Zálohování na vyžádání virtuálního počítače | Operátor zálohování |
| Obnovení virtuálního počítače | Operátor zálohování, Přispěvatel skupiny prostředků, ve kterém se bude virtuální počítač a virtuální sítě tooget nasazení |
| Obnovení disky, jednotlivé soubory ze zálohy virtuálního počítače | Operátor zálohování |
| Vytvořit zásady zálohování pro zálohování virtuálních počítačů Azure | Zálohování přispěvatele |
| Upravit zásady zálohování zálohy virtuálního počítače Azure | Zálohování přispěvatele |
| Odstranit zásady zálohování zálohy virtuálního počítače Azure | Zálohování přispěvatele |
| Zastavení zálohování (při zachování dat nebo odstranit data) na zálohování virtuálních počítačů | Zálohování přispěvatele |
| Zaregistrovat místní Windows serveru nebo klienta nebo SCDPM nebo Azure Backup Server | Operátor zálohování |
| Odstranit zaregistrovaná místně Windows serveru nebo klienta nebo SCDPM nebo Azure Backup Server | Zálohování přispěvatele |

## <a name="next-steps"></a>Další kroky
* [Řízení přístupu na základě role](../active-directory/role-based-access-control-configure.md): Začínáme s RBAC v hello portálu Azure.
* Zjistěte, jak přistupovat k toomanage se:
  * [PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md)
  * [REST API](../active-directory/role-based-access-control-manage-access-rest.md)
* [Na základě rolí řešení potíží s řízení přístupu](../active-directory/role-based-access-control-troubleshooting.md): umožňuje získat návrhy pro řešení běžných problémů.
