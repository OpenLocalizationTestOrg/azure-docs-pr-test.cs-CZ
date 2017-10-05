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
# <a name="use-rest-to-back-up-and-restore-app-service-apps"></a><span data-ttu-id="3c022-103">Použití služby REST k zálohování a obnovení aplikací App Service</span><span class="sxs-lookup"><span data-stu-id="3c022-103">Use REST to back up and restore App Service apps</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3c022-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3c022-104">PowerShell</span></span>](../app-service/app-service-powershell-backup.md)
> * [<span data-ttu-id="3c022-105">REST API</span><span class="sxs-lookup"><span data-stu-id="3c022-105">REST API</span></span>](websites-csm-backup.md)
> 
> 

<span data-ttu-id="3c022-106">[Aplikace služby App Service](https://azure.microsoft.com/services/app-service/web/) lze zálohovat jako objekty BLOB v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="3c022-106">[App Service apps](https://azure.microsoft.com/services/app-service/web/) can be backed up as blobs in Azure storage.</span></span> <span data-ttu-id="3c022-107">Zálohování může také obsahovat databáze aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c022-107">The backup can also contain the app’s databases.</span></span> <span data-ttu-id="3c022-108">Pokud se někdy omylem odstraní aplikaci, nebo pokud aplikace potřebuje k vrátit na předchozí verzi, lze obnovit z jakékoli předchozí zálohy.</span><span class="sxs-lookup"><span data-stu-id="3c022-108">If the app is ever accidentally deleted, or if the app needs to be reverted to a previous version, it can be restored from any previous backup.</span></span> <span data-ttu-id="3c022-109">Zálohy lze provést kdykoli na vyžádání nebo zálohování můžete naplánovat vhodných intervalech.</span><span class="sxs-lookup"><span data-stu-id="3c022-109">Backups can be done at any time on demand, or backups can be scheduled at suitable intervals.</span></span>

<span data-ttu-id="3c022-110">Tento článek vysvětluje, jak zálohovat a obnovit aplikace s žádostí o rozhraní RESTful API.</span><span class="sxs-lookup"><span data-stu-id="3c022-110">This article explains how to backup and restore an app with RESTful API requests.</span></span> <span data-ttu-id="3c022-111">Pokud chcete vytvořit a spravovat zálohy aplikace graficky prostřednictvím portálu Azure, najdete v článku [zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md)</span><span class="sxs-lookup"><span data-stu-id="3c022-111">If you would like to create and manage app backups graphically through the Azure portal, see [Back up a web app in Azure App Service](web-sites-backup.md)</span></span>

<a name="gettingstarted"></a>

## <a name="getting-started"></a><span data-ttu-id="3c022-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="3c022-112">Getting Started</span></span>
<span data-ttu-id="3c022-113">K posílání požadavků REST, musíte znát vaší aplikace **název**, **skupiny prostředků**, a **id předplatného**. Tyto informace najdete kliknutím aplikace v rámci **služby App Service** okno [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3c022-113">To send REST requests, you need to know your app’s **name**, **resource group**, and **subscription id**. This information can be found by clicking your app in the **App Service** blade of the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="3c022-114">Příklady v tomto článku, jsme konfigurujete webu **backuprestoreapiexamples.azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="3c022-114">For the examples in this article, we are configuring the website **backuprestoreapiexamples.azurewebsites.net**.</span></span> <span data-ttu-id="3c022-115">Uloženo ve skupině prostředků výchozí-Web-WestUS a běží na předplatné s ID 00001111-2222-3333-4444-555566667777.</span><span class="sxs-lookup"><span data-stu-id="3c022-115">It is stored in the Default-Web-WestUS resource group and is running on a subscription with the ID 00001111-2222-3333-4444-555566667777.</span></span>

![Informace o ukázkovému webu][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>

## <a name="backup-and-restore-rest-api"></a><span data-ttu-id="3c022-117">Zálohování a obnovení REST API</span><span class="sxs-lookup"><span data-stu-id="3c022-117">Backup and restore REST API</span></span>
<span data-ttu-id="3c022-118">Nyní obsahuje několik příkladů, jak používat rozhraní REST API pro zálohování a obnovení aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c022-118">We will now cover several examples of how to use the REST API to backup and restore an app.</span></span> <span data-ttu-id="3c022-119">Každý příklad obsahuje obsah adresy URL a HTTP žádosti.</span><span class="sxs-lookup"><span data-stu-id="3c022-119">Each example includes a URL and HTTP request body.</span></span> <span data-ttu-id="3c022-120">Adresa URL ukázkových obsahuje zástupné symboly uzavřen do složených závorek, například {id předplatného}.</span><span class="sxs-lookup"><span data-stu-id="3c022-120">The sample URL contains placeholders wrapped in curly braces, such as {subscription-id}.</span></span> <span data-ttu-id="3c022-121">Nahraďte zástupné symboly odpovídající informace pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3c022-121">Replace the placeholders with the corresponding information for your app.</span></span> <span data-ttu-id="3c022-122">Pro referenci tady je vysvětlení jednotlivých zástupný symbol, který se zobrazí v ukázkových adresách URL.</span><span class="sxs-lookup"><span data-stu-id="3c022-122">For reference, here is an explanation of each placeholder that appears in the example URLs.</span></span>

* <span data-ttu-id="3c022-123">id předplatného – ID předplatného Azure obsahující aplikaci</span><span class="sxs-lookup"><span data-stu-id="3c022-123">subscription-id – ID of the Azure subscription containing the app</span></span>
* <span data-ttu-id="3c022-124">Název skupiny prostředků – – název skupiny prostředků obsahující aplikaci</span><span class="sxs-lookup"><span data-stu-id="3c022-124">resource-group-name – Name of the resource group containing the app</span></span>
* <span data-ttu-id="3c022-125">název – název aplikace</span><span class="sxs-lookup"><span data-stu-id="3c022-125">name – Name of the app</span></span>
* <span data-ttu-id="3c022-126">zálohování id – ID aplikace zálohy</span><span class="sxs-lookup"><span data-stu-id="3c022-126">backup-id – ID of the app backup</span></span>

<span data-ttu-id="3c022-127">Kompletní dokumentaci k rozhraní API, včetně několika volitelné parametry, které můžou být součástí požadavku HTTP, najdete v článku [Průzkumníka prostředků Azure](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="3c022-127">For the complete documentation of the API, including several optional parameters that can be included in the HTTP request, see the [Azure Resource Explorer](https://resources.azure.com/).</span></span>

<a name="backup-on-demand"></a>

## <a name="backup-an-app-on-demand"></a><span data-ttu-id="3c022-128">Zálohování aplikace na vyžádání</span><span class="sxs-lookup"><span data-stu-id="3c022-128">Backup an app on demand</span></span>
<span data-ttu-id="3c022-129">Pro aplikaci Zálohování okamžitě, odeslání **POST** žádost o **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/ zálohování nebo**.</span><span class="sxs-lookup"><span data-stu-id="3c022-129">To back up an app immediately, send a **POST** request to **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backup/**.</span></span>

<span data-ttu-id="3c022-130">Zde je adresa URL, které bude vypadat takto pomocí našich příklad webu.</span><span class="sxs-lookup"><span data-stu-id="3c022-130">Here is what the URL looks like using our example website.</span></span> <span data-ttu-id="3c022-131">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backup/**</span><span class="sxs-lookup"><span data-stu-id="3c022-131">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backup/**</span></span>

<span data-ttu-id="3c022-132">Zadejte objekt JSON v textu požadavku k určení, který účet úložiště se má použít pro uložení zálohy.</span><span class="sxs-lookup"><span data-stu-id="3c022-132">Supply a JSON object in the body of your request to specify which storage account to use to store the backup.</span></span> <span data-ttu-id="3c022-133">Objekt JSON musí mít vlastnost s názvem **storageAccountUrl**, který drží [SAS URL](../storage/common/storage-dotnet-shared-access-signature-part-1.md) udělení oprávnění k zápisu do Azure Storage kontejner, který obsahuje zálohy objektu blob.</span><span class="sxs-lookup"><span data-stu-id="3c022-133">The JSON object must have a property named **storageAccountUrl**, which holds a [SAS URL](../storage/common/storage-dotnet-shared-access-signature-part-1.md) granting write access to the Azure Storage container that holds the backup blob.</span></span> <span data-ttu-id="3c022-134">Pokud chcete zálohovat databáze, je třeba zadat seznam obsahující názvy, typy a připojovacích řetězců databází, které mají být zazálohovány.</span><span class="sxs-lookup"><span data-stu-id="3c022-134">If you want to back up your databases, you must also supply a list containing the names, types, and connection strings of the databases to be backed up.</span></span>

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

<span data-ttu-id="3c022-135">Zálohování aplikace začne okamžitě, při přijetí žádosti.</span><span class="sxs-lookup"><span data-stu-id="3c022-135">A backup of the app begins immediately when the request is received.</span></span> <span data-ttu-id="3c022-136">Proces zálohování může trvat dlouhou dobu pro dokončení.</span><span class="sxs-lookup"><span data-stu-id="3c022-136">The backup process may take a long time to complete.</span></span> <span data-ttu-id="3c022-137">Odpověď HTTP, která obsahuje ID, které můžete použít v jiné požadavku na Zobrazit stav zálohování.</span><span class="sxs-lookup"><span data-stu-id="3c022-137">The HTTP response contains an ID that you can use in another request to see the status of the backup.</span></span> <span data-ttu-id="3c022-138">Tady je příklad textu odpovědi HTTP na našem zálohování požadavek.</span><span class="sxs-lookup"><span data-stu-id="3c022-138">Here is an example of the body of the HTTP response to our backup request.</span></span>

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
> <span data-ttu-id="3c022-139">Chybové zprávy naleznete ve vlastnosti protokolu odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="3c022-139">Error messages can be found in the log property of the HTTP response.</span></span>
> 
> 

<a name="schedule-automatic-backups"></a>

## <a name="schedule-automatic-backups"></a><span data-ttu-id="3c022-140">Naplánovat automatické zálohování</span><span class="sxs-lookup"><span data-stu-id="3c022-140">Schedule automatic backups</span></span>
<span data-ttu-id="3c022-141">Kromě zálohování aplikaci na vyžádání, můžete také naplánovat zálohování provést automaticky.</span><span class="sxs-lookup"><span data-stu-id="3c022-141">In addition to backing up an app on demand, you can also schedule a backup to happen automatically.</span></span>

### <a name="set-up-a-new-automatic-backup-schedule"></a><span data-ttu-id="3c022-142">Nastavit nový plán automatické zálohování</span><span class="sxs-lookup"><span data-stu-id="3c022-142">Set up a new automatic backup schedule</span></span>
<span data-ttu-id="3c022-143">Pokud chcete nastavit plán zálohování, odeslání **PUT** žádost o **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/ Zálohování**.</span><span class="sxs-lookup"><span data-stu-id="3c022-143">To set up a backup schedule, send a **PUT** request to **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup**.</span></span>

<span data-ttu-id="3c022-144">Zde je co vypadá adresa URL pro náš příklad Web.</span><span class="sxs-lookup"><span data-stu-id="3c022-144">Here is what the URL looks like for our example website.</span></span> <span data-ttu-id="3c022-145">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/config/Backup**</span><span class="sxs-lookup"><span data-stu-id="3c022-145">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup**</span></span>

<span data-ttu-id="3c022-146">Objekt JSON, který určuje konfigurace zálohování musí mít textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="3c022-146">The request body must have a JSON object that specifies the backup configuration.</span></span> <span data-ttu-id="3c022-147">Tady je příklad s požadovanými parametry.</span><span class="sxs-lookup"><span data-stu-id="3c022-147">Here is an example with all the required parameters.</span></span>

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

<span data-ttu-id="3c022-148">Tento příklad konfiguruje aplikaci automaticky zálohovat každých sedm dní.</span><span class="sxs-lookup"><span data-stu-id="3c022-148">This example configures the app to be automatically backed up every seven days.</span></span> <span data-ttu-id="3c022-149">Parametry **frequencyInterval** a **frequencyUnit** společně určují, jak často zálohám.</span><span class="sxs-lookup"><span data-stu-id="3c022-149">The parameters **frequencyInterval** and **frequencyUnit** together determine how often the backups happen.</span></span> <span data-ttu-id="3c022-150">Platné hodnoty pro **frequencyUnit** jsou **hodinu** a **den**.</span><span class="sxs-lookup"><span data-stu-id="3c022-150">Valid values for **frequencyUnit** are **hour** and **day**.</span></span> <span data-ttu-id="3c022-151">Zálohování aplikace každých 12 hodin, například nastavte frequencyInterval 12 a frequencyUnit hodinu.</span><span class="sxs-lookup"><span data-stu-id="3c022-151">For example, to back up an app every 12 hours, set frequencyInterval to 12 and frequencyUnit to hour.</span></span>

<span data-ttu-id="3c022-152">Starší zálohy se automaticky odeberou z účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="3c022-152">Old backups are automatically removed from the storage account.</span></span> <span data-ttu-id="3c022-153">Můžete určit, kolik zálohování může být nastavením **retentionPeriodInDays** parametr.</span><span class="sxs-lookup"><span data-stu-id="3c022-153">You can control how old the backups can be by setting the **retentionPeriodInDays** parameter.</span></span> <span data-ttu-id="3c022-154">Pokud chcete mít aspoň jednu zálohu uložené, bez ohledu na to, jak staré, nastavení vždy **keepAtLeastOneBackup** na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="3c022-154">If you want to always have at least one backup saved, regardless of how old it is, set **keepAtLeastOneBackup** to true.</span></span>

### <a name="get-the-automatic-backup-schedule"></a><span data-ttu-id="3c022-155">Získat automatické plán zálohování</span><span class="sxs-lookup"><span data-stu-id="3c022-155">Get the automatic backup schedule</span></span>
<span data-ttu-id="3c022-156">Konfigurace zálohování aplikace získáte odesílat **POST** požadavek na adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites / {name} / config/zálohování a seznam**.</span><span class="sxs-lookup"><span data-stu-id="3c022-156">To get an app’s backup configuration, send a **POST** request to the URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup/list**.</span></span>

<span data-ttu-id="3c022-157">Adresa URL pro náš příklad web je **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.</span><span class="sxs-lookup"><span data-stu-id="3c022-157">The URL for our example site is **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.</span></span>

<a name="get-backup-status"></a>

## <a name="get-the-status-of-a-backup"></a><span data-ttu-id="3c022-158">Načíst stav zálohování</span><span class="sxs-lookup"><span data-stu-id="3c022-158">Get the status of a backup</span></span>
<span data-ttu-id="3c022-159">V závislosti na tom, jak velké je aplikace může trvat zálohu dobu.</span><span class="sxs-lookup"><span data-stu-id="3c022-159">Depending on how large the app is, a backup may take a while to complete.</span></span> <span data-ttu-id="3c022-160">Zálohování může také selže, vypršení časového limitu nebo částečně úspěšné.</span><span class="sxs-lookup"><span data-stu-id="3c022-160">Backups might also fail, time out, or partially succeed.</span></span> <span data-ttu-id="3c022-161">Chcete-li zobrazit stav záloh všechny aplikace, odešlete **získat** požadavek na adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/ lokality / {name} / zálohy**.</span><span class="sxs-lookup"><span data-stu-id="3c022-161">To see the status of all an app’s backups, send a **GET** request to the URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups**.</span></span>

<span data-ttu-id="3c022-162">Pokud chcete zobrazit stav konkrétních zálohování, odeslat požadavek GET na adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}** .</span><span class="sxs-lookup"><span data-stu-id="3c022-162">To see the status of a specific backup, send a GET request to the URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.</span></span>

<span data-ttu-id="3c022-163">Zde je co vypadá adresa URL pro náš příklad Web.</span><span class="sxs-lookup"><span data-stu-id="3c022-163">Here is what the URL looks like for our example website.</span></span> <span data-ttu-id="3c022-164">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**</span><span class="sxs-lookup"><span data-stu-id="3c022-164">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**</span></span>

<span data-ttu-id="3c022-165">Text odpovědi obsahuje objekt JSON, podobně jako tento příklad.</span><span class="sxs-lookup"><span data-stu-id="3c022-165">The response body contains a JSON object similar to this example.</span></span>

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

<span data-ttu-id="3c022-166">Stav zálohy je výčtového typu.</span><span class="sxs-lookup"><span data-stu-id="3c022-166">The status of a backup is an enumerated type.</span></span> <span data-ttu-id="3c022-167">Zde je každý možné stavu.</span><span class="sxs-lookup"><span data-stu-id="3c022-167">Here is every possible state.</span></span>

* <span data-ttu-id="3c022-168">0 – Probíhá zpracování: Zálohování běží, ale ještě se nedokončilo.</span><span class="sxs-lookup"><span data-stu-id="3c022-168">0 – InProgress: The backup has been started but has not yet completed.</span></span>
* <span data-ttu-id="3c022-169">1 – Selhalo: Zálohování bylo neúspěšné.</span><span class="sxs-lookup"><span data-stu-id="3c022-169">1 – Failed: The backup was unsuccessful.</span></span>
* <span data-ttu-id="3c022-170">2 – Úspěch: Zálohování je úspěšně dokončené.</span><span class="sxs-lookup"><span data-stu-id="3c022-170">2 – Succeeded: The backup completed successfully.</span></span>
* <span data-ttu-id="3c022-171">3 – TimedOut: Zálohování nebylo dokončeno v čase a byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="3c022-171">3 – TimedOut: The backup did not finish in time and was canceled.</span></span>
* <span data-ttu-id="3c022-172">4 – Vytvořeno: Žádost o zálohování je ve frontě, ale zálohování nezačalo.</span><span class="sxs-lookup"><span data-stu-id="3c022-172">4 – Created: The backup request is queued but has not been started.</span></span>
* <span data-ttu-id="3c022-173">5 – Vynecháno: Záloha neproběhla, protože plán aktivuje příliš mnoho záloh.</span><span class="sxs-lookup"><span data-stu-id="3c022-173">5 – Skipped: The backup did not proceed due to a schedule triggering too many backups.</span></span>
* <span data-ttu-id="3c022-174">6 – Částečný úspěch: Zálohování bylo úspěšné, ale některé soubory se nezálohovaly, protože se nedaly přečíst.</span><span class="sxs-lookup"><span data-stu-id="3c022-174">6 – PartiallySucceeded: The backup succeeded, but some files were not backed up because they could not be read.</span></span> <span data-ttu-id="3c022-175">K tomu obvykle dochází, když mají soubory výhradní zámek.</span><span class="sxs-lookup"><span data-stu-id="3c022-175">This usually happens because an exclusive lock was placed on the files.</span></span>
* <span data-ttu-id="3c022-176">7 – Probíhá odstranění: Požádali jste o odstraněné zálohy, ale k odstranění ještě nedošlo.</span><span class="sxs-lookup"><span data-stu-id="3c022-176">7 – DeleteInProgress: The backup has been requested to be deleted, but has not yet been deleted.</span></span>
* <span data-ttu-id="3c022-177">8 – Odstranění selhalo: Zálohu nešlo odstranit.</span><span class="sxs-lookup"><span data-stu-id="3c022-177">8 – DeleteFailed: The backup could not be deleted.</span></span> <span data-ttu-id="3c022-178">Příčinou může být vypršení platnosti adresy URL SAS použité k vytvoření zálohy.</span><span class="sxs-lookup"><span data-stu-id="3c022-178">This might happen because the SAS URL that was used to create the backup has expired.</span></span>
* <span data-ttu-id="3c022-179">9 – Odstraněno: Záloha je úspěšně odstraněná.</span><span class="sxs-lookup"><span data-stu-id="3c022-179">9 – Deleted: The backup was deleted successfully.</span></span>

<a name="restore-app"></a>

## <a name="restore-an-app-from-a-backup"></a><span data-ttu-id="3c022-180">Obnovení ze zálohy aplikace</span><span class="sxs-lookup"><span data-stu-id="3c022-180">Restore an app from a backup</span></span>
<span data-ttu-id="3c022-181">Pokud vaše aplikace byl odstraněn, nebo pokud se chcete vrátit k předchozí verzi aplikace, můžete aplikaci obnovit ze zálohy.</span><span class="sxs-lookup"><span data-stu-id="3c022-181">If your app has been deleted, or if you want to revert your app to a previous version, you can restore the app from a backup.</span></span> <span data-ttu-id="3c022-182">K vyvolání obnovení, odeslání **POST** požadavek na adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups / {zálohování id} a obnově**.</span><span class="sxs-lookup"><span data-stu-id="3c022-182">To invoke a restore, send a **POST** request to the URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/restore**.</span></span>

<span data-ttu-id="3c022-183">Zde je co vypadá adresa URL pro náš příklad Web.</span><span class="sxs-lookup"><span data-stu-id="3c022-183">Here is what the URL looks like for our example website.</span></span> <span data-ttu-id="3c022-184">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/Restore**</span><span class="sxs-lookup"><span data-stu-id="3c022-184">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/restore**</span></span>

<span data-ttu-id="3c022-185">V těle žádosti odeslat objekt JSON, který obsahuje vlastnosti pro operaci obnovení.</span><span class="sxs-lookup"><span data-stu-id="3c022-185">In the request body, send a JSON object that contains the properties for the restore operation.</span></span> <span data-ttu-id="3c022-186">Tady je příklad, který obsahuje všechny požadované vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="3c022-186">Here is an example containing all required properties:</span></span>

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

### <a name="restore-to-a-new-app"></a><span data-ttu-id="3c022-187">Obnovit do nové aplikace</span><span class="sxs-lookup"><span data-stu-id="3c022-187">Restore to a new app</span></span>
<span data-ttu-id="3c022-188">V některých případech můžete chtít vytvořit novou aplikaci, když obnovíte zálohu místo přepsal stávající aplikace.</span><span class="sxs-lookup"><span data-stu-id="3c022-188">Sometimes you might want to create a new app when you restore a backup, instead of overwriting an already existing app.</span></span> <span data-ttu-id="3c022-189">Chcete-li to provést, změňte adresu URL požadavku tak, aby odkazoval na novou aplikaci, kterou chcete vytvořit a změnit **přepsat** vlastnost ve formátu JSON na **false**.</span><span class="sxs-lookup"><span data-stu-id="3c022-189">To do this, change the request URL to point to the new app you want to create, and change the **overwrite** property in the JSON to **false**.</span></span>

<a name="delete-app-backup"></a>

## <a name="delete-an-app-backup"></a><span data-ttu-id="3c022-190">Odstranit zálohu aplikace</span><span class="sxs-lookup"><span data-stu-id="3c022-190">Delete an app backup</span></span>
<span data-ttu-id="3c022-191">Pokud chcete odstranit zálohy, odeslání **odstranit** požadavek na adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites / {name} {zálohování id} /backups/**.</span><span class="sxs-lookup"><span data-stu-id="3c022-191">If you would like to delete a backup, send a **DELETE** request to the URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.</span></span>

<span data-ttu-id="3c022-192">Zde je co vypadá adresa URL pro náš příklad Web.</span><span class="sxs-lookup"><span data-stu-id="3c022-192">Here is what the URL looks like for our example website.</span></span> <span data-ttu-id="3c022-193">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**</span><span class="sxs-lookup"><span data-stu-id="3c022-193">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**</span></span>

<a name="manage-sas-url"></a>

## <a name="manage-a-backups-sas-url"></a><span data-ttu-id="3c022-194">Správa zálohování SAS adresu URL</span><span class="sxs-lookup"><span data-stu-id="3c022-194">Manage a backup’s SAS URL</span></span>
<span data-ttu-id="3c022-195">Azure App Service se pokusí odstranit zálohování z Azure Storage pomocí adresy URL SAS, který byl zadán při vytvoření zálohy.</span><span class="sxs-lookup"><span data-stu-id="3c022-195">Azure App Service will attempt to delete your backup from Azure Storage using the SAS URL that was provided when the backup was created.</span></span> <span data-ttu-id="3c022-196">Pokud tato adresa URL SAS již není platná, nelze odstranit zálohování přes rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="3c022-196">If this SAS URL is no longer valid, the backup cannot be deleted through the REST API.</span></span> <span data-ttu-id="3c022-197">Však můžete aktualizovat SAS URL přidružené zálohy odesláním **POST** požadavek na adresu URL **https://management.azure.com/subscriptions/ {id předplatného} /resourceGroups/ {resource-group name} / providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.</span><span class="sxs-lookup"><span data-stu-id="3c022-197">However, you can update the SAS URL associated with a backup by sending a **POST** request to the URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.</span></span>

<span data-ttu-id="3c022-198">Zde je co vypadá adresa URL pro náš příklad Web.</span><span class="sxs-lookup"><span data-stu-id="3c022-198">Here is what the URL looks like for our example website.</span></span> <span data-ttu-id="3c022-199">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/list**</span><span class="sxs-lookup"><span data-stu-id="3c022-199">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/list**</span></span>

<span data-ttu-id="3c022-200">V těle žádosti odeslat objekt JSON, který obsahuje nové adrese URL SAS.</span><span class="sxs-lookup"><span data-stu-id="3c022-200">In the request body, send a JSON object that contains the new SAS URL.</span></span> <span data-ttu-id="3c022-201">Tady je příklad.</span><span class="sxs-lookup"><span data-stu-id="3c022-201">Here is an example.</span></span>

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

> [!NOTE]
> <span data-ttu-id="3c022-202">Z bezpečnostních důvodů SAS URL přidružené zálohy nevrátí při odesílání požadavek GET pro konkrétní zálohování.</span><span class="sxs-lookup"><span data-stu-id="3c022-202">For security reasons, the SAS URL associated with a backup is not returned when sending a GET request for a specific backup.</span></span> <span data-ttu-id="3c022-203">Pokud chcete zobrazit SAS URL přidružené zálohy, odeslat požadavek POST na stejnou adresu URL výše.</span><span class="sxs-lookup"><span data-stu-id="3c022-203">If you want to view the SAS URL associated with a backup, send a POST request to the same URL above.</span></span> <span data-ttu-id="3c022-204">Zahrňte prázdný objekt JSON v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="3c022-204">Include an empty JSON object in the request body.</span></span> <span data-ttu-id="3c022-205">Odpověď ze serveru obsahuje všechny informace o tuto zálohu, včetně jeho SAS adresa URL.</span><span class="sxs-lookup"><span data-stu-id="3c022-205">The response from the server contains all of that backup’s information, including its SAS URL.</span></span>
> 
> 

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
