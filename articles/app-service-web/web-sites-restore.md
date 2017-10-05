---
title: "Obnovení aplikace v Azure"
description: "Zjistěte, jak vaše aplikace obnovit ze zálohy."
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
ms.openlocfilehash: 5fe74d992edb7028fa4a2500e427013d98ebc250
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="restore-an-app-in-azure"></a>Obnovení aplikace v Azure
Tento článek ukazuje, jak obnovit aplikace v [Azure App Service](../app-service/app-service-value-prop-what-is.md) který jste dříve zálohovali (viz [zálohování aplikace v Azure](web-sites-backup.md)). Můžete obnovit do předchozího stavu vaší aplikace pomocí jeho propojené databází na vyžádání nebo vytvořit novou aplikaci na základě jedné z původní aplikaci zálohování. Aplikační služba Azure podporuje následující databáze pro zálohování a obnovení:
- [SQL Database](https://azure.microsoft.com/en-us/services/sql-database/)
- [Azure databáze pro databázi MySQL (Preview)](https://azure.microsoft.com/en-us/services/mysql)
- [Azure databázi PostgreSQL (Preview)](https://azure.microsoft.com/en-us/services/postgres)
- [Databáze MySQL cleardb –](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
- [MySQL v aplikaci](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

Obnovení ze zálohy je k dispozici pro aplikace běžící **standardní** a **Premium** vrstvy. Informace o vertikálním navýšení kapacity aplikace naleznete v tématu [škálování aplikace v Azure](web-sites-scale.md). **Premium** úroveň umožňuje větší počet denní zálohování než **standardní** vrstvy.

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a>Obnovení aplikace z existující zálohy
1. Na **nastavení** vaší aplikace na portálu Azure klikněte na **zálohování** zobrazíte **zálohování** okno. Pak klikněte na tlačítko **obnovení**.
   
    ![Zvolte teď obnovení][ChooseRestoreNow]
2. V **obnovení** okno, nejprve vyberte zálohování zdroj.
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    **Zálohování aplikace** možnost ukazuje všechny stávající zálohy aktuální aplikaci, a můžete snadno vybrat jednu.
    **Úložiště** možnost umožňuje vybrat všechny záložní soubor ZIP z jakékoli existující účet služby Azure Storage a kontejner v rámci vašeho předplatného.
    Pokud se pokoušíte obnovit zálohu jiné aplikaci, použijte **úložiště** možnost.
3. Zadejte cíl pro obnovení aplikace v **cíl obnovení**.
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > Pokud se rozhodnete **přepsat**, všechny je vymazat a přepsat existující data v aktuální aplikaci. Před kliknutím na **OK**, ujistěte se, že je přesně co chcete udělat.
   > 
   > 
   
    Můžete vybrat **stávající aplikace** obnovení zálohy aplikace do jiné aplikace ve stejné skupině provedena. Než použijete tuto možnost, měli jste již vytvořili jiné aplikace ve vaší skupině prostředků s zrcadlení konfigurace databáze na jednu definované v aplikaci zálohování. Můžete také vytvořit **nový** obnovit svůj obsah do aplikace.

4. Klikněte na **OK**.

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Stáhnout nebo odstranit zálohy z účtu úložiště
1. Z hlavní **Procházet** okno portálu Azure, vyberte **účty úložiště**. Zobrazí se seznam existující účty úložiště.
2. Vyberte účet úložiště, který obsahuje zálohu, kterou chcete stáhnout nebo odstranit. Zobrazí se okno pro účet úložiště.
3. V okně účtu úložiště vyberte kontejner, který chcete
   
    ![Zobrazení kontejnery][ViewContainers]
4. Vyberte záložní soubor, který chcete stáhnout nebo odstranit.
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. Klikněte na tlačítko **Stáhnout** nebo **odstranit** v závislosti na tom, co chcete udělat.  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a>Monitorování operací obnovení
Chcete-li zobrazit podrobnosti o úspěchu nebo neúspěchu operace obnovení aplikace, přejděte na **protokol aktivit** okno na portálu Azure.  
 

Přejděte dolů a najděte požadovanou operaci obnovení a klikněte na tlačítko ji vyberte.

V okně podrobností zobrazuje dostupné informace související s operací obnovení.

## <a name="next-steps"></a>Další kroky
Lze zálohovat a obnovovat aplikace služby App Service pomocí rozhraní REST API (viz [REST použít k zálohování a obnovení služby App Service aplikace](websites-csm-backup.md)).


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
