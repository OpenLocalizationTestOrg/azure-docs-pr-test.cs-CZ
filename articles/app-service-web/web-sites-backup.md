---
title: "aaaBack vaší aplikaci v Azure"
description: "Zjistěte, jak toocreate zálohování aplikací ve službě Azure App Service."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 6223b6bd-84ec-48df-943f-461d84605694
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: e41d93d322bbc48b45b28eeaa817928d83c2b9d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-your-app-in-azure"></a>Zálohování aplikace v Azure
Hello zálohování a obnovení funkce v [Azure App Service](../app-service/app-service-value-prop-what-is.md) umožňuje snadno vytvářet zálohy aplikaci ručně nebo podle plánu. Přepisování stávající aplikace hello nebo obnovení tooanother aplikace můžete obnovit hello aplikace tooa snímku do předchozího stavu. 

Informace o obnovení ze zálohy aplikace najdete v tématu [obnovení aplikace v Azure](web-sites-restore.md).

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a>Co se zálohuje
Služby App Service můžete zálohovat hello následující informace o účtu úložiště Azure tooan a kontejnerů, které jste nakonfigurovali toouse vaší aplikace. 

* Konfigurace aplikací
* Obsah souboru
* Databáze připojené tooyour aplikace

Funkce zálohování podporuje Hello následující databáze řešení: 
   - [SQL Database](https://azure.microsoft.com/en-us/services/sql-database/)
   - [Azure databáze pro databázi MySQL (Preview)](https://azure.microsoft.com/en-us/services/mysql)
   - [Azure databázi PostgreSQL (Preview)](https://azure.microsoft.com/en-us/services/postgres)
   - [Databáze MySQL cleardb –](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
   - [MySQL v aplikaci](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)
 

> [!NOTE]
>  Každá záloha je kompletní kopie offline vaší aplikace, ne přírůstkové aktualizace.
>  

<a name="requirements"></a>

## <a name="requirements-and-restrictions"></a>Požadavky a omezení
* Hello zálohování a obnovení funkce vyžaduje hello toobe plán služby App Service v hello **standardní** vrstvy nebo **Premium** vrstvy. Další informace o škálování vašeho toouse plán služby App Service vyšší úroveň najdete v tématu [škálování aplikace v Azure](web-sites-scale.md).  
  **Premium** úroveň umožňuje větší počet denní zálohování ups než **standardní** vrstvy.
* Budete potřebovat účet úložiště Azure a kontejneru v hello stejnému předplatnému jako hello aplikace, které chcete toobackup. Další informace o účtech Azure storage najdete v tématu hello [odkazy](#moreaboutstorage) na konci hello tohoto článku.
* Zálohování může být až too10 GB aplikaci a databázi obsahu. Pokud velikost hello zálohování překračuje tento limit, dojde k chybě.

<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a>Vytvoření ruční zálohy
1. V hello [portálu Azure](https://portal.azure.com)přejděte okně tooyour aplikace, vyberte **zálohování**. Hello **zálohování** zobrazí se okno.
   
    ![Stránka zálohy][ChooseBackupsPage]
   
   > [!NOTE]
   > Pokud se zobrazí zpráva hello níže, klikněte na něj tooupgrade plán služby App Service předtím, než můžete pokračovat v zálohování.
   > V tématu [škálování aplikace v Azure](web-sites-scale.md) Další informace.  
   > ![Zvolte účet úložiště](./media/web-sites-backup/01UpgradePlan1.png)
   > 
   > 

2. V hello **zálohování** okně klikněte na tlačítko **konfigurace**
![klikněte na tlačítko Konfigurovat.](./media/web-sites-backup/ClickConfigure1.png)
3. V hello **konfigurace zálohování** okně klikněte na tlačítko **úložiště: není nakonfigurováno** tooconfigure účet úložiště.
   
    ![Zvolte účet úložiště][ChooseStorageAccount]
4. Vyberte cíl zálohování tak, že vyberete **účet úložiště** a **kontejneru**. účet úložiště Hello musí patřit toohello stejnému předplatnému jako aplikace hello chcete tooback nahoru. Pokud chcete, můžete vytvořit nový účet úložiště nebo nový kontejner v odpovídajících podoken hello. Když jste hotovi, klikněte na tlačítko **vyberte**.
   
    ![Zvolte účet úložiště](./media/web-sites-backup/02ChooseStorageAccount1-1.png)
5. V hello **konfigurace zálohování** okno, které je stále ponechány otevřené, můžete nakonfigurovat **příkaz Backup Database**, pak vyberte hello databází, které tooinclude v hello zálohování (databáze SQL nebo MySQL), a potom klikněte na **OK**.  
   
    ![Zvolte účet úložiště](./media/web-sites-backup/03ConfigureDatabase1.png)
   
   > [!NOTE]
   > Pro databáze tooappear v tomto seznamu, musí existovat jeho připojovací řetězec v hello **připojovací řetězce** části hello **nastavení aplikace** okno pro vaši aplikaci.
   > 
   > 
6. V hello **konfigurace zálohování** okně klikněte na tlačítko **Uložit**.    
7. V hello **zálohování** okně klikněte na tlačítko **zálohování**.
   
    ![Tlačítko BackUpNow][BackUpNow]
   
    Zobrazí zprávu o průběhu během procesu zálohování hello.

Po hello účtu úložiště a kontejneru konfigurace můžete kdykoli spustit ruční zálohy.  

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a>Konfigurace automatického zálohování
1. V hello **konfigurace zálohy** okně nastavit **naplánovaná zálohování** příliš**na**. 
   
    ![Zvolte účet úložiště](./media/web-sites-backup/05ScheduleBackup1.png)
2. Nastavte plán zálohování, které se zobrazí možnosti, **naplánované zálohování** příliš**na**, podle potřeby nakonfigurujte plán zálohování hello a klikněte na **OK**.
   
    ![Povolit automatické zálohování][SetAutomatedBackupOn]

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a>Nakonfigurujte částečné zálohy
V některých případech nechcete, aby toobackup všechno, co ve vaší aplikaci. Tady je pár příkladů:

* Můžete [nastavení týdenní zálohování](web-sites-backup.md#configure-automated-backups) vaší aplikace, který obsahuje statický obsah, který nikdy změny, jako starý příspěvcích na blogu nebo bitové kopie.
* Vaše aplikace obsahuje více než 10 GB obsahu (který je hello maximální velikost, můžete zálohovat v čase).
* Nechcete, aby soubory protokolu toobackup hello.

Částečné zálohy umožňuje zvolit přesně který souborů, které jste má toobackup.

### <a name="exclude-files-from-your-backup"></a>Vyloučit soubory ze zálohy
Předpokládejme, že máte aplikaci, která obsahuje soubory protokolu a statické bitové kopie, které byly zálohování jednou a nebudete toochange. V takových případech můžete vyloučit tyto soubory a složky z ukládají v budoucí zálohy. tooexclude soubory a složky ze záloh, vytváření `_backup.filter` souboru v hello `D:\home\site\wwwroot` složky vaší aplikace. Zadejte hello seznam souborů a složek, které chcete tooexclude v tomto souboru. 

Snadný způsob tooaccess toouse Kudu je vaše soubory. Klikněte na tlačítko **Rozšířené nástroje -> – přejděte** nastavení pro vaše webové aplikace tooaccess Kudu.

![Kudu pomocí portálu][kudu-portal]

Identifikujte složky hello chcete tooexclude ze zálohy.  Například chcete toofilter hello zvýrazněné složky a soubory.

![Složky bitových kopií][ImagesFolder]

Vytvořte soubor s názvem `_backup.filter` a do souboru hello umístit hello výše uvedeného seznamu, ale odebrat `D:\home`. Zobrazí seznam jeden adresář nebo soubor na každém řádku. Proto musí být hello obsah souboru hello:
 ```bash
    \site\wwwroot\Images\brand.png
    \site\wwwroot\Images\2014
    \site\wwwroot\Images\2013
```

Nahrát `_backup.filter` souboru toohello `D:\home\site\wwwroot\` adresáři vašeho webu pomocí [ftp](web-sites-deploy.md#ftp) nebo jiné metody. Pokud chcete, můžete vytvořit soubor hello přímo pomocí modulu Kudu `DebugConsole` a vložit obsah hello existuje.

Spuštění zálohování hello stejný způsob, obvyklým způsobem [ručně](#create-a-manual-backup) nebo [automaticky](#configure-automated-backups). Nyní, všechny soubory a složky, které jsou určené v `_backup.filter` je vyloučen z hello budoucí zálohy plánované nebo ručně spustit. 

> [!NOTE]
> Obnovení částečné zálohy vaší lokality hello stejným způsobem, jakým byste [obnovit zálohu regulární](web-sites-restore.md). proces obnovení Hello hello správné věci.
> 
> Po obnovení úplné zálohy, veškerý obsah na webu hello se nahradí ať je hello zálohování. Pokud je soubor na hello lokality, ale není v zálohování hello získá odstranit. Ale když se obnoví částečné zálohy veškerý obsah, který se nachází v jedné adresářů hello zakázáno, nebo jakékoli zakázané souboru, je ponechán beze.
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a>Ukládání záloh
Po provedení jednoho nebo více zálohování pro aplikaci Zálohování hello jsou viditelné na hello **kontejnery** okno účtu úložiště a vaše aplikace. V účtu úložiště hello, každá záloha se skládá z`.zip` soubor, který obsahuje hello zálohovaná data a `.xml` soubor, který obsahuje manifest hello `.zip` souboru obsahu. Můžete rozbalte a procházet tyto soubory, pokud chcete, aby tooaccess zálohování bez ve skutečnosti obnovení aplikace.

Hello zálohy databáze aplikace hello je uložená v kořenovém hello the.zip souboru. Pro databázi SQL je soubor souboru BACPAC (bez přípony souboru) a mohou být naimportovány. toocreate databázi SQL podle hello export souboru BACPAC najdete v tématu [importovat soubor souboru BACPAC tooCreate novou databázi uživatele](http://technet.microsoft.com/library/hh710052.aspx).

> [!WARNING]
> Změna žádné soubory hello ve vaší **websitebackups** kontejner může způsobit hello zálohování toobecome neplatný a proto není – obnovitelné.
> 
> 

<a name="nextsteps"></a>

## <a name="next-steps"></a>Další kroky
Informace o obnovení ze zálohy aplikace najdete v tématu [obnovení aplikace v Azure](web-sites-restore.md). Také lze zálohovat a obnovovat aplikace služby App Service pomocí rozhraní REST API (v tématu [aplikace pomocí REST toobackup a obnovení služby App Service](websites-csm-backup.md)).


<!-- IMAGES -->
[ChooseBackupsPage]: ./media/web-sites-backup/01ChooseBackupsPage1.png
[ChooseStorageAccount]: ./media/web-sites-backup/02ChooseStorageAccount-1.png
[IncludedDatabases]: ./media/web-sites-backup/03IncludedDatabases.png
[BackUpNow]: ./media/web-sites-backup/04BackUpNow1.png
[BackupProgress]: ./media/web-sites-backup/05BackupProgress.png
[SetAutomatedBackupOn]: ./media/web-sites-backup/06SetAutomatedBackupOn1.png
[Frequency]: ./media/web-sites-backup/07Frequency.png
[StartDate]: ./media/web-sites-backup/08StartDate.png
[StartTime]: ./media/web-sites-backup/09StartTime.png
[SaveIcon]: ./media/web-sites-backup/10SaveIcon.png
[ImagesFolder]: ./media/web-sites-backup/11Images.png
[LogsFolder]: ./media/web-sites-backup/12Logs.png
[GhostUpgradeWarning]: ./media/web-sites-backup/13GhostUpgradeWarning.png
[kudu-portal]:./media/web-sites-backup/kudu-portal.PNG

