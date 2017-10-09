---
title: "pomocí obnovení po havárii aaaImplement zálohování a obnovení v Azure API Management | Microsoft Docs"
description: "Zjistěte, jak toouse zálohování a obnovení tooperform zotavení po havárii v Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 6f10be3c-f796-4a6c-bacd-7931b6aa82af
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 058bfb579e3a3f51fb1dac8ea37eb4fdbc83a4ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimplement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a><span data-ttu-id="0299f-103">Jak tooimplement po havárii obnovení pomocí služby zálohování a obnovení v Azure API Management</span><span class="sxs-lookup"><span data-stu-id="0299f-103">How tooimplement disaster recovery using service backup and restore in Azure API Management</span></span>
<span data-ttu-id="0299f-104">Pomocí výběr toopublish a spravovat vaše rozhraní API pomocí Azure API Management jsou využívat výhod mnoha odolnost proti chybám a infrastruktury možnosti, které byste jinak museli toodesign, implementovat a spravovat.</span><span class="sxs-lookup"><span data-stu-id="0299f-104">By choosing toopublish and manage your APIs via Azure API Management you are taking advantage of many fault tolerance and infrastructure capabilities that you would otherwise have toodesign, implement, and manage.</span></span> <span data-ttu-id="0299f-105">Hello platformy Azure snižuje velké zlomek případy možného selhání za zlomek hello náklady.</span><span class="sxs-lookup"><span data-stu-id="0299f-105">hello Azure platform mitigates a large fraction of potential failures at a fraction of hello cost.</span></span>

<span data-ttu-id="0299f-106">toorecover z dostupnosti problémů, které mají vliv na hello oblast, kde služby API Management je hostovaná vám musí být připravené tooreconstitute služby v jiné oblasti kdykoli.</span><span class="sxs-lookup"><span data-stu-id="0299f-106">toorecover from availability problems affecting hello region where your API Management service is hosted you should be ready tooreconstitute your service in a different region at any time.</span></span> <span data-ttu-id="0299f-107">V závislosti na vašich cílů dostupnosti a cíli času obnovení můžete má tooreserve zálohování služby v jedné nebo více oblastech a zkuste toomaintain jejich konfigurace a obsahu, které jsou synchronizované se službou active hello.</span><span class="sxs-lookup"><span data-stu-id="0299f-107">Depending on your availability goals and recovery time objective  you might want tooreserve a backup service in one or more regions and try toomaintain their configuration and content in sync with hello active service.</span></span> <span data-ttu-id="0299f-108">Hello služby zálohování a obnovení funkce poskytuje hello nezbytné stavební blok pro implementaci strategie zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="0299f-108">hello service backup and restore feature provides hello necessary building block for implementing your disaster recovery strategy.</span></span>

<span data-ttu-id="0299f-109">Tato příručka ukazuje, jak požaduje tooauthenticate Azure Resource Manager a jak toobackup a obnovit vaše instance služby API Management.</span><span class="sxs-lookup"><span data-stu-id="0299f-109">This guide shows how tooauthenticate Azure Resource Manager requests, and how toobackup and restore your API Management service instances.</span></span>

> [!NOTE]
> <span data-ttu-id="0299f-110">pro replikaci instance služby API Management pro scénáře, jako je pracovní se lze také Hello proces zálohování a obnovení instance služby API Management pro zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="0299f-110">hello process for backing up and restoring an API Management service instance for disaster recovery can also be used for replicating API Management service instances for scenarios such as staging.</span></span>
>
> <span data-ttu-id="0299f-111">Všimněte si, že každá záloha vyprší po 30 dnech.</span><span class="sxs-lookup"><span data-stu-id="0299f-111">Note that each backup expires after 30 days.</span></span> <span data-ttu-id="0299f-112">Když zkusíte toorestore zálohu po dobu platnosti 30denní hello vypršela platnost, se nezdaří obnovení hello s `Cannot restore: backup expired` zprávy.</span><span class="sxs-lookup"><span data-stu-id="0299f-112">If you attempt toorestore a backup after hello 30 day expiration period has expired, hello restore will fail with a `Cannot restore: backup expired` message.</span></span>
>
>

