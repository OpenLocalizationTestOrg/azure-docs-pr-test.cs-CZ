---
title: "aaaUse Azure Backup server tooback nahoru tooAzure farmy služby SharePoint | Microsoft Docs"
description: "Pomocí serveru Azure Backup tooback a obnovení dat služby SharePoint. Tento článek obsahuje informace o tooconfigure hello farmy služby SharePoint tak, že požadovaná data se uloží v Azure. Chráněná data služby SharePoint můžete obnovit z disku nebo z Azure."
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
ms.openlocfilehash: 350c1ac0f3518f400062f3b586bbe9710a79915a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-a-sharepoint-farm-tooazure"></a><span data-ttu-id="3e43c-105">Zálohování tooAzure farmy služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="3e43c-105">Back up a SharePoint farm tooAzure</span></span>
<span data-ttu-id="3e43c-106">Zálohování služby SharePoint farmu tooMicrosoft Azure pomocí služby Microsoft Azure Backup Server (MABS) v mnohem hello stejným způsobem, který zálohujete jiných zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="3e43c-106">You back up a SharePoint farm tooMicrosoft Azure by using Microsoft Azure Backup Server (MABS) in much hello same way that you back up other data sources.</span></span> <span data-ttu-id="3e43c-107">Azure Backup poskytuje flexibilitu při hello plán zálohování toocreate denní, týdenní, měsíční nebo roční body zálohy a poskytuje možnosti zásad uchovávání informací pro různé body zálohy.</span><span class="sxs-lookup"><span data-stu-id="3e43c-107">Azure Backup provides flexibility in hello backup schedule toocreate daily, weekly, monthly, or yearly backup points and gives you retention policy options for various backup points.</span></span> <span data-ttu-id="3e43c-108">Poskytuje také hello schopností toostore místního disku kopie pro rychlé cíle doba obnovení (RTO) a toostore zkopíruje tooAzure pro ekonomické, dlouhodobé uchovávání.</span><span class="sxs-lookup"><span data-stu-id="3e43c-108">It also provides hello capability toostore local disk copies for quick recovery-time objectives (RTO) and toostore copies tooAzure for economical, long-term retention.</span></span>

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a><span data-ttu-id="3e43c-109">Podporované verze služby SharePoint a související scénáře ochrany</span><span class="sxs-lookup"><span data-stu-id="3e43c-109">SharePoint supported versions and related protection scenarios</span></span>
<span data-ttu-id="3e43c-110">Zálohování Azure pro DPM podporuje hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="3e43c-110">Azure Backup for DPM supports hello following scenarios:</span></span>

| <span data-ttu-id="3e43c-111">Úloha</span><span class="sxs-lookup"><span data-stu-id="3e43c-111">Workload</span></span> | <span data-ttu-id="3e43c-112">Verze</span><span class="sxs-lookup"><span data-stu-id="3e43c-112">Version</span></span> | <span data-ttu-id="3e43c-113">Nasazení služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="3e43c-113">SharePoint deployment</span></span> | <span data-ttu-id="3e43c-114">Ochrana a obnovení</span><span class="sxs-lookup"><span data-stu-id="3e43c-114">Protection and recovery</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="3e43c-115">SharePoint</span><span class="sxs-lookup"><span data-stu-id="3e43c-115">SharePoint</span></span> |<span data-ttu-id="3e43c-116">SharePoint 2013, SharePoint 2010 a SharePoint 2007 SharePoint 3.0</span><span class="sxs-lookup"><span data-stu-id="3e43c-116">SharePoint 2013, SharePoint 2010, SharePoint 2007, SharePoint 3.0</span></span> |<span data-ttu-id="3e43c-117">SharePoint nasazená jako fyzický server nebo virtuální počítač technologie Hyper-V nebo VMware</span><span class="sxs-lookup"><span data-stu-id="3e43c-117">SharePoint deployed as a physical server or Hyper-V/VMware virtual machine</span></span> <br> -------------- <br> <span data-ttu-id="3e43c-118">Technologie AlwaysOn serveru SQL</span><span class="sxs-lookup"><span data-stu-id="3e43c-118">SQL AlwaysOn</span></span> | <span data-ttu-id="3e43c-119">Ochrana farmy služby SharePoint možnosti obnovení: farma obnovení, databáze a soubor nebo položka seznamu z bodů obnovení disku.</span><span class="sxs-lookup"><span data-stu-id="3e43c-119">Protect SharePoint Farm recovery options: Recovery farm, database, and file or list item from disk recovery points.</span></span>  <span data-ttu-id="3e43c-120">Obnovení z bodů obnovení Azure farmy a databáze.</span><span class="sxs-lookup"><span data-stu-id="3e43c-120">Farm and database recovery from Azure recovery points.</span></span> |

## <a name="before-you-start"></a><span data-ttu-id="3e43c-121">Než začnete</span><span class="sxs-lookup"><span data-stu-id="3e43c-121">Before you start</span></span>
<span data-ttu-id="3e43c-122">Existuje několik možností, potřebujete tooconfirm před zálohováním tooAzure farmy služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="3e43c-122">There are a few things you need tooconfirm before you back up a SharePoint farm tooAzure.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="3e43c-123">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3e43c-123">Prerequisites</span></span>
<span data-ttu-id="3e43c-124">Než budete pokračovat, ujistěte se, že máte [nainstalované a připravené hello serveru Azure Backup](backup-azure-microsoft-azure-backup.md) tooprotect úlohy.</span><span class="sxs-lookup"><span data-stu-id="3e43c-124">Before you proceed, make sure that you have [installed and prepared hello Azure Backup Server](backup-azure-microsoft-azure-backup.md) tooprotect workloads.</span></span>

