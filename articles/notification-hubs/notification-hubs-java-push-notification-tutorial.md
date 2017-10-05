---
title: "Jak používat centra oznámení s Javou"
description: "Naučte se používat Azure Notification Hubs z Java back-end."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 4c3f966d-0158-4a48-b949-9fa3666cb7e4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: java
ms.devlang: java
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 41f978750ddef9f7e878c65b0017e909720154aa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-notification-hubs-from-java"></a><span data-ttu-id="a1cc3-103">Jak používat centra oznámení z Java</span><span class="sxs-lookup"><span data-stu-id="a1cc3-103">How to use Notification Hubs from Java</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

<span data-ttu-id="a1cc3-104">Toto téma popisuje klíčové funkce nové plně podporované oficiální Azure oznámení centra sady Java SDK.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-104">This topic describes the key features of the new fully supported official Azure Notification Hub Java SDK.</span></span> <span data-ttu-id="a1cc3-105">Toto je opensourcový projekt a lze je zobrazit celý kód SDK na [sady Java SDK].</span><span class="sxs-lookup"><span data-stu-id="a1cc3-105">This is an open source project and you can view the entire SDK code at [Java SDK].</span></span> 

<span data-ttu-id="a1cc3-106">Obecně platí, můžete ke všem funkcím centra oznámení z Java/PHP nebo Python nebo Ruby back-end pomocí rozhraní REST centra oznámení, jak je popsáno v tématu MSDN [rozhraní API REST centra oznámení](http://msdn.microsoft.com/library/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="a1cc3-106">In general, you can access all Notification Hubs features from a Java/PHP/Python/Ruby back-end using the Notification Hub REST interface as described in the MSDN topic [Notification Hubs REST APIs](http://msdn.microsoft.com/library/dn223264.aspx).</span></span> <span data-ttu-id="a1cc3-107">Tuto sadu Java SDK poskytuje dynamické obálku přes tato rozhraní REST v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-107">This Java SDK provides a thin wrapper over these REST interfaces in Java.</span></span> 

<span data-ttu-id="a1cc3-108">Sada SDK podporuje aktuálně:</span><span class="sxs-lookup"><span data-stu-id="a1cc3-108">The SDK supports currently:</span></span>

* <span data-ttu-id="a1cc3-109">CRUD v Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="a1cc3-109">CRUD on Notification Hubs</span></span> 
* <span data-ttu-id="a1cc3-110">CRUD při registraci</span><span class="sxs-lookup"><span data-stu-id="a1cc3-110">CRUD on Registrations</span></span>
* <span data-ttu-id="a1cc3-111">Instalace správy</span><span class="sxs-lookup"><span data-stu-id="a1cc3-111">Installation Management</span></span>
* <span data-ttu-id="a1cc3-112">Import a Export registrace</span><span class="sxs-lookup"><span data-stu-id="a1cc3-112">Import/Export Registrations</span></span>
* <span data-ttu-id="a1cc3-113">Regulární zasílá</span><span class="sxs-lookup"><span data-stu-id="a1cc3-113">Regular Sends</span></span>
* <span data-ttu-id="a1cc3-114">Naplánované zasílá</span><span class="sxs-lookup"><span data-stu-id="a1cc3-114">Scheduled Sends</span></span>
* <span data-ttu-id="a1cc3-115">Asynchronní operace prostřednictvím Java NIO</span><span class="sxs-lookup"><span data-stu-id="a1cc3-115">Async operations via Java NIO</span></span>
* <span data-ttu-id="a1cc3-116">Podporované platformy: APNS (iOS), služby GCM (Android), WNS (aplikace pro Windows Store), MPNS (Windows Phone), ADM (Amazon Kindle aktivuje), Baidu (bez služeb Google Android)</span><span class="sxs-lookup"><span data-stu-id="a1cc3-116">Supported platforms: APNS (iOS), GCM (Android), WNS (Windows Store apps), MPNS(Windows Phone), ADM (Amazon Kindle Fire), Baidu (Android without Google services)</span></span> 

## <a name="sdk-usage"></a><span data-ttu-id="a1cc3-117">Použití sady SDK</span><span class="sxs-lookup"><span data-stu-id="a1cc3-117">SDK Usage</span></span>
### <a name="compile-and-build"></a><span data-ttu-id="a1cc3-118">Kompilace a sestavení</span><span class="sxs-lookup"><span data-stu-id="a1cc3-118">Compile and build</span></span>
<span data-ttu-id="a1cc3-119">Použití [Maven]</span><span class="sxs-lookup"><span data-stu-id="a1cc3-119">Use [Maven]</span></span>

<span data-ttu-id="a1cc3-120">K sestavení:</span><span class="sxs-lookup"><span data-stu-id="a1cc3-120">To build:</span></span>

    mvn package

## <a name="code"></a><span data-ttu-id="a1cc3-121">Kód</span><span class="sxs-lookup"><span data-stu-id="a1cc3-121">Code</span></span>
### <a name="notification-hub-cruds"></a><span data-ttu-id="a1cc3-122">CRUDs centra oznámení</span><span class="sxs-lookup"><span data-stu-id="a1cc3-122">Notification Hub CRUDs</span></span>
<span data-ttu-id="a1cc3-123">**Vytvořte NamespaceManager:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-123">**Create a NamespaceManager:**</span></span>

    NamespaceManager namespaceManager = new NamespaceManager("connection string")

<span data-ttu-id="a1cc3-124">**Vytvoření centra oznámení:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-124">**Create Notification Hub:**</span></span>

    NotificationHubDescription hub = new NotificationHubDescription("hubname");
    hub.setWindowsCredential(new WindowsCredential("sid","key"));
    hub = namespaceManager.createNotificationHub(hub);

 <span data-ttu-id="a1cc3-125">NEBO</span><span class="sxs-lookup"><span data-stu-id="a1cc3-125">OR</span></span>

    hub = new NotificationHub("connection string", "hubname");

<span data-ttu-id="a1cc3-126">**Získáte Centrum oznámení:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-126">**Get Notification Hub:**</span></span>

    hub = namespaceManager.getNotificationHub("hubname");

<span data-ttu-id="a1cc3-127">**Aktualizujte Centrum oznámení:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-127">**Update Notification Hub:**</span></span>

    hub.setMpnsCredential(new MpnsCredential("mpnscert", "mpnskey"));
    hub = namespaceManager.updateNotificationHub(hub);

<span data-ttu-id="a1cc3-128">**Odstraňte Centrum oznámení:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-128">**Delete Notification Hub:**</span></span>

    namespaceManager.deleteNotificationHub("hubname");

### <a name="registration-cruds"></a><span data-ttu-id="a1cc3-129">Registrace CRUDs</span><span class="sxs-lookup"><span data-stu-id="a1cc3-129">Registration CRUDs</span></span>
<span data-ttu-id="a1cc3-130">**Vytvoření centra oznámení klienta:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-130">**Create a Notification Hub client:**</span></span>

    hub = new NotificationHub("connection string", "hubname");

<span data-ttu-id="a1cc3-131">**Vytvořte registrace systému Windows:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-131">**Create Windows registration:**</span></span>

    WindowsRegistration reg = new WindowsRegistration(new URI(CHANNELURI));
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");    
    hub.createRegistration(reg);

<span data-ttu-id="a1cc3-132">**Vytvořte registrace iOS:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-132">**Create iOS registration:**</span></span>

    AppleRegistration reg = new AppleRegistration(DEVICETOKEN);
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");
    hub.createRegistration(reg);

<span data-ttu-id="a1cc3-133">Podobně můžete vytvořit registrace pro Android (GCM), Windows Phone (MPNS) a Kindle ještě efektivněji (ADM).</span><span class="sxs-lookup"><span data-stu-id="a1cc3-133">Similarly you can create registrations for Android (GCM), Windows Phone (MPNS), and Kindle Fire (ADM).</span></span>

<span data-ttu-id="a1cc3-134">**Vytvořte šablonu registrace:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-134">**Create template registrations:**</span></span>

    WindowsTemplateRegistration reg = new WindowsTemplateRegistration(new URI(CHANNELURI), WNSBODYTEMPLATE);
    reg.getHeaders().put("X-WNS-Type", "wns/toast");
    hub.createRegistration(reg);

<span data-ttu-id="a1cc3-135">**Vytvořit registrace pomocí vytvořte registrationid + upsert vzor**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-135">**Create registrations using create registrationid + upsert pattern**</span></span>

<span data-ttu-id="a1cc3-136">Odebere duplicitní položky z důvodu libovolné ztraceny odpovědi při ukládání ID registrace zařízení:</span><span class="sxs-lookup"><span data-stu-id="a1cc3-136">Removes duplicates due to any lost responses if storing registration ids on the device:</span></span>

    String id = hub.createRegistrationId();
    WindowsRegistration reg = new WindowsRegistration(id, new URI(CHANNELURI));
    hub.upsertRegistration(reg);

<span data-ttu-id="a1cc3-137">**Aktualizujte registrace:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-137">**Update registrations:**</span></span>

    hub.updateRegistration(reg);

<span data-ttu-id="a1cc3-138">**Odstraňte registrace:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-138">**Delete registrations:**</span></span>

    hub.deleteRegistration(regid);

<span data-ttu-id="a1cc3-139">**Registrace dotazu:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-139">**Query registrations:**</span></span>

* <span data-ttu-id="a1cc3-140">**Získáte jeden registrace:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-140">**Get single registration:**</span></span>
  
    <span data-ttu-id="a1cc3-141">hub.getRegistration(regid);</span><span class="sxs-lookup"><span data-stu-id="a1cc3-141">hub.getRegistration(regid);</span></span>
* <span data-ttu-id="a1cc3-142">**V centru získáte všechny registrace:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-142">**Get all registrations in hub:**</span></span>
  
    <span data-ttu-id="a1cc3-143">hub.getRegistrations();</span><span class="sxs-lookup"><span data-stu-id="a1cc3-143">hub.getRegistrations();</span></span>
* <span data-ttu-id="a1cc3-144">**Získání registrace pomocí značky:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-144">**Get registrations with tag:**</span></span>
  
    <span data-ttu-id="a1cc3-145">hub.getRegistrationsByTag("myTag");</span><span class="sxs-lookup"><span data-stu-id="a1cc3-145">hub.getRegistrationsByTag("myTag");</span></span>
* <span data-ttu-id="a1cc3-146">**Získáte registrací kanál:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-146">**Get registrations by channel:**</span></span>
  
    <span data-ttu-id="a1cc3-147">hub.getRegistrationsByChannel("devicetoken");</span><span class="sxs-lookup"><span data-stu-id="a1cc3-147">hub.getRegistrationsByChannel("devicetoken");</span></span>

<span data-ttu-id="a1cc3-148">Všechny dotazy na kolekce podporují $top a pokračování tokenů.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-148">All collection queries support $top and continuation tokens.</span></span>

### <a name="installation-api-usage"></a><span data-ttu-id="a1cc3-149">Instalace rozhraní API využití</span><span class="sxs-lookup"><span data-stu-id="a1cc3-149">Installation API usage</span></span>
<span data-ttu-id="a1cc3-150">Instalace rozhraní API je alternativní mechanismus pro správu registrace.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-150">Installation API is an alternative mechanism for registration management.</span></span> <span data-ttu-id="a1cc3-151">Místo zachování více registrace, které není triviální a může se snadno provést neoprávněně nebo neefektivnímu, je možné použít objekt JEDNOTNOU instalaci.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-151">Instead of maintaining multiple registrations which is not trivial and may be easily done wrongly or inefficiently, it is now possible to use a SINGLE Installation object.</span></span> <span data-ttu-id="a1cc3-152">Instalace obsahuje všechno, co potřebujete: push kanál (token zařízení), značky, šablony, sekundární dlaždice (pro WNS a APNS).</span><span class="sxs-lookup"><span data-stu-id="a1cc3-152">Installation contains everything you need: push channel (device token), tags, templates, secondary tiles (for WNS and APNS).</span></span> <span data-ttu-id="a1cc3-153">Nemusíte volání služby získat Id už – právě generovat identifikátor GUID nebo jakýkoli jiný identifikátor, uložte jej na zařízení a poslat váš back-end společně s nabízené kanál (token zařízení).</span><span class="sxs-lookup"><span data-stu-id="a1cc3-153">You don't need to call the service to get Id anymore - just generate GUID or any other identifier, keep it on device and send to your backend together with push channel (device token).</span></span> <span data-ttu-id="a1cc3-154">Na back-end použijte pouze v jediném volání: CreateOrUpdateInstallation, je plně idempotent, takže Nebojte se v případě potřeby opakujte.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-154">On the backend you should only do a single call: CreateOrUpdateInstallation, it is fully idempotent, so feel free to retry if needed.</span></span>

<span data-ttu-id="a1cc3-155">Jako příklad ještě efektivněji Kindle Amazon vypadat třeba takto:</span><span class="sxs-lookup"><span data-stu-id="a1cc3-155">As example for Amazon Kindle Fire it looks like this:</span></span>

    Installation installation = new Installation("installation-id", NotificationPlatform.Adm, "adm-push-channel");
    hub.createOrUpdateInstallation(installation);

<span data-ttu-id="a1cc3-156">Pokud chcete aktualizovat:</span><span class="sxs-lookup"><span data-stu-id="a1cc3-156">If you want to update it:</span></span> 

    installation.addTag("foo");
    installation.addTemplate("template1", new InstallationTemplate("{\"data\":{\"key1\":\"$(value1)\"}}","tag-for-template1"));
    installation.addTemplate("template2", new InstallationTemplate("{\"data\":{\"key2\":\"$(value2)\"}}","tag-for-template2"));
    hub.createOrUpdateInstallation(installation);

<span data-ttu-id="a1cc3-157">Pro pokročilé scénáře máme částečné aktualizace schopností, který umožňuje upravovat pouze konkrétní vlastnosti objektu instalace.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-157">For advanced scenarios we have partial update capability which allows to modify only particular properties of the installation object.</span></span> <span data-ttu-id="a1cc3-158">V podstatě částečné aktualizace je podmnožinou JSON oprava operace, které můžete spustit instalaci objektu.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-158">Basically partial update is subset of JSON Patch operations you can run against Installation object.</span></span>

    PartialUpdateOperation addChannel = new PartialUpdateOperation(UpdateOperationType.Add, "/pushChannel", "adm-push-channel2");
    PartialUpdateOperation addTag = new PartialUpdateOperation(UpdateOperationType.Add, "/tags", "bar");
    PartialUpdateOperation replaceTemplate = new PartialUpdateOperation(UpdateOperationType.Replace, "/templates/template1", new InstallationTemplate("{\"data\":{\"key3\":\"$(value3)\"}}","tag-for-template1")).toJson());
    hub.patchInstallation("installation-id", addChannel, addTag, replaceTemplate);

