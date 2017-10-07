---
title: "aaaHow toouse centra oznámení s Javou"
description: "Zjistěte, jak toouse Azure Notification Hubs z Java back-end."
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
ms.openlocfilehash: afcf305b1acd9ee28ee4889040ece59d9399d29d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-notification-hubs-from-java"></a><span data-ttu-id="a41a9-103">Jak toouse centra oznámení z Java</span><span class="sxs-lookup"><span data-stu-id="a41a9-103">How toouse Notification Hubs from Java</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

<span data-ttu-id="a41a9-104">Toto téma popisuje klíčové funkce hello hello nové plně podporované oficiální Azure Notification Hub Java SDK.</span><span class="sxs-lookup"><span data-stu-id="a41a9-104">This topic describes hello key features of hello new fully supported official Azure Notification Hub Java SDK.</span></span> <span data-ttu-id="a41a9-105">Toto je opensourcový projekt a lze je zobrazit hello celý kód SDK na [sady Java SDK].</span><span class="sxs-lookup"><span data-stu-id="a41a9-105">This is an open source project and you can view hello entire SDK code at [Java SDK].</span></span> 

<span data-ttu-id="a41a9-106">Obecně platí, můžete ke všem funkcím centra oznámení z Java/PHP nebo Python nebo Ruby back-end pomocí rozhraní REST centra oznámení hello, jak je popsáno v tématu MSDN hello [rozhraní API REST centra oznámení](http://msdn.microsoft.com/library/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="a41a9-106">In general, you can access all Notification Hubs features from a Java/PHP/Python/Ruby back-end using hello Notification Hub REST interface as described in hello MSDN topic [Notification Hubs REST APIs](http://msdn.microsoft.com/library/dn223264.aspx).</span></span> <span data-ttu-id="a41a9-107">Tuto sadu Java SDK poskytuje dynamické obálku přes tato rozhraní REST v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="a41a9-107">This Java SDK provides a thin wrapper over these REST interfaces in Java.</span></span> 

<span data-ttu-id="a41a9-108">Hello SDK podporuje aktuálně:</span><span class="sxs-lookup"><span data-stu-id="a41a9-108">hello SDK supports currently:</span></span>

* <span data-ttu-id="a41a9-109">CRUD v Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="a41a9-109">CRUD on Notification Hubs</span></span> 
* <span data-ttu-id="a41a9-110">CRUD při registraci</span><span class="sxs-lookup"><span data-stu-id="a41a9-110">CRUD on Registrations</span></span>
* <span data-ttu-id="a41a9-111">Instalace správy</span><span class="sxs-lookup"><span data-stu-id="a41a9-111">Installation Management</span></span>
* <span data-ttu-id="a41a9-112">Import a Export registrace</span><span class="sxs-lookup"><span data-stu-id="a41a9-112">Import/Export Registrations</span></span>
* <span data-ttu-id="a41a9-113">Regulární zasílá</span><span class="sxs-lookup"><span data-stu-id="a41a9-113">Regular Sends</span></span>
* <span data-ttu-id="a41a9-114">Naplánované zasílá</span><span class="sxs-lookup"><span data-stu-id="a41a9-114">Scheduled Sends</span></span>
* <span data-ttu-id="a41a9-115">Asynchronní operace prostřednictvím Java NIO</span><span class="sxs-lookup"><span data-stu-id="a41a9-115">Async operations via Java NIO</span></span>
* <span data-ttu-id="a41a9-116">Podporované platformy: APNS (iOS), služby GCM (Android), WNS (aplikace pro Windows Store), MPNS (Windows Phone), ADM (Amazon Kindle aktivuje), Baidu (bez služeb Google Android)</span><span class="sxs-lookup"><span data-stu-id="a41a9-116">Supported platforms: APNS (iOS), GCM (Android), WNS (Windows Store apps), MPNS(Windows Phone), ADM (Amazon Kindle Fire), Baidu (Android without Google services)</span></span> 

## <a name="sdk-usage"></a><span data-ttu-id="a41a9-117">Použití sady SDK</span><span class="sxs-lookup"><span data-stu-id="a41a9-117">SDK Usage</span></span>
### <a name="compile-and-build"></a><span data-ttu-id="a41a9-118">Kompilace a sestavení</span><span class="sxs-lookup"><span data-stu-id="a41a9-118">Compile and build</span></span>
<span data-ttu-id="a41a9-119">Použití [Maven]</span><span class="sxs-lookup"><span data-stu-id="a41a9-119">Use [Maven]</span></span>

<span data-ttu-id="a41a9-120">toobuild:</span><span class="sxs-lookup"><span data-stu-id="a41a9-120">toobuild:</span></span>

    mvn package

## <a name="code"></a><span data-ttu-id="a41a9-121">Kód</span><span class="sxs-lookup"><span data-stu-id="a41a9-121">Code</span></span>
### <a name="notification-hub-cruds"></a><span data-ttu-id="a41a9-122">CRUDs centra oznámení</span><span class="sxs-lookup"><span data-stu-id="a41a9-122">Notification Hub CRUDs</span></span>
<span data-ttu-id="a41a9-123">**Vytvořte NamespaceManager:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-123">**Create a NamespaceManager:**</span></span>

    NamespaceManager namespaceManager = new NamespaceManager("connection string")

<span data-ttu-id="a41a9-124">**Vytvoření centra oznámení:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-124">**Create Notification Hub:**</span></span>

    NotificationHubDescription hub = new NotificationHubDescription("hubname");
    hub.setWindowsCredential(new WindowsCredential("sid","key"));
    hub = namespaceManager.createNotificationHub(hub);

 <span data-ttu-id="a41a9-125">NEBO</span><span class="sxs-lookup"><span data-stu-id="a41a9-125">OR</span></span>

    hub = new NotificationHub("connection string", "hubname");

<span data-ttu-id="a41a9-126">**Získáte Centrum oznámení:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-126">**Get Notification Hub:**</span></span>

    hub = namespaceManager.getNotificationHub("hubname");

<span data-ttu-id="a41a9-127">**Aktualizujte Centrum oznámení:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-127">**Update Notification Hub:**</span></span>

    hub.setMpnsCredential(new MpnsCredential("mpnscert", "mpnskey"));
    hub = namespaceManager.updateNotificationHub(hub);

<span data-ttu-id="a41a9-128">**Odstraňte Centrum oznámení:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-128">**Delete Notification Hub:**</span></span>

    namespaceManager.deleteNotificationHub("hubname");

### <a name="registration-cruds"></a><span data-ttu-id="a41a9-129">Registrace CRUDs</span><span class="sxs-lookup"><span data-stu-id="a41a9-129">Registration CRUDs</span></span>
<span data-ttu-id="a41a9-130">**Vytvoření centra oznámení klienta:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-130">**Create a Notification Hub client:**</span></span>

    hub = new NotificationHub("connection string", "hubname");

<span data-ttu-id="a41a9-131">**Vytvořte registrace systému Windows:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-131">**Create Windows registration:**</span></span>

    WindowsRegistration reg = new WindowsRegistration(new URI(CHANNELURI));
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");    
    hub.createRegistration(reg);

<span data-ttu-id="a41a9-132">**Vytvořte registrace iOS:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-132">**Create iOS registration:**</span></span>

    AppleRegistration reg = new AppleRegistration(DEVICETOKEN);
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");
    hub.createRegistration(reg);

<span data-ttu-id="a41a9-133">Podobně můžete vytvořit registrace pro Android (GCM), Windows Phone (MPNS) a Kindle ještě efektivněji (ADM).</span><span class="sxs-lookup"><span data-stu-id="a41a9-133">Similarly you can create registrations for Android (GCM), Windows Phone (MPNS), and Kindle Fire (ADM).</span></span>

<span data-ttu-id="a41a9-134">**Vytvořte šablonu registrace:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-134">**Create template registrations:**</span></span>

    WindowsTemplateRegistration reg = new WindowsTemplateRegistration(new URI(CHANNELURI), WNSBODYTEMPLATE);
    reg.getHeaders().put("X-WNS-Type", "wns/toast");
    hub.createRegistration(reg);

<span data-ttu-id="a41a9-135">**Vytvořit registrace pomocí vytvořte registrationid + upsert vzor**</span><span class="sxs-lookup"><span data-stu-id="a41a9-135">**Create registrations using create registrationid + upsert pattern**</span></span>

<span data-ttu-id="a41a9-136">Odebere duplicitní položky z důvodu ztráty tooany odpovědi při ukládání ID registrace na hello zařízení:</span><span class="sxs-lookup"><span data-stu-id="a41a9-136">Removes duplicates due tooany lost responses if storing registration ids on hello device:</span></span>

    String id = hub.createRegistrationId();
    WindowsRegistration reg = new WindowsRegistration(id, new URI(CHANNELURI));
    hub.upsertRegistration(reg);

<span data-ttu-id="a41a9-137">**Aktualizujte registrace:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-137">**Update registrations:**</span></span>

    hub.updateRegistration(reg);

<span data-ttu-id="a41a9-138">**Odstraňte registrace:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-138">**Delete registrations:**</span></span>

    hub.deleteRegistration(regid);

<span data-ttu-id="a41a9-139">**Registrace dotazu:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-139">**Query registrations:**</span></span>

* <span data-ttu-id="a41a9-140">**Získáte jeden registrace:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-140">**Get single registration:**</span></span>
  
    <span data-ttu-id="a41a9-141">hub.getRegistration(regid);</span><span class="sxs-lookup"><span data-stu-id="a41a9-141">hub.getRegistration(regid);</span></span>
* <span data-ttu-id="a41a9-142">**V centru získáte všechny registrace:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-142">**Get all registrations in hub:**</span></span>
  
    <span data-ttu-id="a41a9-143">hub.getRegistrations();</span><span class="sxs-lookup"><span data-stu-id="a41a9-143">hub.getRegistrations();</span></span>
* <span data-ttu-id="a41a9-144">**Získání registrace pomocí značky:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-144">**Get registrations with tag:**</span></span>
  
    <span data-ttu-id="a41a9-145">hub.getRegistrationsByTag("myTag");</span><span class="sxs-lookup"><span data-stu-id="a41a9-145">hub.getRegistrationsByTag("myTag");</span></span>
* <span data-ttu-id="a41a9-146">**Získáte registrací kanál:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-146">**Get registrations by channel:**</span></span>
  
    <span data-ttu-id="a41a9-147">hub.getRegistrationsByChannel("devicetoken");</span><span class="sxs-lookup"><span data-stu-id="a41a9-147">hub.getRegistrationsByChannel("devicetoken");</span></span>

<span data-ttu-id="a41a9-148">Všechny dotazy na kolekce podporují $top a pokračování tokenů.</span><span class="sxs-lookup"><span data-stu-id="a41a9-148">All collection queries support $top and continuation tokens.</span></span>

### <a name="installation-api-usage"></a><span data-ttu-id="a41a9-149">Instalace rozhraní API využití</span><span class="sxs-lookup"><span data-stu-id="a41a9-149">Installation API usage</span></span>
<span data-ttu-id="a41a9-150">Instalace rozhraní API je alternativní mechanismus pro správu registrace.</span><span class="sxs-lookup"><span data-stu-id="a41a9-150">Installation API is an alternative mechanism for registration management.</span></span> <span data-ttu-id="a41a9-151">Místo zachování více registrace, které není triviální a může se snadno provést neoprávněně nebo neefektivnímu, je nyní možné toouse objekt JEDNOTNOU instalaci.</span><span class="sxs-lookup"><span data-stu-id="a41a9-151">Instead of maintaining multiple registrations which is not trivial and may be easily done wrongly or inefficiently, it is now possible toouse a SINGLE Installation object.</span></span> <span data-ttu-id="a41a9-152">Instalace obsahuje všechno, co potřebujete: push kanál (token zařízení), značky, šablony, sekundární dlaždice (pro WNS a APNS).</span><span class="sxs-lookup"><span data-stu-id="a41a9-152">Installation contains everything you need: push channel (device token), tags, templates, secondary tiles (for WNS and APNS).</span></span> <span data-ttu-id="a41a9-153">Nepotřebujete toocall hello služby tooget Id – právě generovat identifikátor GUID nebo jakýkoli jiný identifikátor, uložte jej na zařízení a odeslat back-end tooyour společně s nabízené kanál (token zařízení).</span><span class="sxs-lookup"><span data-stu-id="a41a9-153">You don't need toocall hello service tooget Id anymore - just generate GUID or any other identifier, keep it on device and send tooyour backend together with push channel (device token).</span></span> <span data-ttu-id="a41a9-154">Na back-end hello použijte pouze v jediném volání: CreateOrUpdateInstallation, je plně idempotent, takže myslíte, že volné tooretry v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="a41a9-154">On hello backend you should only do a single call: CreateOrUpdateInstallation, it is fully idempotent, so feel free tooretry if needed.</span></span>

<span data-ttu-id="a41a9-155">Jako příklad ještě efektivněji Kindle Amazon vypadat třeba takto:</span><span class="sxs-lookup"><span data-stu-id="a41a9-155">As example for Amazon Kindle Fire it looks like this:</span></span>

    Installation installation = new Installation("installation-id", NotificationPlatform.Adm, "adm-push-channel");
    hub.createOrUpdateInstallation(installation);

<span data-ttu-id="a41a9-156">Pokud chcete, aby tooupdate ho:</span><span class="sxs-lookup"><span data-stu-id="a41a9-156">If you want tooupdate it:</span></span> 

    installation.addTag("foo");
    installation.addTemplate("template1", new InstallationTemplate("{\"data\":{\"key1\":\"$(value1)\"}}","tag-for-template1"));
    installation.addTemplate("template2", new InstallationTemplate("{\"data\":{\"key2\":\"$(value2)\"}}","tag-for-template2"));
    hub.createOrUpdateInstallation(installation);

<span data-ttu-id="a41a9-157">Pro pokročilé scénáře máme schopností částečné aktualizace, která umožňuje toomodify pouze konkrétní vlastnosti objektu hello instalace.</span><span class="sxs-lookup"><span data-stu-id="a41a9-157">For advanced scenarios we have partial update capability which allows toomodify only particular properties of hello installation object.</span></span> <span data-ttu-id="a41a9-158">V podstatě částečné aktualizace je podmnožinou JSON oprava operace, které můžete spustit instalaci objektu.</span><span class="sxs-lookup"><span data-stu-id="a41a9-158">Basically partial update is subset of JSON Patch operations you can run against Installation object.</span></span>

    PartialUpdateOperation addChannel = new PartialUpdateOperation(UpdateOperationType.Add, "/pushChannel", "adm-push-channel2");
    PartialUpdateOperation addTag = new PartialUpdateOperation(UpdateOperationType.Add, "/tags", "bar");
    PartialUpdateOperation replaceTemplate = new PartialUpdateOperation(UpdateOperationType.Replace, "/templates/template1", new InstallationTemplate("{\"data\":{\"key3\":\"$(value3)\"}}","tag-for-template1")).toJson());
    hub.patchInstallation("installation-id", addChannel, addTag, replaceTemplate);

