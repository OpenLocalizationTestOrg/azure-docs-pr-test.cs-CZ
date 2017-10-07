---
title: "aaaCreate a správě Azure databáze pro server databáze MySQL pomocí portálu Azure | Microsoft Docs"
description: "Tento článek popisuje, jak můžete rychle vytvořit novou databázi Azure pro server databáze MySQL a spravovat server hello pomocí hello portálu Azure."
services: mysql
author: v-chenyh
ms.author: nolanwu
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: c532df43b3d2124d7bd34875b32d52357f162af8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a>Vytvoření a správě Azure databáze pro server databáze MySQL pomocí portálu Azure
Tento článek popisuje, jak můžete rychle vytvořit novou databázi Azure pro server databáze MySQL a spravovat server hello pomocí hello portálu Azure. Správa serveru zahrnuje zobrazení Podrobnosti o serveru a databází, resetování hesla a odstranění serveru hello.

## <a name="log-in-toohello-azure-portal"></a>Přihlaste se toohello portálu Azure
Přihlaste se toohello [portál Azure](https://portal.azure.com).

## <a name="create-an-azure-database-for-mysql-server"></a>Vytvoření serveru Azure Database for MySQL
Postupujte podle těchto kroků toocreate Azure Database MySQL serveru s názvem "mysqlserver4demo"

1kliknutím **nový** nalezeno tlačítko na hello levém horním rohu hello portálu Azure.

Vyberte 2 **databáze** novou stránku hello a výběrem **Azure Database pro databázi MySQL** ze stránky hello databáze.

> Azure databáze pro server databáze MySQL byla vytvořena s definovanou sadu [výpočetního prostředí a úložiště](./concepts-compute-unit-and-storage.md) prostředky. Hello databáze byla vytvořena v rámci skupiny prostředků Azure a v databázi aplikace Azure pro server databáze MySQL.

![Vytvořit nový serverem](./media/howto-create-manage-server-portal/create-new-server.png)

3 - vyplňte hello databáze Azure pro formulář MySQL s hello následující informace:

| **Pole formuláře** | **Popis pole** |
|----------------|-----------------------|
| *Název serveru* | Azure mysql (název serveru je globálně jedinečný) |
| *Předplatné* | MySQLaaS (vyberte z rozevíracího seznamu) |
| *Skupina prostředků* | MyResource (vytvořit novou skupinu prostředků nebo použijte existující) |
| *Přihlašovací jméno správce serveru* | myadmin (název účtu správce instalace) |
| *Heslo* | Instalační program heslo účtu správce |
| *Potvrdit heslo* | potvrďte heslo účtu správce |
| *Umístění* | Severní Evropa (vyberte mezi Severní Evropa a západní USA) |
| *Verze* | 5.6 (zvolte Azure databáze MySQL server verze) |

4 kliknutím **cenová úroveň** toospecify hello služby vrstvy a úroveň výkonu pro nový server. Výpočetní jednotky lze konfigurovat rozsahu 50 až 100 v základní vrstvě, 100 až 200 ve standardní vrstvě a úložiště lze přidat podle zahrnuté množství. Tato příručka postupy umožňuje zvolte 50 výpočetní jednotka a 50GB. Klikněte na tlačítko **OK** toosave váš výběr.
![Vytvoření serveru – ceny vrstvy](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

Kliknutím 5 **vytvořit** tooprovision hello serveru. Zřizování trvá několik minut.

> Zkontrolujte hello **Pin toodashboard** možnost tooallow snadné sledování vašich nasazení.
> [!NOTE]
> I když too1000GB v základní vrstvě a 10000GB ve verzi Standard se úroveň nepodporuje pro úložiště pro verzi Public Preview, je maximální velikost úložiště hello stále omezené too1000GB dočasně. 
</Include>

## <a name="update-an-azure-database-for-mysql-server"></a>Aktualizovat databázi služby Azure pro server databáze MySQL
Po zřízení nového serveru uživatel má 2 možnosti tooedit existující server: resetování hesla správce nebo škálování nahoru/dolů hello server změnou hello výpočetní jednotky.

### <a name="change-hello-administrator-user-password"></a>Změnit heslo uživatele správce hello
1 - na serveru hello **přehled** okně klikněte na tlačítko **resetovat heslo** toopopulate okno zadání a potvrzení hesla.

2 – zadejte nové heslo a potvrzení hesla hello v okně hello jak je uvedeno níže: ![resetování hesla](./media/howto-create-manage-server-portal/reset-password.png)

3 klikněte na tlačítko **OK** toosave hello nové heslo.

### <a name="scale-updown-by-changing-compute-units"></a>Změnou výpočetní jednotky škálování nahoru/dolů

1 - v okně hello serveru v části **nastavení**, klikněte na tlačítko **cenová úroveň** tooopen hello cenová úroveň okně hello Azure databáze pro server databáze MySQL.

2 – postupujte podle kroku 4 v **vytvoření databáze Azure pro server databáze MySQL** toochange výpočetní jednotky v hello stejné cenová úroveň.

## <a name="delete-an-azure-database-for-mysql-server"></a>Odstraňte databázi Azure pro server databáze MySQL

1 - na serveru hello **přehled** okně klikněte na tlačítko **odstranit** příkaz tlačítko tooopen hello odstranění potvrzovací okno.

2-type hello správný název serveru vstupní pole hello okno pro potvrzení double.

3 klikněte na tlačítko **odstranit** tlačítko znovu tooconfirm odstraňování akce a počkejte, než pro "Odstranění úspěch" místní na hello oznamovací pruh.

## <a name="list-hello-azure-database-for-mysql-databases"></a>Seznam hello databáze Azure pro databáze MySQL
Na serveru hello **přehled** okno, posuňte se dolů, dokud neuvidíte hello databáze na dolní hello dlaždici. Všechny databáze hello se objeví v tabulce hello. Klikněte na tlačítko **odstranit** příkaz tlačítko tooopen hello odstranění potvrzovací okno.

![Zobrazit databáze](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a>Zobrazit podrobnosti databáze Azure pro server databáze MySQL
Klikněte na tlačítko **vlastnosti** pod **nastavení** na serveru hello se otevře okno hello **vlastnosti** okno. Potom můžete zobrazit všechny podrobné informace o serveru hello.

## <a name="next-steps"></a>Další kroky

[Rychlý úvod: Vytvoření databáze Azure pro server databáze MySQL pomocí portálu Azure](./quickstart-create-mysql-server-database-using-azure-portal.md)
