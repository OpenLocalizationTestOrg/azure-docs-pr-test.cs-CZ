---
title: "Implementace po havárii obnovení pomocí zálohování a obnovení v Azure API Management | Microsoft Docs"
description: "Naučte se používat zálohování a obnovení provést zotavení po havárii v Azure API Management."
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
ms.openlocfilehash: 07c0265490cfae733133b6e0c938f90f9b392da4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a><span data-ttu-id="a2ded-103">Implementace zotavení po havárii pomocí služby zálohování a obnovení v Azure API Management</span><span class="sxs-lookup"><span data-stu-id="a2ded-103">How to implement disaster recovery using service backup and restore in Azure API Management</span></span>
<span data-ttu-id="a2ded-104">Výběrem publikovat a spravovat vaše rozhraní API pomocí Azure API Management jsou využívat výhod mnoha odolnost proti chybám a možnosti infrastruktury, které byste jinak museli k návrhu, implementaci a správě.</span><span class="sxs-lookup"><span data-stu-id="a2ded-104">By choosing to publish and manage your APIs via Azure API Management you are taking advantage of many fault tolerance and infrastructure capabilities that you would otherwise have to design, implement, and manage.</span></span> <span data-ttu-id="a2ded-105">Platformy Azure snižuje velké zlomek případy možného selhání za zlomek ceny.</span><span class="sxs-lookup"><span data-stu-id="a2ded-105">The Azure platform mitigates a large fraction of potential failures at a fraction of the cost.</span></span>

<span data-ttu-id="a2ded-106">Obnovení z dostupnosti otázkách oblast, kde vaše rozhraní API správy služby je hostovat jste měli být připravení rekonstruovat služby v jiné oblasti kdykoli.</span><span class="sxs-lookup"><span data-stu-id="a2ded-106">To recover from availability problems affecting the region where your API Management service is hosted you should be ready to reconstitute your service in a different region at any time.</span></span> <span data-ttu-id="a2ded-107">V závislosti na vašich cílů dostupnosti a cíli času obnovení můžete vyhradit zálohování služby v jedné nebo více oblastech a pokuste se udržovat synchronizované se službou active jejich konfigurace a obsahu.</span><span class="sxs-lookup"><span data-stu-id="a2ded-107">Depending on your availability goals and recovery time objective  you might want to reserve a backup service in one or more regions and try to maintain their configuration and content in sync with the active service.</span></span> <span data-ttu-id="a2ded-108">Služba zálohování a obnovení funkce poskytuje nezbytné stavební blok pro implementaci strategie zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="a2ded-108">The service backup and restore feature provides the necessary building block for implementing your disaster recovery strategy.</span></span>

<span data-ttu-id="a2ded-109">Tato příručka ukazuje, jak k ověřování žádostí o Azure Resource Manager a postup zálohování a obnovení vaší instance služby API Management.</span><span class="sxs-lookup"><span data-stu-id="a2ded-109">This guide shows how to authenticate Azure Resource Manager requests, and how to backup and restore your API Management service instances.</span></span>

> [!NOTE]
> <span data-ttu-id="a2ded-110">Proces zálohování a obnovení instance služby API Management pro zotavení po havárii můžete použít také pro replikaci instance služby API Management pro scénáře, jako je pracovní.</span><span class="sxs-lookup"><span data-stu-id="a2ded-110">The process for backing up and restoring an API Management service instance for disaster recovery can also be used for replicating API Management service instances for scenarios such as staging.</span></span>
>
> <span data-ttu-id="a2ded-111">Všimněte si, že každá záloha vyprší po 30 dnech.</span><span class="sxs-lookup"><span data-stu-id="a2ded-111">Note that each backup expires after 30 days.</span></span> <span data-ttu-id="a2ded-112">Pokud se pokusíte obnovit zálohu po vypršení platnosti 30denní, obnovení se nezdaří s `Cannot restore: backup expired` zprávy.</span><span class="sxs-lookup"><span data-stu-id="a2ded-112">If you attempt to restore a backup after the 30 day expiration period has expired, the restore will fail with a `Cannot restore: backup expired` message.</span></span>
>
>