<span data-ttu-id="a41a9-159">Odstraňte instalace:</span><span class="sxs-lookup"><span data-stu-id="a41a9-159">Delete Installation:</span></span>

    hub.deleteInstallation(installation.getInstallationId());

<span data-ttu-id="a41a9-160">CreateOrUpdate, Patch a Delete jsou nakonec byl konzistentní se Get.</span><span class="sxs-lookup"><span data-stu-id="a41a9-160">CreateOrUpdate, Patch and Delete are eventually consistent with Get.</span></span> <span data-ttu-id="a41a9-161">Požadovaná operace právě přejde fronty toohello systému během volání hello a budou spuštěny v pozadí.</span><span class="sxs-lookup"><span data-stu-id="a41a9-161">Your requested operation just goes toohello system queue during hello call and will be executed in background.</span></span> <span data-ttu-id="a41a9-162">Všimněte si, že Get není určen pro scénář hlavní runtime, ale jenom pro ladění a odstraňování potíží se úzce omezuje službou hello.</span><span class="sxs-lookup"><span data-stu-id="a41a9-162">Note that Get is not designed for main runtime scenario but just for debug and troubleshooting purposes, it is tightly throttled by hello service.</span></span>

<span data-ttu-id="a41a9-163">Postup odeslání pro instalace je hello stejné jako pro registrace.</span><span class="sxs-lookup"><span data-stu-id="a41a9-163">Send flow for Installations is hello same as for Registrations.</span></span> <span data-ttu-id="a41a9-164">Zavedli jsme právě možnost tootarget oznámení toohello konkrétní instalaci - jenom použití značky "InstallationId: {desired-id}".</span><span class="sxs-lookup"><span data-stu-id="a41a9-164">We've just introduced an option tootarget notification toohello particular Installation - just use tag "InstallationId:{desired-id}".</span></span> <span data-ttu-id="a41a9-165">Pro případ nad ním bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="a41a9-165">For case above it would look like this:</span></span>

    Notification n = Notification.createWindowsNotification("WNS body");
    hub.sendNotification(n, "InstallationId:{installation-id}");

