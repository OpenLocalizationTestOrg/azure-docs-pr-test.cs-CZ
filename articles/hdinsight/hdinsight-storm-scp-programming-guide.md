---
title: "Průvodce programováním SCP.NET | Microsoft Docs"
description: "Další informace o použití SCP.NET k vytvoření. Na základě NET topologie Storm pro použití se Storm v HDInsight."
services: hdinsight
documentationcenter: 
author: raviperi
manager: jhubbard
editor: cgronlun
ms.assetid: 34192ed0-b1d1-4cf7-a3d4-5466301cf307
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/16/2016
ms.author: raviperi
ms.openlocfilehash: 3d76aebd2a1fd729c8e0639e6afcbde4c3fb752b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scp-programming-guide"></a><span data-ttu-id="14626-103">Průvodce programováním spojovací bod služby</span><span class="sxs-lookup"><span data-stu-id="14626-103">SCP programming guide</span></span>
<span data-ttu-id="14626-104">Spojovací bod služby je platforma pro sestavení v reálném čase, spolehlivé, konzistentní a vysoký výkon zpracování dat aplikace.</span><span class="sxs-lookup"><span data-stu-id="14626-104">SCP is a platform to build real time, reliable, consistent and high performance data processing application.</span></span> <span data-ttu-id="14626-105">Je založen na [Apache Storm](http://storm.incubator.apache.org/) – systém navrhovány komunit OSS zpracování datového proudu.</span><span class="sxs-lookup"><span data-stu-id="14626-105">It is built on top of [Apache Storm](http://storm.incubator.apache.org/) -- a stream processing system designed by the OSS communities.</span></span> <span data-ttu-id="14626-106">Storm je určen podle Nathan Marz a otevřete která byla vytvořena pomocí služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="14626-106">Storm is designed by Nathan Marz and open sourced by Twitter.</span></span> <span data-ttu-id="14626-107">Využívá [Apache ZooKeeper](http://zookeeper.apache.org/), jiného projektu Apache umožňující vysoce spolehlivé distribuované správy koordinace a stavu.</span><span class="sxs-lookup"><span data-stu-id="14626-107">It leverages [Apache ZooKeeper](http://zookeeper.apache.org/), another Apache project to enable highly reliable distributed coordination and state management.</span></span> 

<span data-ttu-id="14626-108">Pouze spojovací bod služby projektu Storm v systému Windows, která je součástí, ale také projektu přidat rozšíření a přizpůsobení pro prostředí Windows.</span><span class="sxs-lookup"><span data-stu-id="14626-108">Not only the SCP project ported Storm on Windows but also the project added extensions and customization for the Windows ecosystem.</span></span> <span data-ttu-id="14626-109">Rozšíření jsou vývojáře prostředí .NET a knihovny, přizpůsobení zahrnuje nasazení systému Windows.</span><span class="sxs-lookup"><span data-stu-id="14626-109">The extensions include .NET developer experience, and libraries, the customization includes Windows-based deployment.</span></span> 

<span data-ttu-id="14626-110">Rozšíření a přizpůsobení se provádí tak, že je není potřeba rozvětvit projekty operačních systémů a jsme může využít odvozené ekosystémů nástavbou Storm.</span><span class="sxs-lookup"><span data-stu-id="14626-110">The extension and customization is done in such a way that we do not need to fork the OSS projects and we could leverage derived ecosystems built on top of Storm.</span></span>

## <a name="processing-model"></a><span data-ttu-id="14626-111">Zpracování modelu</span><span class="sxs-lookup"><span data-stu-id="14626-111">Processing model</span></span>
<span data-ttu-id="14626-112">Data v spojovací bod služby je modelovaná jako nepřetržité proudy řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="14626-112">The data in SCP is modeled as continuous streams of tuples.</span></span> <span data-ttu-id="14626-113">Obvykle řazené kolekce členů toku do některé fronty nejprve pak zachyceny a transformovat obchodní logiky hostovanou v rámci topologie Storm, nakonec výstup může předat jako řazených kolekcí členů do jiného systému spojovací bod služby, nebo být zapsána do úložiště jako systém souborů DFS nebo databází jako SQL Server.</span><span class="sxs-lookup"><span data-stu-id="14626-113">Typically the tuples flow into some queue first, then picked up, and transformed by business logic hosted inside a Storm topology, finally the output could be piped as tuples to another SCP system, or be committed to stores like distributed file system or databases like SQL Server.</span></span>

![Diagram fronty napájení data pro zpracování, které informační kanály úložiště dat](media/hdinsight-storm-scp-programming-guide/queue-feeding-data-to-processing-to-data-store.png)

<span data-ttu-id="14626-115">Topologie aplikací v Storm, definuje výpočetní graf.</span><span class="sxs-lookup"><span data-stu-id="14626-115">In Storm, an application topology defines a graph of computation.</span></span> <span data-ttu-id="14626-116">Každý uzel v topologii obsahuje logiku zpracování a odkazů mezi uzly indikují datový tok.</span><span class="sxs-lookup"><span data-stu-id="14626-116">Each node in a topology contains processing logic, and links between nodes indicate data flow.</span></span> <span data-ttu-id="14626-117">Uzly vložení vstupní data do topologie se nazývají funkcích Spouts, které se dají použít k sekvencování data.</span><span class="sxs-lookup"><span data-stu-id="14626-117">The nodes to inject input data into the topology are called Spouts, which can be used to sequence the data.</span></span> <span data-ttu-id="14626-118">Vstupních dat může být umístěn v protokoly transakcí databáze, čítače výkonu systému atd. Funkce Bolts, které provádět filtrování skutečná data a výběry a agregace, se označují jako uzly s obou toky vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="14626-118">The input data could reside in file logs, transactional database, system performance counter etc. The nodes with both input and output data flows are called Bolts, which do the actual data filtering and selections and aggregation.</span></span>

<span data-ttu-id="14626-119">Spojovací bod služby podporuje snažit v aspoň jednou a přesně-po zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="14626-119">SCP supports best efforts, at-least-once and exactly-once data processing.</span></span> <span data-ttu-id="14626-120">V distribuované aplikaci streamování zpracování různých chybám může dojít během zpracování dat, jako je výpadek sítě, selhání počítače nebo Chyba uživatelského kódu atd. Zpracování v aspoň jednou zajistí, že všechna data zpracuje alespoň jednou stejná data přehrání automaticky, když se stane chyba.</span><span class="sxs-lookup"><span data-stu-id="14626-120">In a distributed streaming processing application, various errors may happen during data processing, such as network outage, machine failure, or user code error etc. At-least-once processing ensures all data will be processed at least once by replaying automatically the same data when error happens.</span></span> <span data-ttu-id="14626-121">Na alespoň jedno zpracování je jednoduché a spolehlivé a vyhovuje dobře v mnoha aplikacích.</span><span class="sxs-lookup"><span data-stu-id="14626-121">At-least-once processing is simple and reliable and suits well in many applications.</span></span> <span data-ttu-id="14626-122">Pokud aplikace vyžaduje přesné počítání, například na alespoň jedno zpracování je však dostatek vzhledem k tomu, že stejná data může být přehráván potenciálně v topologii aplikace.</span><span class="sxs-lookup"><span data-stu-id="14626-122">However, when the application requires exact counting, for example, at-least-once processing is insufficient since the same data could potentially be played in the application topology.</span></span> <span data-ttu-id="14626-123">V takovém případě, přesně-po zpracování je navržená tak, abyste měli jistotu, výsledek je správný, i když data mohou přehrány a zpracovávat vícekrát.</span><span class="sxs-lookup"><span data-stu-id="14626-123">In that case, exactly-once processing is designed to make sure the result is correct even when the data may be replayed and processed multiple times.</span></span>

<span data-ttu-id="14626-124">Spojovací bod služby umožňuje vývojářům rozhraní .NET k vývoji aplikací proces data reálném čase, při využívání Java Virtual Machine (JVM) v části vztahují na základě Storm.</span><span class="sxs-lookup"><span data-stu-id="14626-124">SCP enables .NET developers to develop real time data process applications while leverage the Java Virtual Machine (JVM) based Storm under the cover.</span></span> <span data-ttu-id="14626-125">Rozhraní .NET a prostředí Java Virtual Machine komunikovat přes TCP místní soketu.</span><span class="sxs-lookup"><span data-stu-id="14626-125">The .NET and JVM communicate via TCP local socket.</span></span> <span data-ttu-id="14626-126">V podstatě každou funkcí Spout/Bolt je proces pár .net nebo Java, kde běží logiku uživatelského v rozhraní .net procesu jako o modul plug-in.</span><span class="sxs-lookup"><span data-stu-id="14626-126">Basically each Spout/Bolt is a .Net/Java process pair, where the user logic runs in .Net process as a plugin.</span></span>

<span data-ttu-id="14626-127">Sestavit aplikaci pro zpracování dat nad spojovací bod služby, jsou potřeba několik kroků:</span><span class="sxs-lookup"><span data-stu-id="14626-127">To build a data processing application on top of SCP, several steps are needed:</span></span>

* <span data-ttu-id="14626-128">Návrh a implementaci funkcích Spouts stáhnout data z fronty.</span><span class="sxs-lookup"><span data-stu-id="14626-128">Design and implement the Spouts to pull in data from queue.</span></span>
* <span data-ttu-id="14626-129">Návrh a implementaci funkce Bolts ke zpracování vstupních dat a uložit data do externího úložiště, například databáze.</span><span class="sxs-lookup"><span data-stu-id="14626-129">Design and implement Bolts to process the input data, and save data to external stores such as Database.</span></span>
* <span data-ttu-id="14626-130">Návrh topologie, odeslání a spusťte topologii.</span><span class="sxs-lookup"><span data-stu-id="14626-130">Design the topology, then submit and run the topology.</span></span> <span data-ttu-id="14626-131">Topologie definuje bodů uchycení a data toků mezi vrcholů.</span><span class="sxs-lookup"><span data-stu-id="14626-131">The topology defines vertexes and the data flows between the vertexes.</span></span> <span data-ttu-id="14626-132">Spojovací bod služby bude trvat specifikace topologie a nasaďte ji na cluster Storm, kde každý vrchol běží na jednom uzlu logické.</span><span class="sxs-lookup"><span data-stu-id="14626-132">SCP will take the topology specification and deploy it on a Storm cluster, where each vertex runs on one logical node.</span></span> <span data-ttu-id="14626-133">Převzetí služeb při selhání a škálování se postarat Storm Plánovač úloh.</span><span class="sxs-lookup"><span data-stu-id="14626-133">The failover and scaling will be taken care of by the Storm task scheduler.</span></span>

<span data-ttu-id="14626-134">Tento dokument pomocí jednoduché příklady ukážeme, jak sestavit aplikaci pro zpracování dat se spojovací bod služby.</span><span class="sxs-lookup"><span data-stu-id="14626-134">This document will use some simple examples to walk through how to build data processing application with SCP.</span></span>

## <a name="scp-plugin-interface"></a><span data-ttu-id="14626-135">Modul plug-in rozhraní spojovací bod služby</span><span class="sxs-lookup"><span data-stu-id="14626-135">SCP Plugin Interface</span></span>
<span data-ttu-id="14626-136">Moduly plug-in spojovací bod služby (nebo aplikace) je samostatná souborů EXE, které můžete i spustit v prostředí Visual Studio v průběhu fáze vývoje a zapojené do kanálu Storm po nasazení v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="14626-136">SCP plugins (or applications) are standalone EXEs that can both run inside Visual Studio during the development phase, and be plugged into the Storm pipeline after deployment in production.</span></span> <span data-ttu-id="14626-137">Psaní modulu plug-in spojovací bod služby je stejně jako zápis jakékoli jiné standardní konzolové aplikace systému Windows.</span><span class="sxs-lookup"><span data-stu-id="14626-137">Writing the SCP plugin is just the same as writing any other standard Windows console applications.</span></span> <span data-ttu-id="14626-138">Platforma SCP.NET deklaruje některé rozhraní pro funkcí spout/bolt a kód modulu plug-in uživatele by měla implementovat tato rozhraní.</span><span class="sxs-lookup"><span data-stu-id="14626-138">SCP.NET platform declares some interface for spout/bolt, and the user plugin code should implement these interfaces.</span></span> <span data-ttu-id="14626-139">Hlavním účelem tohoto návrhu je, že se uživatel zaměřit na své vlastní obchodní logics a nechá jiných věcí zpracovávat SCP.NET platformy.</span><span class="sxs-lookup"><span data-stu-id="14626-139">The main purpose of this design is that the user can focus on their own business logics, and leaving other things to be handled by SCP.NET platform.</span></span>

<span data-ttu-id="14626-140">Modul plug-in kód uživatele by měla implementovat jednu z těchto hodnot rozhraní, závisí na tom, jestli je topologie transakční nebo netransakční, a jestli je součást funkcích spout nebo bolt.</span><span class="sxs-lookup"><span data-stu-id="14626-140">The user plugin code should implement one of the followings interfaces, depends on whether the topology is transactional or non-transactional, and whether the component is spout or bolt.</span></span>

* <span data-ttu-id="14626-141">ISCPSpout</span><span class="sxs-lookup"><span data-stu-id="14626-141">ISCPSpout</span></span>
* <span data-ttu-id="14626-142">ISCPBolt</span><span class="sxs-lookup"><span data-stu-id="14626-142">ISCPBolt</span></span>
* <span data-ttu-id="14626-143">ISCPTxSpout</span><span class="sxs-lookup"><span data-stu-id="14626-143">ISCPTxSpout</span></span>
* <span data-ttu-id="14626-144">ISCPBatchBolt</span><span class="sxs-lookup"><span data-stu-id="14626-144">ISCPBatchBolt</span></span>

### <a name="iscpplugin"></a><span data-ttu-id="14626-145">ISCPPlugin</span><span class="sxs-lookup"><span data-stu-id="14626-145">ISCPPlugin</span></span>
<span data-ttu-id="14626-146">ISCPPlugin je společné rozhraní pro všechny typy modulů plug-in.</span><span class="sxs-lookup"><span data-stu-id="14626-146">ISCPPlugin is the common interface for all kinds of plugins.</span></span> <span data-ttu-id="14626-147">V současné době je fiktivní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="14626-147">Currently, it is a dummy interface.</span></span>

    public interface ISCPPlugin 
    {
    }

### <a name="iscpspout"></a><span data-ttu-id="14626-148">ISCPSpout</span><span class="sxs-lookup"><span data-stu-id="14626-148">ISCPSpout</span></span>
<span data-ttu-id="14626-149">ISCPSpout je rozhraní pro netransakční funkcí spout.</span><span class="sxs-lookup"><span data-stu-id="14626-149">ISCPSpout is the interface for non-transactional spout.</span></span>

     public interface ISCPSpout : ISCPPlugin                    
     {
         void NextTuple(Dictionary<string, Object> parms);         
         void Ack(long seqId, Dictionary<string, Object> parms);   
         void Fail(long seqId, Dictionary<string, Object> parms);  
     }

<span data-ttu-id="14626-150">Když `NextTuple()` nazývá C\# uživatelského kódu můžete emitování jeden nebo více řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="14626-150">When `NextTuple()` is called, the C\# user code can emit one or more tuples.</span></span> <span data-ttu-id="14626-151">Pokud není nic pro vydávání, tato metoda by měla vrátit bez generování nic.</span><span class="sxs-lookup"><span data-stu-id="14626-151">If there is nothing to emit, this method should return without emitting anything.</span></span> <span data-ttu-id="14626-152">Je potřeba poznamenat, `NextTuple()`, `Ack()`, a `Fail()` se nazývají ve smyčce úzkou v jedno vlákno v jazyce C\# procesu.</span><span class="sxs-lookup"><span data-stu-id="14626-152">It should be noted that `NextTuple()`, `Ack()`, and `Fail()` are all called in a tight loop in a single thread in C\# process.</span></span> <span data-ttu-id="14626-153">Pokud nejsou žádné řazených kolekcí členů pro vydávání, je zdvořilý mít NextTuple spánku krátkou dobu (například 10 ms) tak, aby odpady příliš mnoho procesoru.</span><span class="sxs-lookup"><span data-stu-id="14626-153">When there are no tuples to emit, it is courteous to have NextTuple sleep for a short amount of time (such as 10 milliseconds) so as not to waste too much CPU.</span></span>

<span data-ttu-id="14626-154">`Ack()`a `Fail()` bude volat pouze v případě potvrzení mechanismus je povolena v specifikace souboru.</span><span class="sxs-lookup"><span data-stu-id="14626-154">`Ack()` and `Fail()` will be called only when ack mechanism is enabled in spec file.</span></span> <span data-ttu-id="14626-155">`seqId` Slouží k identifikaci řazené kolekce členů, která je acked nebo se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="14626-155">The `seqId` is used to identify the tuple which is acked or failed.</span></span> <span data-ttu-id="14626-156">Takže pokud ack je povoleno v netransakční topologie, následující funkce vysílat by měl používat ve funkcích Spout:</span><span class="sxs-lookup"><span data-stu-id="14626-156">So if ack is enabled in non-transactional topology, the following emit function should be used in Spout:</span></span>

    public abstract void Emit(string streamId, List<object> values, long seqId); 

<span data-ttu-id="14626-157">Pokud ack není podporován v netransakční topologii `Ack()` a `Fail()` může být ponecháno prázdné funkce.</span><span class="sxs-lookup"><span data-stu-id="14626-157">If ack is not supported in non-transactional topology, the `Ack()` and `Fail()` can be left as empty function.</span></span>

<span data-ttu-id="14626-158">`parms` Vstupní parametry v tyto funkce jsou právě prázdný slovník, jsou vyhrazené pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="14626-158">The `parms` input parameters in these functions are just empty Dictionary, they are reserved for future use.</span></span>

### <a name="iscpbolt"></a><span data-ttu-id="14626-159">ISCPBolt</span><span class="sxs-lookup"><span data-stu-id="14626-159">ISCPBolt</span></span>
<span data-ttu-id="14626-160">ISCPBolt je rozhraní pro netransakční funkcí bolt.</span><span class="sxs-lookup"><span data-stu-id="14626-160">ISCPBolt is the interface for non-transactional bolt.</span></span>

    public interface ISCPBolt : ISCPPlugin 
    {
    void Execute(SCPTuple tuple);           
    }

<span data-ttu-id="14626-161">Pokud je k dispozici, nové řazené kolekce členů `Execute()` funkce bude volána k jeho zpracování.</span><span class="sxs-lookup"><span data-stu-id="14626-161">When new tuple is available, the `Execute()` function will be called to process it.</span></span>

### <a name="iscptxspout"></a><span data-ttu-id="14626-162">ISCPTxSpout</span><span class="sxs-lookup"><span data-stu-id="14626-162">ISCPTxSpout</span></span>
<span data-ttu-id="14626-163">ISCPTxSpout je rozhraní pro transakční funkcí spout.</span><span class="sxs-lookup"><span data-stu-id="14626-163">ISCPTxSpout is the interface for transactional spout.</span></span>

    public interface ISCPTxSpout : ISCPPlugin
    {
        void NextTx(out long seqId, Dictionary<string, Object> parms);  
        void Ack(long seqId, Dictionary<string, Object> parms);         
        void Fail(long seqId, Dictionary<string, Object> parms);        
    }

<span data-ttu-id="14626-164">Stejně jako netransakční kontrolní část `NextTx()`, `Ack()`, a `Fail()` se nazývají ve smyčce úzkou v jedno vlákno v jazyce C\# procesu.</span><span class="sxs-lookup"><span data-stu-id="14626-164">Just like their non-transactional counter-part, `NextTx()`, `Ack()`, and `Fail()` are all called in a tight loop in a single thread in C\# process.</span></span> <span data-ttu-id="14626-165">Pokud nejsou žádná data pro vydávání, je zdvořilý tak, aby měl `NextTx` režimu spánku pro krátké množství času (10 ms) tak, aby odpady příliš mnoho procesoru.</span><span class="sxs-lookup"><span data-stu-id="14626-165">When there are no data to emit, it is courteous to have `NextTx` sleep for a short amount of time (10 milliseconds) so as not to waste too much CPU.</span></span>

<span data-ttu-id="14626-166">`NextTx()`je volána spustit novou transakci, výstupní parametr `seqId` slouží k identifikaci transakce, která je použita také v `Ack()` a `Fail()`.</span><span class="sxs-lookup"><span data-stu-id="14626-166">`NextTx()` is called to start a new transaction, the out parameter `seqId` is used to identify the transaction, which is also used in `Ack()` and `Fail()`.</span></span> <span data-ttu-id="14626-167">V `NextTx()`, uživatel může posílat data na straně Java.</span><span class="sxs-lookup"><span data-stu-id="14626-167">In `NextTx()`, user can emit data to Java side.</span></span> <span data-ttu-id="14626-168">Data se uloží v ZooKeeper pro podporu opětovného přehrání.</span><span class="sxs-lookup"><span data-stu-id="14626-168">The data will be stored in ZooKeeper to support replay.</span></span> <span data-ttu-id="14626-169">Kapacitu ZooKeeper je velmi omezená, a proto by měl uživatel pouze emitování metadata, není hromadné dat v transakční funkcí spout.</span><span class="sxs-lookup"><span data-stu-id="14626-169">Because the capacity of ZooKeeper is very limited, user should only emit metadata, not bulk data in transactional spout.</span></span>

<span data-ttu-id="14626-170">Bude Storm přehráním transakce automaticky, pokud se nezdaří, tak `Fail()` by neměl být volán v případě normální.</span><span class="sxs-lookup"><span data-stu-id="14626-170">Storm will replay a transaction automatically if it fails, so `Fail()` should not be called in normal case.</span></span> <span data-ttu-id="14626-171">Ale pokud spojovací bod služby můžete zkontrolovat metadata vysílaných transakční spout, můžete volat `Fail()` po neplatná metadata.</span><span class="sxs-lookup"><span data-stu-id="14626-171">But if SCP can check the metadata emitted by transactional spout, it can call `Fail()` when the metadata is invalid.</span></span>

<span data-ttu-id="14626-172">`parms` Vstupní parametry v tyto funkce jsou právě prázdný slovník, jsou vyhrazené pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="14626-172">The `parms` input parameters in these functions are just empty Dictionary, they are reserved for future use.</span></span>

### <a name="iscpbatchbolt"></a><span data-ttu-id="14626-173">ISCPBatchBolt</span><span class="sxs-lookup"><span data-stu-id="14626-173">ISCPBatchBolt</span></span>
<span data-ttu-id="14626-174">ISCPBatchBolt je rozhraní pro transakční funkcí bolt.</span><span class="sxs-lookup"><span data-stu-id="14626-174">ISCPBatchBolt is the interface for transactional bolt.</span></span>

    public interface ISCPBatchBolt : ISCPPlugin           
    {
        void Execute(SCPTuple tuple);
        void FinishBatch(Dictionary<string, Object> parms);  
    }

<span data-ttu-id="14626-175">`Execute()`je volána, když je nové řazené kolekce členů přicházejících u bolt.</span><span class="sxs-lookup"><span data-stu-id="14626-175">`Execute()` is called when there is new tuple arriving at the bolt.</span></span> <span data-ttu-id="14626-176">`FinishBatch()`je volána, když je tato transakce skončila.</span><span class="sxs-lookup"><span data-stu-id="14626-176">`FinishBatch()` is called when this transaction is ended.</span></span> <span data-ttu-id="14626-177">`parms` Vstupní parametr je vyhrazena pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="14626-177">The `parms` input parameter is reserved for future use.</span></span>

<span data-ttu-id="14626-178">Pro transakční topologie, je důležité koncept – `StormTxAttempt`.</span><span class="sxs-lookup"><span data-stu-id="14626-178">For transactional topology, there is an important concept – `StormTxAttempt`.</span></span> <span data-ttu-id="14626-179">Má dvě pole `TxId` a `AttemptId`.</span><span class="sxs-lookup"><span data-stu-id="14626-179">It has two fields, `TxId` and `AttemptId`.</span></span> <span data-ttu-id="14626-180">`TxId`slouží k identifikaci konkrétní transakce, a pro dané transakci, může mít několik pokus transakce selže a je přehrány.</span><span class="sxs-lookup"><span data-stu-id="14626-180">`TxId` is used to identify a specific transaction, and for a given transaction, there may be multiple attempt if the transaction fails and is replayed.</span></span> <span data-ttu-id="14626-181">SCP.NET bude nové jiný objekt ISCPBatchBolt ke zpracování jednotlivých `StormTxAttempt`, jenom jako proveďte co Storm Java straně.</span><span class="sxs-lookup"><span data-stu-id="14626-181">SCP.NET will new a different ISCPBatchBolt object to process each `StormTxAttempt`, just like what Storm do in Java side.</span></span> <span data-ttu-id="14626-182">Účelem tohoto návrhu je podpora zpracování paralelní transakce.</span><span class="sxs-lookup"><span data-stu-id="14626-182">The purpose of this design is to support parallel transactions processing.</span></span> <span data-ttu-id="14626-183">Uživatel ho měli mít na paměti, pokud dokončení transakce pokus, budou zničena odpovídající objekt ISCPBatchBolt a uvolnění z paměti.</span><span class="sxs-lookup"><span data-stu-id="14626-183">User should keep it in mind that if transaction attempt is finished, the corresponding ISCPBatchBolt object will be destroyed and garbage collected.</span></span>

## <a name="object-model"></a><span data-ttu-id="14626-184">Objektový Model</span><span class="sxs-lookup"><span data-stu-id="14626-184">Object Model</span></span>
<span data-ttu-id="14626-185">SCP.NET také poskytuje jednoduchou sadou objekty klíče pro vývojáře k programu s.</span><span class="sxs-lookup"><span data-stu-id="14626-185">SCP.NET also provides a simple set of key objects for developers to program with.</span></span> <span data-ttu-id="14626-186">Jsou **kontextu**, **úložiště stavu**, a **SCPRuntime**.</span><span class="sxs-lookup"><span data-stu-id="14626-186">They are **Context**, **StateStore**, and **SCPRuntime**.</span></span> <span data-ttu-id="14626-187">Bude se popsané v části rest této části.</span><span class="sxs-lookup"><span data-stu-id="14626-187">They will be discussed in the rest part of this section.</span></span>

### <a name="context"></a><span data-ttu-id="14626-188">Kontext</span><span class="sxs-lookup"><span data-stu-id="14626-188">Context</span></span>
<span data-ttu-id="14626-189">Kontext poskytuje prostředí běžící aplikaci.</span><span class="sxs-lookup"><span data-stu-id="14626-189">Context provides a running environment to the application.</span></span> <span data-ttu-id="14626-190">Každá instance ISCPPlugin (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) má odpovídající instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="14626-190">Each ISCPPlugin instance (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) has a corresponding Context instance.</span></span> <span data-ttu-id="14626-191">Funkce poskytované službou kontextu je možné rozdělit do dvou částí: (1) statickou část, která je k dispozici v celé C\# zpracovat, (2) dynamické část, která je dostupná jenom pro konkrétní instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="14626-191">The functionality provided by Context can be divided into two parts: (1) the static part which is available in the whole C\# process, (2) the dynamic part which is only available for the specific Context instance.</span></span>

### <a name="static-part"></a><span data-ttu-id="14626-192">Statické části</span><span class="sxs-lookup"><span data-stu-id="14626-192">Static Part</span></span>
    public static ILogger Logger = null;
    public static SCPPluginType pluginType;                      
    public static Config Config { get; set; }                    
    public static TopologyContext TopologyContext { get; set; }  

<span data-ttu-id="14626-193">`Logger`pro účely protokolu poskytována.</span><span class="sxs-lookup"><span data-stu-id="14626-193">`Logger` is provided for log purpose.</span></span>

<span data-ttu-id="14626-194">`pluginType`slouží k označení typ modulu plug-in C\# procesu.</span><span class="sxs-lookup"><span data-stu-id="14626-194">`pluginType` is used to indicate the plugin type of the C\# process.</span></span> <span data-ttu-id="14626-195">Pokud C\# proces běží v režimu místního testovacího (bez Java), typ modulu plug-in je `SCP_NET_LOCAL`.</span><span class="sxs-lookup"><span data-stu-id="14626-195">If the C\# process is run in local test mode (without Java), the plugin type is `SCP_NET_LOCAL`.</span></span>

    public enum SCPPluginType 
    {
        SCP_NET_LOCAL = 0,       
        SCP_NET_SPOUT = 1,       
        SCP_NET_BOLT = 2,        
        SCP_NET_TX_SPOUT = 3,   
        SCP_NET_BATCH_BOLT = 4  
    }

<span data-ttu-id="14626-196">`Config`je k dispozici získat parametry konfigurace ze strany Java.</span><span class="sxs-lookup"><span data-stu-id="14626-196">`Config` is provided to get configuration parameters from Java side.</span></span> <span data-ttu-id="14626-197">Parametry jsou předány ze strany Java při C\# modul plug-in je inicializován.</span><span class="sxs-lookup"><span data-stu-id="14626-197">The parameters are passed from Java side when C\# plugin is initialized.</span></span> <span data-ttu-id="14626-198">`Config` Parametry jsou rozdělené do dvou částí: `stormConf` a `pluginConf`.</span><span class="sxs-lookup"><span data-stu-id="14626-198">The `Config` parameters are divided into two parts: `stormConf` and `pluginConf`.</span></span>

    public Dictionary<string, Object> stormConf { get; set; }  
    public Dictionary<string, Object> pluginConf { get; set; }  

<span data-ttu-id="14626-199">`stormConf`je parametry definované Storm a `pluginConf` je parametry definované spojovací bod služby.</span><span class="sxs-lookup"><span data-stu-id="14626-199">`stormConf` is parameters defined by Storm and `pluginConf` is the parameters defined by SCP.</span></span> <span data-ttu-id="14626-200">Například:</span><span class="sxs-lookup"><span data-stu-id="14626-200">For example:</span></span>

    public class Constants
    {
        … …

        // constant string for pluginConf
        public static readonly String NONTRANSACTIONAL_ENABLE_ACK = "nontransactional.ack.enabled";  

        // constant string for stormConf
        public static readonly String STORM_ZOOKEEPER_SERVERS = "storm.zookeeper.servers";           
        public static readonly String STORM_ZOOKEEPER_PORT = "storm.zookeeper.port";                 
    }

<span data-ttu-id="14626-201">`TopologyContext`je k dispozici získat kontext topologie, je nejvhodnější pro komponenty s více stupně paralelního zpracování.</span><span class="sxs-lookup"><span data-stu-id="14626-201">`TopologyContext` is provided to get the topology context, it is most useful for components with multiple parallelism.</span></span> <span data-ttu-id="14626-202">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="14626-202">Here is an example:</span></span>

    //demo how to get TopologyContext info
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)                      
    {
        Context.Logger.Info("TopologyContext info:");
        TopologyContext topologyContext = Context.TopologyContext;                    
        Context.Logger.Info("taskId: {0}", topologyContext.GetThisTaskId());          
        taskIndex = topologyContext.GetThisTaskIndex();
        Context.Logger.Info("taskIndex: {0}", taskIndex);
        string componentId = topologyContext.GetThisComponentId();                    
        Context.Logger.Info("componentId: {0}", componentId);
        List<int> componentTasks = topologyContext.GetComponentTasks(componentId);  
        Context.Logger.Info("taskNum: {0}", componentTasks.Count);                    
    }

