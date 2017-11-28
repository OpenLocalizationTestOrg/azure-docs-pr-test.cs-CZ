---
title: "Trezor služeb zotavení Azure tooan aaaUpgrade trezoru Site Recovery"
description: "Zjistěte, jak tooupgrade trezoru služby Azure Site Recovery tooa trezor služeb zotavení"
documentationcenter: 
author: rajani-janaki-ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/31/2017
ms.author: rajani-janaki-ram
ms.openlocfilehash: a18a923ee3bad91873e654c9b9b5bf8b83acc123
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-site-recovery-vault-tooan-azure-resource-manager-based-recovery-services-vault"></a><span data-ttu-id="fecaa-103">Upgrade trezoru Site Recovery tooan trezoru služeb zotavení založené na Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="fecaa-103">Upgrade a Site Recovery vault tooan Azure Resource Manager-based Recovery Services vault</span></span>

<span data-ttu-id="fecaa-104">Tento článek popisuje, jak tooupgrade Azure Site Recovery trezory trezory služby zotavení založené na správci prostředků tooAzure bez žádný vliv na probíhající replikace.</span><span class="sxs-lookup"><span data-stu-id="fecaa-104">This article describes how tooupgrade Azure Site Recovery vaults tooAzure Resource Manager-based Recovery Service vaults without any impact on ongoing replication.</span></span> <span data-ttu-id="fecaa-105">Další informace o funkce služby Správce prostředků Azure a výhod najdete v tématu [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fecaa-105">For more information about Azure Resource Manager features and benefits, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="fecaa-106">Úvod</span><span class="sxs-lookup"><span data-stu-id="fecaa-106">Introduction</span></span>
<span data-ttu-id="fecaa-107">Trezor služeb zotavení je prostředek Azure Resource Manageru pro správu zálohování a zotavení po havárii nativně v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="fecaa-107">A Recovery Services vault is an Azure Resource Manager resource for managing backup and disaster recovery natively in hello cloud.</span></span> <span data-ttu-id="fecaa-108">Je jednotná trezoru, který můžete použít v hello nový portál Azure, se nahradí hello classic zálohování a trezory Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="fecaa-108">It is a unified vault that you can use in hello new Azure portal, and it replaces hello classic backup and Site Recovery vaults.</span></span>

<span data-ttu-id="fecaa-109">Trezory služeb zotavení nabízí řadu funkcí, včetně:</span><span class="sxs-lookup"><span data-stu-id="fecaa-109">Recovery Services vaults offer an array of features, including:</span></span>

* <span data-ttu-id="fecaa-110">Podpora Azure Resource Manager: můžete chránit a převzetí služeb při selhání virtuálních počítačů a fyzických počítačů do Azure Resource Manager balíku.</span><span class="sxs-lookup"><span data-stu-id="fecaa-110">Azure Resource Manager support: You can protect and fail over your virtual machines and physical machines into an Azure Resource Manager stack.</span></span>

* <span data-ttu-id="fecaa-111">Vyloučení disku: Pokud máte dočasné soubory nebo vysokou změn dat, že nechcete, aby toouse všechny vaše šířka pásma pro, můžete z replikace vyloučit svazky.</span><span class="sxs-lookup"><span data-stu-id="fecaa-111">Exclude disk: If you have temp files or high churn data that you don’t want toouse all your bandwidth for, you can exclude volumes from replication.</span></span> <span data-ttu-id="fecaa-112">Tato funkce byla povolena aktuálně *VMware tooAzure* a *tooAzure technologie Hyper-V* a je rozšířeno také tooother scénáře.</span><span class="sxs-lookup"><span data-stu-id="fecaa-112">This capability has been enabled currently in *VMware tooAzure* and *Hyper-V tooAzure* and is extended tooother scenarios as well.</span></span>

* <span data-ttu-id="fecaa-113">Podpora pro premium a místně redundantního úložiště: teď mohou chránit servery v storage úrovně premium účty, které umožňují zákazníkům tooprotect aplikace s vyšší vstupně-výstupních operací za sekundu (IOPS).</span><span class="sxs-lookup"><span data-stu-id="fecaa-113">Support for premium and locally redundant storage: You can now protect servers in premium storage accounts that allow customers tooprotect applications with higher input/output operations per second (IOPS).</span></span> <span data-ttu-id="fecaa-114">Tato funkce je aktuálně povoleno v *VMware tooAzure*.</span><span class="sxs-lookup"><span data-stu-id="fecaa-114">This capability is currently enabled in *VMware tooAzure*.</span></span>

* <span data-ttu-id="fecaa-115">Zjednodušený Začínáme prostředí: Začínáme prostředí hello rozšířeného byl navržen tak, instalace zotavení po havárii toomake snadné.</span><span class="sxs-lookup"><span data-stu-id="fecaa-115">Streamlined getting-started experience: hello enhanced getting-started experience has been designed toomake disaster-recovery setup easy.</span></span>

* <span data-ttu-id="fecaa-116">Zálohování a obnovení lokality správy z hello stejnému trezoru: nyní může chránit servery pro zotavení po havárii nebo proveďte zálohu z hello stejnému trezoru, což může snížit režie výrazně vaší správy.</span><span class="sxs-lookup"><span data-stu-id="fecaa-116">Backup and Site Recovery management from hello same vault: You can now protect servers for disaster recovery or perform backup from hello same vault, which can reduce your management overhead significantly.</span></span>

<span data-ttu-id="fecaa-117">Další informace o prostředí hello upgradovat a funkcích najdete v tématu hello [úložiště, zálohování a obnovení blog](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).</span><span class="sxs-lookup"><span data-stu-id="fecaa-117">For more information about hello upgraded experience and features, see hello [Storage, Backup, and Recovery blog](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).</span></span>

## <a name="salient-features"></a><span data-ttu-id="fecaa-118">Nejdůležitějšími funkce</span><span class="sxs-lookup"><span data-stu-id="fecaa-118">Salient features</span></span>

* <span data-ttu-id="fecaa-119">Žádný vliv na probíhající replikace: probíhající replikace pokračovat bez výpadku během a post upgradu.</span><span class="sxs-lookup"><span data-stu-id="fecaa-119">No impact on ongoing replication: Ongoing replications continue without any interruption during and post upgrade.</span></span>

* <span data-ttu-id="fecaa-120">Bez dalších nákladů: Získejte celou sadu aktualizované funkcí bez dalších poplatků.</span><span class="sxs-lookup"><span data-stu-id="fecaa-120">No additional cost: Get an entire set of updated capabilities at no additional cost.</span></span>

* <span data-ttu-id="fecaa-121">Nedošlo ke ztrátě dat: protože tento proces je upgrade a není migrace, stávajících bodů obnovení replikace a nastavení zůstanou beze změn během a po provedení upgradu hello.</span><span class="sxs-lookup"><span data-stu-id="fecaa-121">No data loss: Because this process is an upgrade and not a migration, existing replication recovery points and settings remain intact during and after hello upgrade.</span></span>


## <a name="what-happens-during-hello-vault-upgrade"></a><span data-ttu-id="fecaa-122">Co se stane během upgradu hello trezoru?</span><span class="sxs-lookup"><span data-stu-id="fecaa-122">What happens during hello vault upgrade?</span></span>

<span data-ttu-id="fecaa-123">Během upgradu hello nelze provádět operace jako je registrace nového serveru nebo povolení replikace pro virtuální počítač (VM).</span><span class="sxs-lookup"><span data-stu-id="fecaa-123">During hello upgrade, you cannot perform operations such as registering a new server or enabling replication for a virtual machine (VM).</span></span> <span data-ttu-id="fecaa-124">Operace, které zahrnují čtení dat z nebo zápis trezoru toohello data, jako je například probíhající replikace chráněné položky toohello trezor, bez přerušení pokračovat.</span><span class="sxs-lookup"><span data-stu-id="fecaa-124">Operations that involve reading data from or writing data toohello vault, such as ongoing replication of protected items toohello vault, continue uninterrupted.</span></span>

### <a name="changes-tooautomation-and-tooling-after-hello-upgrade"></a><span data-ttu-id="fecaa-125">Změny tooautomation a nástrojů po upgradu hello</span><span class="sxs-lookup"><span data-stu-id="fecaa-125">Changes tooautomation and tooling after hello upgrade</span></span>
<span data-ttu-id="fecaa-126">Při upgradu hello trezoru typ z modelu nasazení classic hello, toohello modelu nasazení Resource Manager, aktualizujte existující automatizace hello nebo nástrojů tooensure pokračuje toowork po upgradu hello.</span><span class="sxs-lookup"><span data-stu-id="fecaa-126">As you upgrade hello vault type from hello classic deployment model toohello Resource Manager deployment model, update hello existing automation or tooling tooensure that it continues toowork after hello upgrade.</span></span>

### <a name="prepare-your-environment-for-hello-upgrade"></a><span data-ttu-id="fecaa-127">Příprava prostředí pro hello upgrade</span><span class="sxs-lookup"><span data-stu-id="fecaa-127">Prepare your environment for hello upgrade</span></span>

* [<span data-ttu-id="fecaa-128">Instalace prostředí PowerShell nebo upgradovat tooversion 5 nebo novější</span><span class="sxs-lookup"><span data-stu-id="fecaa-128">Install PowerShell or upgrade it tooversion 5 or later</span></span>](https://www.microsoft.com/download/details.aspx?id=50395)
* [<span data-ttu-id="fecaa-129">Nainstalujte nejnovější verzi Azure PowerShell MSI hello</span><span class="sxs-lookup"><span data-stu-id="fecaa-129">Install hello latest version of Azure PowerShell MSI</span></span>](https://github.com/Azure/azure-powershell/releases)
* [<span data-ttu-id="fecaa-130">Stáhnout skriptu aktualizace trezoru služeb zotavení hello</span><span class="sxs-lookup"><span data-stu-id="fecaa-130">Download hello Recovery Services vault upgrade script</span></span>](https://aka.ms/vaultupgradescript)

### <a name="prerequisites"></a><span data-ttu-id="fecaa-131">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fecaa-131">Prerequisites</span></span>
<span data-ttu-id="fecaa-132">tooupgrade ze služby Site Recovery trezory tooAzure trezory služby zotavení využívající Resource Manager, musí splňovat následující požadavky hello:</span><span class="sxs-lookup"><span data-stu-id="fecaa-132">tooupgrade from Site Recovery vaults tooAzure Resource Manager-based Recovery Service vaults, you must meet hello following requirements:</span></span>

* <span data-ttu-id="fecaa-133">Verze agenta minimální: hello verzi Azure Site Recovery Provider nainstalovaný na serveru musí být 5.1.1700.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="fecaa-133">Minimum agent version: hello version of Azure Site Recovery Provider installed on your server must be 5.1.1700.0 or later.</span></span>

* <span data-ttu-id="fecaa-134">Podporované konfigurace: trezoru nelze konfigurovat pomocí sítě SAN (SAN) nebo skupiny dostupnosti AlwaysOn serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="fecaa-134">Supported configuration: You cannot configure your vault with storage area network (SAN) or SQL Server AlwaysOn Availability Groups.</span></span> <span data-ttu-id="fecaa-135">Jsou podporovány všechny ostatní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="fecaa-135">All other configurations are supported.</span></span>

    >[!NOTE]
    ><span data-ttu-id="fecaa-136">Po upgradu hello můžete spravovat úložiště mapování pouze pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fecaa-136">After hello upgrade, you can manage storage mapping only via PowerShell.</span></span>

* <span data-ttu-id="fecaa-137">Podporované scénáře: trezoru by neměl být hello *VMware tooAzure* modelu starší verze nasazení.</span><span class="sxs-lookup"><span data-stu-id="fecaa-137">Supported deployment scenario: Your vault shouldn’t be hello *VMware tooAzure* legacy deployment model.</span></span> <span data-ttu-id="fecaa-138">Než budete pokračovat, nejprve přesunete modelu toohello rozšířeného nasazení.</span><span class="sxs-lookup"><span data-stu-id="fecaa-138">Before you proceed, first move toohello enhanced deployment model.</span></span>

* <span data-ttu-id="fecaa-139">Žádné aktivní úlohy spouštěné uživateli, které zahrnují správu roviny, operace: protože během upgradu je omezený přístup toohello správu plocha, dokončete všechny akce správy roviny předtím, než aktivujete hello upgradu.</span><span class="sxs-lookup"><span data-stu-id="fecaa-139">No active user-initiated jobs that involve management plane operations: Because access toohello management plane is restricted during upgrade, complete all your management plane actions before you trigger hello upgrade.</span></span> <span data-ttu-id="fecaa-140">Tento proces neobsahuje probíhající replikace.</span><span class="sxs-lookup"><span data-stu-id="fecaa-140">This process doesn’t include ongoing replication.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="fecaa-141">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="fecaa-141">Frequently asked questions</span></span>

<span data-ttu-id="fecaa-142">**Vliv tento upgrade Moje probíhající replikace?**</span><span class="sxs-lookup"><span data-stu-id="fecaa-142">**Does this upgrade affect my ongoing replication?**</span></span>

<span data-ttu-id="fecaa-143">Ne.</span><span class="sxs-lookup"><span data-stu-id="fecaa-143">No.</span></span> <span data-ttu-id="fecaa-144">Probíhající replikace bude pokračovat bez přerušení, během a po provedení upgradu hello.</span><span class="sxs-lookup"><span data-stu-id="fecaa-144">Your ongoing replication continues uninterrupted during and after hello upgrade.</span></span>

<span data-ttu-id="fecaa-145">**Co se stane toonetwork nastavení třeba site-to-site VPN a IP adresy nastavení?**</span><span class="sxs-lookup"><span data-stu-id="fecaa-145">**What happens toonetwork settings such as site-to-site VPN and IP settings?**</span></span>

<span data-ttu-id="fecaa-146">Hello upgrade neovlivní nastavení sítě hello.</span><span class="sxs-lookup"><span data-stu-id="fecaa-146">hello upgrade doesn't affect hello network settings.</span></span> <span data-ttu-id="fecaa-147">Všechna připojení Azure na místní zůstanou beze změn.</span><span class="sxs-lookup"><span data-stu-id="fecaa-147">All Azure-to-on-premises connections remain intact.</span></span>

<span data-ttu-id="fecaa-148">**Pokud nemáte plánuji tooupgrade v blízké budoucnosti hello co se stane toomy trezory?**</span><span class="sxs-lookup"><span data-stu-id="fecaa-148">**What happens toomy vaults if I don’t plan tooupgrade in hello near future?**</span></span>

<span data-ttu-id="fecaa-149">Podpora pro trezor Site Recovery na portálu Azure staré hello se od září 2017 zastaralé.</span><span class="sxs-lookup"><span data-stu-id="fecaa-149">Support for Site Recovery vault in hello old Azure portal will be deprecated starting September 2017.</span></span> <span data-ttu-id="fecaa-150">Důrazně doporučujeme použít hello funkce upgrade toomove toohello nového portálu.</span><span class="sxs-lookup"><span data-stu-id="fecaa-150">We strongly recommend that you use hello upgrade feature toomove toohello new portal.</span></span>

<span data-ttu-id="fecaa-151">**Co znamená tento plán migrace pro moje existující nástrojů?**</span><span class="sxs-lookup"><span data-stu-id="fecaa-151">**What does this migration plan mean for my existing tooling?**</span></span>  

<span data-ttu-id="fecaa-152">Aktualizace modelu nasazení Resource Manager toohello nástrojů je jedním z hello nejdůležitější změny, které musíte vzít v úvahu pro v upgradu plánu.</span><span class="sxs-lookup"><span data-stu-id="fecaa-152">Updating your tooling toohello Resource Manager deployment model is one of hello most important changes that you must account for in your upgrade plans.</span></span> <span data-ttu-id="fecaa-153">trezory služeb zotavení Hello jsou založené na modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="fecaa-153">hello Recovery Services vaults are based on hello Resource Manager deployment model.</span></span> 

<span data-ttu-id="fecaa-154">**Jak dlouho hello správu roviny výpadek poslední?**</span><span class="sxs-lookup"><span data-stu-id="fecaa-154">**How long does hello management-plane downtime last?**</span></span>

<span data-ttu-id="fecaa-155">Hello upgrade obvykle trvá asi 15 minut too30 a může trvat až tooa maximálně jednu hodinu.</span><span class="sxs-lookup"><span data-stu-id="fecaa-155">hello upgrade ordinarily takes about 15 too30 minutes, and it could take up tooa maximum of one hour.</span></span>

<span data-ttu-id="fecaa-156">**Můžete I vrátit po provedení upgradu?**</span><span class="sxs-lookup"><span data-stu-id="fecaa-156">**Can I roll back after upgrading?**</span></span>

<span data-ttu-id="fecaa-157">Ne.</span><span class="sxs-lookup"><span data-stu-id="fecaa-157">No.</span></span> <span data-ttu-id="fecaa-158">Vrácení zpět není podporována po úspěšném upgradu hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="fecaa-158">Rollback is not supported after hello resources have been successfully upgraded.</span></span>

<span data-ttu-id="fecaa-159">**Můžete I ověřit Moje předplatné nebo prostředky toosee jestli může být upgradována?**</span><span class="sxs-lookup"><span data-stu-id="fecaa-159">**Can I validate my subscription or resources toosee whether they can be upgraded?**</span></span>

<span data-ttu-id="fecaa-160">Ano.</span><span class="sxs-lookup"><span data-stu-id="fecaa-160">Yes.</span></span> <span data-ttu-id="fecaa-161">V hello platforma podporovaná možnost pro upgrade hello prvním krokem při upgradu hello je toovalidate, zda jsou prostředky hello schopna upgradu.</span><span class="sxs-lookup"><span data-stu-id="fecaa-161">In hello platform-supported upgrade option, hello first step in hello upgrade is toovalidate that hello resources are capable of an upgrade.</span></span> <span data-ttu-id="fecaa-162">Pokud hello ověření nezdaří, zobrazí se příslušná chybové zprávy nebo upozornění.</span><span class="sxs-lookup"><span data-stu-id="fecaa-162">If hello validation fails, you will receive appropriate error messages or warnings.</span></span>

<span data-ttu-id="fecaa-163">**Jak mohu ohlásit problém s nástrojem hello upgrade?**</span><span class="sxs-lookup"><span data-stu-id="fecaa-163">**How do I report an issue with hello upgrade?**</span></span>

<span data-ttu-id="fecaa-164">Pokud dojde k jeho selhání při upgradu hello, poznamenejte si ID operace hello, který je uvedený v chybě hello.</span><span class="sxs-lookup"><span data-stu-id="fecaa-164">If you experience any failures during hello upgrade, note hello operation ID that's listed in hello error.</span></span> <span data-ttu-id="fecaa-165">Microsoft Support fungovaly proaktivně v řešení problému hello.</span><span class="sxs-lookup"><span data-stu-id="fecaa-165">Microsoft Support will proactively work on resolving hello issue.</span></span> <span data-ttu-id="fecaa-166">Můžete také kontaktovat tým podpory hello s ID předplatného, název trezoru a ID operace.</span><span class="sxs-lookup"><span data-stu-id="fecaa-166">You can also contact hello Support team with your subscription ID, vault name, and operation ID.</span></span> <span data-ttu-id="fecaa-167">Podpora bude fungovat co nejrychleji tooresolve hello problém.</span><span class="sxs-lookup"><span data-stu-id="fecaa-167">Support will work tooresolve hello issue as quickly as possible.</span></span> <span data-ttu-id="fecaa-168">Nezkoušejte zopakovat operaci hello, pokud si nejste explicitně pokyn toodo Ano společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="fecaa-168">Do not retry hello operation unless you are explicitly instructed toodo so by Microsoft.</span></span>

## <a name="run-hello-script"></a><span data-ttu-id="fecaa-169">Spusťte skript hello</span><span class="sxs-lookup"><span data-stu-id="fecaa-169">Run hello script</span></span>

<span data-ttu-id="fecaa-170">V prostředí PowerShell spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="fecaa-170">In PowerShell, run hello following command:</span></span>

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionID <subscriptionID>  -VaultName <vaultname> -Location <location> -ResourceType HyperVRecoveryManagerVault -TargetResourceGroupName <rgname>

* <span data-ttu-id="fecaa-171">ID předplatného: hello ID odběru, který je spojen s hello trezoru, který upgradujete.</span><span class="sxs-lookup"><span data-stu-id="fecaa-171">SubscriptionID: hello subscription ID that's associated with hello vault that you're upgrading.</span></span>

* <span data-ttu-id="fecaa-172">VaultName: název hello hello úložiště, který upgradujete.</span><span class="sxs-lookup"><span data-stu-id="fecaa-172">VaultName: hello name of hello vault that you're upgrading.</span></span>

* <span data-ttu-id="fecaa-173">Umístění: umístění hello hello trezoru, který upgradujete.</span><span class="sxs-lookup"><span data-stu-id="fecaa-173">Location: hello location of hello vault that you're upgrading.</span></span>

* <span data-ttu-id="fecaa-174">Typ prostředku: Trezory HyperVRecoveryManagerVault Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="fecaa-174">ResourceType: HyperVRecoveryManagerVault for Site Recovery vaults.</span></span>

* <span data-ttu-id="fecaa-175">TargetResourceGroupName: Skupina prostředků hello, do kterého chcete hello upgradovat trezoru toobe umístit.</span><span class="sxs-lookup"><span data-stu-id="fecaa-175">TargetResourceGroupName: hello resource group into which you want hello upgraded vault toobe placed.</span></span> <span data-ttu-id="fecaa-176">TargetResourceGroupName může být existující skupinu prostředků v Azure Resource Manager nebo novou.</span><span class="sxs-lookup"><span data-stu-id="fecaa-176">TargetResourceGroupName can be an existing resource group in Azure Resource Manager or a new one.</span></span> <span data-ttu-id="fecaa-177">Pokud hello TargetResourceGroupName, která se dodává neexistuje, bude vytvořen jako součást upgradu hello v hello stejné umístění jako trezor hello.</span><span class="sxs-lookup"><span data-stu-id="fecaa-177">If hello TargetResourceGroupName that's supplied does not exist, it is created as part of hello upgrade in hello same location as hello vault.</span></span> <span data-ttu-id="fecaa-178">Další informace najdete v části "Prostředku skupiny" hello [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="fecaa-178">For more information, see hello "Resource groups" section of [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>

    >[!NOTE]
    ><span data-ttu-id="fecaa-179">Názvy skupin prostředků je omezení toocertain subjektu.</span><span class="sxs-lookup"><span data-stu-id="fecaa-179">Resource group naming is subject toocertain constraints.</span></span> <span data-ttu-id="fecaa-180">Trezor tooprevent upgrade nezdaří, pečlivě být jisti tooobserve hello zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="fecaa-180">tooprevent vault upgrade failure, be sure tooobserve hello naming convention carefully.</span></span>
    >
    ><span data-ttu-id="fecaa-181">Například:</span><span class="sxs-lookup"><span data-stu-id="fecaa-181">For example:</span></span>
    >
    ><span data-ttu-id="fecaa-182">.\RecoveryServicesVaultUpgrade-1.0.0.ps1 - SubscriptionId 1234-54123-354354-56416-8645 - VaultName gen2dr-umístění "Severní Evropa" - ResourceType hypervrecoverymanagervault - TargetResourceGroupName abc</span><span class="sxs-lookup"><span data-stu-id="fecaa-182">.\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionId 1234-54123-354354-56416-8645 -VaultName gen2dr -Location "north europe" -ResourceType hypervrecoverymanagervault -TargetResourceGroupName abc</span></span>

<span data-ttu-id="fecaa-183">Alternativně můžete spustit následující skript hello.</span><span class="sxs-lookup"><span data-stu-id="fecaa-183">Alternatively, you can run hello following script.</span></span> <span data-ttu-id="fecaa-184">Zadejte hodnoty hello hello požadované parametry.</span><span class="sxs-lookup"><span data-stu-id="fecaa-184">Enter hello values for hello required parameters.</span></span>

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1
    cmdlet RecoveryServicesVaultUpgrade-1.0.0.ps1 at command pipeline position 1

    Supply values for hello following parameters:
    SubscriptionId:
    VaultName:
    Location:
    ResourceType:
    TargetResourceGroupName:

1. <span data-ttu-id="fecaa-185">Hello skript prostředí PowerShell vyzve k zadání tooenter jste svoje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="fecaa-185">hello PowerShell script prompts you tooenter your credentials.</span></span> <span data-ttu-id="fecaa-186">Zadejte jejich dvakrát, jednou pro účet modelu nasazení classic hello a jednou pro hello účet Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="fecaa-186">Enter them twice, once for hello classic deployment model account and once for hello Azure Resource Manager account.</span></span>

2. <span data-ttu-id="fecaa-187">Po zadání přihlašovacích údajů, hello skript se spustí kontrola toodetermine zda vaší infrastruktury instalace splňuje hello už jsme zmínili požadavky.</span><span class="sxs-lookup"><span data-stu-id="fecaa-187">After you've entered your credentials, hello script runs a check toodetermine whether your infrastructure setup meets hello previously mentioned requirements.</span></span>

3. <span data-ttu-id="fecaa-188">Po hello požadavky byly zkontrolovat a potvrdit, jste výzvami tooproceed hello trezoru upgradu.</span><span class="sxs-lookup"><span data-stu-id="fecaa-188">After hello prerequisites have been checked and confirmed, you are prompted tooproceed with hello vault upgrade.</span></span> <span data-ttu-id="fecaa-189">Spustí se proces upgradu Hello upgrade svůj trezor.</span><span class="sxs-lookup"><span data-stu-id="fecaa-189">hello upgrade process starts upgrading your vault.</span></span> <span data-ttu-id="fecaa-190">Hello celý upgrade může trvat 15 minut toocomplete too30.</span><span class="sxs-lookup"><span data-stu-id="fecaa-190">hello entire upgrade can take 15 too30 minutes toocomplete.</span></span>

4. <span data-ttu-id="fecaa-191">Po upgradu hello byla úspěšně dokončena, dostanete hello upgradovaný trezoru hello novou verzi portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fecaa-191">After hello upgrade has been completed successfully, you can access hello upgraded vault in hello new Azure portal.</span></span>

## <a name="post-upgrade-vault-management"></a><span data-ttu-id="fecaa-192">Po upgradu trezoru správy</span><span class="sxs-lookup"><span data-stu-id="fecaa-192">Post-upgrade vault management</span></span>

### <a name="replicate-by-using-azure-site-recovery-in-hello-recovery-services-vault"></a><span data-ttu-id="fecaa-193">Replikace pomocí Azure Site Recovery v hello trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="fecaa-193">Replicate by using Azure Site Recovery in hello Recovery Services vault</span></span>

* <span data-ttu-id="fecaa-194">Nyní můžete chránit virtuální počítače Azure z jedné oblasti tooanother.</span><span class="sxs-lookup"><span data-stu-id="fecaa-194">You can now protect your Azure VMs from one region tooanother.</span></span> <span data-ttu-id="fecaa-195">Další informace najdete v tématu [replikovat virtuální počítače Azure mezi oblastmi s Azure Site Recovery](site-recovery-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="fecaa-195">For more information, see [Replicate Azure VMs between regions with Azure Site Recovery](site-recovery-azure-to-azure.md).</span></span>

* <span data-ttu-id="fecaa-196">Další informace o replikaci virtuálních počítačů VMware tooAzure najdete v tématu [tooAzure replikovat virtuální počítače VMware s Site Recovery](vmware-walkthrough-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fecaa-196">For more information about replicating VMware VMs tooAzure, see [Replicate VMware VMs tooAzure with Site Recovery](vmware-walkthrough-overview.md).</span></span>

* <span data-ttu-id="fecaa-197">Další informace o replikaci tooAzure virtuálních počítačů technologie Hyper-V (bez VMM) najdete v tématu [replikaci technologie Hyper-V virtuální počítače (bez VMM) tooAzure](hyper-v-site-walkthrough-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fecaa-197">For more information about replicating Hyper-V VMs (without VMM) tooAzure, see [Replicate Hyper-V virtual machines (without VMM) tooAzure](hyper-v-site-walkthrough-overview.md).</span></span>

* <span data-ttu-id="fecaa-198">Další informace o replikaci tooAzure virtuálních počítačů Hyper-V (s nástrojem VMM) najdete v tématu [hello replikaci technologie Hyper-V virtuální počítače v tooAzure cloudů VMM pomocí Site Recovery na portálu Azure](vmm-to-azure-walkthrough-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fecaa-198">For more information about replicating Hyper-V VMs (with VMM) tooAzure, see [Replicate Hyper-V virtual machines in VMM clouds tooAzure using Site Recovery in hello Azure portal](vmm-to-azure-walkthrough-overview.md).</span></span>

* <span data-ttu-id="fecaa-199">Další informace o replikaci technologie Hyper-virtuálních počítačů (s VMM) tooa sekundární lokality najdete v tématu [hello replikaci technologie Hyper-V virtuální počítače v nástroji VMM cloudy tooa sekundární VMM lokality pomocí portálu Azure](site-recovery-vmm-to-vmm.md).</span><span class="sxs-lookup"><span data-stu-id="fecaa-199">For more information about replicating Hyper-VMs (with VMM) tooa secondary site, see [Replicate Hyper-V virtual machines in VMM clouds tooa secondary VMM site using hello Azure portal](site-recovery-vmm-to-vmm.md).</span></span>

* <span data-ttu-id="fecaa-200">Další informace o replikaci virtuálních počítačů VMware tooa sekundární lokality najdete v tématu [replikovat místní virtuální počítače VMware nebo fyzických serverů tooa sekundární lokality na portálu Azure classic hello](site-recovery-vmware-to-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="fecaa-200">For more information about replicating VMware VMs tooa secondary site, see [Replicate on-premises VMware virtual machines or physical servers tooa secondary site in hello classic Azure portal](site-recovery-vmware-to-vmware.md).</span></span>

### <a name="view-your-replicated-items"></a><span data-ttu-id="fecaa-201">Zobrazení replikované položky</span><span class="sxs-lookup"><span data-stu-id="fecaa-201">View your replicated items</span></span>

<span data-ttu-id="fecaa-202">Hello následující obrázek znázorňuje hello služeb zotavení trezoru řídicí panel stránky, která zobrazuje klíče entity pro hello trezoru.</span><span class="sxs-lookup"><span data-stu-id="fecaa-202">hello following image shows hello Recovery Services vault dashboard page that displays key entities for hello vault.</span></span> <span data-ttu-id="fecaa-203">Vyberte tooview seznam chráněných entit v trezoru hello **Site Recovery** > **replikované položky**.</span><span class="sxs-lookup"><span data-stu-id="fecaa-203">tooview a list of protected entities in hello vault, select **Site Recovery** > **Replicated items**.</span></span>


![Replikované položky](./media/upgrade-site-recovery-vaults/replicateditems.png)

<span data-ttu-id="fecaa-205">Hello následující obrázek ukazuje pracovní postup hello k zobrazení replikované položky a hello **převzetí služeb při selhání** příkaz pro inicializaci převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="fecaa-205">hello following image shows hello workflow for viewing your replicated items and hello **Failover** command for initiating a failover.</span></span>

![Replikované položky](./media/upgrade-site-recovery-vaults/failover.png)

### <a name="view-your-replication-settings"></a><span data-ttu-id="fecaa-207">Zobrazit nastavení replikace</span><span class="sxs-lookup"><span data-stu-id="fecaa-207">View your replication settings</span></span>

<span data-ttu-id="fecaa-208">V trezoru Site Recovery hello každou skupinu ochrany je nastavena frekvence kopírování, uchování bodu obnovení, frekvence snímků konzistentní aplikace a další nastavení replikace.</span><span class="sxs-lookup"><span data-stu-id="fecaa-208">In hello Site Recovery vault, each protection group is configured with copy frequency, recovery point retention, frequency of application consistent snapshots, and other replication settings.</span></span> <span data-ttu-id="fecaa-209">V trezoru služeb zotavení hello tato nastavení jsou konfigurována jako zásady replikace.</span><span class="sxs-lookup"><span data-stu-id="fecaa-209">In hello Recovery Services vault, these settings are configured as a replication policy.</span></span> <span data-ttu-id="fecaa-210">Dobrý název zásady hello je hello název skupiny ochrany hello nebo hello *primarycloud_Policy*.</span><span class="sxs-lookup"><span data-stu-id="fecaa-210">hello name of hello policy is hello name of hello protection group or hello *primarycloud_Policy*.</span></span>

<span data-ttu-id="fecaa-211">Další informace o zásadách replikace najdete v tématu [správě zásad replikace pro VMware tooAzure](site-recovery-setup-replication-settings-vmware.md).</span><span class="sxs-lookup"><span data-stu-id="fecaa-211">For more information about replication policy, see [Manage replication policy for VMware tooAzure](site-recovery-setup-replication-settings-vmware.md).</span></span>
