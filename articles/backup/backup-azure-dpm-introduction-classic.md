---
title: "Zálohování úloh DPM na portál Azure classic | Microsoft Docs"
description: "Úvod k zálohování serverů aplikace DPM pomocí služby zálohování Azure"
services: backup
documentationcenter: 
author: Nkolli1
manager: shreeshd
editor: 
keywords: "System Center Data Protection Manager, nástroje data protection manager, zálohy aplikace dpm"
ms.assetid: 8f23972b-d167-4231-b331-e198db3b18b4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: nkolli;giridham;markgal
ms.openlocfilehash: a9a516cfdfaf4b95c4f0121a66e90f6e71206e9f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a><span data-ttu-id="a514d-104">Příprava zálohování úloh do Azure pomocí DPM</span><span class="sxs-lookup"><span data-stu-id="a514d-104">Preparing to back up workloads to Azure with DPM</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a514d-105">Azure Backup Server</span><span class="sxs-lookup"><span data-stu-id="a514d-105">Azure Backup Server</span></span>](backup-azure-microsoft-azure-backup.md)
> * [<span data-ttu-id="a514d-106">SCDPM</span><span class="sxs-lookup"><span data-stu-id="a514d-106">SCDPM</span></span>](backup-azure-dpm-introduction.md)
> * [<span data-ttu-id="a514d-107">Server Azure Backup (klasické)</span><span class="sxs-lookup"><span data-stu-id="a514d-107">Azure Backup Server (Classic)</span></span>](backup-azure-microsoft-azure-backup-classic.md)
> * [<span data-ttu-id="a514d-108">SCDPM (klasické)</span><span class="sxs-lookup"><span data-stu-id="a514d-108">SCDPM (Classic)</span></span>](backup-azure-dpm-introduction-classic.md)
>
>

<span data-ttu-id="a514d-109">Tento článek obsahuje úvod do Microsoft Azure Backup používá k ochraně vašich serverů System Center Data Protection Manager (DPM) a úloh.</span><span class="sxs-lookup"><span data-stu-id="a514d-109">This article provides an introduction to using Microsoft Azure Backup to protect your System Center Data Protection Manager (DPM) servers and workloads.</span></span> <span data-ttu-id="a514d-110">Načtením ji budete porozumíte:</span><span class="sxs-lookup"><span data-stu-id="a514d-110">By reading it, you’ll understand:</span></span>

* <span data-ttu-id="a514d-111">Jak funguje Azure zálohování serveru</span><span class="sxs-lookup"><span data-stu-id="a514d-111">How Azure DPM server backup works</span></span>
* <span data-ttu-id="a514d-112">Požadavky pro dosažení smooth zálohování prostředí</span><span class="sxs-lookup"><span data-stu-id="a514d-112">The prerequisites to achieve a smooth backup experience</span></span>
* <span data-ttu-id="a514d-113">Typické došlo k chybám a řešení problémů s nimi</span><span class="sxs-lookup"><span data-stu-id="a514d-113">The typical errors encountered and how to deal with them</span></span>
* <span data-ttu-id="a514d-114">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="a514d-114">Supported scenarios</span></span>

<span data-ttu-id="a514d-115">System Center DPM zálohuje data souborů a aplikací.</span><span class="sxs-lookup"><span data-stu-id="a514d-115">System Center DPM backs up file and application data.</span></span> <span data-ttu-id="a514d-116">Data zálohovaná na DPM můžete uložit na pásce, na disku, nebo zálohovat do Azure se zálohováním Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="a514d-116">Data backed up to DPM can be stored on tape, on disk, or backed up to Azure with Microsoft Azure Backup.</span></span> <span data-ttu-id="a514d-117">Aplikace DPM komunikuje s Azure Backup následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a514d-117">DPM interacts with Azure Backup as follows:</span></span>

