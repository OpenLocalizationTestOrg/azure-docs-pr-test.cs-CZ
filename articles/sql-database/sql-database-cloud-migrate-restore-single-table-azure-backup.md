---
title: "aaaRestore jedné tabulky ze zálohy databáze Azure SQL Database | Microsoft Docs"
description: "Zjistěte, jak toorestore jedné tabulky ze zálohy databáze SQL Azure."
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
ms.openlocfilehash: 696d2ac87a70bccdf063bfecb8255723404aa5bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorestore-a-single-table-from-an-azure-sql-database-backup"></a>Jak toorestore jedné tabulky ze zálohy databáze SQL Azure
Mohou nastat situace, ve kterém můžete upravit omylem některá data v databázi SQL a teď chcete toorecover hello jedné ovlivněných tabulky. Tento článek popisuje, jak toorestore jedné tabulky v databázi z jednoho z hello SQL Database [automatické zálohování](sql-database-automated-backups.md).

## <a name="preparation-steps-rename-hello-table-and-restore-a-copy-of-hello-database"></a>Přípravné kroky: hello tabulku přejmenovat a obnovit kopii databáze hello
1. Identifikujte hello tabulky v databázi Azure SQL, které chcete tooreplace s hello obnovit kopii. Pomocí tabulky hello toorename Microsoft SQL Management Studio. Můžete například přejmenovat tabulku hello jako &lt;název tabulky&gt;_old.
   
   > [!NOTE]
   > tooavoid blokován, ujistěte se, že nebude žádná aktivita systémem hello tabulky, který chcete přejmenovat. Pokud narazíte na potíže, ujistěte se, který tento postup provést během časového období údržby.
   >

2. Obnovení zálohy databáze tooa bodu v čase, které chcete toorecover toousing hello [bodu-In_Time obnovení](sql-database-recovery-using-backups.md#point-in-time-restore) kroky.
   
   > [!NOTE]
   > Název Hello hello obnovit databáze bude mít formát DBName + časové razítko hello; například **Adventureworks2012_2016-01-01T22-12Z**. Tento krok není přepsat hello existující název databáze na serveru hello. Toto je bezpečnostní opatření a je určena tooallow tooverify hello obnovit databáze předtím, než vyřadit své aktuální databázi a přejmenujte hello obnovit databázi pro použití v provozním prostředí.
   
## <a name="copying-hello-table-from-hello-restored-database-by-using-hello-sql-database-migration-tool"></a>Kopírování hello tabulku z hello obnovit databáze pomocí nástroje Migrace databáze SQL hello

1. Stáhněte a nainstalujte hello [Průvodce migrací databáze SQL](https://sqlazuremw.codeplex.com).
2. Průvodce migrací databáze SQL, otevřete hello na hello **vyberte proces** vyberte **databáze v rámci analyzovat nebo migrací**a potom klikněte na **Další**.

   ![Průvodce migrací databáze SQL - vyberte procesu](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)

3. V hello **připojit tooServer** dialogové okno pole, použít hello následující nastavení:

   * Název serveru: **váš systém SQL server**
   * Ověřování: **ověřování systému SQL Server**
   * Přihlášení: **vaše přihlášení**
   * Heslo: **heslo**
   * Databáze: **databázi Master DB (Vypsat všechny databáze)**
   
   > [!NOTE]
   > Ve výchozím nastavení Průvodce hello uloží přihlašovací informace. Pokud nechcete, aby mohla, vyberte **zapomněli přihlašovací informace**.
   >
   
     ![Migrace databáze SQL průvodce - vyberte zdroj – krok 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. V hello **vybrat zdroj** dialogové okno, název databáze vyberte hello obnovit z hello **přípravné kroky** jako svůj zdroj a pak klikněte na **Další**.
   
    ![Migrace databáze SQL průvodce - vyberte zdroj – krok 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)
5. V hello **zvolte objekty** dialogové okno, vyberte hello **vyberte konkrétní databázové objekty** možnost a potom vyberte hello table(or tables) má toomigrate toohello cílový server.
   ![Průvodce migrací databáze SQL - zvolte objekty](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)
6. Na hello **skriptu Průvodce Souhrn** klikněte na tlačítko **Ano** když se zobrazí výzva o tom, jestli jste připravené toogenerate skript SQL. Máte také hello možnost toosave hello TSQL skript pro pozdější použití.
   ![Průvodce migrací databáze SQL - souhrn Průvodce skriptu](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)
7. Na hello **shrnutí výsledků** klikněte na tlačítko **Další**.
   ![Průvodce migrací databáze SQL - shrnutí výsledků](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)
8. Na hello **nastavit připojení k serveru cíl** klikněte na tlačítko **připojit tooServer**a potom zadejte podrobnosti hello následujícím způsobem:
   
   * **Název serveru**: Cílová instance serveru
   * **Ověřování**: **ověřování serveru SQL Server**. Zadejte své přihlašovací údaje.
   * **Databáze**: **databázi Master DB (seznam všech databází)**. Tato možnost zobrazí všechny databáze hello na cílový server hello.
     
     ![Průvodce migrací databáze SQL - nastavit připojení k cílového serveru](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)
9. Klikněte na tlačítko **připojit**, vyberte hello cílová databáze, který chcete toomove hello tabulky a potom klikněte na **Další**. To by měl dokončit skriptu hello dříve generované a měli byste vidět, že hello nově přesouvána tabulky zkopírovaný toohello cílovou databázi.

## <a name="verification-step"></a>Krok ověření

- Dotaz a testování hello nově zkopírovat toomake tabulky se, že hello data beze změn. Po potvrzení, mohou vyřadit hello přejmenovat tabulku formuláře **přípravné kroky** části. (například &lt;název tabulky&gt;_old).

## <a name="next-steps"></a>Další kroky
[Automatické zálohování databáze SQL](sql-database-automated-backups.md)

