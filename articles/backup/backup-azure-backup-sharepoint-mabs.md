---
title: "Použít server Azure Backup pro zálohování farmy služby SharePoint do Azure | Microsoft Docs"
description: "Zálohování a obnovení dat služby SharePoint pomocí serveru Azure Backup. Tento článek obsahuje informace pro konfiguraci farmy služby SharePoint tak, že požadovaná data se uloží v Azure. Chráněná data služby SharePoint můžete obnovit z disku nebo z Azure."
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
ms.assetid: 34ba87a4-91f1-4054-a4a1-272af1e15496
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: 3ed000affd326eb1bd7c99773ec021ad6e03cc3b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-a-sharepoint-farm-to-azure"></a><span data-ttu-id="daad5-105">Zálohování sharepointové farmy do Azure</span><span class="sxs-lookup"><span data-stu-id="daad5-105">Back up a SharePoint farm to Azure</span></span>
<span data-ttu-id="daad5-106">Zálohujete farmy služby SharePoint do služby Microsoft Azure pomocí služby Microsoft Azure Backup Server (MABS) v mnohem stejným způsobem, který zálohujete jiných zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="daad5-106">You back up a SharePoint farm to Microsoft Azure by using Microsoft Azure Backup Server (MABS) in much the same way that you back up other data sources.</span></span> <span data-ttu-id="daad5-107">Azure Backup poskytuje flexibilitu při plán zálohování k vytvoření denní, týdenní, měsíční nebo roční zálohu odkazuje a poskytuje možnosti zásad uchovávání informací pro různé body zálohy.</span><span class="sxs-lookup"><span data-stu-id="daad5-107">Azure Backup provides flexibility in the backup schedule to create daily, weekly, monthly, or yearly backup points and gives you retention policy options for various backup points.</span></span> <span data-ttu-id="daad5-108">Poskytuje taky možnost k uložení kopie místního disku pro rychlé cíle doba obnovení (RTO) a slouží k uložení kopie do Azure pro ekonomické, dlouhodobé uchovávání.</span><span class="sxs-lookup"><span data-stu-id="daad5-108">It also provides the capability to store local disk copies for quick recovery-time objectives (RTO) and to store copies to Azure for economical, long-term retention.</span></span>

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a><span data-ttu-id="daad5-109">Podporované verze služby SharePoint a související scénáře ochrany</span><span class="sxs-lookup"><span data-stu-id="daad5-109">SharePoint supported versions and related protection scenarios</span></span>
<span data-ttu-id="daad5-110">Zálohování Azure pro DPM podporuje následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="daad5-110">Azure Backup for DPM supports the following scenarios:</span></span>

| <span data-ttu-id="daad5-111">Úloha</span><span class="sxs-lookup"><span data-stu-id="daad5-111">Workload</span></span> | <span data-ttu-id="daad5-112">Verze</span><span class="sxs-lookup"><span data-stu-id="daad5-112">Version</span></span> | <span data-ttu-id="daad5-113">Nasazení služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="daad5-113">SharePoint deployment</span></span> | <span data-ttu-id="daad5-114">Ochrana a obnovení</span><span class="sxs-lookup"><span data-stu-id="daad5-114">Protection and recovery</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="daad5-115">SharePoint</span><span class="sxs-lookup"><span data-stu-id="daad5-115">SharePoint</span></span> |<span data-ttu-id="daad5-116">SharePoint 2013, SharePoint 2010 a SharePoint 2007 SharePoint 3.0</span><span class="sxs-lookup"><span data-stu-id="daad5-116">SharePoint 2013, SharePoint 2010, SharePoint 2007, SharePoint 3.0</span></span> |<span data-ttu-id="daad5-117">SharePoint nasazená jako fyzický server nebo virtuální počítač technologie Hyper-V nebo VMware</span><span class="sxs-lookup"><span data-stu-id="daad5-117">SharePoint deployed as a physical server or Hyper-V/VMware virtual machine</span></span> <br> -------------- <br> <span data-ttu-id="daad5-118">Technologie AlwaysOn serveru SQL</span><span class="sxs-lookup"><span data-stu-id="daad5-118">SQL AlwaysOn</span></span> | <span data-ttu-id="daad5-119">Ochrana farmy služby SharePoint možnosti obnovení: farma obnovení, databáze a soubor nebo položka seznamu z bodů obnovení disku.</span><span class="sxs-lookup"><span data-stu-id="daad5-119">Protect SharePoint Farm recovery options: Recovery farm, database, and file or list item from disk recovery points.</span></span>  <span data-ttu-id="daad5-120">Obnovení z bodů obnovení Azure farmy a databáze.</span><span class="sxs-lookup"><span data-stu-id="daad5-120">Farm and database recovery from Azure recovery points.</span></span> |

## <a name="before-you-start"></a><span data-ttu-id="daad5-121">Než začnete</span><span class="sxs-lookup"><span data-stu-id="daad5-121">Before you start</span></span>
<span data-ttu-id="daad5-122">Existuje několik věcí, které je potřeba potvrdit před Zálohování farmy služby SharePoint do Azure.</span><span class="sxs-lookup"><span data-stu-id="daad5-122">There are a few things you need to confirm before you back up a SharePoint farm to Azure.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="daad5-123">Požadavky</span><span class="sxs-lookup"><span data-stu-id="daad5-123">Prerequisites</span></span>
<span data-ttu-id="daad5-124">Než budete pokračovat, ujistěte se, že máte [nainstalované a připravené serveru Azure Backup](backup-azure-microsoft-azure-backup.md) chránit úlohy.</span><span class="sxs-lookup"><span data-stu-id="daad5-124">Before you proceed, make sure that you have [installed and prepared the Azure Backup Server](backup-azure-microsoft-azure-backup.md) to protect workloads.</span></span>

