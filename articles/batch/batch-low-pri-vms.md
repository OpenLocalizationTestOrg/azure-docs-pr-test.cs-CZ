---
title: "Spuštění úloh služby Azure Batch na virtuálních počítačích nákladově efektivní s nízkou prioritou (Preview) | Microsoft Docs"
description: "Zjistěte, jak zřídit virtuální počítače s nízkou prioritou snížit náklady na úloh služby Azure Batch."
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
ms.openlocfilehash: 9bf0ac322020d8a8453011c3207c1930175db6d3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-low-priority-vms-with-batch-preview"></a>Použití nízkou prioritu virtuálních počítačů pomocí služby Batch (Preview)

Azure Batch nabízí nízkou priorty virtuální počítače (VM) ke snížení nákladů úloh služby Batch. Virtuální počítače s nízkou prioritou umožňují nové typy úloh služby Batch, tím, že poskytuje velké množství výpočetního výkonu, která je také ekonomické.

Virtuální počítače s nízkou prioritou využít výhod kapacitní v Azure. Když zadáte nízkou prioritu virtuálních počítačů ve fondech, Azure Batch mohou automaticky používat tento přebytek, pokud je k dispozici.

Kompromis pro používání virtuálních počítačů s nízkou prioritou je, že tyto virtuální počítače může být zrušené při žádné kapacitní je k dispozici v Azure. Z toho důvodu jsou nejvhodnější pro některé typy úloh virtuálních počítačů s nízkou prioritou. Použijte virtuální počítače nízkou prioritu pro batch a asynchronní zpracování úloh, kde čas dokončení úlohy je flexibilní a práce je distribuován do mnoha virtuálních počítačů.