* <span data-ttu-id="a514d-118">**Aplikace DPM nasazená jako fyzický server nebo místní virtuální počítač** – Pokud aplikace DPM je nasazená jako fyzický server nebo jako virtuální počítač technologie Hyper-V místní data můžete zálohovat do trezoru služby Azure Backup kromě disku a pásky Zálohování.</span><span class="sxs-lookup"><span data-stu-id="a514d-118">**DPM deployed as a physical server or on-premises virtual machine** — If DPM is deployed as a physical server or as an on-premises Hyper-V virtual machine you can back up data to an Azure Backup vault in addition to disk and tape backup.</span></span>
* <span data-ttu-id="a514d-119">**Aplikace DPM nasazená jako virtuální počítač Azure** – ze System Center 2012 R2 s aktualizací 3, DPM dá nasadit jako virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="a514d-119">**DPM deployed as an Azure virtual machine** — From System Center 2012 R2 with Update 3, DPM can be deployed as an Azure virtual machine.</span></span> <span data-ttu-id="a514d-120">Pokud je aplikace DPM nasazená jako virtuální počítač Azure, které můžete zálohovat data na disky Azure připojené k virtuálnímu počítači DPM Azure nebo může přenést úložiště data prostřednictvím jejich zálohování až do trezoru zálohování Azure.</span><span class="sxs-lookup"><span data-stu-id="a514d-120">If DPM is deployed as an Azure virtual machine you can back up data to Azure disks attached to the DPM Azure virtual machine, or you can offload the data storage by backing it up to an Azure Backup vault.</span></span>

## <a name="why-backup-your-dpm-servers"></a><span data-ttu-id="a514d-121">Proč zálohovat servery DPM?</span><span class="sxs-lookup"><span data-stu-id="a514d-121">Why backup your DPM servers?</span></span>
<span data-ttu-id="a514d-122">Mezi výhody firmy pomocí služby Azure Backup k zálohování serverů aplikace DPM patří:</span><span class="sxs-lookup"><span data-stu-id="a514d-122">The business benefits of using Azure Backup for backing up DPM servers include:</span></span>

* <span data-ttu-id="a514d-123">Pro místní nasazení aplikace DPM můžete použít zálohování Azure jako alternativu k dlouhodobé nasazení na pásku.</span><span class="sxs-lookup"><span data-stu-id="a514d-123">For on-premises DPM deployment, you can use Azure backup as an alternative to long-term deployment to tape.</span></span>
* <span data-ttu-id="a514d-124">Pro nasazení aplikace DPM v Azure Azure Backup k přesměrování zpracování úloh úložiště z disku Azure vám umožní škálování uložením starších data v Azure Backup a nových dat na disku.</span><span class="sxs-lookup"><span data-stu-id="a514d-124">For DPM deployments in Azure, Azure Backup allows you to offload storage from the Azure disk, allowing you to scale up by storing older data in Azure Backup and new data on disk.</span></span>

## <a name="how-does-dpm-server-backup-work"></a><span data-ttu-id="a514d-125">Funkce Zálohování serveru aplikace DPM</span><span class="sxs-lookup"><span data-stu-id="a514d-125">How does DPM server backup work?</span></span>
<span data-ttu-id="a514d-126">Zálohování virtuálního počítače, nejprve v okamžiku snímek dat je potřeba.</span><span class="sxs-lookup"><span data-stu-id="a514d-126">To back up a virtual machine, first a point-in-time snapshot of the data is needed.</span></span> <span data-ttu-id="a514d-127">Služba Azure Backup v naplánovaném čase spustí úlohu zálohování a aktivuje rozšíření zálohování k pořízení snímku.</span><span class="sxs-lookup"><span data-stu-id="a514d-127">The Azure Backup service initiates the backup job at the scheduled time, and triggers the backup extension to take a snapshot.</span></span> <span data-ttu-id="a514d-128">Rozšíření zálohování koordinuje spolu se službou VSS v hosta k zajištění konzistence a vyvolá rozhraní API snímku objektu blob služby Azure Storage dosažení konzistence.</span><span class="sxs-lookup"><span data-stu-id="a514d-128">The backup extension coordinates with the in-guest VSS service to achieve consistency, and invokes the blob snapshot API of the Azure Storage service once consistency has been reached.</span></span> <span data-ttu-id="a514d-129">To slouží k získání konzistentního snímku disků virtuálního počítače, aniž by bylo nutné ho vypnout.</span><span class="sxs-lookup"><span data-stu-id="a514d-129">This is done to get a consistent snapshot of the disks of the virtual machine, without having to shut it down.</span></span>