<span data-ttu-id="a1cc3-159">Odstraňte instalace:</span><span class="sxs-lookup"><span data-stu-id="a1cc3-159">Delete Installation:</span></span>

    hub.deleteInstallation(installation.getInstallationId());

<span data-ttu-id="a1cc3-160">CreateOrUpdate, Patch a Delete jsou nakonec byl konzistentní se Get.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-160">CreateOrUpdate, Patch and Delete are eventually consistent with Get.</span></span> <span data-ttu-id="a1cc3-161">Požadovaná operace právě přejde do fronty systému během volání a budou spuštěny v pozadí.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-161">Your requested operation just goes to the system queue during the call and will be executed in background.</span></span> <span data-ttu-id="a1cc3-162">Všimněte si, že Get není určen pro scénář hlavní runtime, ale jenom pro ladění a odstraňování potíží je úzce omezen pomocí služby.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-162">Note that Get is not designed for main runtime scenario but just for debug and troubleshooting purposes, it is tightly throttled by the service.</span></span>

<span data-ttu-id="a1cc3-163">Postup odeslání pro instalace je stejné jako registrace.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-163">Send flow for Installations is the same as for Registrations.</span></span> <span data-ttu-id="a1cc3-164">Zavedli jsme právě možnost cíle oznámení na konkrétní instalaci – stačí použít značka "InstallationId: {desired-id}".</span><span class="sxs-lookup"><span data-stu-id="a1cc3-164">We've just introduced an option to target notification to the particular Installation - just use tag "InstallationId:{desired-id}".</span></span> <span data-ttu-id="a1cc3-165">Pro případ nad ním bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="a1cc3-165">For case above it would look like this:</span></span>

    Notification n = Notification.createWindowsNotification("WNS body");
    hub.sendNotification(n, "InstallationId:{installation-id}");

