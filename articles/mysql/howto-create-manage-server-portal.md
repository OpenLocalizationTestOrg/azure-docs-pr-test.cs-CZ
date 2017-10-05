---
title: "Vytvoření a správě Azure databáze pro server databáze MySQL pomocí portálu Azure | Microsoft Docs"
description: "Tento článek popisuje, jak můžete rychle vytvořit novou databázi MySQL server Azure a spravovat server pomocí portálu Azure."
services: mysql
author: v-chenyh
ms.author: nolanwu
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 4605518b6955d9943e76c25df2d4105a6a94433d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a>Vytvoření a správě Azure databáze pro server databáze MySQL pomocí portálu Azure
Tento článek popisuje, jak můžete rychle vytvořit novou databázi MySQL server Azure a spravovat server pomocí portálu Azure. Správa serveru zahrnuje zobrazení Podrobnosti o serveru a databází, resetování hesla a odstranění serveru.

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal
Přihlaste se k portálu [Azure Portal](https://portal.azure.com).

## <a name="create-an-azure-database-for-mysql-server"></a>Vytvoření serveru Azure Database for MySQL
Postupujte podle těchto kroků k vytvoření databáze MySQL serveru s názvem "mysqlserver4demo" Azure

1kliknutím **nový** nalezeno tlačítko v levém horním rohu portálu Azure.

Vyberte 2 **databáze** z novou stránku a vyberte **Azure Database pro databázi MySQL** ze stránky databáze.

> Azure databáze pro server databáze MySQL byla vytvořena s definovanou sadu [výpočetního prostředí a úložiště](./concepts-compute-unit-and-storage.md) prostředky. Vytvoření databáze v rámci skupiny prostředků Azure a v databázi aplikace Azure pro server databáze MySQL.

![Vytvořit nový serverem](./media/howto-create-manage-server-portal/create-new-server.png)

3 - vyplňte Azure databáze MySQL formuláře s následujícími informacemi:

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

4 kliknutím **cenová úroveň** k určení úrovně služby a výkonu pro nový server. Výpočetní jednotky lze konfigurovat rozsahu 50 až 100 v základní vrstvě, 100 až 200 ve standardní vrstvě a úložiště lze přidat podle zahrnuté množství. Tato příručka postupy umožňuje zvolte 50 výpočetní jednotka a 50GB. Klikněte na tlačítko **OK** uložte svůj výběr.
![Vytvoření serveru – ceny vrstvy](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

Kliknutím 5 **vytvořit** ke zřízení serveru. Zřizování trvá několik minut.

> Zaškrtněte možnost **Připnout na řídicí panel**, abyste povolili snadné sledování vašich nasazení.
> [!NOTE]
> I když až 1 000 GB v základní vrstvě a 10000 ve standardní vrstvě budou podporované pro úložiště pro verzi Public Preview, je maximální velikost úložiště stále omezen na 1000GB dočasně. 
</Include>

## <a name="update-an-azure-database-for-mysql-server"></a>Aktualizovat databázi služby Azure pro server databáze MySQL
Po zřízení nového serveru uživatel má 2 možností pro upravit existující server: resetování hesla správce nebo škálování nahoru/dolů serveru tak, že změníte výpočetní jednotky.

### <a name="change-the-administrator-user-password"></a>Změna hesla správce uživatele
1 - na serveru **přehled** okně klikněte na tlačítko **resetovat heslo** k naplnění okno zadání a potvrzení hesla.

2 – zadejte nové heslo a potvrzení hesla do okna, jak je uvedeno níže: ![resetování hesla](./media/howto-create-manage-server-portal/reset-password.png)

3 klikněte na tlačítko **OK** uložit nové heslo.

### <a name="scale-updown-by-changing-compute-units"></a>Změnou výpočetní jednotky škálování nahoru/dolů

1 - v okně serveru v části **nastavení**, klikněte na tlačítko **cenová úroveň** otevřete okno cenová úroveň pro databázi Azure pro server databáze MySQL.

2 – postupujte podle kroku 4 v **vytvoření databáze Azure pro server databáze MySQL** změnit výpočetní jednotky ve stejné cenovou úroveň.

## <a name="delete-an-azure-database-for-mysql-server"></a>Odstraňte databázi Azure pro server databáze MySQL

1 - na serveru **přehled** okně klikněte na tlačítko **odstranit** příkazového tlačítka, otevřete okno potvrzení odstranění.

2 – zadejte správný název serveru do vstupního pole v okně pro dvojité potvrzení.

3 klikněte na tlačítko **odstranit** tlačítko znovu odstraňování akci potvrďte a počkejte "Odstranění úspěch" místní na panelu oznámení.

## <a name="list-the-azure-database-for-mysql-databases"></a>Seznam databáze Azure pro databází MySQL
Na serveru **přehled** okno, posuňte se dolů, dokud neuvidíte databázi na dolní dlaždici. Zobrazí se všechny databáze v tabulce. Klikněte na tlačítko **odstranit** příkazového tlačítka, otevřete okno potvrzení odstranění.

![Zobrazit databáze](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a>Zobrazit podrobnosti databáze Azure pro server databáze MySQL
Klikněte na tlačítko **vlastnosti** pod **nastavení** na serveru se otevře okno **vlastnosti** okno. Potom můžete zobrazit všechny podrobné informace o serveru.

## <a name="next-steps"></a>Další kroky

[Rychlý úvod: Vytvoření databáze Azure pro server databáze MySQL pomocí portálu Azure](./quickstart-create-mysql-server-database-using-azure-portal.md)
