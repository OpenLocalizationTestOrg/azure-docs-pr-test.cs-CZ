---
title: "Automatizované zálohování v2 pro SQL Server 2016 virtuální počítače Azure | Microsoft Docs"
description: "Vysvětluje funkci automatizované zálohování pro SQL Server 2016 virtuální počítače spuštěné v Azure. Tento článek je specifické pro virtuální počítače pomocí Správce prostředků."
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
ms.openlocfilehash: e7e14b0243f82c672392d5ab4bb6aca01156465b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="automated-backup-v2-for-sql-server-2016-azure-virtual-machines-resource-manager"></a><span data-ttu-id="954ef-104">Automatizované zálohování v2 pro SQL Server 2016 virtuální počítače Azure (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="954ef-104">Automated Backup v2 for SQL Server 2016 Azure Virtual Machines (Resource Manager)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="954ef-105">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="954ef-105">SQL Server 2014</span></span>](virtual-machines-windows-sql-automated-backup.md)
> * [<span data-ttu-id="954ef-106">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="954ef-106">SQL Server 2016</span></span>](virtual-machines-windows-sql-automated-backup-v2.md)

<span data-ttu-id="954ef-107">Automatizované zálohování v2 automaticky nakonfiguruje [spravovaného zálohování Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) pro všechny stávající a nové databáze na virtuální počítač Azure běžet tyto edice SQL Server 2016 Standard, Enterprise nebo Developer.</span><span class="sxs-lookup"><span data-stu-id="954ef-107">Automated Backup v2 automatically configures [Managed Backup to Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) for all existing and new databases on an Azure VM running SQL Server 2016 Standard, Enterprise, or Developer editions.</span></span> <span data-ttu-id="954ef-108">To umožňuje nakonfigurovat standardní databázi zálohování, které využívají služby odolné Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="954ef-108">This enables you to configure regular database backups that utilize durable Azure blob storage.</span></span> <span data-ttu-id="954ef-109">Automatizované zálohování v2 závisí na [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="954ef-109">Automated Backup v2 depends on the [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="954ef-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="954ef-110">Prerequisites</span></span>
<span data-ttu-id="954ef-111">Pokud chcete používat v2 automatizovaného zálohování, zkontrolujte splnění následujících předpokladů:</span><span class="sxs-lookup"><span data-stu-id="954ef-111">To use Automated Backup v2, review the following prerequisites:</span></span>

<span data-ttu-id="954ef-112">**Operační systém**:</span><span class="sxs-lookup"><span data-stu-id="954ef-112">**Operating System**:</span></span>

- <span data-ttu-id="954ef-113">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="954ef-113">Windows Server 2012 R2</span></span>
- <span data-ttu-id="954ef-114">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="954ef-114">Windows Server 2016</span></span>

<span data-ttu-id="954ef-115">**Verzi nebo edici systému SQL Server**:</span><span class="sxs-lookup"><span data-stu-id="954ef-115">**SQL Server version/edition**:</span></span>

- <span data-ttu-id="954ef-116">SQL Server 2016 Standard</span><span class="sxs-lookup"><span data-stu-id="954ef-116">SQL Server 2016 Standard</span></span>
- <span data-ttu-id="954ef-117">SQL Server 2016 Enterprise</span><span class="sxs-lookup"><span data-stu-id="954ef-117">SQL Server 2016 Enterprise</span></span>
- <span data-ttu-id="954ef-118">SQL Server 2016 Developer</span><span class="sxs-lookup"><span data-stu-id="954ef-118">SQL Server 2016 Developer</span></span>

> [!IMPORTANT]
> <span data-ttu-id="954ef-119">Automatizované zálohování v2 spolupracuje se službou SQL Server 2016.</span><span class="sxs-lookup"><span data-stu-id="954ef-119">Automated Backup v2 works with SQL Server 2016.</span></span> <span data-ttu-id="954ef-120">Pokud používáte SQL Server 2014, můžete zálohovat databáze automatizovaného zálohování v1.</span><span class="sxs-lookup"><span data-stu-id="954ef-120">If you are using SQL Server 2014, you can use Automated Backup v1 to back up your databases.</span></span> <span data-ttu-id="954ef-121">Další informace najdete v tématu [automatizované zálohování pro SQL Server 2014 virtuální počítače Azure](virtual-machines-windows-sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="954ef-121">For more information, see [Automated Backup for SQL Server 2014 Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).</span></span>

<span data-ttu-id="954ef-122">**Konfigurace databáze**:</span><span class="sxs-lookup"><span data-stu-id="954ef-122">**Database configuration**:</span></span>

- <span data-ttu-id="954ef-123">Cílové databáze musí mít úplném modelu obnovení.</span><span class="sxs-lookup"><span data-stu-id="954ef-123">Target databases must use the full recovery model.</span></span> <span data-ttu-id="954ef-124">Další informace o vlivu úplném modelu obnovení na zálohování najdete v tématu [zálohování v části the úplný Model obnovení](https://technet.microsoft.com/library/ms190217.aspx).</span><span class="sxs-lookup"><span data-stu-id="954ef-124">For more information about the impact of the full recovery model on backups, see [Backup Under the Full Recovery Model](https://technet.microsoft.com/library/ms190217.aspx).</span></span>
- <span data-ttu-id="954ef-125">Systémové databáze není nutné používat úplném modelu obnovení.</span><span class="sxs-lookup"><span data-stu-id="954ef-125">System databases do not have to use full recovery model.</span></span> <span data-ttu-id="954ef-126">Pokud budete potřebovat zálohy protokolu, které mají být provedeny pro Model, nebo databázi MSDB, ale musí používat úplném modelu obnovení.</span><span class="sxs-lookup"><span data-stu-id="954ef-126">However, if you require log backups to be taken for Model or MSDB, you must use full recovery model.</span></span>
- <span data-ttu-id="954ef-127">Cílové databáze musí být na výchozí instanci SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="954ef-127">Target databases must be on the default SQL Server instance.</span></span> <span data-ttu-id="954ef-128">IaaS rozšíření systému SQL Server nepodporuje pojmenované instance.</span><span class="sxs-lookup"><span data-stu-id="954ef-128">The SQL Server IaaS Extension does not support named instances.</span></span>

<span data-ttu-id="954ef-129">**Model nasazení Azure**:</span><span class="sxs-lookup"><span data-stu-id="954ef-129">**Azure deployment model**:</span></span>

- <span data-ttu-id="954ef-130">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="954ef-130">Resource Manager</span></span>

> [!NOTE]
> <span data-ttu-id="954ef-131">Automatizované zálohování spoléhá na **rozšíření agenta systému SQL Server IaaS**.</span><span class="sxs-lookup"><span data-stu-id="954ef-131">Automated Backup relies on the **SQL Server IaaS Agent Extension**.</span></span> <span data-ttu-id="954ef-132">Aktuální SQL bitové kopie virtuálních počítačů Galerie přidejte toto rozšíření ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="954ef-132">Current SQL virtual machine gallery images add this extension by default.</span></span> <span data-ttu-id="954ef-133">Další informace najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="954ef-133">For more information, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

## <a name="settings"></a><span data-ttu-id="954ef-134">Nastavení</span><span class="sxs-lookup"><span data-stu-id="954ef-134">Settings</span></span>
<span data-ttu-id="954ef-135">Následující tabulka popisuje možnosti, které mohou být konfigurovány pro automatizované zálohování v2.</span><span class="sxs-lookup"><span data-stu-id="954ef-135">The following table describes the options that can be configured for Automated Backup v2.</span></span> <span data-ttu-id="954ef-136">Skutečné konfiguračních kroků se liší v závislosti na tom, zda používáte portál Azure nebo příkazů prostředí Windows PowerShell pro Azure.</span><span class="sxs-lookup"><span data-stu-id="954ef-136">The actual configuration steps vary depending on whether you use the Azure portal or Azure Windows PowerShell commands.</span></span>

### <a name="basic-settings"></a><span data-ttu-id="954ef-137">Základní nastavení</span><span class="sxs-lookup"><span data-stu-id="954ef-137">Basic Settings</span></span>

| <span data-ttu-id="954ef-138">Nastavení</span><span class="sxs-lookup"><span data-stu-id="954ef-138">Setting</span></span> | <span data-ttu-id="954ef-139">Rozsah (výchozí)</span><span class="sxs-lookup"><span data-stu-id="954ef-139">Range (Default)</span></span> | <span data-ttu-id="954ef-140">Popis</span><span class="sxs-lookup"><span data-stu-id="954ef-140">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="954ef-141">**Automatizované zálohování**</span><span class="sxs-lookup"><span data-stu-id="954ef-141">**Automated Backup**</span></span> | <span data-ttu-id="954ef-142">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="954ef-142">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="954ef-143">Povolí nebo zakáže automatizované zálohování pro virtuální počítač Azure SQL Server 2016 Standard nebo Enterprise.</span><span class="sxs-lookup"><span data-stu-id="954ef-143">Enables or disables Automated Backup for an Azure VM running SQL Server 2016 Standard or Enterprise.</span></span> |
| <span data-ttu-id="954ef-144">**Doba uchování dat**</span><span class="sxs-lookup"><span data-stu-id="954ef-144">**Retention Period**</span></span> | <span data-ttu-id="954ef-145">1 až 30 dní (30 dní)</span><span class="sxs-lookup"><span data-stu-id="954ef-145">1-30 days (30 days)</span></span> | <span data-ttu-id="954ef-146">Počet dní uchování záloh.</span><span class="sxs-lookup"><span data-stu-id="954ef-146">The number of days to retain backups.</span></span> |
| <span data-ttu-id="954ef-147">**Účet úložiště**</span><span class="sxs-lookup"><span data-stu-id="954ef-147">**Storage Account**</span></span> | <span data-ttu-id="954ef-148">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="954ef-148">Azure storage account</span></span> | <span data-ttu-id="954ef-149">Účet úložiště Azure pro ukládání souborů automatizovaného zálohování do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="954ef-149">An Azure storage account to use for storing Automated Backup files in blob storage.</span></span> <span data-ttu-id="954ef-150">Kontejner se vytvoří v tomto umístění pro uložení všechny záložní soubory.</span><span class="sxs-lookup"><span data-stu-id="954ef-150">A container is created at this location to store all backup files.</span></span> <span data-ttu-id="954ef-151">Zásady vytváření názvů záložní soubor obsahuje date, time a GUID databáze.</span><span class="sxs-lookup"><span data-stu-id="954ef-151">The backup file naming convention includes the date, time, and database GUID.</span></span> |
| <span data-ttu-id="954ef-152">**Šifrování**</span><span class="sxs-lookup"><span data-stu-id="954ef-152">**Encryption**</span></span> |<span data-ttu-id="954ef-153">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="954ef-153">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="954ef-154">Povolí nebo zakáže šifrování.</span><span class="sxs-lookup"><span data-stu-id="954ef-154">Enables or disables encryption.</span></span> <span data-ttu-id="954ef-155">Když je povolené šifrování, certifikátů používaných pro obnovení zálohy jsou umístěné v zadaný účet úložiště ve stejné **automaticbackup** kontejneru pomocí stejné zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="954ef-155">When encryption is enabled, the certificates used to restore the backup are located in the specified storage account in the same **automaticbackup** container using the same naming convention.</span></span> <span data-ttu-id="954ef-156">Pokud se změní heslo, se toto heslo se vygeneruje nový certifikát, ale pořád starý certifikát pro obnovení předchozí zálohy.</span><span class="sxs-lookup"><span data-stu-id="954ef-156">If the password changes, a new certificate is generated with that password, but the old certificate remains to restore prior backups.</span></span> |
| <span data-ttu-id="954ef-157">**Heslo**</span><span class="sxs-lookup"><span data-stu-id="954ef-157">**Password**</span></span> |<span data-ttu-id="954ef-158">Heslo</span><span class="sxs-lookup"><span data-stu-id="954ef-158">Password text</span></span> | <span data-ttu-id="954ef-159">Heslo pro šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="954ef-159">A password for encryption keys.</span></span> <span data-ttu-id="954ef-160">Toto je pouze vyžaduje, pokud je povolené šifrování.</span><span class="sxs-lookup"><span data-stu-id="954ef-160">This is only required if encryption is enabled.</span></span> <span data-ttu-id="954ef-161">Chcete-li obnovit šifrované zálohování, musí mít správné heslo a související certifikátu, který byl použit v době, kdy bylo provedeno zálohování.</span><span class="sxs-lookup"><span data-stu-id="954ef-161">In order to restore an encrypted backup, you must have the correct password and related certificate that was used at the time the backup was taken.</span></span> |

### <a name="advanced-settings"></a><span data-ttu-id="954ef-162">Upřesnit nastavení</span><span class="sxs-lookup"><span data-stu-id="954ef-162">Advanced Settings</span></span>

| <span data-ttu-id="954ef-163">Nastavení</span><span class="sxs-lookup"><span data-stu-id="954ef-163">Setting</span></span> | <span data-ttu-id="954ef-164">Rozsah (výchozí)</span><span class="sxs-lookup"><span data-stu-id="954ef-164">Range (Default)</span></span> | <span data-ttu-id="954ef-165">Popis</span><span class="sxs-lookup"><span data-stu-id="954ef-165">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="954ef-166">**Zálohování databáze systému**</span><span class="sxs-lookup"><span data-stu-id="954ef-166">**System Database Backups**</span></span> | <span data-ttu-id="954ef-167">Povolí nebo zakáže (zakázáno)</span><span class="sxs-lookup"><span data-stu-id="954ef-167">Enable/Disable (Disabled)</span></span> | <span data-ttu-id="954ef-168">Když je povolené, tato funkce bude také zálohování databází systému: hlavní server, databázi MSDB a modelu.</span><span class="sxs-lookup"><span data-stu-id="954ef-168">When enabled, this feature will also back up the system databases: Master, MSDB, and Model.</span></span> <span data-ttu-id="954ef-169">Pro databázi MSDB a Model databáze ověřte, zda je v režimu úplného obnovení zálohy protokolu, které mají být provedeny, chcete-li.</span><span class="sxs-lookup"><span data-stu-id="954ef-169">For the MSDB and Model databases, verify that they are in full recovery mode if you want log backups to be taken.</span></span> <span data-ttu-id="954ef-170">Zálohy protokolů se nikdy provádějí pro hlavní server.</span><span class="sxs-lookup"><span data-stu-id="954ef-170">Log backups are never taken for Master.</span></span> <span data-ttu-id="954ef-171">A jsou provedeny žádné zálohy pro databázi TempDB.</span><span class="sxs-lookup"><span data-stu-id="954ef-171">And no backups are taken for TempDB.</span></span> |
| <span data-ttu-id="954ef-172">**Plán zálohování**</span><span class="sxs-lookup"><span data-stu-id="954ef-172">**Backup Schedule**</span></span> | <span data-ttu-id="954ef-173">Ruční nebo automatické (Automated)</span><span class="sxs-lookup"><span data-stu-id="954ef-173">Manual/Automated (Automated)</span></span> | <span data-ttu-id="954ef-174">Ve výchozím nastavení plán zálohování automaticky určí založené na protokolu růst.</span><span class="sxs-lookup"><span data-stu-id="954ef-174">By default, the backup schedule will be automatically determined based on the log growth.</span></span> <span data-ttu-id="954ef-175">Ruční plán zálohování umožňuje uživateli zadat časový interval pro zálohy.</span><span class="sxs-lookup"><span data-stu-id="954ef-175">Manual backup schedule allows the user to specify the time window for backups.</span></span> <span data-ttu-id="954ef-176">V takovém případě zálohování bude vždy jen probíhat na zadané četnosti a během zadaného časového okna pro daný den.</span><span class="sxs-lookup"><span data-stu-id="954ef-176">In this case, backups will only ever take place at the specified frequency and during the specified time window of a given day.</span></span> |
| <span data-ttu-id="954ef-177">**Četnost úplné zálohování**</span><span class="sxs-lookup"><span data-stu-id="954ef-177">**Full backup frequency**</span></span> | <span data-ttu-id="954ef-178">Denně nebo týdně</span><span class="sxs-lookup"><span data-stu-id="954ef-178">Daily/Weekly</span></span> | <span data-ttu-id="954ef-179">Četnost úplné zálohy.</span><span class="sxs-lookup"><span data-stu-id="954ef-179">Frequency of full backups.</span></span> <span data-ttu-id="954ef-180">V obou případech bude zahájena úplné zálohy během okna další naplánovanou dobu.</span><span class="sxs-lookup"><span data-stu-id="954ef-180">In both cases, full backups will begin during the next scheduled time window.</span></span> <span data-ttu-id="954ef-181">Pokud je vybraná týdně, zálohování může zahrnovat více dní, dokud všechny databáze úspěšně zálohovali.</span><span class="sxs-lookup"><span data-stu-id="954ef-181">When weekly is selected, backups could span multiple days until all databases have successfully backed up.</span></span> |
| <span data-ttu-id="954ef-182">**Čas spuštění úplného zálohování**</span><span class="sxs-lookup"><span data-stu-id="954ef-182">**Full backup start time**</span></span> | <span data-ttu-id="954ef-183">00:00 – 23:00 (01:00)</span><span class="sxs-lookup"><span data-stu-id="954ef-183">00:00 – 23:00 (01:00)</span></span> | <span data-ttu-id="954ef-184">Počáteční čas daný den, během které úplné zálohování lze provést.</span><span class="sxs-lookup"><span data-stu-id="954ef-184">Start time of a given day during which full backups can take place.</span></span> |
| <span data-ttu-id="954ef-185">**Úplné zálohování časový interval**</span><span class="sxs-lookup"><span data-stu-id="954ef-185">**Full backup time window**</span></span> | <span data-ttu-id="954ef-186">1 – 23 hodin (1 hodina)</span><span class="sxs-lookup"><span data-stu-id="954ef-186">1 – 23 hours (1 hour)</span></span> | <span data-ttu-id="954ef-187">Doba trvání časový interval daný den, během které úplné zálohování lze provést.</span><span class="sxs-lookup"><span data-stu-id="954ef-187">Duration of the time window of a given day during which full backups can take place.</span></span> |
| <span data-ttu-id="954ef-188">**Četnost záloh protokolu**</span><span class="sxs-lookup"><span data-stu-id="954ef-188">**Log backup frequency**</span></span> | <span data-ttu-id="954ef-189">5 – 60 minut (60 minut)</span><span class="sxs-lookup"><span data-stu-id="954ef-189">5 – 60 minutes (60 minutes)</span></span> | <span data-ttu-id="954ef-190">Četnost záloh protokolu.</span><span class="sxs-lookup"><span data-stu-id="954ef-190">Frequency of log backups.</span></span> |

## <a name="understanding-full-backup-frequency"></a><span data-ttu-id="954ef-191">Principy četnost úplné zálohování</span><span class="sxs-lookup"><span data-stu-id="954ef-191">Understanding full backup frequency</span></span>
<span data-ttu-id="954ef-192">Je důležité si uvědomit rozdíl mezi denní nebo týdenní úplné zálohování.</span><span class="sxs-lookup"><span data-stu-id="954ef-192">It is important to understand the difference between daily and weekly full backups.</span></span> <span data-ttu-id="954ef-193">V této snahy můžeme provede dva ukázkové scénáře.</span><span class="sxs-lookup"><span data-stu-id="954ef-193">In this effort, we will walk through two example scenarios.</span></span>

### <a name="scenario-1-weekly-backups"></a><span data-ttu-id="954ef-194">Scénář 1: Týdenní zálohy</span><span class="sxs-lookup"><span data-stu-id="954ef-194">Scenario 1: Weekly backups</span></span>
<span data-ttu-id="954ef-195">Máte virtuální počítač na serveru SQL, který obsahuje počet velmi velké databáze.</span><span class="sxs-lookup"><span data-stu-id="954ef-195">You have a SQL Server VM which contains a number of very large databases.</span></span>

<span data-ttu-id="954ef-196">V pondělí povolíte automatizované zálohování v2 s následujícím nastavením:</span><span class="sxs-lookup"><span data-stu-id="954ef-196">On Monday, you enable Automated Backup v2 with the following settings:</span></span>

- <span data-ttu-id="954ef-197">Plán zálohování: **ruční**</span><span class="sxs-lookup"><span data-stu-id="954ef-197">Backup schedule: **Manual**</span></span>
- <span data-ttu-id="954ef-198">Úplné četnost zálohování: **týdně**</span><span class="sxs-lookup"><span data-stu-id="954ef-198">Full backup frequency: **Weekly**</span></span>
- <span data-ttu-id="954ef-199">Úplné zálohování počáteční čas: **01:00**</span><span class="sxs-lookup"><span data-stu-id="954ef-199">Full backup start time: **01:00**</span></span>
- <span data-ttu-id="954ef-200">Úplné zálohování časové okno: **1 hodina**</span><span class="sxs-lookup"><span data-stu-id="954ef-200">Full backup time window: **1 hour**</span></span>

<span data-ttu-id="954ef-201">To znamená, že dalším dostupném časovém intervalu zálohování je úterý v 1: 00 1 hodinu.</span><span class="sxs-lookup"><span data-stu-id="954ef-201">This means that the next available backup window is Tuesday at 1 AM for 1 hour.</span></span> <span data-ttu-id="954ef-202">V té době zahájíte automatizovaného zálohování, zálohování databází jeden najednou.</span><span class="sxs-lookup"><span data-stu-id="954ef-202">At that time, Automated Backup will begin backing up your databases one at a time.</span></span> <span data-ttu-id="954ef-203">V tomto scénáři jsou dostatečně velký, že úplné zálohy dokončí pro první databáze několik databází.</span><span class="sxs-lookup"><span data-stu-id="954ef-203">In this scenario, your databases are large enough that full backups will complete for the first couple databases.</span></span> <span data-ttu-id="954ef-204">Ale po jedné hodině všechny databáze byly zálohovány.</span><span class="sxs-lookup"><span data-stu-id="954ef-204">However, after one hour not all of the databases have been backed up.</span></span>

<span data-ttu-id="954ef-205">V takovém případě automatizovaného zálohování bude zahájeno zálohování databází zbývající další den středa v 1: 00 1 hodinu.</span><span class="sxs-lookup"><span data-stu-id="954ef-205">When this happens, Automated Backup will begin backing up the remaining databases the next day, Wednesday at 1 AM for 1 hour.</span></span> <span data-ttu-id="954ef-206">Pokud nejsou všechny databáze byly zálohovány v tento čas, pokusí se znovu další den ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="954ef-206">If not all databases have been backed up in that time, it will try again the next day at the same time.</span></span> <span data-ttu-id="954ef-207">To bude pokračovat, dokud všechny databáze byly úspěšně zálohovány.</span><span class="sxs-lookup"><span data-stu-id="954ef-207">This will continue until all databases have been successfully backed up.</span></span>

<span data-ttu-id="954ef-208">Jakmile ho znovu dosáhne úterý, bude automatizovaného zálohování začít zálohovat všechny databáze ještě jednou.</span><span class="sxs-lookup"><span data-stu-id="954ef-208">Once it reaches Tuesday again, Automated Backup will begin backing up all databases once again.</span></span>

<span data-ttu-id="954ef-209">Tento scénář popisuje automatizovaného zálohování bude fungovat pouze v rámci určeného časového období, a každou databázi, budou zálohovány jednou za týden.</span><span class="sxs-lookup"><span data-stu-id="954ef-209">This scenario shows that Automated Backup will only operate within the specified time window, and each database will be backed up once per week.</span></span> <span data-ttu-id="954ef-210">To také ukazuje, že je možné pro zálohy do zahrnovat více dní v případě, kde není možné dokončit všechny zálohy za jeden den.</span><span class="sxs-lookup"><span data-stu-id="954ef-210">This also shows that it is possible for backups to span multiple days in the case where it is not possible to complete all backups in a single day.</span></span>

### <a name="scenario-2-daily-backups"></a><span data-ttu-id="954ef-211">Scénář 2: Denní zálohy</span><span class="sxs-lookup"><span data-stu-id="954ef-211">Scenario 2: Daily backups</span></span>
<span data-ttu-id="954ef-212">Máte virtuální počítač na serveru SQL, který obsahuje počet velmi velké databáze.</span><span class="sxs-lookup"><span data-stu-id="954ef-212">You have a SQL Server VM which contains a number of very large databases.</span></span>

<span data-ttu-id="954ef-213">V pondělí povolíte automatizované zálohování v2 s následujícím nastavením:</span><span class="sxs-lookup"><span data-stu-id="954ef-213">On Monday, you enable Automated Backup v2 with the following settings:</span></span>

- <span data-ttu-id="954ef-214">Plán zálohování: ruční</span><span class="sxs-lookup"><span data-stu-id="954ef-214">Backup schedule: Manual</span></span>
- <span data-ttu-id="954ef-215">Úplných záloh četnost: denně</span><span class="sxs-lookup"><span data-stu-id="954ef-215">Full backup frequency: Daily</span></span>
- <span data-ttu-id="954ef-216">Úplné zálohování počáteční čas: 22:00</span><span class="sxs-lookup"><span data-stu-id="954ef-216">Full backup start time: 22:00</span></span>
- <span data-ttu-id="954ef-217">Úplné zálohování časové okno: 6 hodin</span><span class="sxs-lookup"><span data-stu-id="954ef-217">Full backup time window: 6 hours</span></span>

<span data-ttu-id="954ef-218">To znamená, že dalším dostupném časovém intervalu zálohování je pondělí na 22: 00 6 hodin.</span><span class="sxs-lookup"><span data-stu-id="954ef-218">This means that the next available backup window is Monday at 10 PM for 6 hours.</span></span> <span data-ttu-id="954ef-219">V té době zahájíte automatizovaného zálohování, zálohování databází jeden najednou.</span><span class="sxs-lookup"><span data-stu-id="954ef-219">At that time, Automated Backup will begin backing up your databases one at a time.</span></span>

<span data-ttu-id="954ef-220">Pak úterý v 10 6 hodin, budou úplné zálohy všech databází spusťte znovu.</span><span class="sxs-lookup"><span data-stu-id="954ef-220">Then, on Tuesday at 10 for 6 hours, full backups of all databases will start again.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="954ef-221">Při plánování denní zálohy, doporučujeme, abyste naplánovali široké časové okno zajistit, že všechny databáze lze zálohovat v rámci této doby.</span><span class="sxs-lookup"><span data-stu-id="954ef-221">When scheduling daily backups, it is recommended that you schedule a wide time window to ensure all databases can be backed up within this time.</span></span> <span data-ttu-id="954ef-222">To je obzvláště důležité v případě, kdy máte velké množství dat. Chcete-li zálohovat.</span><span class="sxs-lookup"><span data-stu-id="954ef-222">This is especially important in the case where you have a large amount of data to back up.</span></span>

## <a name="configuration-in-the-portal"></a><span data-ttu-id="954ef-223">Konfigurace na portálu</span><span class="sxs-lookup"><span data-stu-id="954ef-223">Configuration in the Portal</span></span>

<span data-ttu-id="954ef-224">Na portálu Azure můžete použít ke konfiguraci automatizovaného zálohování v2 při zřizování nebo pro existující SQL Server 2016 virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="954ef-224">You can use the Azure portal to configure Automated Backup v2 during provisioning or for existing SQL Server 2016 VMs.</span></span>

### <a name="new-vms"></a><span data-ttu-id="954ef-225">Nové virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="954ef-225">New VMs</span></span>

<span data-ttu-id="954ef-226">Použití portálu Azure ke konfiguraci automatizovaného zálohování v2 při vytváření nového SQL serveru 2016 virtuálního počítače v modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="954ef-226">Use the Azure portal to configure Automated Backup v2 when you create a new SQL Server 2016 Virtual Machine in the Resource Manager deployment model.</span></span> 

<span data-ttu-id="954ef-227">V **nastavení systému SQL Server** vyberte **automatizované zálohování**.</span><span class="sxs-lookup"><span data-stu-id="954ef-227">In the **SQL Server settings** blade, select **Automated backup**.</span></span> <span data-ttu-id="954ef-228">Následující Azure portálu snímek obrazovky ukazuje **automatizované zálohování SQL** okno.</span><span class="sxs-lookup"><span data-stu-id="954ef-228">The following Azure portal screenshot shows the **SQL Automated Backup** blade.</span></span>

![Konfigurace automatického zálohování SQL na portálu Azure](./media/virtual-machines-windows-sql-automated-backup-v2/automated-backup-blade.png)

> [!NOTE]
> <span data-ttu-id="954ef-230">Automatizované zálohování v2 ve výchozím nastavení vypnutá.</span><span class="sxs-lookup"><span data-stu-id="954ef-230">Automated Backup v2 is disabled by default.</span></span>

<span data-ttu-id="954ef-231">Kontext, naleznete v tématu dokončení na [zřizování virtuálního počítače s SQL serverem v Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="954ef-231">For context, see the complete topic on [provisioning a SQL Server virtual machine in Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

### <a name="existing-vms"></a><span data-ttu-id="954ef-232">Existující virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="954ef-232">Existing VMs</span></span>

<span data-ttu-id="954ef-233">Pro existující virtuální počítače systému SQL Server vyberte virtuální počítač systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="954ef-233">For existing SQL Server virtual machines, select your SQL Server virtual machine.</span></span> <span data-ttu-id="954ef-234">Vyberte **konfigurace systému SQL Server** části **nastavení** okno.</span><span class="sxs-lookup"><span data-stu-id="954ef-234">Then select the **SQL Server configuration** section of the **Settings** blade.</span></span>

![Automatizované zálohování SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration.png)

<span data-ttu-id="954ef-236">V **konfigurace systému SQL Server** okně klikněte **upravit** tlačítko v části Automatické zálohování.</span><span class="sxs-lookup"><span data-stu-id="954ef-236">In the **SQL Server configuration** blade, click the **Edit** button in the Automated backup section.</span></span>

![Konfigurace automatického zálohování SQL pro existující virtuální počítače](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration-edit.png)

<span data-ttu-id="954ef-238">Po dokončení klikněte **OK** tlačítko v dolní části **konfigurace systému SQL Server** okno a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="954ef-238">When finished, click the **OK** button on the bottom of the **SQL Server configuration** blade to save your changes.</span></span>

<span data-ttu-id="954ef-239">Jestliže povolíte automatizované zálohování poprvé, nakonfiguruje Azure IaaS Agent serveru SQL Server na pozadí.</span><span class="sxs-lookup"><span data-stu-id="954ef-239">If you are enabling Automated Backup for the first time, Azure configures the SQL Server IaaS Agent in the background.</span></span> <span data-ttu-id="954ef-240">Během této doby nemusí zobrazit na portálu Azure, že automatizované zálohování je nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="954ef-240">During this time, the Azure portal might not show that Automated Backup is configured.</span></span> <span data-ttu-id="954ef-241">Počkejte několik minut, než agent, který se má nainstalovat, nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="954ef-241">Wait several minutes for the agent to be installed, configured.</span></span> <span data-ttu-id="954ef-242">Potom se projeví na portálu Azure nové nastavení.</span><span class="sxs-lookup"><span data-stu-id="954ef-242">After that the Azure portal will reflect the new settings.</span></span>

## <a name="configuration-with-powershell"></a><span data-ttu-id="954ef-243">Konfigurace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="954ef-243">Configuration with PowerShell</span></span>

<span data-ttu-id="954ef-244">Prostředí PowerShell můžete použít ke konfiguraci automatizovaného zálohování v2.</span><span class="sxs-lookup"><span data-stu-id="954ef-244">You can use PowerShell to configure Automated Backup v2.</span></span> <span data-ttu-id="954ef-245">Než začnete, musíte provést tyto akce:</span><span class="sxs-lookup"><span data-stu-id="954ef-245">Before you begin, you must:</span></span>

- <span data-ttu-id="954ef-246">[Stáhněte a nainstalujte nejnovější prostředí Azure PowerShell](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="954ef-246">[Download and install the latest Azure PowerShell](http://aka.ms/webpi-azps).</span></span>
- <span data-ttu-id="954ef-247">Otevřete prostředí Windows PowerShell a přidružit svůj účet.</span><span class="sxs-lookup"><span data-stu-id="954ef-247">Open Windows PowerShell and associate it with your account.</span></span> <span data-ttu-id="954ef-248">To provedete podle kroků v [konfigurovat předplatné](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) zřizování tématu.</span><span class="sxs-lookup"><span data-stu-id="954ef-248">You can do this by following the steps in the [Configure your subscription](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) section of the provisioning topic.</span></span>

### <a name="install-the-sql-iaas-extension"></a><span data-ttu-id="954ef-249">Nainstalujte rozšíření IaaS SQL</span><span class="sxs-lookup"><span data-stu-id="954ef-249">Install the SQL IaaS Extension</span></span>
<span data-ttu-id="954ef-250">Pokud zřízení virtuálního počítače s SQL serverem na portálu Azure IaaS rozšíření systému SQL Server musí být již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="954ef-250">If you provisioned a SQL Server virtual machine from the Azure portal, the SQL Server IaaS Extension should already be installed.</span></span> <span data-ttu-id="954ef-251">Můžete určit, pokud je nainstalovaná pro virtuální počítač voláním **Get-AzureRmVM** příkaz a prozkoumání **rozšíření** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="954ef-251">You can determine if it is installed for your VM by calling **Get-AzureRmVM** command and examining the **Extensions** property.</span></span>

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions 
```

<span data-ttu-id="954ef-252">Pokud je nainstalovaná rozšíření IaaS agenta systému SQL Server, měli byste vidět, že je uveden jako "SqlIaaSAgent" nebo "SQLIaaSExtension".</span><span class="sxs-lookup"><span data-stu-id="954ef-252">If the SQL Server IaaS Agent extension is installed, you should see it listed as “SqlIaaSAgent” or “SQLIaaSExtension”.</span></span> <span data-ttu-id="954ef-253">**Stav zřizování** pro rozšíření by měl také zobrazit, "Succeeded".</span><span class="sxs-lookup"><span data-stu-id="954ef-253">**ProvisioningState** for the extension should also show “Succeeded”.</span></span> 

<span data-ttu-id="954ef-254">Pokud není nainstalovaná nebo se nepovedlo zřídit, můžete ho nainstalovat pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="954ef-254">If it is not installed or failed to be provisioned, you can install it with the following command.</span></span> <span data-ttu-id="954ef-255">Kromě názvu a prostředek skupiny virtuálních počítačů, je nutné také zadat oblast (**$region**) umístěnou ve virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="954ef-255">In addition to the VM name and resource group, you must also specify the region (**$region**) that your VM is located in.</span></span>

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region 
```

### <span data-ttu-id="954ef-256"><a id="verifysettings"></a>Ověřte aktuální nastavení</span><span class="sxs-lookup"><span data-stu-id="954ef-256"><a id="verifysettings"></a> Verify current settings</span></span>
<span data-ttu-id="954ef-257">Pokud jste povolili automatizované zálohování při zřizování, můžete použít PowerShell ke kontrole aktuální konfigurace.</span><span class="sxs-lookup"><span data-stu-id="954ef-257">If you enabled automated backup during provisioning, you can use PowerShell to check your current configuration.</span></span> <span data-ttu-id="954ef-258">Spustit **Get-AzureRmVMSqlServerExtension** příkaz a zkontrolujte **AutoBackupSettings** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="954ef-258">Run the **Get-AzureRmVMSqlServerExtension** command and examine the **AutoBackupSettings** property:</span></span>

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

<span data-ttu-id="954ef-259">Měli byste obdržet výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="954ef-259">You should get output similar to the following:</span></span>

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

<span data-ttu-id="954ef-260">Pokud vaše výstup ukazuje, že **povolit** je nastaven na **False**, budete muset povolit automatizované zálohování.</span><span class="sxs-lookup"><span data-stu-id="954ef-260">If your output shows that **Enable** is set to **False**, then you have to enable automated backup.</span></span> <span data-ttu-id="954ef-261">Dobrá zpráva je, že můžete povolit a nakonfigurovat automatizovaného zálohování stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="954ef-261">The good news is that you enable and configure Automated Backup in the same way.</span></span> <span data-ttu-id="954ef-262">Najdete v další části pro tyto informace.</span><span class="sxs-lookup"><span data-stu-id="954ef-262">See the next section for this information.</span></span>

> [!NOTE] 
> <span data-ttu-id="954ef-263">Pokud zaškrtnete okamžitě po provedení změny nastavení, je možné, zobrazí se zpět původní hodnoty konfigurace.</span><span class="sxs-lookup"><span data-stu-id="954ef-263">If you check the settings immediately after making a change, it is possible that you will get back the old configuration values.</span></span> <span data-ttu-id="954ef-264">Počkejte několik minut a zkontrolujte nastavení znovu a ujistěte se, že byly použity změny.</span><span class="sxs-lookup"><span data-stu-id="954ef-264">Wait a few minutes and check the settings again to make sure that your changes were applied.</span></span>

### <a name="configure-automated-backup-v2"></a><span data-ttu-id="954ef-265">Konfigurace automatického zálohování v2</span><span class="sxs-lookup"><span data-stu-id="954ef-265">Configure Automated Backup v2</span></span>
<span data-ttu-id="954ef-266">Prostředí PowerShell můžete použít k povolení automatizované zálohování i, kdykoli upravit jeho konfiguraci a chování.</span><span class="sxs-lookup"><span data-stu-id="954ef-266">You can use PowerShell to enable Automated Backup as well as to modify its configuration and behavior at any time.</span></span> 

<span data-ttu-id="954ef-267">Nejdřív vyberte nebo vytvořte účet úložiště pro záložní soubory.</span><span class="sxs-lookup"><span data-stu-id="954ef-267">First, select or create a storage account for the backup files.</span></span> <span data-ttu-id="954ef-268">Následující skript vybere účet úložiště, nebo ji vytvoří, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="954ef-268">The following script selects a storage account or creates it if it does not exist.</span></span>

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
> <span data-ttu-id="954ef-269">Automatizované zálohování nepodporuje ukládání záloh v storage úrovně premium, ale může trvat zálohy z disků virtuálních počítačů, které používají úložiště Premium.</span><span class="sxs-lookup"><span data-stu-id="954ef-269">Automated Backup does not support storing backups in premium storage, but it can take backups from VM disks which use Premium Storage.</span></span>

<span data-ttu-id="954ef-270">Potom pomocí **New-AzureRmVMSqlServerAutoBackupConfig** příkaz povolení a konfigurace nastavení automatizovaného zálohování v2 pro ukládání záloh v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="954ef-270">Then use the **New-AzureRmVMSqlServerAutoBackupConfig** command to enable and configure the Automated Backup v2 settings to store backups in the Azure storage account.</span></span> <span data-ttu-id="954ef-271">V tomto příkladu jsou zálohy nastaveny pro zachování 10 dní.</span><span class="sxs-lookup"><span data-stu-id="954ef-271">In this example, the backups are set to be retained for 10 days.</span></span> <span data-ttu-id="954ef-272">Zálohování databáze systému jsou povolené.</span><span class="sxs-lookup"><span data-stu-id="954ef-272">System database backups are enabled.</span></span> <span data-ttu-id="954ef-273">Úplné zálohy jsou naplánovány pro každý týden s časovým oknem začínající na 20:00 pro dvě hodiny.</span><span class="sxs-lookup"><span data-stu-id="954ef-273">Full backups are scheduled for weekly with a time window starting at 20:00 for two hours.</span></span> <span data-ttu-id="954ef-274">Zálohy protokolů jsou naplánovány pro každých 30 minut.</span><span class="sxs-lookup"><span data-stu-id="954ef-274">Log backups are scheduled for every 30 minutes.</span></span> <span data-ttu-id="954ef-275">V druhém příkazu **Set-AzureRmVMSqlServerExtension**, aktualizuje zadaný virtuální počítač Azure s těmito nastaveními.</span><span class="sxs-lookup"><span data-stu-id="954ef-275">The second command, **Set-AzureRmVMSqlServerExtension**, updates the specified Azure VM with these settings.</span></span>

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

<span data-ttu-id="954ef-276">Ho může trvat několik minut k instalaci a konfiguraci IaaS Agent serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="954ef-276">It could take several minutes to install and configure the SQL Server IaaS Agent.</span></span> 

<span data-ttu-id="954ef-277">Chcete-li povolit šifrování, změňte předchozí skript, který chcete předat **EnableEncryption** společně s heslo (zabezpečený řetězec) pro parametr **CertificatePassword** parametr.</span><span class="sxs-lookup"><span data-stu-id="954ef-277">To enable encryption, modify the previous script to pass the **EnableEncryption** parameter along with a password (secure string) for the **CertificatePassword** parameter.</span></span> <span data-ttu-id="954ef-278">Následující skript umožňuje nastavení automatizovaného zálohování v předchozím příkladu a přidá šifrování.</span><span class="sxs-lookup"><span data-stu-id="954ef-278">The following script enables the Automated Backup settings in the previous example and adds encryption.</span></span>

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

<span data-ttu-id="954ef-279">Potvrďte nastavení se použijí, [ověřit konfiguraci automatizovaného zálohování](#verifysettings).</span><span class="sxs-lookup"><span data-stu-id="954ef-279">To confirm your settings are applied, [verify the Automated Backup configuration](#verifysettings).</span></span>

### <a name="disable-automated-backup"></a><span data-ttu-id="954ef-280">Zakázat automatizované zálohování</span><span class="sxs-lookup"><span data-stu-id="954ef-280">Disable Automated Backup</span></span>
<span data-ttu-id="954ef-281">Pokud chcete zakázat automatizovaného zálohování, spusťte stejný skriptu bez **-povolit** parametru **New-AzureRmVMSqlServerAutoBackupConfig** příkaz.</span><span class="sxs-lookup"><span data-stu-id="954ef-281">To disable Automated Backup, run the same script without the **-Enable** parameter to the **New-AzureRmVMSqlServerAutoBackupConfig** command.</span></span> <span data-ttu-id="954ef-282">Neexistence **-povolit** parametr signály příkaz funkci zakážete.</span><span class="sxs-lookup"><span data-stu-id="954ef-282">The absence of the **-Enable** parameter signals the command to disable the feature.</span></span> <span data-ttu-id="954ef-283">Stejně jako u instalace, se může trvat několik minut zakázat automatizovaného zálohování.</span><span class="sxs-lookup"><span data-stu-id="954ef-283">As with installation, it can take several minutes to disable Automated Backup.</span></span>

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a><span data-ttu-id="954ef-284">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="954ef-284">Example script</span></span>
<span data-ttu-id="954ef-285">Následující skript představuje sadu proměnných, které můžete přizpůsobit povolit a konfigurovat automatizované zálohování pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="954ef-285">The following script provides a set of variables that you can customize to enable and configure Automated Backup for your VM.</span></span> <span data-ttu-id="954ef-286">V váš případ může být nutné přizpůsobit skript podle svých požadavků.</span><span class="sxs-lookup"><span data-stu-id="954ef-286">In your case, you might need to customize the script based on your requirements.</span></span> <span data-ttu-id="954ef-287">Například nutné provést změny, pokud chcete zakázat zálohování databází systému nebo povolit šifrování.</span><span class="sxs-lookup"><span data-stu-id="954ef-287">For example, you would have to make changes if you wanted to disable the backup of system databases or enable encryption.</span></span>

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

# ResourceGroupName is the resource group which is hosting the VM where you are deploying the SQL IaaS Extension 

Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region

# Creates/use a storage account to store the backups

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

# Apply the Automated Backup settings to the VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a><span data-ttu-id="954ef-288">Další kroky</span><span class="sxs-lookup"><span data-stu-id="954ef-288">Next steps</span></span>
<span data-ttu-id="954ef-289">Automatizované zálohování v2 nakonfiguruje spravovaného zálohování na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="954ef-289">Automated Backup v2 configures Managed Backup on Azure VMs.</span></span> <span data-ttu-id="954ef-290">Proto je důležité [najdete v dokumentaci pro spravovanou zálohu](https://msdn.microsoft.com/library/dn449496.aspx) pochopit chování a důsledky.</span><span class="sxs-lookup"><span data-stu-id="954ef-290">So it is important to [review the documentation for Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) to understand the behavior and implications.</span></span>

<span data-ttu-id="954ef-291">Můžete najít další zálohování a obnovení pokyny pro SQL Server na virtuálních počítačích Azure v následujícím tématu: [zálohování a obnovení pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="954ef-291">You can find additional backup and restore guidance for SQL Server on Azure VMs in the following topic: [Backup and Restore for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).</span></span>

<span data-ttu-id="954ef-292">Informace o dalších úlohách, k dispozici automation najdete v tématu [rozšíření agenta systému SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="954ef-292">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

<span data-ttu-id="954ef-293">Další informace o spuštění systému SQL Server na virtuálních počítačích Azure najdete v tématu [SQL Server na virtuálních počítačích Azure přehled](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="954ef-293">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