<span data-ttu-id="a1cc3-166">Pro jednu z několika šablon:</span><span class="sxs-lookup"><span data-stu-id="a1cc3-166">For one of several templates:</span></span>

    Map<String, String> prop =  new HashMap<String, String>();
    prop.put("value3", "some value");
    Notification n = Notification.createTemplateNotification(prop);
    hub.sendNotification(n, "InstallationId:{installation-id} && tag-for-template1");

### <a name="schedule-notifications-available-for-standard-tier"></a><span data-ttu-id="a1cc3-167">Naplánovat oznámení (k dispozici pro úroveň STANDARD)</span><span class="sxs-lookup"><span data-stu-id="a1cc3-167">Schedule Notifications (available for STANDARD Tier)</span></span>
<span data-ttu-id="a1cc3-168">Stejné jako regulární send, ale jeden další parametr - scheduledTime s informacemi o tom, kdy bude doručena oznámení.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-168">The same as regular send but with one additional parameter - scheduledTime which says when notification should be delivered.</span></span> <span data-ttu-id="a1cc3-169">Služba přijímá libovolného bodu čas mezi nyní + 5 minut a nyní + 7 dní.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-169">Service accepts any point of time between now + 5 minutes and now + 7 days.</span></span>

<span data-ttu-id="a1cc3-170">**Plán Windows nativní oznámení:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-170">**Schedule a Windows native notification:**</span></span>

    Calendar c = Calendar.getInstance();
    c.add(Calendar.DATE, 1);    
    Notification n = Notification.createWindowsNotification("WNS body");
    hub.scheduleNotification(n, c.getTime());

