---
title: "Plánování kapacity pro aplikace Service Fabric | Microsoft Docs"
description: "Popisuje, jak identifikovat počet výpočetních uzlů, které jsou potřebné pro aplikace Service Fabric"
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: markfuss
editor: 
ms.assetid: 9fa47be0-50a2-4a51-84a5-20992af94bea
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: fc98bdd8b3597810b0c07563af507e93c611f769
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="capacity-planning-for-service-fabric-applications"></a><span data-ttu-id="11dfe-103">Plánování kapacity pro aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="11dfe-103">Capacity planning for Service Fabric applications</span></span>
<span data-ttu-id="11dfe-104">Tento dokument se naučíte, jak odhadovat objem prostředků (procesory, paměť RAM, disk v úložišti), budete muset spustit aplikace Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="11dfe-104">This document teaches you how to estimate the amount of resources (CPUs, RAM, disk storage) you need to run your Azure Service Fabric applications.</span></span> <span data-ttu-id="11dfe-105">Je běžné pro požadavky na prostředky pro časem změnit.</span><span class="sxs-lookup"><span data-stu-id="11dfe-105">It is common for your resource requirements to change over time.</span></span> <span data-ttu-id="11dfe-106">Několik prostředků se obvykle vyžadují, jako je vývoj/testování vaší služby a pak vyžadují více prostředků, jak můžete přejít do produkčního prostředí a aplikace zvětšování popularita.</span><span class="sxs-lookup"><span data-stu-id="11dfe-106">You typically require few resources as you develop/test your service, and then require more resources as you go into production and your application grows in popularity.</span></span> <span data-ttu-id="11dfe-107">Při návrhu vaší aplikace, pečlivě promyslete požadavky na dlouhodobé a rozhodování, umožňujících služby škálování potřeby vysoké zákazníka.</span><span class="sxs-lookup"><span data-stu-id="11dfe-107">When you design your application, think through the long-term requirements and make choices that allow your service to scale to meet high customer demand.</span></span>

 <span data-ttu-id="11dfe-108">Když vytvoříte cluster Service Fabric, je rozhodnout, jaké druhy virtuální počítače (VM) se skládá clusteru.</span><span class="sxs-lookup"><span data-stu-id="11dfe-108">When you create a Service Fabric cluster, you decide what kinds of virtual machines (VMs) make up the cluster.</span></span> <span data-ttu-id="11dfe-109">Každý virtuální počítač se dodává s omezené množství prostředků ve formě procesorů (jader a rychlost), šířku pásma sítě, paměť RAM a diskových úložišť.</span><span class="sxs-lookup"><span data-stu-id="11dfe-109">Each VM comes with a limited amount of resources in the form of CPUs (cores and speed), network bandwidth, RAM, and disk storage.</span></span> <span data-ttu-id="11dfe-110">S růstem vaší služby v čase, můžete upgradovat na virtuální počítače, které nabízejí větší prostředky nebo přidejte další virtuální počítače do clusteru.</span><span class="sxs-lookup"><span data-stu-id="11dfe-110">As your service grows over time, you can upgrade to VMs that offer greater resources and/or add more VMs to your cluster.</span></span> <span data-ttu-id="11dfe-111">Pokud chcete provést k tomu, musí vaše služba architektury původně, ho můžete využít výhod nové virtuální počítače, které se dynamicky přidají do clusteru.</span><span class="sxs-lookup"><span data-stu-id="11dfe-111">To do the latter, you must architect your service initially so it can take advantage of new VMs that get dynamically added to the cluster.</span></span>

<span data-ttu-id="11dfe-112">Některé služby spravovat málo na žádná data na vlastních virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="11dfe-112">Some services manage little to no data on the VMs themselves.</span></span> <span data-ttu-id="11dfe-113">Plánování kapacity pro tyto služby proto měli zaměřit především na výkon, což znamená, výběrem příslušné procesorů (jader a rychlost) virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="11dfe-113">Therefore, capacity planning for these services should focus primarily on performance, which means selecting the appropriate CPUs (cores and speed) of the VMs.</span></span> <span data-ttu-id="11dfe-114">Kromě toho byste měli zvážit šířky pásma sítě, včetně jak často dochází k síťové přenosy a kolik dat je přenosu.</span><span class="sxs-lookup"><span data-stu-id="11dfe-114">In addition, you should consider network bandwidth, including how frequently network transfers are occurring and how much data is being transferred.</span></span> <span data-ttu-id="11dfe-115">Pokud vaše služba potřebuje provést a taky zvýší využití služby, můžete přidat další virtuální počítače v clusteru a zatížení vyrovnávání síťové požadavky napříč všech virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="11dfe-115">If your service needs to perform well as service usage increases, you can add more VMs to the cluster and load balance the network requests across all the VMs.</span></span>

