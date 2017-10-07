---
title: "aaaUse REST tooback zálohu a obnovení aplikace služby App Service"
description: "Další informace, jakým způsobem volá tooback toouse rozhraní RESTful API a obnovení aplikace v Azure App Service"
services: app-service
documentationcenter: 
author: NKing92
manager: erikre
editor: 
ms.assetid: f94d0aea-edc1-43ab-9b51-ea7375859276
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2016
ms.author: nicking
ms.openlocfilehash: f77bdfc7de1626d04d8d0c0e8979231ae1ced81a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-rest-tooback-up-and-restore-app-service-apps"></a>Použít REST tooback a obnovení aplikace služby App Service
> [!div class="op_single_selector"]
> * [PowerShell](../app-service/app-service-powershell-backup.md)
> * [REST API](websites-csm-backup.md)
> 
> 

[Aplikace služby App Service](https://azure.microsoft.com/services/app-service/web/) lze zálohovat jako objekty BLOB v úložišti Azure. Hello zálohování může také obsahovat aplikace hello databáze. Pokud aplikace hello někdy omylem odstraněna, nebo pokud toobe potřebám aplikace hello vrácení předchozí verze tooa, lze obnovit z jakékoli předchozí zálohy. Zálohy lze provést kdykoli na vyžádání nebo zálohování můžete naplánovat vhodných intervalech.

Tento článek vysvětluje, jak toobackup a obnovení aplikace pomocí rozhraní RESTful API požadavky. Pokud chcete vytvořit toocreate a spravovat zálohy aplikace graficky prostřednictvím hello portálu Azure, najdete v části [zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md)

<a name="gettingstarted"></a>

## <a name="getting-started"></a>Začínáme
toosend REST požadavky, je nutné tooknow vaší aplikace **název**, **skupiny prostředků**, a **id předplatného**. Tyto informace najdete kliknutím aplikace v rámci hello **služby App Service** okno hello [portál Azure](https://portal.azure.com). Hello příklady v tomto článku, jsme konfigurujete hello webu **backuprestoreapiexamples.azurewebsites.net**. Uloženo ve skupině prostředků výchozí-Web-WestUS hello a běží na předplatné s ID 00001111-2222-3333-4444-555566667777 hello.

![Informace o ukázkovému webu][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>

## <a name="backup-and-restore-rest-api"></a>Zálohování a obnovení REST API
Nyní obsahuje několik příkladů, jak toouse hello toobackup REST API a obnovení aplikace. Každý příklad obsahuje obsah adresy URL a HTTP žádosti. Adresa URL ukázkových Hello obsahuje zástupné symboly uzavřen do složených závorek, například {id předplatného}. Nahraďte zástupné symboly hello hello odpovídající informace pro vaši aplikaci. Pro referenci tady je vysvětlení jednotlivých zástupný symbol, který se zobrazí v ukázkových adresách URL hello.

* id předplatného – ID aplikace obsahující hello hello předplatného Azure
* Název skupiny prostředků – – název hello prostředků skupiny obsahující hello aplikace
* název – název aplikace hello
* zálohování id – ID zálohy aplikace hello

Hello kompletní dokumentaci o hello rozhraní API, včetně několika volitelné parametry, které můžou být součástí hello HTTP požadavku, najdete v části hello [Průzkumníka prostředků Azure](https://resources.azure.com/).

<a name="backup-on-demand"></a>

## <a name="backup-an-app-on-demand"></a>Zálohování aplikace na vyžádání
tooback se aplikace hned, odešle **POST** požadavku příliš**https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/ zálohování nebo**.

Zde je co hello vypadá adresa URL pomocí našich příklad webu. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backup/**

Zadejte objekt JSON v textu hello vaše žádost o toospecify, které toostore toouse účet úložiště hello zálohování. Objekt JSON Hello musí mít vlastnost s názvem **storageAccountUrl**, který drží [SAS URL](../storage/common/storage-dotnet-shared-access-signature-part-1.md) udělení oprávnění k zápisu toohello Azure Storage kontejner, který obsahuje objekt blob zálohování hello. Pokud chcete tooback zálohu vaší databáze, je třeba zadat seznam obsahující hello názvy, typy a připojovací řetězce toobe databáze hello zálohovat.

```
{
    "properties":
    {
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        “databases”: [
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”,
                "connectionString": "<your database connection string>"
            }
        ]
    }
}
```

Zálohování aplikace hello začne okamžitě, když obdrží žádost hello. proces zálohování Hello může trvat dlouhou dobu toocomplete. Hello HTTP odpovědi obsahuje ID, které můžete použít v jiné stav žádosti o toosee hello hello zálohy. Tady je příklad textu hello hello HTTP odpovědi tooour zálohování požadavku.

```
{
    "id": "/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples",
    "name": " backuprestoreapiexamples ",
    "type": "Microsoft.Web/sites",
    "location": "WestUS",
    "properties":    {
        "id": 1,
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        "blobName": “backup_201509291825.zip”,
        "name": "backup_201509291825",
        "status": 4,
        "sizeInBytes": 0,
        "created": "2015-09-29T18:25:26.3992707Z",
        "log": null,
        "databases": [
            {
                "databaseType": "SqlAzure",
                "name": "MyDatabase1",
                "connectionString": "<your database connection string>"
            }
        ],
        "scheduled": false,
        "correlationId": "ea730f4d-dd06-4f7f-a090-9aa09k662h36",
    }
}
```

> [!NOTE]
> Chybové zprávy naleznete ve vlastnosti protokolu hello hello odpověď HTTP.
> 
> 

<a name="schedule-automatic-backups"></a>

## <a name="schedule-automatic-backups"></a>Naplánovat automatické zálohování
V přidání toobacking aplikaci na vyžádání můžete také naplánovat zálohování toohappen automaticky.

### <a name="set-up-a-new-automatic-backup-schedule"></a>Nastavit nový plán automatické zálohování
Odeslat tooset si plán zálohování **PUT** požadavku příliš**https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config / backup**.

Zde je co hello vypadá adresa URL pro náš příklad Web. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/config/Backup**

text žádosti Hello musí mít objekt JSON, který určuje hello konfigurace zálohy. Tady je příklad se všemi hello požadované parametry.

```
{
    "location": "WestUS",
    "properties": // Represents an app restore request
    {
        "backupSchedule": { // Required for automatically scheduled backups
            "frequencyInterval": "7",
            "frequencyUnit": "Day",
            "keepAtLeastOneBackup": "True",
            "retentionPeriodInDays": "10",
        },
        "enabled": "True", // Must be set tootrue tooenable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

Tento příklad konfiguruje toobe aplikace hello automaticky zálohovat každých sedm dní. Hello parametry **frequencyInterval** a **frequencyUnit** společně určují, jak často hello zálohám. Platné hodnoty pro **frequencyUnit** jsou **hodinu** a **den**. Například tooback aplikaci každých 12 hodin, nastavte frequencyInterval too12 a frequencyUnit toohour.

Starší zálohy se automaticky odeberou z účtu úložiště hello. Můžete řídit stáří hello zálohování může být podle nastavení hello **retentionPeriodInDays** parametr. Pokud chcete mít aspoň jednu zálohu uložené, bez ohledu na to, jak staré, nastavení tooalways **keepAtLeastOneBackup** tootrue.

### <a name="get-hello-automatic-backup-schedule"></a>Získat hello automatické plán zálohování
tooget aplikace pro zálohování konfigurace, odeslání **POST** toohello adresa URL požadavku **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/ lokality / {name} / config/zálohování nebo seznamu**.

Adresa URL Hello pro náš příklad web je **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.

<a name="get-backup-status"></a>

## <a name="get-hello-status-of-a-backup"></a>Získat stav hello zálohy
V závislosti na tom, jak velké aplikace hello je, může chvíli trvat zálohu toocomplete. Zálohování může také selže, vypršení časového limitu nebo částečně úspěšné. Stav hello toosee zálohování všechny aplikace, odeslání **získat** toohello adresa URL požadavku **/providers/ https://management.azure.com/subscriptions/ {id předplatného} /resourceGroups/ {resource-group name} Microsoft.Web/sites/{name}/backups**.

Stav hello toosee zálohu konkrétní odeslat požadavek GET toohello URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/ { zálohování id}**.

Zde je co hello vypadá adresa URL pro náš příklad Web. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**

text odpovědi Hello obsahuje podobné toothis příkladu objektů JSON.

```
{
    "properties":  {
        "id": 1,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl",
        "blobName": "backup_201509291734.zip",
        "name": "backup_201509291734",
        "status": 2,
        "sizeInBytes": 151973,
        "created": "2015-09-29T17:34:57.2091605",
        "scheduled": false,
        "lastRestoreTimeStamp": null,
        "finishedTimeStamp": "2015-09-29T17:35:11.3293602",
        "correlationId": "2379fc13-418a-4b9c-920d-d06e73c5028d",
        "websiteSizeInBytes": 209920
    }
}
```

Stav Hello zálohy je výčtového typu. Zde je každý možné stavu.

* 0 – InProgress: hello zálohování se spustil, ale zatím není dokončený.
* 1 – se nezdařilo: hello zálohování se nezdařila.
* 2 – úspěšně: hello zálohování bylo úspěšně dokončeno.
* 3 – TimedOut: hello zálohování nebylo dokončeno v čase a byla zrušena.
* 4 – vytvoření: hello zálohování požadavek ve frontě, ale nebyla spuštěna.
* 5 – přeskočeno: hello zálohování není pokračovat z důvodu tooa plán spuštění příliš mnoho zálohy.
* 6 – PartiallySucceeded: hello zálohování úspěšně, ale některé soubory nebyly zálohovány, protože nelze načíst. K tomu obvykle dochází, protože výhradní zámek byl umístěn na hello soubory.
* 7 – DeleteInProgress: zálohování hello byla požadovaná toobe odstranit, ale nebyla ještě odstraněna.
* 8 – DeleteFailed: hello zálohování se nepodařilo odstranit. Může k tomu dojít, protože vypršela platnost hello SAS adresa URL, která byla použité toocreate hello zálohování.
* 9 – odstranit: byl hello zálohování úspěšně odstraněn.

<a name="restore-app"></a>

## <a name="restore-an-app-from-a-backup"></a>Obnovení ze zálohy aplikace
Pokud vaše aplikace byl odstraněn, nebo pokud chcete toorevert tooa předchozí verzi aplikace, můžete aplikace hello obnovit ze zálohy. Odeslat tooinvoke obnovení, **POST** toohello adresa URL požadavku **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/ zálohování / {zálohování id} a obnově**.

Zde je co hello vypadá adresa URL pro náš příklad Web. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/Restore**

V textu žádosti hello odesílat objekt JSON, který obsahuje hello vlastnosti pro operaci obnovení hello. Tady je příklad, který obsahuje všechny požadované vlastnosti:

```
{
    "location": "WestUS",
    "properties":
    {
        "blobName": "backup_201509280431.zip",
        "databases": [ // Not required unless you’re restoring databases
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”
        }],
        "overwrite": "true",
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL toostorage container containing your website backup
    }
}
```

### <a name="restore-tooa-new-app"></a>Obnovit tooa nové aplikace
V některých případech můžete chtít toocreate novou aplikaci při obnovování zálohování, místo přepsal stávající aplikace. toodo se změna hello požadavek adresy URL toopoint toohello novou aplikaci chcete toocreate a změnit hello **přepsat** vlastnost hello JSON příliš**false**.

<a name="delete-app-backup"></a>

## <a name="delete-an-app-backup"></a>Odstranit zálohu aplikace
Pokud chcete toodelete zálohu, odeslání **odstranit** toohello adresa URL požadavku **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/ lokality / {name} {zálohování id} /backups/**.

Zde je co hello vypadá adresa URL pro náš příklad Web. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**

<a name="manage-sas-url"></a>

## <a name="manage-a-backups-sas-url"></a>Správa zálohování SAS adresu URL
Azure App Service se pokusí toodelete zálohováním ze služby Azure Storage pomocí hello SAS adresa URL, která byla poskytnuta při vytvoření zálohy hello. Pokud tato adresa URL SAS již není platná, nelze odstranit hello zálohování prostřednictvím hello REST API. Však můžete aktualizovat hello SAS URL přidružené zálohy odesláním **POST** toohello adresa URL požadavku **https://management.azure.com/subscriptions/ {id předplatného} /resourceGroups/ {resource-group name} / providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.

Zde je co hello vypadá adresa URL pro náš příklad Web. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/list**

V textu žádosti hello odesílat objekt JSON, který obsahuje hello nové adrese URL SAS. Tady je příklad.

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

> [!NOTE]
> Z bezpečnostních důvodů hello SAS URL přidruženého zálohu nevrátí při odesílání požadavek GET pro konkrétní zálohování. Pokud chcete tooview hello SAS URL přidružené zálohy, odeslání toohello požadavek POST stejnou adresu URL výše. Zahrňte prázdný objekt JSON v textu žádosti hello. Hello odpovědi ze serveru hello obsahuje všechny informace o tuto zálohu, včetně jeho SAS adresa URL.
> 
> 

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