### <a name="importexport-available-for-standard-tier"></a><span data-ttu-id="a1cc3-171">Import a Export (k dispozici pro úroveň STANDARD)</span><span class="sxs-lookup"><span data-stu-id="a1cc3-171">Import/Export (available for STANDARD Tier)</span></span>
<span data-ttu-id="a1cc3-172">Někdy je nutné provést hromadné operace s registrací.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-172">Sometimes it is required to perform bulk operation against registrations.</span></span> <span data-ttu-id="a1cc3-173">Obvykle je pro integraci v rámci jiného systému nebo stejně masivní opravu. Tím vyjádříte aktualizaci značek.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-173">Usually it is for integration with another system or just a massive fix to say update the tags.</span></span> <span data-ttu-id="a1cc3-174">Důrazně není doporučujeme používat toku Get nebo aktualizovat, pokud jsme mluvíme o tisíce registrace.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-174">It is strongly not recommended to use Get/Update flow if we are talking about thousands of registrations.</span></span> <span data-ttu-id="a1cc3-175">Funkce importu a exportu je navržené tak, aby pokrýval scénáře.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-175">Import/Export capability is designed to cover the scenario.</span></span> <span data-ttu-id="a1cc3-176">V podstatě poskytnete přístup na některé kontejner objektů blob v rámci účtu úložiště jako zdroj příchozích dat a umístění pro výstup.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-176">Basically you provide an access to some blob container under your storage account as a source of incoming data and location for output.</span></span>