## <a name="authenticating-azure-resource-manager-requests"></a><span data-ttu-id="0299f-113">Požadavky na ověřování Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0299f-113">Authenticating Azure Resource Manager requests</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0299f-114">Hello REST API pro zálohování a obnovení používá Azure Resource Manager a má jiný mechanismus ověřování než hello rozhraní REST API pro správu vaší entity API Management.</span><span class="sxs-lookup"><span data-stu-id="0299f-114">hello REST API for backup and restore uses Azure Resource Manager and has a different authentication mechanism than hello REST APIs for managing your API Management entities.</span></span> <span data-ttu-id="0299f-115">Hello kroky v této části popisují, jak požaduje tooauthenticate Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0299f-115">hello steps in this section describe how tooauthenticate Azure Resource Manager requests.</span></span> <span data-ttu-id="0299f-116">Další informace najdete v tématu [požadavků ověřování Azure Resource Manager](http://msdn.microsoft.com/library/azure/dn790557.aspx).</span><span class="sxs-lookup"><span data-stu-id="0299f-116">For more information, see [Authenticating Azure Resource Manager requests](http://msdn.microsoft.com/library/azure/dn790557.aspx).</span></span>
>
>

<span data-ttu-id="0299f-117">Všechny hello úlohy, které můžete provést na prostředků pomocí Azure Resource Manager hello musí být ověřeny v Azure Active Directory pomocí hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="0299f-117">All of hello tasks that you do on resources using hello Azure Resource Manager must be authenticated with Azure Active Directory using hello following steps.</span></span>

* <span data-ttu-id="0299f-118">Přidáte klienta Azure Active Directory toohello aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0299f-118">Add an application toohello Azure Active Directory tenant.</span></span>
* <span data-ttu-id="0299f-119">Nastavte oprávnění pro hello aplikace, které jste přidali.</span><span class="sxs-lookup"><span data-stu-id="0299f-119">Set permissions for hello application that you added.</span></span>
* <span data-ttu-id="0299f-120">Získáte hello token pro ověřování požadavků tooAzure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0299f-120">Get hello token for authenticating requests tooAzure Resource Manager.</span></span>

