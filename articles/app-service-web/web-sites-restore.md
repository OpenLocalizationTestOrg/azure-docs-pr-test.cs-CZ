---
title: aaaRestore aplikace v Azure
description: "Zjistěte, jak toorestore aplikace ze zálohy."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 4444dbf7-363c-47e2-b24a-dbd45cb08491
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: 4b54029a9197064f990f29a3c4558c8322668714
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-an-app-in-azure"></a>Obnovení aplikace v Azure
Tento článek ukazuje, jak toorestore na aplikace v [Azure App Service](../app-service/app-service-value-prop-what-is.md) který jste dříve zálohovali (najdete v části [zálohování aplikace v Azure](web-sites-backup.md)). Dokáže obnovit vaší aplikace pomocí předchozího stavu na vyžádání tooa propojené databáze nebo vytvořit novou aplikaci na základě jedné z původní aplikaci zálohování. Aplikační služba Azure podporuje následující databáze pro zálohování a obnovení hello:
- [SQL Database](https://azure.microsoft.com/en-us/services/sql-database/)
- [Azure databáze pro databázi MySQL (Preview)](https://azure.microsoft.com/en-us/services/mysql)
- [Azure databázi PostgreSQL (Preview)](https://azure.microsoft.com/en-us/services/postgres)
- [Databáze MySQL cleardb –](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
- [MySQL v aplikaci](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

Obnovení ze zálohy je k dispozici tooapps spuštěné v **standardní** a **Premium** vrstvy. Informace o vertikálním navýšení kapacity aplikace naleznete v tématu [škálování aplikace v Azure](web-sites-scale.md). **Premium** úroveň umožňuje větší počet denní zálohy toobe provést než **standardní** vrstvy.

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a>Obnovení aplikace z existující zálohy
1. Na hello **nastavení** okna aplikace na hello portálu Azure, klikněte na tlačítko **zálohování** toodisplay hello **zálohování** okno. Pak klikněte na tlačítko **obnovení**.
   
    ![Zvolte teď obnovení][ChooseRestoreNow]
2. V hello **obnovení** okno, první zálohování zdroj vyberte hello.
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    Hello **zálohování aplikace** možnost uvádí všechny hello existující zálohy hello aktuální aplikaci, a můžete snadno vybrat jednu.
    Hello **úložiště** možnost umožňuje vybrat všechny záložní soubor ZIP z jakékoli existující účet služby Azure Storage a kontejner v rámci vašeho předplatného.
    Pokud se snažíte toorestore zálohu jiné aplikaci, použijte hello **úložiště** možnost.
3. Potom můžete určit hello cíl pro obnovení aplikace hello v **cíl obnovení**.
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > Pokud se rozhodnete **přepsat**, všechny je vymazat a přepsat existující data v aktuální aplikaci. Před kliknutím na **OK**, ujistěte se, že je přesně co chcete toodo.
   > 
   > 
   
    Můžete vybrat **stávající aplikace** toorestore hello aplikaci Zálohování tooanother aplikace v hello stejnou skupinu provedena. Než použijete tuto možnost, měli jste již vytvořili jiné aplikace ve vaší skupině prostředků s zrcadlení databáze konfigurace toohello, definovaná v zálohování aplikace hello. Můžete také vytvořit **nový** toorestore aplikace svůj obsah do.

4. Klikněte na **OK**.

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Stáhnout nebo odstranit zálohy z účtu úložiště
1. Z hlavní hello **Procházet** okno hello portál Azure, vyberte **účty úložiště**. Zobrazí se seznam existující účty úložiště.
2. Vyberte účet úložiště hello, který obsahuje hello zálohování, který chcete, zobrazí se okno toodownload nebo delete.hello pro účet úložiště hello.
3. V okně účtu úložiště hello vyberte hello kontejner, který chcete
   
    ![Zobrazení kontejnery][ViewContainers]
4. Vyberte soubor zálohy mají toodownload nebo odstranit.
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. Klikněte na tlačítko **Stáhnout** nebo **odstranit** v závislosti na tom, co jste má toodo.  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a>Monitorování operací obnovení
toosee podrobnosti o hello úspěšné nebo neúspěšné operace obnovení hello aplikace, přejděte toohello **protokol aktivit** okno v hello portálu Azure.  
 

Přejděte dolů tooselect toofind hello potřeby obnovení operaci a klikněte na tlačítko ji.

Hello podrobnosti okno zobrazí hello k dispozici informace týkající se toohello operace obnovení.

## <a name="next-steps"></a>Další kroky
Lze zálohovat a obnovovat aplikace služby App Service pomocí rozhraní REST API (viz [tooback pomocí REST a obnovení služby App Service aplikace](websites-csm-backup.md)).


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow1.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