<span data-ttu-id="11dfe-116">Pro služby, které spravují velké objemy dat na virtuálních počítačích měli primárně na velikosti zaměřit plánování kapacity.</span><span class="sxs-lookup"><span data-stu-id="11dfe-116">For services that manage large amounts of data on the VMs, capacity planning should focus primarily on size.</span></span> <span data-ttu-id="11dfe-117">Proto měli pečlivě zvážit kapacitu paměti RAM Virtuálního počítače a diskových úložišť.</span><span class="sxs-lookup"><span data-stu-id="11dfe-117">Thus, you should carefully consider the capacity of the VM's RAM and disk storage.</span></span> <span data-ttu-id="11dfe-118">Systém správy virtuální paměti v systému Windows umožňuje místa na disku vypadat RAM do kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="11dfe-118">The virtual memory management system in Windows makes disk space look like RAM to application code.</span></span> <span data-ttu-id="11dfe-119">Kromě toho poskytuje modulu runtime Service Fabric inteligentní stránkování zachovat data pouze aktivní v paměti a přesunutí pomaleji přístupná data na disk.</span><span class="sxs-lookup"><span data-stu-id="11dfe-119">In addition, the Service Fabric runtime provides smart paging keeping only hot data in memory and moving the cold data to disk.</span></span> <span data-ttu-id="11dfe-120">Aplikace můžou proto používat více paměti, než je fyzicky k dispozici ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="11dfe-120">Applications can thus use more memory than is physically available on the VM.</span></span> <span data-ttu-id="11dfe-121">S víc paměti RAM zvyšuje výkon, jednoduše, vzhledem k tomu, že virtuální počítač můžete ponechat další diskového úložiště v paměti RAM.</span><span class="sxs-lookup"><span data-stu-id="11dfe-121">Having more RAM simply increases performance, since the VM can keep more disk storage in RAM.</span></span> <span data-ttu-id="11dfe-122">Virtuální počítač, který vyberete by měl mít disk dostatečně velký pro ukládání dat, která chcete na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="11dfe-122">The VM you select should have a disk large enough to store the data that you want on the VM.</span></span> <span data-ttu-id="11dfe-123">Podobně virtuální počítač by měl mít dostatek paměti RAM, kde přinášejí výkonu, které očekáváte.</span><span class="sxs-lookup"><span data-stu-id="11dfe-123">Similarly, the VM should have enough RAM to provide you with the performance you desire.</span></span> <span data-ttu-id="11dfe-124">Pokud časem naroste data vaší služby, můžete přidat další virtuální počítače do clusteru a oddíl data mezi všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="11dfe-124">If your service's data grows over time, you can add more VMs to the cluster and partition the data across all the VMs.</span></span>

## <a name="determine-how-many-nodes-you-need"></a><span data-ttu-id="11dfe-125">Určit, kolik uzly, budete potřebovat</span><span class="sxs-lookup"><span data-stu-id="11dfe-125">Determine how many nodes you need</span></span>
<span data-ttu-id="11dfe-126">Vytváření oddílů služby umožňuje škálovat data vaší služby.</span><span class="sxs-lookup"><span data-stu-id="11dfe-126">Partitioning your service allows you to scale out your service's data.</span></span> <span data-ttu-id="11dfe-127">Další informace o vytváření oddílů, najdete v části [dělení Service Fabric](service-fabric-concepts-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="11dfe-127">For more information on partitioning, see [Partitioning Service Fabric](service-fabric-concepts-partitioning.md).</span></span> <span data-ttu-id="11dfe-128">Každý oddíl se musí vejít do jednoho virtuálního počítače, ale více oddílů (malé) mohou být umístěny na jeden virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="11dfe-128">Each partition must fit within a single VM, but multiple (small) partitions can be placed on a single VM.</span></span> <span data-ttu-id="11dfe-129">Ano s více oddíly malé vám dává větší flexibilitu, než má několik větší oddíly.</span><span class="sxs-lookup"><span data-stu-id="11dfe-129">So, having more small partitions gives you greater flexibility than having a few larger partitions.</span></span> <span data-ttu-id="11dfe-130">O kompromisu je, že spousta oddíly zvyšuje režii Service Fabric a napříč oddíly nelze provést zpracovaných operací.</span><span class="sxs-lookup"><span data-stu-id="11dfe-130">The trade-off is that having lots of partitions increases Service Fabric overhead and you cannot perform transacted operations across partitions.</span></span> <span data-ttu-id="11dfe-131">Je zde také další potenciální síťový provoz Pokud kódu služby často potřebuje přístup k kusy data, která za provozu v různých oddílů.</span><span class="sxs-lookup"><span data-stu-id="11dfe-131">There is also more potential network traffic if your service code frequently needs to access pieces of data that live in different partitions.</span></span> <span data-ttu-id="11dfe-132">Při navrhování vaší služby, pečlivě zvažte tyto výhody a nevýhody přijaty ve efektivní strategie dělení.</span><span class="sxs-lookup"><span data-stu-id="11dfe-132">When designing your service, you should carefully consider these pros and cons to arrive at an effective partitioning strategy.</span></span>