## <a name="authenticating-azure-resource-manager-requests"></a><span data-ttu-id="a2ded-113">Požadavky na ověřování Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a2ded-113">Authenticating Azure Resource Manager requests</span></span>
> [!IMPORTANT]
> <span data-ttu-id="a2ded-114">Rozhraní REST API pro zálohování a obnovení používá Azure Resource Manager a má jiný mechanismus ověřování než rozhraní REST API pro správu vaší entity API Management.</span><span class="sxs-lookup"><span data-stu-id="a2ded-114">The REST API for backup and restore uses Azure Resource Manager and has a different authentication mechanism than the REST APIs for managing your API Management entities.</span></span> <span data-ttu-id="a2ded-115">Postup v této části popisují, jak k ověřování žádostí o Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a2ded-115">The steps in this section describe how to authenticate Azure Resource Manager requests.</span></span> <span data-ttu-id="a2ded-116">Další informace najdete v tématu [požadavků ověřování Azure Resource Manager](http://msdn.microsoft.com/library/azure/dn790557.aspx).</span><span class="sxs-lookup"><span data-stu-id="a2ded-116">For more information, see [Authenticating Azure Resource Manager requests](http://msdn.microsoft.com/library/azure/dn790557.aspx).</span></span>
>
>

<span data-ttu-id="a2ded-117">Všechny úlohy, které můžete provést na prostředků pomocí Azure Resource Manager musí být ověřeny v Azure Active Directory pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="a2ded-117">All of the tasks that you do on resources using the Azure Resource Manager must be authenticated with Azure Active Directory using the following steps.</span></span>

* <span data-ttu-id="a2ded-118">Přidání aplikace do klienta Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a2ded-118">Add an application to the Azure Active Directory tenant.</span></span>
* <span data-ttu-id="a2ded-119">Nastavte oprávnění pro aplikace, která jste přidali.</span><span class="sxs-lookup"><span data-stu-id="a2ded-119">Set permissions for the application that you added.</span></span>
* <span data-ttu-id="a2ded-120">Získáte token pro ověřování požadavků na Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a2ded-120">Get the token for authenticating requests to Azure Resource Manager.</span></span>