<span data-ttu-id="a41a9-166">Pro jednu z několika šablon:</span><span class="sxs-lookup"><span data-stu-id="a41a9-166">For one of several templates:</span></span>

    Map<String, String> prop =  new HashMap<String, String>();
    prop.put("value3", "some value");
    Notification n = Notification.createTemplateNotification(prop);
    hub.sendNotification(n, "InstallationId:{installation-id} && tag-for-template1");

### <a name="schedule-notifications-available-for-standard-tier"></a><span data-ttu-id="a41a9-167">Naplánovat oznámení (k dispozici pro úroveň STANDARD)</span><span class="sxs-lookup"><span data-stu-id="a41a9-167">Schedule Notifications (available for STANDARD Tier)</span></span>
<span data-ttu-id="a41a9-168">Hello stejné jako regulární send, ale jeden další parametr - scheduledTime s informacemi o tom, kdy bude doručena oznámení.</span><span class="sxs-lookup"><span data-stu-id="a41a9-168">hello same as regular send but with one additional parameter - scheduledTime which says when notification should be delivered.</span></span> <span data-ttu-id="a41a9-169">Služba přijímá libovolného bodu čas mezi nyní + 5 minut a nyní + 7 dní.</span><span class="sxs-lookup"><span data-stu-id="a41a9-169">Service accepts any point of time between now + 5 minutes and now + 7 days.</span></span>