### <a name="dynamic-part"></a><span data-ttu-id="14626-203">Dynamické části</span><span class="sxs-lookup"><span data-stu-id="14626-203">Dynamic Part</span></span>
<span data-ttu-id="14626-204">Následující rozhraní jsou relevantní pro určité instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="14626-204">The following interfaces are pertinent to a certain Context instance.</span></span> <span data-ttu-id="14626-205">Instance kontextu je vytvořený SCP.NET platformy a předaný uživatelského kódu:</span><span class="sxs-lookup"><span data-stu-id="14626-205">The Context instance is created by SCP.NET platform and passed to the user code:</span></span>

    // Declare the Output and Input Stream Schemas

    public void DeclareComponentSchema(ComponentStreamSchema schema);   

    // Emit tuple to default stream.
    public abstract void Emit(List<object> values);                   

    // Emit tuple to the specific stream.
    public abstract void Emit(string streamId, List<object> values);  

<span data-ttu-id="14626-206">Pro podporu ack netransakční funkcí spout je k dispozici následující metodu:</span><span class="sxs-lookup"><span data-stu-id="14626-206">For non-transactional spout supporting ack, the following method is provided:</span></span>

    // for non-transactional Spout which supports ack
    public abstract void Emit(string streamId, List<object> values, long seqId);  

