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
# <a name="upgrade-a-site-recovery-vault-tooan-azure-resource-manager-based-recovery-services-vault"></a>Upgrade trezoru Site Recovery tooan trezoru služeb zotavení založené na Azure Resource Manager

Tento článek popisuje, jak tooupgrade Azure Site Recovery trezory trezory služby zotavení založené na správci prostředků tooAzure bez žádný vliv na probíhající replikace. Další informace o funkce služby Správce prostředků Azure a výhod najdete v tématu [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).

## <a name="introduction"></a>Úvod
Trezor služeb zotavení je prostředek Azure Resource Manageru pro správu zálohování a zotavení po havárii nativně v cloudu hello. Je jednotná trezoru, který můžete použít v hello nový portál Azure, se nahradí hello classic zálohování a trezory Site Recovery.

Trezory služeb zotavení nabízí řadu funkcí, včetně:

* Podpora Azure Resource Manager: můžete chránit a převzetí služeb při selhání virtuálních počítačů a fyzických počítačů do Azure Resource Manager balíku.

* Vyloučení disku: Pokud máte dočasné soubory nebo vysokou změn dat, že nechcete, aby toouse všechny vaše šířka pásma pro, můžete z replikace vyloučit svazky. Tato funkce byla povolena aktuálně *VMware tooAzure* a *tooAzure technologie Hyper-V* a je rozšířeno také tooother scénáře.

* Podpora pro premium a místně redundantního úložiště: teď mohou chránit servery v storage úrovně premium účty, které umožňují zákazníkům tooprotect aplikace s vyšší vstupně-výstupních operací za sekundu (IOPS). Tato funkce je aktuálně povoleno v *VMware tooAzure*.

* Zjednodušený Začínáme prostředí: Začínáme prostředí hello rozšířeného byl navržen tak, instalace zotavení po havárii toomake snadné.

* Zálohování a obnovení lokality správy z hello stejnému trezoru: nyní může chránit servery pro zotavení po havárii nebo proveďte zálohu z hello stejnému trezoru, což může snížit režie výrazně vaší správy.

Další informace o prostředí hello upgradovat a funkcích najdete v tématu hello [úložiště, zálohování a obnovení blog](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).

## <a name="salient-features"></a>Nejdůležitějšími funkce

* Žádný vliv na probíhající replikace: probíhající replikace pokračovat bez výpadku během a post upgradu.

* Bez dalších nákladů: Získejte celou sadu aktualizované funkcí bez dalších poplatků.

* Nedošlo ke ztrátě dat: protože tento proces je upgrade a není migrace, stávajících bodů obnovení replikace a nastavení zůstanou beze změn během a po provedení upgradu hello.


## <a name="what-happens-during-hello-vault-upgrade"></a>Co se stane během upgradu hello trezoru?

Během upgradu hello nelze provádět operace jako je registrace nového serveru nebo povolení replikace pro virtuální počítač (VM). Operace, které zahrnují čtení dat z nebo zápis trezoru toohello data, jako je například probíhající replikace chráněné položky toohello trezor, bez přerušení pokračovat.

### <a name="changes-tooautomation-and-tooling-after-hello-upgrade"></a>Změny tooautomation a nástrojů po upgradu hello
Při upgradu hello trezoru typ z modelu nasazení classic hello, toohello modelu nasazení Resource Manager, aktualizujte existující automatizace hello nebo nástrojů tooensure pokračuje toowork po upgradu hello.

### <a name="prepare-your-environment-for-hello-upgrade"></a>Příprava prostředí pro hello upgrade