Virtuální počítače s nízkou prioritou jsou výrazně levnější než vyhrazených virtuálních počítačích. Podrobnosti o cenách najdete v části [ceny služby Batch](https://azure.microsoft.com/pricing/details/batch/).

Další informace o virtuálních počítačích nízkou prioritu, najdete v blogovém příspěvku: [computing za zlomek ceny služby Batch](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).

> [!IMPORTANT]
> Nízkou prioritu virtuální počítače jsou aktuálně ve verzi preview a jsou dostupné pouze pro úlohy běžící v dávce. 
>
>

## <a name="use-cases-for-low-priority-vms"></a>Případy použití pro virtuální počítače s nízkou prioritou

Vzhledem k vlastnostem virtuálních počítačů, nízkou prioritu, co úlohy můžete a nelze je použít? Obecně platí zpracování úloh služby batch jsou vhodné, úlohy jsou rozdělená do mnoho paralelních úloh nebo existuje mnoho úloh, které jsou škálovat na více systémů a distribuován do mnoha virtuálních počítačů.

-   Pokud chcete maximalizovat využití přebytečných kapacity v Azure, můžete škálovat vhodný úlohy.

-   Příležitostně virtuální počítače nemusí být k dispozici, nebo bude možné zrušené, který bude mít za následek menší kapacitu pro úlohy a může dojít k přerušení úloh a opakování. Proto musí být flexibilní v době, kterou můžete využít ke spuštění úlohy.

-   Úlohy se delší úloh může být ovlivněno více, pokud dojde k přerušení. Pokud dlouho běžící, že úlohy implementovat vytváření kontrolních bodů uložit průběh jako jejich provedení, pak dopad přerušení bude mnohem menší. Úlohy s kratší časy spouštění zpravidla nejlépe fungovat s virtuálními počítači nízkou prioritu jako dopad přerušení je mnohem menší.

-   Dlouhotrvajících úloh MPI, které využívají víc virtuálních počítačů nejsou dobře hodí používat virtuální počítače s nízkou prioritou jako jeden zrušené virtuální počítač pravděpodobně povede k celé úlohy museli znovu spusťte.

Některé příklady případy použití zpracování dávky dobře vhodné použít nízkou prioritu virtuální počítače jsou:

-   **Vývoj a testování**: zejména, pokud jsou vyvíjených rozsáhlé řešení výrazné úspory může být dosaženo. Všechny typy testování může přinést výhody, ale ve velkém měřítku zátěžové testování a testování regrese jsou skvělý používá.

-   **Dodávání kapacity na vyžádání**: virtuální počítače s nízkou prioritou lze použít k doplnění regular vyhrazených virtuálních počítačích – Pokud je k dispozici, můžete škálovat a proto dokončení rychlejší nižší náklady úlohy; Pokud není k dispozici, je použito účaří vyhrazených virtuálních počítačích jsou k dispozici.

-   **Čas spuštění úlohy flexibilní**: Pokud je flexibilitu při čas úlohy mají k dokončení, pak potenciální vyřazuje kapacity lze tolerovat, ale s přidáním virtuálních počítačů nízkou prioritu úlohy se často spustí rychleji a s nižšími náklady.

Fondy batch lze nakonfigurovat k využívání virtuálních počítačů nízkou prioritu několika různými způsoby v závislosti na flexibilitu v čase spuštění úlohy:

-   Virtuální počítače nízkou prioritu lze použít pouze ve fondu a Batch bude jednoduše obnovit všechny preempted kapacity, pokud je k dispozici. To je nejlevnější způsob k provedení úlohy se používají pouze nízkou prioritu virtuálních počítačů.

-   Virtuální počítače s nízkou prioritou lze použít ve spojení s pevnou směrného plánu vyhrazených virtuálních počítačů. Pevný počet vyhrazených virtuálních počítačích zajistí, že je vždy některé kapacity zachovat úlohy pokročíte.

-   Může být dynamické směs vyhrazené a nízkou prioritu virtuálních počítačů, tak, aby virtuální počítače levnějších nízkou prioritu se používají výhradně, pokud je k dispozici, ale jsou za cenu úplné vyhrazených virtuálních počítačích škálovat, aby minimální množství kapacity, které jsou k dispozici zachovat úlohy pokročíte vyžádání.

## <a name="batch-support-for-low-priority-vms"></a>Batch podporu pro virtuální počítače s nízkou prioritou

Azure Batch poskytuje několik možností, které usnadňují spotřebovávají a těžit z virtuálních počítačů nízkou prioritu:

-   Fondy batch může obsahovat vyhrazených virtuálních počítačích a virtuální počítače s nízkou prioritou. Počet jednotlivých typů virtuálních počítačů lze zadat, pokud fond je vytvořené nebo změněné kdykoli pro existující fond, pomocí operace změny velikosti explicitní nebo pomocí automatické škálování. Odeslání úlohy a úlohy může zůstat beze změny a nemusíte dělat starosti s typy virtuálních počítačů ve fondu. Je také možné, že fond úplně použít ke spuštění úloh levné jako možný, ale otočení až vyhrazených virtuálních počítačích, pokud je kapacita klesne pod minimální prahové hodnoty, aby úlohy spuštěné virtuální počítače s nízkou prioritou.

-   Fondy batch automaticky snaží cílový počet virtuálních počítačů nízkou prioritu. Pokud přerušené virtuální počítače, se pokusí Batch nahradit ztraceny kapacitu a vrátíte se k cíli.

-   V případě úloh se přerušení Batch rozpozná a automaticky znovu úlohy spustit znovu.

-   Virtuální počítače s nízkou prioritou mají kvóta na jádra, která se liší od vyhrazených virtuálních počítačích. 
    Uvozovky pro virtuální počítače s nízkou prioritou je vyšší než u vyhrazených virtuálních počítačích, protože virtuální počítače s nízkou prioritou nižší náklady. V tématu [Batch, kvóty a omezení služby](batch-quota-limit.md#resource-quotas) Další informace.    

> [!NOTE]
> Virtuální počítače s nízkou prioritou nejsou aktuálně podporovány pro účty Batch, kde je nastaven režim přidělení fondu na [uživatele předplatné](batch-account-create-portal.md#user-subscription-mode).
>
>

## <a name="create-and-update-pools"></a>Vytváření a aktualizaci fondy

Fondu služby Batch může obsahovat vyhrazené a nízkou prioritu virtuálních počítačů (také označované jako výpočetní uzly). Pro vyhrazené i nízkou prioritu virtuální počítače můžete nastavit cílovým počtem výpočetních uzlů. Cílový počet uzlů určuje počet virtuálních počítačů, které chcete mít ve fondu.

Například vytvoření fondu pomocí služby Azure cloud virtuálních počítačů s cílem 5 vyhrazené virtuální počítače a 20 virtuálních počítačů nízkou prioritu:

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4") // WS 2012 R2
);
```

Vytvoření fondu pomocí virtuální počítače Azure (v tomto případě virtuální počítače s Linuxem) s cílem 5 vyhrazené virtuální počítače a 20 virtuálních počítačů nízkou prioritu:

```csharp
ImageReference imageRef = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "16.04.0-LTS",
    version: "latest");

// Create the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration("batch.node.ubuntu 16.04", imageRef);

pool = batchClient.PoolOperations.CreatePool(
    poolId: "vmpool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard\_D2\_v2",
    virtualMachineConfiguration: virtualMachineConfiguration);
```

Aktuální počet uzlů můžete získat pro vyhrazené i nízkou prioritu virtuální počítače:

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

Uzly fondu mají vlastnost označující, zda je uzel vyhrazené nebo nízkou prioritu virtuálního počítače:

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

Přerušené jeden nebo více uzlů ve fondu, operace výpisu uzlů ve fondu pořád vrátí tyto uzly, aktuální počet uzlů nízkou prioritu zůstanou nezměněna, ale tyto uzly budou mít jejich stavu, nastavte na **přepnuto** stav. Batch se pokusí najít nahrazení virtuální počítače, a pokud bylo úspěšné, bude projít uzly **vytváření** a potom **počáteční** stavy poté jsou dostupné pro provádění úkolů, stejně jako nové uzly.

## <a name="scale-a-pool-containing-low-priority-vms"></a>Škálování fond, který obsahuje virtuální počítače s nízkou prioritou

Stejně jako u fondy výhradně skládající se z vyhrazených virtuálních počítačích, je možné škálovat obsahující virtuální počítače, nízkou prioritu, voláním metody změny velikosti nebo pomocí automatickému škálování fondu.

Operace změny velikosti fondu trvá druhý volitelný parametr, který aktualizuje hodnotu **targetLowPriorityNodes**:

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

Vzorec automatickému škálování fondu podporuje virtuální počítače s nízkou prioritou následujícím způsobem:

-   Můžete získat nebo nastavit hodnotu proměnné definované služby **$TargetLowPriorityNodes**.

-   Můžete získat hodnoty proměnné definované služby **$CurrentLowPriorityNodes**.

-   Můžete získat hodnoty proměnné definované služby **$PreemptedNodeCount**. 
    Tato proměnná vrátí počet uzlů v preempted stavu a umožňuje škálování nahoru nebo dolů počet vyhrazených uzlů, v závislosti na počtu zrušené uzlů, které jsou k dispozici.

## <a name="jobs-and-tasks"></a>Úlohy a úkoly

Úlohy a úkoly vyžadují velmi malé podporu pro uzly nízkou prioritu; podporuje se jen vypadá takto:

-   Vlastnost JobManagerTask úlohy má novou vlastnost **AllowLowPriorityNode**. 
    Pokud tato vlastnost hodnotu true, úkolu Správce úloh bude naplánována na vyhrazené nebo nízkou prioritu uzlu. Pokud je tato vlastnost hodnotu false, úkolu Správce úloh bude naplánována na vyhrazené uzel.

-   [Proměnnou prostředí](batch-compute-node-environment-variables.md) je k dispozici pro aplikace úkolů, aby mohla určit, zda je spuštěn na nízkou prioritu, nebo vyhrazený uzlu. Proměnná prostředí je AZ_BATCH_NODE_IS_DEDICATED.

## <a name="handling-preemption"></a>Zpracování přerušení

Virtuální počítače může být někdy zrušené; v takovém případě Batch provede následující akce:

-   Preempted virtuální počítače mají jejich stav, aktualizovat, aby **přepnuto**.
-   Pokud úlohy byly spuštěné na virtuálních počítačích zrušené uzlu, jsou tyto úlohy zařazena a spusťte znovu.
-   Virtuální počítač je efektivně odstranit, což všechna data uložená místně na virtuální počítač ke ztrátě.
-   Fond se průběžně pokusí kontaktovat cílový počet nízkou prioritu uzlů, které jsou k dispozici. Když se najde náhradní kapacity uzly zachovat jejich ID, ale jsou znovu inicializovat, projít **vytváření** a **počáteční** stavy předtím, než jsou k dispozici pro plánování úkolů.
-   Přerušování počty jsou k dispozici jako metrika na portálu Azure.

## <a name="metrics"></a>Metriky

Jsou k dispozici v nové metriky [portál Azure](https://portal.azure.com) pro uzly nízkou prioritu. Jsou tyto metriky:

- Počet uzlů s nízkou prioritou
- Počet jader s nízkou prioritou 
- Zrušené počet uzlů

Chcete-li zobrazit metriky na portálu Azure:

1. Přejděte na svůj účet Batch na portálu a zobrazit nastavení vašeho účtu Batch.
2. Vyberte **metriky** z **monitorování** části.
3. Vybrat metriky, které mají **dostupné metriky** seznamu.

![Metriky pro uzly s nízkou prioritou](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a>Další kroky

* Přečtěte si téma [Přehled funkcí Batch pro vývojáře](batch-api-basics.md), kde jsou základní informace pro každého, kdo se připravuje použít Batch. Článek obsahuje podrobné informace o prostředcích služby Batch, jako jsou fondy, uzly a úlohy, a mnoha funkcích rozhraní API, které můžete použít při vytváření aplikace Batch.
* Další informace o dostupných [rozhraních API a nástrojích služby Batch](batch-apis-tools.md) pro sestavování řešení Batch.