<span data-ttu-id="11dfe-133">Předpokládejme, že aplikace má jeden stavové služby, která má velikost úložiště, který chcete dosáhnout DB_Size GB v roce.</span><span class="sxs-lookup"><span data-stu-id="11dfe-133">Let's assume your application has a single stateful service that has a store size that you expect to grow to DB_Size GB in a year.</span></span> <span data-ttu-id="11dfe-134">Chcete-li přidat další aplikace (a oddíly) jako prostředí růstu nad rámec tohoto roku.</span><span class="sxs-lookup"><span data-stu-id="11dfe-134">You are willing to add more applications (and partitions) as you experience growth beyond that year.</span></span>  <span data-ttu-id="11dfe-135">Replikace faktor (RF), která určuje počet replik služby má dopad na celkový DB_Size.</span><span class="sxs-lookup"><span data-stu-id="11dfe-135">The replication factor (RF), which determines the number of replicas for your service impacts the total DB_Size.</span></span> <span data-ttu-id="11dfe-136">Celkový počet DB_Size napříč všechny repliky je násobí hodnotou DB_Size faktor replikace.</span><span class="sxs-lookup"><span data-stu-id="11dfe-136">The total DB_Size across all replicas is the Replication Factor multiplied by DB_Size.</span></span>  <span data-ttu-id="11dfe-137">Node_Size představuje místa nebo RAM disku na jeden uzel, který chcete použít pro vaši službu.</span><span class="sxs-lookup"><span data-stu-id="11dfe-137">Node_Size represents the disk space/RAM per node you want to use for your service.</span></span> <span data-ttu-id="11dfe-138">Pro nejlepší výkon DB_Size by měl začlenit do paměti v clusteru a je třeba zvolit Node_Size, který je kolem paměť RAM virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="11dfe-138">For best performance, the DB_Size should fit into memory across the cluster, and a Node_Size that is around the RAM of the VM should be chosen.</span></span> <span data-ttu-id="11dfe-139">Přidělí Node_Size, která je větší než kapacita paměti RAM, se spoléhat na stránkování poskytované modulu runtime Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="11dfe-139">By allocating a Node_Size that is larger than the RAM capacity, you are relying on the paging provided by the Service Fabric runtime.</span></span> <span data-ttu-id="11dfe-140">Proto nemusí být optimální, pokud se považuje za aktivního celého datového výkon (od té doby dat je stránkovaného vstup/výstup).</span><span class="sxs-lookup"><span data-stu-id="11dfe-140">Thus, your performance may not be optimal if your entire data is considered to be hot (since then the data is paged in/out).</span></span> <span data-ttu-id="11dfe-141">Pro mnoho služeb, kde je aktivní pouze část dat, je však cenově výhodnější.</span><span class="sxs-lookup"><span data-stu-id="11dfe-141">However, for many services where only a fraction of the data is hot, it is more cost-effective.</span></span>

