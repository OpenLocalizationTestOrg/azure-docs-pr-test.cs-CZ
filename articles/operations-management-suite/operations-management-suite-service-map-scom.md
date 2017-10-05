---
title: "Integrace mapy služeb s nástrojem System Center Operations Manager | Microsoft Docs"
description: "Mapa služeb je Operations Management Suite řešení, které automaticky zjistí součásti aplikace v systémech Windows a Linux a mapuje komunikace mezi službami. Tento článek popisuje pomocí mapy služeb pro automatické vytvoření diagramy distribuované aplikace v nástroji Operations Manager."
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
ms.openlocfilehash: a7dbe54ffb4daa941c19b51ba263dd3d23b7a98b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="service-map-integration-with-system-center-operations-manager"></a><span data-ttu-id="7da6a-104">Integrace mapy služeb s nástrojem System Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="7da6a-104">Service Map integration with System Center Operations Manager</span></span>
  > [!NOTE]
  > <span data-ttu-id="7da6a-105">Tato funkce je ve verzi public preview.</span><span class="sxs-lookup"><span data-stu-id="7da6a-105">This feature is in public preview.</span></span>
  > 
  
<span data-ttu-id="7da6a-106">Mapy Operations Management Suite služby automaticky vyhledá součásti aplikace v systémech Windows a Linux a mapuje komunikace mezi službami.</span><span class="sxs-lookup"><span data-stu-id="7da6a-106">Operations Management Suite Service Map automatically discovers application components on Windows and Linux systems and maps the communication between services.</span></span> <span data-ttu-id="7da6a-107">Mapa služeb umožňuje zobrazit vaše servery způsob, jak si myslíte z nich, jako vzájemně propojena systémy, které doručují důležité služby.</span><span class="sxs-lookup"><span data-stu-id="7da6a-107">Service Map allows you to view your servers the way you think of them, as interconnected systems that deliver critical services.</span></span> <span data-ttu-id="7da6a-108">Mapy služeb zobrazí připojení mezi servery, procesy a porty mezi žádné připojení TCP architektura žádnou konfiguraci vyžaduje kromě instalaci agenta.</span><span class="sxs-lookup"><span data-stu-id="7da6a-108">Service Map shows connections between servers, processes, and ports across any TCP-connected architecture, with no configuration required besides the installation of an agent.</span></span> <span data-ttu-id="7da6a-109">Další informace najdete v tématu [mapy služeb dokumentaci](operations-management-suite-service-map.md).</span><span class="sxs-lookup"><span data-stu-id="7da6a-109">For more information, see the [Service Map documentation](operations-management-suite-service-map.md).</span></span>