<span data-ttu-id="a41a9-170">**Plán Windows nativní oznámení:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-170">**Schedule a Windows native notification:**</span></span>

    Calendar c = Calendar.getInstance();
    c.add(Calendar.DATE, 1);    
    Notification n = Notification.createWindowsNotification("WNS body");
    hub.scheduleNotification(n, c.getTime());

### <a name="importexport-available-for-standard-tier"></a><span data-ttu-id="a41a9-171">Import a Export (k dispozici pro úroveň STANDARD)</span><span class="sxs-lookup"><span data-stu-id="a41a9-171">Import/Export (available for STANDARD Tier)</span></span>
<span data-ttu-id="a41a9-172">Někdy je požadovaná tooperform hromadné operace s registrací.</span><span class="sxs-lookup"><span data-stu-id="a41a9-172">Sometimes it is required tooperform bulk operation against registrations.</span></span> <span data-ttu-id="a41a9-173">Obvykle je pro integraci v rámci jiného systému nebo jenom masivní opravu toosay aktualizace hello značky.</span><span class="sxs-lookup"><span data-stu-id="a41a9-173">Usually it is for integration with another system or just a massive fix toosay update hello tags.</span></span> <span data-ttu-id="a41a9-174">Důrazně není doporučuje toouse Get nebo aktualizovat toku Pokud jsme mluvíme o tisíce registrace.</span><span class="sxs-lookup"><span data-stu-id="a41a9-174">It is strongly not recommended toouse Get/Update flow if we are talking about thousands of registrations.</span></span> <span data-ttu-id="a41a9-175">Funkce importu a exportu je navrženou toocover hello scénář.</span><span class="sxs-lookup"><span data-stu-id="a41a9-175">Import/Export capability is designed toocover hello scenario.</span></span> <span data-ttu-id="a41a9-176">V podstatě zadáte kontejner objektů blob toosome přístup v rámci účtu úložiště jako zdroj příchozích dat a umístění pro výstup.</span><span class="sxs-lookup"><span data-stu-id="a41a9-176">Basically you provide an access toosome blob container under your storage account as a source of incoming data and location for output.</span></span>