<span data-ttu-id="a1cc3-177">**Odeslání úlohy exportu:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-177">**Submit export job:**</span></span>

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ExportRegistrations);
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);


<span data-ttu-id="a1cc3-178">**Odeslání úlohy importu:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-178">**Submit import job:**</span></span>

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ImportCreateRegistrations);
    job.setImportFileUri("input file uri with SAS signature");
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);

<span data-ttu-id="a1cc3-179">**Počkejte, dokud se provádí úlohy:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-179">**Wait until job is done:**</span></span>

    while(true){
        Thread.sleep(1000);
        job = hub.getNotificationHubJob(job.getJobId());
        if(job.getJobStatus() == NotificationHubJobStatus.Completed)
            break;
    }       

<span data-ttu-id="a1cc3-180">**Získání všech úloh:**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-180">**Get all jobs:**</span></span>

    List<NotificationHubJob> jobs = hub.getAllNotificationHubJobs();

<span data-ttu-id="a1cc3-181">**Identifikátor URI s SAS podpis:** Toto je adresa URL některé objektu blob souboru nebo kontejneru objektů blob a sadu parametrů, jako jsou oprávnění a čas vypršení platnosti a podpis všechny tyto věci, které jsou vytvořené pomocí klíče SAS účtu.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-181">**URI with SAS signature:** This is the URL of some blob file or blob container plus set of parameters like permissions and expiration time plus signature of all these things made using account's SAS key.</span></span> <span data-ttu-id="a1cc3-182">Azure SDK pro jazyk Java úložiště má bohaté možnosti, včetně vytváření takové druh identifikátorů URI.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-182">Azure Storage Java SDK has rich capabilities including creation of such kind of URIs.</span></span> <span data-ttu-id="a1cc3-183">Jako alternativní jednoduché může trvat podívejte se na ImportExportE2E třídy test (z githubu umístění), který má velmi základní a compact implementace podpisový algoritmus.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-183">As simple alternative you can take a look at ImportExportE2E test class (from the github location) which has very basic and compact implementation of signing algorithm.</span></span>

