---
title: "úloh služby Azure Batch aaaRun na virtuálních počítačích nákladově efektivní s nízkou prioritou (Preview) | Microsoft Docs"
description: "Zjistěte, jak tooprovision nízkou prioritu virtuální počítače tooreduce hello náklady úloh Azure Batch."
services: batch
author: mscurrell
manager: timlt
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 07/21/2017
ms.author: markscu
ms.openlocfilehash: 91a5e89a819d05583e6b146932d925e217b4be4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-low-priority-vms-with-batch-preview"></a>Použití nízkou prioritu virtuálních počítačů pomocí služby Batch (Preview)

Azure Batch nabízí nízkou priorty virtuální počítače (VM) tooreduce hello náklady úloh služby Batch. Virtuální počítače s nízkou prioritou umožňují nové typy úloh služby Batch, tím, že poskytuje velké množství výpočetního výkonu, která je také ekonomické.

Virtuální počítače s nízkou prioritou využít výhod kapacitní v Azure. Když zadáte nízkou prioritu virtuálních počítačů ve fondech, Azure Batch mohou automaticky používat tento přebytek, pokud je k dispozici.

Hello kompromis pro používání virtuálních počítačů nízkou prioritu je, že tyto virtuální počítače může být zrušené při žádné kapacitní je k dispozici v Azure. Z toho důvodu jsou nejvhodnější pro některé typy úloh virtuálních počítačů s nízkou prioritou. Použijte virtuální počítače nízkou prioritu pro batch a asynchronní zpracování úloh, kde čas dokončení úlohy hello je flexibilní a pracovní hello je distribuován do mnoha virtuálních počítačů.

