---
title: "Zálohování aplikace v Azure"
description: "Naučte se vytvářet zálohy aplikací ve službě Azure App Service."
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
ms.openlocfilehash: 77e983afaaba8e944ab1f337e1c28ced83b63205
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="back-up-your-app-in-azure"></a>Zálohování aplikace v Azure
Back up a obnovení funkce v [Azure App Service](../app-service/app-service-value-prop-what-is.md) umožňuje snadno vytvářet zálohy aplikaci ručně nebo podle plánu. Aplikace můžete obnovit do snímku do předchozího stavu pomocí přepsal stávající aplikace nebo při obnovování jiné aplikaci. 

Informace o obnovení ze zálohy aplikace najdete v tématu [obnovení aplikace v Azure](web-sites-restore.md).

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a>Co se zálohuje
Služby App Service můžete zálohovat na účtu úložiště Azure a kontejner, který jste nakonfigurovali v aplikaci použijte následující informace. 

* Konfigurace aplikací
* Obsah souboru
* Databáze připojenou k aplikaci

Funkce zálohování podporuje následující databáze řešení: 
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
* Zálohování a obnovení funkce vyžaduje plán služby App Service ve **standardní** vrstvy nebo **Premium** vrstvy. Další informace o škálování používat vyšší úroveň plánu služby App Service najdete v tématu [škálování aplikace v Azure](web-sites-scale.md).  
  **Premium** úroveň umožňuje větší počet denní zálohování ups než **standardní** vrstvy.