### <a name="protection-agent"></a><span data-ttu-id="daad5-125">Agent ochrany</span><span class="sxs-lookup"><span data-stu-id="daad5-125">Protection agent</span></span>
<span data-ttu-id="daad5-126">Na serveru, na kterém běží SharePoint, serverech se systémem SQL Server a všechny ostatní servery, které jsou součástí farmy služby SharePoint musí být nainstalován agent ochrany.</span><span class="sxs-lookup"><span data-stu-id="daad5-126">The Protection agent must be installed on the server that's running SharePoint, the servers that are running SQL Server, and all other servers that are part of the SharePoint farm.</span></span> <span data-ttu-id="daad5-127">Další informace o tom, jak nastavit agenta ochrany najdete v tématu [instalace agenta ochrany](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).</span><span class="sxs-lookup"><span data-stu-id="daad5-127">For more information about how to set up the protection agent, see [Setup Protection Agent](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).</span></span>  <span data-ttu-id="daad5-128">Jedinou výjimkou je, že instalujete agenta pouze na jednom webovém serveru front-end (WFE).</span><span class="sxs-lookup"><span data-stu-id="daad5-128">The one exception is that you install the agent only on a single web front end (WFE) server.</span></span> <span data-ttu-id="daad5-129">Aplikace DPM vyžaduje agenta na jednom serveru WFE pouze sloužit jako vstupní bod pro ochranu.</span><span class="sxs-lookup"><span data-stu-id="daad5-129">DPM needs the agent on one WFE server only to serve as the entry point for protection.</span></span>

### <a name="sharepoint-farm"></a><span data-ttu-id="daad5-130">Farmy služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="daad5-130">SharePoint farm</span></span>
<span data-ttu-id="daad5-131">Pro každých 10 milionů položek ve farmě musí být alespoň 2 GB místa na svazku, kde je umístěna složka MABS.</span><span class="sxs-lookup"><span data-stu-id="daad5-131">For every 10 million items in the farm, there must be at least 2 GB of space on the volume where the MABS folder is located.</span></span> <span data-ttu-id="daad5-132">Tento prostor je nezbytné pro generování katalogu.</span><span class="sxs-lookup"><span data-stu-id="daad5-132">This space is required for catalog generation.</span></span> <span data-ttu-id="daad5-133">Pro MABS obnovit konkrétní položky (kolekce webů, weby, seznamy, knihovny dokumentů, složky, jednotlivé dokumenty a položky seznamu) generování katalogu vytvoří seznam adres URL, které jsou obsaženy v jednotlivých databázích obsahu.</span><span class="sxs-lookup"><span data-stu-id="daad5-133">For MABS to recover specific items (site collections, sites, lists, document libraries, folders, individual documents, and list items), catalog generation creates a list of the URLs that are contained within each content database.</span></span> <span data-ttu-id="daad5-134">Seznam adres URL můžete zobrazit v podokně obnovitelných položek **obnovení** oblasti konzoly pro správu MABS úlohy.</span><span class="sxs-lookup"><span data-stu-id="daad5-134">You can view the list of URLs in the recoverable item pane in the **Recovery** task area of MABS Administrator Console.</span></span>

### <a name="sql-server"></a><span data-ttu-id="daad5-135">SQL Server</span><span class="sxs-lookup"><span data-stu-id="daad5-135">SQL Server</span></span>
<span data-ttu-id="daad5-136">MABS běží pod účtem LocalSystem.</span><span class="sxs-lookup"><span data-stu-id="daad5-136">MABS runs as a LocalSystem account.</span></span> <span data-ttu-id="daad5-137">Zálohování databází systému SQL Server, musí MABS oprávnění sysadmin na tento účet pro server, který se systémem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="daad5-137">To back up SQL Server databases, MABS needs sysadmin privileges on that account for the server that's running SQL Server.</span></span> <span data-ttu-id="daad5-138">Nastavte NT AUTHORITY\SYSTEM na *sysadmin* na serveru, který je spuštěn SQL Server předtím, než ho zálohovat.</span><span class="sxs-lookup"><span data-stu-id="daad5-138">Set NT AUTHORITY\SYSTEM to *sysadmin* on the server that's running SQL Server before you back it up.</span></span>

<span data-ttu-id="daad5-139">Pokud farmy služby SharePoint databáze systému SQL Server, které jsou nakonfigurovány s aliasy systému SQL Server, nainstalujte komponenty klienta systému SQL Server na front-end webovém serveru, který bude chránit MABS.</span><span class="sxs-lookup"><span data-stu-id="daad5-139">If the SharePoint farm has SQL Server databases that are configured with SQL Server aliases, install the SQL Server client components on the front-end Web server that MABS will protect.</span></span>

### <a name="sharepoint-server"></a><span data-ttu-id="daad5-140">SharePoint Server</span><span class="sxs-lookup"><span data-stu-id="daad5-140">SharePoint Server</span></span>
<span data-ttu-id="daad5-141">Při výkonu závislá na mnoha faktorech, jako je například velikost farmy služby SharePoint, jako obecné pokyny jeden MABS můžete chránit 25 TB farmu služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="daad5-141">While performance depends on many factors such as size of SharePoint farm, as general guidance one MABS can protect a 25 TB SharePoint farm.</span></span>