<span data-ttu-id="7da6a-110">Díky této integraci mezi mapy služeb a System Center Operations Manager můžete automaticky vytvořit diagramy distribuované aplikace v nástroji Operations Manager, které jsou založeny na map dynamické závislostí v mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="7da6a-110">With this integration between Service Map and System Center Operations Manager, you can automatically create distributed application diagrams in Operations Manager that are based on the dynamic dependency maps in Service Map.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7da6a-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7da6a-111">Prerequisites</span></span>
* <span data-ttu-id="7da6a-112">Skupinu správy nástroje Operations Manager, který je Správa u sady serverů.</span><span class="sxs-lookup"><span data-stu-id="7da6a-112">An Operations Manager management group that is managing a set of servers.</span></span>
* <span data-ttu-id="7da6a-113">Prostoru Operations Management Suite s řešením mapy služby povolena.</span><span class="sxs-lookup"><span data-stu-id="7da6a-113">An Operations Management Suite workspace with the Service Map solution enabled.</span></span>
* <span data-ttu-id="7da6a-114">Sada serverů (alespoň jeden), které spravuje nástroj Operations Manager a odesílání dat do mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="7da6a-114">A set of servers (at least one) that are being managed by Operations Manager and sending data to Service Map.</span></span> <span data-ttu-id="7da6a-115">Servery se systémy Windows a Linux jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="7da6a-115">Windows and Linux servers are supported.</span></span>
* <span data-ttu-id="7da6a-116">Objekt služby s přístupem k předplatnému Azure, která je přidružena k pracovnímu prostoru služby Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="7da6a-116">A service principal with access to the Azure subscription that is associated with the Operations Management Suite workspace.</span></span> <span data-ttu-id="7da6a-117">Další informace, přejděte na [vytvoření instančního objektu](#creating-a-service-principal).</span><span class="sxs-lookup"><span data-stu-id="7da6a-117">For more information, go to [Create a service principal](#creating-a-service-principal).</span></span>

## <a name="install-the-service-map-management-pack"></a><span data-ttu-id="7da6a-118">Nainstalujte sadu management pack mapy služeb</span><span class="sxs-lookup"><span data-stu-id="7da6a-118">Install the Service Map management pack</span></span>
<span data-ttu-id="7da6a-119">Povolit integraci mezi nástrojem Operations Manager a Service Map importováním Microsoft.SystemCenter.ServiceMap komplet sady management pack (Microsoft.SystemCenter.ServiceMap.mpb).</span><span class="sxs-lookup"><span data-stu-id="7da6a-119">You enable the integration between Operations Manager and Service Map by importing the Microsoft.SystemCenter.ServiceMap management pack bundle (Microsoft.SystemCenter.ServiceMap.mpb).</span></span> <span data-ttu-id="7da6a-120">Sada obsahuje následující sady management Pack:</span><span class="sxs-lookup"><span data-stu-id="7da6a-120">The bundle contains the following management packs:</span></span>
* <span data-ttu-id="7da6a-121">Zobrazení aplikací služby Microsoft mapy</span><span class="sxs-lookup"><span data-stu-id="7da6a-121">Microsoft Service Map Application Views</span></span>
* <span data-ttu-id="7da6a-122">Mapa služeb Microsoft System Center interní</span><span class="sxs-lookup"><span data-stu-id="7da6a-122">Microsoft System Center Service Map Internal</span></span>
* <span data-ttu-id="7da6a-123">Přepsání mapy služby Microsoft System Center</span><span class="sxs-lookup"><span data-stu-id="7da6a-123">Microsoft System Center Service Map Overrides</span></span>
* <span data-ttu-id="7da6a-124">Mapa služeb Microsoft System Center</span><span class="sxs-lookup"><span data-stu-id="7da6a-124">Microsoft System Center Service Map</span></span>

## <a name="configure-the-service-map-integration"></a><span data-ttu-id="7da6a-125">Konfigurace integrace mapy služeb</span><span class="sxs-lookup"><span data-stu-id="7da6a-125">Configure the Service Map integration</span></span>
<span data-ttu-id="7da6a-126">Po instalaci mapy služeb sady pro správu, nový uzel, **mapy služeb**, se zobrazí v části **Operations Management Suite** v **správy** podokně.</span><span class="sxs-lookup"><span data-stu-id="7da6a-126">After you install the Service Map management pack, a new node, **Service Map**, is displayed under **Operations Management Suite** in the **Administration** pane.</span></span> 

<span data-ttu-id="7da6a-127">Při konfiguraci integrace mapy služeb, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="7da6a-127">To configure Service Map integration, do the following:</span></span>

1. <span data-ttu-id="7da6a-128">Chcete-li spustit Průvodce konfigurací v **přehled mapy služby** podokně klikněte na tlačítko **přidání prostoru**.</span><span class="sxs-lookup"><span data-stu-id="7da6a-128">To open the configuration wizard, in the **Service Map Overview** pane, click **Add workspace**.</span></span>  

    ![Přehled služby mapy podokno](media/oms-service-map/scom-configuration.png)

2. <span data-ttu-id="7da6a-130">V **konfigurace připojení** okno, zadejte název klienta nebo ID, ID aplikace (také označované jako uživatelské jméno nebo clientID) a heslo instanční objekt a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="7da6a-130">In the **Connection Configuration** window, enter the tenant name or ID, application ID (also known as the username or clientID), and password of the service principal, and then click **Next**.</span></span> <span data-ttu-id="7da6a-131">Další informace, přejděte na [vytvoření instančního objektu](#creating-a-service-principal).</span><span class="sxs-lookup"><span data-stu-id="7da6a-131">For more information, go to [Create a service principal](#creating-a-service-principal).</span></span>

    ![Okno Konfigurace připojení](media/oms-service-map/scom-config-spn.png)

3. <span data-ttu-id="7da6a-133">V **výběr předplatného** okno, vyberte předplatné, skupinu prostředků Azure (ten, který obsahuje pracovní prostor služby Operations Management Suite) a pracovní prostor služby Operations Management Suite a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="7da6a-133">In the **Subscription Selection** window, select the Azure subscription, Azure resource group (the one that contains the Operations Management Suite workspace), and Operations Management Suite workspace, and then click **Next**.</span></span>

    ![Pracovní prostor Operations Manager konfigurace](media/oms-service-map/scom-config-workspace.png)

4. <span data-ttu-id="7da6a-135">V **výběr skupiny počítače** okně vyberte skupiny počítačů mapy které služby chcete synchronizovat do nástroje Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="7da6a-135">In the **Machine Group Selection** window, you choose which Service Map Machine Groups you want to sync to Operations Manager.</span></span> <span data-ttu-id="7da6a-136">Klikněte na tlačítko **skupiny počítačů přidat nebo odebrat**, vyberte skupiny ze seznamu **dostupných skupin počítačů**a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="7da6a-136">Click **Add/Remove Machine Groups**, choose groups from the list of **Available Machine Groups**, and click **Add**.</span></span>  <span data-ttu-id="7da6a-137">Po dokončení výběru skupiny klikněte na **Ok** ukončíte.</span><span class="sxs-lookup"><span data-stu-id="7da6a-137">When you are finished selecting groups, click **Ok** to finish.</span></span>
    
    ![Operace skupiny počítače Configuration Manager](media/oms-service-map/scom-config-machine-groups.png)
    
5. <span data-ttu-id="7da6a-139">V **výběr serveru** okně konfiguraci skupiny serverů mapy služby se servery, které chcete synchronizovat mezi nástrojem Operations Manager a mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="7da6a-139">In the **Server Selection** window, you configure the Service Map Servers Group with the servers that you want to sync between Operations Manager and Service Map.</span></span> <span data-ttu-id="7da6a-140">Klikněte na tlačítko **přidat nebo odebrat servery**.</span><span class="sxs-lookup"><span data-stu-id="7da6a-140">Click **Add/Remove Servers**.</span></span>   
    
    <span data-ttu-id="7da6a-141">Pro integraci sestavení diagramu distribuované aplikace pro server musí být server:</span><span class="sxs-lookup"><span data-stu-id="7da6a-141">For the integration to build a distributed application diagram for a server, the server must be:</span></span>

    * <span data-ttu-id="7da6a-142">Spravován nástrojem Operations Manager</span><span class="sxs-lookup"><span data-stu-id="7da6a-142">Managed by Operations Manager</span></span>
    * <span data-ttu-id="7da6a-143">Spravuje mapy služeb</span><span class="sxs-lookup"><span data-stu-id="7da6a-143">Managed by Service Map</span></span>
    * <span data-ttu-id="7da6a-144">Uvedené ve skupině služby mapy servery</span><span class="sxs-lookup"><span data-stu-id="7da6a-144">Listed in the Service Map Servers Group</span></span>

    ![Skupinu konfigurace nástroje Operations Manager](media/oms-service-map/scom-config-group.png)

6. <span data-ttu-id="7da6a-146">Volitelné: Vyberte fond zdrojů serveru pro správu ke komunikaci s Operations Management Suite a pak klikněte na tlačítko **přidání prostoru**.</span><span class="sxs-lookup"><span data-stu-id="7da6a-146">Optional: Select the Management Server resource pool to communicate with Operations Management Suite, and then click **Add Workspace**.</span></span>

    ![Operace fondu zdrojů Configuration Manager](media/oms-service-map/scom-config-pool.png)

    <span data-ttu-id="7da6a-148">Může trvat několik minut, konfigurace a registrace pracovní prostor služby Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="7da6a-148">It might take a minute to configure and register the Operations Management Suite workspace.</span></span> <span data-ttu-id="7da6a-149">Po dokončení své konfigurace, Operations Manager zahájí první synchronizace mapy služeb z Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="7da6a-149">After it is configured, Operations Manager initiates the first Service Map sync from Operations Management Suite.</span></span>

    ![Operace fondu zdrojů Configuration Manager](media/oms-service-map/scom-config-success.png)


## <a name="monitor-service-map"></a><span data-ttu-id="7da6a-151">Monitorování mapy služeb</span><span class="sxs-lookup"><span data-stu-id="7da6a-151">Monitor Service Map</span></span>
<span data-ttu-id="7da6a-152">Až se připojí pracovní prostor služby Operations Management Suite, novou složku, mapy služeb, se zobrazí v **monitorování** podokně konzoly nástroje Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="7da6a-152">After the Operations Management Suite workspace is connected, a new folder, Service Map, is displayed in the **Monitoring** pane of the Operations Manager console.</span></span>

![V podokně monitorování nástroje Operations Manager](media/oms-service-map/scom-monitoring.png)

<span data-ttu-id="7da6a-154">Mapa služeb složka obsahuje čtyři uzly:</span><span class="sxs-lookup"><span data-stu-id="7da6a-154">The Service Map folder has four nodes:</span></span>
* <span data-ttu-id="7da6a-155">**Aktivní výstrahy**: uvádí všechny aktivní výstrahy o komunikaci mezi nástrojem Operations Manager a mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="7da6a-155">**Active Alerts**: Lists all the active alerts about the communication between Operations Manager and Service Map.</span></span>  <span data-ttu-id="7da6a-156">Všimněte si, že tyto výstrahy nejsou Operations Management Suite výstrahy se synchronizované do nástroje Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="7da6a-156">Note that these alerts are not Operations Management Suite alerts being synced to Operations Manager.</span></span> 

* <span data-ttu-id="7da6a-157">**Servery**: seznam monitorovaných serverů, které jsou nakonfigurovány pro synchronizaci z mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="7da6a-157">**Servers**: Lists the monitored servers that are configured to sync from Service Map.</span></span>

    ![V podokně monitorování servery nástroje Operations Manager](media/oms-service-map/scom-monitoring-servers.png)

* <span data-ttu-id="7da6a-159">**Počítač zobrazení závislostí skupiny**: uvádí všechny skupiny počítačů, které jsou synchronizované z mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="7da6a-159">**Machine Group Dependency Views**: Lists all machine groups that are synced from Service Map.</span></span> <span data-ttu-id="7da6a-160">Klikněte na možnost žádné skupiny k zobrazení jeho diagramu distribuované aplikace.</span><span class="sxs-lookup"><span data-stu-id="7da6a-160">You can click any group to view its distributed application diagram.</span></span>

    ![Diagram distribuovaných aplikací nástroje Operations Manager](media/oms-service-map/scom-group-dad.png)

* <span data-ttu-id="7da6a-162">**Zobrazení závislostí server**: uvádí všechny servery, které jsou synchronizované z mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="7da6a-162">**Server Dependency Views**: Lists all servers that are synced from Service Map.</span></span> <span data-ttu-id="7da6a-163">Můžete kliknout na jakýkoli server zobrazíte jeho diagramu distribuované aplikace.</span><span class="sxs-lookup"><span data-stu-id="7da6a-163">You can click any server to view its distributed application diagram.</span></span>

    ![Diagram distribuovaných aplikací nástroje Operations Manager](media/oms-service-map/scom-dad.png)

## <a name="edit-or-delete-the-workspace"></a><span data-ttu-id="7da6a-165">Upravit nebo odstranit pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="7da6a-165">Edit or delete the workspace</span></span>
<span data-ttu-id="7da6a-166">Můžete upravit nebo odstranit nakonfigurované prostoru prostřednictvím **přehled mapy služby** podokně (**správy** podokně > **Operations Management Suite**  >  **Služby mapy**).</span><span class="sxs-lookup"><span data-stu-id="7da6a-166">You can edit or delete the configured workspace through the **Service Map Overview** pane (**Administration** pane > **Operations Management Suite** > **Service Map**).</span></span> <span data-ttu-id="7da6a-167">Teď můžete konfigurovat jenom jeden pracovní prostor služby Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="7da6a-167">You can configure only one Operations Management Suite workspace for now.</span></span>

![V podokně pracovní prostor upravit služby Operations Manager](media/oms-service-map/scom-edit-workspace.png)

## <a name="configure-rules-and-overrides"></a><span data-ttu-id="7da6a-169">Konfigurace pravidla a přepsání</span><span class="sxs-lookup"><span data-stu-id="7da6a-169">Configure rules and overrides</span></span>
<span data-ttu-id="7da6a-170">Pravidlo, _Microsoft.SystemCenter.ServiceMapImport.Rule_, vytvoření pravidelně načíst informace z mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="7da6a-170">A rule, _Microsoft.SystemCenter.ServiceMapImport.Rule_, is created to periodically fetch information from Service Map.</span></span> <span data-ttu-id="7da6a-171">Chcete-li změnit časování synchronizace, můžete nakonfigurovat přepsání pravidla (**vytváření** podokně > **pravidla** > **Microsoft.SystemCenter.ServiceMapImport.Rule**) .</span><span class="sxs-lookup"><span data-stu-id="7da6a-171">To change sync timings, you can configure overrides of the rule (**Authoring** pane > **Rules** > **Microsoft.SystemCenter.ServiceMapImport.Rule**).</span></span>

![Operations Manager přepíše vlastnosti – okno](media/oms-service-map/scom-overrides.png)

* <span data-ttu-id="7da6a-173">**Povolit**: Povolit nebo zakázat automatické aktualizace.</span><span class="sxs-lookup"><span data-stu-id="7da6a-173">**Enabled**: Enable or disable automatic updates.</span></span> 
* <span data-ttu-id="7da6a-174">**IntervalMinutes**: resetování času mezi aktualizacemi.</span><span class="sxs-lookup"><span data-stu-id="7da6a-174">**IntervalMinutes**: Reset the time between updates.</span></span> <span data-ttu-id="7da6a-175">Výchozí interval je jedna hodina.</span><span class="sxs-lookup"><span data-stu-id="7da6a-175">The default interval is one hour.</span></span> <span data-ttu-id="7da6a-176">Pokud chcete synchronizovat častěji mapy serveru, můžete změnit hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7da6a-176">If you want to sync server maps more frequently, you can change the value.</span></span>
* <span data-ttu-id="7da6a-177">**TimeoutSeconds**: resetování dobu před vypršením časového limitu požadavku.</span><span class="sxs-lookup"><span data-stu-id="7da6a-177">**TimeoutSeconds**: Reset the length of time before the request times out.</span></span> 
* <span data-ttu-id="7da6a-178">**TimeWindowMinutes**: resetování časový interval pro dotazování na data.</span><span class="sxs-lookup"><span data-stu-id="7da6a-178">**TimeWindowMinutes**: Reset the time window for querying data.</span></span> <span data-ttu-id="7da6a-179">Výchozí hodnota je 60 minut okno.</span><span class="sxs-lookup"><span data-stu-id="7da6a-179">Default is a 60-minute window.</span></span> <span data-ttu-id="7da6a-180">Maximální povolená pomocí mapy služeb hodnota je 60 minut.</span><span class="sxs-lookup"><span data-stu-id="7da6a-180">The maximum value allowed by Service Map is 60 minutes.</span></span>

## <a name="known-issues-and-limitations"></a><span data-ttu-id="7da6a-181">Známé problémy a omezení</span><span class="sxs-lookup"><span data-stu-id="7da6a-181">Known issues and limitations</span></span>

<span data-ttu-id="7da6a-182">Současný návrh uvede následující problémy a omezení:</span><span class="sxs-lookup"><span data-stu-id="7da6a-182">The current design presents the following issues and limitations:</span></span>
* <span data-ttu-id="7da6a-183">Můžete připojit pouze k jedné pracovní prostor služby Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="7da6a-183">You can only connect to a single Operations Management Suite workspace.</span></span>
* <span data-ttu-id="7da6a-184">I když přidáte servery do skupin serverů mapy služby ručně pomocí **vytváření** podokně mapy pro tyto servery není synchronizovaná okamžitě.</span><span class="sxs-lookup"><span data-stu-id="7da6a-184">Although you can add servers to the Service Map Servers Group manually through the **Authoring** pane, the maps for those servers are not synced immediately.</span></span>  <span data-ttu-id="7da6a-185">Bude se synchronizovat ze služby mapy při příštím synchronizačním cyklu.</span><span class="sxs-lookup"><span data-stu-id="7da6a-185">They will be synced from Service Map during the next sync cycle.</span></span>
* <span data-ttu-id="7da6a-186">Pokud provedete změny diagramy distribuované aplikace vytvořené sady management pack, tyto změny budou přepsány pravděpodobně na příští synchronizaci s mapy služeb.</span><span class="sxs-lookup"><span data-stu-id="7da6a-186">If you make any changes to the Distributed Application Diagrams created by the management pack, those changes will likely be overwritten on the next sync with Service Map.</span></span>

## <a name="create-a-service-principal"></a><span data-ttu-id="7da6a-187">Vytvoření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="7da6a-187">Create a service principal</span></span>
<span data-ttu-id="7da6a-188">Oficiální Azure dokumentaci o vytvoření objektu služby najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="7da6a-188">For official Azure documentation about creating a service principal, see:</span></span>
* [<span data-ttu-id="7da6a-189">Vytvoření objektu služby pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="7da6a-189">Create a service principal by using PowerShell</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [<span data-ttu-id="7da6a-190">Vytvořit objekt služby pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="7da6a-190">Create a service principal by using Azure CLI</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)
* [<span data-ttu-id="7da6a-191">Vytvořit objekt služby pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7da6a-191">Create a service principal by using the Azure portal</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)

### <a name="feedback"></a><span data-ttu-id="7da6a-192">Váš názor</span><span class="sxs-lookup"><span data-stu-id="7da6a-192">Feedback</span></span>
<span data-ttu-id="7da6a-193">Máte k dispozici žádné zpětná vazba nám o mapy služeb nebo této dokumentace?</span><span class="sxs-lookup"><span data-stu-id="7da6a-193">Do you have any feedback for us about Service Map or this documentation?</span></span> <span data-ttu-id="7da6a-194">Navštivte naše [User Voice stránky](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), kde můžete navrhnout funkce nebo hlasovat o existující návrhy.</span><span class="sxs-lookup"><span data-stu-id="7da6a-194">Visit our [User Voice page](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), where you can suggest features or vote on existing suggestions.</span></span>