### <a name="protection-agent"></a><span data-ttu-id="3e43c-125">Agent ochrany</span><span class="sxs-lookup"><span data-stu-id="3e43c-125">Protection agent</span></span>
<span data-ttu-id="3e43c-126">agent ochrany Hello musí být nainstalován na server hello se systémem SharePoint, hello serverech se systémem SQL Server a všechny ostatní servery, které jsou součástí farmy služby SharePoint hello.</span><span class="sxs-lookup"><span data-stu-id="3e43c-126">hello Protection agent must be installed on hello server that's running SharePoint, hello servers that are running SQL Server, and all other servers that are part of hello SharePoint farm.</span></span> <span data-ttu-id="3e43c-127">Další informace o tom, tooset až hello agenta ochrany, najdete v části [instalace agenta ochrany](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).</span><span class="sxs-lookup"><span data-stu-id="3e43c-127">For more information about how tooset up hello protection agent, see [Setup Protection Agent](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).</span></span>  <span data-ttu-id="3e43c-128">Hello jedinou výjimkou je, že instalujete agenta hello pouze na jednom webovém serveru front-end (WFE).</span><span class="sxs-lookup"><span data-stu-id="3e43c-128">hello one exception is that you install hello agent only on a single web front end (WFE) server.</span></span> <span data-ttu-id="3e43c-129">Aplikace DPM vyžaduje hello agenta na jednu pouze tooserve serveru WFE jako hello vstupní bod pro ochranu.</span><span class="sxs-lookup"><span data-stu-id="3e43c-129">DPM needs hello agent on one WFE server only tooserve as hello entry point for protection.</span></span>

### <a name="sharepoint-farm"></a><span data-ttu-id="3e43c-130">Farmy služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="3e43c-130">SharePoint farm</span></span>
<span data-ttu-id="3e43c-131">Pro každých 10 milionů položek ve farmě hello musí být alespoň 2 GB místa na svazku hello, kde je umístěna složka MABS hello.</span><span class="sxs-lookup"><span data-stu-id="3e43c-131">For every 10 million items in hello farm, there must be at least 2 GB of space on hello volume where hello MABS folder is located.</span></span> <span data-ttu-id="3e43c-132">Tento prostor je nezbytné pro generování katalogu.</span><span class="sxs-lookup"><span data-stu-id="3e43c-132">This space is required for catalog generation.</span></span> <span data-ttu-id="3e43c-133">Pro MABS toorecover konkrétní položky (kolekce webů, weby, seznamy, knihovny dokumentů, složky, jednotlivé dokumenty a položky seznamu) generování katalogu vytvoří seznam hello adres URL, které jsou obsaženy v jednotlivých databázích obsahu.</span><span class="sxs-lookup"><span data-stu-id="3e43c-133">For MABS toorecover specific items (site collections, sites, lists, document libraries, folders, individual documents, and list items), catalog generation creates a list of hello URLs that are contained within each content database.</span></span> <span data-ttu-id="3e43c-134">Hello seznam adres URL můžete zobrazit v podokně hello obnovitelných položek v hello **obnovení** oblasti konzoly pro správu MABS úlohy.</span><span class="sxs-lookup"><span data-stu-id="3e43c-134">You can view hello list of URLs in hello recoverable item pane in hello **Recovery** task area of MABS Administrator Console.</span></span>

### <a name="sql-server"></a><span data-ttu-id="3e43c-135">SQL Server</span><span class="sxs-lookup"><span data-stu-id="3e43c-135">SQL Server</span></span>
<span data-ttu-id="3e43c-136">MABS běží pod účtem LocalSystem.</span><span class="sxs-lookup"><span data-stu-id="3e43c-136">MABS runs as a LocalSystem account.</span></span> <span data-ttu-id="3e43c-137">tooback zálohu databáze systému SQL Server, MABS potřebuje oprávnění sysadmin na tento účet pro hello serveru se systémem SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3e43c-137">tooback up SQL Server databases, MABS needs sysadmin privileges on that account for hello server that's running SQL Server.</span></span> <span data-ttu-id="3e43c-138">Nastavit NT AUTHORITY\SYSTEM příliš*sysadmin* na hello serveru, který je spuštěn SQL Server předtím, než ho zálohovat.</span><span class="sxs-lookup"><span data-stu-id="3e43c-138">Set NT AUTHORITY\SYSTEM too*sysadmin* on hello server that's running SQL Server before you back it up.</span></span>

<span data-ttu-id="3e43c-139">Pokud hello farmy služby SharePoint databáze systému SQL Server, které jsou nakonfigurovány s aliasy systému SQL Server, nainstalujte na hello front-end webový server, který bude chránit MABS hello součásti klienta systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3e43c-139">If hello SharePoint farm has SQL Server databases that are configured with SQL Server aliases, install hello SQL Server client components on hello front-end Web server that MABS will protect.</span></span>

### <a name="sharepoint-server"></a><span data-ttu-id="3e43c-140">SharePoint Server</span><span class="sxs-lookup"><span data-stu-id="3e43c-140">SharePoint Server</span></span>
<span data-ttu-id="3e43c-141">Při výkonu závislá na mnoha faktorech, jako je například velikost farmy služby SharePoint, jako obecné pokyny jeden MABS můžete chránit 25 TB farmu služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="3e43c-141">While performance depends on many factors such as size of SharePoint farm, as general guidance one MABS can protect a 25 TB SharePoint farm.</span></span>