<span data-ttu-id="a41a9-177">**Odeslání úlohy exportu:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-177">**Submit export job:**</span></span>

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ExportRegistrations);
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);


<span data-ttu-id="a41a9-178">**Odeslání úlohy importu:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-178">**Submit import job:**</span></span>

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ImportCreateRegistrations);
    job.setImportFileUri("input file uri with SAS signature");
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);

<span data-ttu-id="a41a9-179">**Počkejte, dokud se provádí úlohy:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-179">**Wait until job is done:**</span></span>

    while(true){
        Thread.sleep(1000);
        job = hub.getNotificationHubJob(job.getJobId());
        if(job.getJobStatus() == NotificationHubJobStatus.Completed)
            break;
    }       

<span data-ttu-id="a41a9-180">**Získání všech úloh:**</span><span class="sxs-lookup"><span data-stu-id="a41a9-180">**Get all jobs:**</span></span>

    List<NotificationHubJob> jobs = hub.getAllNotificationHubJobs();

<span data-ttu-id="a41a9-181">**Identifikátor URI s SAS podpis:** Toto je adresa URL hello některé objektu blob souboru nebo kontejneru objektů blob a sadu parametrů, jako jsou oprávnění a čas vypršení platnosti a podpis všechny tyto věci, které jsou vytvořené pomocí klíče SAS účtu.</span><span class="sxs-lookup"><span data-stu-id="a41a9-181">**URI with SAS signature:** This is hello URL of some blob file or blob container plus set of parameters like permissions and expiration time plus signature of all these things made using account's SAS key.</span></span> <span data-ttu-id="a41a9-182">Azure SDK pro jazyk Java úložiště má bohaté možnosti, včetně vytváření takové druh identifikátorů URI.</span><span class="sxs-lookup"><span data-stu-id="a41a9-182">Azure Storage Java SDK has rich capabilities including creation of such kind of URIs.</span></span> <span data-ttu-id="a41a9-183">Jako alternativní jednoduché může trvat podívejte se na ImportExportE2E třídy testu (z umístění githubu hello), který má velmi základní a compact implementace podpisový algoritmus.</span><span class="sxs-lookup"><span data-stu-id="a41a9-183">As simple alternative you can take a look at ImportExportE2E test class (from hello github location) which has very basic and compact implementation of signing algorithm.</span></span>