<span data-ttu-id="0299f-121">prvním krokem Hello je toocreate aplikaci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0299f-121">hello first step is toocreate an Azure Active Directory application.</span></span> <span data-ttu-id="0299f-122">Přihlaste se k hello [portálu Azure Classic](http://manage.windowsazure.com/) pomocí hello předplatného, který obsahuje služby API Management instance a přejděte toohello **aplikace** kartu pro výchozí Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0299f-122">Log into hello [Azure Classic Portal](http://manage.windowsazure.com/) using hello subscription that contains your API Management service instance and navigate toohello **Applications** tab for your default Azure Active Directory.</span></span>

> [!NOTE]
> <span data-ttu-id="0299f-123">Pokud hello Azure Active Directory výchozí adresář není viditelné tooyour účtu, správce kontaktní hello hello předplatného Azure toogrant hello vyžaduje oprávnění tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="0299f-123">If hello Azure Active Directory default directory is not visible tooyour account, contact hello administrator of hello Azure subscription toogrant hello required permissions tooyour account.</span></span>

![Vytvoření aplikace Azure Active Directory][api-management-add-aad-application]

<span data-ttu-id="0299f-125">Klikněte na tlačítko **přidat**, **přidat aplikaci, kterou vyvíjí Moje organizace**a zvolte **nativní klientskou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="0299f-125">Click **Add**, **Add an application my organization is developing**, and choose **Native client application**.</span></span> <span data-ttu-id="0299f-126">Zadejte popisný název a klikněte na šipku další hello.</span><span class="sxs-lookup"><span data-stu-id="0299f-126">Enter a descriptive name, and click hello next arrow.</span></span> <span data-ttu-id="0299f-127">Zadejte adresu URL zástupného symbolu, jako `http://resources` pro hello **identifikátor URI pro přesměrování**, jako je povinné pole, ale hodnota hello nepoužívá později.</span><span class="sxs-lookup"><span data-stu-id="0299f-127">Enter a placeholder URL such as `http://resources` for hello **Redirect URI**, as it is a required field, but hello value is not used later.</span></span> <span data-ttu-id="0299f-128">Klikněte na tlačítko hello políčko toosave hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="0299f-128">Click hello check box toosave hello application.</span></span>

<span data-ttu-id="0299f-129">Jakmile aplikace hello je uložena, klikněte na možnost **konfigurace**, projděte dolů toohello **oprávnění tooother aplikace** a klikněte na **přidat aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="0299f-129">Once hello application is saved, click **Configure**, scroll down toohello **permissions tooother applications** section, and click **Add application**.</span></span>

![Přidání oprávnění][api-management-aad-permissions-add]

<span data-ttu-id="0299f-131">Vyberte **Windows** **Azure Service Management API** a klikněte na tlačítko hello políčko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="0299f-131">Select **Windows** **Azure Service Management API** and click hello checkbox tooadd hello application.</span></span>

![Přidání oprávnění][api-management-aad-permissions]

<span data-ttu-id="0299f-133">Klikněte na tlačítko **delegovaná oprávnění** vedle hello nově přidaná **Windows** **Azure Service Management API** aplikace, políčko hello **přístup k Azure Správa služeb (preview)**a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0299f-133">Click **Delegated Permissions** beside hello newly added **Windows** **Azure Service Management API** application, check hello box for **Access Azure Service Management (preview)**, and click **Save**.</span></span>

![Přidání oprávnění][api-management-aad-delegated-permissions]

<span data-ttu-id="0299f-135">Předchozí tooinvoking hello rozhraní API, která generovat hello zálohování a obnovte ji, je nutné tooget token.</span><span class="sxs-lookup"><span data-stu-id="0299f-135">Prior tooinvoking hello APIs that generate hello backup and restore it, it is necessary tooget a token.</span></span> <span data-ttu-id="0299f-136">Hello následující příklad používá hello [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) token hello tooretrieve balíček nuget.</span><span class="sxs-lookup"><span data-stu-id="0299f-136">hello following example uses hello [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) nuget package tooretrieve hello token.</span></span>

```c#
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace GetTokenResourceManagerRequests
{
    class Program
    {
        static void Main(string[] args)
        {
            var authenticationContext = new AuthenticationContext("https://login.microsoftonline.com/{tenant id}");
            var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

            if (result == null) {
                throw new InvalidOperationException("Failed tooobtain hello JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

<span data-ttu-id="0299f-137">Nahraďte `{tentand id}`, `{application id}`, a `{redirect uri}` pomocí pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="0299f-137">Replace `{tentand id}`, `{application id}`, and `{redirect uri}` using hello following instructions.</span></span>

<span data-ttu-id="0299f-138">Nahraďte `{tenant id}` s id klienta hello hello aplikaci Azure Active Directory, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="0299f-138">Replace `{tenant id}` with hello tenant id of hello Azure Active Directory application you just created.</span></span> <span data-ttu-id="0299f-139">Přistupujete hello id kliknutím **zobrazit koncové body**.</span><span class="sxs-lookup"><span data-stu-id="0299f-139">You can access hello id by clicking **View endpoints**.</span></span>

![Koncové body][api-management-aad-default-directory]

![Koncové body][api-management-endpoint]

<span data-ttu-id="0299f-142">Nahraďte `{application id}` a `{redirect uri}` pomocí hello **Id klienta** a hello adresu URL z hello **identifikátory URI přesměrování** části z vaší aplikace Azure Active Directory **konfigurace**  kartě.</span><span class="sxs-lookup"><span data-stu-id="0299f-142">Replace `{application id}` and `{redirect uri}` using hello **Client Id** and  hello URL from hello **Redirect Uris** section from your Azure Active Directory application's **Configure** tab.</span></span>

![Zdroje][api-management-aad-resources]

<span data-ttu-id="0299f-144">Jakmile jsou zadané hodnoty hello, by měl vrátit příklad kódu hello tokenu podobné toohello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="0299f-144">Once hello values are specified, hello code example should return a token similar toohello following example.</span></span>

![Token][api-management-arm-token]

<span data-ttu-id="0299f-146">Před voláním hello zálohování a obnovení popsaný v následující části hello, nastavte hello autorizační hlavičky požadavku pro volání REST.</span><span class="sxs-lookup"><span data-stu-id="0299f-146">Before calling hello backup and restore operations described in hello following sections, set hello authorization request header for your REST call.</span></span>

```c#
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

## <span data-ttu-id="0299f-147"><a name="step1"></a>Zálohování služby API Management</span><span class="sxs-lookup"><span data-stu-id="0299f-147"><a name="step1"> </a>Back up an API Management service</span></span>
<span data-ttu-id="0299f-148">tooback až hello problém služby API Management následující požadavek HTTP:</span><span class="sxs-lookup"><span data-stu-id="0299f-148">tooback up an API Management service issue hello following HTTP request:</span></span>

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

<span data-ttu-id="0299f-149">Kde:</span><span class="sxs-lookup"><span data-stu-id="0299f-149">where:</span></span>

* <span data-ttu-id="0299f-150">`subscriptionId`– id předplatného hello obsahující hello služba API Management se pokoušíte toobackup</span><span class="sxs-lookup"><span data-stu-id="0299f-150">`subscriptionId` - id of hello subscription containing hello API Management service you are attempting toobackup</span></span>
* <span data-ttu-id="0299f-151">`resourceGroupName`-řetězec v podobě hello 'API - výchozí – {služba region}, kde `service-region` identifikuje hello oblast Azure, kde je umístěn hello služba API Management se pokoušíte toobackup, například`North-Central-US`</span><span class="sxs-lookup"><span data-stu-id="0299f-151">`resourceGroupName` - a string in hello form of 'Api-Default-{service-region}' where `service-region` identifies hello Azure region where hello API Management service you are trying toobackup is hosted, e.g. `North-Central-US`</span></span>
* <span data-ttu-id="0299f-152">`serviceName`-Název hello hello služba API Management provádíte zálohování zadaný během hello jeho vytvoření</span><span class="sxs-lookup"><span data-stu-id="0299f-152">`serviceName` - hello name of hello API Management service you are making a backup of specified at hello time of its creation</span></span>
* <span data-ttu-id="0299f-153">`api-version`-nahradit`2014-02-14`</span><span class="sxs-lookup"><span data-stu-id="0299f-153">`api-version` - replace  with `2014-02-14`</span></span>

<span data-ttu-id="0299f-154">V textu hello hello požadavku zadejte název účtu úložiště Azure hello cíl, přístupový klíč, název kontejneru objektu blob a název zálohy:</span><span class="sxs-lookup"><span data-stu-id="0299f-154">In hello body of hello request, specify hello target Azure storage account name, access key, blob container name, and backup name:</span></span>

```
'{  
    storageAccount : {storage account name for hello backup},  
    accessKey : {access key for hello account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

<span data-ttu-id="0299f-155">Nastavte hodnotu hello hello `Content-Type` hlavička požadavku příliš`application/json`.</span><span class="sxs-lookup"><span data-stu-id="0299f-155">Set hello value of hello `Content-Type` request header too`application/json`.</span></span>

<span data-ttu-id="0299f-156">Zálohování je dlouhotrvající operace, která může trvat několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="0299f-156">Backup is a long running operation that may take multiple minutes toocomplete.</span></span>  <span data-ttu-id="0299f-157">Pokud hello požadavek byl úspěšný a byl zahájen proces zálohování hello dostanete `202 Accepted` stavový kód odpovědi s `Location` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="0299f-157">If hello request was successful and hello backup process was initiated you’ll receive a `202 Accepted` response status code with a `Location` header.</span></span>  <span data-ttu-id="0299f-158">Zkontrolujte požadavky "GET" toohello adresu URL v hello `Location` záhlaví toofind out hello stav operace hello.</span><span class="sxs-lookup"><span data-stu-id="0299f-158">Make 'GET' requests toohello URL in hello `Location` header toofind out hello status of hello operation.</span></span> <span data-ttu-id="0299f-159">Když probíhá hello zálohování bude pokračovat tooreceive 202 přijata stavový kód.</span><span class="sxs-lookup"><span data-stu-id="0299f-159">While hello backup is in progress you will continue tooreceive a '202 Accepted' status code.</span></span> <span data-ttu-id="0299f-160">Kód odpovědi `200 OK` označí úspěšném dokončení operace zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="0299f-160">A Response code of `200 OK` will indicate successful completion of hello backup operation.</span></span>

<span data-ttu-id="0299f-161">Všimněte si hello následující omezení při vytváření zálohy požadavku.</span><span class="sxs-lookup"><span data-stu-id="0299f-161">Please note hello following constraints when making a backup request.</span></span>

* <span data-ttu-id="0299f-162">**Kontejner** zadaná v textu žádosti hello **musí existovat**.</span><span class="sxs-lookup"><span data-stu-id="0299f-162">**Container** specified in hello request body **must exist**.</span></span>
* <span data-ttu-id="0299f-163">Při zálohování se v průběhu **neměli žádné operace správy, služba** například SKU upgrade nebo přechod na starší verzi, změna názvu domény, atd.</span><span class="sxs-lookup"><span data-stu-id="0299f-163">While backup is in progress you **should not attempt any service management operations** such as SKU upgrade or downgrade, domain name change, etc.</span></span>
* <span data-ttu-id="0299f-164">Obnovení systému **zálohování záruku, že se pouze po dobu 30 dnů** od okamžiku hello jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="0299f-164">Restore of a **backup is guaranteed only for 30 days** since hello moment of its creation.</span></span>
* <span data-ttu-id="0299f-165">**Data o využití** použitý k vytvoření sestavy analýzy **nezahrnuje** hello zálohování.</span><span class="sxs-lookup"><span data-stu-id="0299f-165">**Usage data** used for creating analytics reports **is not included** in hello backup.</span></span> <span data-ttu-id="0299f-166">Použití [REST API služby Azure API Management] [ Azure API Management REST API] tooperiodically načíst analytics sestavy z bezpečnostních důvodů udržován.</span><span class="sxs-lookup"><span data-stu-id="0299f-166">Use [Azure API Management REST API][Azure API Management REST API] tooperiodically retrieve analytics reports for safekeeping.</span></span>
* <span data-ttu-id="0299f-167">Hello četnost, se kterým můžete provést zálohování služby ovlivní vaše plánovaného bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="0299f-167">hello frequency with which you perform service backups will affect your recovery point objective.</span></span> <span data-ttu-id="0299f-168">toominimize ji doporučujeme, provádění pravidelných záloh a také provádění zálohování na vyžádání po provedení důležité změny tooyour služba API Management.</span><span class="sxs-lookup"><span data-stu-id="0299f-168">toominimize it we advise implementing regular backups as well as performing on-demand backups after making important changes tooyour API Management service.</span></span>
* <span data-ttu-id="0299f-169">**Změny** učiněna toohello konfigurace služby (např. rozhraní API, zásady, vzhled portálu vývojáře) během operace zálohování je v procesu **nemusí být součástí hello zálohování a proto budou ztraceny**.</span><span class="sxs-lookup"><span data-stu-id="0299f-169">**Changes** made toohello service configuration (e.g. APIs, policies, developer portal appearance) while backup operation is in process **might not be included in hello backup and therefore will be lost**.</span></span>

## <span data-ttu-id="0299f-170"><a name="step2"></a>Obnovení služby API Management</span><span class="sxs-lookup"><span data-stu-id="0299f-170"><a name="step2"> </a>Restore an API Management service</span></span>
<span data-ttu-id="0299f-171">toorestore služby API Management z dříve vytvořeného zkontrolujte hello následující požadavek HTTP:</span><span class="sxs-lookup"><span data-stu-id="0299f-171">toorestore an API Management service from a previously created backup make hello following HTTP request:</span></span>

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

<span data-ttu-id="0299f-172">Kde:</span><span class="sxs-lookup"><span data-stu-id="0299f-172">where:</span></span>

* <span data-ttu-id="0299f-173">`subscriptionId`– id předplatného hello obsahující hello služba API Management jsou obnovení ze zálohy do</span><span class="sxs-lookup"><span data-stu-id="0299f-173">`subscriptionId` - id of hello subscription containing hello API Management service you are restoring a backup into</span></span>
* <span data-ttu-id="0299f-174">`resourceGroupName`-řetězec v podobě hello 'API - výchozí – {služba region}, kde `service-region` identifikuje hello oblast Azure, kde je umístěn hello obnovujete zálohu do služby API Management, například`North-Central-US`</span><span class="sxs-lookup"><span data-stu-id="0299f-174">`resourceGroupName` - a string in hello form of 'Api-Default-{service-region}' where `service-region` identifies hello Azure region where hello API Management service you are restoring a backup into is hosted, e.g. `North-Central-US`</span></span>
* <span data-ttu-id="0299f-175">`serviceName`-Název hello hello API Management zadat služby obnovena do okamžiku hello jeho vytvoření</span><span class="sxs-lookup"><span data-stu-id="0299f-175">`serviceName` - hello name of hello API Management service being restored into specified at hello time of its creation</span></span>
* <span data-ttu-id="0299f-176">`api-version`-nahradit`2014-02-14`</span><span class="sxs-lookup"><span data-stu-id="0299f-176">`api-version` - replace  with `2014-02-14`</span></span>

<span data-ttu-id="0299f-177">V textu hello hello požadavku zadejte umístění záložního souboru hello, tj. název účtu úložiště Azure, přístupový klíč, název kontejneru objektu blob a název zálohy:</span><span class="sxs-lookup"><span data-stu-id="0299f-177">In hello body of hello request, specify hello backup file location, i.e. Azure storage account name, access key, blob container name, and backup name:</span></span>

```
'{  
    storageAccount : {storage account name for hello backup},  
    accessKey : {access key for hello account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

<span data-ttu-id="0299f-178">Nastavte hodnotu hello hello `Content-Type` hlavička požadavku příliš`application/json`.</span><span class="sxs-lookup"><span data-stu-id="0299f-178">Set hello value of hello `Content-Type` request header too`application/json`.</span></span>

<span data-ttu-id="0299f-179">Obnovení je dlouhotrvající operace, která může trvat až too30 nebo další toocomplete minut.</span><span class="sxs-lookup"><span data-stu-id="0299f-179">Restore is a long running operation that may take up too30 or more minutes toocomplete.</span></span>  <span data-ttu-id="0299f-180">Pokud hello požadavek byl úspěšný a byl zahájen proces obnovení hello dostanete `202 Accepted` stavový kód odpovědi s `Location` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="0299f-180">If hello request was successful and hello restore process was initiated you’ll receive a `202 Accepted` response status code with a `Location` header.</span></span>  <span data-ttu-id="0299f-181">Zkontrolujte požadavky "GET" toohello adresu URL v hello `Location` záhlaví toofind out hello stav operace hello.</span><span class="sxs-lookup"><span data-stu-id="0299f-181">Make 'GET' requests toohello URL in hello `Location` header toofind out hello status of hello operation.</span></span> <span data-ttu-id="0299f-182">Když probíhá hello obnovení bude pokračovat tooreceive '202 platných' stavový kód.</span><span class="sxs-lookup"><span data-stu-id="0299f-182">While hello restore is in progress you will continue tooreceive '202 Accepted' status code.</span></span> <span data-ttu-id="0299f-183">Kód odpovědi `200 OK` označí úspěšném dokončení operace obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="0299f-183">A response code of `200 OK` will indicate successful completion of hello restore operation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0299f-184">**Hello SKU** služby hello obnovena do **musí odpovídat** hello SKU hello zálohovat služba obnovena.</span><span class="sxs-lookup"><span data-stu-id="0299f-184">**hello SKU** of hello service being restored into **must match** hello SKU of hello backed up service being restored.</span></span>
>
> <span data-ttu-id="0299f-185">**Změny** učiněna toohello konfigurace služby (např. rozhraní API, zásady, vzhled portálu vývojáře) během operace obnovení je v průběhu **může dojít k přepsání**.</span><span class="sxs-lookup"><span data-stu-id="0299f-185">**Changes** made toohello service configuration (e.g. APIs, policies, developer portal appearance) while restore operation is in progress **could be overwritten**.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="0299f-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0299f-186">Next steps</span></span>
<span data-ttu-id="0299f-187">Podívejte se na následující blogy od Microsoftu pro dva různé postupy procesu zálohování a obnovení hello hello.</span><span class="sxs-lookup"><span data-stu-id="0299f-187">Check out hello following Microsoft blogs for two different walkthroughs of hello backup/restore process.</span></span>

* [<span data-ttu-id="0299f-188">Replikovat účty Azure API Management</span><span class="sxs-lookup"><span data-stu-id="0299f-188">Replicate Azure API Management Accounts</span></span>](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
  * <span data-ttu-id="0299f-189">Děkujeme, že tooGisela pro svůj článek toothis příspěvku.</span><span class="sxs-lookup"><span data-stu-id="0299f-189">Thank you tooGisela for her contribution toothis article.</span></span>
* [<span data-ttu-id="0299f-190">Azure API Management: Zálohování a obnovení konfigurace</span><span class="sxs-lookup"><span data-stu-id="0299f-190">Azure API Management: Backing Up and Restoring Configuration</span></span>](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
  * <span data-ttu-id="0299f-191">přístup Hello podrobné podle Stanislav neodpovídá hello oficiální pokyny, ale je velmi zajímavé.</span><span class="sxs-lookup"><span data-stu-id="0299f-191">hello approach detailed by Stuart does not match hello official guidance but it is very interesting.</span></span>

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API Management REST API]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