### <a name="whats-not-supported"></a><span data-ttu-id="3e43c-142">Co není podporováno</span><span class="sxs-lookup"><span data-stu-id="3e43c-142">What's not supported</span></span>
* <span data-ttu-id="3e43c-143">MABS, který chrání farmy služby SharePoint nechrání indexy hledání nebo databáze aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="3e43c-143">MABS that protects a SharePoint farm does not protect search indexes or application service databases.</span></span> <span data-ttu-id="3e43c-144">Tooconfigure hello ochranu těchto databází, budete potřebovat samostatně.</span><span class="sxs-lookup"><span data-stu-id="3e43c-144">You will need tooconfigure hello protection of these databases separately.</span></span>
* <span data-ttu-id="3e43c-145">MABS neposkytuje zálohování databází serveru SQL služby SharePoint, které jsou hostované na sdílených složkách škálovatelného souborového serveru (SOFS).</span><span class="sxs-lookup"><span data-stu-id="3e43c-145">MABS does not provide backup of SharePoint SQL Server databases that are hosted on scale-out file server (SOFS) shares.</span></span>

## <a name="configure-sharepoint-protection"></a><span data-ttu-id="3e43c-146">Konfigurace ochrany Sharepointu</span><span class="sxs-lookup"><span data-stu-id="3e43c-146">Configure SharePoint protection</span></span>
<span data-ttu-id="3e43c-147">Než budete moct použít MABS tooprotect služby SharePoint, musíte nakonfigurovat hello SharePoint VSS Writer (služby WSS Writer) pomocí **ConfigureSharePoint.exe**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-147">Before you can use MABS tooprotect SharePoint, you must configure hello SharePoint VSS Writer service (WSS Writer service) by using **ConfigureSharePoint.exe**.</span></span>

<span data-ttu-id="3e43c-148">Můžete najít **ConfigureSharePoint.exe** v složky \bin hello [MABS Instalační cesta] na hello front-end webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="3e43c-148">You can find **ConfigureSharePoint.exe** in hello [MABS Installation Path]\bin folder on hello front-end web server.</span></span> <span data-ttu-id="3e43c-149">Tento nástroj poskytuje hello agenta ochrany s přihlašovacími údaji hello pro farmu služby SharePoint hello.</span><span class="sxs-lookup"><span data-stu-id="3e43c-149">This tool provides hello protection agent with hello credentials for hello SharePoint farm.</span></span> <span data-ttu-id="3e43c-150">Spustíte ji na jeden server WFE.</span><span class="sxs-lookup"><span data-stu-id="3e43c-150">You run it on a single WFE server.</span></span> <span data-ttu-id="3e43c-151">Pokud máte více serverů WFE, vyberte pouze jeden, když konfigurujete skupinu ochrany.</span><span class="sxs-lookup"><span data-stu-id="3e43c-151">If you have multiple WFE servers, select just one when you configure a protection group.</span></span>

### <a name="tooconfigure-hello-sharepoint-vss-writer-service"></a><span data-ttu-id="3e43c-152">tooconfigure hello SharePoint VSS Writer service</span><span class="sxs-lookup"><span data-stu-id="3e43c-152">tooconfigure hello SharePoint VSS Writer service</span></span>
1. <span data-ttu-id="3e43c-153">Na serveru WFE hello na příkazovém řádku přejděte příliš \bin\ [umístění instalace MABS]</span><span class="sxs-lookup"><span data-stu-id="3e43c-153">On hello WFE server, at a command prompt, go too[MABS installation location]\bin\\</span></span>
2. <span data-ttu-id="3e43c-154">Zadejte ConfigureSharePoint - EnableSharePointProtection.</span><span class="sxs-lookup"><span data-stu-id="3e43c-154">Enter ConfigureSharePoint -EnableSharePointProtection.</span></span>
3. <span data-ttu-id="3e43c-155">Zadejte přihlašovací údaje správce farmy hello.</span><span class="sxs-lookup"><span data-stu-id="3e43c-155">Enter hello farm administrator credentials.</span></span> <span data-ttu-id="3e43c-156">Tento účet by měl být členem místní skupiny správců hello na serveru WFE hello.</span><span class="sxs-lookup"><span data-stu-id="3e43c-156">This account should be a member of hello local Administrator group on hello WFE server.</span></span> <span data-ttu-id="3e43c-157">Pokud není správcem farmy hello hello místního správce udělit následující oprávnění na serveru WFE hello:</span><span class="sxs-lookup"><span data-stu-id="3e43c-157">If hello farm administrator isn’t a local admin grant hello following permissions on hello WFE server:</span></span>
   * <span data-ttu-id="3e43c-158">Úplné řízení skupině WSS_Admin_WPG hello grant DPM toohello složky (% Program Files%\Microsoft Azure Backup\DPM).</span><span class="sxs-lookup"><span data-stu-id="3e43c-158">Grant hello WSS_Admin_WPG group full control toohello DPM folder (%Program Files%\Microsoft Azure Backup\DPM).</span></span>
   * <span data-ttu-id="3e43c-159">Udělte přístup pro čtení skupině WSS_Admin_WPG hello toohello klíč registru aplikace DPM (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager).</span><span class="sxs-lookup"><span data-stu-id="3e43c-159">Grant hello WSS_Admin_WPG group read access toohello DPM Registry key (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager).</span></span>

