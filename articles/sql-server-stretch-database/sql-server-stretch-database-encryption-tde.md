---
title: "aaaEnable transparentní šifrování dat pro funkci Stretch Database - Azure | Microsoft Docs"
description: "Povolit transparentní šifrování dat (šifrování TDE) pro SQL Server Stretch Database na Azure"
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: barbkess
editor: 
ms.assetid: a44ed8f5-b416-4c41-9b1e-b7271f10bdc3
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: douglasl
ms.openlocfilehash: 1d6bff455030ac8851b2184c1e8097afd61361d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Povolit transparentní šifrování dat (šifrování TDE) pro funkce Stretch Database na Azure
> [!div class="op_single_selector"]
> * [Azure Portal](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

Transparentní šifrování šifrování dat (TDE) pomáhá chránit před ohrožením hello škodlivých aktivit provedením v reálném čase šifrování a dešifrování hello databáze, přidružených záloh a souborů protokolů transakci bez nutnosti změny toohello aplikace.

Šifrování TDE zašifruje úložiště hello celé databáze pomocí symetrického klíče volané hello šifrovací klíč databáze. šifrovací klíč databáze Hello je chráněn certifikát integrovaného serveru. certifikát integrovaného serveru Hello je jedinečný pro každý server Azure. Microsoft automaticky otočí tyto certifikáty alespoň jednou za 90 dní. Obecný popis TDE, najdete v části [transparentní šifrování šifrování dat (TDE)].

## <a name="enabling-encryption"></a>Povolení šifrování
tooenable TDE pro databázi aplikace Azure, která ukládá hello, které se migrují data z databáze povolenou funkcí Stretch SQL Server hello následující věci:

1. Otevřete hello databáze v hello [portálu Azure](https://portal.azure.com)
2. V okně databáze hello, klikněte na tlačítko hello **nastavení** tlačítko
3. Vyberte hello **transparentní šifrování dat** možnost![][1]
4. Vyberte hello **na** nastavení a potom vyberte **uložit**
   ![][2]

## <a name="disabling-encryption"></a>Zakázáním šifrování
toodisable TDE pro databázi aplikace Azure, která ukládá hello, které se migrují data z databáze povolenou funkcí Stretch SQL Server hello následující věci:

1. Otevřete hello databáze v hello [portálu Azure](https://portal.azure.com)
2. V okně databáze hello, klikněte na tlačítko hello **nastavení** tlačítko
3. Vyberte hello **transparentní šifrování dat** možnost
4. Vyberte hello **vypnout** nastavení a potom vyberte **uložit**

<!--Anchors-->
[transparentní šifrování šifrování dat (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