### <a name="whats-not-supported"></a><span data-ttu-id="daad5-142">Co není podporováno</span><span class="sxs-lookup"><span data-stu-id="daad5-142">What's not supported</span></span>
* <span data-ttu-id="daad5-143">MABS, který chrání farmy služby SharePoint nechrání indexy hledání nebo databáze aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="daad5-143">MABS that protects a SharePoint farm does not protect search indexes or application service databases.</span></span> <span data-ttu-id="daad5-144">Musíte konfigurovat ochranu pro tyto databáze samostatně.</span><span class="sxs-lookup"><span data-stu-id="daad5-144">You will need to configure the protection of these databases separately.</span></span>
* <span data-ttu-id="daad5-145">MABS neposkytuje zálohování databází serveru SQL služby SharePoint, které jsou hostované na sdílených složkách škálovatelného souborového serveru (SOFS).</span><span class="sxs-lookup"><span data-stu-id="daad5-145">MABS does not provide backup of SharePoint SQL Server databases that are hosted on scale-out file server (SOFS) shares.</span></span>

## <a name="configure-sharepoint-protection"></a><span data-ttu-id="daad5-146">Konfigurace ochrany Sharepointu</span><span class="sxs-lookup"><span data-stu-id="daad5-146">Configure SharePoint protection</span></span>
<span data-ttu-id="daad5-147">Než MABS můžete použít k ochraně služby SharePoint, musíte nakonfigurovat službu SharePoint VSS Writer (WSS Writer service) pomocí **ConfigureSharePoint.exe**.</span><span class="sxs-lookup"><span data-stu-id="daad5-147">Before you can use MABS to protect SharePoint, you must configure the SharePoint VSS Writer service (WSS Writer service) by using **ConfigureSharePoint.exe**.</span></span>

<span data-ttu-id="daad5-148">Můžete najít **ConfigureSharePoint.exe** ve složce [Instalační cesta MABS] \bin na front-end webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="daad5-148">You can find **ConfigureSharePoint.exe** in the [MABS Installation Path]\bin folder on the front-end web server.</span></span> <span data-ttu-id="daad5-149">Tento nástroj poskytuje agenta ochrany s přihlašovacími údaji pro farmu služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="daad5-149">This tool provides the protection agent with the credentials for the SharePoint farm.</span></span> <span data-ttu-id="daad5-150">Spustíte ji na jeden server WFE.</span><span class="sxs-lookup"><span data-stu-id="daad5-150">You run it on a single WFE server.</span></span> <span data-ttu-id="daad5-151">Pokud máte více serverů WFE, vyberte pouze jeden, když konfigurujete skupinu ochrany.</span><span class="sxs-lookup"><span data-stu-id="daad5-151">If you have multiple WFE servers, select just one when you configure a protection group.</span></span>

### <a name="to-configure-the-sharepoint-vss-writer-service"></a><span data-ttu-id="daad5-152">Konfigurace služby SharePoint VSS Writer</span><span class="sxs-lookup"><span data-stu-id="daad5-152">To configure the SharePoint VSS Writer service</span></span>
1. <span data-ttu-id="daad5-153">Na serveru WFE, na příkazovém řádku přejděte do \bin\ [umístění instalace MABS]</span><span class="sxs-lookup"><span data-stu-id="daad5-153">On the WFE server, at a command prompt, go to [MABS installation location]\bin\\</span></span>
2. <span data-ttu-id="daad5-154">Zadejte ConfigureSharePoint - EnableSharePointProtection.</span><span class="sxs-lookup"><span data-stu-id="daad5-154">Enter ConfigureSharePoint -EnableSharePointProtection.</span></span>
3. <span data-ttu-id="daad5-155">Zadejte přihlašovací údaje správce farmy.</span><span class="sxs-lookup"><span data-stu-id="daad5-155">Enter the farm administrator credentials.</span></span> <span data-ttu-id="daad5-156">Tento účet by měl být členem místní skupiny správců na serveru WFE.</span><span class="sxs-lookup"><span data-stu-id="daad5-156">This account should be a member of the local Administrator group on the WFE server.</span></span> <span data-ttu-id="daad5-157">Pokud není správcem farmy místní správce, udělte následující oprávnění na serveru WFE:</span><span class="sxs-lookup"><span data-stu-id="daad5-157">If the farm administrator isn’t a local admin grant the following permissions on the WFE server:</span></span>
   * <span data-ttu-id="daad5-158">Udělte skupině WSS_Admin_WPG úplnou kontrolu ke složce aplikace DPM (% Program Files%\Microsoft Azure Backup\DPM).</span><span class="sxs-lookup"><span data-stu-id="daad5-158">Grant the WSS_Admin_WPG group full control to the DPM folder (%Program Files%\Microsoft Azure Backup\DPM).</span></span>
   * <span data-ttu-id="daad5-159">Udělte oprávnění ke čtení skupině WSS_Admin_WPG ke klíči registru aplikace DPM (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager).</span><span class="sxs-lookup"><span data-stu-id="daad5-159">Grant the WSS_Admin_WPG group read access to the DPM Registry key (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager).</span></span>

