---
title: "Použití služby REST k zálohování a obnovení aplikací App Service"
description: "Naučte se používat rozhraní RESTful API volání pro zálohování a obnovení aplikace v Azure App Service"
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
ms.openlocfilehash: c1b8fc3be3af46279bf35bddbc82acf1827b9eb9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-rest-to-back-up-and-restore-app-service-apps"></a>Použití služby REST k zálohování a obnovení aplikací App Service
> [!div class="op_single_selector"]
> * [PowerShell](../app-service/app-service-powershell-backup.md)
> * [REST API](websites-csm-backup.md)
> 
> 

[Aplikace služby App Service](https://azure.microsoft.com/services/app-service/web/) lze zálohovat jako objekty BLOB v úložišti Azure. Zálohování může také obsahovat databáze aplikace. Pokud se někdy omylem odstraní aplikaci, nebo pokud aplikace potřebuje k vrátit na předchozí verzi, lze obnovit z jakékoli předchozí zálohy. Zálohy lze provést kdykoli na vyžádání nebo zálohování můžete naplánovat vhodných intervalech.

Tento článek vysvětluje, jak zálohovat a obnovit aplikace s žádostí o rozhraní RESTful API. Pokud chcete vytvořit a spravovat zálohy aplikace graficky prostřednictvím portálu Azure, najdete v článku [zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md)

<a name="gettingstarted"></a>

## <a name="getting-started"></a>Začínáme
K posílání požadavků REST, musíte znát vaší aplikace **název**, **skupiny prostředků**, a **id předplatného**. Tyto informace najdete kliknutím aplikace v rámci **služby App Service** okno [portál Azure](https://portal.azure.com). Příklady v tomto článku, jsme konfigurujete webu **backuprestoreapiexamples.azurewebsites.net**. Uloženo ve skupině prostředků výchozí-Web-WestUS a běží na předplatné s ID 00001111-2222-3333-4444-555566667777.

![Informace o ukázkovému webu][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>

## <a name="backup-and-restore-rest-api"></a>Zálohování a obnovení REST API
Nyní obsahuje několik příkladů, jak používat rozhraní REST API pro zálohování a obnovení aplikace. Každý příklad obsahuje obsah adresy URL a HTTP žádosti. Adresa URL ukázkových obsahuje zástupné symboly uzavřen do složených závorek, například {id předplatného}. Nahraďte zástupné symboly odpovídající informace pro vaši aplikaci. Pro referenci tady je vysvětlení jednotlivých zástupný symbol, který se zobrazí v ukázkových adresách URL.

* id předplatného – ID předplatného Azure obsahující aplikaci
* Název skupiny prostředků – – název skupiny prostředků obsahující aplikaci
* název – název aplikace
* zálohování id – ID aplikace zálohy

Kompletní dokumentaci k rozhraní API, včetně několika volitelné parametry, které můžou být součástí požadavku HTTP, najdete v článku [Průzkumníka prostředků Azure](https://resources.azure.com/).

<a name="backup-on-demand"></a>

## <a name="backup-an-app-on-demand"></a>Zálohování aplikace na vyžádání
Pro aplikaci Zálohování okamžitě, odeslání **POST** žádost o **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/ zálohování nebo**.

Zde je adresa URL, které bude vypadat takto pomocí našich příklad webu. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backup/**

Zadejte objekt JSON v textu požadavku k určení, který účet úložiště se má použít pro uložení zálohy. Objekt JSON musí mít vlastnost s názvem **storageAccountUrl**, který drží [SAS URL](../storage/common/storage-dotnet-shared-access-signature-part-1.md) udělení oprávnění k zápisu do Azure Storage kontejner, který obsahuje zálohy objektu blob. Pokud chcete zálohovat databáze, je třeba zadat seznam obsahující názvy, typy a připojovacích řetězců databází, které mají být zazálohovány.

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

Zálohování aplikace začne okamžitě, při přijetí žádosti. Proces zálohování může trvat dlouhou dobu pro dokončení. Odpověď HTTP, která obsahuje ID, které můžete použít v jiné požadavku na Zobrazit stav zálohování. Tady je příklad textu odpovědi HTTP na našem zálohování požadavek.

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
> Chybové zprávy naleznete ve vlastnosti protokolu odpovědi HTTP.
> 
> 

<a name="schedule-automatic-backups"></a>

## <a name="schedule-automatic-backups"></a>Naplánovat automatické zálohování
Kromě zálohování aplikaci na vyžádání, můžete také naplánovat zálohování provést automaticky.

### <a name="set-up-a-new-automatic-backup-schedule"></a>Nastavit nový plán automatické zálohování
Pokud chcete nastavit plán zálohování, odeslání **PUT** žádost o **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/ Zálohování**.

Zde je co vypadá adresa URL pro náš příklad Web. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/config/Backup**

Objekt JSON, který určuje konfigurace zálohování musí mít textu požadavku. Tady je příklad s požadovanými parametry.

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
        "enabled": "True", // Must be set to true to enable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

Tento příklad konfiguruje aplikaci automaticky zálohovat každých sedm dní. Parametry **frequencyInterval** a **frequencyUnit** společně určují, jak často zálohám. Platné hodnoty pro **frequencyUnit** jsou **hodinu** a **den**. Zálohování aplikace každých 12 hodin, například nastavte frequencyInterval 12 a frequencyUnit hodinu.

Starší zálohy se automaticky odeberou z účtu úložiště. Můžete určit, kolik zálohování může být nastavením **retentionPeriodInDays** parametr. Pokud chcete mít aspoň jednu zálohu uložené, bez ohledu na to, jak staré, nastavení vždy **keepAtLeastOneBackup** na hodnotu true.

### <a name="get-the-automatic-backup-schedule"></a>Získat automatické plán zálohování
Konfigurace zálohování aplikace získáte odesílat **POST** požadavek na adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites / {name} / config/zálohování a seznam**.

Adresa URL pro náš příklad web je **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.

<a name="get-backup-status"></a>

## <a name="get-the-status-of-a-backup"></a>Načíst stav zálohování
V závislosti na tom, jak velké je aplikace může trvat zálohu dobu. Zálohování může také selže, vypršení časového limitu nebo částečně úspěšné. Chcete-li zobrazit stav záloh všechny aplikace, odešlete **získat** požadavek na adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/ lokality / {name} / zálohy**.

Pokud chcete zobrazit stav konkrétních zálohování, odeslat požadavek GET na adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}** .

Zde je co vypadá adresa URL pro náš příklad Web. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**

Text odpovědi obsahuje objekt JSON, podobně jako tento příklad.

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

Stav zálohy je výčtového typu. Zde je každý možné stavu.

* 0 – Probíhá zpracování: Zálohování běží, ale ještě se nedokončilo.
* 1 – Selhalo: Zálohování bylo neúspěšné.
* 2 – Úspěch: Zálohování je úspěšně dokončené.
* 3 – TimedOut: Zálohování nebylo dokončeno v čase a byla zrušena.
* 4 – Vytvořeno: Žádost o zálohování je ve frontě, ale zálohování nezačalo.
* 5 – Vynecháno: Záloha neproběhla, protože plán aktivuje příliš mnoho záloh.
* 6 – Částečný úspěch: Zálohování bylo úspěšné, ale některé soubory se nezálohovaly, protože se nedaly přečíst. K tomu obvykle dochází, když mají soubory výhradní zámek.
* 7 – Probíhá odstranění: Požádali jste o odstraněné zálohy, ale k odstranění ještě nedošlo.
* 8 – Odstranění selhalo: Zálohu nešlo odstranit. Příčinou může být vypršení platnosti adresy URL SAS použité k vytvoření zálohy.
* 9 – Odstraněno: Záloha je úspěšně odstraněná.

<a name="restore-app"></a>

## <a name="restore-an-app-from-a-backup"></a>Obnovení ze zálohy aplikace
Pokud vaše aplikace byl odstraněn, nebo pokud se chcete vrátit k předchozí verzi aplikace, můžete aplikaci obnovit ze zálohy. K vyvolání obnovení, odeslání **POST** požadavek na adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups / {zálohování id} a obnově**.

Zde je co vypadá adresa URL pro náš příklad Web. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/Restore**

V těle žádosti odeslat objekt JSON, který obsahuje vlastnosti pro operaci obnovení. Tady je příklad, který obsahuje všechny požadované vlastnosti:

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
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL to storage container containing your website backup
    }
}
```

### <a name="restore-to-a-new-app"></a>Obnovit do nové aplikace
V některých případech můžete chtít vytvořit novou aplikaci, když obnovíte zálohu místo přepsal stávající aplikace. Chcete-li to provést, změňte adresu URL požadavku tak, aby odkazoval na novou aplikaci, kterou chcete vytvořit a změnit **přepsat** vlastnost ve formátu JSON na **false**.

<a name="delete-app-backup"></a>

## <a name="delete-an-app-backup"></a>Odstranit zálohu aplikace
Pokud chcete odstranit zálohy, odeslání **odstranit** požadavek na adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites / {name} {zálohování id} /backups/**.

Zde je co vypadá adresa URL pro náš příklad Web. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**

<a name="manage-sas-url"></a>

## <a name="manage-a-backups-sas-url"></a>Správa zálohování SAS adresu URL
Azure App Service se pokusí odstranit zálohování z Azure Storage pomocí adresy URL SAS, který byl zadán při vytvoření zálohy. Pokud tato adresa URL SAS již není platná, nelze odstranit zálohování přes rozhraní REST API. Však můžete aktualizovat SAS URL přidružené zálohy odesláním **POST** požadavek na adresu URL **https://management.azure.com/subscriptions/ {id předplatného} /resourceGroups/ {resource-group name} / providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.

Zde je co vypadá adresa URL pro náš příklad Web. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/list**

V těle žádosti odeslat objekt JSON, který obsahuje nové adrese URL SAS. Tady je příklad.

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

> [!NOTE]
> Z bezpečnostních důvodů SAS URL přidružené zálohy nevrátí při odesílání požadavek GET pro konkrétní zálohování. Pokud chcete zobrazit SAS URL přidružené zálohy, odeslat požadavek POST na stejnou adresu URL výše. Zahrňte prázdný objekt JSON v textu požadavku. Odpověď ze serveru obsahuje všechny informace o tuto zálohu, včetně jeho SAS adresa URL.
> 
> 

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