<span data-ttu-id="14626-207">Pro podporu ack netransakční bolt, by měl explicitně `Ack()` nebo `Fail()` řazené kolekce členů přijala.</span><span class="sxs-lookup"><span data-stu-id="14626-207">For non-transactional bolt supporting ack, it should explicitly `Ack()` or `Fail()` the tuple it received.</span></span> <span data-ttu-id="14626-208">A při vytváření nové řazené kolekce členů, musíte také zadat kotvy nové řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="14626-208">And when emitting new tuple, it must also specify the anchors of the new tuple.</span></span> <span data-ttu-id="14626-209">Tyto metody jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="14626-209">The following methods are provided.</span></span>

    public abstract void Emit(string streamId, IEnumerable<SCPTuple> anchors, List<object> values); 
    public abstract void Ack(SCPTuple tuple);
    public abstract void Fail(SCPTuple tuple);

### <a name="statestore"></a><span data-ttu-id="14626-210">Úložiště stavu</span><span class="sxs-lookup"><span data-stu-id="14626-210">StateStore</span></span>
<span data-ttu-id="14626-211">`StateStore`poskytuje metadata služby, generování monotónní pořadí a bez čekání spolupráce.</span><span class="sxs-lookup"><span data-stu-id="14626-211">`StateStore` provides metadata services, monotonic sequence generation, and wait-free coordination.</span></span> <span data-ttu-id="14626-212">Abstrakce vyšší úrovně distribuované souběžnosti se dají vytvářet `StateStore`, včetně distribuované zámky, distribuované fronty, překážek a transakce služby.</span><span class="sxs-lookup"><span data-stu-id="14626-212">Higher-level distributed concurrency abstractions can be built on `StateStore`, including distributed locks, distributed queues, barriers, and transaction services.</span></span>

<span data-ttu-id="14626-213">Spojovací bod služby aplikace mohou používat `State` objekt, který chcete zachovat některé informace v ZooKeeper, zejména pro transakční topologii.</span><span class="sxs-lookup"><span data-stu-id="14626-213">SCP applications may use the `State` object to persist some information in ZooKeeper, especially for transactional topology.</span></span> <span data-ttu-id="14626-214">To tím, že pokud transakční spout zhroucení a restartování, může načíst informace potřebné z ZooKeeper a restartujte kanálu.</span><span class="sxs-lookup"><span data-stu-id="14626-214">Doing so, if transactional spout crashes and restart, it can retrieve the necessary information from ZooKeeper and restart the pipeline.</span></span>