> [!NOTE]
> <span data-ttu-id="daad5-160">Budete muset znovu spustit ConfigureSharePoint.exe vždy, když dojde ke změně v pověřeních správce farmy.</span><span class="sxs-lookup"><span data-stu-id="daad5-160">You’ll need to rerun ConfigureSharePoint.exe whenever there’s a change in the SharePoint farm administrator credentials.</span></span>
>
>

## <a name="back-up-a-sharepoint-farm-by-using-mabs"></a><span data-ttu-id="daad5-161">Zálohování farmy služby SharePoint pomocí MABS</span><span class="sxs-lookup"><span data-stu-id="daad5-161">Back up a SharePoint farm by using MABS</span></span>
<span data-ttu-id="daad5-162">Po nakonfigurování MABS a farmy služby SharePoint, jak je popsáno dříve, můžete pomocí MABS chráněný službou SharePoint.</span><span class="sxs-lookup"><span data-stu-id="daad5-162">After you have configured MABS and the SharePoint farm as explained previously, SharePoint can be protected by MABS.</span></span>

### <a name="to-protect-a-sharepoint-farm"></a><span data-ttu-id="daad5-163">Chcete-li chránit farmu služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="daad5-163">To protect a SharePoint farm</span></span>
1. <span data-ttu-id="daad5-164">Z **ochrany** kartu konzoly pro správu MABS, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="daad5-164">From the **Protection** tab of the MABS Administrator Console, click **New**.</span></span>
    <span data-ttu-id="daad5-165">![Nové karty ochrana](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)</span><span class="sxs-lookup"><span data-stu-id="daad5-165">![New Protection Tab](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)</span></span>
2. <span data-ttu-id="daad5-166">Na **vybrat typ skupiny ochrany** stránky **vytvořením nové skupiny ochrany** průvodce, vyberte **servery**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="daad5-166">On the **Select Protection Group Type** page of the **Create New Protection Group** wizard, select **Servers**, and then click **Next**.</span></span>

    ![Typ skupiny ochrany vyberte](./media/backup-azure-backup-sharepoint/select-protection-group-type.png)
3. <span data-ttu-id="daad5-168">Na **vybrat členy skupiny** obrazovky, zaškrtněte políčko pro server SharePoint, které chcete chránit a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="daad5-168">On the **Select Group Members** screen, select the check box for the SharePoint server you want to protect and click **Next**.</span></span>

    ![Vybrat členy skupiny](./media/backup-azure-backup-sharepoint/select-group-members2.png)

   > [!NOTE]
   > <span data-ttu-id="daad5-170">S nainstalovaným agentem ochrany můžete zobrazit serveru v průvodci.</span><span class="sxs-lookup"><span data-stu-id="daad5-170">With the protection agent installed, you can see the server in the wizard.</span></span> <span data-ttu-id="daad5-171">MABS také ukazuje jeho strukturu.</span><span class="sxs-lookup"><span data-stu-id="daad5-171">MABS also shows its structure.</span></span> <span data-ttu-id="daad5-172">Protože jste spustili ConfigureSharePoint.exe, MABS komunikuje se službou SharePoint VSS Writer service a její odpovídající databáze systému SQL Server a rozpozná strukturu farmy služby SharePoint, přidružených databázích obsahu a všechny odpovídající položky.</span><span class="sxs-lookup"><span data-stu-id="daad5-172">Because you ran ConfigureSharePoint.exe, MABS communicates with the SharePoint VSS Writer service and its corresponding SQL Server databases and recognizes the SharePoint farm structure, the associated content databases, and any corresponding items.</span></span>
   >
   >
4. <span data-ttu-id="daad5-173">Na **vyberte způsob ochrany dat** stránky, zadejte název **skupiny ochrany**a vyberte upřednostňovanou *metody ochrany*.</span><span class="sxs-lookup"><span data-stu-id="daad5-173">On the **Select Data Protection Method** page, enter the name of the **Protection Group**, and select your preferred *protection methods*.</span></span> <span data-ttu-id="daad5-174">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="daad5-174">Click **Next**.</span></span>

    ![Vyberte způsob ochrany dat](./media/backup-azure-backup-sharepoint/select-data-protection-method1.png)

   > [!NOTE]
   > <span data-ttu-id="daad5-176">Způsob ochrany disku pomáhá cíle krátkou dobu obnovení.</span><span class="sxs-lookup"><span data-stu-id="daad5-176">The disk protection method helps to meet short recovery-time objectives.</span></span>
   >
   >
5. <span data-ttu-id="daad5-177">Na **zadat krátkodobé cíle** vyberte upřednostňovanou **rozsah uchování** a zjistíte, kdy chcete vytváření záloh každý.</span><span class="sxs-lookup"><span data-stu-id="daad5-177">On the **Specify Short-Term Goals** page, select your preferred **Retention range** and identify when you want backups to occur.</span></span>

    ![Určení krátkodobých cílů](./media/backup-azure-backup-sharepoint/specify-short-term-goals2.png)

   > [!NOTE]
   > <span data-ttu-id="daad5-179">Protože obnovení je nejčastěji požadované pro data, která je menší než pět dní, jsme vybrali rozsahem uchování 5 dní na disku a zajistit, že zálohování probíhá při mimo provozní hodiny, v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="daad5-179">Because recovery is most often required for data that's less than five days old, we selected a retention range of five days on disk and ensured that the backup happens during non-production hours, for this example.</span></span>
   >
   >