<span data-ttu-id="a514d-130">Po provedení výpisu data se přenáší přes služba Azure Backup do trezoru záloh.</span><span class="sxs-lookup"><span data-stu-id="a514d-130">After the snapshot has been taken, the data is transferred by the Azure Backup service to the backup vault.</span></span> <span data-ttu-id="a514d-131">Služba má na starosti identifikace a přenáší pouze bloky, které se změnily od poslední zálohy zvýšení efektivity sítě a úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="a514d-131">The service takes care of identifying and transferring only the blocks that have changed from the last backup making the backups storage and network efficient.</span></span> <span data-ttu-id="a514d-132">Po dokončení přenosu dat se odebere snímku a vytvoří bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="a514d-132">When the data transfer is completed, the snapshot is removed and a recovery point is created.</span></span> <span data-ttu-id="a514d-133">Tento bod obnovení lze zobrazit v portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="a514d-133">This recovery point can be seen in the  Azure classic portal.</span></span>

> [!NOTE]
> <span data-ttu-id="a514d-134">Pro virtuální počítače s Linuxem je možné pouze soubor zálohování s konzistentními.</span><span class="sxs-lookup"><span data-stu-id="a514d-134">For Linux virtual machines, only file-consistent backup is possible.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="a514d-135">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a514d-135">Prerequisites</span></span>
<span data-ttu-id="a514d-136">Příprava Azure Backup k zálohování dat aplikace DPM následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a514d-136">Prepare Azure Backup to back up DPM data as follows:</span></span>