<span data-ttu-id="14626-215">`StateStore` Objekt především má tyto metody:</span><span class="sxs-lookup"><span data-stu-id="14626-215">The `StateStore` object mainly has these methods:</span></span>

    /// <summary>
    /// Static method to retrieve a state store of the given path and connStr 
    /// </summary>
    /// <param name="storePath">StateStore Path</param>
    /// <param name="connStr">StateStore Address</param>
    /// <returns>Instance of StateStore</returns>
    public static StateStore Get(string storePath, string connStr);

    /// <summary>
    /// Create a new state object in this state store instance
    /// </summary>
    /// <returns>State from StateStore</returns>
    public State Create();

    /// <summary>
    /// Retrieve all states that were previously uncommitted, excluding all aborted states 
    /// </summary>
    /// <returns>Uncommited States</returns>
    public IEnumerable<State> GetUnCommitted();

    /// <summary>
    /// Get all the States in the StateStore
    /// </summary>
    /// <returns>All the States</returns>
    public IEnumerable<State> States();

    /// <summary>
    /// Get state or registry object
    /// </summary>
    /// <param name="info">Registry Name(Registry only)</param>
    /// <typeparam name="T">Type, Registry or State</typeparam>
    /// <returns>Return Registry or State</returns>
    public T Get<T>(string info = null);

    /// <summary>
    /// List all the committed states
    /// </summary>
    /// <returns>Registries contain the Committed State </returns> 
    public IEnumerable<Registry> Commited();

    /// <summary>
    /// List all the Aborted State in the StateStore
    /// </summary>
    /// <returns>Registries contain the Aborted State</returns>
    public IEnumerable<Registry> Aborted();

    /// <summary>
    /// Retrieve an existing state object from this state store instance 
    /// </summary>
    /// <returns>State from StateStore</returns>
    /// <typeparam name="T">stateId, id of the State</typeparam>
    public State GetState(long stateId)

<span data-ttu-id="14626-216">`State` Objekt především má tyto metody:</span><span class="sxs-lookup"><span data-stu-id="14626-216">The `State` object mainly has these methods:</span></span>

    /// <summary>
    /// Set the status of the state object to commit 
    /// </summary>
    public void Commit(bool simpleMode = true); 

    /// <summary>
    /// Set the status of the state object to abort 
    /// </summary>
    public void Abort();

    /// <summary>
    /// Put an attribute value under the give key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <param name="attribute">State Attribute</param> 
    public void PutAttribute<T>(string key, T attribute); 

    /// <summary>
    /// Get the attribute value associated with the given key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <returns>State Attribute</returns>               
    public T GetAttribute<T>(string key);                    

<span data-ttu-id="14626-217">Pro `Commit()` metoda simpleMode nastavena na hodnotu true, jednoduše odstraní odpovídající ZNode v ZooKeeper.</span><span class="sxs-lookup"><span data-stu-id="14626-217">For the `Commit()` method, when simpleMode is set to true, it will simply delete the corresponding ZNode in ZooKeeper.</span></span> <span data-ttu-id="14626-218">Jinak se odstraní aktuální ZNode a přidání nového uzlu v potvrzení\_cesta.</span><span class="sxs-lookup"><span data-stu-id="14626-218">Otherwise, it will delete the current ZNode, and adding a new node in the COMMITTED\_PATH.</span></span>

### <a name="scpruntime"></a><span data-ttu-id="14626-219">SCPRuntime</span><span class="sxs-lookup"><span data-stu-id="14626-219">SCPRuntime</span></span>
<span data-ttu-id="14626-220">SCPRuntime poskytuje následujících dvou metod.</span><span class="sxs-lookup"><span data-stu-id="14626-220">SCPRuntime provides the following two methods.</span></span>

    public static void Initialize();

    public static void LaunchPlugin(newSCPPlugin createDelegate);  

<span data-ttu-id="14626-221">`Initialize()`slouží k inicializaci běhové prostředí spojovací bod služby.</span><span class="sxs-lookup"><span data-stu-id="14626-221">`Initialize()` is used to initialize the SCP runtime environment.</span></span> <span data-ttu-id="14626-222">Tato metoda C\# proces se budou připojovat k straně Java a získá parametry konfigurace a topologie kontextu.</span><span class="sxs-lookup"><span data-stu-id="14626-222">In this method, the C\# process will connect to the Java side, and gets configuration parameters and topology context.</span></span>

<span data-ttu-id="14626-223">`LaunchPlugin()`slouží k ji smyčka zpracování zpráv.</span><span class="sxs-lookup"><span data-stu-id="14626-223">`LaunchPlugin()` is used to kick off the message processing loop.</span></span> <span data-ttu-id="14626-224">V této smyčky C\# modulu plug-in přijímat zprávy formuláře Java straně (včetně řazených kolekcí členů a řízení signály) a pak zpracování zprávy, například voláním metody rozhraní poskytují pomocí uživatelského kódu.</span><span class="sxs-lookup"><span data-stu-id="14626-224">In this loop, the C\# plugin will receive messages form Java side (including tuples and control signals), and then process the messages, perhaps calling the interface method provide by the user code.</span></span> <span data-ttu-id="14626-225">Vstupní parametr metody `LaunchPlugin()` je delegáta, který může vrátit objekt, který ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt rozhraní implementovat.</span><span class="sxs-lookup"><span data-stu-id="14626-225">The input parameter for method `LaunchPlugin()` is a delegate that can return an object that implement ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt interface.</span></span>

    public delegate ISCPPlugin newSCPPlugin(Context ctx, Dictionary\<string, Object\> parms); 

<span data-ttu-id="14626-226">Pro ISCPBatchBolt, nám získat `StormTxAttempt` z `parms`a použít ho k posoudit, zda se jedná o přehraná pokus.</span><span class="sxs-lookup"><span data-stu-id="14626-226">For ISCPBatchBolt, we can get `StormTxAttempt` from `parms`, and use it to judge whether it is a replayed attempt.</span></span> <span data-ttu-id="14626-227">To se obvykle provádí na bolt potvrzení a je znázorněn v `HelloWorldTx` příklad.</span><span class="sxs-lookup"><span data-stu-id="14626-227">This is usually done at the commit bolt, and it is demonstrated in the `HelloWorldTx` example.</span></span>

<span data-ttu-id="14626-228">Obecně řečeno moduly plug-in spojovací bod služby může spustit ve dvou režimech tady:</span><span class="sxs-lookup"><span data-stu-id="14626-228">Generally speaking, the SCP plugins may run in two modes here:</span></span>

1. <span data-ttu-id="14626-229">Místní testovací režim: V tomto režimu, moduly plug-in spojovací bod služby (C\# uživatelského kódu) spustit v průběhu fáze vývoje v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="14626-229">Local Test Mode: In this mode, the SCP plugins (the C\# user code) run inside Visual Studio during the development phase.</span></span> <span data-ttu-id="14626-230">`LocalContext`můžete použít v tomto režimu, který poskytuje metodu, jak serializovat emitovaného řazené kolekce členů do místní soubory a číst je zpět do paměti.</span><span class="sxs-lookup"><span data-stu-id="14626-230">`LocalContext` can be used in this mode, which provides method to serialize the emitted tuples to local files, and read them back to memory.</span></span>
   
        public interface ILocalContext
        {
            List\<SCPTuple\> RecvFromMsgQueue();
            void WriteMsgQueueToFile(string filepath, bool append = false);  
            void ReadFromFileToMsgQueue(string filepath);                    
        }
2. <span data-ttu-id="14626-231">Regulární režim: V tomto režimu, moduly plug-in spojovací bod služby spustily procesem storm java.</span><span class="sxs-lookup"><span data-stu-id="14626-231">Regular Mode: In this mode, the SCP plugins are launched by storm java process.</span></span>
   
    <span data-ttu-id="14626-232">Tady je příklad spuštění modulu plug-in spojovací bod služby:</span><span class="sxs-lookup"><span data-stu-id="14626-232">Here is an example of launching SCP plugin:</span></span>
   
        namespace Scp.App.HelloWorld
        {
        public class Generator : ISCPSpout
        {
            … …
            public static Generator Get(Context ctx, Dictionary<string, Object> parms)
            {
            return new Generator(ctx);
            }
        }
   
        class HelloWorld
        {
            static void Main(string[] args)
            {
            /* Setting the environment variable here can change the log file name */
            System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "HelloWorld");
   
            SCPRuntime.Initialize();
            SCPRuntime.LaunchPlugin(new newSCPPlugin(Generator.Get));
            }
        }
        }

## <a name="topology-specification-language"></a><span data-ttu-id="14626-233">Topologie specifikace jazyka</span><span class="sxs-lookup"><span data-stu-id="14626-233">Topology Specification Language</span></span>
<span data-ttu-id="14626-234">Určení topologie spojovací bod služby je domény konkrétní jazyk pro popis a konfiguraci topologie spojovací bod služby.</span><span class="sxs-lookup"><span data-stu-id="14626-234">SCP Topology Specification is a domain specific language for describing and configuring SCP topologies.</span></span> <span data-ttu-id="14626-235">Je založena na Storm na Clojure DSL (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) a je prodloužena spojovací bod služby.</span><span class="sxs-lookup"><span data-stu-id="14626-235">It is based on Storm’s Clojure DSL (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) and is extended by SCP.</span></span>

<span data-ttu-id="14626-236">Specifikace topologie jde odeslat přímo na clusteru storm pro provedení prostřednictvím ***runspec*** příkaz.</span><span class="sxs-lookup"><span data-stu-id="14626-236">Topology specifications can be submitted directly to storm cluster for execution via the ***runspec*** command.</span></span>

<span data-ttu-id="14626-237">SCP.NET má přidat funkce použijte k definování transakční topologie:</span><span class="sxs-lookup"><span data-stu-id="14626-237">SCP.NET has add follow functions to define the Transactional Topology:</span></span>