### <a name="send-notifications"></a><span data-ttu-id="a1cc3-184">Odesílání oznámení</span><span class="sxs-lookup"><span data-stu-id="a1cc3-184">Send Notifications</span></span>
<span data-ttu-id="a1cc3-185">Objekt oznámení je jednoduše text se záhlavími, některé metody nástroj pomoci při vytváření objektů nativní a šablony oznámení.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-185">The Notification object is simply a body with headers, some utility methods help in building the native and template notifications objects.</span></span>

* <span data-ttu-id="a1cc3-186">**Windows Store a Windows Phone 8.1 (bez Silverlight)**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-186">**Windows Store and Windows Phone 8.1 (non-Silverlight)**</span></span>
  
        String toast = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello from Java!</text></binding></visual></toast>";
        Notification n = Notification.createWindowsNotification(toast);
        hub.sendNotification(n);
* <span data-ttu-id="a1cc3-187">**iOS**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-187">**iOS**</span></span>
  
        String alert = "{\"aps\":{\"alert\":\"Hello from Java!\"}}";
        Notification n = Notification.createAppleNotification(alert);
        hub.sendNotification(n);
* <span data-ttu-id="a1cc3-188">**Android**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-188">**Android**</span></span>
  
        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createGcmNotification(message);
        hub.sendNotification(n);
* <span data-ttu-id="a1cc3-189">**Windows Phone 8.0 a 8.1 Silverlight**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-189">**Windows Phone 8.0 and 8.1 Silverlight**</span></span>
  
        String toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>Hello from Java!</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
        Notification n = Notification.createMpnsNotification(toast);
        hub.sendNotification(n);
* <span data-ttu-id="a1cc3-190">**Kindle ještě efektivněji**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-190">**Kindle Fire**</span></span>
  
        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createAdmNotification(message);
        hub.sendNotification(n);
* <span data-ttu-id="a1cc3-191">**Poslat značky**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-191">**Send to Tags**</span></span>
  
        Set<String> tags = new HashSet<String>();
        tags.add("boo");
        tags.add("foo");
        hub.sendNotification(n, tags);
* <span data-ttu-id="a1cc3-192">**Poslat výraz značky**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-192">**Send to tag expression**</span></span>       
  
        hub.sendNotification(n, "foo && ! bar");
* <span data-ttu-id="a1cc3-193">**Odeslat oznámení šablony**</span><span class="sxs-lookup"><span data-stu-id="a1cc3-193">**Send template notification**</span></span>
  
        Map<String, String> prop =  new HashMap<String, String>();
        prop.put("prop1", "v1");
        prop.put("prop2", "v2");
        Notification n = Notification.createTemplateNotification(prop);
        hub.sendNotification(n);

<span data-ttu-id="a1cc3-194">Spuštěním kódu Java by měl nyní vytvořit oznámení, které jsou na cílovém zařízení.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-194">Running your Java code should now produce a notification appearing on your target device.</span></span>