1. <span data-ttu-id="a514d-137">**Vytvořte úložiště záloh**.</span><span class="sxs-lookup"><span data-stu-id="a514d-137">**Create a Backup vault**.</span></span> <span data-ttu-id="a514d-138">Pokud jste dosud nevytvořili úložiště záloh ve vašem předplatném, najdete v části portálu Azure verzi tohoto článku - [Příprava zálohování úloh do Azure pomocí DPM](backup-azure-dpm-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a514d-138">If you haven't created a Backup vault in your subscription, see the Azure portal version of this article - [Prepare to back up workloads to Azure with DPM](backup-azure-dpm-introduction.md).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="a514d-139">Od března 2017 již nelze k vytvoření trezorů služby Backup použít portál Classic.</span><span class="sxs-lookup"><span data-stu-id="a514d-139">Starting March 2017, you can no longer use the classic portal to create Backup vaults.</span></span>
  > <span data-ttu-id="a514d-140">Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="a514d-140">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="a514d-141">Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="a514d-141">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="a514d-142">Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="a514d-142">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="a514d-143">Od 15. října 2017 nebude možné pomocí PowerShellu vytvářet trezory služby Backup.</span><span class="sxs-lookup"><span data-stu-id="a514d-143">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="a514d-144">**Do 1. listopadu 2017:**</span><span class="sxs-lookup"><span data-stu-id="a514d-144">**By November 1, 2017**:</span></span>
  >- <span data-ttu-id="a514d-145">Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="a514d-145">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
  >- <span data-ttu-id="a514d-146">Nebudete mít přístup k datům záloh na portálu Classic.</span><span class="sxs-lookup"><span data-stu-id="a514d-146">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="a514d-147">Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="a514d-147">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
  >

2. <span data-ttu-id="a514d-148">**Stažení přihlašovacích údajů trezoru** – ve službě Azure Backup odešlete certifikát správy, který jste vytvořili do trezoru.</span><span class="sxs-lookup"><span data-stu-id="a514d-148">**Download vault credentials** — In Azure Backup, upload the management certificate you created to the vault.</span></span>
3. <span data-ttu-id="a514d-149">**Instalace agenta zálohování Azure a zaregistrujte serveru** – z Azure Backup, nainstalujte agenta na každém serveru DPM a registrace serveru DPM v trezoru záloh.</span><span class="sxs-lookup"><span data-stu-id="a514d-149">**Install the Azure Backup Agent and register the server** — From Azure Backup, install the agent on each DPM server and register the DPM server in the backup vault.</span></span>

[!INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[!INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]

## <a name="requirements-and-limitations"></a><span data-ttu-id="a514d-150">Požadavky (a omezení)</span><span class="sxs-lookup"><span data-stu-id="a514d-150">Requirements (and limitations)</span></span>
* <span data-ttu-id="a514d-151">Aplikace DPM může být spuštěná jako fyzický server nebo virtuální počítač technologie Hyper-V nainstalovaná v System Center 2012 SP1 nebo System Center 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="a514d-151">DPM can be running as a physical server or a Hyper-V virtual machine installed on System Center 2012 SP1 or System Center 2012 R2.</span></span> <span data-ttu-id="a514d-152">Můžete také používat jako virtuální počítač Azure používá aspoň na System Center 2012 R2 s kumulativní aktualizace 3 pro DPM 2012 R2 nebo virtuálního počítače s Windows v prostředí VMWare alespoň systémem System Center 2012 R2 s kumulativní aktualizací 5.</span><span class="sxs-lookup"><span data-stu-id="a514d-152">It can also be running as an Azure virtual machine running on System Center 2012 R2 with at least DPM 2012 R2 Update Rollup 3 or a Windows virtual machine in VMWare running on System Center 2012 R2 with at least Update Rollup 5.</span></span>
* <span data-ttu-id="a514d-153">Pokud spouštíte aplikaci DPM s nástrojem System Center 2012 SP1, nainstalujte kumulativní aktualizaci 2 pro System Center Data Protection Manager SP1.</span><span class="sxs-lookup"><span data-stu-id="a514d-153">If you’re running DPM with System Center 2012 SP1, you should install Update Rollup 2 for System Center Data Protection Manager SP1.</span></span> <span data-ttu-id="a514d-154">To je potřeba, před instalací programu Azure Backup Agent.</span><span class="sxs-lookup"><span data-stu-id="a514d-154">This is required before you can install the Azure Backup Agent.</span></span>
* <span data-ttu-id="a514d-155">Server aplikace DPM by měl mít prostředí Windows PowerShell a rozhraní .net Framework 4.5 nainstalované.</span><span class="sxs-lookup"><span data-stu-id="a514d-155">The DPM server should have Windows PowerShell and .Net Framework 4.5 installed.</span></span>
* <span data-ttu-id="a514d-156">Aplikace DPM můžete zálohovat většinu úloh do služby Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="a514d-156">DPM can back up most workloads to Azure Backup.</span></span> <span data-ttu-id="a514d-157">Úplný seznam, co je podporováno najdete podporovat Azure Backup položky dole.</span><span class="sxs-lookup"><span data-stu-id="a514d-157">For a full list of what’s supported see the Azure Backup support items below.</span></span>
* <span data-ttu-id="a514d-158">Data uložená ve službě Azure Backup nelze obnovit pomocí možnosti "Kopírovat na pásku".</span><span class="sxs-lookup"><span data-stu-id="a514d-158">Data stored in Azure Backup can’t be recovered with the “copy to tape” option.</span></span>
* <span data-ttu-id="a514d-159">Budete potřebovat účet Azure s povolenou funkcí zálohování Azure.</span><span class="sxs-lookup"><span data-stu-id="a514d-159">You’ll need an Azure account with the Azure Backup feature enabled.</span></span> <span data-ttu-id="a514d-160">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="a514d-160">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="a514d-161">Přečtěte si informace o [cenách zálohování Azure](https://azure.microsoft.com/pricing/details/backup/).</span><span class="sxs-lookup"><span data-stu-id="a514d-161">Read about [Azure Backup pricing](https://azure.microsoft.com/pricing/details/backup/).</span></span>
* <span data-ttu-id="a514d-162">Použití zálohování Azure vyžaduje Azure Backup Agent k instalaci na serverech, které chcete zálohovat.</span><span class="sxs-lookup"><span data-stu-id="a514d-162">Using Azure Backup requires the Azure Backup Agent to be installed on the servers you want to back up.</span></span> <span data-ttu-id="a514d-163">Každý server musí mít minimálně 10 % velikosti dat, která se zálohuje, k dispozici jako místního volného úložného.</span><span class="sxs-lookup"><span data-stu-id="a514d-163">Each server must have at least 10% of the size of the data that is being backed up, available as local free storage.</span></span> <span data-ttu-id="a514d-164">Například zálohování 100 GB dat vyžaduje minimálně 10 GB volného místa v pomocné umístění.</span><span class="sxs-lookup"><span data-stu-id="a514d-164">For example, backing up 100 GB of data requires a minimum of 10 GB of free space in the scratch location.</span></span> <span data-ttu-id="a514d-165">Při minimální hodnota je 10 %, se doporučuje 15 % místního volného úložného místa pro umístění mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="a514d-165">While the minimum is 10%, 15% of free local storage space to be used for the cache location is recommended.</span></span>
* <span data-ttu-id="a514d-166">Data budou uložena v úložišti Azure trezoru.</span><span class="sxs-lookup"><span data-stu-id="a514d-166">Data will be stored in the Azure vault storage.</span></span> <span data-ttu-id="a514d-167">Neexistuje žádné omezení množství dat, které jste můžete zálohovat Azure Backup trezoru, ale velikost zdroje dat (třeba virtuální počítač nebo databáze) by neměl být delší než 54,400 GB.</span><span class="sxs-lookup"><span data-stu-id="a514d-167">There’s no limit to the amount of data you can back up to an Azure Backup vault but the size of a data source (for example a virtual machine or database) shouldn’t exceed 54,400 GB.</span></span>

<span data-ttu-id="a514d-168">Tyto typy souborů jsou podporovány pro zálohování na Azure:</span><span class="sxs-lookup"><span data-stu-id="a514d-168">These file types are supported for back up to Azure:</span></span>

* <span data-ttu-id="a514d-169">Šifrované (pouze úplné zálohy)</span><span class="sxs-lookup"><span data-stu-id="a514d-169">Encrypted (Full backups only)</span></span>
* <span data-ttu-id="a514d-170">Komprimované (je podporováno přírůstkové zálohování)</span><span class="sxs-lookup"><span data-stu-id="a514d-170">Compressed (Incremental backups supported)</span></span>
* <span data-ttu-id="a514d-171">Zhuštěné (je podporováno přírůstkové zálohování)</span><span class="sxs-lookup"><span data-stu-id="a514d-171">Sparse (Incremental backups supported)</span></span>
* <span data-ttu-id="a514d-172">Komprimované a zhuštěné (zpracovány jako zhuštěné)</span><span class="sxs-lookup"><span data-stu-id="a514d-172">Compressed and sparse (Treated as Sparse)</span></span>

<span data-ttu-id="a514d-173">A tyto nejsou podporovány:</span><span class="sxs-lookup"><span data-stu-id="a514d-173">And these are unsupported:</span></span>

* <span data-ttu-id="a514d-174">Servery v systémech souborů s rozlišením velkých nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="a514d-174">Servers on case-sensitive file systems aren’t supported.</span></span>
* <span data-ttu-id="a514d-175">Pevné odkazy (vynecháno)</span><span class="sxs-lookup"><span data-stu-id="a514d-175">Hard links (Skipped)</span></span>
* <span data-ttu-id="a514d-176">Body rozboru (vynecháno)</span><span class="sxs-lookup"><span data-stu-id="a514d-176">Reparse points (Skipped)</span></span>
* <span data-ttu-id="a514d-177">Zašifrované a komprimované (vynecháno)</span><span class="sxs-lookup"><span data-stu-id="a514d-177">Encrypted and compressed (Skipped)</span></span>
* <span data-ttu-id="a514d-178">Šifrované a zhuštěné (vynecháno)</span><span class="sxs-lookup"><span data-stu-id="a514d-178">Encrypted and sparse (Skipped)</span></span>
* <span data-ttu-id="a514d-179">Komprimovaný datový proud</span><span class="sxs-lookup"><span data-stu-id="a514d-179">Compressed stream</span></span>
* <span data-ttu-id="a514d-180">Zhuštěný datový proud</span><span class="sxs-lookup"><span data-stu-id="a514d-180">Sparse stream</span></span>

> [!NOTE]
> <span data-ttu-id="a514d-181">Z v System Center DPM 2012 s aktualizací SP1 a vyšší, můžete zálohovat do úlohy, které jsou chráněné pomocí DPM do Azure pomocí služby Microsoft Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="a514d-181">From in System Center 2012 DPM with SP1 onwards, you can backup up workloads protected by DPM to Azure using Microsoft Azure Backup.</span></span>
>
>