| <span data-ttu-id="14626-238">**Nové funkce**</span><span class="sxs-lookup"><span data-stu-id="14626-238">**New Functions**</span></span> | <span data-ttu-id="14626-239">**Parametry**</span><span class="sxs-lookup"><span data-stu-id="14626-239">**Parameters**</span></span> | <span data-ttu-id="14626-240">**Popis**</span><span class="sxs-lookup"><span data-stu-id="14626-240">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="14626-241">**TX topolopy**</span><span class="sxs-lookup"><span data-stu-id="14626-241">**tx-topolopy**</span></span> |<span data-ttu-id="14626-242">název topologie</span><span class="sxs-lookup"><span data-stu-id="14626-242">topology-name</span></span><br /><span data-ttu-id="14626-243">spout mapy</span><span class="sxs-lookup"><span data-stu-id="14626-243">spout-map</span></span><br /><span data-ttu-id="14626-244">bolt mapy</span><span class="sxs-lookup"><span data-stu-id="14626-244">bolt-map</span></span> |<span data-ttu-id="14626-245">Definovat topologii transakcí s názvem topologie &nbsp;spouts definice mapy a funkce bolts definice mapy</span><span class="sxs-lookup"><span data-stu-id="14626-245">Define a transactional topology with the topology name, &nbsp;spouts definition map and the bolts definition map</span></span> |
| <span data-ttu-id="14626-246">**spojovací bod služby. tx spout**</span><span class="sxs-lookup"><span data-stu-id="14626-246">**scp-tx-spout**</span></span> |<span data-ttu-id="14626-247">Exec – název</span><span class="sxs-lookup"><span data-stu-id="14626-247">exec-name</span></span><br /><span data-ttu-id="14626-248">argumentů</span><span class="sxs-lookup"><span data-stu-id="14626-248">args</span></span><br /><span data-ttu-id="14626-249">Pole</span><span class="sxs-lookup"><span data-stu-id="14626-249">fields</span></span> |<span data-ttu-id="14626-250">Definujte transakční funkcí spout.</span><span class="sxs-lookup"><span data-stu-id="14626-250">Define a transactional spout.</span></span> <span data-ttu-id="14626-251">Spustí aplikaci s ***exec název*** pomocí ***argumentů***.</span><span class="sxs-lookup"><span data-stu-id="14626-251">It will run the application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="14626-252">***Pole*** je výstup pole pro spout</span><span class="sxs-lookup"><span data-stu-id="14626-252">The ***fields*** is the Output Fields for spout</span></span> |
| <span data-ttu-id="14626-253">**spojovací bod služby tx-batch-bolt**</span><span class="sxs-lookup"><span data-stu-id="14626-253">**scp-tx-batch-bolt**</span></span> |<span data-ttu-id="14626-254">Exec – název</span><span class="sxs-lookup"><span data-stu-id="14626-254">exec-name</span></span><br /><span data-ttu-id="14626-255">argumentů</span><span class="sxs-lookup"><span data-stu-id="14626-255">args</span></span><br /><span data-ttu-id="14626-256">Pole</span><span class="sxs-lookup"><span data-stu-id="14626-256">fields</span></span> |<span data-ttu-id="14626-257">Definujte transakční Bolt dávky.</span><span class="sxs-lookup"><span data-stu-id="14626-257">Define a transactional Batch Bolt.</span></span> <span data-ttu-id="14626-258">Spustí aplikaci s ***exec název*** pomocí ***argumentů.***</span><span class="sxs-lookup"><span data-stu-id="14626-258">It will run the application with ***exec-name*** using ***args.***</span></span><br /><br /><span data-ttu-id="14626-259">Pole je pole výstup pro funkcí bolt.</span><span class="sxs-lookup"><span data-stu-id="14626-259">The Fields is the Output Fields for bolt.</span></span> |
| <span data-ttu-id="14626-260">**spojovací bod služby tx potvrzení bolt**</span><span class="sxs-lookup"><span data-stu-id="14626-260">**scp-tx-commit-bolt**</span></span> |<span data-ttu-id="14626-261">Exec – název</span><span class="sxs-lookup"><span data-stu-id="14626-261">exec-name</span></span><br /><span data-ttu-id="14626-262">argumentů</span><span class="sxs-lookup"><span data-stu-id="14626-262">args</span></span><br /><span data-ttu-id="14626-263">Pole</span><span class="sxs-lookup"><span data-stu-id="14626-263">fields</span></span> |<span data-ttu-id="14626-264">Definujte transakční Committer Bolt.</span><span class="sxs-lookup"><span data-stu-id="14626-264">Define a transactional Committer Bolt.</span></span> <span data-ttu-id="14626-265">Spustí aplikaci s ***exec název*** pomocí ***argumentů***.</span><span class="sxs-lookup"><span data-stu-id="14626-265">It will run the application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="14626-266">***Pole*** je výstup pole pro bolt</span><span class="sxs-lookup"><span data-stu-id="14626-266">The ***fields*** is the Output Fields for bolt</span></span> |
| <span data-ttu-id="14626-267">**nontx topolopy**</span><span class="sxs-lookup"><span data-stu-id="14626-267">**nontx-topolopy**</span></span> |<span data-ttu-id="14626-268">název topologie</span><span class="sxs-lookup"><span data-stu-id="14626-268">topology-name</span></span><br /><span data-ttu-id="14626-269">spout mapy</span><span class="sxs-lookup"><span data-stu-id="14626-269">spout-map</span></span><br /><span data-ttu-id="14626-270">bolt mapy</span><span class="sxs-lookup"><span data-stu-id="14626-270">bolt-map</span></span> |<span data-ttu-id="14626-271">Definovat topologii netransakční s názvem topologie&nbsp; spouts definice mapy a funkce bolts definice mapy</span><span class="sxs-lookup"><span data-stu-id="14626-271">Define a nontransactional topology with the topology name,&nbsp; spouts definition map and the bolts definition map</span></span> |
| <span data-ttu-id="14626-272">**spout spojovací bod služby**</span><span class="sxs-lookup"><span data-stu-id="14626-272">**scp-spout**</span></span> |<span data-ttu-id="14626-273">Exec – název</span><span class="sxs-lookup"><span data-stu-id="14626-273">exec-name</span></span><br /><span data-ttu-id="14626-274">argumentů</span><span class="sxs-lookup"><span data-stu-id="14626-274">args</span></span><br /><span data-ttu-id="14626-275">Pole</span><span class="sxs-lookup"><span data-stu-id="14626-275">fields</span></span><br /><span data-ttu-id="14626-276">Parametry</span><span class="sxs-lookup"><span data-stu-id="14626-276">parameters</span></span> |<span data-ttu-id="14626-277">Definujte netransakční spout.</span><span class="sxs-lookup"><span data-stu-id="14626-277">Define a nontransactional spout.</span></span> <span data-ttu-id="14626-278">Spustí aplikaci s ***exec název*** pomocí ***argumentů***.</span><span class="sxs-lookup"><span data-stu-id="14626-278">It will run the application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="14626-279">***Pole*** je výstup pole pro spout</span><span class="sxs-lookup"><span data-stu-id="14626-279">The ***fields*** is the Output Fields for spout</span></span><br /><br /><span data-ttu-id="14626-280">***Parametry*** je volitelná, jej použijete k zadání některých parametrů, třeba "nontransactional.ack.enabled".</span><span class="sxs-lookup"><span data-stu-id="14626-280">The ***parameters*** is optional, using it to specify some parameters such as "nontransactional.ack.enabled".</span></span> |
| <span data-ttu-id="14626-281">**bolt spojovací bod služby**</span><span class="sxs-lookup"><span data-stu-id="14626-281">**scp-bolt**</span></span> |<span data-ttu-id="14626-282">Exec – název</span><span class="sxs-lookup"><span data-stu-id="14626-282">exec-name</span></span><br /><span data-ttu-id="14626-283">argumentů</span><span class="sxs-lookup"><span data-stu-id="14626-283">args</span></span><br /><span data-ttu-id="14626-284">Pole</span><span class="sxs-lookup"><span data-stu-id="14626-284">fields</span></span><br /><span data-ttu-id="14626-285">Parametry</span><span class="sxs-lookup"><span data-stu-id="14626-285">parameters</span></span> |<span data-ttu-id="14626-286">Definujte netransakční funkcí Bolt.</span><span class="sxs-lookup"><span data-stu-id="14626-286">Define a nontransactional Bolt.</span></span> <span data-ttu-id="14626-287">Spustí aplikaci s ***exec název*** pomocí ***argumentů***.</span><span class="sxs-lookup"><span data-stu-id="14626-287">It will run the application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="14626-288">***Pole*** je výstup pole pro bolt</span><span class="sxs-lookup"><span data-stu-id="14626-288">The ***fields*** is the Output Fields for bolt</span></span><br /><br /><span data-ttu-id="14626-289">***Parametry*** je volitelná, jej použijete k zadání některých parametrů, třeba "nontransactional.ack.enabled".</span><span class="sxs-lookup"><span data-stu-id="14626-289">The ***parameters*** is optional, using it to specify some parameters such as "nontransactional.ack.enabled".</span></span> |

<span data-ttu-id="14626-290">SCP.NET má postupujte podle klíče slova definované:</span><span class="sxs-lookup"><span data-stu-id="14626-290">SCP.NET has follow keys words defined:</span></span>

| <span data-ttu-id="14626-291">**Klíčová slova**</span><span class="sxs-lookup"><span data-stu-id="14626-291">**Key Words**</span></span> | <span data-ttu-id="14626-292">**Popis**</span><span class="sxs-lookup"><span data-stu-id="14626-292">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="14626-293">**: název**</span><span class="sxs-lookup"><span data-stu-id="14626-293">**:name**</span></span> |<span data-ttu-id="14626-294">Definovat název topologie</span><span class="sxs-lookup"><span data-stu-id="14626-294">Define the Topology Name</span></span> |
| <span data-ttu-id="14626-295">**: topologie**</span><span class="sxs-lookup"><span data-stu-id="14626-295">**:topology**</span></span> |<span data-ttu-id="14626-296">Definovat topologii pomocí výše uvedených funkcí a sestavení v těch, které jsou.</span><span class="sxs-lookup"><span data-stu-id="14626-296">Define the Topology using the above functions and build in ones.</span></span> |
| <span data-ttu-id="14626-297">**: p**</span><span class="sxs-lookup"><span data-stu-id="14626-297">**:p**</span></span> |<span data-ttu-id="14626-298">Definujte paralelismus nápovědu pro každou funkcí spout nebo bolt.</span><span class="sxs-lookup"><span data-stu-id="14626-298">Define the parallelism hint for each spout or bolt.</span></span> |
| <span data-ttu-id="14626-299">**: Konfigurace**</span><span class="sxs-lookup"><span data-stu-id="14626-299">**:config**</span></span> |<span data-ttu-id="14626-300">Definování konfigurace parametru nebo aktualizovat existující</span><span class="sxs-lookup"><span data-stu-id="14626-300">Define configure parameter or update the existing ones</span></span> |
| <span data-ttu-id="14626-301">**: schéma**</span><span class="sxs-lookup"><span data-stu-id="14626-301">**:schema**</span></span> |<span data-ttu-id="14626-302">Definování schématu datového proudu.</span><span class="sxs-lookup"><span data-stu-id="14626-302">Define the Schema of Stream.</span></span> |

<span data-ttu-id="14626-303">A často používané parametry:</span><span class="sxs-lookup"><span data-stu-id="14626-303">And frequently-used parameters:</span></span>

| <span data-ttu-id="14626-304">**Parametr**</span><span class="sxs-lookup"><span data-stu-id="14626-304">**Parameter**</span></span> | <span data-ttu-id="14626-305">**Popis**</span><span class="sxs-lookup"><span data-stu-id="14626-305">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="14626-306">**"plugin.name"**</span><span class="sxs-lookup"><span data-stu-id="14626-306">**"plugin.name"**</span></span> |<span data-ttu-id="14626-307">Název souboru EXE modulu plug-in C#</span><span class="sxs-lookup"><span data-stu-id="14626-307">exe file name of the C# plugin</span></span> |
| <span data-ttu-id="14626-308">**"plugin.args"**</span><span class="sxs-lookup"><span data-stu-id="14626-308">**"plugin.args"**</span></span> |<span data-ttu-id="14626-309">argumentů modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="14626-309">plugin args</span></span> |
| <span data-ttu-id="14626-310">**"output.schema"**</span><span class="sxs-lookup"><span data-stu-id="14626-310">**"output.schema"**</span></span> |<span data-ttu-id="14626-311">Schéma výstupu</span><span class="sxs-lookup"><span data-stu-id="14626-311">Output schema</span></span> |
| <span data-ttu-id="14626-312">**"nontransactional.ack.enabled"**</span><span class="sxs-lookup"><span data-stu-id="14626-312">**"nontransactional.ack.enabled"**</span></span> |<span data-ttu-id="14626-313">Určuje, jestli je povolená ack pro netransakční topologie</span><span class="sxs-lookup"><span data-stu-id="14626-313">Whether ack is enabled for nontransactional topology</span></span> |

<span data-ttu-id="14626-314">Příkaz runspec nasadí společně s službu bits, je použití jako:</span><span class="sxs-lookup"><span data-stu-id="14626-314">The runspec command will be deployed together with the bits, the usage is like:</span></span>

    .\bin\runSpec.cmd
    usage: runSpec [spec-file target-dir [resource-dir] [-cp classpath]]
    ex: runSpec examples\HelloWorld\HelloWorld.spec specs examples\HelloWorld\Target