<span data-ttu-id="11dfe-142">Počet uzlů, které jsou potřebné pro maximální výkon můžete vypočítat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="11dfe-142">The number of nodes required for maximum performance can be computed as follows:</span></span>

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a><span data-ttu-id="11dfe-143">Účet pro růst</span><span class="sxs-lookup"><span data-stu-id="11dfe-143">Account for growth</span></span>
<span data-ttu-id="11dfe-144">Můžete vypočítat počet uzlů podle DB_Size, které očekáváte, že služby růst, kromě DB_Size, které začne s.</span><span class="sxs-lookup"><span data-stu-id="11dfe-144">You may want to compute the number of nodes based on the DB_Size that you expect your service to grow to, in addition to the DB_Size that you began with.</span></span> <span data-ttu-id="11dfe-145">Potom růst počet uzlů s růstem vaší služby, aby nejsou předimenzování počet uzlů.</span><span class="sxs-lookup"><span data-stu-id="11dfe-145">Then, grow the number of nodes as your service grows so that you are not over-provisioning the number of nodes.</span></span> <span data-ttu-id="11dfe-146">Ale počet oddílů by měl být podle počtu uzlů, které jsou potřebné, pokud používáte služby v maximální růstu.</span><span class="sxs-lookup"><span data-stu-id="11dfe-146">But the number of partitions should be based on the number of nodes that are needed when you're running your service at maximum growth.</span></span>

<span data-ttu-id="11dfe-147">Je vhodné mít některé další počítače, které jsou k dispozici kdykoli tak, aby bylo možné zpracovat všechny neočekávané špičky nebo selhání, (například pokud několika virtuálními počítači přejděte).</span><span class="sxs-lookup"><span data-stu-id="11dfe-147">It is good to have some extra machines available at any time so that you can handle any unexpected spikes or failure (for example, if a few VMs go down).</span></span>  <span data-ttu-id="11dfe-148">Při kapacitu navíc by měl být určen pomocí vaší očekávané špičky, výchozí bod je můžete vyhradit několik navíc za virtuální počítače (5-10 procent navíc).</span><span class="sxs-lookup"><span data-stu-id="11dfe-148">While the extra capacity should be determined by using your expected spikes, a starting point is to reserve a few extra VMs (5-10 percent extra).</span></span>

<span data-ttu-id="11dfe-149">Podle předchozích předpokládá jednu stavové služby.</span><span class="sxs-lookup"><span data-stu-id="11dfe-149">The preceding assumes a single stateful service.</span></span> <span data-ttu-id="11dfe-150">Pokud máte více než jeden stavové služby, budete muset přidat DB_Size spojené s jinými službami do rovnice.</span><span class="sxs-lookup"><span data-stu-id="11dfe-150">If you have more than one stateful service, you have to add the DB_Size associated with the other services into the equation.</span></span> <span data-ttu-id="11dfe-151">Alternativně můžete vypočítat počet uzlů samostatně pro každou stavové služby.</span><span class="sxs-lookup"><span data-stu-id="11dfe-151">Alternatively, you can compute the number of nodes separately for each stateful service.</span></span>  <span data-ttu-id="11dfe-152">Vaše služba může mít repliky nebo oddíly, které nejsou spárovány.</span><span class="sxs-lookup"><span data-stu-id="11dfe-152">Your service may have replicas or partitions that aren't balanced.</span></span> <span data-ttu-id="11dfe-153">Uvědomte si, že oddíly mohou mít i více dat než jiné.</span><span class="sxs-lookup"><span data-stu-id="11dfe-153">Keep in mind that partitions may also have more data than others.</span></span> <span data-ttu-id="11dfe-154">Další informace o vytváření oddílů, najdete v části [dělení článek o osvědčených postupech](service-fabric-concepts-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="11dfe-154">For more information on partitioning, see [partitioning article on best practices](service-fabric-concepts-partitioning.md).</span></span> <span data-ttu-id="11dfe-155">Ale předchozí vztah je oddíl a repliky lhostejné, protože Service Fabric zajistí, že repliky jsou rozprostřeny mezi uzly optimalizovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="11dfe-155">However, the preceding equation is partition and replica agnostic, because Service Fabric ensures that the replicas are spread out among the nodes in an optimized manner.</span></span>

