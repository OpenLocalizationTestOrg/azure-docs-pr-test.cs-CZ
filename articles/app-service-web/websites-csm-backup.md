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
# <a name="use-rest-tooback-up-and-restore-app-service-apps"></a><span data-ttu-id="133b2-103">Použít REST tooback a obnovení aplikace služby App Service</span><span class="sxs-lookup"><span data-stu-id="133b2-103">Use REST tooback up and restore App Service apps</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="133b2-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="133b2-104">PowerShell</span></span>](../app-service/app-service-powershell-backup.md)
> * [<span data-ttu-id="133b2-105">REST API</span><span class="sxs-lookup"><span data-stu-id="133b2-105">REST API</span></span>](websites-csm-backup.md)
> 
> 

<span data-ttu-id="133b2-106">[Aplikace služby App Service](https://azure.microsoft.com/services/app-service/web/) lze zálohovat jako objekty BLOB v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="133b2-106">[App Service apps](https://azure.microsoft.com/services/app-service/web/) can be backed up as blobs in Azure storage.</span></span> <span data-ttu-id="133b2-107">Hello zálohování může také obsahovat aplikace hello databáze.</span><span class="sxs-lookup"><span data-stu-id="133b2-107">hello backup can also contain hello app’s databases.</span></span> <span data-ttu-id="133b2-108">Pokud aplikace hello někdy omylem odstraněna, nebo pokud toobe potřebám aplikace hello vrácení předchozí verze tooa, lze obnovit z jakékoli předchozí zálohy.</span><span class="sxs-lookup"><span data-stu-id="133b2-108">If hello app is ever accidentally deleted, or if hello app needs toobe reverted tooa previous version, it can be restored from any previous backup.</span></span> <span data-ttu-id="133b2-109">Zálohy lze provést kdykoli na vyžádání nebo zálohování můžete naplánovat vhodných intervalech.</span><span class="sxs-lookup"><span data-stu-id="133b2-109">Backups can be done at any time on demand, or backups can be scheduled at suitable intervals.</span></span>

<span data-ttu-id="133b2-110">Tento článek vysvětluje, jak toobackup a obnovení aplikace pomocí rozhraní RESTful API požadavky.</span><span class="sxs-lookup"><span data-stu-id="133b2-110">This article explains how toobackup and restore an app with RESTful API requests.</span></span> <span data-ttu-id="133b2-111">Pokud chcete vytvořit toocreate a spravovat zálohy aplikace graficky prostřednictvím hello portálu Azure, najdete v části [zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md)</span><span class="sxs-lookup"><span data-stu-id="133b2-111">If you would like toocreate and manage app backups graphically through hello Azure portal, see [Back up a web app in Azure App Service](web-sites-backup.md)</span></span>

<a name="gettingstarted"></a>

## <a name="getting-started"></a><span data-ttu-id="133b2-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="133b2-112">Getting Started</span></span>
<span data-ttu-id="133b2-113">toosend REST požadavky, je nutné tooknow vaší aplikace **název**, **skupiny prostředků**, a **id předplatného**. Tyto informace najdete kliknutím aplikace v rámci hello **služby App Service** okno hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="133b2-113">toosend REST requests, you need tooknow your app’s **name**, **resource group**, and **subscription id**. This information can be found by clicking your app in hello **App Service** blade of hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="133b2-114">Hello příklady v tomto článku, jsme konfigurujete hello webu **backuprestoreapiexamples.azurewebsites.net**.</span><span class="sxs-lookup"><span data-stu-id="133b2-114">For hello examples in this article, we are configuring hello website **backuprestoreapiexamples.azurewebsites.net**.</span></span> <span data-ttu-id="133b2-115">Uloženo ve skupině prostředků výchozí-Web-WestUS hello a běží na předplatné s ID 00001111-2222-3333-4444-555566667777 hello.</span><span class="sxs-lookup"><span data-stu-id="133b2-115">It is stored in hello Default-Web-WestUS resource group and is running on a subscription with hello ID 00001111-2222-3333-4444-555566667777.</span></span>

![Informace o ukázkovému webu][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>

## <a name="backup-and-restore-rest-api"></a><span data-ttu-id="133b2-117">Zálohování a obnovení REST API</span><span class="sxs-lookup"><span data-stu-id="133b2-117">Backup and restore REST API</span></span>
<span data-ttu-id="133b2-118">Nyní obsahuje několik příkladů, jak toouse hello toobackup REST API a obnovení aplikace.</span><span class="sxs-lookup"><span data-stu-id="133b2-118">We will now cover several examples of how toouse hello REST API toobackup and restore an app.</span></span> <span data-ttu-id="133b2-119">Každý příklad obsahuje obsah adresy URL a HTTP žádosti.</span><span class="sxs-lookup"><span data-stu-id="133b2-119">Each example includes a URL and HTTP request body.</span></span> <span data-ttu-id="133b2-120">Adresa URL ukázkových Hello obsahuje zástupné symboly uzavřen do složených závorek, například {id předplatného}.</span><span class="sxs-lookup"><span data-stu-id="133b2-120">hello sample URL contains placeholders wrapped in curly braces, such as {subscription-id}.</span></span> <span data-ttu-id="133b2-121">Nahraďte zástupné symboly hello hello odpovídající informace pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="133b2-121">Replace hello placeholders with hello corresponding information for your app.</span></span> <span data-ttu-id="133b2-122">Pro referenci tady je vysvětlení jednotlivých zástupný symbol, který se zobrazí v ukázkových adresách URL hello.</span><span class="sxs-lookup"><span data-stu-id="133b2-122">For reference, here is an explanation of each placeholder that appears in hello example URLs.</span></span>

* <span data-ttu-id="133b2-123">id předplatného – ID aplikace obsahující hello hello předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="133b2-123">subscription-id – ID of hello Azure subscription containing hello app</span></span>
* <span data-ttu-id="133b2-124">Název skupiny prostředků – – název hello prostředků skupiny obsahující hello aplikace</span><span class="sxs-lookup"><span data-stu-id="133b2-124">resource-group-name – Name of hello resource group containing hello app</span></span>
* <span data-ttu-id="133b2-125">název – název aplikace hello</span><span class="sxs-lookup"><span data-stu-id="133b2-125">name – Name of hello app</span></span>
* <span data-ttu-id="133b2-126">zálohování id – ID zálohy aplikace hello</span><span class="sxs-lookup"><span data-stu-id="133b2-126">backup-id – ID of hello app backup</span></span>

<span data-ttu-id="133b2-127">Hello kompletní dokumentaci o hello rozhraní API, včetně několika volitelné parametry, které můžou být součástí hello HTTP požadavku, najdete v části hello [Průzkumníka prostředků Azure](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="133b2-127">For hello complete documentation of hello API, including several optional parameters that can be included in hello HTTP request, see hello [Azure Resource Explorer](https://resources.azure.com/).</span></span>

<a name="backup-on-demand"></a>

## <a name="backup-an-app-on-demand"></a><span data-ttu-id="133b2-128">Zálohování aplikace na vyžádání</span><span class="sxs-lookup"><span data-stu-id="133b2-128">Backup an app on demand</span></span>
<span data-ttu-id="133b2-129">tooback se aplikace hned, odešle **POST** požadavku příliš**https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/ zálohování nebo**.</span><span class="sxs-lookup"><span data-stu-id="133b2-129">tooback up an app immediately, send a **POST** request too**https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backup/**.</span></span>

<span data-ttu-id="133b2-130">Zde je co hello vypadá adresa URL pomocí našich příklad webu.</span><span class="sxs-lookup"><span data-stu-id="133b2-130">Here is what hello URL looks like using our example website.</span></span> <span data-ttu-id="133b2-131">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backup/**</span><span class="sxs-lookup"><span data-stu-id="133b2-131">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backup/**</span></span>

<span data-ttu-id="133b2-132">Zadejte objekt JSON v textu hello vaše žádost o toospecify, které toostore toouse účet úložiště hello zálohování.</span><span class="sxs-lookup"><span data-stu-id="133b2-132">Supply a JSON object in hello body of your request toospecify which storage account toouse toostore hello backup.</span></span> <span data-ttu-id="133b2-133">Objekt JSON Hello musí mít vlastnost s názvem **storageAccountUrl**, který drží [SAS URL](../storage/common/storage-dotnet-shared-access-signature-part-1.md) udělení oprávnění k zápisu toohello Azure Storage kontejner, který obsahuje objekt blob zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="133b2-133">hello JSON object must have a property named **storageAccountUrl**, which holds a [SAS URL](../storage/common/storage-dotnet-shared-access-signature-part-1.md) granting write access toohello Azure Storage container that holds hello backup blob.</span></span> <span data-ttu-id="133b2-134">Pokud chcete tooback zálohu vaší databáze, je třeba zadat seznam obsahující hello názvy, typy a připojovací řetězce toobe databáze hello zálohovat.</span><span class="sxs-lookup"><span data-stu-id="133b2-134">If you want tooback up your databases, you must also supply a list containing hello names, types, and connection strings of hello databases toobe backed up.</span></span>

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

<span data-ttu-id="133b2-135">Zálohování aplikace hello začne okamžitě, když obdrží žádost hello.</span><span class="sxs-lookup"><span data-stu-id="133b2-135">A backup of hello app begins immediately when hello request is received.</span></span> <span data-ttu-id="133b2-136">proces zálohování Hello může trvat dlouhou dobu toocomplete.</span><span class="sxs-lookup"><span data-stu-id="133b2-136">hello backup process may take a long time toocomplete.</span></span> <span data-ttu-id="133b2-137">Hello HTTP odpovědi obsahuje ID, které můžete použít v jiné stav žádosti o toosee hello hello zálohy.</span><span class="sxs-lookup"><span data-stu-id="133b2-137">hello HTTP response contains an ID that you can use in another request toosee hello status of hello backup.</span></span> <span data-ttu-id="133b2-138">Tady je příklad textu hello hello HTTP odpovědi tooour zálohování požadavku.</span><span class="sxs-lookup"><span data-stu-id="133b2-138">Here is an example of hello body of hello HTTP response tooour backup request.</span></span>

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
> <span data-ttu-id="133b2-139">Chybové zprávy naleznete ve vlastnosti protokolu hello hello odpověď HTTP.</span><span class="sxs-lookup"><span data-stu-id="133b2-139">Error messages can be found in hello log property of hello HTTP response.</span></span>
> 
> 

<a name="schedule-automatic-backups"></a>

## <a name="schedule-automatic-backups"></a><span data-ttu-id="133b2-140">Naplánovat automatické zálohování</span><span class="sxs-lookup"><span data-stu-id="133b2-140">Schedule automatic backups</span></span>
<span data-ttu-id="133b2-141">V přidání toobacking aplikaci na vyžádání můžete také naplánovat zálohování toohappen automaticky.</span><span class="sxs-lookup"><span data-stu-id="133b2-141">In addition toobacking up an app on demand, you can also schedule a backup toohappen automatically.</span></span>

### <a name="set-up-a-new-automatic-backup-schedule"></a><span data-ttu-id="133b2-142">Nastavit nový plán automatické zálohování</span><span class="sxs-lookup"><span data-stu-id="133b2-142">Set up a new automatic backup schedule</span></span>
<span data-ttu-id="133b2-143">Odeslat tooset si plán zálohování **PUT** požadavku příliš**https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config / backup**.</span><span class="sxs-lookup"><span data-stu-id="133b2-143">tooset up a backup schedule, send a **PUT** request too**https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup**.</span></span>

<span data-ttu-id="133b2-144">Zde je co hello vypadá adresa URL pro náš příklad Web.</span><span class="sxs-lookup"><span data-stu-id="133b2-144">Here is what hello URL looks like for our example website.</span></span> <span data-ttu-id="133b2-145">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/config/Backup**</span><span class="sxs-lookup"><span data-stu-id="133b2-145">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup**</span></span>

<span data-ttu-id="133b2-146">text žádosti Hello musí mít objekt JSON, který určuje hello konfigurace zálohy.</span><span class="sxs-lookup"><span data-stu-id="133b2-146">hello request body must have a JSON object that specifies hello backup configuration.</span></span> <span data-ttu-id="133b2-147">Tady je příklad se všemi hello požadované parametry.</span><span class="sxs-lookup"><span data-stu-id="133b2-147">Here is an example with all hello required parameters.</span></span>

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

<span data-ttu-id="133b2-148">Tento příklad konfiguruje toobe aplikace hello automaticky zálohovat každých sedm dní.</span><span class="sxs-lookup"><span data-stu-id="133b2-148">This example configures hello app toobe automatically backed up every seven days.</span></span> <span data-ttu-id="133b2-149">Hello parametry **frequencyInterval** a **frequencyUnit** společně určují, jak často hello zálohám.</span><span class="sxs-lookup"><span data-stu-id="133b2-149">hello parameters **frequencyInterval** and **frequencyUnit** together determine how often hello backups happen.</span></span> <span data-ttu-id="133b2-150">Platné hodnoty pro **frequencyUnit** jsou **hodinu** a **den**.</span><span class="sxs-lookup"><span data-stu-id="133b2-150">Valid values for **frequencyUnit** are **hour** and **day**.</span></span> <span data-ttu-id="133b2-151">Například tooback aplikaci každých 12 hodin, nastavte frequencyInterval too12 a frequencyUnit toohour.</span><span class="sxs-lookup"><span data-stu-id="133b2-151">For example, tooback up an app every 12 hours, set frequencyInterval too12 and frequencyUnit toohour.</span></span>

<span data-ttu-id="133b2-152">Starší zálohy se automaticky odeberou z účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="133b2-152">Old backups are automatically removed from hello storage account.</span></span> <span data-ttu-id="133b2-153">Můžete řídit stáří hello zálohování může být podle nastavení hello **retentionPeriodInDays** parametr.</span><span class="sxs-lookup"><span data-stu-id="133b2-153">You can control how old hello backups can be by setting hello **retentionPeriodInDays** parameter.</span></span> <span data-ttu-id="133b2-154">Pokud chcete mít aspoň jednu zálohu uložené, bez ohledu na to, jak staré, nastavení tooalways **keepAtLeastOneBackup** tootrue.</span><span class="sxs-lookup"><span data-stu-id="133b2-154">If you want tooalways have at least one backup saved, regardless of how old it is, set **keepAtLeastOneBackup** tootrue.</span></span>

### <a name="get-hello-automatic-backup-schedule"></a><span data-ttu-id="133b2-155">Získat hello automatické plán zálohování</span><span class="sxs-lookup"><span data-stu-id="133b2-155">Get hello automatic backup schedule</span></span>
<span data-ttu-id="133b2-156">tooget aplikace pro zálohování konfigurace, odeslání **POST** toohello adresa URL požadavku **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/ lokality / {name} / config/zálohování nebo seznamu**.</span><span class="sxs-lookup"><span data-stu-id="133b2-156">tooget an app’s backup configuration, send a **POST** request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup/list**.</span></span>

<span data-ttu-id="133b2-157">Adresa URL Hello pro náš příklad web je **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.</span><span class="sxs-lookup"><span data-stu-id="133b2-157">hello URL for our example site is **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.</span></span>

<a name="get-backup-status"></a>

## <a name="get-hello-status-of-a-backup"></a><span data-ttu-id="133b2-158">Získat stav hello zálohy</span><span class="sxs-lookup"><span data-stu-id="133b2-158">Get hello status of a backup</span></span>
<span data-ttu-id="133b2-159">V závislosti na tom, jak velké aplikace hello je, může chvíli trvat zálohu toocomplete.</span><span class="sxs-lookup"><span data-stu-id="133b2-159">Depending on how large hello app is, a backup may take a while toocomplete.</span></span> <span data-ttu-id="133b2-160">Zálohování může také selže, vypršení časového limitu nebo částečně úspěšné.</span><span class="sxs-lookup"><span data-stu-id="133b2-160">Backups might also fail, time out, or partially succeed.</span></span> <span data-ttu-id="133b2-161">Stav hello toosee zálohování všechny aplikace, odeslání **získat** toohello adresa URL požadavku **/providers/ https://management.azure.com/subscriptions/ {id předplatného} /resourceGroups/ {resource-group name} Microsoft.Web/sites/{name}/backups**.</span><span class="sxs-lookup"><span data-stu-id="133b2-161">toosee hello status of all an app’s backups, send a **GET** request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups**.</span></span>

<span data-ttu-id="133b2-162">Stav hello toosee zálohu konkrétní odeslat požadavek GET toohello URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/ { zálohování id}**.</span><span class="sxs-lookup"><span data-stu-id="133b2-162">toosee hello status of a specific backup, send a GET request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.</span></span>

<span data-ttu-id="133b2-163">Zde je co hello vypadá adresa URL pro náš příklad Web.</span><span class="sxs-lookup"><span data-stu-id="133b2-163">Here is what hello URL looks like for our example website.</span></span> <span data-ttu-id="133b2-164">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**</span><span class="sxs-lookup"><span data-stu-id="133b2-164">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**</span></span>

<span data-ttu-id="133b2-165">text odpovědi Hello obsahuje podobné toothis příkladu objektů JSON.</span><span class="sxs-lookup"><span data-stu-id="133b2-165">hello response body contains a JSON object similar toothis example.</span></span>

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

<span data-ttu-id="133b2-166">Stav Hello zálohy je výčtového typu.</span><span class="sxs-lookup"><span data-stu-id="133b2-166">hello status of a backup is an enumerated type.</span></span> <span data-ttu-id="133b2-167">Zde je každý možné stavu.</span><span class="sxs-lookup"><span data-stu-id="133b2-167">Here is every possible state.</span></span>

* <span data-ttu-id="133b2-168">0 – InProgress: hello zálohování se spustil, ale zatím není dokončený.</span><span class="sxs-lookup"><span data-stu-id="133b2-168">0 – InProgress: hello backup has been started but has not yet completed.</span></span>
* <span data-ttu-id="133b2-169">1 – se nezdařilo: hello zálohování se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="133b2-169">1 – Failed: hello backup was unsuccessful.</span></span>
* <span data-ttu-id="133b2-170">2 – úspěšně: hello zálohování bylo úspěšně dokončeno.</span><span class="sxs-lookup"><span data-stu-id="133b2-170">2 – Succeeded: hello backup completed successfully.</span></span>
* <span data-ttu-id="133b2-171">3 – TimedOut: hello zálohování nebylo dokončeno v čase a byla zrušena.</span><span class="sxs-lookup"><span data-stu-id="133b2-171">3 – TimedOut: hello backup did not finish in time and was canceled.</span></span>
* <span data-ttu-id="133b2-172">4 – vytvoření: hello zálohování požadavek ve frontě, ale nebyla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="133b2-172">4 – Created: hello backup request is queued but has not been started.</span></span>
* <span data-ttu-id="133b2-173">5 – přeskočeno: hello zálohování není pokračovat z důvodu tooa plán spuštění příliš mnoho zálohy.</span><span class="sxs-lookup"><span data-stu-id="133b2-173">5 – Skipped: hello backup did not proceed due tooa schedule triggering too many backups.</span></span>
* <span data-ttu-id="133b2-174">6 – PartiallySucceeded: hello zálohování úspěšně, ale některé soubory nebyly zálohovány, protože nelze načíst.</span><span class="sxs-lookup"><span data-stu-id="133b2-174">6 – PartiallySucceeded: hello backup succeeded, but some files were not backed up because they could not be read.</span></span> <span data-ttu-id="133b2-175">K tomu obvykle dochází, protože výhradní zámek byl umístěn na hello soubory.</span><span class="sxs-lookup"><span data-stu-id="133b2-175">This usually happens because an exclusive lock was placed on hello files.</span></span>
* <span data-ttu-id="133b2-176">7 – DeleteInProgress: zálohování hello byla požadovaná toobe odstranit, ale nebyla ještě odstraněna.</span><span class="sxs-lookup"><span data-stu-id="133b2-176">7 – DeleteInProgress: hello backup has been requested toobe deleted, but has not yet been deleted.</span></span>
* <span data-ttu-id="133b2-177">8 – DeleteFailed: hello zálohování se nepodařilo odstranit.</span><span class="sxs-lookup"><span data-stu-id="133b2-177">8 – DeleteFailed: hello backup could not be deleted.</span></span> <span data-ttu-id="133b2-178">Může k tomu dojít, protože vypršela platnost hello SAS adresa URL, která byla použité toocreate hello zálohování.</span><span class="sxs-lookup"><span data-stu-id="133b2-178">This might happen because hello SAS URL that was used toocreate hello backup has expired.</span></span>
* <span data-ttu-id="133b2-179">9 – odstranit: byl hello zálohování úspěšně odstraněn.</span><span class="sxs-lookup"><span data-stu-id="133b2-179">9 – Deleted: hello backup was deleted successfully.</span></span>

<a name="restore-app"></a>

## <a name="restore-an-app-from-a-backup"></a><span data-ttu-id="133b2-180">Obnovení ze zálohy aplikace</span><span class="sxs-lookup"><span data-stu-id="133b2-180">Restore an app from a backup</span></span>
<span data-ttu-id="133b2-181">Pokud vaše aplikace byl odstraněn, nebo pokud chcete toorevert tooa předchozí verzi aplikace, můžete aplikace hello obnovit ze zálohy.</span><span class="sxs-lookup"><span data-stu-id="133b2-181">If your app has been deleted, or if you want toorevert your app tooa previous version, you can restore hello app from a backup.</span></span> <span data-ttu-id="133b2-182">Odeslat tooinvoke obnovení, **POST** toohello adresa URL požadavku **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/ zálohování / {zálohování id} a obnově**.</span><span class="sxs-lookup"><span data-stu-id="133b2-182">tooinvoke a restore, send a **POST** request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/restore**.</span></span>

<span data-ttu-id="133b2-183">Zde je co hello vypadá adresa URL pro náš příklad Web.</span><span class="sxs-lookup"><span data-stu-id="133b2-183">Here is what hello URL looks like for our example website.</span></span> <span data-ttu-id="133b2-184">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/Restore**</span><span class="sxs-lookup"><span data-stu-id="133b2-184">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/restore**</span></span>

<span data-ttu-id="133b2-185">V textu žádosti hello odesílat objekt JSON, který obsahuje hello vlastnosti pro operaci obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="133b2-185">In hello request body, send a JSON object that contains hello properties for hello restore operation.</span></span> <span data-ttu-id="133b2-186">Tady je příklad, který obsahuje všechny požadované vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="133b2-186">Here is an example containing all required properties:</span></span>

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

### <a name="restore-tooa-new-app"></a><span data-ttu-id="133b2-187">Obnovit tooa nové aplikace</span><span class="sxs-lookup"><span data-stu-id="133b2-187">Restore tooa new app</span></span>
<span data-ttu-id="133b2-188">V některých případech můžete chtít toocreate novou aplikaci při obnovování zálohování, místo přepsal stávající aplikace.</span><span class="sxs-lookup"><span data-stu-id="133b2-188">Sometimes you might want toocreate a new app when you restore a backup, instead of overwriting an already existing app.</span></span> <span data-ttu-id="133b2-189">toodo se změna hello požadavek adresy URL toopoint toohello novou aplikaci chcete toocreate a změnit hello **přepsat** vlastnost hello JSON příliš**false**.</span><span class="sxs-lookup"><span data-stu-id="133b2-189">toodo this, change hello request URL toopoint toohello new app you want toocreate, and change hello **overwrite** property in hello JSON too**false**.</span></span>

<a name="delete-app-backup"></a>

## <a name="delete-an-app-backup"></a><span data-ttu-id="133b2-190">Odstranit zálohu aplikace</span><span class="sxs-lookup"><span data-stu-id="133b2-190">Delete an app backup</span></span>
<span data-ttu-id="133b2-191">Pokud chcete toodelete zálohu, odeslání **odstranit** toohello adresa URL požadavku **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/ lokality / {name} {zálohování id} /backups/**.</span><span class="sxs-lookup"><span data-stu-id="133b2-191">If you would like toodelete a backup, send a **DELETE** request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.</span></span>

<span data-ttu-id="133b2-192">Zde je co hello vypadá adresa URL pro náš příklad Web.</span><span class="sxs-lookup"><span data-stu-id="133b2-192">Here is what hello URL looks like for our example website.</span></span> <span data-ttu-id="133b2-193">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**</span><span class="sxs-lookup"><span data-stu-id="133b2-193">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**</span></span>

<a name="manage-sas-url"></a>

## <a name="manage-a-backups-sas-url"></a><span data-ttu-id="133b2-194">Správa zálohování SAS adresu URL</span><span class="sxs-lookup"><span data-stu-id="133b2-194">Manage a backup’s SAS URL</span></span>
<span data-ttu-id="133b2-195">Azure App Service se pokusí toodelete zálohováním ze služby Azure Storage pomocí hello SAS adresa URL, která byla poskytnuta při vytvoření zálohy hello.</span><span class="sxs-lookup"><span data-stu-id="133b2-195">Azure App Service will attempt toodelete your backup from Azure Storage using hello SAS URL that was provided when hello backup was created.</span></span> <span data-ttu-id="133b2-196">Pokud tato adresa URL SAS již není platná, nelze odstranit hello zálohování prostřednictvím hello REST API.</span><span class="sxs-lookup"><span data-stu-id="133b2-196">If this SAS URL is no longer valid, hello backup cannot be deleted through hello REST API.</span></span> <span data-ttu-id="133b2-197">Však můžete aktualizovat hello SAS URL přidružené zálohy odesláním **POST** toohello adresa URL požadavku **https://management.azure.com/subscriptions/ {id předplatného} /resourceGroups/ {resource-group name} / providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.</span><span class="sxs-lookup"><span data-stu-id="133b2-197">However, you can update hello SAS URL associated with a backup by sending a **POST** request toohello URL **https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.</span></span>

<span data-ttu-id="133b2-198">Zde je co hello vypadá adresa URL pro náš příklad Web.</span><span class="sxs-lookup"><span data-stu-id="133b2-198">Here is what hello URL looks like for our example website.</span></span> <span data-ttu-id="133b2-199">**https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/list**</span><span class="sxs-lookup"><span data-stu-id="133b2-199">**https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/list**</span></span>

<span data-ttu-id="133b2-200">V textu žádosti hello odesílat objekt JSON, který obsahuje hello nové adrese URL SAS.</span><span class="sxs-lookup"><span data-stu-id="133b2-200">In hello request body, send a JSON object that contains hello new SAS URL.</span></span> <span data-ttu-id="133b2-201">Tady je příklad.</span><span class="sxs-lookup"><span data-stu-id="133b2-201">Here is an example.</span></span>

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

> [!NOTE]
> <span data-ttu-id="133b2-202">Z bezpečnostních důvodů hello SAS URL přidruženého zálohu nevrátí při odesílání požadavek GET pro konkrétní zálohování.</span><span class="sxs-lookup"><span data-stu-id="133b2-202">For security reasons, hello SAS URL associated with a backup is not returned when sending a GET request for a specific backup.</span></span> <span data-ttu-id="133b2-203">Pokud chcete tooview hello SAS URL přidružené zálohy, odeslání toohello požadavek POST stejnou adresu URL výše.</span><span class="sxs-lookup"><span data-stu-id="133b2-203">If you want tooview hello SAS URL associated with a backup, send a POST request toohello same URL above.</span></span> <span data-ttu-id="133b2-204">Zahrňte prázdný objekt JSON v textu žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="133b2-204">Include an empty JSON object in hello request body.</span></span> <span data-ttu-id="133b2-205">Hello odpovědi ze serveru hello obsahuje všechny informace o tuto zálohu, včetně jeho SAS adresa URL.</span><span class="sxs-lookup"><span data-stu-id="133b2-205">hello response from hello server contains all of that backup’s information, including its SAS URL.</span></span>
> 
> 

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