* [Instalace prostředí PowerShell nebo upgradovat tooversion 5 nebo novější](https://www.microsoft.com/download/details.aspx?id=50395)
* [Nainstalujte nejnovější verzi Azure PowerShell MSI hello](https://github.com/Azure/azure-powershell/releases)
* [Stáhnout skriptu aktualizace trezoru služeb zotavení hello](https://aka.ms/vaultupgradescript)

### <a name="prerequisites"></a>Požadavky
tooupgrade ze služby Site Recovery trezory tooAzure trezory služby zotavení využívající Resource Manager, musí splňovat následující požadavky hello:

* Verze agenta minimální: hello verzi Azure Site Recovery Provider nainstalovaný na serveru musí být 5.1.1700.0 nebo novější.

* Podporované konfigurace: trezoru nelze konfigurovat pomocí sítě SAN (SAN) nebo skupiny dostupnosti AlwaysOn serveru SQL. Jsou podporovány všechny ostatní konfigurace.

    >[!NOTE]
    >Po upgradu hello můžete spravovat úložiště mapování pouze pomocí prostředí PowerShell.

* Podporované scénáře: trezoru by neměl být hello *VMware tooAzure* modelu starší verze nasazení. Než budete pokračovat, nejprve přesunete modelu toohello rozšířeného nasazení.

* Žádné aktivní úlohy spouštěné uživateli, které zahrnují správu roviny, operace: protože během upgradu je omezený přístup toohello správu plocha, dokončete všechny akce správy roviny předtím, než aktivujete hello upgradu. Tento proces neobsahuje probíhající replikace.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

**Vliv tento upgrade Moje probíhající replikace?**

Ne. Probíhající replikace bude pokračovat bez přerušení, během a po provedení upgradu hello.

**Co se stane toonetwork nastavení třeba site-to-site VPN a IP adresy nastavení?**

Hello upgrade neovlivní nastavení sítě hello. Všechna připojení Azure na místní zůstanou beze změn.

**Pokud nemáte plánuji tooupgrade v blízké budoucnosti hello co se stane toomy trezory?**

Podpora pro trezor Site Recovery na portálu Azure staré hello se od září 2017 zastaralé. Důrazně doporučujeme použít hello funkce upgrade toomove toohello nového portálu.

**Co znamená tento plán migrace pro moje existující nástrojů?**  

Aktualizace modelu nasazení Resource Manager toohello nástrojů je jedním z hello nejdůležitější změny, které musíte vzít v úvahu pro v upgradu plánu. trezory služeb zotavení Hello jsou založené na modelu nasazení Resource Manager hello. 

**Jak dlouho hello správu roviny výpadek poslední?**

Hello upgrade obvykle trvá asi 15 minut too30 a může trvat až tooa maximálně jednu hodinu.

**Můžete I vrátit po provedení upgradu?**

Ne. Vrácení zpět není podporována po úspěšném upgradu hello prostředky.

**Můžete I ověřit Moje předplatné nebo prostředky toosee jestli může být upgradována?**

Ano. V hello platforma podporovaná možnost pro upgrade hello prvním krokem při upgradu hello je toovalidate, zda jsou prostředky hello schopna upgradu. Pokud hello ověření nezdaří, zobrazí se příslušná chybové zprávy nebo upozornění.

**Jak mohu ohlásit problém s nástrojem hello upgrade?**

Pokud dojde k jeho selhání při upgradu hello, poznamenejte si ID operace hello, který je uvedený v chybě hello. Microsoft Support fungovaly proaktivně v řešení problému hello. Můžete také kontaktovat tým podpory hello s ID předplatného, název trezoru a ID operace. Podpora bude fungovat co nejrychleji tooresolve hello problém. Nezkoušejte zopakovat operaci hello, pokud si nejste explicitně pokyn toodo Ano společností Microsoft.

## <a name="run-hello-script"></a>Spusťte skript hello

V prostředí PowerShell spusťte následující příkaz hello:

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionID <subscriptionID>  -VaultName <vaultname> -Location <location> -ResourceType HyperVRecoveryManagerVault -TargetResourceGroupName <rgname>

* ID předplatného: hello ID odběru, který je spojen s hello trezoru, který upgradujete.

* VaultName: název hello hello úložiště, který upgradujete.

* Umístění: umístění hello hello trezoru, který upgradujete.

* Typ prostředku: Trezory HyperVRecoveryManagerVault Site Recovery.

* TargetResourceGroupName: Skupina prostředků hello, do kterého chcete hello upgradovat trezoru toobe umístit. TargetResourceGroupName může být existující skupinu prostředků v Azure Resource Manager nebo novou. Pokud hello TargetResourceGroupName, která se dodává neexistuje, bude vytvořen jako součást upgradu hello v hello stejné umístění jako trezor hello. Další informace najdete v části "Prostředku skupiny" hello [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md#resource-groups).

    >[!NOTE]
    >Názvy skupin prostředků je omezení toocertain subjektu. Trezor tooprevent upgrade nezdaří, pečlivě být jisti tooobserve hello zásady vytváření názvů.
    >
    >Například:
    >
    >.\RecoveryServicesVaultUpgrade-1.0.0.ps1 - SubscriptionId 1234-54123-354354-56416-8645 - VaultName gen2dr-umístění "Severní Evropa" - ResourceType hypervrecoverymanagervault - TargetResourceGroupName abc

Alternativně můžete spustit následující skript hello. Zadejte hodnoty hello hello požadované parametry.

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1
    cmdlet RecoveryServicesVaultUpgrade-1.0.0.ps1 at command pipeline position 1

    Supply values for hello following parameters:
    SubscriptionId:
    VaultName:
    Location:
    ResourceType:
    TargetResourceGroupName:

1. Hello skript prostředí PowerShell vyzve k zadání tooenter jste svoje přihlašovací údaje. Zadejte jejich dvakrát, jednou pro účet modelu nasazení classic hello a jednou pro hello účet Azure Resource Manager.

2. Po zadání přihlašovacích údajů, hello skript se spustí kontrola toodetermine zda vaší infrastruktury instalace splňuje hello už jsme zmínili požadavky.

3. Po hello požadavky byly zkontrolovat a potvrdit, jste výzvami tooproceed hello trezoru upgradu. Spustí se proces upgradu Hello upgrade svůj trezor. Hello celý upgrade může trvat 15 minut toocomplete too30.

4. Po upgradu hello byla úspěšně dokončena, dostanete hello upgradovaný trezoru hello novou verzi portálu Azure.

## <a name="post-upgrade-vault-management"></a>Po upgradu trezoru správy

### <a name="replicate-by-using-azure-site-recovery-in-hello-recovery-services-vault"></a>Replikace pomocí Azure Site Recovery v hello trezoru služeb zotavení

* Nyní můžete chránit virtuální počítače Azure z jedné oblasti tooanother. Další informace najdete v tématu [replikovat virtuální počítače Azure mezi oblastmi s Azure Site Recovery](site-recovery-azure-to-azure.md).

* Další informace o replikaci virtuálních počítačů VMware tooAzure najdete v tématu [tooAzure replikovat virtuální počítače VMware s Site Recovery](vmware-walkthrough-overview.md).

* Další informace o replikaci tooAzure virtuálních počítačů technologie Hyper-V (bez VMM) najdete v tématu [replikaci technologie Hyper-V virtuální počítače (bez VMM) tooAzure](hyper-v-site-walkthrough-overview.md).

* Další informace o replikaci tooAzure virtuálních počítačů Hyper-V (s nástrojem VMM) najdete v tématu [hello replikaci technologie Hyper-V virtuální počítače v tooAzure cloudů VMM pomocí Site Recovery na portálu Azure](vmm-to-azure-walkthrough-overview.md).

* Další informace o replikaci technologie Hyper-virtuálních počítačů (s VMM) tooa sekundární lokality najdete v tématu [hello replikaci technologie Hyper-V virtuální počítače v nástroji VMM cloudy tooa sekundární VMM lokality pomocí portálu Azure](site-recovery-vmm-to-vmm.md).

* Další informace o replikaci virtuálních počítačů VMware tooa sekundární lokality najdete v tématu [replikovat místní virtuální počítače VMware nebo fyzických serverů tooa sekundární lokality na portálu Azure classic hello](site-recovery-vmware-to-vmware.md).

### <a name="view-your-replicated-items"></a>Zobrazení replikované položky

Hello následující obrázek znázorňuje hello služeb zotavení trezoru řídicí panel stránky, která zobrazuje klíče entity pro hello trezoru. Vyberte tooview seznam chráněných entit v trezoru hello **Site Recovery** > **replikované položky**.


![Replikované položky](./media/upgrade-site-recovery-vaults/replicateditems.png)

Hello následující obrázek ukazuje pracovní postup hello k zobrazení replikované položky a hello **převzetí služeb při selhání** příkaz pro inicializaci převzetí služeb při selhání.

![Replikované položky](./media/upgrade-site-recovery-vaults/failover.png)

### <a name="view-your-replication-settings"></a>Zobrazit nastavení replikace

V trezoru Site Recovery hello každou skupinu ochrany je nastavena frekvence kopírování, uchování bodu obnovení, frekvence snímků konzistentní aplikace a další nastavení replikace. V trezoru služeb zotavení hello tato nastavení jsou konfigurována jako zásady replikace. Dobrý název zásady hello je hello název skupiny ochrany hello nebo hello *primarycloud_Policy*.

Další informace o zásadách replikace najdete v tématu [správě zásad replikace pro VMware tooAzure](site-recovery-setup-replication-settings-vmware.md).