<span data-ttu-id="14626-315">***Prostředků dir*** parametr je nepovinný, budete muset zadat ho, když chcete zařadit a C\# aplikace a tento adresář bude obsahovat aplikaci, závislosti a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="14626-315">The ***resource-dir*** parameter is optional, you need to specify it when you want to plug a C\# application, and this directory will contain the application, the dependencies and configurations.</span></span>

<span data-ttu-id="14626-316">***Cesty pro třídy*** parametr je nepovinný.</span><span class="sxs-lookup"><span data-stu-id="14626-316">The ***classpath*** parameter is also optional.</span></span> <span data-ttu-id="14626-317">Slouží k určení cesty pro třídy Java, pokud specifikace soubor obsahuje Java Spout nebo Bolt.</span><span class="sxs-lookup"><span data-stu-id="14626-317">It is used to specify the Java classpath if the spec file contains Java Spout or Bolt.</span></span>

## <a name="miscellaneous-features"></a><span data-ttu-id="14626-318">Různé funkce</span><span class="sxs-lookup"><span data-stu-id="14626-318">Miscellaneous Features</span></span>
### <a name="input-and-output-schema-declaration"></a><span data-ttu-id="14626-319">Vstup a výstup schématu deklarace</span><span class="sxs-lookup"><span data-stu-id="14626-319">Input and Output Schema Declaration</span></span>
<span data-ttu-id="14626-320">Uživatel může emitování řazené kolekce členů v jazyce C\# procesu platformy musí serializovat řazenou kolekci členů do byte [], přenést na straně Java a Storm přenese tento řazené kolekce členů k cílům.</span><span class="sxs-lookup"><span data-stu-id="14626-320">The user can emit tuple in C\# process, the platform needs to serialize the tuple into byte[], transfer to Java side, and Storm will transfer this tuple to the targets.</span></span> <span data-ttu-id="14626-321">Mezitím v podřízené součásti, C\# proces bude přijímat řazené kolekce členů zpět z straně java a převést jej do původní typy podle platformy, všechny tyto operace jsou skryté platformou.</span><span class="sxs-lookup"><span data-stu-id="14626-321">Meanwhile in downstream component, the C\# process will receive tuple back from java side, and convert it to the original types by platform, all these operations are hidden by the Platform.</span></span>

<span data-ttu-id="14626-322">Pro podporu serializace a deserializace, musí deklarovat schéma vstupy a výstupy uživatelského kódu.</span><span class="sxs-lookup"><span data-stu-id="14626-322">To support the serialization and deserialization, user code needs to declare the schema of the inputs and outputs.</span></span>

<span data-ttu-id="14626-323">Schéma vstupu a výstupu datového proudu je definován jako slovník, klíč je StreamId a hodnota je typy sloupců.</span><span class="sxs-lookup"><span data-stu-id="14626-323">The input/output stream schema is defined as a dictionary, the key is the StreamId and the value is the Types of the columns.</span></span> <span data-ttu-id="14626-324">Součást může mít víc datových proudů deklarován.</span><span class="sxs-lookup"><span data-stu-id="14626-324">The component can have multi-streams declared.</span></span>

    public class ComponentStreamSchema
    {
        public Dictionary<string, List<Type>> InputStreamSchema { get; set; }
        public Dictionary<string, List<Type>> OutputStreamSchema { get; set; }
        public ComponentStreamSchema(Dictionary<string, List<Type>> input, Dictionary<string, List<Type>> output)
        {
            InputStreamSchema = input;
            OutputStreamSchema = output;
        }
    }


<span data-ttu-id="14626-325">V kontextu objektu máme následující rozhraní API, které jsou přidány:</span><span class="sxs-lookup"><span data-stu-id="14626-325">In Context object, we have the following API added:</span></span>

    public void DeclareComponentSchema(ComponentStreamSchema schema)

<span data-ttu-id="14626-326">Uživatelský kód musí zajistit řazené kolekce členů vygenerované orientují schéma definované pro tento datový proud, nebo systém vyvolá výjimku modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="14626-326">User code must make sure the tuples emitted obey the schema defined for that stream, or the system will throw a runtime exception.</span></span>

### <a name="multi-stream-support"></a><span data-ttu-id="14626-327">Podpora více datového proudu</span><span class="sxs-lookup"><span data-stu-id="14626-327">Multi-Stream Support</span></span>
<span data-ttu-id="14626-328">Spojovací bod služby podporuje uživatelského kódu emitování nebo přijmout z několika různých datových proudů ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="14626-328">SCP supports user code to emit or receive from multiple distinct streams at the same time.</span></span> <span data-ttu-id="14626-329">Podpora odráží v kontextu objektu jako vysílat metoda přebírá parametr ID typu volitelné datového proudu.</span><span class="sxs-lookup"><span data-stu-id="14626-329">The support reflects in the Context object as the Emit method takes an optional stream ID parameter.</span></span>

<span data-ttu-id="14626-330">Byly přidány dvě metody v SCP.NET kontextu objektu.</span><span class="sxs-lookup"><span data-stu-id="14626-330">Two methods in the SCP.NET Context object have been added.</span></span> <span data-ttu-id="14626-331">Používají se pro vydávání řazené kolekce členů nebo řazené kolekce členů, které slouží k zadání StreamId.</span><span class="sxs-lookup"><span data-stu-id="14626-331">They are used to emit Tuple or Tuples to specify StreamId.</span></span> <span data-ttu-id="14626-332">StreamId je řetězec a musí se jednat o konzistentní v obou C\# a specifikace definice topologie.</span><span class="sxs-lookup"><span data-stu-id="14626-332">The StreamId is a string and it needs to be consistent in both C\# and the Topology Definition Spec.</span></span>

        /* Emit tuple to the specific stream. */
        public abstract void Emit(string streamId, List<object> values);

        /* for non-transactional Spout only */
        public abstract void Emit(string streamId, List<object> values, long seqId);

<span data-ttu-id="14626-333">Emitování do datového proudu neexistující způsobí, že výjimky za běhu.</span><span class="sxs-lookup"><span data-stu-id="14626-333">The emitting to a non-existing stream will cause runtime exceptions.</span></span>

### <a name="fields-grouping"></a><span data-ttu-id="14626-334">Pole seskupení</span><span class="sxs-lookup"><span data-stu-id="14626-334">Fields Grouping</span></span>
<span data-ttu-id="14626-335">Seskupení pole sestavení v v Strom v SCP.NET nepracuje správně.</span><span class="sxs-lookup"><span data-stu-id="14626-335">The build-in Fields Grouping in Strom is not working properly in SCP.NET.</span></span> <span data-ttu-id="14626-336">Na straně Java Proxy se všechny datové typy polí ve skutečnosti byte [] a pole seskupování používá hash byte [] objektu k seskupení.</span><span class="sxs-lookup"><span data-stu-id="14626-336">On the Java Proxy side, all the fields data types are actually byte[], and the fields grouping uses the byte[] object hash code to perform the grouping.</span></span> <span data-ttu-id="14626-337">Hodnota hash objektu byte [] je adresa tohoto objektu v paměti.</span><span class="sxs-lookup"><span data-stu-id="14626-337">The byte[] object hash code is the address of this object in memory.</span></span> <span data-ttu-id="14626-338">Seskupení tak bude nesprávný pro dva bajty [] objekty, které sdílejí stejný obsah, ale není stejnou adresu.</span><span class="sxs-lookup"><span data-stu-id="14626-338">So the grouping will be wrong for two byte[] objects that share the same content but not the same address.</span></span>

<span data-ttu-id="14626-339">SCP.NET přidá metoda přizpůsobené seskupení a použije obsah byte [] k seskupení.</span><span class="sxs-lookup"><span data-stu-id="14626-339">SCP.NET adds a customized grouping method, and it will use the content of the byte[] to do the grouping.</span></span> <span data-ttu-id="14626-340">V **specifikace** soubor, syntaxe je jako:</span><span class="sxs-lookup"><span data-stu-id="14626-340">In **SPEC** file, the syntax is like:</span></span>

    (bolt-spec
        {
            "spout_test" (scp-field-group :non-tx [0,1])
        }
        …
    )


<span data-ttu-id="14626-341">Tady</span><span class="sxs-lookup"><span data-stu-id="14626-341">Here,</span></span>

1. <span data-ttu-id="14626-342">"spojovací bod služby skupiny polí" znamená "Vlastní pole seskupení implementované spojovací bod služby".</span><span class="sxs-lookup"><span data-stu-id="14626-342">"scp-field-group" means "Customized field grouping implemented by SCP".</span></span>
2. <span data-ttu-id="14626-343">": tx"nebo": bez tx" znamená, pokud je transakční topologie.</span><span class="sxs-lookup"><span data-stu-id="14626-343">":tx" or ":non-tx" means if it’s transactional topology.</span></span> <span data-ttu-id="14626-344">Tyto informace potřebujeme od počáteční index se liší v tx oproti bez tx topologie.</span><span class="sxs-lookup"><span data-stu-id="14626-344">We need this information since the starting index is different in tx vs. non-tx topologies.</span></span>
3. <span data-ttu-id="14626-345">[0,1] znamená hashset ID pole, počínaje od 0.</span><span class="sxs-lookup"><span data-stu-id="14626-345">[0,1] means a hashset of field Ids, starting from 0.</span></span>

### <a name="hybrid-topology"></a><span data-ttu-id="14626-346">Hybridní topologie</span><span class="sxs-lookup"><span data-stu-id="14626-346">Hybrid topology</span></span>
<span data-ttu-id="14626-347">Nativní Storm je napsán v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="14626-347">The native Storm is written in Java.</span></span> <span data-ttu-id="14626-348">A SCP.Net má rozšířené jej a povolte naše celní zápis C\# kód pro zpracování jejich obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="14626-348">And SCP.Net has enhanced it to enable our customs to write C\# code to handle their business logic.</span></span> <span data-ttu-id="14626-349">Také podporujeme hybridní topologie, která obsahuje nejen C, ale\# funkcích spouts nebo funkce bolts, ale také Java funkcí Spout/funkce Bolts.</span><span class="sxs-lookup"><span data-stu-id="14626-349">But we also support hybrid topologies, which contains not only C\# spouts/bolts, but also Java Spout/Bolts.</span></span>

### <a name="specify-java-spoutbolt-in-spec-file"></a><span data-ttu-id="14626-350">Zadat Java funkcí Spout/Bolt specifikace souboru</span><span class="sxs-lookup"><span data-stu-id="14626-350">Specify Java Spout/Bolt in spec file</span></span>
<span data-ttu-id="14626-351">V souboru specifikace "spojovací bod služby funkcí spout" a "spojovací bod služby funkcí bolt" lze použít také k určení Java Spouts a funkce Bolts, tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="14626-351">In spec file, "scp-spout" and "scp-bolt" can also be used to specify Java Spouts and Bolts, here is an example:</span></span>

    (spout-spec 
      (microsoft.scp.example.HybridTopology.Generator.)           
      :p 1)

<span data-ttu-id="14626-352">Zde `microsoft.scp.example.HybridTopology.Generator` je název třídy Java Spout.</span><span class="sxs-lookup"><span data-stu-id="14626-352">Here `microsoft.scp.example.HybridTopology.Generator` is the name of the Java Spout class.</span></span>

### <a name="specify-java-classpath-in-runspec-command"></a><span data-ttu-id="14626-353">Zadejte cestu pro třídy Java v runSpec příkaz</span><span class="sxs-lookup"><span data-stu-id="14626-353">Specify Java Classpath in runSpec Command</span></span>
<span data-ttu-id="14626-354">Pokud chcete odeslat topologie obsahující Java Spouts nebo funkce Bolts, musíte napřed zkompilovat Java Spouts nebo funkce Bolts a získání souborů Jar.</span><span class="sxs-lookup"><span data-stu-id="14626-354">If you want to submit topology containing Java Spouts or Bolts, you need to first compile the Java Spouts or Bolts and get the Jar files.</span></span> <span data-ttu-id="14626-355">Pak musíte zadat cestu pro třídy java, která obsahuje soubory Jar při odesílání topologie.</span><span class="sxs-lookup"><span data-stu-id="14626-355">Then you should specify the java classpath that contains the Jar files when submitting topology.</span></span> <span data-ttu-id="14626-356">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="14626-356">Here is an example:</span></span>

    bin\runSpec.cmd examples\HybridTopology\HybridTopology.spec specs examples\HybridTopology\net\Target -cp examples\HybridTopology\java\target\*

