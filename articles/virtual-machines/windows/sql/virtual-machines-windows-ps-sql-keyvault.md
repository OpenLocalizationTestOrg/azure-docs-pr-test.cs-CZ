---
title: "aaaIntegrate Key Vault se systémem SQL Server na virtuálních počítačích Windows v Azure (Resource Manager) | Microsoft Docs"
description: "Zjistěte, jak tooautomate hello konfigurace systému SQL Server šifrování pro použití s Azure Key Vault. Toto téma vysvětluje, jak vytvořit toouse Azure Key Vault integrace s virtuálními počítači systému SQL Server pomocí Správce prostředků."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: cd66dfb1-0e9b-4fb0-a471-9deaf4ab4ab8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/23/2017
ms.author: jroth
ms.openlocfilehash: 0d36d3d075d6538c18cd5ecb43c19a4b000a99e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-resource-manager"></a>Konfigurace integrace Azure Key Vault pro SQL Server na virtuálních počítačích Azure (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-ps-sql-keyvault.md)
> * [Classic](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a>Přehled
Existuje více funkcí šifrování systému SQL Server, například [transparentní šifrování dat (šifrování TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [šifrování na úrovni sloupce (Vymazat)](https://msdn.microsoft.com/library/ms173744.aspx), a [zálohu šifrovacího](https://msdn.microsoft.com/library/dn449489.aspx). Tyto formuláře šifrování vyžadovat toomanage a uložení hello kryptografických klíčů, které používáte pro šifrování. Hello službou Azure Key Vault (AZURE) služby je navrženou tooimprove hello zabezpečení a správu tyto klíče v umístění zabezpečené a vysoce dostupné. Hello [konektor služby serveru SQL](http://www.microsoft.com/download/details.aspx?id=45344) umožňuje tyto klíče z Azure Key Vault toouse systému SQL Server.

Pokud jste s místním systémem SQL Server počítačů existuje jsou [kroky, které vám pomůžou tooaccess Azure Key Vault z vašeho místního počítače systému SQL Server](https://msdn.microsoft.com/library/dn198405.aspx). Ale pro SQL Server ve virtuálních počítačích Azure, můžete ušetřit čas pomocí hello *Azure Key Vault integrace* funkce.

Pokud je tato funkce povolena, automaticky se nainstaluje hello konektor služby serveru SQL, nakonfiguruje hello EKM provider tooaccess Azure Key Vault a vytvoří tooallow hello přihlašovacích údajů můžete tooaccess svůj trezor. Pokud hledá v hello kroky v hello už jsme zmínili místní dokumentaci, uvidíte, že tato funkce automatizuje kroky 2 a 3. Hello pouze věc stále nutné toodo ručně, je trezor klíčů hello toocreate a klíče. Z tohoto místa se automatizované hello celý nastavení virtuálního počítače SQL. Po dokončení této instalace této funkce můžete spustit T-SQL příkazy toobegin šifrování databáze nebo zálohy běžným způsobem.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Povolení a konfigurace integrace se službou AZURE
Můžete povolit integrace se službou AZURE při zřizování nebo ho nakonfigurovat pro existující virtuální počítače.

### <a name="new-vms"></a>Nové virtuální počítače
Pokud zřizujete nového virtuálního počítače systému SQL Server s Resource Managerem, hello portál Azure poskytuje krok tooenable integrace se službou Azure Key Vault. Funkce Hello Azure Key Vault je dostupná pouze pro hello Enterprise, Developer a zkušební edice systému SQL Server.

![Integrace se službou Azure Key Vault pro SQL](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Podrobný návod k zřizování, najdete v části [zřízení virtuálního počítače s SQL Server v hello portálu Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Existující virtuální počítače
Pro existující virtuální počítače systému SQL Server vyberte virtuální počítač systému SQL Server. Potom vyberte hello **konfigurace systému SQL Server** části hello **nastavení** okno.

![Integrace se službou AZURE SQL pro existující virtuální počítače](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

V hello **konfigurace systému SQL Server** okně klikněte na tlačítko hello **upravit** tlačítka na hello části Integrace automatizované Key Vault.

![Konfigurace integrace se službou AZURE SQL pro existující virtuální počítače](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Po dokončení klikněte na tlačítko hello **OK** na konci hello hello tlačítko **konfigurace systému SQL Server** okno toosave změny.

> [!NOTE]
> Můžete také nakonfigurovat integrace se službou AZURE pomocí šablony. Další informace najdete v tématu [šablony Azure rychlý start pro integraci Azure Key Vault](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).
> 
> 

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