### <a name="send-notifications"></a><span data-ttu-id="a41a9-184">Odesílání oznámení</span><span class="sxs-lookup"><span data-stu-id="a41a9-184">Send Notifications</span></span>
<span data-ttu-id="a41a9-185">objekt oznámení Hello je jednoduše text se záhlavími, některé metody nástroj pomoci při vytváření objektů hello nativní a šablony oznámení.</span><span class="sxs-lookup"><span data-stu-id="a41a9-185">hello Notification object is simply a body with headers, some utility methods help in building hello native and template notifications objects.</span></span>

* <span data-ttu-id="a41a9-186">**Windows Store a Windows Phone 8.1 (bez Silverlight)**</span><span class="sxs-lookup"><span data-stu-id="a41a9-186">**Windows Store and Windows Phone 8.1 (non-Silverlight)**</span></span>
  
        String toast = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello from Java!</text></binding></visual></toast>";
        Notification n = Notification.createWindowsNotification(toast);
        hub.sendNotification(n);
* <span data-ttu-id="a41a9-187">**iOS**</span><span class="sxs-lookup"><span data-stu-id="a41a9-187">**iOS**</span></span>
  
        String alert = "{\"aps\":{\"alert\":\"Hello from Java!\"}}";
        Notification n = Notification.createAppleNotification(alert);
        hub.sendNotification(n);
* <span data-ttu-id="a41a9-188">**Android**</span><span class="sxs-lookup"><span data-stu-id="a41a9-188">**Android**</span></span>
  
        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createGcmNotification(message);
        hub.sendNotification(n);
* <span data-ttu-id="a41a9-189">**Windows Phone 8.0 a 8.1 Silverlight**</span><span class="sxs-lookup"><span data-stu-id="a41a9-189">**Windows Phone 8.0 and 8.1 Silverlight**</span></span>
  
        String toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>Hello from Java!</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
        Notification n = Notification.createMpnsNotification(toast);
        hub.sendNotification(n);