> [!NOTE]
> <span data-ttu-id="3e43c-160">Vždy, když dojde ke změně v hello přihlašovací údaje správce farmy služby SharePoint, budete potřebovat toorerun ConfigureSharePoint.exe.</span><span class="sxs-lookup"><span data-stu-id="3e43c-160">You’ll need toorerun ConfigureSharePoint.exe whenever there’s a change in hello SharePoint farm administrator credentials.</span></span>
>
>

## <a name="back-up-a-sharepoint-farm-by-using-mabs"></a><span data-ttu-id="3e43c-161">Zálohování farmy služby SharePoint pomocí MABS</span><span class="sxs-lookup"><span data-stu-id="3e43c-161">Back up a SharePoint farm by using MABS</span></span>
<span data-ttu-id="3e43c-162">Po nakonfigurování MABS a hello farmy služby SharePoint, jak je popsáno dříve, můžete pomocí MABS chráněný službou SharePoint.</span><span class="sxs-lookup"><span data-stu-id="3e43c-162">After you have configured MABS and hello SharePoint farm as explained previously, SharePoint can be protected by MABS.</span></span>

### <a name="tooprotect-a-sharepoint-farm"></a><span data-ttu-id="3e43c-163">tooprotect farmy služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="3e43c-163">tooprotect a SharePoint farm</span></span>
1. <span data-ttu-id="3e43c-164">Z hello **ochrany** klikněte na kartě hello konzole pro správu MABS **nový**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-164">From hello **Protection** tab of hello MABS Administrator Console, click **New**.</span></span>
    <span data-ttu-id="3e43c-165">![Nové karty ochrana](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)</span><span class="sxs-lookup"><span data-stu-id="3e43c-165">![New Protection Tab](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)</span></span>
2. <span data-ttu-id="3e43c-166">Na hello **vybrat typ skupiny ochrany** stránku hello **vytvořením nové skupiny ochrany** průvodce, vyberte **servery**a potom klikněte na **Další** .</span><span class="sxs-lookup"><span data-stu-id="3e43c-166">On hello **Select Protection Group Type** page of hello **Create New Protection Group** wizard, select **Servers**, and then click **Next**.</span></span>

    ![Typ skupiny ochrany vyberte](./media/backup-azure-backup-sharepoint/select-protection-group-type.png)
3. <span data-ttu-id="3e43c-168">Na hello **vybrat členy skupiny** obrazovku, vyberte hello zaškrtávací políčko pro server SharePoint hello tooprotect a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-168">On hello **Select Group Members** screen, select hello check box for hello SharePoint server you want tooprotect and click **Next**.</span></span>

    ![Vybrat členy skupiny](./media/backup-azure-backup-sharepoint/select-group-members2.png)

   > [!NOTE]
   > <span data-ttu-id="3e43c-170">S nainstalovaným agentem ochrany hello uvidíte hello serveru v Průvodci hello.</span><span class="sxs-lookup"><span data-stu-id="3e43c-170">With hello protection agent installed, you can see hello server in hello wizard.</span></span> <span data-ttu-id="3e43c-171">MABS také ukazuje jeho strukturu.</span><span class="sxs-lookup"><span data-stu-id="3e43c-171">MABS also shows its structure.</span></span> <span data-ttu-id="3e43c-172">Protože jste spustili ConfigureSharePoint.exe, MABS komunikuje s hello SharePoint VSS Writer service a její odpovídající databáze systému SQL Server a rozpozná hello strukturu farmy služby SharePoint, hello přidružených databází obsahu a všechny odpovídající položky.</span><span class="sxs-lookup"><span data-stu-id="3e43c-172">Because you ran ConfigureSharePoint.exe, MABS communicates with hello SharePoint VSS Writer service and its corresponding SQL Server databases and recognizes hello SharePoint farm structure, hello associated content databases, and any corresponding items.</span></span>
   >
   >
4. <span data-ttu-id="3e43c-173">Na hello **vyberte způsob ochrany dat** stránky, zadejte název hello hello **skupiny ochrany**a vyberte upřednostňovanou *metody ochrany*.</span><span class="sxs-lookup"><span data-stu-id="3e43c-173">On hello **Select Data Protection Method** page, enter hello name of hello **Protection Group**, and select your preferred *protection methods*.</span></span> <span data-ttu-id="3e43c-174">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-174">Click **Next**.</span></span>

    ![Vyberte způsob ochrany dat](./media/backup-azure-backup-sharepoint/select-data-protection-method1.png)

   > [!NOTE]
   > <span data-ttu-id="3e43c-176">způsob ochrany disku Hello pomáhá toomeet krátkou dobu obnovení cíle.</span><span class="sxs-lookup"><span data-stu-id="3e43c-176">hello disk protection method helps toomeet short recovery-time objectives.</span></span>
   >
   >
5. <span data-ttu-id="3e43c-177">Na hello **zadat krátkodobé cíle** vyberte upřednostňovanou **rozsah uchování** a zjistíte, kdy chcete toooccur zálohy.</span><span class="sxs-lookup"><span data-stu-id="3e43c-177">On hello **Specify Short-Term Goals** page, select your preferred **Retention range** and identify when you want backups toooccur.</span></span>

    ![Určení krátkodobých cílů](./media/backup-azure-backup-sharepoint/specify-short-term-goals2.png)

   > [!NOTE]
   > <span data-ttu-id="3e43c-179">Protože obnovení je nejčastěji požadované pro data, která je menší než pět dní, jsme vybrali rozsahem uchování 5 dní na disku a zajistit, že během mimo provozní hodiny, v tomto příkladu se stane hello zálohování.</span><span class="sxs-lookup"><span data-stu-id="3e43c-179">Because recovery is most often required for data that's less than five days old, we selected a retention range of five days on disk and ensured that hello backup happens during non-production hours, for this example.</span></span>
   >
   >