<span data-ttu-id="14626-357">Zde **příklady\\HybridTopology\\java\\cíl\\ ** je ve složce obsahující soubor Jar funkcí Spout/Bolt Java.</span><span class="sxs-lookup"><span data-stu-id="14626-357">Here **examples\\HybridTopology\\java\\target\\** is the folder containing the Java Spout/Bolt Jar file.</span></span>

### <a name="serialization-and-deserialization-between-java-and-c"></a><span data-ttu-id="14626-358">Serializace a deserializace mezi Java a C\\</span><span class="sxs-lookup"><span data-stu-id="14626-358">Serialization and Deserialization between Java and C\\</span></span>
<span data-ttu-id="14626-359">Součásti naší spojovací bod služby zahrnuje straně Java a C\# straně.</span><span class="sxs-lookup"><span data-stu-id="14626-359">Our SCP component includes Java side and C\# side.</span></span> <span data-ttu-id="14626-360">Chcete-li pracovat s nativní Java funkcích Spouts nebo funkce Bolts, musí být provedena serializaci nebo deserializaci mezi straně Java a C\# straně, jak je znázorněno v následujícím grafu.</span><span class="sxs-lookup"><span data-stu-id="14626-360">In order to interact with native Java Spouts/Bolts, Serialization/Deserialization must be carried out between Java side and C\# side, as illustrated in the following graph.</span></span>

![Diagram součásti java odesílání do komponenty spojovací bod služby odesílání do komponent v jazyce Java](media/hdinsight-storm-scp-programming-guide/java-compent-sending-to-scp-component-sending-to-java-component.png)

1. <span data-ttu-id="14626-362">**Serializace v jazyce Java straně a deserializace v jazyce C\# straně**</span><span class="sxs-lookup"><span data-stu-id="14626-362">**Serialization in Java side and Deserialization in C\# side**</span></span>
   
   <span data-ttu-id="14626-363">Nejprve poskytujeme výchozí implementace v jazyce Java straně serializace a deserializace v jazyce C\# straně.</span><span class="sxs-lookup"><span data-stu-id="14626-363">First we provide default implementation for serialization in Java side and deserialization in C\# side.</span></span> <span data-ttu-id="14626-364">Metoda serializace v jazyce Java straně lze zadat v souboru specifikace:</span><span class="sxs-lookup"><span data-stu-id="14626-364">The serialization method in Java side can be specified in SPEC file:</span></span>
   
       (scp-bolt
           {
               "plugin.name" "HybridTopology.exe"
               "plugin.args" ["displayer"]
               "output.schema" {}
               "customized.java.serializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer"]
           })
   
   <span data-ttu-id="14626-365">Metoda deserializace v jazyce C\# straně musí být zadán v jazyce C\# uživatelského kódu:</span><span class="sxs-lookup"><span data-stu-id="14626-365">The deserialization method in C\# side should be specified in C\# user code:</span></span>
   
       Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
       inputSchema.Add("default", new List<Type>() { typeof(Person) });
       this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
       this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());            
   
   <span data-ttu-id="14626-366">Tato výchozí implementace by měla řídit většinou, pokud datový typ není příliš složitý.</span><span class="sxs-lookup"><span data-stu-id="14626-366">This default implementation should handle most cases if the data type is not too complex.</span></span> <span data-ttu-id="14626-367">U některých případů, protože je příliš složitý uživatelský datový typ nebo protože výkon naše výchozí implementace nesplňuje požadavek na uživatele, uživatel může modul plug-in vlastní implementaci.</span><span class="sxs-lookup"><span data-stu-id="14626-367">For certain cases, either because the user data type is too complex, or because the performance of our default implementation does not meet the user's requirement, user can plug-in their own implementation.</span></span>
   
   <span data-ttu-id="14626-368">Serializace rozhraní na straně java je definován jako:</span><span class="sxs-lookup"><span data-stu-id="14626-368">The serialize interface in java side is defined as:</span></span>
   
       public interface ICustomizedInteropJavaSerializer {
           public void prepare(String[] args);
           public List<ByteBuffer> serialize(List<Object> objectList);
       }
   
   <span data-ttu-id="14626-369">Deserialize rozhraní v jazyce C\# straně je definován jako:</span><span class="sxs-lookup"><span data-stu-id="14626-369">The deserialize interface in C\# side is defined as:</span></span>
   
   <span data-ttu-id="14626-370">veřejné rozhraní ICustomizedInteropCSharpDeserializer</span><span class="sxs-lookup"><span data-stu-id="14626-370">public interface ICustomizedInteropCSharpDeserializer</span></span>
   
       public interface ICustomizedInteropCSharpDeserializer
       {
           List<Object> Deserialize(List<byte[]> dataList, List<Type> targetTypes);
       }
2. <span data-ttu-id="14626-371">**Serializace v jazyce C\# straně a deserializace v jazyce Java souběžný**</span><span class="sxs-lookup"><span data-stu-id="14626-371">**Serialization in C\# side and Deserialization in Java side side**</span></span>
   
   <span data-ttu-id="14626-372">Metoda serializace v jazyce C\# straně musí být zadán v jazyce C\# uživatelského kódu:</span><span class="sxs-lookup"><span data-stu-id="14626-372">The serialization method in C\# side should be specified in C\# user code:</span></span>
   
       this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer()); 
   
   <span data-ttu-id="14626-373">Metoda deserializace na straně Java musí být zadán v souboru specifikace:</span><span class="sxs-lookup"><span data-stu-id="14626-373">The Deserialization method in Java side should be specified in SPEC file:</span></span>
   
     <span data-ttu-id="14626-374">(spojovací bod služby spout</span><span class="sxs-lookup"><span data-stu-id="14626-374">(scp-spout</span></span>
   
       {
         "plugin.name" "HybridTopology.exe"
         "plugin.args" ["generator"]
         "output.schema" {"default" ["person"]}
         "customized.java.deserializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" "microsoft.scp.example.HybridTopology.Person"]
       })
   
   <span data-ttu-id="14626-375">Následuje název deserializátor "microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" a "microsoft.scp.example.HybridTopology.Person" je, že cílová třída dat se deserializovat k.</span><span class="sxs-lookup"><span data-stu-id="14626-375">Here "microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" is the name of Deserializer, and "microsoft.scp.example.HybridTopology.Person" is the target class the data is deserialized to.</span></span>
   
   <span data-ttu-id="14626-376">Uživatel může také modulu plug-in vlastní implementace C\# serializátor a deserializátor Java.</span><span class="sxs-lookup"><span data-stu-id="14626-376">User can also plug-in their own implementation of C\# serializer and Java Deserializer.</span></span> <span data-ttu-id="14626-377">Toto je rozhraní pro C\# serializátor:</span><span class="sxs-lookup"><span data-stu-id="14626-377">This is the interface for C\# serializer:</span></span>
   
       public interface ICustomizedInteropCSharpSerializer
       {
           List<byte[]> Serialize(List<object> dataList);
       }
   
   <span data-ttu-id="14626-378">Toto je rozhraní pro deserializátor Java:</span><span class="sxs-lookup"><span data-stu-id="14626-378">This is the interface for Java Deserializer:</span></span>
   
       public interface ICustomizedInteropJavaDeserializer {
           public void prepare(String[] targetClassNames);
           public List<Object> Deserialize(List<ByteBuffer> dataList);
       }

## <a name="scp-host-mode"></a><span data-ttu-id="14626-379">Spojovací bod služby hostitele režimu</span><span class="sxs-lookup"><span data-stu-id="14626-379">SCP Host Mode</span></span>
<span data-ttu-id="14626-380">V tomto režimu můžete uživatele zkompilovat jejich kódů DLL a používat k odesílání topologie SCPHost.exe poskytované spojovací bod služby.</span><span class="sxs-lookup"><span data-stu-id="14626-380">In this mode, user can compile their codes to DLL, and use SCPHost.exe provided by SCP to submit topology.</span></span> <span data-ttu-id="14626-381">Specifikace soubor vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="14626-381">The spec file looks like this:</span></span>

    (scp-spout
      {
        "plugin.name" "SCPHost.exe"
        "plugin.args" ["HelloWorld.dll" "Scp.App.HelloWorld.Generator" "Get"]
        "output.schema" {"default" ["sentence"]}
      })

<span data-ttu-id="14626-382">Zde `plugin.name` je zadán jako `SCPHost.exe` poskytované spojovací bod služby SDK.</span><span class="sxs-lookup"><span data-stu-id="14626-382">Here, `plugin.name` is specified as `SCPHost.exe` provided by SCP SDK.</span></span> <span data-ttu-id="14626-383">SCPHost.exe které přijímá přesně tři parametry:</span><span class="sxs-lookup"><span data-stu-id="14626-383">SCPHost.exe which accepts exactly three parameters:</span></span>

1. <span data-ttu-id="14626-384">První z nich je název knihovny DLL, která je `"HelloWorld.dll"` v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="14626-384">The first one is the DLL name, which is `"HelloWorld.dll"` in this example.</span></span>
2. <span data-ttu-id="14626-385">Druhá je název třídy, což je `"Scp.App.HelloWorld.Generator"` v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="14626-385">The second one is the Class name, which is `"Scp.App.HelloWorld.Generator"` in this example.</span></span>
3. <span data-ttu-id="14626-386">Třetí ten je název veřejné statické metody, který může vyvolat získat instanci ISCPPlugin.</span><span class="sxs-lookup"><span data-stu-id="14626-386">The third one is the name of a public static method, which can be invoked to get an instance of ISCPPlugin.</span></span>

<span data-ttu-id="14626-387">V režimu hostitele uživatelský kód kompiluje jako knihovny DLL a je vyvolána platformou spojovací bod služby.</span><span class="sxs-lookup"><span data-stu-id="14626-387">In host mode, user code is compiled as DLL, and is invoked by SCP platform.</span></span> <span data-ttu-id="14626-388">Spojovací bod služby platformy, můžete získat úplné řízení pro logiku celou zpracování.</span><span class="sxs-lookup"><span data-stu-id="14626-388">So SCP platform can get full control of the whole processing logic.</span></span> <span data-ttu-id="14626-389">Proto doporučujeme, abyste naše zákazníky, odeslání topologie v režimu hostitele spojovací bod služby, protože můžete zjednodušit vývojového prostředí a přineste nám větší flexibilitu a zpětnou kompatibilitu pro také novější verze.</span><span class="sxs-lookup"><span data-stu-id="14626-389">So we recommend our customers to submit topology in SCP host mode since it can simplify the development experience and bring us more flexibility and better backward compatibility for later release as well.</span></span>