* <span data-ttu-id="a41a9-190">**Kindle ještě efektivněji**</span><span class="sxs-lookup"><span data-stu-id="a41a9-190">**Kindle Fire**</span></span>
  
        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createAdmNotification(message);
        hub.sendNotification(n);
* <span data-ttu-id="a41a9-191">**Odeslat tooTags**</span><span class="sxs-lookup"><span data-stu-id="a41a9-191">**Send tooTags**</span></span>
  
        Set<String> tags = new HashSet<String>();
        tags.add("boo");
        tags.add("foo");
        hub.sendNotification(n, tags);
* <span data-ttu-id="a41a9-192">**Odeslat tootag výraz**</span><span class="sxs-lookup"><span data-stu-id="a41a9-192">**Send tootag expression**</span></span>       
  
        hub.sendNotification(n, "foo && ! bar");
* <span data-ttu-id="a41a9-193">**Odeslat oznámení šablony**</span><span class="sxs-lookup"><span data-stu-id="a41a9-193">**Send template notification**</span></span>
  
        Map<String, String> prop =  new HashMap<String, String>();
        prop.put("prop1", "v1");
        prop.put("prop2", "v2");
        Notification n = Notification.createTemplateNotification(prop);
        hub.sendNotification(n);

<span data-ttu-id="a41a9-194">Spuštěním kódu Java by měl nyní vytvořit oznámení, které jsou na cílovém zařízení.</span><span class="sxs-lookup"><span data-stu-id="a41a9-194">Running your Java code should now produce a notification appearing on your target device.</span></span>

## <span data-ttu-id="a41a9-195"><a name="next-steps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="a41a9-195"><a name="next-steps"></a>Next Steps</span></span>
<span data-ttu-id="a41a9-196">V tomto tématu jsme vám ukázal, jak toocreate jednoduché Java REST klienta pro centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="a41a9-196">In this topic we showed how toocreate a simple Java REST client for Notification Hubs.</span></span> <span data-ttu-id="a41a9-197">Odsud můžete:</span><span class="sxs-lookup"><span data-stu-id="a41a9-197">From here you can:</span></span>

* <span data-ttu-id="a41a9-198">Stáhnout hello úplné [sady Java SDK], který obsahuje celý kód SDK hello.</span><span class="sxs-lookup"><span data-stu-id="a41a9-198">Download hello full [Java SDK], which contains hello entire SDK code.</span></span> 
* <span data-ttu-id="a41a9-199">Přehrání s hello ukázky:</span><span class="sxs-lookup"><span data-stu-id="a41a9-199">Play with hello samples:</span></span>
  * <span data-ttu-id="a41a9-200">[Začínáme s Notification Hubs]</span><span class="sxs-lookup"><span data-stu-id="a41a9-200">[Get Started with Notification Hubs]</span></span>
  * <span data-ttu-id="a41a9-201">[Odesílání novinek]</span><span class="sxs-lookup"><span data-stu-id="a41a9-201">[Send breaking news]</span></span>
  * <span data-ttu-id="a41a9-202">[Odesílání lokalizované novinek]</span><span class="sxs-lookup"><span data-stu-id="a41a9-202">[Send localized breaking news]</span></span>
  * <span data-ttu-id="a41a9-203">[Odesílání oznámení tooauthenticated uživatelů]</span><span class="sxs-lookup"><span data-stu-id="a41a9-203">[Send notifications tooauthenticated users]</span></span>
  * <span data-ttu-id="a41a9-204">[Odesílání oznámení napříč platformami tooauthenticated uživatelů]</span><span class="sxs-lookup"><span data-stu-id="a41a9-204">[Send cross-platform notifications tooauthenticated users]</span></span>

[sady Java SDK]: https://github.com/Azure/azure-notificationhubs-java-backend
[Get started tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Začínáme s Notification Hubs]: http://www.windowsazure.com/manage/services/notification-hubs/getting-started-windows-dotnet/
[Odesílání novinek]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-dotnet/
[Odesílání lokalizované novinek]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-localized-dotnet/
[Odesílání oznámení tooauthenticated uživatelů]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users/
[Odesílání oznámení napříč platformami tooauthenticated uživatelů]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users-xplat-mobile-services/
[Maven]: http://maven.apache.org/