6. <span data-ttu-id="3e43c-180">Zkontrolujte hello úložiště fondu místo na disku přidělené pro skupinu ochrany hello a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-180">Review hello storage pool disk space allocated for hello protection group, and click then **Next**.</span></span>
7. <span data-ttu-id="3e43c-181">Pro každou skupinu ochrany MABS přiděluje toostore místa na disku a spravovat repliky.</span><span class="sxs-lookup"><span data-stu-id="3e43c-181">For every protection group, MABS allocates disk space toostore and manage replicas.</span></span> <span data-ttu-id="3e43c-182">V tomto okamžiku MABS, musíte vytvořit kopii hello vybraná data.</span><span class="sxs-lookup"><span data-stu-id="3e43c-182">At this point, MABS must create a copy of hello selected data.</span></span> <span data-ttu-id="3e43c-183">Vyberte, jak a kdy chcete hello repliky vytvořit a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-183">Select how and when you want hello replica created, and then click **Next**.</span></span>

    ![Vyberte způsob vytvoření repliky](./media/backup-azure-backup-sharepoint/choose-replica-creation-method.png)

   > [!NOTE]
   > <span data-ttu-id="3e43c-185">toomake se, že není uskutečněn síťový provoz, vyberte čas mimo pracovní hodiny.</span><span class="sxs-lookup"><span data-stu-id="3e43c-185">toomake sure that network traffic is not effected, select a time outside production hours.</span></span>
   >
   >
8. <span data-ttu-id="3e43c-186">MABS zajistíte integritu dat provedením kontroly konzistence na replice hello.</span><span class="sxs-lookup"><span data-stu-id="3e43c-186">MABS ensures data integrity by performing consistency checks on hello replica.</span></span> <span data-ttu-id="3e43c-187">Existují dvě možnosti k dispozici.</span><span class="sxs-lookup"><span data-stu-id="3e43c-187">There are two available options.</span></span> <span data-ttu-id="3e43c-188">Můžete definovat kontroly konzistence toorun plán nebo vždy, když se stane nekonzistentní se DPM dá spustit kontrolu konzistence automaticky na hello repliky.</span><span class="sxs-lookup"><span data-stu-id="3e43c-188">You can define a schedule toorun consistency checks, or DPM can run consistency checks automatically on hello replica whenever it becomes inconsistent.</span></span> <span data-ttu-id="3e43c-189">Vyberte požadovanou možnost a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-189">Select your preferred option, and then click **Next**.</span></span>

    ![Kontrola konzistence](./media/backup-azure-backup-sharepoint/consistency-check.png)
9. <span data-ttu-id="3e43c-191">Na hello **zadat Data Online ochrany** vyberte hello farmy služby SharePoint má tooprotect a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-191">On hello **Specify Online Protection Data** page, select hello SharePoint farm that you want tooprotect, and then click **Next**.</span></span>

    ![Aplikace DPM Protection1 služby SharePoint](./media/backup-azure-backup-sharepoint/select-online-protection1.png)