6. <span data-ttu-id="daad5-180">Zkontrolujte místo přidělené pro skupinu ochrany na disku fondu úložiště a potom na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="daad5-180">Review the storage pool disk space allocated for the protection group, and click then **Next**.</span></span>
7. <span data-ttu-id="daad5-181">Pro každou skupinu ochrany MABS přiděluje místo na disku k uložení a správě repliky.</span><span class="sxs-lookup"><span data-stu-id="daad5-181">For every protection group, MABS allocates disk space to store and manage replicas.</span></span> <span data-ttu-id="daad5-182">V tomto okamžiku MABS, musíte vytvořit kopii vybraná data.</span><span class="sxs-lookup"><span data-stu-id="daad5-182">At this point, MABS must create a copy of the selected data.</span></span> <span data-ttu-id="daad5-183">Vyberte, jak a kdy chcete repliku vytvořit a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="daad5-183">Select how and when you want the replica created, and then click **Next**.</span></span>

    ![Vyberte způsob vytvoření repliky](./media/backup-azure-backup-sharepoint/choose-replica-creation-method.png)

   > [!NOTE]
   > <span data-ttu-id="daad5-185">Abyste měli jistotu, že není uskutečněn síťový provoz, vyberte dobu mimo provozní hodiny.</span><span class="sxs-lookup"><span data-stu-id="daad5-185">To make sure that network traffic is not effected, select a time outside production hours.</span></span>
   >
   >
8. <span data-ttu-id="daad5-186">MABS zajistíte integritu dat provedením kontroly konzistence na replice.</span><span class="sxs-lookup"><span data-stu-id="daad5-186">MABS ensures data integrity by performing consistency checks on the replica.</span></span> <span data-ttu-id="daad5-187">Existují dvě možnosti k dispozici.</span><span class="sxs-lookup"><span data-stu-id="daad5-187">There are two available options.</span></span> <span data-ttu-id="daad5-188">Můžete definovat plán, který chcete spustit kontrolu konzistence, nebo aplikace DPM můžete spustit kontrolu konzistence na replice automaticky vždy, když se stane nekonzistentní.</span><span class="sxs-lookup"><span data-stu-id="daad5-188">You can define a schedule to run consistency checks, or DPM can run consistency checks automatically on the replica whenever it becomes inconsistent.</span></span> <span data-ttu-id="daad5-189">Vyberte požadovanou možnost a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="daad5-189">Select your preferred option, and then click **Next**.</span></span>

    ![Kontrola konzistence](./media/backup-azure-backup-sharepoint/consistency-check.png)
9. <span data-ttu-id="daad5-191">Na **zadat Data Online ochrany** vyberte farmy služby SharePoint, který chcete chránit a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="daad5-191">On the **Specify Online Protection Data** page, select the SharePoint farm that you want to protect, and then click **Next**.</span></span>

    ![Aplikace DPM Protection1 služby SharePoint](./media/backup-azure-backup-sharepoint/select-online-protection1.png)