## <a name="scp-programming-examples"></a><span data-ttu-id="14626-390">Příklady programování spojovací bod služby</span><span class="sxs-lookup"><span data-stu-id="14626-390">SCP Programming Examples</span></span>
### <a name="helloworld"></a><span data-ttu-id="14626-391">Hello World</span><span class="sxs-lookup"><span data-stu-id="14626-391">HelloWorld</span></span>
<span data-ttu-id="14626-392">**Hello World** je velmi jednoduchý příklad zobrazíte chuť SCP.Net.</span><span class="sxs-lookup"><span data-stu-id="14626-392">**HelloWorld** is a very simple example to show a taste of SCP.Net.</span></span> <span data-ttu-id="14626-393">Používá topologii netransakční s spout, nazývá **generátor**a dvě funkce bolts názvem **rozdělovače** a **čítač**.</span><span class="sxs-lookup"><span data-stu-id="14626-393">It uses a non-transactional topology, with a spout called **generator**, and two bolts called **splitter** and **counter**.</span></span> <span data-ttu-id="14626-394">Spout **generátor** se náhodně generovat některé věty a tyto věty k vydávání **rozdělovače**.</span><span class="sxs-lookup"><span data-stu-id="14626-394">The spout **generator** will randomly generate some sentences, and emit these sentences to **splitter**.</span></span> <span data-ttu-id="14626-395">Bolt **rozdělovače** bude rozdělení věty na slova a emitování tyto slova **čítač** funkcí bolt.</span><span class="sxs-lookup"><span data-stu-id="14626-395">The bolt **splitter** will split the sentences to words and emit these words to **counter** bolt.</span></span> <span data-ttu-id="14626-396">Bolt "čítač" používá slovník pro záznam výskyt počet jednotlivých slov.</span><span class="sxs-lookup"><span data-stu-id="14626-396">The bolt "counter" uses a dictionary to record the occurrence number of each word.</span></span>

<span data-ttu-id="14626-397">Existují dva soubory specifikace, **HelloWorld.spec** a **HelloWorld\_EnableAck.spec** v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="14626-397">There are two spec files, **HelloWorld.spec** and **HelloWorld\_EnableAck.spec** for this example.</span></span> <span data-ttu-id="14626-398">V C\# kódu, ho můžete zjistit, zda je povoleno potvrzení získáním pluginConf ze strany Java.</span><span class="sxs-lookup"><span data-stu-id="14626-398">In the C\# code, it can find out whether ack is enabled by getting the pluginConf from Java side.</span></span>

    /* demo how to get pluginConf info */
    if (Context.Config.pluginConf.ContainsKey(Constants.NONTRANSACTIONAL_ENABLE_ACK))
    {
        enableAck = (bool)(Context.Config.pluginConf[Constants.NONTRANSACTIONAL_ENABLE_ACK]);
    }
    Context.Logger.Info("enableAck: {0}", enableAck);

<span data-ttu-id="14626-399">Ve funkcích spout Pokud je povoleno potvrzení, slouží slovník pro ukládání do mezipaměti řazené kolekce členů, které nebyly acked.</span><span class="sxs-lookup"><span data-stu-id="14626-399">In the spout, if ack is enabled, a dictionary is used to cache the tuples that have not been acked.</span></span> <span data-ttu-id="14626-400">Pokud je volána Fail(), budou přehrány selhání řazené kolekce členů:</span><span class="sxs-lookup"><span data-stu-id="14626-400">If Fail() is called, the failed tuple will be replayed:</span></span>

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        Context.Logger.Info("Fail, seqId: {0}", seqId);
        if (cachedTuples.ContainsKey(seqId))
        {
            /* get the cached tuple */
            string sentence = cachedTuples[seqId];

            /* replay the failed tuple */
            Context.Logger.Info("Re-Emit: {0}, seqId: {1}", sentence, seqId);
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), seqId);
        }
        else
        {
            Context.Logger.Warn("Fail(), can't find cached tuple for seqId {0}!", seqId);
        }
    }

### <a name="helloworldtx"></a><span data-ttu-id="14626-401">HelloWorldTx</span><span class="sxs-lookup"><span data-stu-id="14626-401">HelloWorldTx</span></span>
<span data-ttu-id="14626-402">**HelloWorldTx** příklad ukazuje, jak implementovat transakční topologie.</span><span class="sxs-lookup"><span data-stu-id="14626-402">The **HelloWorldTx** example demonstrates how to implement transactional topology.</span></span> <span data-ttu-id="14626-403">Neměl mít jeden spout názvem **generátor**, funkce bolts batch názvem **partial počet**, potvrzení bolt s názvem **počet součet**.</span><span class="sxs-lookup"><span data-stu-id="14626-403">It have one spout called **generator**, a batch bolts called **partial-count**, and a commit bolt called **count-sum**.</span></span> <span data-ttu-id="14626-404">Existují také tři soubory txt předem vytvořené: **DataSource0.txt**, **DataSource1.txt** a **DataSource2.txt**.</span><span class="sxs-lookup"><span data-stu-id="14626-404">There are also three pre-created txt files: **DataSource0.txt**, **DataSource1.txt** and **DataSource2.txt**.</span></span>

<span data-ttu-id="14626-405">V každou transakci, spout **generátor** náhodně zvolím dva soubory z předem vytvořené tři soubory a emitování názvy dvou souborů k **partial počet** funkcí bolt.</span><span class="sxs-lookup"><span data-stu-id="14626-405">In each transaction, the spout **generator** will randomly choose two files from the pre-created three files, and emit the two file names to the **partial-count** bolt.</span></span> <span data-ttu-id="14626-406">Bolt **partial počet** bude nejprve získat název souboru z přijaté řazené kolekce členů, pak otevřete soubor a počet slov v tomto souboru a nakonec emitování word číslo, které má **počet součet** bolt.</span><span class="sxs-lookup"><span data-stu-id="14626-406">The bolt **partial-count** will first get the file name from the received tuple, then open the file and count the number of words in this file, and finally emit the word number to the **count-sum** bolt.</span></span> <span data-ttu-id="14626-407">**Počet součet** bolt představuje souhrn celkového počtu.</span><span class="sxs-lookup"><span data-stu-id="14626-407">The **count-sum** bolt will summarize the total count.</span></span>

<span data-ttu-id="14626-408">K dosažení **právě jednou** sémantiku, potvrzení bolt **počet součet** potřeba posoudit, zda se jedná o přehraná transakce.</span><span class="sxs-lookup"><span data-stu-id="14626-408">To achieve **exactly once** semantics, the commit bolt **count-sum** need to judge whether it is a replayed transaction.</span></span> <span data-ttu-id="14626-409">V tomto příkladu má proměnná statický člen:</span><span class="sxs-lookup"><span data-stu-id="14626-409">In this example, it has a static member variable:</span></span>

    public static long lastCommittedTxId = -1; 

<span data-ttu-id="14626-410">Když je vytvořena ISCPBatchBolt instance, se budou získávat `txAttempt` ze vstupní parametry:</span><span class="sxs-lookup"><span data-stu-id="14626-410">When an ISCPBatchBolt instance is created, it will get the `txAttempt` from input parameters:</span></span>

    public static CountSum Get(Context ctx, Dictionary<string, Object> parms)
    {
        /* for transactional topology, we can get txAttempt from the input parms */
        if (parms.ContainsKey(Constants.STORM_TX_ATTEMPT))
        {
            StormTxAttempt txAttempt = (StormTxAttempt)parms[Constants.STORM_TX_ATTEMPT];
            return new CountSum(ctx, txAttempt);
        }
        else
        {
            throw new Exception("null txAttempt");
        }
    }

<span data-ttu-id="14626-411">Když `FinishBatch()` je volána, `lastCommittedTxId` bude aktualizace, pokud není přehraná transakce.</span><span class="sxs-lookup"><span data-stu-id="14626-411">When `FinishBatch()` is called, the `lastCommittedTxId` will be update if it is not a replayed transaction.</span></span>

    public void FinishBatch(Dictionary<string, Object> parms)
    {
        /* judge whether it is a replayed transaction? */
        bool replay = (this.txAttempt.TxId <= lastCommittedTxId);

        if (!replay)
        {
            /* If it is not replayed, update the toalCount and lastCommittedTxId vaule */
            totalCount = totalCount + this.count;
            lastCommittedTxId = this.txAttempt.TxId;
        }
        … …
    }


### <a name="hybridtopology"></a><span data-ttu-id="14626-412">HybridTopology</span><span class="sxs-lookup"><span data-stu-id="14626-412">HybridTopology</span></span>
<span data-ttu-id="14626-413">Tato topologie obsahuje Java Spout a a C\# funkcí Bolt.</span><span class="sxs-lookup"><span data-stu-id="14626-413">This topology contains a Java Spout and a C\# Bolt.</span></span> <span data-ttu-id="14626-414">Používá výchozí serializace a deserializace implementace poskytované spojovací bod služby platformy.</span><span class="sxs-lookup"><span data-stu-id="14626-414">It uses the default serialization and deserialization implementation provided by SCP platform.</span></span> <span data-ttu-id="14626-415">Ref se prosím **HybridTopology.spec** v **příklady\\HybridTopology** složku podrobnosti specifikace souboru, a **SubmitTopology.bat** pro určení Cesta pro třídy Java.</span><span class="sxs-lookup"><span data-stu-id="14626-415">Please ref the **HybridTopology.spec** in **examples\\HybridTopology** folder for the spec file details, and **SubmitTopology.bat** for how to specify Java classpath.</span></span>

### <a name="scphostdemo"></a><span data-ttu-id="14626-416">SCPHostDemo</span><span class="sxs-lookup"><span data-stu-id="14626-416">SCPHostDemo</span></span>
<span data-ttu-id="14626-417">Tento příklad je stejný jako HelloWorld v zásadě.</span><span class="sxs-lookup"><span data-stu-id="14626-417">This example is the same as HelloWorld in essence.</span></span> <span data-ttu-id="14626-418">Jediným rozdílem je, že kompilace kódu uživatele jako knihovny DLL a topologii je odeslána pomocí SCPHost.exe.</span><span class="sxs-lookup"><span data-stu-id="14626-418">The only difference is that the user code is compiled as DLL and the topology is submitted by using SCPHost.exe.</span></span> <span data-ttu-id="14626-419">Prosím ref v části "Spojovací bod služby hostitele režim" podrobnější vysvětlení.</span><span class="sxs-lookup"><span data-stu-id="14626-419">Please ref the section "SCP Host Mode" for more detailed explanation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="14626-420">Další kroky</span><span class="sxs-lookup"><span data-stu-id="14626-420">Next Steps</span></span>
<span data-ttu-id="14626-421">Příklady topologií Storm vytvořený spojovací bod služby naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="14626-421">For examples of Storm topologies created using SCP, see the following:</span></span>

* [<span data-ttu-id="14626-422">Vývoj topologie C# pro Apache Storm v HDInsight pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="14626-422">Develop C# topologies for Apache Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [<span data-ttu-id="14626-423">Zpracování událostí z Azure Event Hubs se Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="14626-423">Process events from Azure Event Hubs with Storm on HDInsight</span></span>](hdinsight-storm-develop-csharp-event-hub-topology.md)
* [<span data-ttu-id="14626-424">Vytvoření více datových proudů v topologie C# Storm</span><span class="sxs-lookup"><span data-stu-id="14626-424">Create multiple data streams in a C# Storm topology</span></span>](hdinsight-storm-twitter-trending.md)
* [<span data-ttu-id="14626-425">Pomocí Power Bi vizualizovat data z topologie Storm</span><span class="sxs-lookup"><span data-stu-id="14626-425">Use Power Bi to visualize data from a Storm topology</span></span>](hdinsight-storm-power-bi-topology.md)
* [<span data-ttu-id="14626-426">Zpracování dat snímačů vehicle ze služby Event Hubs pomocí Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="14626-426">Process vehicle sensor data from Event Hubs using Storm on HDInsight</span></span>](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/IotExample)
* [<span data-ttu-id="14626-427">Extrakce, transformace a načítání (ETL) ze služby Azure Event Hubs k HBase</span><span class="sxs-lookup"><span data-stu-id="14626-427">Extract, Transform, and Load (ETL) from Azure Event Hubs to HBase</span></span>](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample)
* [<span data-ttu-id="14626-428">Korelovat události pomocí nástrojů Storm a HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="14626-428">Correlate events using Storm and HBase on HDInsight</span></span>](hdinsight-storm-correlation-topology.md)

