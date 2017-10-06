---
title: "Mapa integrace s nástrojem System Center Operations Manager aaaService | Microsoft Docs"
description: "Mapa služeb je do řešení služby Operations Management Suite, který automaticky zjišťuje součásti aplikace v systému Windows a systémy Linux a mapy hello komunikace mezi službami. Tento článek popisuje pomocí mapy služeb tooautomatically vytváření diagramů distribuované aplikace v nástroji Operations Manager."
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: e8614a5a-9cf8-4c81-8931-896d358ad2cb
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: bwren;dairwin
ms.openlocfilehash: cff9cce2559448ec3a5fd14087b867f314716560
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-map-integration-with-system-center-operations-manager"></a><span data-ttu-id="e81d6-104">Integrace mapy služeb s nástrojem System Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="e81d6-104">Service Map integration with System Center Operations Manager</span></span>
  > [!NOTE]
  > <span data-ttu-id="e81d6-105">Tato funkce je ve verzi public preview.</span><span class="sxs-lookup"><span data-stu-id="e81d6-105">This feature is in public preview.</span></span>
  > 
  
<span data-ttu-id="e81d6-106">Mapy Operations Management Suite služby automaticky vyhledá součásti aplikace v systémech Windows a Linux a mapuje hello komunikace mezi službami.</span><span class="sxs-lookup"><span data-stu-id="e81d6-106">Operations Management Suite Service Map automatically discovers application components on Windows and Linux systems and maps hello communication between services.</span></span> <span data-ttu-id="e81d6-107">Mapa služeb umožňuje tooview způsob hello servery, jak si myslíte z nich, jako vzájemně propojena systémy, které doručují důležité služby.</span><span class="sxs-lookup"><span data-stu-id="e81d6-107">Service Map allows you tooview your servers hello way you think of them, as interconnected systems that deliver critical services.</span></span> <span data-ttu-id="e81d6-108">Mapy služeb zobrazí připojení mezi servery, procesy a porty mezi žádné připojení TCP architektura žádnou konfiguraci vyžaduje kromě hello instalace agenta.</span><span class="sxs-lookup"><span data-stu-id="e81d6-108">Service Map shows connections between servers, processes, and ports across any TCP-connected architecture, with no configuration required besides hello installation of an agent.</span></span> <span data-ttu-id="e81d6-109">Další informace najdete v tématu hello [mapy služeb dokumentaci](operations-management-suite-service-map.md).</span><span class="sxs-lookup"><span data-stu-id="e81d6-109">For more information, see hello [Service Map documentation](operations-management-suite-service-map.md).</span></span>