## <a name="use-a-spreadsheet-for-cost-calculation"></a><span data-ttu-id="11dfe-156">Používání tabulky pro výpočet nákladů</span><span class="sxs-lookup"><span data-stu-id="11dfe-156">Use a spreadsheet for cost calculation</span></span>
<span data-ttu-id="11dfe-157">Nyní ve vzorci přidejme některé reálná čísla.</span><span class="sxs-lookup"><span data-stu-id="11dfe-157">Now let's put some real numbers in the formula.</span></span> <span data-ttu-id="11dfe-158">[Příklad tabulky](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) ukazuje, jak plánování kapacity pro aplikaci, která obsahuje tři typy datových objektů.</span><span class="sxs-lookup"><span data-stu-id="11dfe-158">An [example spreadsheet](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) shows how to plan the capacity for an application that contains three types of data objects.</span></span> <span data-ttu-id="11dfe-159">Pro každý objekt jsme Přibližná jeho velikost a kolik objekty že Očekáváme, že má.</span><span class="sxs-lookup"><span data-stu-id="11dfe-159">For each object, we approximate its size and how many objects we expect to have.</span></span> <span data-ttu-id="11dfe-160">Můžeme také vybrat kolik repliky chceme každého typu objektu.</span><span class="sxs-lookup"><span data-stu-id="11dfe-160">We also select how many replicas we want of each object type.</span></span> <span data-ttu-id="11dfe-161">Tabulku vypočítá celkové množství paměti k uložení do clusteru.</span><span class="sxs-lookup"><span data-stu-id="11dfe-161">The spreadsheet calculates the total amount of memory to be stored in the cluster.</span></span>

<span data-ttu-id="11dfe-162">Potom jsme zadejte velikost virtuálního počítače a měsíční náklady.</span><span class="sxs-lookup"><span data-stu-id="11dfe-162">Then we enter a VM size and monthly cost.</span></span> <span data-ttu-id="11dfe-163">Podle velikosti virtuálního počítače, tabulce zjistíte minimální počet oddílů, které je nutné použít k rozdělení dat, aby se fyzicky vejít na uzly.</span><span class="sxs-lookup"><span data-stu-id="11dfe-163">Based on the VM size, the spreadsheet tells you the minimum number of partitions you must use to split your data to physically fit on the nodes.</span></span> <span data-ttu-id="11dfe-164">Očekáváte větší počet oddílů pro přizpůsobení výpočetní konkrétní aplikace a je třeba síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="11dfe-164">You may desire a larger number of partitions to accommodate your application's specific computation and network traffic needs.</span></span> <span data-ttu-id="11dfe-165">Tabulku zobrazuje počet oddílů, které budete spravovat objekty uživatelského profilu zvýšilo 1 až šest.</span><span class="sxs-lookup"><span data-stu-id="11dfe-165">The spreadsheet shows the number of partitions that are managing the user profile objects has increased from one to six.</span></span>

<span data-ttu-id="11dfe-166">Nyní založená na všechny tyto informace, tabulku zobrazuje, může získat všechna data s požadovanou oddíly a repliky fyzicky na 26 uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="11dfe-166">Now, based on all this information, the spreadsheet shows that you could physically get all the data with the desired partitions and replicas on a 26-node cluster.</span></span> <span data-ttu-id="11dfe-167">Však tento cluster by hustě balí, takže můžete chtít, aby dokázala pojmout selhání uzlů a upgrady některé další uzly.</span><span class="sxs-lookup"><span data-stu-id="11dfe-167">However, this cluster would be densely packed, so you may want some additional nodes to accommodate node failures and upgrades.</span></span> <span data-ttu-id="11dfe-168">Tabulku také ukazuje, že s více než 57 uzly poskytuje žádná další hodnota, protože bude mít prázdný uzly.</span><span class="sxs-lookup"><span data-stu-id="11dfe-168">The spreadsheet also shows that having more than 57 nodes provides no additional value because you would have empty nodes.</span></span> <span data-ttu-id="11dfe-169">Chcete znovu přejděte výše 57 uzly přesto zohlednit selhání uzlů a upgrady.</span><span class="sxs-lookup"><span data-stu-id="11dfe-169">Again, you may want to go above 57 nodes anyway to accommodate node failures and upgrades.</span></span> <span data-ttu-id="11dfe-170">Tabulku tak, aby odpovídala konkrétním potřebám vaší aplikace, můžete upravit.</span><span class="sxs-lookup"><span data-stu-id="11dfe-170">You can tweak the spreadsheet to match your application's specific needs.</span></span>   

![Tabulky pro výpočet nákladů][Image1]

## <a name="next-steps"></a><span data-ttu-id="11dfe-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="11dfe-172">Next steps</span></span>
<span data-ttu-id="11dfe-173">Podívejte se na [dělení Service Fabric služby] [ 10] Další informace o vytváření oddílů služby.</span><span class="sxs-lookup"><span data-stu-id="11dfe-173">Check out [Partitioning Service Fabric services][10] to learn more about partitioning your service.</span></span>

<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-concepts-partitioning.md