Virtuální počítače s nízkou prioritou jsou výrazně levnější než vyhrazených virtuálních počítačích. Podrobnosti o cenách najdete v části [ceny služby Batch](https://azure.microsoft.com/pricing/details/batch/).

Další informace o virtuálních počítačích nízkou prioritu, najdete v blogovém příspěvku hello: [dávky computing za zlomek ceny hello](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).

> [!IMPORTANT]
> Nízkou prioritu virtuální počítače jsou aktuálně ve verzi preview a jsou dostupné pouze pro úlohy běžící v dávce. 
>
>

## <a name="use-cases-for-low-priority-vms"></a>Případy použití pro virtuální počítače s nízkou prioritou

Zadané vlastnosti hello nízkou prioritu virtuálních počítačů, jaké úlohy může nebo nemůže používat je? Obecně platí zpracování úloh služby batch jsou vhodné, úlohy jsou rozdělená do mnoho paralelních úloh nebo existuje mnoho úloh, které jsou škálovat na více systémů a distribuován do mnoha virtuálních počítačů.

-   použití toomaximize kapacitní v Azure, vhodný úloh můžete škálovat.

-   Příležitostně virtuální počítače nemusí být k dispozici, nebo bude možné zrušené, který bude mít za následek menší kapacitu pro úlohy a může způsobit přerušení tootask a opakování. Proto musí být v době hello jejich zajištění může trvat toorun flexibilní úlohy.

-   Úlohy se delší úloh může být ovlivněno více, pokud dojde k přerušení. Pokud dlouho běžící, že úlohy implementovat vytváření kontrolních bodů průběhu toosave jako jejich provedení, pak dopad přerušení bude mnohem menší. Jako hello dopad přerušení je mnohem méně úlohy s kratší časy spouštění nejlépe zpravidla toowork s virtuálními počítači nízkou prioritu.

-   Dlouhotrvajících úloh MPI, které využívají víc virtuálních počítačů nejsou dobře hodí toouse nízkou prioritu virtuální počítače jako jeden zrušené virtuální počítač bude nejspíš realizace toohello celou úlohy s toobe spusťte znovu.

Některé příklady dávkové zpracování použijte případech skvěle hodí toouse, které jsou virtuální počítače nízkou prioritu:

-   **Vývoj a testování**: zejména, pokud jsou vyvíjených rozsáhlé řešení výrazné úspory může být dosaženo. Všechny typy testování může přinést výhody, ale ve velkém měřítku zátěžové testování a testování regrese jsou skvělý používá.

-   **Dodávání kapacity na vyžádání**: virtuální počítače s nízkou prioritou lze použít k doplnění regular vyhrazených virtuálních počítačích – Pokud je k dispozici, můžete škálovat a proto dokončení rychlejší nižší náklady úlohy; Pokud není k dispozici, hello účaří vyhrazené virtuální počítače jsou k dispozici.

-   **Čas spuštění úlohy flexibilní**: Pokud je flexibilitu v hello čas úlohy mají toocomplete, pak lze tolerovat potenciální vyřazuje kapacity, však s hello přidání nízkou prioritu úlohy virtuálních počítačů se často spustí rychleji a s nižšími náklady.

Fondy batch může být nakonfigurované toouse nízkou prioritu virtuálních počítačů v několika různými způsoby v závislosti na hello flexibilitu při úlohy čas spuštění:

-   Virtuální počítače nízkou prioritu lze použít pouze ve fondu a Batch bude jednoduše obnovit všechny preempted kapacity, pokud je k dispozici. Toto je hello nejlevnější způsob tooexecute úlohy se používají pouze nízkou prioritu virtuálních počítačů.

-   Virtuální počítače s nízkou prioritou lze použít ve spojení s pevnou směrného plánu vyhrazených virtuálních počítačů. Hello pevný počet vyhrazených virtuálních počítačích zajišťuje, že je vždy některé kapacity tookeep úlohy pokročíte.

-   Může být dynamické směs vyhrazené a nízkou prioritu virtuální počítače, aby levnější nízkou prioritu virtuálních počítačů se používají výhradně, pokud je k dispozici, ale za cenu úplné hello vyhrazené virtuální počítače jsou rozšířit tak, pokud jsou povinné, tookeep minimální množství kapacity k dispozici tookeep hello úlohy pokročíte.

## <a name="batch-support-for-low-priority-vms"></a>Batch podporu pro virtuální počítače s nízkou prioritou

Azure Batch poskytuje několik možností, které bylo snadné tooconsume a těžit z virtuálních počítačů nízkou prioritu:

-   Fondy batch může obsahovat vyhrazených virtuálních počítačích a virtuální počítače s nízkou prioritou. Hello počty jednotlivých typů virtuálních počítačů lze zadat, pokud fond je vytvořené nebo změněné kdykoli pro existující fond, pomocí operace změny velikosti explicitní hello nebo pomocí automatické škálování. Odeslání úlohy a úlohy může zůstat beze změny a nemusíte dělat starosti s typy hello virtuálního počítače ve fondu hello. Je také možné toohave fond úplně pomocí nízkou prioritu virtuální počítače toorun úloh jako možný, ale otočení až vyhrazených virtuálních počítačích levné Pokud hello kapacity klesne pod minimální prahové hodnoty, aby spuštěné úlohy.

-   Fondy batch automaticky hledat toohello cílový počet virtuálních počítačů, nízkou prioritu. Pokud přerušené virtuální počítače, se pokusí Batch tooreplace hello ztráty kapacity a návratové toohello cíle.

-   V případě hello úloh se přerušena Batch rozpozná a automaticky znovu toobe úlohy znovu spusťte.

-   Virtuální počítače s nízkou prioritou mají kvóta na jádra, která se liší od vyhrazených virtuálních počítačích. 
    Hello uvozovky pro virtuální počítače s nízkou prioritou je vyšší než u vyhrazených virtuálních počítačích, protože virtuální počítače s nízkou prioritou nižší náklady. V tématu [Batch, kvóty a omezení služby](batch-quota-limit.md#resource-quotas) Další informace.    

> [!NOTE]
> Virtuální počítače s nízkou prioritou nejsou aktuálně podporovány pro účty Batch, kde hello fondu přidělení režim je nastaven příliš[uživatele předplatné](batch-account-create-portal.md#user-subscription-mode).
>
>

## <a name="create-and-update-pools"></a>Vytváření a aktualizaci fondy

Fondu služby Batch může obsahovat vyhrazené a nízkou prioritu virtuální počítače (taky odkazované tooas výpočetní uzly). Pro vyhrazené i nízkou prioritu virtuální počítače můžete nastavit hello cílovým počtem výpočetních uzlů. Hello cílový počet uzlů určuje hello počet virtuálních počítačů, na které má toohave ve fondu hello.

Například toocreate fondu pomocí služby Azure cloud virtuálních počítačů s cílem 5 vyhrazené virtuální počítače a 20 virtuálních počítačů nízkou prioritu:

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4") // WS 2012 R2
);
```

toocreate fondu pomocí virtuální počítače Azure (v tomto případě virtuální počítače s Linuxem) s cílem 5 vyhrazené virtuální počítače a 20 virtuálních počítačů nízkou prioritu:

```csharp
ImageReference imageRef = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "16.04.0-LTS",
    version: "latest");

// Create hello pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration("batch.node.ubuntu 16.04", imageRef);

pool = batchClient.PoolOperations.CreatePool(
    poolId: "vmpool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard\_D2\_v2",
    virtualMachineConfiguration: virtualMachineConfiguration);
```

Aktuální počet uzlů hello můžete získat pro vyhrazené i nízkou prioritu virtuální počítače:

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

Uzly fondu mají vlastnost tooindicate, pokud uzel hello je vyhrazené nebo nízkou prioritu virtuálního počítače:

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

Přerušené jeden nebo více uzlů ve fondu, operace výpisu uzlů ve fondu pořád vrátí tyto uzly, aktuální počet uzlů nízkou prioritu hello zůstanou nezměněna, ale tyto uzly budou mít jejich stav nastaven toothe **přepnuto**stavu. Batch se pokusí toofind nahrazení virtuální počítače, a pokud bylo úspěšné, bude projít hello uzly **vytváření** a potom **počáteční** stavy poté jsou dostupné pro provádění úkolů, stejně jako nové uzly.

## <a name="scale-a-pool-containing-low-priority-vms"></a>Škálování fond, který obsahuje virtuální počítače s nízkou prioritou

Stejně jako u fondy výhradně skládající se z vyhrazených virtuálních počítačích, je možné tooscale fondu obsahujícího nízkou prioritu virtuálních počítačů pomocí volání metody změny velikosti hello nebo pomocí automatické škálování.

Hello operace změny velikosti fondu trvá druhý volitelný parametr, který aktualizuje hodnotu **targetLowPriorityNodes**:

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

Vzorec automatickému škálování fondu Hello podporuje virtuální počítače s nízkou prioritou následujícím způsobem:

-   Můžete získat nebo nastavit hello hodnotu proměnné definované služby hello **$TargetLowPriorityNodes**.

-   Můžete získat hello hodnoty proměnné definované služby hello **$CurrentLowPriorityNodes**.

-   Můžete získat hello hodnoty proměnné definované služby hello **$PreemptedNodeCount**. 
    Tato proměnná vrátí hello počet uzlů v hello zrušené stavu a umožňuje škálování nahoru nebo dolů hello počet vyhrazených uzlů, v závislosti na počtu hello zrušené uzly, které jsou k dispozici.

## <a name="jobs-and-tasks"></a>Úlohy a úkoly

Úlohy a úkoly vyžadují velmi malé podporu pro uzly nízkou prioritu; podporuje se jen Hello vypadá takto:

-   Hello JobManagerTask vlastnost úlohy má novou vlastnost **AllowLowPriorityNode**. 
    Pokud tato vlastnost hodnotu true, hello úkolu Správce úloh bude naplánována na vyhrazené nebo nízkou prioritu uzlu. Pokud je tato vlastnost hodnotu false, hello úkolu Správce úloh bude naplánované tooa pouze vyhrazené uzlu.

-   [Proměnnou prostředí](batch-compute-node-environment-variables.md) je k dispozici tooa úloh aplikace, aby mohla určit, zda je spuštěn na nízkou prioritu, nebo vyhrazený uzlu. Proměnná prostředí Hello je AZ_BATCH_NODE_IS_DEDICATED.

## <a name="handling-preemption"></a>Zpracování přerušení

Virtuální počítače může být někdy zrušené; v takovém případě Batch hello následující:

-   Hello zrušené virtuální počítače mají jejich stav aktualizovat příliš**přepnuto**.
-   Pokud úlohy běžely hello zabránilo uzlu virtuální počítače, pak jsou tyto úlohy zařazena a znovu spusťte.
-   Odstranit je efektivně Hello virtuálních počítačů, úvodní tooany data uložená místně na hello ke ztrátě virtuálních počítačů.
-   fond Hello průběžně pokusí tooreach hello cílový počet uzlů nízkou prioritu k dispozici. Když se najde náhradní kapacity uzly zachovat jejich ID, ale jsou znovu inicializovat, projít **vytváření** a **počáteční** stavy předtím, než jsou k dispozici pro plánování úkolů.
-   Přerušování počty jsou k dispozici jako metriky v hello portálu Azure.

## <a name="metrics"></a>Metriky

Nové metriky, které jsou k dispozici v hello [portál Azure](https://portal.azure.com) pro uzly nízkou prioritu. Jsou tyto metriky:

- Počet uzlů s nízkou prioritou
- Počet jader s nízkou prioritou 
- Zrušené počet uzlů

Metrika tooview v hello portálu Azure:

1. Přejděte tooyour účtu Batch na portálu hello a zobrazit hello nastavení vašeho účtu Batch.
2. Vyberte **metriky** z hello **monitorování** části.
3. Vybrat metriky hello požadavky z hello **dostupné metriky** seznamu.

![Metriky pro uzly s nízkou prioritou](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a>Další kroky

* Čtení hello [přehled funkcí Batch pro vývojáře](batch-api-basics.md), základní informace pro každý, kdo Příprava toouse dávky. Hello článek obsahuje podrobnější informace o prostředky služby Batch, například fondy, uzlů, úlohy a úkoly a hello mnoho funkcí rozhraní API, které můžete použít při vytváření aplikace Batch.
* Další informace o hello [nástroje a rozhraní API služby Batch](batch-apis-tools.md) dostupné pro vytváření řešení Batch.