10. <span data-ttu-id="3e43c-193">Na hello **zadejte plán Online zálohování** vyberte upřednostňovaný plán a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-193">On hello **Specify Online Backup Schedule** page, select your preferred schedule, and then click **Next**.</span></span>

    ![Online_backup_schedule](./media/backup-azure-backup-sharepoint/specify-online-backup-schedule.png)

    > [!NOTE]
    > <span data-ttu-id="3e43c-195">MABS poskytuje maximálně dvě tooAzure denní zálohy z hello pak k dispozici nejnovější bod zálohy disku.</span><span class="sxs-lookup"><span data-stu-id="3e43c-195">MABS provides a maximum of two daily backups tooAzure from hello then available latest disk backup point.</span></span> <span data-ttu-id="3e43c-196">Zálohování Azure můžete také ovládat hello množství šířky pásma sítě WAN, který lze použít pro zálohování ve špičce a špičku pomocí [omezení sítě Azure Backup](https://azure.microsoft.com/documentation/articles/backup-configure-vault/#enable-network-throttling).</span><span class="sxs-lookup"><span data-stu-id="3e43c-196">Azure Backup can also control hello amount of WAN bandwidth that can be used for backups in peak and off-peak hours by using [Azure Backup Network Throttling](https://azure.microsoft.com/documentation/articles/backup-configure-vault/#enable-network-throttling).</span></span>
    >
    >
11. <span data-ttu-id="3e43c-197">V závislosti na hello plán zálohování, který jste vybrali, na hello **zadejte zásady uchovávání Online** stránky, vyberte hello zásady uchovávání informací pro denní, týdenní, měsíční a roční body zálohy.</span><span class="sxs-lookup"><span data-stu-id="3e43c-197">Depending on hello backup schedule that you selected, on hello **Specify Online Retention Policy** page, select hello retention policy for daily, weekly, monthly, and yearly backup points.</span></span>

    ![Online_retention_policy](./media/backup-azure-backup-sharepoint/specify-online-retention.png)

    > [!NOTE]
    > <span data-ttu-id="3e43c-199">MABS používá schéma uchovávání historických. otec SYN ve které je možné vybrat rozdílné zásady pro různé body zálohy.</span><span class="sxs-lookup"><span data-stu-id="3e43c-199">MABS uses a grandfather-father-son retention scheme in which a different retention policy can be chosen for different backup points.</span></span>
    >
    >
12. <span data-ttu-id="3e43c-200">Podobně jako toodisk repliku bodu počáteční odkaz musí toobe vytvoří v Azure.</span><span class="sxs-lookup"><span data-stu-id="3e43c-200">Similar toodisk, an initial reference point replica needs toobe created in Azure.</span></span> <span data-ttu-id="3e43c-201">Vyberte vaší toocreate upřednostňovanou možnost ověřování tooAzure kopie prvotní zálohy a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-201">Select your preferred option toocreate an initial backup copy tooAzure, and then click **Next**.</span></span>

    ![Online_replica](./media/backup-azure-backup-sharepoint/online-replication.png)
13. <span data-ttu-id="3e43c-203">Zkontrolujte vybrané nastavení na hello **Souhrn** a pak klikněte na tlačítko **vytvořit skupinu**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-203">Review your selected settings on hello **Summary** page, and then click **Create Group**.</span></span> <span data-ttu-id="3e43c-204">Zobrazí se zpráva o úspěšném provedení a po vytvoření skupiny ochrany hello.</span><span class="sxs-lookup"><span data-stu-id="3e43c-204">You will see a success message after hello protection group has been created.</span></span>

    ![Souhrn](./media/backup-azure-backup-sharepoint/summary.png)

## <a name="restore-a-sharepoint-item-from-disk-by-using-mabs"></a><span data-ttu-id="3e43c-206">Obnovení položky služby SharePoint z disku pomocí MABS</span><span class="sxs-lookup"><span data-stu-id="3e43c-206">Restore a SharePoint item from disk by using MABS</span></span>
<span data-ttu-id="3e43c-207">V následujícím příkladu hello, hello *položky obnovení služby SharePoint* omylem odstraněný a je třeba toobe obnovit.</span><span class="sxs-lookup"><span data-stu-id="3e43c-207">In hello following example, hello *Recovering SharePoint item* has been accidentally deleted and needs toobe recovered.</span></span>
<span data-ttu-id="3e43c-208">![Protection4 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)</span><span class="sxs-lookup"><span data-stu-id="3e43c-208">![MABS SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)</span></span>

1. <span data-ttu-id="3e43c-209">Otevřete hello **konzole správce aplikace DPM**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-209">Open hello **DPM Administrator Console**.</span></span> <span data-ttu-id="3e43c-210">Všechny farmy služby SharePoint, které jsou chráněné službou DPM se zobrazují v hello **ochrany** kartě.</span><span class="sxs-lookup"><span data-stu-id="3e43c-210">All SharePoint farms that are protected by DPM are shown in hello **Protection** tab.</span></span>

    ![Protection3 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection4.png)
2. <span data-ttu-id="3e43c-212">toobegin toorecover hello položku, vyberte hello **obnovení** kartě.</span><span class="sxs-lookup"><span data-stu-id="3e43c-212">toobegin toorecover hello item, select hello **Recovery** tab.</span></span>

    ![Protection5 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection6.png)
3. <span data-ttu-id="3e43c-214">Můžete hledat SharePoint pro *položky obnovení služby SharePoint* pomocí vyhledávání na základě zástupný znak v rámci obnovení bodu rozsahu.</span><span class="sxs-lookup"><span data-stu-id="3e43c-214">You can search SharePoint for *Recovering SharePoint item* by using a wildcard-based search within a recovery point range.</span></span>

    ![Protection6 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection7.png)
4. <span data-ttu-id="3e43c-216">Vyberte bod obnovení odpovídající hello z výsledků hledání hello, klikněte pravým tlačítkem na položku hello a pak vyberte **obnovit**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-216">Select hello appropriate recovery point from hello search results, right-click hello item, and then select **Recover**.</span></span>
5. <span data-ttu-id="3e43c-217">Můžete také procházet různé body obnovení a vyberte databázi nebo položky toorecover.</span><span class="sxs-lookup"><span data-stu-id="3e43c-217">You can also browse through various recovery points and select a database or item toorecover.</span></span> <span data-ttu-id="3e43c-218">Vyberte **datum > čas obnovení**a potom vyberte správné hello **databáze > farmy služby SharePoint > bod obnovení > položky**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-218">Select **Date > Recovery time**, and then select hello correct **Database > SharePoint farm > Recovery point > Item**.</span></span>

    ![Protection7 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection8.png)
6. <span data-ttu-id="3e43c-220">Klikněte pravým tlačítkem na položku hello a potom vyberte **obnovit** tooopen hello **Průvodce obnovením**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-220">Right-click hello item, and then select **Recover** tooopen hello **Recovery Wizard**.</span></span> <span data-ttu-id="3e43c-221">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-221">Click **Next**.</span></span>

    ![Revidovat výběr obnovení](./media/backup-azure-backup-sharepoint/review-recovery-selection.png)
7. <span data-ttu-id="3e43c-223">Vyberte typ hello obnovení má tooperform a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-223">Select hello type of recovery that you want tooperform, and then click **Next**.</span></span>

    ![Typ obnovení](./media/backup-azure-backup-sharepoint/select-recovery-type.png)

   > [!NOTE]
   > <span data-ttu-id="3e43c-225">Hello výběr **obnovit toooriginal** v hello příklad obnoví hello položky toohello původní web služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="3e43c-225">hello selection of **Recover toooriginal** in hello example recovers hello item toohello original SharePoint site.</span></span>
   >
   >
8. <span data-ttu-id="3e43c-226">Vyberte hello **proces obnovení** , které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="3e43c-226">Select hello **Recovery Process** that you want toouse.</span></span>

   * <span data-ttu-id="3e43c-227">Vyberte **obnovit bez použití farmy obnovení** Pokud nebylo změněno hello farmy služby SharePoint a je stejné jako obnovení hello bod, který je hello obnovena.</span><span class="sxs-lookup"><span data-stu-id="3e43c-227">Select **Recover without using a recovery farm** if hello SharePoint farm has not changed and is hello same as hello recovery point that is being restored.</span></span>
   * <span data-ttu-id="3e43c-228">Vyberte **obnovit použití farmy obnovení** Pokud od vytvoření bodu obnovení hello změnila hello farmy služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="3e43c-228">Select **Recover using a recovery farm** if hello SharePoint farm has changed since hello recovery point was created.</span></span>

     ![Proces obnovení](./media/backup-azure-backup-sharepoint/recovery-process.png)
9. <span data-ttu-id="3e43c-230">Dočasně zadat pracovní toorecover hello databázi umístění instance SQL serveru a poskytnout pracovní sdílené složky na MABS a hello serveru se systémem SharePoint toorecover hello položky.</span><span class="sxs-lookup"><span data-stu-id="3e43c-230">Provide a staging SQL Server instance location toorecover hello database temporarily, and provide a staging file share on MABS and hello server that's running SharePoint toorecover hello item.</span></span>

    ![Pracovní Location1](./media/backup-azure-backup-sharepoint/staging-location1.png)

    <span data-ttu-id="3e43c-232">MABS připojí hello databáze obsahu, který je hostitelem hello SharePoint položky toohello dočasné instance systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3e43c-232">MABS attaches hello content database that is hosting hello SharePoint item toohello temporary SQL Server instance.</span></span> <span data-ttu-id="3e43c-233">Z databáze obsahu hello obnoví hello položky a vloží ho hello pracovní umístění souboru na MABS.</span><span class="sxs-lookup"><span data-stu-id="3e43c-233">From hello content database, it recovers hello item and puts it on hello staging file location on MABS.</span></span> <span data-ttu-id="3e43c-234">Hello obnovit položku, která je na hello pracovního umístění teď toohello toobe exportovat potřeb pracovního umístění na hello farmy služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="3e43c-234">hello recovered item that's on hello staging location now needs toobe exported toohello staging location on hello SharePoint farm.</span></span>

    ![Pracovní Location2](./media/backup-azure-backup-sharepoint/staging-location2.png)
10. <span data-ttu-id="3e43c-236">Vyberte **nastavte možnosti obnovení**a použít toohello nastavení zabezpečení farmy služby SharePoint nebo použít nastavení zabezpečení hello hello bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="3e43c-236">Select **Specify recovery options**, and apply security settings toohello SharePoint farm or apply hello security settings of hello recovery point.</span></span> <span data-ttu-id="3e43c-237">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-237">Click **Next**.</span></span>

    ![Možnosti obnovení](./media/backup-azure-backup-sharepoint/recovery-options.png)

    > [!NOTE]
    > <span data-ttu-id="3e43c-239">Můžete zvolit využití šířky pásma sítě toothrottle hello.</span><span class="sxs-lookup"><span data-stu-id="3e43c-239">You can choose toothrottle hello network bandwidth usage.</span></span> <span data-ttu-id="3e43c-240">Tím se minimalizují dopad toohello provozním serveru během pracovní doby.</span><span class="sxs-lookup"><span data-stu-id="3e43c-240">This minimizes impact toohello production server during production hours.</span></span>
    >
    >
11. <span data-ttu-id="3e43c-241">Zkontrolujte hello souhrnné informace a pak klikněte na tlačítko **obnovit** toobegin obnovení souboru hello.</span><span class="sxs-lookup"><span data-stu-id="3e43c-241">Review hello summary information, and then click **Recover** toobegin recovery of hello file.</span></span>

    ![Obnovení souhrn](./media/backup-azure-backup-sharepoint/recovery-summary.png)
12. <span data-ttu-id="3e43c-243">Nyní vyberte hello **monitorování** ve hello **konzoly pro správu MABS** tooview hello **stav** hello obnovení.</span><span class="sxs-lookup"><span data-stu-id="3e43c-243">Now select hello **Monitoring** tab in hello **MABS Administrator Console** tooview hello **Status** of hello recovery.</span></span>

    ![Stav obnovení](./media/backup-azure-backup-sharepoint/recovery-monitoring.png)

    > [!NOTE]
    > <span data-ttu-id="3e43c-245">soubor Hello je nyní obnovit.</span><span class="sxs-lookup"><span data-stu-id="3e43c-245">hello file is now restored.</span></span> <span data-ttu-id="3e43c-246">Můžete obnovit hello SharePoint lokality toocheck hello obnovit soubor.</span><span class="sxs-lookup"><span data-stu-id="3e43c-246">You can refresh hello SharePoint site toocheck hello restored file.</span></span>
    >
    >

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a><span data-ttu-id="3e43c-247">Obnovení databáze služby SharePoint z Azure pomocí aplikace DPM</span><span class="sxs-lookup"><span data-stu-id="3e43c-247">Restore a SharePoint database from Azure by using DPM</span></span>
1. <span data-ttu-id="3e43c-248">toorecover databázi obsahu služby SharePoint, procházet různé body obnovení (jak je uvedeno výše) a vyberte bod obnovení hello, které chcete toorestore.</span><span class="sxs-lookup"><span data-stu-id="3e43c-248">toorecover a SharePoint content database, browse through various recovery points (as shown previously), and select hello recovery point that you want toorestore.</span></span>

    ![Protection8 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. <span data-ttu-id="3e43c-250">Dvakrát klikněte na hello SharePoint bodu tooshow hello k dispozici SharePoint katalogu informace pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="3e43c-250">Double-click hello SharePoint recovery point tooshow hello available SharePoint catalog information.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3e43c-251">Protože pro dlouhodobé uchovávání v Azure je chráněn hello farmy služby SharePoint, nejsou dostupné na MABS žádné informace katalogu (metadata).</span><span class="sxs-lookup"><span data-stu-id="3e43c-251">Because hello SharePoint farm is protected for long-term retention in Azure, no catalog information (metadata) is available on MABS.</span></span> <span data-ttu-id="3e43c-252">V důsledku toho vždy, když databáze obsahu služby SharePoint v daném okamžiku je toobe obnovit, musíte farmy služby SharePoint hello toocatalog znovu.</span><span class="sxs-lookup"><span data-stu-id="3e43c-252">As a result, whenever a point-in-time SharePoint content database needs toobe recovered, you need toocatalog hello SharePoint farm again.</span></span>
   >
   >
3. <span data-ttu-id="3e43c-253">Klikněte na tlačítko **opětovného zařazení do katalogu**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-253">Click **Re-catalog**.</span></span>

    ![Protection10 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)

    <span data-ttu-id="3e43c-255">Hello **provést novou katalogizaci cloudu** otevře se okno stav.</span><span class="sxs-lookup"><span data-stu-id="3e43c-255">hello **Cloud Recatalog** status window opens.</span></span>

    ![Protection11 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)

    <span data-ttu-id="3e43c-257">Po dokončení katalogizaci hello stav změní příliš*úspěch*.</span><span class="sxs-lookup"><span data-stu-id="3e43c-257">After cataloging is finished, hello status changes too*Success*.</span></span> <span data-ttu-id="3e43c-258">Klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-258">Click **Close**.</span></span>

    ![Protection12 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. <span data-ttu-id="3e43c-260">Klikněte na objekt služby SharePoint hello ukazuje hello MABS **obnovení** kartě struktura databáze obsahu tooget hello.</span><span class="sxs-lookup"><span data-stu-id="3e43c-260">Click hello SharePoint object shown in hello MABS **Recovery** tab tooget hello content database structure.</span></span> <span data-ttu-id="3e43c-261">Klikněte pravým tlačítkem na položku hello a pak klikněte na **obnovit**.</span><span class="sxs-lookup"><span data-stu-id="3e43c-261">Right-click hello item, and then click **Recover**.</span></span>

    ![Protection13 MABS služby SharePoint](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. <span data-ttu-id="3e43c-263">V tomto okamžiku postupujte podle hello [kroky obnovení dříve v tomto článku](#restore-a-sharepoint-item-from-disk-using-dpm) toorecover databázi obsahu služby SharePoint z disku.</span><span class="sxs-lookup"><span data-stu-id="3e43c-263">At this point, follow hello [recovery steps earlier in this article](#restore-a-sharepoint-item-from-disk-using-dpm) toorecover a SharePoint content database from disk.</span></span>

## <a name="faqs"></a><span data-ttu-id="3e43c-264">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="3e43c-264">FAQs</span></span>
<span data-ttu-id="3e43c-265">Otázka: je možné obnovit původní umístění toohello položky služby SharePoint, pokud je služba SharePoint nakonfigurována pomocí technologie AlwaysOn serveru SQL (s ochrany na disku)?</span><span class="sxs-lookup"><span data-stu-id="3e43c-265">Q: Can I recover a SharePoint item toohello original location if SharePoint is configured by using SQL AlwaysOn (with protection on disk)?</span></span><br>
<span data-ttu-id="3e43c-266">Odpověď: Ano hello položka může být obnovené toohello původní web služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="3e43c-266">A: Yes, hello item can be recovered toohello original SharePoint site.</span></span>

<span data-ttu-id="3e43c-267">Otázka: je možné obnovit původní umístění toohello databáze služby SharePoint, pokud je služba SharePoint nakonfigurována pomocí technologie AlwaysOn serveru SQL?</span><span class="sxs-lookup"><span data-stu-id="3e43c-267">Q: Can I recover a SharePoint database toohello original location if SharePoint is configured by using SQL AlwaysOn?</span></span><br>
<span data-ttu-id="3e43c-268">A:, protože databáze služby SharePoint jsou konfigurované v SQL AlwaysOn, je nelze změnit, pokud je skupina dostupnosti hello odebrána.</span><span class="sxs-lookup"><span data-stu-id="3e43c-268">A: Because SharePoint databases are configured in SQL AlwaysOn, they cannot be modified unless hello availability group is removed.</span></span> <span data-ttu-id="3e43c-269">V důsledku toho MABS nelze obnovit původní umístění toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="3e43c-269">As a result, MABS cannot restore a database toohello original location.</span></span> <span data-ttu-id="3e43c-270">Můžete obnovit instanci systému SQL Server tooanother databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3e43c-270">You can recover a SQL Server database tooanother SQL Server instance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e43c-271">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3e43c-271">Next steps</span></span>
* <span data-ttu-id="3e43c-272">Další informace o MABS ochrany služby SharePoint – viz [řady Video - DPM ochrany služby SharePoint](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)</span><span class="sxs-lookup"><span data-stu-id="3e43c-272">Learn more about MABS Protection of SharePoint - see [Video Series - DPM Protection of SharePoint](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)</span></span>