## <span data-ttu-id="a1cc3-195"><a name="next-steps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="a1cc3-195"><a name="next-steps"></a>Next Steps</span></span>
<span data-ttu-id="a1cc3-196">V tomto tématu jsme vám ukázal, jak vytvořit jednoduché Java REST klienta pro centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-196">In this topic we showed how to create a simple Java REST client for Notification Hubs.</span></span> <span data-ttu-id="a1cc3-197">Odsud můžete:</span><span class="sxs-lookup"><span data-stu-id="a1cc3-197">From here you can:</span></span>

* <span data-ttu-id="a1cc3-198">Stáhnout kompletní [sady Java SDK], který obsahuje celý kód SDK.</span><span class="sxs-lookup"><span data-stu-id="a1cc3-198">Download the full [Java SDK], which contains the entire SDK code.</span></span> 
* <span data-ttu-id="a1cc3-199">Přehrání s ukázky:</span><span class="sxs-lookup"><span data-stu-id="a1cc3-199">Play with the samples:</span></span>
  * <span data-ttu-id="a1cc3-200">[Začínáme s Notification Hubs]</span><span class="sxs-lookup"><span data-stu-id="a1cc3-200">[Get Started with Notification Hubs]</span></span>
  * <span data-ttu-id="a1cc3-201">[Odesílání novinek]</span><span class="sxs-lookup"><span data-stu-id="a1cc3-201">[Send breaking news]</span></span>
  * <span data-ttu-id="a1cc3-202">[Odesílání lokalizované novinek]</span><span class="sxs-lookup"><span data-stu-id="a1cc3-202">[Send localized breaking news]</span></span>
  * <span data-ttu-id="a1cc3-203">[Odesílání oznámení ověřeným uživatelům]</span><span class="sxs-lookup"><span data-stu-id="a1cc3-203">[Send notifications to authenticated users]</span></span>
  * <span data-ttu-id="a1cc3-204">[Odesílání oznámení napříč platformami ověřeným uživatelům]</span><span class="sxs-lookup"><span data-stu-id="a1cc3-204">[Send cross-platform notifications to authenticated users]</span></span>

<span data-ttu-id="a1cc3-205">[sady Java SDK]: https://github.com/Azure/azure-notificationhubs-java-backend</span><span class="sxs-lookup"><span data-stu-id="a1cc3-205">[Java SDK]: https://github.com/Azure/azure-notificationhubs-java-backend</span></span>
[Get started tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
<span data-ttu-id="a1cc3-206">[Začínáme s Notification Hubs]: http://www.windowsazure.com/manage/services/notification-hubs/getting-started-windows-dotnet/</span><span class="sxs-lookup"><span data-stu-id="a1cc3-206">[Get Started with Notification Hubs]: http://www.windowsazure.com/manage/services/notification-hubs/getting-started-windows-dotnet/</span></span>
<span data-ttu-id="a1cc3-207">[Odesílání novinek]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-dotnet/</span><span class="sxs-lookup"><span data-stu-id="a1cc3-207">[Send breaking news]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-dotnet/</span></span>
<span data-ttu-id="a1cc3-208">[Odesílání lokalizované novinek]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-localized-dotnet/</span><span class="sxs-lookup"><span data-stu-id="a1cc3-208">[Send localized breaking news]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-localized-dotnet/</span></span>
<span data-ttu-id="a1cc3-209">[Odesílání oznámení ověřeným uživatelům]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users/</span><span class="sxs-lookup"><span data-stu-id="a1cc3-209">[Send notifications to authenticated users]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users/</span></span>
<span data-ttu-id="a1cc3-210">[Odesílání oznámení napříč platformami ověřeným uživatelům]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users-xplat-mobile-services/</span><span class="sxs-lookup"><span data-stu-id="a1cc3-210">[Send cross-platform notifications to authenticated users]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users-xplat-mobile-services/</span></span>
<span data-ttu-id="a1cc3-211">[Maven]: http://maven.apache.org/</span><span class="sxs-lookup"><span data-stu-id="a1cc3-211">[Maven]: http://maven.apache.org/</span></span>