10. <span data-ttu-id="daad5-193">Na **zadejte plán Online zálohování** vyberte upřednostňovaný plán a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="daad5-193">On the **Specify Online Backup Schedule** page, select your preferred schedule, and then click **Next**.</span></span>

    ![Online_backup_schedule](./media/backup-azure-backup-sharepoint/specify-online-backup-schedule.png)

    > [!NOTE]
    > <span data-ttu-id="daad5-195">MABS poskytuje maximálně dvě denní zálohování do Azure ze pak k dispozici nejnovější disku bod zálohy.</span><span class="sxs-lookup"><span data-stu-id="daad5-195">MABS provides a maximum of two daily backups to Azure from the then available latest disk backup point.</span></span> <span data-ttu-id="daad5-196">Zálohování Azure můžete také ovládat velikost šířky pásma sítě WAN, který lze použít pro zálohování ve špičce a špičku pomocí [omezení sítě Azure Backup](https://azure.microsoft.com/documentation/articles/backup-configure-vault/#enable-network-throttling).</span><span class="sxs-lookup"><span data-stu-id="daad5-196">Azure Backup can also control the amount of WAN bandwidth that can be used for backups in peak and off-peak hours by using [Azure Backup Network Throttling](https://azure.microsoft.com/documentation/articles/backup-configure-vault/#enable-network-throttling).</span></span>
    >
    >
11. <span data-ttu-id="daad5-197">V závislosti na plán zálohování, který jste vybrali, na **zadejte zásady uchovávání Online** vyberte zásady uchovávání informací pro denní, týdenní, měsíční a roční body zálohy.</span><span class="sxs-lookup"><span data-stu-id="daad5-197">Depending on the backup schedule that you selected, on the **Specify Online Retention Policy** page, select the retention policy for daily, weekly, monthly, and yearly backup points.</span></span>

    ![Online_retention_policy](./media/backup-azure-backup-sharepoint/specify-online-retention.png)

    > [!NOTE]
    > <span data-ttu-id="daad5-199">MABS používá schéma uchovávání historických. otec SYN ve které je možné vybrat rozdílné zásady pro různé body zálohy.</span><span class="sxs-lookup"><span data-stu-id="daad5-199">MABS uses a grandfather-father-son retention scheme in which a different retention policy can be chosen for different backup points.</span></span>
    >
    >
12. <span data-ttu-id="daad5-200">Podobně jako u disku, repliku bodu počáteční odkaz musí být vytvořen v Azure.</span><span class="sxs-lookup"><span data-stu-id="daad5-200">Similar to disk, an initial reference point replica needs to be created in Azure.</span></span> <span data-ttu-id="daad5-201">Vyberte upřednostňovanou možnost vytvořit kopii prvotní zálohy do Azure, a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="daad5-201">Select your preferred option to create an initial backup copy to Azure, and then click **Next**.</span></span>

    ![Online_replica](./media/backup-azure-backup-sharepoint/online-replication.png)
13. <span data-ttu-id="daad5-203">Zkontrolujte vybrané nastavení na **Souhrn** a pak klikněte na tlačítko **vytvořit skupinu**.</span><span class="sxs-lookup"><span data-stu-id="daad5-203">Review your selected settings on the **Summary** page, and then click **Create Group**.</span></span> <span data-ttu-id="daad5-204">Zobrazí se zpráva o úspěšném provedení a po vytvoření skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="daad5-204">You will see a success message after the protection group has been created.</span></span>

    ![Souhrn](./media/backup-azure-backup-sharepoint/summary.png)

## <a name="restore-a-sharepoint-item-from-disk-by-using-mabs"></a><span data-ttu-id="daad5-206">Obnovení položky služby SharePoint z disku pomocí MABS</span><span class="sxs-lookup"><span data-stu-id="daad5-206">Restore a SharePoint item from disk by using MABS</span></span>
<span data-ttu-id="daad5-207">V následujícím příkladu *položky obnovení služby SharePoint* omylem odstraněný a je potřeba obnovit.</span><span class="sxs-lookup"><span data-stu-id="daad5-207">In the following example, the *Recovering SharePoint item* has been accidentally deleted and needs to be recovered.</span></span>
<span data-ttu-id="daad5-208">![Protection4 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)</span><span class="sxs-lookup"><span data-stu-id="daad5-208">![MABS SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)</span></span>

1. <span data-ttu-id="daad5-209">Otevřete **konzole správce aplikace DPM**.</span><span class="sxs-lookup"><span data-stu-id="daad5-209">Open the **DPM Administrator Console**.</span></span> <span data-ttu-id="daad5-210">V jsou uvedeny všechny farmy služby SharePoint, které jsou chráněné službou DPM **ochrany** kartě.</span><span class="sxs-lookup"><span data-stu-id="daad5-210">All SharePoint farms that are protected by DPM are shown in the **Protection** tab.</span></span>

    ![Protection3 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection4.png)
2. <span data-ttu-id="daad5-212">Chcete-li začít obnovili položku, vyberte **obnovení** kartě.</span><span class="sxs-lookup"><span data-stu-id="daad5-212">To begin to recover the item, select the **Recovery** tab.</span></span>

    ![Protection5 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection6.png)
3. <span data-ttu-id="daad5-214">Můžete hledat SharePoint pro *položky obnovení služby SharePoint* pomocí vyhledávání na základě zástupný znak v rámci obnovení bodu rozsahu.</span><span class="sxs-lookup"><span data-stu-id="daad5-214">You can search SharePoint for *Recovering SharePoint item* by using a wildcard-based search within a recovery point range.</span></span>

    ![Protection6 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection7.png)
4. <span data-ttu-id="daad5-216">Vyberte bod obnovení odpovídající ve výsledcích hledání, klikněte pravým tlačítkem položku a pak vyberte **obnovit**.</span><span class="sxs-lookup"><span data-stu-id="daad5-216">Select the appropriate recovery point from the search results, right-click the item, and then select **Recover**.</span></span>
5. <span data-ttu-id="daad5-217">Také můžete procházet různé body obnovení a vyberte databázi nebo položku, kterou chcete obnovit.</span><span class="sxs-lookup"><span data-stu-id="daad5-217">You can also browse through various recovery points and select a database or item to recover.</span></span> <span data-ttu-id="daad5-218">Vyberte **datum > čas obnovení**a potom vyberte správný **databáze > farmy služby SharePoint > bod obnovení > položky**.</span><span class="sxs-lookup"><span data-stu-id="daad5-218">Select **Date > Recovery time**, and then select the correct **Database > SharePoint farm > Recovery point > Item**.</span></span>

    ![Protection7 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection8.png)
6. <span data-ttu-id="daad5-220">Klikněte pravým tlačítkem položku a pak vyberte **obnovit** otevřete **Průvodce obnovením**.</span><span class="sxs-lookup"><span data-stu-id="daad5-220">Right-click the item, and then select **Recover** to open the **Recovery Wizard**.</span></span> <span data-ttu-id="daad5-221">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="daad5-221">Click **Next**.</span></span>

    ![Revidovat výběr obnovení](./media/backup-azure-backup-sharepoint/review-recovery-selection.png)
7. <span data-ttu-id="daad5-223">Vyberte typ obnovení, který chcete provést a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="daad5-223">Select the type of recovery that you want to perform, and then click **Next**.</span></span>

    ![Typ obnovení](./media/backup-azure-backup-sharepoint/select-recovery-type.png)

   > [!NOTE]
   > <span data-ttu-id="daad5-225">Výběr **obnovit na původní** v příkladu obnoví položka k původní web služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="daad5-225">The selection of **Recover to original** in the example recovers the item to the original SharePoint site.</span></span>
   >
   >
8. <span data-ttu-id="daad5-226">Vyberte **proces obnovení** , kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="daad5-226">Select the **Recovery Process** that you want to use.</span></span>

   * <span data-ttu-id="daad5-227">Vyberte **obnovit bez použití farmy obnovení** Pokud farmy služby SharePoint se nezměnila a je stejná jako bod obnovení, který se obnovuje.</span><span class="sxs-lookup"><span data-stu-id="daad5-227">Select **Recover without using a recovery farm** if the SharePoint farm has not changed and is the same as the recovery point that is being restored.</span></span>
   * <span data-ttu-id="daad5-228">Vyberte **obnovit použití farmy obnovení** Pokud od vytvoření bodu obnovení změnila farmy služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="daad5-228">Select **Recover using a recovery farm** if the SharePoint farm has changed since the recovery point was created.</span></span>

     ![Proces obnovení](./media/backup-azure-backup-sharepoint/recovery-process.png)
9. <span data-ttu-id="daad5-230">Zadejte pracovní umístění instance SQL serveru k obnovení databáze dočasně a zadejte pracovní sdílené složky na MABS a na serveru, na kterém běží SharePoint o obnovení položky.</span><span class="sxs-lookup"><span data-stu-id="daad5-230">Provide a staging SQL Server instance location to recover the database temporarily, and provide a staging file share on MABS and the server that's running SharePoint to recover the item.</span></span>

    ![Pracovní Location1](./media/backup-azure-backup-sharepoint/staging-location1.png)

    <span data-ttu-id="daad5-232">MABS připojí databázi obsahu, který je hostitelem položky služby SharePoint dočasné instanci systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="daad5-232">MABS attaches the content database that is hosting the SharePoint item to the temporary SQL Server instance.</span></span> <span data-ttu-id="daad5-233">Z databáze obsahu obnoví položku a vloží ho na pracovní umístění souboru na MABS.</span><span class="sxs-lookup"><span data-stu-id="daad5-233">From the content database, it recovers the item and puts it on the staging file location on MABS.</span></span> <span data-ttu-id="daad5-234">Položku obnovení, který je teď na pracovní umístění musí být exportovány do pracovní umístění na farmě služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="daad5-234">The recovered item that's on the staging location now needs to be exported to the staging location on the SharePoint farm.</span></span>

    ![Pracovní Location2](./media/backup-azure-backup-sharepoint/staging-location2.png)
10. <span data-ttu-id="daad5-236">Vyberte **nastavte možnosti obnovení**a použít nastavení zabezpečení pro farmu služby SharePoint nebo použít nastavení zabezpečení bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="daad5-236">Select **Specify recovery options**, and apply security settings to the SharePoint farm or apply the security settings of the recovery point.</span></span> <span data-ttu-id="daad5-237">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="daad5-237">Click **Next**.</span></span>

    ![Možnosti obnovení](./media/backup-azure-backup-sharepoint/recovery-options.png)

    > [!NOTE]
    > <span data-ttu-id="daad5-239">Můžete k omezení využití šířky pásma sítě.</span><span class="sxs-lookup"><span data-stu-id="daad5-239">You can choose to throttle the network bandwidth usage.</span></span> <span data-ttu-id="daad5-240">Tím se minimalizují dopad na provozním serveru během pracovní doby.</span><span class="sxs-lookup"><span data-stu-id="daad5-240">This minimizes impact to the production server during production hours.</span></span>
    >
    >
11. <span data-ttu-id="daad5-241">Zkontrolujte souhrnné informace a pak klikněte na **obnovit** zahájíte obnovení souboru.</span><span class="sxs-lookup"><span data-stu-id="daad5-241">Review the summary information, and then click **Recover** to begin recovery of the file.</span></span>

    ![Obnovení souhrn](./media/backup-azure-backup-sharepoint/recovery-summary.png)
12. <span data-ttu-id="daad5-243">Nyní vybrat **monitorování** ve **konzoly pro správu MABS** zobrazíte **stav** obnovení.</span><span class="sxs-lookup"><span data-stu-id="daad5-243">Now select the **Monitoring** tab in the **MABS Administrator Console** to view the **Status** of the recovery.</span></span>

    ![Stav obnovení](./media/backup-azure-backup-sharepoint/recovery-monitoring.png)

    > [!NOTE]
    > <span data-ttu-id="daad5-245">Soubor je nyní obnovit.</span><span class="sxs-lookup"><span data-stu-id="daad5-245">The file is now restored.</span></span> <span data-ttu-id="daad5-246">Můžete obnovit web služby SharePoint k obnovené najdete v souboru.</span><span class="sxs-lookup"><span data-stu-id="daad5-246">You can refresh the SharePoint site to check the restored file.</span></span>
    >
    >

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a><span data-ttu-id="daad5-247">Obnovení databáze služby SharePoint z Azure pomocí aplikace DPM</span><span class="sxs-lookup"><span data-stu-id="daad5-247">Restore a SharePoint database from Azure by using DPM</span></span>
1. <span data-ttu-id="daad5-248">Pokud chcete obnovit databázi obsahu služby SharePoint, procházet různé body obnovení (jak je uvedeno výše) a vyberte bod obnovení, který chcete obnovit.</span><span class="sxs-lookup"><span data-stu-id="daad5-248">To recover a SharePoint content database, browse through various recovery points (as shown previously), and select the recovery point that you want to restore.</span></span>

    ![Protection8 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. <span data-ttu-id="daad5-250">Dvakrát klikněte na bod obnovení služby SharePoint zobrazíte dostupné informace o katalogu služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="daad5-250">Double-click the SharePoint recovery point to show the available SharePoint catalog information.</span></span>

   > [!NOTE]
   > <span data-ttu-id="daad5-251">Protože pro dlouhodobé uchovávání v Azure je chráněn farmy služby SharePoint, nejsou dostupné na MABS žádné informace katalogu (metadata).</span><span class="sxs-lookup"><span data-stu-id="daad5-251">Because the SharePoint farm is protected for long-term retention in Azure, no catalog information (metadata) is available on MABS.</span></span> <span data-ttu-id="daad5-252">V důsledku toho vždy, když databáze obsahu služby SharePoint v daném okamžiku je možné obnovit, budete muset znovu katalogu farmy služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="daad5-252">As a result, whenever a point-in-time SharePoint content database needs to be recovered, you need to catalog the SharePoint farm again.</span></span>
   >
   >
3. <span data-ttu-id="daad5-253">Klikněte na tlačítko **opětovného zařazení do katalogu**.</span><span class="sxs-lookup"><span data-stu-id="daad5-253">Click **Re-catalog**.</span></span>

    ![Protection10 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)

    <span data-ttu-id="daad5-255">**Provést novou katalogizaci cloudu** otevře se okno stav.</span><span class="sxs-lookup"><span data-stu-id="daad5-255">The **Cloud Recatalog** status window opens.</span></span>

    ![Protection11 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)

    <span data-ttu-id="daad5-257">Po dokončení do katalogu se stav změní na *úspěch*.</span><span class="sxs-lookup"><span data-stu-id="daad5-257">After cataloging is finished, the status changes to *Success*.</span></span> <span data-ttu-id="daad5-258">Klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="daad5-258">Click **Close**.</span></span>

    ![Protection12 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. <span data-ttu-id="daad5-260">Klikněte na objekt SharePoint ukazuje MABS **obnovení** karty lze získat strukturu databázi obsahu.</span><span class="sxs-lookup"><span data-stu-id="daad5-260">Click the SharePoint object shown in the MABS **Recovery** tab to get the content database structure.</span></span> <span data-ttu-id="daad5-261">Klikněte pravým tlačítkem položku a pak klikněte na **obnovit**.</span><span class="sxs-lookup"><span data-stu-id="daad5-261">Right-click the item, and then click **Recover**.</span></span>

    ![Protection13 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. <span data-ttu-id="daad5-263">Postupujte v tomto okamžiku [kroky obnovení dříve v tomto článku](#restore-a-sharepoint-item-from-disk-using-dpm) k obnovení databázi obsahu služby SharePoint z disku.</span><span class="sxs-lookup"><span data-stu-id="daad5-263">At this point, follow the [recovery steps earlier in this article](#restore-a-sharepoint-item-from-disk-using-dpm) to recover a SharePoint content database from disk.</span></span>

## <a name="faqs"></a><span data-ttu-id="daad5-264">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="daad5-264">FAQs</span></span>
<span data-ttu-id="daad5-265">Otázka: je možné obnovit položky služby SharePoint do původního umístění, pokud je služba SharePoint nakonfigurována pomocí technologie AlwaysOn serveru SQL (s ochrany na disku)?</span><span class="sxs-lookup"><span data-stu-id="daad5-265">Q: Can I recover a SharePoint item to the original location if SharePoint is configured by using SQL AlwaysOn (with protection on disk)?</span></span><br>
<span data-ttu-id="daad5-266">Odpověď: Ano, položka je možné obnovit do původního webu služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="daad5-266">A: Yes, the item can be recovered to the original SharePoint site.</span></span>

<span data-ttu-id="daad5-267">Otázka: je možné obnovit databázi služby SharePoint do původního umístění, pokud je služba SharePoint nakonfigurována pomocí technologie AlwaysOn serveru SQL?</span><span class="sxs-lookup"><span data-stu-id="daad5-267">Q: Can I recover a SharePoint database to the original location if SharePoint is configured by using SQL AlwaysOn?</span></span><br>
<span data-ttu-id="daad5-268">Odpověď: protože databáze služby SharePoint jsou konfigurované v SQL AlwaysOn, nemůže být upraven Pokud skupina dostupnosti je odebrána.</span><span class="sxs-lookup"><span data-stu-id="daad5-268">A: Because SharePoint databases are configured in SQL AlwaysOn, they cannot be modified unless the availability group is removed.</span></span> <span data-ttu-id="daad5-269">V důsledku toho MABS nelze obnovit databázi do původního umístění.</span><span class="sxs-lookup"><span data-stu-id="daad5-269">As a result, MABS cannot restore a database to the original location.</span></span> <span data-ttu-id="daad5-270">Můžete obnovit databázi systému SQL Server do jiné instance systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="daad5-270">You can recover a SQL Server database to another SQL Server instance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="daad5-271">Další kroky</span><span class="sxs-lookup"><span data-stu-id="daad5-271">Next steps</span></span>
* <span data-ttu-id="daad5-272">Další informace o MABS ochrany služby SharePoint – viz [řady Video - DPM ochrany služby SharePoint](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)</span><span class="sxs-lookup"><span data-stu-id="daad5-272">Learn more about MABS Protection of SharePoint - see [Video Series - DPM Protection of SharePoint](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)</span></span>
