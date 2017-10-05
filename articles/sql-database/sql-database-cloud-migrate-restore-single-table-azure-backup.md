---
title: "Obnovení jedné tabulky ze zálohy databáze Azure SQL Database | Microsoft Docs"
description: "Zjistěte, jak obnovit jednu tabulku ze zálohy databáze Azure SQL Database."
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: 340b41bd-9df8-47fb-adfc-03216de38a5e
ms.service: sql-database
ms.custom: business continuity
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: daleche
ms.openlocfilehash: 8c750c503d10ea63b9665958b96db2dfea3d9a3c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-restore-a-single-table-from-an-azure-sql-database-backup"></a>Postup při obnovení jedné tabulky ze zálohy Azure SQL Database
Mohou nastat situace, ve kterém omylem změnil některá data v databázi SQL a teď chcete obnovit jednu tabulku ovlivněné. Tento článek popisuje, jak obnovit jednu tabulku v databázi z jednoho z databáze SQL [automatické zálohování](sql-database-automated-backups.md).

## <a name="preparation-steps-rename-the-table-and-restore-a-copy-of-the-database"></a>Přípravné kroky: Přejmenovat tabulku a obnovte kopii databáze
1. Identifikujte tabulku v databázi Azure SQL, kterou chcete nahradit obnovené kopií. Pomocí Microsoft SQL Management Studio přejmenovat tabulku. Můžete například přejmenovat tabulku jako &lt;název tabulky&gt;_old.
   
   > [!NOTE]
   > Abyste se vyhnuli blokován, ujistěte se, že nebude žádná aktivita spuštěná na tabulku, která chcete přejmenovat. Pokud narazíte na potíže, ujistěte se, který tento postup provést během časového období údržby.
   >

2. Obnovit zálohu databáze k určitému bodu v čase, který chcete obnovit pomocí [bodu-In_Time obnovení](sql-database-recovery-using-backups.md#point-in-time-restore) kroky.
   
   > [!NOTE]
   > Název obnovené databáze bude ve formátu DBName + časové razítko. například **Adventureworks2012_2016-01-01T22-12Z**. Tento krok není přepsat existující název databáze na serveru. Toto je bezpečnostní opatření a je určená umožňují ověření obnovenou databázi předtím, než vyřadit své aktuální databázi a přejmenujte obnovenou databázi pro použití v provozním prostředí.
   
## <a name="copying-the-table-from-the-restored-database-by-using-the-sql-database-migration-tool"></a>Kopírování tabulky z obnovené databáze pomocí nástroje Migrace databáze SQL

1. Stáhněte a nainstalujte [Průvodce migrací databáze SQL](https://sqlazuremw.codeplex.com).
2. Otevřete Průvodce migrací databáze SQL na **vyberte proces** vyberte **databáze v rámci analyzovat nebo migrací**a potom klikněte na **Další**.

   ![Průvodce migrací databáze SQL - vyberte procesu](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)

3. V **připojit k serveru** dialogové okno pole, použijte následující nastavení:

   * Název serveru: **váš systém SQL server**
   * Ověřování: **ověřování systému SQL Server**
   * Přihlášení: **vaše přihlášení**
   * Heslo: **heslo**
   * Databáze: **databázi Master DB (Vypsat všechny databáze)**
   
   > [!NOTE]
   > Ve výchozím nastavení uloží přihlašovací informace. Pokud nechcete, aby mohla, vyberte **zapomněli přihlašovací informace**.
   >
   
     ![Migrace databáze SQL průvodce - vyberte zdroj – krok 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. V **vybrat zdroj** dialogové okno, vyberte název obnovené databáze z **přípravné kroky** jako svůj zdroj a pak klikněte na **Další**.
   
    ![Migrace databáze SQL průvodce - vyberte zdroj – krok 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)
5. V **zvolte objekty** dialogové okno, vyberte **vyberte konkrétní databázové objekty** možnost a potom vyberte table(or tables), kterou chcete migrovat na cílový server.
   ![Průvodce migrací databáze SQL - zvolte objekty](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)
6. Na **skriptu Průvodce Souhrn** klikněte na tlačítko **Ano** když se zobrazí výzva o tom, jestli jste připraveni generování skriptu SQL. Máte také možnost uložení TSQL skriptu pro pozdější použití.
   ![Průvodce migrací databáze SQL - souhrn Průvodce skriptu](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)
7. Na **shrnutí výsledků** klikněte na tlačítko **Další**.
   ![Průvodce migrací databáze SQL - shrnutí výsledků](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)
8. Na **nastavit připojení k serveru cíl** klikněte na tlačítko **připojit k serveru**a potom zadejte následující podrobnosti:
   
   * **Název serveru**: Cílová instance serveru
   * **Ověřování**: **ověřování serveru SQL Server**. Zadejte své přihlašovací údaje.
   * **Databáze**: **databázi Master DB (seznam všech databází)**. Tato možnost zobrazí všechny databáze na cílovém serveru.
     
     ![Průvodce migrací databáze SQL - nastavit připojení k cílového serveru](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)
9. Klikněte na tlačítko **připojit**, vyberte cílovou databázi, který chcete přesunout tabulka, která se a pak klikněte na **Další**. To by měl dokončit skriptu vytvořených předtím a měli byste vidět nově přesunutý tabulky zkopírován do cílové databáze.

## <a name="verification-step"></a>Krok ověření

- Dotaz a otestovat na nově zkopírovaný tabulku a ujistěte se, že data jsou beze změn. Po potvrzení, mohou vyřadit formuláře přejmenovat tabulku **přípravné kroky** části. (například &lt;název tabulky&gt;_old).

## <a name="next-steps"></a>Další kroky
[Automatické zálohování databáze SQL](sql-database-automated-backups.md)

