---
title: "aaaAutomated v2 zálohování pro SQL Server 2016 Azure Virtual Machines | Microsoft Docs"
description: "Vysvětluje funkci hello automatizované zálohování pro SQL Server 2016 virtuální počítače spuštěné v Azure. Tento článek je konkrétní tooVMs pomocí hello Resource Manager."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: ebd23868-821c-475b-b867-06d4a2e310c7
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/05/2017
ms.author: jroth
ms.openlocfilehash: a322792fb22c76bfa74fafb711b8b1927a6e2b3a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automated-backup-v2-for-sql-server-2016-azure-virtual-machines-resource-manager"></a><span data-ttu-id="12c4a-104">Automatizované zálohování v2 pro SQL Server 2016 virtuální počítače Azure (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="12c4a-104">Automated Backup v2 for SQL Server 2016 Azure Virtual Machines (Resource Manager)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="12c4a-105">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="12c4a-105">SQL Server 2014</span></span>](virtual-machines-windows-sql-automated-backup.md)
> * [<span data-ttu-id="12c4a-106">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="12c4a-106">SQL Server 2016</span></span>](virtual-machines-windows-sql-automated-backup-v2.md)

<span data-ttu-id="12c4a-107">Automatizované zálohování v2 automaticky nakonfiguruje [tooMicrosoft spravovaného zálohování Azure](https://msdn.microsoft.com/library/dn449496.aspx) pro všechny stávající a nové databáze na virtuální počítač Azure běžet tyto edice SQL Server 2016 Standard, Enterprise nebo Developer.</span><span class="sxs-lookup"><span data-stu-id="12c4a-107">Automated Backup v2 automatically configures [Managed Backup tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) for all existing and new databases on an Azure VM running SQL Server 2016 Standard, Enterprise, or Developer editions.</span></span> <span data-ttu-id="12c4a-108">To vám umožní tooconfigure standardní databázi zálohování, které využívají služby odolné Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="12c4a-108">This enables you tooconfigure regular database backups that utilize durable Azure blob storage.</span></span> <span data-ttu-id="12c4a-109">Automatizované zálohování v2 závisí na hello [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="12c4a-109">Automated Backup v2 depends on hello [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="12c4a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="12c4a-110">Prerequisites</span></span>
<span data-ttu-id="12c4a-111">toouse v2 automatizovaného zálohování, projděte si hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="12c4a-111">toouse Automated Backup v2, review hello following prerequisites:</span></span>

<span data-ttu-id="12c4a-112">**Operační systém**:</span><span class="sxs-lookup"><span data-stu-id="12c4a-112">**Operating System**:</span></span>

- <span data-ttu-id="12c4a-113">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="12c4a-113">Windows Server 2012 R2</span></span>
- <span data-ttu-id="12c4a-114">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="12c4a-114">Windows Server 2016</span></span>

<span data-ttu-id="12c4a-115">**Verzi nebo edici systému SQL Server**:</span><span class="sxs-lookup"><span data-stu-id="12c4a-115">**SQL Server version/edition**:</span></span>

- <span data-ttu-id="12c4a-116">SQL Server 2016 Standard</span><span class="sxs-lookup"><span data-stu-id="12c4a-116">SQL Server 2016 Standard</span></span>
- <span data-ttu-id="12c4a-117">SQL Server 2016 Enterprise</span><span class="sxs-lookup"><span data-stu-id="12c4a-117">SQL Server 2016 Enterprise</span></span>
- <span data-ttu-id="12c4a-118">SQL Server 2016 Developer</span><span class="sxs-lookup"><span data-stu-id="12c4a-118">SQL Server 2016 Developer</span></span>

> [!IMPORTANT]
> <span data-ttu-id="12c4a-119">Automatizované zálohování v2 spolupracuje se službou SQL Server 2016.</span><span class="sxs-lookup"><span data-stu-id="12c4a-119">Automated Backup v2 works with SQL Server 2016.</span></span> <span data-ttu-id="12c4a-120">Pokud používáte SQL Server 2014, můžete použít tooback v1 automatizovaného zálohování do své databáze.</span><span class="sxs-lookup"><span data-stu-id="12c4a-120">If you are using SQL Server 2014, you can use Automated Backup v1 tooback up your databases.</span></span> <span data-ttu-id="12c4a-121">Další informace najdete v tématu [automatizované zálohování pro SQL Server 2014 virtuální počítače Azure](virtual-machines-windows-sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="12c4a-121">For more information, see [Automated Backup for SQL Server 2014 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).</span></span>

<span data-ttu-id="12c4a-122">**Konfigurace databáze**:</span><span class="sxs-lookup"><span data-stu-id="12c4a-122">**Database configuration**:</span></span>

- <span data-ttu-id="12c4a-123">Cílové databáze musí mít hello úplném modelu obnovení.</span><span class="sxs-lookup"><span data-stu-id="12c4a-123">Target databases must use hello full recovery model.</span></span> <span data-ttu-id="12c4a-124">Další informace o dopadu hello hello úplném modelu obnovení na zálohování najdete v tématu [zálohování pod hello úplný Model obnovení](https://technet.microsoft.com/library/ms190217.aspx).</span><span class="sxs-lookup"><span data-stu-id="12c4a-124">For more information about hello impact of hello full recovery model on backups, see [Backup Under hello Full Recovery Model](https://technet.microsoft.com/library/ms190217.aspx).</span></span>
- <span data-ttu-id="12c4a-125">Systémové databáze nemají toouse úplném modelu obnovení.</span><span class="sxs-lookup"><span data-stu-id="12c4a-125">System databases do not have toouse full recovery model.</span></span> <span data-ttu-id="12c4a-126">Pokud budete potřebovat toobe zálohy protokolu pro Model, nebo databázi MSDB, ale musí používat úplném modelu obnovení.</span><span class="sxs-lookup"><span data-stu-id="12c4a-126">However, if you require log backups toobe taken for Model or MSDB, you must use full recovery model.</span></span>
- <span data-ttu-id="12c4a-127">Cílové databáze musí být na hello výchozí instanci SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="12c4a-127">Target databases must be on hello default SQL Server instance.</span></span> <span data-ttu-id="12c4a-128">Hello IaaS rozšíření systému SQL Server nepodporuje pojmenované instance.</span><span class="sxs-lookup"><span data-stu-id="12c4a-128">hello SQL Server IaaS Extension does not support named instances.</span></span>

<span data-ttu-id="12c4a-129">**Model nasazení Azure**:</span><span class="sxs-lookup"><span data-stu-id="12c4a-129">**Azure deployment model**:</span></span>

- <span data-ttu-id="12c4a-130">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="12c4a-130">Resource Manager</span></span>

> [!NOTE]
> <span data-ttu-id="12c4a-131">Automatizované zálohování spoléhá na hello **rozšíření agenta systému SQL Server IaaS**.</span><span class="sxs-lookup"><span data-stu-id="12c4a-131">Automated Backup relies on hello **SQL Server IaaS Agent Extension**.</span></span> <span data-ttu-id="12c4a-132">Aktuální SQL bitové kopie virtuálních počítačů Galerie přidejte toto rozšíření ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="12c4a-132">Current SQL virtual machine gallery images add this extension by default.</span></span> <span data-ttu-id="12c4a-133">Další informace najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="12c4a-133">For more information, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

## <a name="settings"></a><span data-ttu-id="12c4a-134">Nastavení</span><span class="sxs-lookup"><span data-stu-id="12c4a-134">Settings</span></span>
<span data-ttu-id="12c4a-135">Hello následující tabulka popisuje možnosti hello, které mohou být konfigurovány pro automatizované zálohování v2.</span><span class="sxs-lookup"><span data-stu-id="12c4a-135">hello following table describes hello options that can be configured for Automated Backup v2.</span></span> <span data-ttu-id="12c4a-136">kroky skutečné konfigurace Hello lišit v závislosti na tom, zda používáte hello portál Azure nebo příkazů prostředí Windows PowerShell pro Azure.</span><span class="sxs-lookup"><span data-stu-id="12c4a-136">hello actual configuration steps vary depending on whether you use hello Azure portal or Azure Windows PowerShell commands.</span></span>

### <a name="basic-settings"></a><span data-ttu-id="12c4a-137">Základní nastavení</span><span class="sxs-lookup"><span data-stu-id="12c4a-137">Basic Settings</span></span>

| <span data-ttu-id="12c4a-138">Nastavení</span><span class="sxs-lookup"><span data-stu-id="12c4a-138">Setting</span></span> | <span data-ttu-id="12c4a-139">Rozsah (výchozí)</span><span class="sxs-lookup"><span data-stu-id="12c4a-139">Range (Default)</span></span> | <span data-ttu-id="12c4a-140">Popis</span><span class="sxs-lookup"><span data-stu-id="12c4a-140">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="12c4a-141">**Automatizované zálohování**</span><span class="sxs-lookup"><span data-stu-id="12c4a-141">**Automated Backup**</span></span> | <span data-ttu-id="12c4a-142">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="12c4a-142">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="12c4a-143">Povolí nebo zakáže automatizované zálohování pro virtuální počítač Azure SQL Server 2016 Standard nebo Enterprise.</span><span class="sxs-lookup"><span data-stu-id="12c4a-143">Enables or disables Automated Backup for an Azure VM running SQL Server 2016 Standard or Enterprise.</span></span> |
| <span data-ttu-id="12c4a-144">**Doba uchování dat**</span><span class="sxs-lookup"><span data-stu-id="12c4a-144">**Retention Period**</span></span> | <span data-ttu-id="12c4a-145">1 až 30 dní (30 dní)</span><span class="sxs-lookup"><span data-stu-id="12c4a-145">1-30 days (30 days)</span></span> | <span data-ttu-id="12c4a-146">Hello počet dní tooretain záloh.</span><span class="sxs-lookup"><span data-stu-id="12c4a-146">hello number of days tooretain backups.</span></span> |
| <span data-ttu-id="12c4a-147">**Účet úložiště**</span><span class="sxs-lookup"><span data-stu-id="12c4a-147">**Storage Account**</span></span> | <span data-ttu-id="12c4a-148">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="12c4a-148">Azure storage account</span></span> | <span data-ttu-id="12c4a-149">Toouse účet úložiště Azure pro ukládání souborů automatizovaného zálohování do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="12c4a-149">An Azure storage account toouse for storing Automated Backup files in blob storage.</span></span> <span data-ttu-id="12c4a-150">Kontejner se vytvoří v této toostore umístění všechny záložní soubory.</span><span class="sxs-lookup"><span data-stu-id="12c4a-150">A container is created at this location toostore all backup files.</span></span> <span data-ttu-id="12c4a-151">záložní soubor Hello zásady vytváření názvů zahrnuje hello date, time a GUID databáze.</span><span class="sxs-lookup"><span data-stu-id="12c4a-151">hello backup file naming convention includes hello date, time, and database GUID.</span></span> |
| <span data-ttu-id="12c4a-152">**Šifrování**</span><span class="sxs-lookup"><span data-stu-id="12c4a-152">**Encryption**</span></span> |<span data-ttu-id="12c4a-153">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="12c4a-153">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="12c4a-154">Povolí nebo zakáže šifrování.</span><span class="sxs-lookup"><span data-stu-id="12c4a-154">Enables or disables encryption.</span></span> <span data-ttu-id="12c4a-155">Když je povolené šifrování, hello certifikátů používaných toorestore hello zálohování jsou umístěné v hello zadaný účet úložiště v hello stejné **automaticbackup** kontejneru pomocí hello stejné zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="12c4a-155">When encryption is enabled, hello certificates used toorestore hello backup are located in hello specified storage account in hello same **automaticbackup** container using hello same naming convention.</span></span> <span data-ttu-id="12c4a-156">Pokud se změní heslo hello, se toto heslo se vygeneruje nový certifikát, ale starý certifikát hello zůstane toorestore předchozí zálohy.</span><span class="sxs-lookup"><span data-stu-id="12c4a-156">If hello password changes, a new certificate is generated with that password, but hello old certificate remains toorestore prior backups.</span></span> |
| <span data-ttu-id="12c4a-157">**Heslo**</span><span class="sxs-lookup"><span data-stu-id="12c4a-157">**Password**</span></span> |<span data-ttu-id="12c4a-158">Heslo</span><span class="sxs-lookup"><span data-stu-id="12c4a-158">Password text</span></span> | <span data-ttu-id="12c4a-159">Heslo pro šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="12c4a-159">A password for encryption keys.</span></span> <span data-ttu-id="12c4a-160">Toto je pouze vyžaduje, pokud je povolené šifrování.</span><span class="sxs-lookup"><span data-stu-id="12c4a-160">This is only required if encryption is enabled.</span></span> <span data-ttu-id="12c4a-161">V pořadí toorestore šifrované zálohování, musíte mít hello správné heslo a související certifikátu, který byl použit v době hello hello zálohy.</span><span class="sxs-lookup"><span data-stu-id="12c4a-161">In order toorestore an encrypted backup, you must have hello correct password and related certificate that was used at hello time hello backup was taken.</span></span> |

### <a name="advanced-settings"></a><span data-ttu-id="12c4a-162">Upřesnit nastavení</span><span class="sxs-lookup"><span data-stu-id="12c4a-162">Advanced Settings</span></span>

| <span data-ttu-id="12c4a-163">Nastavení</span><span class="sxs-lookup"><span data-stu-id="12c4a-163">Setting</span></span> | <span data-ttu-id="12c4a-164">Rozsah (výchozí)</span><span class="sxs-lookup"><span data-stu-id="12c4a-164">Range (Default)</span></span> | <span data-ttu-id="12c4a-165">Popis</span><span class="sxs-lookup"><span data-stu-id="12c4a-165">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="12c4a-166">**Zálohování databáze systému**</span><span class="sxs-lookup"><span data-stu-id="12c4a-166">**System Database Backups**</span></span> | <span data-ttu-id="12c4a-167">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="12c4a-167">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="12c4a-168">Když je povolené, tato funkce bude také zálohování databází systému hello: hlavní server, databázi MSDB a modelu.</span><span class="sxs-lookup"><span data-stu-id="12c4a-168">When enabled, this feature will also back up hello system databases: Master, MSDB, and Model.</span></span> <span data-ttu-id="12c4a-169">Hello databázi MSDB a databáze modelu ověřte, že jsou v režimu úplného obnovení, pokud chcete, aby toobe zálohy protokolu prováděné.</span><span class="sxs-lookup"><span data-stu-id="12c4a-169">For hello MSDB and Model databases, verify that they are in full recovery mode if you want log backups toobe taken.</span></span> <span data-ttu-id="12c4a-170">Zálohy protokolů se nikdy provádějí pro hlavní server.</span><span class="sxs-lookup"><span data-stu-id="12c4a-170">Log backups are never taken for Master.</span></span> <span data-ttu-id="12c4a-171">A jsou provedeny žádné zálohy pro databázi TempDB.</span><span class="sxs-lookup"><span data-stu-id="12c4a-171">And no backups are taken for TempDB.</span></span> |
| <span data-ttu-id="12c4a-172">**Plán zálohování**</span><span class="sxs-lookup"><span data-stu-id="12c4a-172">**Backup Schedule**</span></span> | <span data-ttu-id="12c4a-173">Ruční nebo automatické (Automated)</span><span class="sxs-lookup"><span data-stu-id="12c4a-173">Manual/Automated (Automated)</span></span> | <span data-ttu-id="12c4a-174">Ve výchozím nastavení plán zálohování hello automaticky určí založené na protokolu růst hello.</span><span class="sxs-lookup"><span data-stu-id="12c4a-174">By default, hello backup schedule will be automatically determined based on hello log growth.</span></span> <span data-ttu-id="12c4a-175">Ruční plán zálohování umožňuje hello uživatele toospecify hello časové okno zálohování.</span><span class="sxs-lookup"><span data-stu-id="12c4a-175">Manual backup schedule allows hello user toospecify hello time window for backups.</span></span> <span data-ttu-id="12c4a-176">V takovém případě bude vždy jen trvat zálohy na hello zadané frekvence a během hello zadaný časový interval daný den.</span><span class="sxs-lookup"><span data-stu-id="12c4a-176">In this case, backups will only ever take place at hello specified frequency and during hello specified time window of a given day.</span></span> |
| <span data-ttu-id="12c4a-177">**Četnost úplné zálohování**</span><span class="sxs-lookup"><span data-stu-id="12c4a-177">**Full backup frequency**</span></span> | <span data-ttu-id="12c4a-178">Denně nebo týdně</span><span class="sxs-lookup"><span data-stu-id="12c4a-178">Daily/Weekly</span></span> | <span data-ttu-id="12c4a-179">Četnost úplné zálohy.</span><span class="sxs-lookup"><span data-stu-id="12c4a-179">Frequency of full backups.</span></span> <span data-ttu-id="12c4a-180">V obou případech úplné zálohování bude zahájena při dalším plánovaném čase intervalu hello.</span><span class="sxs-lookup"><span data-stu-id="12c4a-180">In both cases, full backups will begin during hello next scheduled time window.</span></span> <span data-ttu-id="12c4a-181">Pokud je vybraná týdně, zálohování může zahrnovat více dní, dokud všechny databáze úspěšně zálohovali.</span><span class="sxs-lookup"><span data-stu-id="12c4a-181">When weekly is selected, backups could span multiple days until all databases have successfully backed up.</span></span> |
| <span data-ttu-id="12c4a-182">**Čas spuštění úplného zálohování**</span><span class="sxs-lookup"><span data-stu-id="12c4a-182">**Full backup start time**</span></span> | <span data-ttu-id="12c4a-183">00:00 – 23:00 (01:00)</span><span class="sxs-lookup"><span data-stu-id="12c4a-183">00:00 – 23:00 (01:00)</span></span> | <span data-ttu-id="12c4a-184">Počáteční čas daný den, během které úplné zálohování lze provést.</span><span class="sxs-lookup"><span data-stu-id="12c4a-184">Start time of a given day during which full backups can take place.</span></span> |
| <span data-ttu-id="12c4a-185">**Úplné zálohování časový interval**</span><span class="sxs-lookup"><span data-stu-id="12c4a-185">**Full backup time window**</span></span> | <span data-ttu-id="12c4a-186">1 – 23 hodin (1 hodina)</span><span class="sxs-lookup"><span data-stu-id="12c4a-186">1 – 23 hours (1 hour)</span></span> | <span data-ttu-id="12c4a-187">Doba trvání hello časové okno daný den, během které úplné zálohování lze provést.</span><span class="sxs-lookup"><span data-stu-id="12c4a-187">Duration of hello time window of a given day during which full backups can take place.</span></span> |
| <span data-ttu-id="12c4a-188">**Četnost záloh protokolu**</span><span class="sxs-lookup"><span data-stu-id="12c4a-188">**Log backup frequency**</span></span> | <span data-ttu-id="12c4a-189">5 – 60 minut (60 minut)</span><span class="sxs-lookup"><span data-stu-id="12c4a-189">5 – 60 minutes (60 minutes)</span></span> | <span data-ttu-id="12c4a-190">Četnost záloh protokolu.</span><span class="sxs-lookup"><span data-stu-id="12c4a-190">Frequency of log backups.</span></span> |

## <a name="understanding-full-backup-frequency"></a><span data-ttu-id="12c4a-191">Principy četnost úplné zálohování</span><span class="sxs-lookup"><span data-stu-id="12c4a-191">Understanding full backup frequency</span></span>
<span data-ttu-id="12c4a-192">Je důležité toounderstand hello rozdíl mezi denní nebo týdenní úplné zálohování.</span><span class="sxs-lookup"><span data-stu-id="12c4a-192">It is important toounderstand hello difference between daily and weekly full backups.</span></span> <span data-ttu-id="12c4a-193">V této snahy můžeme provede dva ukázkové scénáře.</span><span class="sxs-lookup"><span data-stu-id="12c4a-193">In this effort, we will walk through two example scenarios.</span></span>

### <a name="scenario-1-weekly-backups"></a><span data-ttu-id="12c4a-194">Scénář 1: Týdenní zálohy</span><span class="sxs-lookup"><span data-stu-id="12c4a-194">Scenario 1: Weekly backups</span></span>
<span data-ttu-id="12c4a-195">Máte virtuální počítač na serveru SQL, který obsahuje počet velmi velké databáze.</span><span class="sxs-lookup"><span data-stu-id="12c4a-195">You have a SQL Server VM which contains a number of very large databases.</span></span>

<span data-ttu-id="12c4a-196">V pondělí povolíte automatizované zálohování v2 hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="12c4a-196">On Monday, you enable Automated Backup v2 with hello following settings:</span></span>

- <span data-ttu-id="12c4a-197">Plán zálohování: **ruční**</span><span class="sxs-lookup"><span data-stu-id="12c4a-197">Backup schedule: **Manual**</span></span>
- <span data-ttu-id="12c4a-198">Úplné četnost zálohování: **týdně**</span><span class="sxs-lookup"><span data-stu-id="12c4a-198">Full backup frequency: **Weekly**</span></span>
- <span data-ttu-id="12c4a-199">Úplné zálohování počáteční čas: **01:00**</span><span class="sxs-lookup"><span data-stu-id="12c4a-199">Full backup start time: **01:00**</span></span>
- <span data-ttu-id="12c4a-200">Úplné zálohování časové okno: **1 hodina**</span><span class="sxs-lookup"><span data-stu-id="12c4a-200">Full backup time window: **1 hour**</span></span>

<span data-ttu-id="12c4a-201">To znamená, že tento hello další dostupné intervalu zálohování bude úterý v 1: 00 1 hodinu.</span><span class="sxs-lookup"><span data-stu-id="12c4a-201">This means that hello next available backup window is Tuesday at 1 AM for 1 hour.</span></span> <span data-ttu-id="12c4a-202">V té době zahájíte automatizovaného zálohování, zálohování databází jeden najednou.</span><span class="sxs-lookup"><span data-stu-id="12c4a-202">At that time, Automated Backup will begin backing up your databases one at a time.</span></span> <span data-ttu-id="12c4a-203">V tomto scénáři jsou dostatečně velké na to, že úplné zálohy dokončí hello první několik databází vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="12c4a-203">In this scenario, your databases are large enough that full backups will complete for hello first couple databases.</span></span> <span data-ttu-id="12c4a-204">Ale po jedné hodině všechny hello databáze byly zálohovány.</span><span class="sxs-lookup"><span data-stu-id="12c4a-204">However, after one hour not all of hello databases have been backed up.</span></span>

<span data-ttu-id="12c4a-205">V takovém případě automatizovaného zálohování bude zahájeno zálohování hello zbývající databáze hello další den, středu v 1: 00 1 hodinu.</span><span class="sxs-lookup"><span data-stu-id="12c4a-205">When this happens, Automated Backup will begin backing up hello remaining databases hello next day, Wednesday at 1 AM for 1 hour.</span></span> <span data-ttu-id="12c4a-206">Pokud nejsou všechny databáze byly zálohovány v tento čas, se pokusí znovu hello další den v hello stejný čas.</span><span class="sxs-lookup"><span data-stu-id="12c4a-206">If not all databases have been backed up in that time, it will try again hello next day at hello same time.</span></span> <span data-ttu-id="12c4a-207">To bude pokračovat, dokud všechny databáze byly úspěšně zálohovány.</span><span class="sxs-lookup"><span data-stu-id="12c4a-207">This will continue until all databases have been successfully backed up.</span></span>

<span data-ttu-id="12c4a-208">Jakmile ho znovu dosáhne úterý, bude automatizovaného zálohování začít zálohovat všechny databáze ještě jednou.</span><span class="sxs-lookup"><span data-stu-id="12c4a-208">Once it reaches Tuesday again, Automated Backup will begin backing up all databases once again.</span></span>

<span data-ttu-id="12c4a-209">Tento scénář popisuje automatizovaného zálohování bude fungovat pouze v rámci hello zadané časové období, a každou databázi, budou zálohovány jednou za týden.</span><span class="sxs-lookup"><span data-stu-id="12c4a-209">This scenario shows that Automated Backup will only operate within hello specified time window, and each database will be backed up once per week.</span></span> <span data-ttu-id="12c4a-210">To také ukazuje, že je možné pro zálohování toospan více dní v hello případ Pokud to není možné toocomplete všechny zálohy za jeden den.</span><span class="sxs-lookup"><span data-stu-id="12c4a-210">This also shows that it is possible for backups toospan multiple days in hello case where it is not possible toocomplete all backups in a single day.</span></span>

### <a name="scenario-2-daily-backups"></a><span data-ttu-id="12c4a-211">Scénář 2: Denní zálohy</span><span class="sxs-lookup"><span data-stu-id="12c4a-211">Scenario 2: Daily backups</span></span>
<span data-ttu-id="12c4a-212">Máte virtuální počítač na serveru SQL, který obsahuje počet velmi velké databáze.</span><span class="sxs-lookup"><span data-stu-id="12c4a-212">You have a SQL Server VM which contains a number of very large databases.</span></span>

<span data-ttu-id="12c4a-213">V pondělí povolíte automatizované zálohování v2 hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="12c4a-213">On Monday, you enable Automated Backup v2 with hello following settings:</span></span>

- <span data-ttu-id="12c4a-214">Plán zálohování: ruční</span><span class="sxs-lookup"><span data-stu-id="12c4a-214">Backup schedule: Manual</span></span>
- <span data-ttu-id="12c4a-215">Úplných záloh četnost: denně</span><span class="sxs-lookup"><span data-stu-id="12c4a-215">Full backup frequency: Daily</span></span>
- <span data-ttu-id="12c4a-216">Úplné zálohování počáteční čas: 22:00</span><span class="sxs-lookup"><span data-stu-id="12c4a-216">Full backup start time: 22:00</span></span>
- <span data-ttu-id="12c4a-217">Úplné zálohování časové okno: 6 hodin</span><span class="sxs-lookup"><span data-stu-id="12c4a-217">Full backup time window: 6 hours</span></span>

<span data-ttu-id="12c4a-218">To znamená, že tento hello další dostupné intervalu zálohování bude pondělí v 22: 00 6 hodin.</span><span class="sxs-lookup"><span data-stu-id="12c4a-218">This means that hello next available backup window is Monday at 10 PM for 6 hours.</span></span> <span data-ttu-id="12c4a-219">V té době zahájíte automatizovaného zálohování, zálohování databází jeden najednou.</span><span class="sxs-lookup"><span data-stu-id="12c4a-219">At that time, Automated Backup will begin backing up your databases one at a time.</span></span>

<span data-ttu-id="12c4a-220">Pak úterý v 10 6 hodin, budou úplné zálohy všech databází spusťte znovu.</span><span class="sxs-lookup"><span data-stu-id="12c4a-220">Then, on Tuesday at 10 for 6 hours, full backups of all databases will start again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="12c4a-221">Při plánování denní zálohy, doporučujeme, abyste naplánovali tooensure okno celou dobu, všechny databáze může být zálohována během této doby.</span><span class="sxs-lookup"><span data-stu-id="12c4a-221">When scheduling daily backups, it is recommended that you schedule a wide time window tooensure all databases can be backed up within this time.</span></span> <span data-ttu-id="12c4a-222">To je obzvláště důležité v případě hello, kdy máte velké množství dat tooback nahoru.</span><span class="sxs-lookup"><span data-stu-id="12c4a-222">This is especially important in hello case where you have a large amount of data tooback up.</span></span>

## <a name="configuration-in-hello-portal"></a><span data-ttu-id="12c4a-223">Konfigurace v hello portálu</span><span class="sxs-lookup"><span data-stu-id="12c4a-223">Configuration in hello Portal</span></span>

<span data-ttu-id="12c4a-224">V2 automatizovaného zálohování Azure portálu tooconfigure hello můžete použít při zřizování nebo pro existující SQL Server 2016 virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="12c4a-224">You can use hello Azure portal tooconfigure Automated Backup v2 during provisioning or for existing SQL Server 2016 VMs.</span></span>

### <a name="new-vms"></a><span data-ttu-id="12c4a-225">Nové virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="12c4a-225">New VMs</span></span>

<span data-ttu-id="12c4a-226">Použijte v2 automatizovaného zálohování Azure portálu tooconfigure hello při vytváření nového SQL serveru 2016 virtuálního počítače v modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="12c4a-226">Use hello Azure portal tooconfigure Automated Backup v2 when you create a new SQL Server 2016 Virtual Machine in hello Resource Manager deployment model.</span></span> 

<span data-ttu-id="12c4a-227">V hello **nastavení systému SQL Server** vyberte **automatizované zálohování**.</span><span class="sxs-lookup"><span data-stu-id="12c4a-227">In hello **SQL Server settings** blade, select **Automated backup**.</span></span> <span data-ttu-id="12c4a-228">Hello následující Azure portálu snímek obrazovky ukazuje hello **automatizované zálohování SQL** okno.</span><span class="sxs-lookup"><span data-stu-id="12c4a-228">hello following Azure portal screenshot shows hello **SQL Automated Backup** blade.</span></span>

![Konfigurace automatického zálohování SQL na portálu Azure](./media/virtual-machines-windows-sql-automated-backup-v2/automated-backup-blade.png)

> [!NOTE]
> <span data-ttu-id="12c4a-230">Automatizované zálohování v2 ve výchozím nastavení vypnutá.</span><span class="sxs-lookup"><span data-stu-id="12c4a-230">Automated Backup v2 is disabled by default.</span></span>

<span data-ttu-id="12c4a-231">Kontext, tématu hello dokončení na [zřizování virtuálního počítače s SQL serverem v Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="12c4a-231">For context, see hello complete topic on [provisioning a SQL Server virtual machine in Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

### <a name="existing-vms"></a><span data-ttu-id="12c4a-232">Existující virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="12c4a-232">Existing VMs</span></span>

<span data-ttu-id="12c4a-233">Pro existující virtuální počítače systému SQL Server vyberte virtuální počítač systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="12c4a-233">For existing SQL Server virtual machines, select your SQL Server virtual machine.</span></span> <span data-ttu-id="12c4a-234">Potom vyberte hello **konfigurace systému SQL Server** části hello **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="12c4a-234">Then select hello **SQL Server configuration** section of hello **Settings** blade.</span></span>

![Automatizované zálohování SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration.png)

<span data-ttu-id="12c4a-236">V hello **konfigurace systému SQL Server** okně klikněte na tlačítko hello **upravit** tlačítka na hello automatizované zálohování části.</span><span class="sxs-lookup"><span data-stu-id="12c4a-236">In hello **SQL Server configuration** blade, click hello **Edit** button in hello Automated backup section.</span></span>

![Konfigurace automatického zálohování SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration-edit.png)

<span data-ttu-id="12c4a-238">Po dokončení klikněte na tlačítko hello **OK** na konci hello hello tlačítko **konfigurace systému SQL Server** okno toosave změny.</span><span class="sxs-lookup"><span data-stu-id="12c4a-238">When finished, click hello **OK** button on hello bottom of hello **SQL Server configuration** blade toosave your changes.</span></span>

<span data-ttu-id="12c4a-239">Pokud povolíte automatizované zálohování pro hello poprvé, Azure automaticky nakonfiguruje hello IaaS agenta systému SQL Server hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="12c4a-239">If you are enabling Automated Backup for hello first time, Azure configures hello SQL Server IaaS Agent in hello background.</span></span> <span data-ttu-id="12c4a-240">Během této doby se nemusí zobrazit hello portál Azure, automatizované zálohování je nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="12c4a-240">During this time, hello Azure portal might not show that Automated Backup is configured.</span></span> <span data-ttu-id="12c4a-241">Počkejte několik minut, než toobe hello agenta nainstalovat, nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="12c4a-241">Wait several minutes for hello agent toobe installed, configured.</span></span> <span data-ttu-id="12c4a-242">Po této hello Azure bude odrážet portál hello nové nastavení.</span><span class="sxs-lookup"><span data-stu-id="12c4a-242">After that hello Azure portal will reflect hello new settings.</span></span>

## <a name="configuration-with-powershell"></a><span data-ttu-id="12c4a-243">Konfigurace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="12c4a-243">Configuration with PowerShell</span></span>

<span data-ttu-id="12c4a-244">Pomocí prostředí PowerShell tooconfigure v2 automatizovaného zálohování.</span><span class="sxs-lookup"><span data-stu-id="12c4a-244">You can use PowerShell tooconfigure Automated Backup v2.</span></span> <span data-ttu-id="12c4a-245">Než začnete, musíte provést tyto akce:</span><span class="sxs-lookup"><span data-stu-id="12c4a-245">Before you begin, you must:</span></span>

- <span data-ttu-id="12c4a-246">[Stáhněte a nainstalujte nejnovější prostředí Azure PowerShell text hello](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="12c4a-246">[Download and install hello latest Azure PowerShell](http://aka.ms/webpi-azps).</span></span>
- <span data-ttu-id="12c4a-247">Otevřete prostředí Windows PowerShell a přidružit svůj účet.</span><span class="sxs-lookup"><span data-stu-id="12c4a-247">Open Windows PowerShell and associate it with your account.</span></span> <span data-ttu-id="12c4a-248">Můžete to provést pomocí následujících kroků hello v hello [konfigurovat předplatné](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) části hello zřizování tématu.</span><span class="sxs-lookup"><span data-stu-id="12c4a-248">You can do this by following hello steps in hello [Configure your subscription](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) section of hello provisioning topic.</span></span>

### <a name="install-hello-sql-iaas-extension"></a><span data-ttu-id="12c4a-249">Nainstalujte hello IaaS rozšíření systému SQL</span><span class="sxs-lookup"><span data-stu-id="12c4a-249">Install hello SQL IaaS Extension</span></span>
<span data-ttu-id="12c4a-250">Pokud jste zřídili virtuálního počítače systému SQL Server z hello portálu Azure, by měl hello IaaS rozšíření systému SQL Server již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="12c4a-250">If you provisioned a SQL Server virtual machine from hello Azure portal, hello SQL Server IaaS Extension should already be installed.</span></span> <span data-ttu-id="12c4a-251">Můžete určit, pokud je nainstalovaná pro virtuální počítač voláním **Get-AzureRmVM** příkaz a prozkoumání hello **rozšíření** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="12c4a-251">You can determine if it is installed for your VM by calling **Get-AzureRmVM** command and examining hello **Extensions** property.</span></span>

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions 
```

<span data-ttu-id="12c4a-252">Pokud je nainstalovaná hello rozšíření IaaS agenta systému SQL Server, měli byste vidět, že je uveden jako "SqlIaaSAgent" nebo "SQLIaaSExtension".</span><span class="sxs-lookup"><span data-stu-id="12c4a-252">If hello SQL Server IaaS Agent extension is installed, you should see it listed as “SqlIaaSAgent” or “SQLIaaSExtension”.</span></span> <span data-ttu-id="12c4a-253">**Stav zřizování** pro hello rozšíření by měl také zobrazit, "Succeeded".</span><span class="sxs-lookup"><span data-stu-id="12c4a-253">**ProvisioningState** for hello extension should also show “Succeeded”.</span></span> 

<span data-ttu-id="12c4a-254">Pokud není nainstalována nebo se nezdařilo toobe zřízený, můžete ho nainstalovat s hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="12c4a-254">If it is not installed or failed toobe provisioned, you can install it with hello following command.</span></span> <span data-ttu-id="12c4a-255">Přidání toohello virtuálních počítačů název nebo skupině prostředků, musíte také určit oblasti hello (**$region**) umístěnou ve virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="12c4a-255">In addition toohello VM name and resource group, you must also specify hello region (**$region**) that your VM is located in.</span></span>

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region 
```

### <span data-ttu-id="12c4a-256"><a id="verifysettings"></a>Ověřte aktuální nastavení</span><span class="sxs-lookup"><span data-stu-id="12c4a-256"><a id="verifysettings"></a> Verify current settings</span></span>
<span data-ttu-id="12c4a-257">Pokud jste povolili automatizované zálohování při zřizování, můžete použít PowerShell toocheck aktuální konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="12c4a-257">If you enabled automated backup during provisioning, you can use PowerShell toocheck your current configuration.</span></span> <span data-ttu-id="12c4a-258">Spustit hello **Get-AzureRmVMSqlServerExtension** příkaz a zkontrolujte hello **AutoBackupSettings** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="12c4a-258">Run hello **Get-AzureRmVMSqlServerExtension** command and examine hello **AutoBackupSettings** property:</span></span>

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

<span data-ttu-id="12c4a-259">Měli byste obdržet výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="12c4a-259">You should get output similar toohello following:</span></span>

```
Enable                      : True
EnableEncryption            : False
RetentionPeriod             : 30
StorageUrl                  : https://test.blob.core.windows.net/
StorageAccessKey            :  
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : Manual
FullBackupFrequency         : WEEKLY
FullBackupStartTime         : 2
FullBackupWindowHours       : 2
LogBackupFrequency          : 60
```

<span data-ttu-id="12c4a-260">Pokud vaše výstup ukazuje, že **povolit** je nastaven příliš**False**, pak máte tooenable automatizované zálohování.</span><span class="sxs-lookup"><span data-stu-id="12c4a-260">If your output shows that **Enable** is set too**False**, then you have tooenable automated backup.</span></span> <span data-ttu-id="12c4a-261">Hello Dobrá zpráva je, že jste povolit a konfigurovat automatizovaného zálohování v hello stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="12c4a-261">hello good news is that you enable and configure Automated Backup in hello same way.</span></span> <span data-ttu-id="12c4a-262">Viz další část hello tyto informace.</span><span class="sxs-lookup"><span data-stu-id="12c4a-262">See hello next section for this information.</span></span>

> [!NOTE] 
> <span data-ttu-id="12c4a-263">Pokud zaškrtnete nastavení hello okamžitě po provedení změny, je možné, že budete mít zpět hello původní hodnoty konfigurace.</span><span class="sxs-lookup"><span data-stu-id="12c4a-263">If you check hello settings immediately after making a change, it is possible that you will get back hello old configuration values.</span></span> <span data-ttu-id="12c4a-264">Počkejte několik minut a zkontrolujte nastavení hello znovu toomake jistotu, že byly použity změny.</span><span class="sxs-lookup"><span data-stu-id="12c4a-264">Wait a few minutes and check hello settings again toomake sure that your changes were applied.</span></span>

### <a name="configure-automated-backup-v2"></a><span data-ttu-id="12c4a-265">Konfigurace automatického zálohování v2</span><span class="sxs-lookup"><span data-stu-id="12c4a-265">Configure Automated Backup v2</span></span>
<span data-ttu-id="12c4a-266">Můžete PowerShell tooenable automatizované zálohování, jakož i toomodify jeho konfiguraci a chování kdykoli.</span><span class="sxs-lookup"><span data-stu-id="12c4a-266">You can use PowerShell tooenable Automated Backup as well as toomodify its configuration and behavior at any time.</span></span> 

<span data-ttu-id="12c4a-267">Nejdřív vyberte nebo vytvořte účet úložiště pro hello záložní soubory.</span><span class="sxs-lookup"><span data-stu-id="12c4a-267">First, select or create a storage account for hello backup files.</span></span> <span data-ttu-id="12c4a-268">Hello následující skript vybere účet úložiště nebo ji vytvoří, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="12c4a-268">hello following script selects a storage account or creates it if it does not exist.</span></span>

```powershell
$storage_accountname = “yourstorageaccount”
$storage_resourcegroupname = $resourcegroupname

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region } 
```

> [!NOTE]
> <span data-ttu-id="12c4a-269">Automatizované zálohování nepodporuje ukládání záloh v storage úrovně premium, ale může trvat zálohy z disků virtuálních počítačů, které používají úložiště Premium.</span><span class="sxs-lookup"><span data-stu-id="12c4a-269">Automated Backup does not support storing backups in premium storage, but it can take backups from VM disks which use Premium Storage.</span></span>

<span data-ttu-id="12c4a-270">Potom pomocí hello **New-AzureRmVMSqlServerAutoBackupConfig** příkaz tooenable a nakonfigurujte hello v2 automatizovaného zálohování nastavení toostore zálohy v účtu úložiště Azure hello.</span><span class="sxs-lookup"><span data-stu-id="12c4a-270">Then use hello **New-AzureRmVMSqlServerAutoBackupConfig** command tooenable and configure hello Automated Backup v2 settings toostore backups in hello Azure storage account.</span></span> <span data-ttu-id="12c4a-271">V tomto příkladu jsou hello zálohy nastaveny toobe uchovávat 10 dní.</span><span class="sxs-lookup"><span data-stu-id="12c4a-271">In this example, hello backups are set toobe retained for 10 days.</span></span> <span data-ttu-id="12c4a-272">Zálohování databáze systému jsou povolené.</span><span class="sxs-lookup"><span data-stu-id="12c4a-272">System database backups are enabled.</span></span> <span data-ttu-id="12c4a-273">Úplné zálohy jsou naplánovány pro každý týden s časovým oknem začínající na 20:00 pro dvě hodiny.</span><span class="sxs-lookup"><span data-stu-id="12c4a-273">Full backups are scheduled for weekly with a time window starting at 20:00 for two hours.</span></span> <span data-ttu-id="12c4a-274">Zálohy protokolů jsou naplánovány pro každých 30 minut.</span><span class="sxs-lookup"><span data-stu-id="12c4a-274">Log backups are scheduled for every 30 minutes.</span></span> <span data-ttu-id="12c4a-275">Hello druhý příkaz, **Set-AzureRmVMSqlServerExtension**, aktualizace hello zadaný virtuální počítač Azure s těmito nastaveními.</span><span class="sxs-lookup"><span data-stu-id="12c4a-275">hello second command, **Set-AzureRmVMSqlServerExtension**, updates hello specified Azure VM with these settings.</span></span>

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname 
```

<span data-ttu-id="12c4a-276">Ho může trvat několik minut tooinstall a nakonfigurujte hello IaaS agenta systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="12c4a-276">It could take several minutes tooinstall and configure hello SQL Server IaaS Agent.</span></span> 

<span data-ttu-id="12c4a-277">šifrování tooenable upravit hello předchozí skript toopass hello **EnableEncryption** parametr společně s heslo (zabezpečený řetězec) pro hello **CertificatePassword** parametr.</span><span class="sxs-lookup"><span data-stu-id="12c4a-277">tooenable encryption, modify hello previous script toopass hello **EnableEncryption** parameter along with a password (secure string) for hello **CertificatePassword** parameter.</span></span> <span data-ttu-id="12c4a-278">Hello následující skript umožňuje hello nastavení automatizovaného zálohování v předchozím příkladu hello a přidá šifrování.</span><span class="sxs-lookup"><span data-stu-id="12c4a-278">hello following script enables hello Automated Backup settings in hello previous example and adds encryption.</span></span>

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

<span data-ttu-id="12c4a-279">nastavení se použijí, tooconfirm [ověřte konfiguraci automatizovaného zálohování hello](#verifysettings).</span><span class="sxs-lookup"><span data-stu-id="12c4a-279">tooconfirm your settings are applied, [verify hello Automated Backup configuration](#verifysettings).</span></span>

### <a name="disable-automated-backup"></a><span data-ttu-id="12c4a-280">Zakázat automatizované zálohování</span><span class="sxs-lookup"><span data-stu-id="12c4a-280">Disable Automated Backup</span></span>
<span data-ttu-id="12c4a-281">toodisable automatizované zálohování, spusťte hello stejný skript bez hello **-povolit** parametr toohello **New-AzureRmVMSqlServerAutoBackupConfig** příkaz.</span><span class="sxs-lookup"><span data-stu-id="12c4a-281">toodisable Automated Backup, run hello same script without hello **-Enable** parameter toohello **New-AzureRmVMSqlServerAutoBackupConfig** command.</span></span> <span data-ttu-id="12c4a-282">Hello absenci hello **-povolit** parametr signály hello příkaz toodisable hello funkce.</span><span class="sxs-lookup"><span data-stu-id="12c4a-282">hello absence of hello **-Enable** parameter signals hello command toodisable hello feature.</span></span> <span data-ttu-id="12c4a-283">Stejně jako u instalace může trvat několik minut toodisable automatizované zálohování.</span><span class="sxs-lookup"><span data-stu-id="12c4a-283">As with installation, it can take several minutes toodisable Automated Backup.</span></span>

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a><span data-ttu-id="12c4a-284">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="12c4a-284">Example script</span></span>
<span data-ttu-id="12c4a-285">Hello následující skript představuje sadu proměnných, můžete přizpůsobit tooenable a konfiguraci automatizovaného zálohování pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="12c4a-285">hello following script provides a set of variables that you can customize tooenable and configure Automated Backup for your VM.</span></span> <span data-ttu-id="12c4a-286">Ve vašem případě bude pravděpodobně nutné toocustomize hello skript podle svých požadavků.</span><span class="sxs-lookup"><span data-stu-id="12c4a-286">In your case, you might need toocustomize hello script based on your requirements.</span></span> <span data-ttu-id="12c4a-287">Například by mít toomake změny, pokud byste chtěli toodisable hello zálohu systémové databáze nebo povolit šifrování.</span><span class="sxs-lookup"><span data-stu-id="12c4a-287">For example, you would have toomake changes if you wanted toodisable hello backup of system databases or enable encryption.</span></span>

```powershell
$vmname = "yourvmname"
$resourcegroupname = "vmresourcegroupname"
$region = “Azure region name such as EASTUS2”
$storage_accountname = “storageaccountname”
$storage_resourcegroupname = $resourcegroupname
$retentionperiod = 10
$backupscheduletype = "Manual"
$fullbackupfrequency = "Weekly"
$fullbackupstarthour = "20"
$fullbackupwindow = "2"
$logbackupfrequency = "30"

# ResourceGroupName is hello resource group which is hosting hello VM where you are deploying hello SQL IaaS Extension 

Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region

# Creates/use a storage account toostore hello backups

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }

# Configure Automated Backup settings

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays $retentionperiod -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType $backupscheduletype -FullBackupFrequency $fullbackupfrequency `
    -FullBackupStartHour $fullbackupstarthour -FullBackupWindowInHours $fullbackupwindow `
    -LogBackupFrequencyInMinutes $logbackupfrequency

# Apply hello Automated Backup settings toohello VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a><span data-ttu-id="12c4a-288">Další kroky</span><span class="sxs-lookup"><span data-stu-id="12c4a-288">Next steps</span></span>
<span data-ttu-id="12c4a-289">Automatizované zálohování v2 nakonfiguruje spravovaného zálohování na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="12c4a-289">Automated Backup v2 configures Managed Backup on Azure VMs.</span></span> <span data-ttu-id="12c4a-290">Proto je důležité příliš[hello v dokumentaci pro spravovanou zálohu](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello chování a důsledky.</span><span class="sxs-lookup"><span data-stu-id="12c4a-290">So it is important too[review hello documentation for Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello behavior and implications.</span></span>

<span data-ttu-id="12c4a-291">Můžete najít další zálohování a obnovení pokyny pro SQL Server na virtuálních počítačích Azure v následujícím tématu hello: [zálohování a obnovení pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="12c4a-291">You can find additional backup and restore guidance for SQL Server on Azure VMs in hello following topic: [Backup and Restore for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).</span></span>

<span data-ttu-id="12c4a-292">Informace o dalších úlohách, k dispozici automation najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="12c4a-292">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

<span data-ttu-id="12c4a-293">Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="12c4a-293">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