<span data-ttu-id="e81d6-110">Díky této integraci mezi mapy služeb a System Center Operations Manager můžete automaticky vytvořit diagramy distribuované aplikace v nástroji Operations Manager, které jsou založeny na hello map dynamické závislostí v mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="e81d6-110">With this integration between Service Map and System Center Operations Manager, you can automatically create distributed application diagrams in Operations Manager that are based on hello dynamic dependency maps in Service Map.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e81d6-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e81d6-111">Prerequisites</span></span>
* <span data-ttu-id="e81d6-112">Skupinu správy nástroje Operations Manager, který je Správa u sady serverů.</span><span class="sxs-lookup"><span data-stu-id="e81d6-112">An Operations Manager management group that is managing a set of servers.</span></span>
* <span data-ttu-id="e81d6-113">Prostoru Operations Management Suite s hello povolené žádné řešení mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="e81d6-113">An Operations Management Suite workspace with hello Service Map solution enabled.</span></span>
* <span data-ttu-id="e81d6-114">Sada serverů (alespoň jeden), které spravuje nástroj Operations Manager a odesílání dat tooService mapy.</span><span class="sxs-lookup"><span data-stu-id="e81d6-114">A set of servers (at least one) that are being managed by Operations Manager and sending data tooService Map.</span></span> <span data-ttu-id="e81d6-115">Servery se systémy Windows a Linux jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="e81d6-115">Windows and Linux servers are supported.</span></span>
* <span data-ttu-id="e81d6-116">Objekt služby s přístup toohello předplatné Azure, který je přidružený k pracovnímu prostoru služby Operations Management Suite hello.</span><span class="sxs-lookup"><span data-stu-id="e81d6-116">A service principal with access toohello Azure subscription that is associated with hello Operations Management Suite workspace.</span></span> <span data-ttu-id="e81d6-117">Další informace, přejděte příliš[vytvoření instančního objektu](#creating-a-service-principal).</span><span class="sxs-lookup"><span data-stu-id="e81d6-117">For more information, go too[Create a service principal](#creating-a-service-principal).</span></span>

## <a name="install-hello-service-map-management-pack"></a><span data-ttu-id="e81d6-118">Instalovat sadu management pack hello mapy služeb</span><span class="sxs-lookup"><span data-stu-id="e81d6-118">Install hello Service Map management pack</span></span>
<span data-ttu-id="e81d6-119">Povolíte hello integrace mezi nástrojem Operations Manager a Service Map importováním hello Microsoft.SystemCenter.ServiceMap komplet sady management pack (Microsoft.SystemCenter.ServiceMap.mpb).</span><span class="sxs-lookup"><span data-stu-id="e81d6-119">You enable hello integration between Operations Manager and Service Map by importing hello Microsoft.SystemCenter.ServiceMap management pack bundle (Microsoft.SystemCenter.ServiceMap.mpb).</span></span> <span data-ttu-id="e81d6-120">Hello sady obsahuje hello následující sady management Pack:</span><span class="sxs-lookup"><span data-stu-id="e81d6-120">hello bundle contains hello following management packs:</span></span>
* <span data-ttu-id="e81d6-121">Zobrazení aplikací služby Microsoft mapy</span><span class="sxs-lookup"><span data-stu-id="e81d6-121">Microsoft Service Map Application Views</span></span>
* <span data-ttu-id="e81d6-122">Mapa služeb Microsoft System Center interní</span><span class="sxs-lookup"><span data-stu-id="e81d6-122">Microsoft System Center Service Map Internal</span></span>
* <span data-ttu-id="e81d6-123">Přepsání mapy služby Microsoft System Center</span><span class="sxs-lookup"><span data-stu-id="e81d6-123">Microsoft System Center Service Map Overrides</span></span>
* <span data-ttu-id="e81d6-124">Mapa služeb Microsoft System Center</span><span class="sxs-lookup"><span data-stu-id="e81d6-124">Microsoft System Center Service Map</span></span>

## <a name="configure-hello-service-map-integration"></a><span data-ttu-id="e81d6-125">Konfigurace integrace mapy služeb hello</span><span class="sxs-lookup"><span data-stu-id="e81d6-125">Configure hello Service Map integration</span></span>
<span data-ttu-id="e81d6-126">Po instalaci hello mapy služeb management pack, nový uzel, **mapy služeb**, se zobrazí v části **Operations Management Suite** v hello **správy** podokně.</span><span class="sxs-lookup"><span data-stu-id="e81d6-126">After you install hello Service Map management pack, a new node, **Service Map**, is displayed under **Operations Management Suite** in hello **Administration** pane.</span></span> 

<span data-ttu-id="e81d6-127">tooconfigure integrace mapy služeb hello následující:</span><span class="sxs-lookup"><span data-stu-id="e81d6-127">tooconfigure Service Map integration, do hello following:</span></span>

1. <span data-ttu-id="e81d6-128">Průvodce konfigurací hello tooopen, v hello **přehled mapy služby** podokně klikněte na tlačítko **přidání prostoru**.</span><span class="sxs-lookup"><span data-stu-id="e81d6-128">tooopen hello configuration wizard, in hello **Service Map Overview** pane, click **Add workspace**.</span></span>  

    ![Přehled služby mapy podokno](media/oms-service-map/scom-configuration.png)

2. <span data-ttu-id="e81d6-130">V hello **konfigurace připojení** okno, zadejte název klienta hello nebo ID, ID aplikace (také označované jako uživatelské jméno hello nebo clientID) a heslo hello instanční objekt a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e81d6-130">In hello **Connection Configuration** window, enter hello tenant name or ID, application ID (also known as hello username or clientID), and password of hello service principal, and then click **Next**.</span></span> <span data-ttu-id="e81d6-131">Další informace, přejděte příliš[vytvoření instančního objektu](#creating-a-service-principal).</span><span class="sxs-lookup"><span data-stu-id="e81d6-131">For more information, go too[Create a service principal](#creating-a-service-principal).</span></span>

    ![okno Konfigurace připojení Hello](media/oms-service-map/scom-config-spn.png)

3. <span data-ttu-id="e81d6-133">V hello **výběr předplatného** okně vyberte hello předplatné, skupinu prostředků Azure (hello, která obsahuje pracovní prostor služby Operations Management Suite hello) a pracovní prostor služby Operations Management Suite a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e81d6-133">In hello **Subscription Selection** window, select hello Azure subscription, Azure resource group (hello one that contains hello Operations Management Suite workspace), and Operations Management Suite workspace, and then click **Next**.</span></span>

    ![Hello pracovní prostor služby Operations Manager konfigurace](media/oms-service-map/scom-config-workspace.png)

4. <span data-ttu-id="e81d6-135">V hello **výběr skupiny počítače** okně vyberte skupiny, které služba mapy počítače chcete toosync tooOperations Manager.</span><span class="sxs-lookup"><span data-stu-id="e81d6-135">In hello **Machine Group Selection** window, you choose which Service Map Machine Groups you want toosync tooOperations Manager.</span></span> <span data-ttu-id="e81d6-136">Klikněte na tlačítko **skupiny počítačů přidat nebo odebrat**, vyberte skupiny ze seznamu hello **dostupných skupin počítačů**a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e81d6-136">Click **Add/Remove Machine Groups**, choose groups from hello list of **Available Machine Groups**, and click **Add**.</span></span>  <span data-ttu-id="e81d6-137">Po dokončení výběru skupiny klikněte na **Ok** toofinish.</span><span class="sxs-lookup"><span data-stu-id="e81d6-137">When you are finished selecting groups, click **Ok** toofinish.</span></span>
    
    ![Hello skupiny počítače konfigurace Operations Manager](media/oms-service-map/scom-config-machine-groups.png)
    
5. <span data-ttu-id="e81d6-139">V hello **výběr serveru** okně nakonfigurovat hello skupin serverů mapy služby se hello servery, které chcete toosync mezi nástrojem Operations Manager a mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="e81d6-139">In hello **Server Selection** window, you configure hello Service Map Servers Group with hello servers that you want toosync between Operations Manager and Service Map.</span></span> <span data-ttu-id="e81d6-140">Klikněte na tlačítko **přidat nebo odebrat servery**.</span><span class="sxs-lookup"><span data-stu-id="e81d6-140">Click **Add/Remove Servers**.</span></span>   
    
    <span data-ttu-id="e81d6-141">Pro hello integrace toobuild diagramu distribuované aplikace pro server musí být hello server:</span><span class="sxs-lookup"><span data-stu-id="e81d6-141">For hello integration toobuild a distributed application diagram for a server, hello server must be:</span></span>

    * <span data-ttu-id="e81d6-142">Spravován nástrojem Operations Manager</span><span class="sxs-lookup"><span data-stu-id="e81d6-142">Managed by Operations Manager</span></span>
    * <span data-ttu-id="e81d6-143">Spravuje mapy služeb</span><span class="sxs-lookup"><span data-stu-id="e81d6-143">Managed by Service Map</span></span>
    * <span data-ttu-id="e81d6-144">Uvedené v hello skupin serverů služby mapy</span><span class="sxs-lookup"><span data-stu-id="e81d6-144">Listed in hello Service Map Servers Group</span></span>

    ![Hello skupina konfigurace nástroje Operations Manager](media/oms-service-map/scom-config-group.png)

6. <span data-ttu-id="e81d6-146">Volitelné: Vyberte hello serveru pro správu prostředků fondu toocommunicate s Operations Management Suite a pak klikněte na tlačítko **přidání prostoru**.</span><span class="sxs-lookup"><span data-stu-id="e81d6-146">Optional: Select hello Management Server resource pool toocommunicate with Operations Management Suite, and then click **Add Workspace**.</span></span>

    ![Hello fond zdrojů konfigurace Operations Manager](media/oms-service-map/scom-config-pool.png)

    <span data-ttu-id="e81d6-148">Ho může trvat minutu tooconfigure a zaregistrujte hello prostoru služby Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="e81d6-148">It might take a minute tooconfigure and register hello Operations Management Suite workspace.</span></span> <span data-ttu-id="e81d6-149">Po dokončení své konfigurace, Operations Manager inicializuje hello první mapy služeb synchronizace ze služby Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="e81d6-149">After it is configured, Operations Manager initiates hello first Service Map sync from Operations Management Suite.</span></span>

    ![Hello fond zdrojů konfigurace Operations Manager](media/oms-service-map/scom-config-success.png)


## <a name="monitor-service-map"></a><span data-ttu-id="e81d6-151">Monitorování mapy služeb</span><span class="sxs-lookup"><span data-stu-id="e81d6-151">Monitor Service Map</span></span>
<span data-ttu-id="e81d6-152">Až se pracovní prostor služby Operations Management Suite hello připojí, novou složku, mapy služeb, se zobrazí v hello **monitorování** podokně konzoly nástroje Operations Manager hello.</span><span class="sxs-lookup"><span data-stu-id="e81d6-152">After hello Operations Management Suite workspace is connected, a new folder, Service Map, is displayed in hello **Monitoring** pane of hello Operations Manager console.</span></span>

![Monitorování nástroje Operations Manager podokně Hello](media/oms-service-map/scom-monitoring.png)

<span data-ttu-id="e81d6-154">Hello mapy služeb složka obsahuje čtyři uzly:</span><span class="sxs-lookup"><span data-stu-id="e81d6-154">hello Service Map folder has four nodes:</span></span>
* <span data-ttu-id="e81d6-155">**Aktivní výstrahy**: uvádí všechny aktivní výstrahy hello o hello komunikaci mezi nástrojem Operations Manager a mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="e81d6-155">**Active Alerts**: Lists all hello active alerts about hello communication between Operations Manager and Service Map.</span></span>  <span data-ttu-id="e81d6-156">Všimněte si, že tyto výstrahy nejsou výstrah Operations Management Suite se synchronizoval tooOperations správce.</span><span class="sxs-lookup"><span data-stu-id="e81d6-156">Note that these alerts are not Operations Management Suite alerts being synced tooOperations Manager.</span></span> 

* <span data-ttu-id="e81d6-157">**Servery**: seznamy hello monitorované servery, které jsou nakonfigurované toosync z mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="e81d6-157">**Servers**: Lists hello monitored servers that are configured toosync from Service Map.</span></span>

    ![Podokno Hello servery monitorování nástroje Operations Manager](media/oms-service-map/scom-monitoring-servers.png)

* <span data-ttu-id="e81d6-159">**Počítač zobrazení závislostí skupiny**: uvádí všechny skupiny počítačů, které jsou synchronizované z mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="e81d6-159">**Machine Group Dependency Views**: Lists all machine groups that are synced from Service Map.</span></span> <span data-ttu-id="e81d6-160">Můžete kliknout na všechny skupiny tooview jeho diagramu distribuované aplikace.</span><span class="sxs-lookup"><span data-stu-id="e81d6-160">You can click any group tooview its distributed application diagram.</span></span>

    ![diagram distribuované aplikace Hello nástroje Operations Manager](media/oms-service-map/scom-group-dad.png)

* <span data-ttu-id="e81d6-162">**Zobrazení závislostí server**: uvádí všechny servery, které jsou synchronizované z mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="e81d6-162">**Server Dependency Views**: Lists all servers that are synced from Service Map.</span></span> <span data-ttu-id="e81d6-163">Můžete kliknout na libovolný server tooview jeho diagramu distribuované aplikace.</span><span class="sxs-lookup"><span data-stu-id="e81d6-163">You can click any server tooview its distributed application diagram.</span></span>

    ![diagram distribuované aplikace Hello nástroje Operations Manager](media/oms-service-map/scom-dad.png)

## <a name="edit-or-delete-hello-workspace"></a><span data-ttu-id="e81d6-165">Upravit nebo odstranit pracovní prostor hello</span><span class="sxs-lookup"><span data-stu-id="e81d6-165">Edit or delete hello workspace</span></span>
<span data-ttu-id="e81d6-166">Můžete upravit nebo odstranit pracovní prostor hello nakonfigurované prostřednictvím hello **přehled mapy služby** podokně (**správy** podokně > **Operations Management Suite**  >  **Služby mapy**).</span><span class="sxs-lookup"><span data-stu-id="e81d6-166">You can edit or delete hello configured workspace through hello **Service Map Overview** pane (**Administration** pane > **Operations Management Suite** > **Service Map**).</span></span> <span data-ttu-id="e81d6-167">Teď můžete konfigurovat jenom jeden pracovní prostor služby Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="e81d6-167">You can configure only one Operations Management Suite workspace for now.</span></span>

![Hello podokně pracovní prostor upravit služby Operations Manager](media/oms-service-map/scom-edit-workspace.png)

## <a name="configure-rules-and-overrides"></a><span data-ttu-id="e81d6-169">Konfigurace pravidla a přepsání</span><span class="sxs-lookup"><span data-stu-id="e81d6-169">Configure rules and overrides</span></span>
<span data-ttu-id="e81d6-170">Pravidlo, _Microsoft.SystemCenter.ServiceMapImport.Rule_, je vytvořen tooperiodically načítání informací z mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="e81d6-170">A rule, _Microsoft.SystemCenter.ServiceMapImport.Rule_, is created tooperiodically fetch information from Service Map.</span></span> <span data-ttu-id="e81d6-171">časování toochange synchronizace, můžete nakonfigurovat přepsání pravidel hello (**vytváření** podokně > **pravidla** > **Microsoft.SystemCenter.ServiceMapImport.Rule**).</span><span class="sxs-lookup"><span data-stu-id="e81d6-171">toochange sync timings, you can configure overrides of hello rule (**Authoring** pane > **Rules** > **Microsoft.SystemCenter.ServiceMapImport.Rule**).</span></span>

![okno vlastností Operations Manager přepíše Hello](media/oms-service-map/scom-overrides.png)

* <span data-ttu-id="e81d6-173">**Povolit**: Povolit nebo zakázat automatické aktualizace.</span><span class="sxs-lookup"><span data-stu-id="e81d6-173">**Enabled**: Enable or disable automatic updates.</span></span> 
* <span data-ttu-id="e81d6-174">**IntervalMinutes**: resetování hello čas mezi aktualizace.</span><span class="sxs-lookup"><span data-stu-id="e81d6-174">**IntervalMinutes**: Reset hello time between updates.</span></span> <span data-ttu-id="e81d6-175">Výchozí interval Hello je jedna hodina.</span><span class="sxs-lookup"><span data-stu-id="e81d6-175">hello default interval is one hour.</span></span> <span data-ttu-id="e81d6-176">Pokud chcete server mapy toosync častěji, můžete změnit hodnotu hello.</span><span class="sxs-lookup"><span data-stu-id="e81d6-176">If you want toosync server maps more frequently, you can change hello value.</span></span>
* <span data-ttu-id="e81d6-177">**TimeoutSeconds**: resetování hello délku doby, než vyprší časový limit žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="e81d6-177">**TimeoutSeconds**: Reset hello length of time before hello request times out.</span></span> 
* <span data-ttu-id="e81d6-178">**TimeWindowMinutes**: resetování hello časový interval pro dotazování na data.</span><span class="sxs-lookup"><span data-stu-id="e81d6-178">**TimeWindowMinutes**: Reset hello time window for querying data.</span></span> <span data-ttu-id="e81d6-179">Výchozí hodnota je 60 minut okno.</span><span class="sxs-lookup"><span data-stu-id="e81d6-179">Default is a 60-minute window.</span></span> <span data-ttu-id="e81d6-180">Hello povolená pomocí mapy služeb maximální hodnota je 60 minut.</span><span class="sxs-lookup"><span data-stu-id="e81d6-180">hello maximum value allowed by Service Map is 60 minutes.</span></span>

## <a name="known-issues-and-limitations"></a><span data-ttu-id="e81d6-181">Známé problémy a omezení</span><span class="sxs-lookup"><span data-stu-id="e81d6-181">Known issues and limitations</span></span>

<span data-ttu-id="e81d6-182">Hello aktuálního návrhu uvede hello následující problémy a omezení:</span><span class="sxs-lookup"><span data-stu-id="e81d6-182">hello current design presents hello following issues and limitations:</span></span>
* <span data-ttu-id="e81d6-183">Budete moct připojit jen tooa jeden pracovní prostor služby Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="e81d6-183">You can only connect tooa single Operations Management Suite workspace.</span></span>
* <span data-ttu-id="e81d6-184">I když můžete přidat servery toohello skupin serverů mapy služby ručně prostřednictvím hello **vytváření** podokně hello mapy pro tyto servery není synchronizovaná okamžitě.</span><span class="sxs-lookup"><span data-stu-id="e81d6-184">Although you can add servers toohello Service Map Servers Group manually through hello **Authoring** pane, hello maps for those servers are not synced immediately.</span></span>  <span data-ttu-id="e81d6-185">Bude se synchronizovat ze služby mapy při příštím synchronizačním cyklu hello.</span><span class="sxs-lookup"><span data-stu-id="e81d6-185">They will be synced from Service Map during hello next sync cycle.</span></span>
* <span data-ttu-id="e81d6-186">Pokud provedete změny toohello distribuované aplikace diagramy vytvořené hello sady management pack, tyto změny budou přepsány pravděpodobně na příští synchronizaci hello pomocí mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="e81d6-186">If you make any changes toohello Distributed Application Diagrams created by hello management pack, those changes will likely be overwritten on hello next sync with Service Map.</span></span>

## <a name="create-a-service-principal"></a><span data-ttu-id="e81d6-187">Vytvoření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="e81d6-187">Create a service principal</span></span>
<span data-ttu-id="e81d6-188">Oficiální Azure dokumentaci o vytvoření objektu služby najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="e81d6-188">For official Azure documentation about creating a service principal, see:</span></span>
* [<span data-ttu-id="e81d6-189">Vytvoření objektu služby pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e81d6-189">Create a service principal by using PowerShell</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [<span data-ttu-id="e81d6-190">Vytvořit objekt služby pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="e81d6-190">Create a service principal by using Azure CLI</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)
* [<span data-ttu-id="e81d6-191">Vytvořit objekt služby s použitím hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e81d6-191">Create a service principal by using hello Azure portal</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)

### <a name="feedback"></a><span data-ttu-id="e81d6-192">Váš názor</span><span class="sxs-lookup"><span data-stu-id="e81d6-192">Feedback</span></span>
<span data-ttu-id="e81d6-193">Máte k dispozici žádné zpětná vazba nám o mapy služeb nebo této dokumentace?</span><span class="sxs-lookup"><span data-stu-id="e81d6-193">Do you have any feedback for us about Service Map or this documentation?</span></span> <span data-ttu-id="e81d6-194">Navštivte naše [User Voice stránky](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), kde můžete navrhnout funkce nebo hlasovat o existující návrhy.</span><span class="sxs-lookup"><span data-stu-id="e81d6-194">Visit our [User Voice page](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), where you can suggest features or vote on existing suggestions.</span></span>