* Budete potřebovat účet úložiště Azure a kontejner ve stejném předplatném jako aplikace, které chcete zálohovat. Další informace o účtech Azure storage, najdete v článku [odkazy](#moreaboutstorage) na konci tohoto článku.
* Zálohování může být až 10 GB aplikaci a databázi obsahu. Pokud velikost zálohování překračuje tento limit, dojde k chybě.

<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a>Vytvoření ruční zálohy
1. V [portálu Azure](https://portal.azure.com), přejděte do okna vaší aplikace, vyberte **zálohování**. **Zálohování** zobrazí se okno.
   
    ![Stránka zálohy][ChooseBackupsPage]
   
   > [!NOTE]
   > Pokud se zobrazí následující zpráva, klikněte na něj upgradovat plán služby App Service, abyste mohli pokračovat v zálohování.
   > V tématu [škálování aplikace v Azure](web-sites-scale.md) Další informace.  
   > ![Zvolte účet úložiště](./media/web-sites-backup/01UpgradePlan1.png)
   > 
   > 

2. V **zálohování** okně klikněte na tlačítko **konfigurace**
![klikněte na tlačítko Konfigurovat.](./media/web-sites-backup/ClickConfigure1.png)
3. V **konfigurace zálohování** okně klikněte na tlačítko **úložiště: není nakonfigurováno** ke konfiguraci účtu úložiště.
   
    ![Zvolte účet úložiště][ChooseStorageAccount]
4. Vyberte cíl zálohování tak, že vyberete **účet úložiště** a **kontejneru**. Účet úložiště musí patřit do stejného předplatného jako aplikace, které chcete zálohovat. Pokud chcete, můžete vytvořit nový účet úložiště nebo nový kontejner v odpovídajících podoken. Když jste hotovi, klikněte na tlačítko **vyberte**.
   
    ![Zvolte účet úložiště](./media/web-sites-backup/02ChooseStorageAccount1-1.png)
5. V **konfigurace zálohování** okno, které je stále ponechány otevřené, můžete nakonfigurovat **příkaz Backup Database**, vyberte databáze, které chcete zahrnout do zálohy (databáze SQL nebo MySQL) a pak klikněte na tlačítko **OK**.  
   
    ![Zvolte účet úložiště](./media/web-sites-backup/03ConfigureDatabase1.png)
   
   > [!NOTE]
   > Pro databázi se objeví v tomto seznamu, musí existovat jeho připojovací řetězec v **připojovací řetězce** části **nastavení aplikace** okno pro vaši aplikaci.
   > 
   > 
6. V **konfigurace zálohování** okně klikněte na tlačítko **Uložit**.    
7. V **zálohování** okně klikněte na tlačítko **zálohování**.
   
    ![Tlačítko BackUpNow][BackUpNow]
   
    Zobrazí zprávu o průběhu během procesu zálohování.

Jakmile nakonfigurovaný účet úložiště a kontejneru můžete kdykoli spustit ruční zálohy.  

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a>Konfigurace automatického zálohování
1. V **konfigurace zálohy** okně nastavit **naplánovaná zálohování** k **na**. 
   
    ![Zvolte účet úložiště](./media/web-sites-backup/05ScheduleBackup1.png)
2. Nastavte plán zálohování, které se zobrazí možnosti, **naplánované zálohování** k **na**, podle potřeby nakonfigurujte plán zálohování a klikněte na **OK**.
   
    ![Povolit automatické zálohování][SetAutomatedBackupOn]

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a>Nakonfigurujte částečné zálohy
Někdy nechcete zálohování všechno ve vaší aplikaci. Tady je pár příkladů:

* Můžete [nastavení týdenní zálohování](web-sites-backup.md#configure-automated-backups) vaší aplikace, který obsahuje statický obsah, který nikdy změny, jako starý příspěvcích na blogu nebo bitové kopie.
* Vaše aplikace obsahuje více než 10 GB obsahu (který je maximální velikost, můžete zálohovat v čase).
* Nechcete zálohování souborů protokolu.

Částečné zálohy umožňuje zvolit přesně který souborů, které jste chcete zálohovat.

### <a name="exclude-files-from-your-backup"></a>Vyloučit soubory ze zálohy
Předpokládejme, že máte aplikaci, která obsahuje soubory protokolu a statické bitové kopie, které byly zálohování jednou a nebudete změnit. V takových případech můžete vyloučit tyto soubory a složky z ukládají v budoucí zálohy. Vyloučit soubory a složky ze záloh, vytváření `_backup.filter` v soubor `D:\home\site\wwwroot` složky vaší aplikace. Zadejte seznam souborů a složek, které chcete vyloučit v tomto souboru. 

Snadný způsob, jak přístup k souborům je použití Kudu. Klikněte na tlačítko **Rozšířené nástroje -> – přejděte** nastavení pro vaši webovou aplikaci pro přístup k modulu Kudu.

![Kudu pomocí portálu][kudu-portal]

Identifikujte složky, které chcete vyloučit ze zálohy.  Například chcete filtrovat zvýrazněné složky a soubory.

![Složky bitových kopií][ImagesFolder]

Vytvořte soubor s názvem `_backup.filter` a put v seznamu nahoře v souboru, ale odebrat `D:\home`. Zobrazí seznam jeden adresář nebo soubor na každém řádku. Takže obsah souboru by mělo být:
 ```bash
    \site\wwwroot\Images\brand.png
    \site\wwwroot\Images\2014
    \site\wwwroot\Images\2013
```

Nahrát `_backup.filter` do souboru `D:\home\site\wwwroot\` adresáři vašeho webu pomocí [ftp](web-sites-deploy.md#ftp) nebo jiné metody. Pokud chcete, můžete vytvořit soubor přímo pomocí modulu Kudu `DebugConsole` a vložit obsah existuje.

Spuštění zálohování stejným způsobem, obvyklým způsobem [ručně](#create-a-manual-backup) nebo [automaticky](#configure-automated-backups). Nyní, všechny soubory a složky, které jsou určené v `_backup.filter` je vyloučen z budoucí zálohy plánované nebo ručně spustit. 

> [!NOTE]
> Obnovit částečné zálohy lokality stejně jako kdybyste [obnovit zálohu regulární](web-sites-restore.md). Proces obnovení nemá správné věci.
> 
> Po obnovení úplné zálohy, veškerý obsah na webu se nahradí ať je v záloze. Pokud je soubor v lokalitě, ale není v zálohování získá odstranit. Ale když se obnoví částečné zálohy veškerý obsah, který se nachází v jedné z zakázané adresářů nebo všechny zakázané souboru, je ponechán beze.
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a>Ukládání záloh
Po provedení jednoho nebo více zálohování pro aplikaci Zálohování, se zobrazují **kontejnery** okno účtu úložiště a vaše aplikace. V účtu úložiště, každá záloha se skládá z`.zip` soubor, který obsahuje data záloh a `.xml` soubor, který obsahuje manifest z `.zip` souboru obsahu. Můžete rozbalte a procházet tyto soubory, pokud chcete přístup k zálohování bez ve skutečnosti provádí obnovení aplikaci.

V kořenovém souboru the.zip je uložena záloha databáze pro aplikaci. Pro databázi SQL je soubor souboru BACPAC (bez přípony souboru) a mohou být naimportovány. Vytvoření databáze SQL podle export souboru BACPAC naleznete v tématu [Import souboru BACPAC soubor, který chcete vytvořit novou databázi uživatele](http://technet.microsoft.com/library/hh710052.aspx).

> [!WARNING]
> Změna některý ze souborů ve vaší **websitebackups** kontejner může způsobit zálohování stane neplatnou a proto není – obnovitelné.
> 
> 

<a name="nextsteps"></a>

## <a name="next-steps"></a>Další kroky
Informace o obnovení ze zálohy aplikace najdete v tématu [obnovení aplikace v Azure](web-sites-restore.md). Také lze zálohovat a obnovovat aplikace služby App Service pomocí rozhraní REST API (v tématu [použití REST pro zálohování a obnovení služby App Service aplikace](websites-csm-backup.md)).


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