<span data-ttu-id="a2ded-121">Prvním krokem je vytvoření aplikace Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a2ded-121">The first step is to create an Azure Active Directory application.</span></span> <span data-ttu-id="a2ded-122">Přihlaste se [portálu Azure Classic](http://manage.windowsazure.com/) pomocí předplatného, který obsahuje služby API Management instance a přejděte do **aplikace** kartu pro výchozí Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a2ded-122">Log into the [Azure Classic Portal](http://manage.windowsazure.com/) using the subscription that contains your API Management service instance and navigate to the **Applications** tab for your default Azure Active Directory.</span></span>

> [!NOTE]
> <span data-ttu-id="a2ded-123">Pokud výchozí adresář služby Azure Active Directory není váš účet vidí, obraťte se na správce předplatného Azure udělit požadovaná oprávnění ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="a2ded-123">If the Azure Active Directory default directory is not visible to your account, contact the administrator of the Azure subscription to grant the required permissions to your account.</span></span>

![Vytvoření aplikace Azure Active Directory][api-management-add-aad-application]

<span data-ttu-id="a2ded-125">Klikněte na tlačítko **přidat**, **přidat aplikaci, kterou vyvíjí Moje organizace**a zvolte **nativní klientskou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="a2ded-125">Click **Add**, **Add an application my organization is developing**, and choose **Native client application**.</span></span> <span data-ttu-id="a2ded-126">Zadejte popisný název a klikněte na šipku Další.</span><span class="sxs-lookup"><span data-stu-id="a2ded-126">Enter a descriptive name, and click the next arrow.</span></span> <span data-ttu-id="a2ded-127">Zadejte adresu URL zástupného symbolu, jako `http://resources` pro **identifikátor URI pro přesměrování**, jako je povinné pole, ale hodnota nepoužívá později.</span><span class="sxs-lookup"><span data-stu-id="a2ded-127">Enter a placeholder URL such as `http://resources` for the **Redirect URI**, as it is a required field, but the value is not used later.</span></span> <span data-ttu-id="a2ded-128">Klikněte na zaškrtávací políčko pro uložení aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2ded-128">Click the check box to save the application.</span></span>

<span data-ttu-id="a2ded-129">Po uložení aplikace, klikněte na tlačítko **konfigurace**, přejděte dolů k položce **oprávnění k ostatním aplikacím** a klikněte na **přidat aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="a2ded-129">Once the application is saved, click **Configure**, scroll down to the **permissions to other applications** section, and click **Add application**.</span></span>

![Přidání oprávnění][api-management-aad-permissions-add]

<span data-ttu-id="a2ded-131">Vyberte **Windows** **Azure Service Management API** a klikněte na zaškrtávací políčko pro tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="a2ded-131">Select **Windows** **Azure Service Management API** and click the checkbox to add the application.</span></span>

![Přidání oprávnění][api-management-aad-permissions]

<span data-ttu-id="a2ded-133">Klikněte na tlačítko **delegovaná oprávnění** vedle nově přidaný **Windows** **Azure Service Management API** aplikace, zaškrtněte políčko pro **přístupu Azure Service managementu (preview)**a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a2ded-133">Click **Delegated Permissions** beside the newly added **Windows** **Azure Service Management API** application, check the box for **Access Azure Service Management (preview)**, and click **Save**.</span></span>

![Přidání oprávnění][api-management-aad-delegated-permissions]

<span data-ttu-id="a2ded-135">Před vyvoláním rozhraní API, která generovat zálohu a obnovte ji, je nutné získat token.</span><span class="sxs-lookup"><span data-stu-id="a2ded-135">Prior to invoking the APIs that generate the backup and restore it, it is necessary to get a token.</span></span> <span data-ttu-id="a2ded-136">Následující příklad používá [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) balíček nuget pro načtení tokenu.</span><span class="sxs-lookup"><span data-stu-id="a2ded-136">The following example uses the [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) nuget package to retrieve the token.</span></span>

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
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

<span data-ttu-id="a2ded-137">Nahraďte `{tentand id}`, `{application id}`, a `{redirect uri}` podle následujících pokynů.</span><span class="sxs-lookup"><span data-stu-id="a2ded-137">Replace `{tentand id}`, `{application id}`, and `{redirect uri}` using the following instructions.</span></span>

<span data-ttu-id="a2ded-138">Nahraďte `{tenant id}` s id klienta aplikace Azure Active Directory, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a2ded-138">Replace `{tenant id}` with the tenant id of the Azure Active Directory application you just created.</span></span> <span data-ttu-id="a2ded-139">Kliknutím můžete přistupovat k id **zobrazit koncové body**.</span><span class="sxs-lookup"><span data-stu-id="a2ded-139">You can access the id by clicking **View endpoints**.</span></span>

![Koncové body][api-management-aad-default-directory]

![Koncové body][api-management-endpoint]

<span data-ttu-id="a2ded-142">Nahraďte `{application id}` a `{redirect uri}` pomocí **Id klienta** a adresu URL z **identifikátory URI přesměrování** části z vaší aplikace Azure Active Directory **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="a2ded-142">Replace `{application id}` and `{redirect uri}` using the **Client Id** and  the URL from the **Redirect Uris** section from your Azure Active Directory application's **Configure** tab.</span></span>

![Zdroje][api-management-aad-resources]

<span data-ttu-id="a2ded-144">Jakmile jsou hodnoty zadané, příklad kódu by měl vrátit token podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="a2ded-144">Once the values are specified, the code example should return a token similar to the following example.</span></span>

![Token][api-management-arm-token]

<span data-ttu-id="a2ded-146">Před voláním operace zálohování a obnovení, které jsou popsané v následujících částech, nastavte hlavičce autorizace požadavku pro volání REST.</span><span class="sxs-lookup"><span data-stu-id="a2ded-146">Before calling the backup and restore operations described in the following sections, set the authorization request header for your REST call.</span></span>

```c#
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

## <span data-ttu-id="a2ded-147"><a name="step1"></a>Zálohování služby API Management</span><span class="sxs-lookup"><span data-stu-id="a2ded-147"><a name="step1"> </a>Back up an API Management service</span></span>
<span data-ttu-id="a2ded-148">Zálohování problém služby API Management následující požadavek HTTP:</span><span class="sxs-lookup"><span data-stu-id="a2ded-148">To back up an API Management service issue the following HTTP request:</span></span>

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

<span data-ttu-id="a2ded-149">Kde:</span><span class="sxs-lookup"><span data-stu-id="a2ded-149">where:</span></span>

* <span data-ttu-id="a2ded-150">`subscriptionId`– id předplatného obsahující služba API Management, kterou zkoušíte pro zálohování</span><span class="sxs-lookup"><span data-stu-id="a2ded-150">`subscriptionId` - id of the subscription containing the API Management service you are attempting to backup</span></span>
* <span data-ttu-id="a2ded-151">`resourceGroupName`-řetězec ve formátu 'Api - výchozí – {služba region}, kde `service-region` identifikuje oblasti Azure, kde je služba API Management se pokoušíte zálohování hostovaný, například`North-Central-US`</span><span class="sxs-lookup"><span data-stu-id="a2ded-151">`resourceGroupName` - a string in the form of 'Api-Default-{service-region}' where `service-region` identifies the Azure region where the API Management service you are trying to backup is hosted, e.g. `North-Central-US`</span></span>
* <span data-ttu-id="a2ded-152">`serviceName`-název služby API Management provádíte zálohování určenou při jeho vytvoření</span><span class="sxs-lookup"><span data-stu-id="a2ded-152">`serviceName` - the name of the API Management service you are making a backup of specified at the time of its creation</span></span>
* <span data-ttu-id="a2ded-153">`api-version`-nahradit`2014-02-14`</span><span class="sxs-lookup"><span data-stu-id="a2ded-153">`api-version` - replace  with `2014-02-14`</span></span>

<span data-ttu-id="a2ded-154">V těle žádosti zadejte název účtu úložiště Azure cíl, přístupový klíč, název kontejneru objektu blob a název zálohy:</span><span class="sxs-lookup"><span data-stu-id="a2ded-154">In the body of the request, specify the target Azure storage account name, access key, blob container name, and backup name:</span></span>

```
'{  
    storageAccount : {storage account name for the backup},  
    accessKey : {access key for the account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

<span data-ttu-id="a2ded-155">Nastavte hodnotu `Content-Type` hlavička požadavku na `application/json`.</span><span class="sxs-lookup"><span data-stu-id="a2ded-155">Set the value of the `Content-Type` request header to `application/json`.</span></span>

<span data-ttu-id="a2ded-156">Zálohování je dlouhotrvající operace, která může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="a2ded-156">Backup is a long running operation that may take multiple minutes to complete.</span></span>  <span data-ttu-id="a2ded-157">Pokud požadavek byl úspěšný a byl zahájen proces zálohování, dostanete `202 Accepted` stavový kód odpovědi s `Location` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="a2ded-157">If the request was successful and the backup process was initiated you’ll receive a `202 Accepted` response status code with a `Location` header.</span></span>  <span data-ttu-id="a2ded-158">Ujistěte se, "GET" požadavky na adresu URL v `Location` hlavičky k zjištění stavu operace.</span><span class="sxs-lookup"><span data-stu-id="a2ded-158">Make 'GET' requests to the URL in the `Location` header to find out the status of the operation.</span></span> <span data-ttu-id="a2ded-159">V průběhu zálohování můžete nadále budete dostávat '202 platných' stavový kód.</span><span class="sxs-lookup"><span data-stu-id="a2ded-159">While the backup is in progress you will continue to receive a '202 Accepted' status code.</span></span> <span data-ttu-id="a2ded-160">Kód odpovědi `200 OK` označí úspěšném dokončení operace zálohování.</span><span class="sxs-lookup"><span data-stu-id="a2ded-160">A Response code of `200 OK` will indicate successful completion of the backup operation.</span></span>

<span data-ttu-id="a2ded-161">Poznámka: následující omezení při vytváření zálohy požadavku.</span><span class="sxs-lookup"><span data-stu-id="a2ded-161">Please note the following constraints when making a backup request.</span></span>

* <span data-ttu-id="a2ded-162">**Kontejner** zadaná v těle žádosti **musí existovat**.</span><span class="sxs-lookup"><span data-stu-id="a2ded-162">**Container** specified in the request body **must exist**.</span></span>
* <span data-ttu-id="a2ded-163">Při zálohování se v průběhu **neměli žádné operace správy, služba** například SKU upgrade nebo přechod na starší verzi, změna názvu domény, atd.</span><span class="sxs-lookup"><span data-stu-id="a2ded-163">While backup is in progress you **should not attempt any service management operations** such as SKU upgrade or downgrade, domain name change, etc.</span></span>
* <span data-ttu-id="a2ded-164">Obnovení systému **zálohování záruku, že se pouze po dobu 30 dnů** od okamžiku jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="a2ded-164">Restore of a **backup is guaranteed only for 30 days** since the moment of its creation.</span></span>
* <span data-ttu-id="a2ded-165">**Data o využití** použitý k vytvoření sestavy analýzy **nezahrnuje** v záloze.</span><span class="sxs-lookup"><span data-stu-id="a2ded-165">**Usage data** used for creating analytics reports **is not included** in the backup.</span></span> <span data-ttu-id="a2ded-166">Použití [REST API služby Azure API Management] [ Azure API Management REST API] pravidelně načíst analytics sestavy z bezpečnostních důvodů udržován.</span><span class="sxs-lookup"><span data-stu-id="a2ded-166">Use [Azure API Management REST API][Azure API Management REST API] to periodically retrieve analytics reports for safekeeping.</span></span>
* <span data-ttu-id="a2ded-167">Frekvence, se kterým můžete provést zálohování služby ovlivní vaše plánovaného bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="a2ded-167">The frequency with which you perform service backups will affect your recovery point objective.</span></span> <span data-ttu-id="a2ded-168">Chcete-li minimalizovat ho doporučujeme implementace pravidelných záloh a také provádění zálohování na vyžádání po důležitých změnách služby API Management.</span><span class="sxs-lookup"><span data-stu-id="a2ded-168">To minimize it we advise implementing regular backups as well as performing on-demand backups after making important changes to your API Management service.</span></span>
* <span data-ttu-id="a2ded-169">**Změny** provedené v konfiguraci služby (např. rozhraní API, zásady, vzhled portálu vývojáře) při zálohování operace je v procesu **nemusí být zahrnuty do zálohování a proto budou ztraceny**.</span><span class="sxs-lookup"><span data-stu-id="a2ded-169">**Changes** made to the service configuration (e.g. APIs, policies, developer portal appearance) while backup operation is in process **might not be included in the backup and therefore will be lost**.</span></span>

## <span data-ttu-id="a2ded-170"><a name="step2"></a>Obnovení služby API Management</span><span class="sxs-lookup"><span data-stu-id="a2ded-170"><a name="step2"> </a>Restore an API Management service</span></span>
<span data-ttu-id="a2ded-171">Chcete-li obnovit API Management služby z dříve vytvořeného udělejte následující požadavek HTTP:</span><span class="sxs-lookup"><span data-stu-id="a2ded-171">To restore an API Management service from a previously created backup make the following HTTP request:</span></span>

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

<span data-ttu-id="a2ded-172">Kde:</span><span class="sxs-lookup"><span data-stu-id="a2ded-172">where:</span></span>

* <span data-ttu-id="a2ded-173">`subscriptionId`– id předplatného obsahující obnovujete zálohu do služby API Management</span><span class="sxs-lookup"><span data-stu-id="a2ded-173">`subscriptionId` - id of the subscription containing the API Management service you are restoring a backup into</span></span>
* <span data-ttu-id="a2ded-174">`resourceGroupName`-řetězec ve formátu 'Api - výchozí – {služba region}, kde `service-region` identifikuje oblasti Azure, kde je umístěn obnovujete zálohu do služby API Management, například`North-Central-US`</span><span class="sxs-lookup"><span data-stu-id="a2ded-174">`resourceGroupName` - a string in the form of 'Api-Default-{service-region}' where `service-region` identifies the Azure region where the API Management service you are restoring a backup into is hosted, e.g. `North-Central-US`</span></span>
* <span data-ttu-id="a2ded-175">`serviceName`-Název rozhraní API pro správu služby obnovena do určenou při jeho vytvoření</span><span class="sxs-lookup"><span data-stu-id="a2ded-175">`serviceName` - the name of the API Management service being restored into specified at the time of its creation</span></span>
* <span data-ttu-id="a2ded-176">`api-version`-nahradit`2014-02-14`</span><span class="sxs-lookup"><span data-stu-id="a2ded-176">`api-version` - replace  with `2014-02-14`</span></span>

<span data-ttu-id="a2ded-177">V těle žádosti zadejte umístění záložního souboru, tj. název účtu úložiště Azure, přístupový klíč, název kontejneru objektu blob a název zálohy:</span><span class="sxs-lookup"><span data-stu-id="a2ded-177">In the body of the request, specify the backup file location, i.e. Azure storage account name, access key, blob container name, and backup name:</span></span>

```
'{  
    storageAccount : {storage account name for the backup},  
    accessKey : {access key for the account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

<span data-ttu-id="a2ded-178">Nastavte hodnotu `Content-Type` hlavička požadavku na `application/json`.</span><span class="sxs-lookup"><span data-stu-id="a2ded-178">Set the value of the `Content-Type` request header to `application/json`.</span></span>

<span data-ttu-id="a2ded-179">Obnovení je dlouhotrvající operace, která může trvat až 30 nebo více minut na dokončení.</span><span class="sxs-lookup"><span data-stu-id="a2ded-179">Restore is a long running operation that may take up to 30 or more minutes to complete.</span></span>  <span data-ttu-id="a2ded-180">Pokud požadavek byl úspěšný a byl zahájen proces obnovení, dostanete `202 Accepted` stavový kód odpovědi s `Location` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="a2ded-180">If the request was successful and the restore process was initiated you’ll receive a `202 Accepted` response status code with a `Location` header.</span></span>  <span data-ttu-id="a2ded-181">Ujistěte se, "GET" požadavky na adresu URL v `Location` hlavičky k zjištění stavu operace.</span><span class="sxs-lookup"><span data-stu-id="a2ded-181">Make 'GET' requests to the URL in the `Location` header to find out the status of the operation.</span></span> <span data-ttu-id="a2ded-182">V průběhu obnovení budou nadále přijímat '202 platných' stavový kód.</span><span class="sxs-lookup"><span data-stu-id="a2ded-182">While the restore is in progress you will continue to receive '202 Accepted' status code.</span></span> <span data-ttu-id="a2ded-183">Kód odpovědi `200 OK` označí úspěšném dokončení operace obnovení.</span><span class="sxs-lookup"><span data-stu-id="a2ded-183">A response code of `200 OK` will indicate successful completion of the restore operation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a2ded-184">**Verze SKU** služby obnovena do **musí odpovídat** SKU služby zálohovaných obnovena.</span><span class="sxs-lookup"><span data-stu-id="a2ded-184">**The SKU** of the service being restored into **must match** the SKU of the backed up service being restored.</span></span>
>
> <span data-ttu-id="a2ded-185">**Změny** provedené konfigurace služby (např. rozhraní API, zásady, vzhled portálu vývojáře) při obnovení v probíhá operace **může dojít k přepsání**.</span><span class="sxs-lookup"><span data-stu-id="a2ded-185">**Changes** made to the service configuration (e.g. APIs, policies, developer portal appearance) while restore operation is in progress **could be overwritten**.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="a2ded-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a2ded-186">Next steps</span></span>
<span data-ttu-id="a2ded-187">Podívejte se v těchto blozích Microsoft pro dva různé postupy procesu zálohování a obnovení.</span><span class="sxs-lookup"><span data-stu-id="a2ded-187">Check out the following Microsoft blogs for two different walkthroughs of the backup/restore process.</span></span>

* [<span data-ttu-id="a2ded-188">Replikovat účty Azure API Management</span><span class="sxs-lookup"><span data-stu-id="a2ded-188">Replicate Azure API Management Accounts</span></span>](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
  * <span data-ttu-id="a2ded-189">Děkujeme, že k Gisela jeho příspěvku v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="a2ded-189">Thank you to Gisela for her contribution to this article.</span></span>
* [<span data-ttu-id="a2ded-190">Azure API Management: Zálohování a obnovení konfigurace</span><span class="sxs-lookup"><span data-stu-id="a2ded-190">Azure API Management: Backing Up and Restoring Configuration</span></span>](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
  * <span data-ttu-id="a2ded-191">Přístup podrobné podle Stanislav neodpovídá oficiální pokyny, ale je velmi zajímavé.</span><span class="sxs-lookup"><span data-stu-id="a2ded-191">The approach detailed by Stuart does not match the official guidance but it is very interesting.</span></span>

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
