---
title: "Průvodce programováním aaaSCP.NET | Microsoft Docs"
description: "Zjistěte, jak toouse SCP.NET toocreate. Na základě NET topologie Storm pro použití se Storm v HDInsight."
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
ms.openlocfilehash: a57f4217b07e0e82a3f36650308695fbb45d9128
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scp-programming-guide"></a><span data-ttu-id="eaf4d-103">Průvodce programováním spojovací bod služby</span><span class="sxs-lookup"><span data-stu-id="eaf4d-103">SCP programming guide</span></span>
<span data-ttu-id="eaf4d-104">Spojovací bod služby je platforma toobuild reálném čase, spolehlivé, konzistentní a vysoký výkon zpracování dat aplikace.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-104">SCP is a platform toobuild real time, reliable, consistent and high performance data processing application.</span></span> <span data-ttu-id="eaf4d-105">Je založen na [Apache Storm](http://storm.incubator.apache.org/) – systém navrhovány hello OSS komunit zpracování datového proudu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-105">It is built on top of [Apache Storm](http://storm.incubator.apache.org/) -- a stream processing system designed by hello OSS communities.</span></span> <span data-ttu-id="eaf4d-106">Storm je určen podle Nathan Marz a otevřete která byla vytvořena pomocí služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-106">Storm is designed by Nathan Marz and open sourced by Twitter.</span></span> <span data-ttu-id="eaf4d-107">Využívá [Apache ZooKeeper](http://zookeeper.apache.org/), jiné Apache projektu tooenable vysoce spolehlivé distribuované koordinace a správa stavu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-107">It leverages [Apache ZooKeeper](http://zookeeper.apache.org/), another Apache project tooenable highly reliable distributed coordination and state management.</span></span> 

<span data-ttu-id="eaf4d-108">Pouze hello spojovací bod služby projektu Storm v systému Windows, která je součástí, ale také hello projektu přidat rozšíření a přizpůsobení pro ekosystém Windows hello.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-108">Not only hello SCP project ported Storm on Windows but also hello project added extensions and customization for hello Windows ecosystem.</span></span> <span data-ttu-id="eaf4d-109">rozšíření Hello zahrnují vývojáře prostředí .NET a knihovny, přizpůsobení hello obsahuje nasazení systému Windows.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-109">hello extensions include .NET developer experience, and libraries, hello customization includes Windows-based deployment.</span></span> 

<span data-ttu-id="eaf4d-110">Hello rozšíření a přizpůsobení se provádí tak, že jsme nemusejí toofork hello OSS projekty a jsme může využít odvozené ekosystémů nástavbou Storm.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-110">hello extension and customization is done in such a way that we do not need toofork hello OSS projects and we could leverage derived ecosystems built on top of Storm.</span></span>

## <a name="processing-model"></a><span data-ttu-id="eaf4d-111">Zpracování modelu</span><span class="sxs-lookup"><span data-stu-id="eaf4d-111">Processing model</span></span>
<span data-ttu-id="eaf4d-112">Hello data v spojovací bod služby je modelovaná jako nepřetržité proudy řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-112">hello data in SCP is modeled as continuous streams of tuples.</span></span> <span data-ttu-id="eaf4d-113">Obvykle hello řazených kolekcí členů toku do některé fronty nejprve pak zachyceny a transformovat obchodní logiky hostovanou v rámci topologie Storm, nakonec výstup hello může předat jako systém tooanother spojovací bod služby řazené kolekce členů, nebo být potvrdit toostores jako systém souborů DFS nebo databáze, jako SQL Server.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-113">Typically hello tuples flow into some queue first, then picked up, and transformed by business logic hosted inside a Storm topology, finally hello output could be piped as tuples tooanother SCP system, or be committed toostores like distributed file system or databases like SQL Server.</span></span>

![Diagram fronty napájení tooprocessing data, která kanály úložiště dat](media/hdinsight-storm-scp-programming-guide/queue-feeding-data-to-processing-to-data-store.png)

<span data-ttu-id="eaf4d-115">Topologie aplikací v Storm, definuje výpočetní graf.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-115">In Storm, an application topology defines a graph of computation.</span></span> <span data-ttu-id="eaf4d-116">Každý uzel v topologii obsahuje logiku zpracování a odkazů mezi uzly indikují datový tok.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-116">Each node in a topology contains processing logic, and links between nodes indicate data flow.</span></span> <span data-ttu-id="eaf4d-117">vstupní data tooinject Hello uzly do topologie hello se nazývají funkcích Spouts, které můžou být použité toosequence hello data.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-117">hello nodes tooinject input data into hello topology are called Spouts, which can be used toosequence hello data.</span></span> <span data-ttu-id="eaf4d-118">Hello vstupních dat může být umístěn v protokoly transakcí databáze, čítače výkonu systému atd. hello uzly s obou toky vstupní a výstupní data se označují jako funkce Bolts, které hello filtrování skutečná data a výběry a agregaci.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-118">hello input data could reside in file logs, transactional database, system performance counter etc. hello nodes with both input and output data flows are called Bolts, which do hello actual data filtering and selections and aggregation.</span></span>

<span data-ttu-id="eaf4d-119">Spojovací bod služby podporuje snažit v aspoň jednou a přesně-po zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-119">SCP supports best efforts, at-least-once and exactly-once data processing.</span></span> <span data-ttu-id="eaf4d-120">V distribuované aplikaci streamování zpracování různých chybám může dojít během zpracování dat, jako je výpadek sítě, selhání počítače nebo Chyba uživatelského kódu atd. Zpracování v aspoň jednou zajistí všechna data zpracují alespoň jednou podle přehrání automaticky hello stejná data při dochází k chybě.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-120">In a distributed streaming processing application, various errors may happen during data processing, such as network outage, machine failure, or user code error etc. At-least-once processing ensures all data will be processed at least once by replaying automatically hello same data when error happens.</span></span> <span data-ttu-id="eaf4d-121">Na alespoň jedno zpracování je jednoduché a spolehlivé a vyhovuje dobře v mnoha aplikacích.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-121">At-least-once processing is simple and reliable and suits well in many applications.</span></span> <span data-ttu-id="eaf4d-122">Pokud aplikace hello vyžaduje přesné počítání, například na alespoň jedno zpracování je však dostatek vzhledem k tomu, že hello stejných dat může potenciálně se přehrávají v topologii aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-122">However, when hello application requires exact counting, for example, at-least-once processing is insufficient since hello same data could potentially be played in hello application topology.</span></span> <span data-ttu-id="eaf4d-123">V takovém případě, přesně-po zpracování je navržený tak, že výsledek hello toomake je správný, i v případě, že hello data může být přehrány a zpracování více než jednou..</span><span class="sxs-lookup"><span data-stu-id="eaf4d-123">In that case, exactly-once processing is designed toomake sure hello result is correct even when hello data may be replayed and processed multiple times.</span></span>

<span data-ttu-id="eaf4d-124">Spojovací bod služby umožňuje .NET vývojáři toodevelop reálném čase data proces aplikacím při využívání hello Java Virtual Machine (JVM) na základě Storm v části titulní hello.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-124">SCP enables .NET developers toodevelop real time data process applications while leverage hello Java Virtual Machine (JVM) based Storm under hello cover.</span></span> <span data-ttu-id="eaf4d-125">Hello .NET a prostředí Java Virtual Machine komunikovat přes TCP místní soketu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-125">hello .NET and JVM communicate via TCP local socket.</span></span> <span data-ttu-id="eaf4d-126">V podstatě každou funkcí Spout/Bolt je proces pár .net nebo Java, kde běží logiku uživatelského hello v rozhraní .net procesu jako o modul plug-in.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-126">Basically each Spout/Bolt is a .Net/Java process pair, where hello user logic runs in .Net process as a plugin.</span></span>

<span data-ttu-id="eaf4d-127">toobuild data, zpracování aplikace nad spojovací bod služby, jsou potřeba několik kroků:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-127">toobuild a data processing application on top of SCP, several steps are needed:</span></span>

* <span data-ttu-id="eaf4d-128">Návrh a implementaci hello funkcích Spouts toopull v datech z fronty.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-128">Design and implement hello Spouts toopull in data from queue.</span></span>
* <span data-ttu-id="eaf4d-129">Návrh a implementaci funkce Bolts tooprocess hello vstupní data a uložit tooexternal úložiště dat, například databáze.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-129">Design and implement Bolts tooprocess hello input data, and save data tooexternal stores such as Database.</span></span>
* <span data-ttu-id="eaf4d-130">Návrh hello topologie, odeslání a spusťte hello topologie.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-130">Design hello topology, then submit and run hello topology.</span></span> <span data-ttu-id="eaf4d-131">Definuje bodů uchycení a hello data zprostředkovatele Hello topologie toků mezi bodů uchycení hello.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-131">hello topology defines vertexes and hello data flows between hello vertexes.</span></span> <span data-ttu-id="eaf4d-132">Spojovací bod služby bude trvat specifikace hello topologie a nasaďte ji na cluster Storm, kde každý vrchol běží na jednom uzlu logické.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-132">SCP will take hello topology specification and deploy it on a Storm cluster, where each vertex runs on one logical node.</span></span> <span data-ttu-id="eaf4d-133">Hello převzetí služeb při selhání a škálování se postarat pomocí plánovače úloh Storm hello.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-133">hello failover and scaling will be taken care of by hello Storm task scheduler.</span></span>

<span data-ttu-id="eaf4d-134">Tento dokument bude používat některé toowalk jednoduché příklady, jak pomocí aplikace toobuild zpracování dat se spojovací bod služby.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-134">This document will use some simple examples toowalk through how toobuild data processing application with SCP.</span></span>

## <a name="scp-plugin-interface"></a><span data-ttu-id="eaf4d-135">Modul plug-in rozhraní spojovací bod služby</span><span class="sxs-lookup"><span data-stu-id="eaf4d-135">SCP Plugin Interface</span></span>
<span data-ttu-id="eaf4d-136">Moduly plug-in spojovací bod služby (nebo aplikace) je samostatná souborů EXE, které můžete i spustit v prostředí Visual Studio v průběhu fáze vývoje hello a zapojené do kanálu hello Storm po nasazení v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-136">SCP plugins (or applications) are standalone EXEs that can both run inside Visual Studio during hello development phase, and be plugged into hello Storm pipeline after deployment in production.</span></span> <span data-ttu-id="eaf4d-137">Zápis modulu plug-in hello spojovací bod služby je hello právě stejné jako zápis jakékoli jiné standardní konzolové aplikace systému Windows.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-137">Writing hello SCP plugin is just hello same as writing any other standard Windows console applications.</span></span> <span data-ttu-id="eaf4d-138">Platforma SCP.NET deklaruje některé rozhraní pro funkcí spout/bolt a hello uživatelského modulu plug-in kódu by měla implementovat tato rozhraní.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-138">SCP.NET platform declares some interface for spout/bolt, and hello user plugin code should implement these interfaces.</span></span> <span data-ttu-id="eaf4d-139">hlavním účelem Hello tohoto návrhu je, že tento uživatel hello se zaměřit na své vlastní obchodní logics a nechá jiných věcí toobe zpracovávaných SCP.NET platformy.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-139">hello main purpose of this design is that hello user can focus on their own business logics, and leaving other things toobe handled by SCP.NET platform.</span></span>

<span data-ttu-id="eaf4d-140">Hello uživatelského modulu plug-in kódu by měla implementovat jednu z těchto hodnot rozhraní hello, závisí na tom, zda text hello topologie je transakční nebo netransakční, a jestli je součást hello funkcích spout nebo bolt.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-140">hello user plugin code should implement one of hello followings interfaces, depends on whether hello topology is transactional or non-transactional, and whether hello component is spout or bolt.</span></span>

* <span data-ttu-id="eaf4d-141">ISCPSpout</span><span class="sxs-lookup"><span data-stu-id="eaf4d-141">ISCPSpout</span></span>
* <span data-ttu-id="eaf4d-142">ISCPBolt</span><span class="sxs-lookup"><span data-stu-id="eaf4d-142">ISCPBolt</span></span>
* <span data-ttu-id="eaf4d-143">ISCPTxSpout</span><span class="sxs-lookup"><span data-stu-id="eaf4d-143">ISCPTxSpout</span></span>
* <span data-ttu-id="eaf4d-144">ISCPBatchBolt</span><span class="sxs-lookup"><span data-stu-id="eaf4d-144">ISCPBatchBolt</span></span>

### <a name="iscpplugin"></a><span data-ttu-id="eaf4d-145">ISCPPlugin</span><span class="sxs-lookup"><span data-stu-id="eaf4d-145">ISCPPlugin</span></span>
<span data-ttu-id="eaf4d-146">ISCPPlugin je hello společné rozhraní pro všechny typy modulů plug-in.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-146">ISCPPlugin is hello common interface for all kinds of plugins.</span></span> <span data-ttu-id="eaf4d-147">V současné době je fiktivní rozhraní.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-147">Currently, it is a dummy interface.</span></span>

    public interface ISCPPlugin 
    {
    }

### <a name="iscpspout"></a><span data-ttu-id="eaf4d-148">ISCPSpout</span><span class="sxs-lookup"><span data-stu-id="eaf4d-148">ISCPSpout</span></span>
<span data-ttu-id="eaf4d-149">ISCPSpout je hello rozhraní pro netransakční funkcí spout.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-149">ISCPSpout is hello interface for non-transactional spout.</span></span>

     public interface ISCPSpout : ISCPPlugin                    
     {
         void NextTuple(Dictionary<string, Object> parms);         
         void Ack(long seqId, Dictionary<string, Object> parms);   
         void Fail(long seqId, Dictionary<string, Object> parms);  
     }

<span data-ttu-id="eaf4d-150">Když `NextTuple()` nazývá hello C\# uživatelského kódu můžete emitování jeden nebo více řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-150">When `NextTuple()` is called, hello C\# user code can emit one or more tuples.</span></span> <span data-ttu-id="eaf4d-151">Pokud není nic tooemit, tato metoda by měla vrátit bez generování nic.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-151">If there is nothing tooemit, this method should return without emitting anything.</span></span> <span data-ttu-id="eaf4d-152">Je potřeba poznamenat, `NextTuple()`, `Ack()`, a `Fail()` se nazývají ve smyčce úzkou v jedno vlákno v jazyce C\# procesu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-152">It should be noted that `NextTuple()`, `Ack()`, and `Fail()` are all called in a tight loop in a single thread in C\# process.</span></span> <span data-ttu-id="eaf4d-153">Pokud neexistují žádné tooemit řazené kolekce členů, je zdvořilý toohave NextTuple spánku pro za krátkou dobu čas (například 10 ms), jako toowaste není příliš mnoho procesoru.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-153">When there are no tuples tooemit, it is courteous toohave NextTuple sleep for a short amount of time (such as 10 milliseconds) so as not toowaste too much CPU.</span></span>

<span data-ttu-id="eaf4d-154">`Ack()`a `Fail()` bude volat pouze v případě potvrzení mechanismus je povolena v specifikace souboru.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-154">`Ack()` and `Fail()` will be called only when ack mechanism is enabled in spec file.</span></span> <span data-ttu-id="eaf4d-155">Hello `seqId` je použité tooidentify hello řazené kolekce členů, které jsou acked nebo se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-155">hello `seqId` is used tooidentify hello tuple which is acked or failed.</span></span> <span data-ttu-id="eaf4d-156">Takže pokud ack je povoleno v netransakční topologie, je třeba používat následující funkce vysílat hello v Spout:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-156">So if ack is enabled in non-transactional topology, hello following emit function should be used in Spout:</span></span>

    public abstract void Emit(string streamId, List<object> values, long seqId); 

<span data-ttu-id="eaf4d-157">Pokud ack není podporován v netransakční topologii, hello `Ack()` a `Fail()` může být ponecháno prázdné funkce.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-157">If ack is not supported in non-transactional topology, hello `Ack()` and `Fail()` can be left as empty function.</span></span>

<span data-ttu-id="eaf4d-158">Hello `parms` vstupní parametry v tyto funkce jsou právě prázdný slovník, jsou vyhrazené pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-158">hello `parms` input parameters in these functions are just empty Dictionary, they are reserved for future use.</span></span>

### <a name="iscpbolt"></a><span data-ttu-id="eaf4d-159">ISCPBolt</span><span class="sxs-lookup"><span data-stu-id="eaf4d-159">ISCPBolt</span></span>
<span data-ttu-id="eaf4d-160">ISCPBolt je hello rozhraní pro netransakční funkcí bolt.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-160">ISCPBolt is hello interface for non-transactional bolt.</span></span>

    public interface ISCPBolt : ISCPPlugin 
    {
    void Execute(SCPTuple tuple);           
    }

<span data-ttu-id="eaf4d-161">Pokud je k dispozici nové řazené kolekce členů, hello `Execute()` funkce volaná tooprocess ho.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-161">When new tuple is available, hello `Execute()` function will be called tooprocess it.</span></span>

### <a name="iscptxspout"></a><span data-ttu-id="eaf4d-162">ISCPTxSpout</span><span class="sxs-lookup"><span data-stu-id="eaf4d-162">ISCPTxSpout</span></span>
<span data-ttu-id="eaf4d-163">ISCPTxSpout je hello rozhraní pro transakční funkcí spout.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-163">ISCPTxSpout is hello interface for transactional spout.</span></span>

    public interface ISCPTxSpout : ISCPPlugin
    {
        void NextTx(out long seqId, Dictionary<string, Object> parms);  
        void Ack(long seqId, Dictionary<string, Object> parms);         
        void Fail(long seqId, Dictionary<string, Object> parms);        
    }

<span data-ttu-id="eaf4d-164">Stejně jako netransakční kontrolní část `NextTx()`, `Ack()`, a `Fail()` se nazývají ve smyčce úzkou v jedno vlákno v jazyce C\# procesu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-164">Just like their non-transactional counter-part, `NextTx()`, `Ack()`, and `Fail()` are all called in a tight loop in a single thread in C\# process.</span></span> <span data-ttu-id="eaf4d-165">Pokud neexistují žádné tooemit dat, je zdvořilý toohave `NextTx` režimu spánku pro krátkou dobu (10 ms), jako toowaste není příliš mnoho procesoru.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-165">When there are no data tooemit, it is courteous toohave `NextTx` sleep for a short amount of time (10 milliseconds) so as not toowaste too much CPU.</span></span>

<span data-ttu-id="eaf4d-166">`NextTx()`je volána toostart novou transakci, hello vnější parametr `seqId` je použité tooidentify hello transakce, která je použita také v `Ack()` a `Fail()`.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-166">`NextTx()` is called toostart a new transaction, hello out parameter `seqId` is used tooidentify hello transaction, which is also used in `Ack()` and `Fail()`.</span></span> <span data-ttu-id="eaf4d-167">V `NextTx()`, uživatel může emitování straně tooJava data.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-167">In `NextTx()`, user can emit data tooJava side.</span></span> <span data-ttu-id="eaf4d-168">Hello data budou uložena v ZooKeeper toosupport opakování.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-168">hello data will be stored in ZooKeeper toosupport replay.</span></span> <span data-ttu-id="eaf4d-169">Hello kapacitu ZooKeeper je velmi omezená, a proto by měl uživatel emitování pouze metadata, ne hromadné data v transakční spout.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-169">Because hello capacity of ZooKeeper is very limited, user should only emit metadata, not bulk data in transactional spout.</span></span>

<span data-ttu-id="eaf4d-170">Bude Storm přehráním transakce automaticky, pokud se nezdaří, tak `Fail()` by neměl být volán v případě normální.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-170">Storm will replay a transaction automatically if it fails, so `Fail()` should not be called in normal case.</span></span> <span data-ttu-id="eaf4d-171">Ale pokud spojovací bod služby můžete zkontrolovat hello metadata vysílaných transakční spout, můžete volat `Fail()` při hello metadat je neplatná.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-171">But if SCP can check hello metadata emitted by transactional spout, it can call `Fail()` when hello metadata is invalid.</span></span>

<span data-ttu-id="eaf4d-172">Hello `parms` vstupní parametry v tyto funkce jsou právě prázdný slovník, jsou vyhrazené pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-172">hello `parms` input parameters in these functions are just empty Dictionary, they are reserved for future use.</span></span>

### <a name="iscpbatchbolt"></a><span data-ttu-id="eaf4d-173">ISCPBatchBolt</span><span class="sxs-lookup"><span data-stu-id="eaf4d-173">ISCPBatchBolt</span></span>
<span data-ttu-id="eaf4d-174">ISCPBatchBolt je hello rozhraní pro transakční funkcí bolt.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-174">ISCPBatchBolt is hello interface for transactional bolt.</span></span>

    public interface ISCPBatchBolt : ISCPPlugin           
    {
        void Execute(SCPTuple tuple);
        void FinishBatch(Dictionary<string, Object> parms);  
    }

<span data-ttu-id="eaf4d-175">`Execute()`je volána, když je nové řazené kolekce členů přicházejících u hello bolt.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-175">`Execute()` is called when there is new tuple arriving at hello bolt.</span></span> <span data-ttu-id="eaf4d-176">`FinishBatch()`je volána, když je tato transakce skončila.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-176">`FinishBatch()` is called when this transaction is ended.</span></span> <span data-ttu-id="eaf4d-177">Hello `parms` vstupní parametr je vyhrazena pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-177">hello `parms` input parameter is reserved for future use.</span></span>

<span data-ttu-id="eaf4d-178">Pro transakční topologie, je důležité koncept – `StormTxAttempt`.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-178">For transactional topology, there is an important concept – `StormTxAttempt`.</span></span> <span data-ttu-id="eaf4d-179">Má dvě pole `TxId` a `AttemptId`.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-179">It has two fields, `TxId` and `AttemptId`.</span></span> <span data-ttu-id="eaf4d-180">`TxId`je použité tooidentify konkrétní transakce a pro dané transakci, může mít několik pokus hello transakce selže a je přehrány.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-180">`TxId` is used tooidentify a specific transaction, and for a given transaction, there may be multiple attempt if hello transaction fails and is replayed.</span></span> <span data-ttu-id="eaf4d-181">SCP.NET se nový různých ISCPBatchBolt objekt tooprocess každý `StormTxAttempt`, jenom jako proveďte co Storm Java straně.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-181">SCP.NET will new a different ISCPBatchBolt object tooprocess each `StormTxAttempt`, just like what Storm do in Java side.</span></span> <span data-ttu-id="eaf4d-182">účelem Hello tohoto návrhu je toosupport zpracování paralelní transakce.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-182">hello purpose of this design is toosupport parallel transactions processing.</span></span> <span data-ttu-id="eaf4d-183">Uživatel by měl mějte ho, pokud transakce pokus po dokončení, odpovídající objekt ISCPBatchBolt hello budou zničena a uklizeny.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-183">User should keep it in mind that if transaction attempt is finished, hello corresponding ISCPBatchBolt object will be destroyed and garbage collected.</span></span>

## <a name="object-model"></a><span data-ttu-id="eaf4d-184">Objektový Model</span><span class="sxs-lookup"><span data-stu-id="eaf4d-184">Object Model</span></span>
<span data-ttu-id="eaf4d-185">SCP.NET také poskytuje pro vývojáře tooprogram s jednoduchou sadou objekty klíče.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-185">SCP.NET also provides a simple set of key objects for developers tooprogram with.</span></span> <span data-ttu-id="eaf4d-186">Jsou **kontextu**, **úložiště stavu**, a **SCPRuntime**.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-186">They are **Context**, **StateStore**, and **SCPRuntime**.</span></span> <span data-ttu-id="eaf4d-187">Bude se zabývá hello rest součástí této části.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-187">They will be discussed in hello rest part of this section.</span></span>

### <a name="context"></a><span data-ttu-id="eaf4d-188">Kontext</span><span class="sxs-lookup"><span data-stu-id="eaf4d-188">Context</span></span>
<span data-ttu-id="eaf4d-189">Poskytuje kontext spuštěné aplikace toohello prostředí.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-189">Context provides a running environment toohello application.</span></span> <span data-ttu-id="eaf4d-190">Každá instance ISCPPlugin (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) má odpovídající instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-190">Each ISCPPlugin instance (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) has a corresponding Context instance.</span></span> <span data-ttu-id="eaf4d-191">Hello funkce poskytované službou kontextu je možné rozdělit do dvou částí: (1) hello statickou část, která je k dispozici v hello celou C\# zpracovat, (2) hello dynamické část, která je dostupná jenom pro konkrétní instance kontextu hello.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-191">hello functionality provided by Context can be divided into two parts: (1) hello static part which is available in hello whole C\# process, (2) hello dynamic part which is only available for hello specific Context instance.</span></span>

### <a name="static-part"></a><span data-ttu-id="eaf4d-192">Statické části</span><span class="sxs-lookup"><span data-stu-id="eaf4d-192">Static Part</span></span>
    public static ILogger Logger = null;
    public static SCPPluginType pluginType;                      
    public static Config Config { get; set; }                    
    public static TopologyContext TopologyContext { get; set; }  

<span data-ttu-id="eaf4d-193">`Logger`pro účely protokolu poskytována.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-193">`Logger` is provided for log purpose.</span></span>

<span data-ttu-id="eaf4d-194">`pluginType`je použit typ modulu plug-in hello tooindicate hello C\# procesu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-194">`pluginType` is used tooindicate hello plugin type of hello C\# process.</span></span> <span data-ttu-id="eaf4d-195">Pokud hello C\# proces běží v režimu místního testovacího (bez Java), typ modulu plug-in hello je `SCP_NET_LOCAL`.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-195">If hello C\# process is run in local test mode (without Java), hello plugin type is `SCP_NET_LOCAL`.</span></span>

    public enum SCPPluginType 
    {
        SCP_NET_LOCAL = 0,       
        SCP_NET_SPOUT = 1,       
        SCP_NET_BOLT = 2,        
        SCP_NET_TX_SPOUT = 3,   
        SCP_NET_BATCH_BOLT = 4  
    }

<span data-ttu-id="eaf4d-196">`Config`je k dispozici parametry konfigurace tooget ze strany Java.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-196">`Config` is provided tooget configuration parameters from Java side.</span></span> <span data-ttu-id="eaf4d-197">Hello parametry se jí předávají ze strany Java při C\# modul plug-in je inicializován.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-197">hello parameters are passed from Java side when C\# plugin is initialized.</span></span> <span data-ttu-id="eaf4d-198">Hello `Config` parametry jsou rozdělené do dvou částí: `stormConf` a `pluginConf`.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-198">hello `Config` parameters are divided into two parts: `stormConf` and `pluginConf`.</span></span>

    public Dictionary<string, Object> stormConf { get; set; }  
    public Dictionary<string, Object> pluginConf { get; set; }  

<span data-ttu-id="eaf4d-199">`stormConf`je parametry definované Storm a `pluginConf` je hello parametry definované spojovací bod služby.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-199">`stormConf` is parameters defined by Storm and `pluginConf` is hello parameters defined by SCP.</span></span> <span data-ttu-id="eaf4d-200">Například:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-200">For example:</span></span>

    public class Constants
    {
        … …

        // constant string for pluginConf
        public static readonly String NONTRANSACTIONAL_ENABLE_ACK = "nontransactional.ack.enabled";  

        // constant string for stormConf
        public static readonly String STORM_ZOOKEEPER_SERVERS = "storm.zookeeper.servers";           
        public static readonly String STORM_ZOOKEEPER_PORT = "storm.zookeeper.port";                 
    }

<span data-ttu-id="eaf4d-201">`TopologyContext`je zadaný tooget hello topologie kontextu, je nejvhodnější pro komponenty s více stupně paralelního zpracování.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-201">`TopologyContext` is provided tooget hello topology context, it is most useful for components with multiple parallelism.</span></span> <span data-ttu-id="eaf4d-202">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-202">Here is an example:</span></span>

    //demo how tooget TopologyContext info
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

### <a name="dynamic-part"></a><span data-ttu-id="eaf4d-203">Dynamické části</span><span class="sxs-lookup"><span data-stu-id="eaf4d-203">Dynamic Part</span></span>
<span data-ttu-id="eaf4d-204">Hello následující rozhraní jsou příslušné tooa určité instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-204">hello following interfaces are pertinent tooa certain Context instance.</span></span> <span data-ttu-id="eaf4d-205">instance kontextu Hello se vytvoří platformou SCP.NET a předán toohello uživatelského kódu:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-205">hello Context instance is created by SCP.NET platform and passed toohello user code:</span></span>

    // Declare hello Output and Input Stream Schemas

    public void DeclareComponentSchema(ComponentStreamSchema schema);   

    // Emit tuple toodefault stream.
    public abstract void Emit(List<object> values);                   

    // Emit tuple toohello specific stream.
    public abstract void Emit(string streamId, List<object> values);  

<span data-ttu-id="eaf4d-206">Pro podporu ack netransakční funkcí spout je poskytována hello následující metodu:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-206">For non-transactional spout supporting ack, hello following method is provided:</span></span>

    // for non-transactional Spout which supports ack
    public abstract void Emit(string streamId, List<object> values, long seqId);  

<span data-ttu-id="eaf4d-207">Pro podporu ack netransakční bolt, by měl explicitně `Ack()` nebo `Fail()` hello obdržel řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-207">For non-transactional bolt supporting ack, it should explicitly `Ack()` or `Fail()` hello tuple it received.</span></span> <span data-ttu-id="eaf4d-208">A při vytváření nové řazené kolekce členů, musíte také zadat hello kotvy hello nové řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-208">And when emitting new tuple, it must also specify hello anchors of hello new tuple.</span></span> <span data-ttu-id="eaf4d-209">Hello následující metody jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-209">hello following methods are provided.</span></span>

    public abstract void Emit(string streamId, IEnumerable<SCPTuple> anchors, List<object> values); 
    public abstract void Ack(SCPTuple tuple);
    public abstract void Fail(SCPTuple tuple);

### <a name="statestore"></a><span data-ttu-id="eaf4d-210">Úložiště stavu</span><span class="sxs-lookup"><span data-stu-id="eaf4d-210">StateStore</span></span>
<span data-ttu-id="eaf4d-211">`StateStore`poskytuje metadata služby, generování monotónní pořadí a bez čekání spolupráce.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-211">`StateStore` provides metadata services, monotonic sequence generation, and wait-free coordination.</span></span> <span data-ttu-id="eaf4d-212">Abstrakce vyšší úrovně distribuované souběžnosti se dají vytvářet `StateStore`, včetně distribuované zámky, distribuované fronty, překážek a transakce služby.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-212">Higher-level distributed concurrency abstractions can be built on `StateStore`, including distributed locks, distributed queues, barriers, and transaction services.</span></span>

<span data-ttu-id="eaf4d-213">Spojovací bod služby aplikace mohou používat hello `State` objektu toopersist některé informace v ZooKeeper, zejména pro transakční topologii.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-213">SCP applications may use hello `State` object toopersist some information in ZooKeeper, especially for transactional topology.</span></span> <span data-ttu-id="eaf4d-214">To proto, pokud transakční spout zhroucení a restartování, může načíst hello nezbytné informace z ZooKeeper a restartujte hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-214">Doing so, if transactional spout crashes and restart, it can retrieve hello necessary information from ZooKeeper and restart hello pipeline.</span></span>

<span data-ttu-id="eaf4d-215">Hello `StateStore` objekt především má tyto metody:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-215">hello `StateStore` object mainly has these methods:</span></span>

    /// <summary>
    /// Static method tooretrieve a state store of hello given path and connStr 
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
    /// Get all hello States in hello StateStore
    /// </summary>
    /// <returns>All hello States</returns>
    public IEnumerable<State> States();

    /// <summary>
    /// Get state or registry object
    /// </summary>
    /// <param name="info">Registry Name(Registry only)</param>
    /// <typeparam name="T">Type, Registry or State</typeparam>
    /// <returns>Return Registry or State</returns>
    public T Get<T>(string info = null);

    /// <summary>
    /// List all hello committed states
    /// </summary>
    /// <returns>Registries contain hello Committed State </returns> 
    public IEnumerable<Registry> Commited();

    /// <summary>
    /// List all hello Aborted State in hello StateStore
    /// </summary>
    /// <returns>Registries contain hello Aborted State</returns>
    public IEnumerable<Registry> Aborted();

    /// <summary>
    /// Retrieve an existing state object from this state store instance 
    /// </summary>
    /// <returns>State from StateStore</returns>
    /// <typeparam name="T">stateId, id of hello State</typeparam>
    public State GetState(long stateId)

<span data-ttu-id="eaf4d-216">Hello `State` objekt především má tyto metody:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-216">hello `State` object mainly has these methods:</span></span>

    /// <summary>
    /// Set hello status of hello state object toocommit 
    /// </summary>
    public void Commit(bool simpleMode = true); 

    /// <summary>
    /// Set hello status of hello state object tooabort 
    /// </summary>
    public void Abort();

    /// <summary>
    /// Put an attribute value under hello give key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <param name="attribute">State Attribute</param> 
    public void PutAttribute<T>(string key, T attribute); 

    /// <summary>
    /// Get hello attribute value associated with hello given key 
    /// </summary>
    /// <param name="key">Key</param> 
    /// <returns>State Attribute</returns>               
    public T GetAttribute<T>(string key);                    

<span data-ttu-id="eaf4d-217">Pro hello `Commit()` metodu, pokud je nastavena simpleMode tootrue, jednoduše odstraní odpovídající ZNode v ZooKeeper hello.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-217">For hello `Commit()` method, when simpleMode is set tootrue, it will simply delete hello corresponding ZNode in ZooKeeper.</span></span> <span data-ttu-id="eaf4d-218">Jinak se odstraní hello aktuální ZNode a přidání nového uzlu v hello potvrzení\_cesta.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-218">Otherwise, it will delete hello current ZNode, and adding a new node in hello COMMITTED\_PATH.</span></span>

### <a name="scpruntime"></a><span data-ttu-id="eaf4d-219">SCPRuntime</span><span class="sxs-lookup"><span data-stu-id="eaf4d-219">SCPRuntime</span></span>
<span data-ttu-id="eaf4d-220">SCPRuntime poskytuje hello následující dvě metody.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-220">SCPRuntime provides hello following two methods.</span></span>

    public static void Initialize();

    public static void LaunchPlugin(newSCPPlugin createDelegate);  

<span data-ttu-id="eaf4d-221">`Initialize()`je použité tooinitialize hello spojovací bod služby modulu runtime prostředí.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-221">`Initialize()` is used tooinitialize hello SCP runtime environment.</span></span> <span data-ttu-id="eaf4d-222">Tato metoda hello C\# proces připojí straně toohello Java a získá konfigurace parametrů a kontextu topologie.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-222">In this method, hello C\# process will connect toohello Java side, and gets configuration parameters and topology context.</span></span>

<span data-ttu-id="eaf4d-223">`LaunchPlugin()`použít tookick vypnout uvítací zprávu zpracovává smyčky.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-223">`LaunchPlugin()` is used tookick off hello message processing loop.</span></span> <span data-ttu-id="eaf4d-224">V této smyčky hello C\# modulu plug-in se zobrazí formulář zprávy Java straně (včetně řazených kolekcí členů a řízení signály) a pak zadejte zprávy hello procesu, například voláním metody rozhraní hello pomocí hello uživatelského kódu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-224">In this loop, hello C\# plugin will receive messages form Java side (including tuples and control signals), and then process hello messages, perhaps calling hello interface method provide by hello user code.</span></span> <span data-ttu-id="eaf4d-225">Hello vstupního parametru pro metodu `LaunchPlugin()` je delegáta, který může vrátit objekt, který ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt rozhraní implementovat.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-225">hello input parameter for method `LaunchPlugin()` is a delegate that can return an object that implement ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt interface.</span></span>

    public delegate ISCPPlugin newSCPPlugin(Context ctx, Dictionary\<string, Object\> parms); 

<span data-ttu-id="eaf4d-226">Pro ISCPBatchBolt, nám získat `StormTxAttempt` z `parms`a použít ho toojudge, jestli je přehraná pokus.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-226">For ISCPBatchBolt, we can get `StormTxAttempt` from `parms`, and use it toojudge whether it is a replayed attempt.</span></span> <span data-ttu-id="eaf4d-227">To se obvykle provádí na hello potvrzení bolt a je znázorněn v hello `HelloWorldTx` příklad.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-227">This is usually done at hello commit bolt, and it is demonstrated in hello `HelloWorldTx` example.</span></span>

<span data-ttu-id="eaf4d-228">Obecně řečeno hello modulů plug-in spojovací bod služby může spustit ve dvou režimech tady:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-228">Generally speaking, hello SCP plugins may run in two modes here:</span></span>

1. <span data-ttu-id="eaf4d-229">Místní testovací režim: V tomto režimu hello modulů plug-in spojovací bod služby (hello C\# uživatelského kódu) v sadě Visual Studio spustit v průběhu fáze vývoje hello.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-229">Local Test Mode: In this mode, hello SCP plugins (hello C\# user code) run inside Visual Studio during hello development phase.</span></span> <span data-ttu-id="eaf4d-230">`LocalContext`můžete použít v tomto režimu, který poskytuje metoda tooserialize hello vygenerované řazených kolekcí členů toolocal soubory a číst jejich zpět toomemory.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-230">`LocalContext` can be used in this mode, which provides method tooserialize hello emitted tuples toolocal files, and read them back toomemory.</span></span>
   
        public interface ILocalContext
        {
            List\<SCPTuple\> RecvFromMsgQueue();
            void WriteMsgQueueToFile(string filepath, bool append = false);  
            void ReadFromFileToMsgQueue(string filepath);                    
        }
2. <span data-ttu-id="eaf4d-231">Regulární režim: V tomto režimu, moduly plug-in spojovací bod služby hello spustily procesem storm java.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-231">Regular Mode: In this mode, hello SCP plugins are launched by storm java process.</span></span>
   
    <span data-ttu-id="eaf4d-232">Tady je příklad spuštění modulu plug-in spojovací bod služby:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-232">Here is an example of launching SCP plugin:</span></span>
   
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
            /* Setting hello environment variable here can change hello log file name */
            System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "HelloWorld");
   
            SCPRuntime.Initialize();
            SCPRuntime.LaunchPlugin(new newSCPPlugin(Generator.Get));
            }
        }
        }

## <a name="topology-specification-language"></a><span data-ttu-id="eaf4d-233">Topologie specifikace jazyka</span><span class="sxs-lookup"><span data-stu-id="eaf4d-233">Topology Specification Language</span></span>
<span data-ttu-id="eaf4d-234">Určení topologie spojovací bod služby je domény konkrétní jazyk pro popis a konfiguraci topologie spojovací bod služby.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-234">SCP Topology Specification is a domain specific language for describing and configuring SCP topologies.</span></span> <span data-ttu-id="eaf4d-235">Je založena na Storm na Clojure DSL (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) a je prodloužena spojovací bod služby.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-235">It is based on Storm’s Clojure DSL (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) and is extended by SCP.</span></span>

<span data-ttu-id="eaf4d-236">Specifikace topologie jde odeslat přímo toostorm clusteru pro provedení prostřednictvím hello ***runspec*** příkaz.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-236">Topology specifications can be submitted directly toostorm cluster for execution via hello ***runspec*** command.</span></span>

<span data-ttu-id="eaf4d-237">SCP.NET má přidejte postupujte podle funkce toodefine hello transakční topologie:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-237">SCP.NET has add follow functions toodefine hello Transactional Topology:</span></span>

| <span data-ttu-id="eaf4d-238">**Nové funkce**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-238">**New Functions**</span></span> | <span data-ttu-id="eaf4d-239">**Parametry**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-239">**Parameters**</span></span> | <span data-ttu-id="eaf4d-240">**Popis**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-240">**Description**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="eaf4d-241">**TX topolopy**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-241">**tx-topolopy**</span></span> |<span data-ttu-id="eaf4d-242">název topologie</span><span class="sxs-lookup"><span data-stu-id="eaf4d-242">topology-name</span></span><br /><span data-ttu-id="eaf4d-243">spout mapy</span><span class="sxs-lookup"><span data-stu-id="eaf4d-243">spout-map</span></span><br /><span data-ttu-id="eaf4d-244">bolt mapy</span><span class="sxs-lookup"><span data-stu-id="eaf4d-244">bolt-map</span></span> |<span data-ttu-id="eaf4d-245">Definovat topologii transakcí s názvem topologie hello &nbsp;spouts definice mapy a hello funkce bolts definice mapy</span><span class="sxs-lookup"><span data-stu-id="eaf4d-245">Define a transactional topology with hello topology name, &nbsp;spouts definition map and hello bolts definition map</span></span> |
| <span data-ttu-id="eaf4d-246">**spojovací bod služby. tx spout**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-246">**scp-tx-spout**</span></span> |<span data-ttu-id="eaf4d-247">Exec – název</span><span class="sxs-lookup"><span data-stu-id="eaf4d-247">exec-name</span></span><br /><span data-ttu-id="eaf4d-248">argumentů</span><span class="sxs-lookup"><span data-stu-id="eaf4d-248">args</span></span><br /><span data-ttu-id="eaf4d-249">Pole</span><span class="sxs-lookup"><span data-stu-id="eaf4d-249">fields</span></span> |<span data-ttu-id="eaf4d-250">Definujte transakční funkcí spout.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-250">Define a transactional spout.</span></span> <span data-ttu-id="eaf4d-251">Spustí aplikace hello s ***exec název*** pomocí ***argumentů***.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-251">It will run hello application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="eaf4d-252">Hello ***pole*** je pole výstup hello spout</span><span class="sxs-lookup"><span data-stu-id="eaf4d-252">hello ***fields*** is hello Output Fields for spout</span></span> |
| <span data-ttu-id="eaf4d-253">**spojovací bod služby tx-batch-bolt**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-253">**scp-tx-batch-bolt**</span></span> |<span data-ttu-id="eaf4d-254">Exec – název</span><span class="sxs-lookup"><span data-stu-id="eaf4d-254">exec-name</span></span><br /><span data-ttu-id="eaf4d-255">argumentů</span><span class="sxs-lookup"><span data-stu-id="eaf4d-255">args</span></span><br /><span data-ttu-id="eaf4d-256">Pole</span><span class="sxs-lookup"><span data-stu-id="eaf4d-256">fields</span></span> |<span data-ttu-id="eaf4d-257">Definujte transakční Bolt dávky.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-257">Define a transactional Batch Bolt.</span></span> <span data-ttu-id="eaf4d-258">Spustí aplikace hello s ***exec název*** pomocí ***argumentů.***</span><span class="sxs-lookup"><span data-stu-id="eaf4d-258">It will run hello application with ***exec-name*** using ***args.***</span></span><br /><br /><span data-ttu-id="eaf4d-259">Hello pole je hello výstup polí pro funkcí bolt.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-259">hello Fields is hello Output Fields for bolt.</span></span> |
| <span data-ttu-id="eaf4d-260">**spojovací bod služby tx potvrzení bolt**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-260">**scp-tx-commit-bolt**</span></span> |<span data-ttu-id="eaf4d-261">Exec – název</span><span class="sxs-lookup"><span data-stu-id="eaf4d-261">exec-name</span></span><br /><span data-ttu-id="eaf4d-262">argumentů</span><span class="sxs-lookup"><span data-stu-id="eaf4d-262">args</span></span><br /><span data-ttu-id="eaf4d-263">Pole</span><span class="sxs-lookup"><span data-stu-id="eaf4d-263">fields</span></span> |<span data-ttu-id="eaf4d-264">Definujte transakční Committer Bolt.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-264">Define a transactional Committer Bolt.</span></span> <span data-ttu-id="eaf4d-265">Spustí aplikace hello s ***exec název*** pomocí ***argumentů***.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-265">It will run hello application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="eaf4d-266">Hello ***pole*** je pole výstup hello bolt</span><span class="sxs-lookup"><span data-stu-id="eaf4d-266">hello ***fields*** is hello Output Fields for bolt</span></span> |
| <span data-ttu-id="eaf4d-267">**nontx topolopy**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-267">**nontx-topolopy**</span></span> |<span data-ttu-id="eaf4d-268">název topologie</span><span class="sxs-lookup"><span data-stu-id="eaf4d-268">topology-name</span></span><br /><span data-ttu-id="eaf4d-269">spout mapy</span><span class="sxs-lookup"><span data-stu-id="eaf4d-269">spout-map</span></span><br /><span data-ttu-id="eaf4d-270">bolt mapy</span><span class="sxs-lookup"><span data-stu-id="eaf4d-270">bolt-map</span></span> |<span data-ttu-id="eaf4d-271">Definovat topologii netransakční s názvem topologie hello&nbsp; spouts definice mapy a hello funkce bolts definice mapy</span><span class="sxs-lookup"><span data-stu-id="eaf4d-271">Define a nontransactional topology with hello topology name,&nbsp; spouts definition map and hello bolts definition map</span></span> |
| <span data-ttu-id="eaf4d-272">**spout spojovací bod služby**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-272">**scp-spout**</span></span> |<span data-ttu-id="eaf4d-273">Exec – název</span><span class="sxs-lookup"><span data-stu-id="eaf4d-273">exec-name</span></span><br /><span data-ttu-id="eaf4d-274">argumentů</span><span class="sxs-lookup"><span data-stu-id="eaf4d-274">args</span></span><br /><span data-ttu-id="eaf4d-275">Pole</span><span class="sxs-lookup"><span data-stu-id="eaf4d-275">fields</span></span><br /><span data-ttu-id="eaf4d-276">parameters</span><span class="sxs-lookup"><span data-stu-id="eaf4d-276">parameters</span></span> |<span data-ttu-id="eaf4d-277">Definujte netransakční spout.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-277">Define a nontransactional spout.</span></span> <span data-ttu-id="eaf4d-278">Spustí aplikace hello s ***exec název*** pomocí ***argumentů***.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-278">It will run hello application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="eaf4d-279">Hello ***pole*** je pole výstup hello spout</span><span class="sxs-lookup"><span data-stu-id="eaf4d-279">hello ***fields*** is hello Output Fields for spout</span></span><br /><br /><span data-ttu-id="eaf4d-280">Hello ***parametry*** je volitelná, použití toospecify některé parametry, jako je například "nontransactional.ack.enabled".</span><span class="sxs-lookup"><span data-stu-id="eaf4d-280">hello ***parameters*** is optional, using it toospecify some parameters such as "nontransactional.ack.enabled".</span></span> |
| <span data-ttu-id="eaf4d-281">**bolt spojovací bod služby**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-281">**scp-bolt**</span></span> |<span data-ttu-id="eaf4d-282">Exec – název</span><span class="sxs-lookup"><span data-stu-id="eaf4d-282">exec-name</span></span><br /><span data-ttu-id="eaf4d-283">argumentů</span><span class="sxs-lookup"><span data-stu-id="eaf4d-283">args</span></span><br /><span data-ttu-id="eaf4d-284">Pole</span><span class="sxs-lookup"><span data-stu-id="eaf4d-284">fields</span></span><br /><span data-ttu-id="eaf4d-285">parameters</span><span class="sxs-lookup"><span data-stu-id="eaf4d-285">parameters</span></span> |<span data-ttu-id="eaf4d-286">Definujte netransakční funkcí Bolt.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-286">Define a nontransactional Bolt.</span></span> <span data-ttu-id="eaf4d-287">Spustí aplikace hello s ***exec název*** pomocí ***argumentů***.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-287">It will run hello application with ***exec-name*** using ***args***.</span></span><br /><br /><span data-ttu-id="eaf4d-288">Hello ***pole*** je pole výstup hello bolt</span><span class="sxs-lookup"><span data-stu-id="eaf4d-288">hello ***fields*** is hello Output Fields for bolt</span></span><br /><br /><span data-ttu-id="eaf4d-289">Hello ***parametry*** je volitelná, použití toospecify některé parametry, jako je například "nontransactional.ack.enabled".</span><span class="sxs-lookup"><span data-stu-id="eaf4d-289">hello ***parameters*** is optional, using it toospecify some parameters such as "nontransactional.ack.enabled".</span></span> |

<span data-ttu-id="eaf4d-290">SCP.NET má postupujte podle klíče slova definované:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-290">SCP.NET has follow keys words defined:</span></span>

| <span data-ttu-id="eaf4d-291">**Klíčová slova**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-291">**Key Words**</span></span> | <span data-ttu-id="eaf4d-292">**Popis**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-292">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="eaf4d-293">**: název**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-293">**:name**</span></span> |<span data-ttu-id="eaf4d-294">Definování hello název topologie</span><span class="sxs-lookup"><span data-stu-id="eaf4d-294">Define hello Topology Name</span></span> |
| <span data-ttu-id="eaf4d-295">**: topologie**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-295">**:topology**</span></span> |<span data-ttu-id="eaf4d-296">Definování hello topologie pomocí hello výše funkce a sestavení v těch, které jsou.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-296">Define hello Topology using hello above functions and build in ones.</span></span> |
| <span data-ttu-id="eaf4d-297">**: p**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-297">**:p**</span></span> |<span data-ttu-id="eaf4d-298">Definujte hello paralelismus nápovědu pro každou funkcí spout nebo bolt.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-298">Define hello parallelism hint for each spout or bolt.</span></span> |
| <span data-ttu-id="eaf4d-299">**: Konfigurace**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-299">**:config**</span></span> |<span data-ttu-id="eaf4d-300">Definování konfigurace parametru nebo aktualizace hello existující</span><span class="sxs-lookup"><span data-stu-id="eaf4d-300">Define configure parameter or update hello existing ones</span></span> |
| <span data-ttu-id="eaf4d-301">**: schéma**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-301">**:schema**</span></span> |<span data-ttu-id="eaf4d-302">Definujte hello schématu datového proudu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-302">Define hello Schema of Stream.</span></span> |

<span data-ttu-id="eaf4d-303">A často používané parametry:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-303">And frequently-used parameters:</span></span>

| <span data-ttu-id="eaf4d-304">**Parametr**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-304">**Parameter**</span></span> | <span data-ttu-id="eaf4d-305">**Popis**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-305">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="eaf4d-306">**"plugin.name"**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-306">**"plugin.name"**</span></span> |<span data-ttu-id="eaf4d-307">Název souboru EXE modulu plug-in hello C#</span><span class="sxs-lookup"><span data-stu-id="eaf4d-307">exe file name of hello C# plugin</span></span> |
| <span data-ttu-id="eaf4d-308">**"plugin.args"**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-308">**"plugin.args"**</span></span> |<span data-ttu-id="eaf4d-309">argumentů modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="eaf4d-309">plugin args</span></span> |
| <span data-ttu-id="eaf4d-310">**"output.schema"**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-310">**"output.schema"**</span></span> |<span data-ttu-id="eaf4d-311">Schéma výstupu</span><span class="sxs-lookup"><span data-stu-id="eaf4d-311">Output schema</span></span> |
| <span data-ttu-id="eaf4d-312">**"nontransactional.ack.enabled"**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-312">**"nontransactional.ack.enabled"**</span></span> |<span data-ttu-id="eaf4d-313">Určuje, jestli je povolená ack pro netransakční topologie</span><span class="sxs-lookup"><span data-stu-id="eaf4d-313">Whether ack is enabled for nontransactional topology</span></span> |

<span data-ttu-id="eaf4d-314">příkaz runspec Hello nasadí společně s hello bits, využití hello je jako:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-314">hello runspec command will be deployed together with hello bits, hello usage is like:</span></span>

    .\bin\runSpec.cmd
    usage: runSpec [spec-file target-dir [resource-dir] [-cp classpath]]
    ex: runSpec examples\HelloWorld\HelloWorld.spec specs examples\HelloWorld\Target

<span data-ttu-id="eaf4d-315">Hello ***prostředků dir*** parametr je volitelný, je třeba toospecify ho, když chcete, aby tooplug a C\# aplikace a tento adresář bude obsahovat aplikaci hello hello závislosti a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-315">hello ***resource-dir*** parameter is optional, you need toospecify it when you want tooplug a C\# application, and this directory will contain hello application, hello dependencies and configurations.</span></span>

<span data-ttu-id="eaf4d-316">Hello ***cesty pro třídy*** parametr je nepovinný.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-316">hello ***classpath*** parameter is also optional.</span></span> <span data-ttu-id="eaf4d-317">Pokud soubor specifikace hello obsahuje Java Spout nebo Bolt je použité toospecify hello Java cesty pro třídy.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-317">It is used toospecify hello Java classpath if hello spec file contains Java Spout or Bolt.</span></span>

## <a name="miscellaneous-features"></a><span data-ttu-id="eaf4d-318">Různé funkce</span><span class="sxs-lookup"><span data-stu-id="eaf4d-318">Miscellaneous Features</span></span>
### <a name="input-and-output-schema-declaration"></a><span data-ttu-id="eaf4d-319">Vstup a výstup schématu deklarace</span><span class="sxs-lookup"><span data-stu-id="eaf4d-319">Input and Output Schema Declaration</span></span>
<span data-ttu-id="eaf4d-320">Hello uživatele můžete emitování řazené kolekce členů v jazyce C\# zpracování, hello platformy musí tooserialize hello řazené kolekce členů do byte [] straně tooJava přenos a Storm přenese tento cíle toohello řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-320">hello user can emit tuple in C\# process, hello platform needs tooserialize hello tuple into byte[], transfer tooJava side, and Storm will transfer this tuple toohello targets.</span></span> <span data-ttu-id="eaf4d-321">Mezitím v podřízené součásti hello C\# proces bude přijímat řazené kolekce členů zpět z straně java a převádět je původní typy toohello podle platformy, všechny tyto operace jsou skryt hello platformy.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-321">Meanwhile in downstream component, hello C\# process will receive tuple back from java side, and convert it toohello original types by platform, all these operations are hidden by hello Platform.</span></span>

<span data-ttu-id="eaf4d-322">toosupport hello serializace a deserializace, uživatelský kód musí toodeclare hello schéma hello vstupy a výstupy.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-322">toosupport hello serialization and deserialization, user code needs toodeclare hello schema of hello inputs and outputs.</span></span>

<span data-ttu-id="eaf4d-323">schéma vstupu a výstupu datového proudu Hello je definován jako slovník, hello klíč je hello StreamId a hello hodnota je hello typy sloupců hello.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-323">hello input/output stream schema is defined as a dictionary, hello key is hello StreamId and hello value is hello Types of hello columns.</span></span> <span data-ttu-id="eaf4d-324">součást Hello může mít víc datových proudů deklarován.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-324">hello component can have multi-streams declared.</span></span>

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


<span data-ttu-id="eaf4d-325">V kontextu objektu máme hello přidat následující rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-325">In Context object, we have hello following API added:</span></span>

    public void DeclareComponentSchema(ComponentStreamSchema schema)

<span data-ttu-id="eaf4d-326">Uživatelský kód musí zajistit vygenerované řazených kolekcí členů hello orientují hello schéma definované pro tento datový proud nebo hello systému vyvolá výjimku modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-326">User code must make sure hello tuples emitted obey hello schema defined for that stream, or hello system will throw a runtime exception.</span></span>

### <a name="multi-stream-support"></a><span data-ttu-id="eaf4d-327">Podpora více datového proudu</span><span class="sxs-lookup"><span data-stu-id="eaf4d-327">Multi-Stream Support</span></span>
<span data-ttu-id="eaf4d-328">Spojovací bod služby podporuje uživatele code tooemit nebo přijímat z několika různých datových proudů v hello stejný čas.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-328">SCP supports user code tooemit or receive from multiple distinct streams at hello same time.</span></span> <span data-ttu-id="eaf4d-329">Podpora Hello odráží v kontextu objektu hello jako hello vysílat metoda přijímá parametr ID volitelné datového proudu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-329">hello support reflects in hello Context object as hello Emit method takes an optional stream ID parameter.</span></span>

<span data-ttu-id="eaf4d-330">Byly přidány dvě metody v hello SCP.NET kontextu objektu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-330">Two methods in hello SCP.NET Context object have been added.</span></span> <span data-ttu-id="eaf4d-331">Jsou použité tooemit řazené kolekce členů nebo řazené kolekce členů toospecify StreamId.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-331">They are used tooemit Tuple or Tuples toospecify StreamId.</span></span> <span data-ttu-id="eaf4d-332">Hello StreamId je řetězec a je nutné toobe konzistentní v obou C\# a hello specifikace definice topologie.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-332">hello StreamId is a string and it needs toobe consistent in both C\# and hello Topology Definition Spec.</span></span>

        /* Emit tuple toohello specific stream. */
        public abstract void Emit(string streamId, List<object> values);

        /* for non-transactional Spout only */
        public abstract void Emit(string streamId, List<object> values, long seqId);

<span data-ttu-id="eaf4d-333">Hello datového proudu neexistující generování tooa způsobí, že výjimky za běhu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-333">hello emitting tooa non-existing stream will cause runtime exceptions.</span></span>

### <a name="fields-grouping"></a><span data-ttu-id="eaf4d-334">Pole seskupení</span><span class="sxs-lookup"><span data-stu-id="eaf4d-334">Fields Grouping</span></span>
<span data-ttu-id="eaf4d-335">Hello sestavení v seskupení pole v Strom nepracuje správně v SCP.NET.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-335">hello build-in Fields Grouping in Strom is not working properly in SCP.NET.</span></span> <span data-ttu-id="eaf4d-336">Na hello straně Java Proxy všechny hello pole datové typy jsou ve skutečnosti byte [] a seskupení pole hello používá hello byte [] objekt hash kód tooperform hello seskupení.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-336">On hello Java Proxy side, all hello fields data types are actually byte[], and hello fields grouping uses hello byte[] object hash code tooperform hello grouping.</span></span> <span data-ttu-id="eaf4d-337">Kód hash pro objekt Hello byte [] je hello adresa tohoto objektu v paměti.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-337">hello byte[] object hash code is hello address of this object in memory.</span></span> <span data-ttu-id="eaf4d-338">Tak bude hello seskupení nesprávný pro dva bajty [] objekty hello této sdílené složky stejný obsah, ale není hello stejnou adresu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-338">So hello grouping will be wrong for two byte[] objects that share hello same content but not hello same address.</span></span>

<span data-ttu-id="eaf4d-339">SCP.NET přidá metoda přizpůsobené seskupení a použije obsah hello hello byte [] toodo hello seskupení.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-339">SCP.NET adds a customized grouping method, and it will use hello content of hello byte[] toodo hello grouping.</span></span> <span data-ttu-id="eaf4d-340">V **specifikace** souboru hello syntaxe je jako:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-340">In **SPEC** file, hello syntax is like:</span></span>

    (bolt-spec
        {
            "spout_test" (scp-field-group :non-tx [0,1])
        }
        …
    )


<span data-ttu-id="eaf4d-341">Tady</span><span class="sxs-lookup"><span data-stu-id="eaf4d-341">Here,</span></span>

1. <span data-ttu-id="eaf4d-342">"spojovací bod služby skupiny polí" znamená "Vlastní pole seskupení implementované spojovací bod služby".</span><span class="sxs-lookup"><span data-stu-id="eaf4d-342">"scp-field-group" means "Customized field grouping implemented by SCP".</span></span>
2. <span data-ttu-id="eaf4d-343">": tx"nebo": bez tx" znamená, pokud je transakční topologie.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-343">":tx" or ":non-tx" means if it’s transactional topology.</span></span> <span data-ttu-id="eaf4d-344">Tyto informace potřebujeme od hello od indexu se liší v tx oproti bez tx topologie.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-344">We need this information since hello starting index is different in tx vs. non-tx topologies.</span></span>
3. <span data-ttu-id="eaf4d-345">[0,1] znamená hashset ID pole, počínaje od 0.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-345">[0,1] means a hashset of field Ids, starting from 0.</span></span>

### <a name="hybrid-topology"></a><span data-ttu-id="eaf4d-346">Hybridní topologie</span><span class="sxs-lookup"><span data-stu-id="eaf4d-346">Hybrid topology</span></span>
<span data-ttu-id="eaf4d-347">nativní Hello Storm je napsán v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-347">hello native Storm is written in Java.</span></span> <span data-ttu-id="eaf4d-348">A SCP.Net má rozšířené tooenable naše celní toowrite C\# code toohandle své obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-348">And SCP.Net has enhanced it tooenable our customs toowrite C\# code toohandle their business logic.</span></span> <span data-ttu-id="eaf4d-349">Také podporujeme hybridní topologie, která obsahuje nejen C, ale\# funkcích spouts nebo funkce bolts, ale také Java funkcí Spout/funkce Bolts.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-349">But we also support hybrid topologies, which contains not only C\# spouts/bolts, but also Java Spout/Bolts.</span></span>

### <a name="specify-java-spoutbolt-in-spec-file"></a><span data-ttu-id="eaf4d-350">Zadat Java funkcí Spout/Bolt specifikace souboru</span><span class="sxs-lookup"><span data-stu-id="eaf4d-350">Specify Java Spout/Bolt in spec file</span></span>
<span data-ttu-id="eaf4d-351">V souboru specifikace "spojovací bod služby funkcí spout" a "spojovací bod služby funkcí bolt" může být také použít toospecify Spouts Java a funkce Bolts, tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-351">In spec file, "scp-spout" and "scp-bolt" can also be used toospecify Java Spouts and Bolts, here is an example:</span></span>

    (spout-spec 
      (microsoft.scp.example.HybridTopology.Generator.)           
      :p 1)

<span data-ttu-id="eaf4d-352">Zde `microsoft.scp.example.HybridTopology.Generator` je název hello hello Java Spout třídy.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-352">Here `microsoft.scp.example.HybridTopology.Generator` is hello name of hello Java Spout class.</span></span>

### <a name="specify-java-classpath-in-runspec-command"></a><span data-ttu-id="eaf4d-353">Zadejte cestu pro třídy Java v runSpec příkaz</span><span class="sxs-lookup"><span data-stu-id="eaf4d-353">Specify Java Classpath in runSpec Command</span></span>
<span data-ttu-id="eaf4d-354">Pokud chcete toosubmit topologie obsahující Java Spouts nebo funkce Bolts, potřebovat hello kompilace toofirst Java Spouts nebo funkce Bolts a získání souborů Jar hello.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-354">If you want toosubmit topology containing Java Spouts or Bolts, you need toofirst compile hello Java Spouts or Bolts and get hello Jar files.</span></span> <span data-ttu-id="eaf4d-355">Pak musíte zadat cestě třídy hello java, která obsahuje soubory Jar hello při odesílání topologie.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-355">Then you should specify hello java classpath that contains hello Jar files when submitting topology.</span></span> <span data-ttu-id="eaf4d-356">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-356">Here is an example:</span></span>

    bin\runSpec.cmd examples\HybridTopology\HybridTopology.spec specs examples\HybridTopology\net\Target -cp examples\HybridTopology\java\target\*

<span data-ttu-id="eaf4d-357">Zde **příklady\\HybridTopology\\java\\cíl\\**  je hello složku obsahující soubor Jar funkcí Spout/Bolt Java hello.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-357">Here **examples\\HybridTopology\\java\\target\\** is hello folder containing hello Java Spout/Bolt Jar file.</span></span>

### <a name="serialization-and-deserialization-between-java-and-c"></a><span data-ttu-id="eaf4d-358">Serializace a deserializace mezi Java a C\\</span><span class="sxs-lookup"><span data-stu-id="eaf4d-358">Serialization and Deserialization between Java and C\\</span></span>
<span data-ttu-id="eaf4d-359">Součásti naší spojovací bod služby zahrnuje straně Java a C\# straně.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-359">Our SCP component includes Java side and C\# side.</span></span> <span data-ttu-id="eaf4d-360">V pořadí toointeract s nativní Java funkcích Spouts nebo funkce Bolts, musí být provedena serializaci nebo deserializaci mezi straně Java a C\# straně, jak je znázorněno v následujícím grafu hello.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-360">In order toointeract with native Java Spouts/Bolts, Serialization/Deserialization must be carried out between Java side and C\# side, as illustrated in hello following graph.</span></span>

![Diagram součásti java odesílání tooSCP součást odesílání tooJava součásti](media/hdinsight-storm-scp-programming-guide/java-compent-sending-to-scp-component-sending-to-java-component.png)

1. <span data-ttu-id="eaf4d-362">**Serializace v jazyce Java straně a deserializace v jazyce C\# straně**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-362">**Serialization in Java side and Deserialization in C\# side**</span></span>
   
   <span data-ttu-id="eaf4d-363">Nejprve poskytujeme výchozí implementace v jazyce Java straně serializace a deserializace v jazyce C\# straně.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-363">First we provide default implementation for serialization in Java side and deserialization in C\# side.</span></span> <span data-ttu-id="eaf4d-364">Metoda serializace Hello na straně Java lze zadat v souboru specifikace:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-364">hello serialization method in Java side can be specified in SPEC file:</span></span>
   
       (scp-bolt
           {
               "plugin.name" "HybridTopology.exe"
               "plugin.args" ["displayer"]
               "output.schema" {}
               "customized.java.serializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer"]
           })
   
   <span data-ttu-id="eaf4d-365">Hello deserializace metody v jazyce C\# straně musí být zadán v jazyce C\# uživatelského kódu:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-365">hello deserialization method in C\# side should be specified in C\# user code:</span></span>
   
       Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
       inputSchema.Add("default", new List<Type>() { typeof(Person) });
       this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
       this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());            
   
   <span data-ttu-id="eaf4d-366">Tato výchozí implementace by měla řídit většinou, pokud hello datový typ není příliš složitý.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-366">This default implementation should handle most cases if hello data type is not too complex.</span></span> <span data-ttu-id="eaf4d-367">Pro určité případy buď protože hello uživatelský datový typ je příliš složité, nebo protože hello výkon naše výchozí implementace nesplňuje hello požadavek uživatele, uživatel může modul plug-in vlastní implementaci.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-367">For certain cases, either because hello user data type is too complex, or because hello performance of our default implementation does not meet hello user's requirement, user can plug-in their own implementation.</span></span>
   
   <span data-ttu-id="eaf4d-368">Hello serializovat rozhraní v jazyce java straně je definován jako:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-368">hello serialize interface in java side is defined as:</span></span>
   
       public interface ICustomizedInteropJavaSerializer {
           public void prepare(String[] args);
           public List<ByteBuffer> serialize(List<Object> objectList);
       }
   
   <span data-ttu-id="eaf4d-369">Hello deserializovat rozhraní v jazyce C\# straně je definován jako:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-369">hello deserialize interface in C\# side is defined as:</span></span>
   
   <span data-ttu-id="eaf4d-370">veřejné rozhraní ICustomizedInteropCSharpDeserializer</span><span class="sxs-lookup"><span data-stu-id="eaf4d-370">public interface ICustomizedInteropCSharpDeserializer</span></span>
   
       public interface ICustomizedInteropCSharpDeserializer
       {
           List<Object> Deserialize(List<byte[]> dataList, List<Type> targetTypes);
       }
2. <span data-ttu-id="eaf4d-371">**Serializace v jazyce C\# straně a deserializace v jazyce Java souběžný**</span><span class="sxs-lookup"><span data-stu-id="eaf4d-371">**Serialization in C\# side and Deserialization in Java side side**</span></span>
   
   <span data-ttu-id="eaf4d-372">Hello metoda serializace v jazyce C\# straně musí být zadán v jazyce C\# uživatelského kódu:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-372">hello serialization method in C\# side should be specified in C\# user code:</span></span>
   
       this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer()); 
   
   <span data-ttu-id="eaf4d-373">Hello deserializace metody na straně Java musí být zadán v souboru specifikace:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-373">hello Deserialization method in Java side should be specified in SPEC file:</span></span>
   
     <span data-ttu-id="eaf4d-374">(spojovací bod služby spout</span><span class="sxs-lookup"><span data-stu-id="eaf4d-374">(scp-spout</span></span>
   
       {
         "plugin.name" "HybridTopology.exe"
         "plugin.args" ["generator"]
         "output.schema" {"default" ["person"]}
         "customized.java.deserializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" "microsoft.scp.example.HybridTopology.Person"]
       })
   
   <span data-ttu-id="eaf4d-375">Následuje název hello deserializátor "microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" a "microsoft.scp.example.HybridTopology.Person" je, že je k deserializaci hello cílová třída hello data.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-375">Here "microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" is hello name of Deserializer, and "microsoft.scp.example.HybridTopology.Person" is hello target class hello data is deserialized to.</span></span>
   
   <span data-ttu-id="eaf4d-376">Uživatel může také modulu plug-in vlastní implementace C\# serializátor a deserializátor Java.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-376">User can also plug-in their own implementation of C\# serializer and Java Deserializer.</span></span> <span data-ttu-id="eaf4d-377">Toto je hello rozhraní pro C\# serializátor:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-377">This is hello interface for C\# serializer:</span></span>
   
       public interface ICustomizedInteropCSharpSerializer
       {
           List<byte[]> Serialize(List<object> dataList);
       }
   
   <span data-ttu-id="eaf4d-378">Toto je hello rozhraní pro deserializátor Java:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-378">This is hello interface for Java Deserializer:</span></span>
   
       public interface ICustomizedInteropJavaDeserializer {
           public void prepare(String[] targetClassNames);
           public List<Object> Deserialize(List<ByteBuffer> dataList);
       }

## <a name="scp-host-mode"></a><span data-ttu-id="eaf4d-379">Spojovací bod služby hostitele režimu</span><span class="sxs-lookup"><span data-stu-id="eaf4d-379">SCP Host Mode</span></span>
<span data-ttu-id="eaf4d-380">V tomto režimu můžete uživatele jejich kódy tooDLL zkompilovat a použít SCPHost.exe poskytované topologie toosubmit spojovací bod služby.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-380">In this mode, user can compile their codes tooDLL, and use SCPHost.exe provided by SCP toosubmit topology.</span></span> <span data-ttu-id="eaf4d-381">Specifikace souboru Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-381">hello spec file looks like this:</span></span>

    (scp-spout
      {
        "plugin.name" "SCPHost.exe"
        "plugin.args" ["HelloWorld.dll" "Scp.App.HelloWorld.Generator" "Get"]
        "output.schema" {"default" ["sentence"]}
      })

<span data-ttu-id="eaf4d-382">Zde `plugin.name` je zadán jako `SCPHost.exe` poskytované spojovací bod služby SDK.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-382">Here, `plugin.name` is specified as `SCPHost.exe` provided by SCP SDK.</span></span> <span data-ttu-id="eaf4d-383">SCPHost.exe které přijímá přesně tři parametry:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-383">SCPHost.exe which accepts exactly three parameters:</span></span>

1. <span data-ttu-id="eaf4d-384">Hello nejprve jeden je hello DLL název, který je `"HelloWorld.dll"` v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-384">hello first one is hello DLL name, which is `"HelloWorld.dll"` in this example.</span></span>
2. <span data-ttu-id="eaf4d-385">Hello druhá je hello názvu třídy, což je `"Scp.App.HelloWorld.Generator"` v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-385">hello second one is hello Class name, which is `"Scp.App.HelloWorld.Generator"` in this example.</span></span>
3. <span data-ttu-id="eaf4d-386">Hello třetí jeden je hello název veřejné statické metody, které může být vyvolaná tooget instanci ISCPPlugin.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-386">hello third one is hello name of a public static method, which can be invoked tooget an instance of ISCPPlugin.</span></span>

<span data-ttu-id="eaf4d-387">V režimu hostitele uživatelský kód kompiluje jako knihovny DLL a je vyvolána platformou spojovací bod služby.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-387">In host mode, user code is compiled as DLL, and is invoked by SCP platform.</span></span> <span data-ttu-id="eaf4d-388">Spojovací bod služby platformy, můžete získat úplné řízení pro logiku celou zpracování hello.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-388">So SCP platform can get full control of hello whole processing logic.</span></span> <span data-ttu-id="eaf4d-389">Proto doporučujeme naše zákazníky toosubmit topologii spojovací bod služby hostitele režimu vzhledem k tomu, že ho můžete zjednodušit hello vývojového prostředí a přineste nám větší flexibilitu a zpětnou kompatibilitu pro také novější verzi.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-389">So we recommend our customers toosubmit topology in SCP host mode since it can simplify hello development experience and bring us more flexibility and better backward compatibility for later release as well.</span></span>

## <a name="scp-programming-examples"></a><span data-ttu-id="eaf4d-390">Příklady programování spojovací bod služby</span><span class="sxs-lookup"><span data-stu-id="eaf4d-390">SCP Programming Examples</span></span>
### <a name="helloworld"></a><span data-ttu-id="eaf4d-391">Hello World</span><span class="sxs-lookup"><span data-stu-id="eaf4d-391">HelloWorld</span></span>
<span data-ttu-id="eaf4d-392">**Hello World** je velmi jednoduchý příklad tooshow chuť SCP.Net.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-392">**HelloWorld** is a very simple example tooshow a taste of SCP.Net.</span></span> <span data-ttu-id="eaf4d-393">Používá topologii netransakční s spout, nazývá **generátor**a dvě funkce bolts názvem **rozdělovače** a **čítač**.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-393">It uses a non-transactional topology, with a spout called **generator**, and two bolts called **splitter** and **counter**.</span></span> <span data-ttu-id="eaf4d-394">Hello spout **generátor** se náhodně generovat některé věty a příliš emitování tyto věty**rozdělovače**.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-394">hello spout **generator** will randomly generate some sentences, and emit these sentences too**splitter**.</span></span> <span data-ttu-id="eaf4d-395">Hello bolt **rozdělovače** bude rozdělení toowords věty hello a emitování tato slova příliš**čítač** funkcí bolt.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-395">hello bolt **splitter** will split hello sentences toowords and emit these words too**counter** bolt.</span></span> <span data-ttu-id="eaf4d-396">Čítač"Hello bolt" používá slovník toorecord hello výskyt počet jednotlivých slov.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-396">hello bolt "counter" uses a dictionary toorecord hello occurrence number of each word.</span></span>

<span data-ttu-id="eaf4d-397">Existují dva soubory specifikace, **HelloWorld.spec** a **HelloWorld\_EnableAck.spec** v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-397">There are two spec files, **HelloWorld.spec** and **HelloWorld\_EnableAck.spec** for this example.</span></span> <span data-ttu-id="eaf4d-398">V hello C\# kódu, ho můžete zjistit, zda je povoleno potvrzení získáním hello pluginConf ze strany Java.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-398">In hello C\# code, it can find out whether ack is enabled by getting hello pluginConf from Java side.</span></span>

    /* demo how tooget pluginConf info */
    if (Context.Config.pluginConf.ContainsKey(Constants.NONTRANSACTIONAL_ENABLE_ACK))
    {
        enableAck = (bool)(Context.Config.pluginConf[Constants.NONTRANSACTIONAL_ENABLE_ACK]);
    }
    Context.Logger.Info("enableAck: {0}", enableAck);

<span data-ttu-id="eaf4d-399">Ve funkcích spout hello Pokud je povoleno potvrzení, slovník je použité toocache hello záznamů, které nebyly acked.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-399">In hello spout, if ack is enabled, a dictionary is used toocache hello tuples that have not been acked.</span></span> <span data-ttu-id="eaf4d-400">Pokud je volána Fail(), hello selhání řazené kolekce členů budou přehrány:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-400">If Fail() is called, hello failed tuple will be replayed:</span></span>

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        Context.Logger.Info("Fail, seqId: {0}", seqId);
        if (cachedTuples.ContainsKey(seqId))
        {
            /* get hello cached tuple */
            string sentence = cachedTuples[seqId];

            /* replay hello failed tuple */
            Context.Logger.Info("Re-Emit: {0}, seqId: {1}", sentence, seqId);
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), seqId);
        }
        else
        {
            Context.Logger.Warn("Fail(), can't find cached tuple for seqId {0}!", seqId);
        }
    }

### <a name="helloworldtx"></a><span data-ttu-id="eaf4d-401">HelloWorldTx</span><span class="sxs-lookup"><span data-stu-id="eaf4d-401">HelloWorldTx</span></span>
<span data-ttu-id="eaf4d-402">Hello **HelloWorldTx** příklad ukazuje, jak tooimplement transakční topologie.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-402">hello **HelloWorldTx** example demonstrates how tooimplement transactional topology.</span></span> <span data-ttu-id="eaf4d-403">Neměl mít jeden spout názvem **generátor**, funkce bolts batch názvem **partial počet**, potvrzení bolt s názvem **počet součet**.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-403">It have one spout called **generator**, a batch bolts called **partial-count**, and a commit bolt called **count-sum**.</span></span> <span data-ttu-id="eaf4d-404">Existují také tři soubory txt předem vytvořené: **DataSource0.txt**, **DataSource1.txt** a **DataSource2.txt**.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-404">There are also three pre-created txt files: **DataSource0.txt**, **DataSource1.txt** and **DataSource2.txt**.</span></span>

<span data-ttu-id="eaf4d-405">V každou transakci, hello spout **generátor** náhodně zvolím dva soubory z hello předem vytvořené tři soubory a emitování hello dva souboru názvy toohello **partial počet** funkcí bolt.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-405">In each transaction, hello spout **generator** will randomly choose two files from hello pre-created three files, and emit hello two file names toohello **partial-count** bolt.</span></span> <span data-ttu-id="eaf4d-406">Hello bolt **partial počet** nejdřív získat název souboru hello z hello přijatých řazené kolekce členů a pak otevřete hello souboru a počet hello počtu slov v tomto souboru a nakonec emitování hello word číslo toohello **počet – součet**funkcí bolt.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-406">hello bolt **partial-count** will first get hello file name from hello received tuple, then open hello file and count hello number of words in this file, and finally emit hello word number toohello **count-sum** bolt.</span></span> <span data-ttu-id="eaf4d-407">Hello **počet součet** bolt shrnuje celkový počet hello.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-407">hello **count-sum** bolt will summarize hello total count.</span></span>

<span data-ttu-id="eaf4d-408">tooachieve **právě jednou** sémantiku, hello potvrzení bolt **počet součet** potřebovat toojudge toho, jestli je přehraná transakce.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-408">tooachieve **exactly once** semantics, hello commit bolt **count-sum** need toojudge whether it is a replayed transaction.</span></span> <span data-ttu-id="eaf4d-409">V tomto příkladu má proměnná statický člen:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-409">In this example, it has a static member variable:</span></span>

    public static long lastCommittedTxId = -1; 

<span data-ttu-id="eaf4d-410">Když je vytvořena ISCPBatchBolt instance, se budou získávat hello `txAttempt` ze vstupní parametry:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-410">When an ISCPBatchBolt instance is created, it will get hello `txAttempt` from input parameters:</span></span>

    public static CountSum Get(Context ctx, Dictionary<string, Object> parms)
    {
        /* for transactional topology, we can get txAttempt from hello input parms */
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

<span data-ttu-id="eaf4d-411">Když `FinishBatch()` je volána, hello `lastCommittedTxId` bude aktualizace, pokud není přehraná transakce.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-411">When `FinishBatch()` is called, hello `lastCommittedTxId` will be update if it is not a replayed transaction.</span></span>

    public void FinishBatch(Dictionary<string, Object> parms)
    {
        /* judge whether it is a replayed transaction? */
        bool replay = (this.txAttempt.TxId <= lastCommittedTxId);

        if (!replay)
        {
            /* If it is not replayed, update hello toalCount and lastCommittedTxId vaule */
            totalCount = totalCount + this.count;
            lastCommittedTxId = this.txAttempt.TxId;
        }
        … …
    }


### <a name="hybridtopology"></a><span data-ttu-id="eaf4d-412">HybridTopology</span><span class="sxs-lookup"><span data-stu-id="eaf4d-412">HybridTopology</span></span>
<span data-ttu-id="eaf4d-413">Tato topologie obsahuje Java Spout a a C\# funkcí Bolt.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-413">This topology contains a Java Spout and a C\# Bolt.</span></span> <span data-ttu-id="eaf4d-414">Používá hello výchozí serializace a deserializace implementace poskytovaná spojovací bod služby platformy.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-414">It uses hello default serialization and deserialization implementation provided by SCP platform.</span></span> <span data-ttu-id="eaf4d-415">Prosím ref hello **HybridTopology.spec** v **příklady\\HybridTopology** složku podrobnosti hello specifikace souboru, a **SubmitTopology.bat** jak Cesta pro třídy toospecify Java.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-415">Please ref hello **HybridTopology.spec** in **examples\\HybridTopology** folder for hello spec file details, and **SubmitTopology.bat** for how toospecify Java classpath.</span></span>

### <a name="scphostdemo"></a><span data-ttu-id="eaf4d-416">SCPHostDemo</span><span class="sxs-lookup"><span data-stu-id="eaf4d-416">SCPHostDemo</span></span>
<span data-ttu-id="eaf4d-417">V tomto příkladu je hello v podstatě stejné jako Hello World.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-417">This example is hello same as HelloWorld in essence.</span></span> <span data-ttu-id="eaf4d-418">Hello pouze rozdílem je, že hello uživatele kódu jako knihovny DLL a odeslání hello topologie pomocí SCPHost.exe.</span><span class="sxs-lookup"><span data-stu-id="eaf4d-418">hello only difference is that hello user code is compiled as DLL and hello topology is submitted by using SCPHost.exe.</span></span> <span data-ttu-id="eaf4d-419">Podrobnější vysvětlení prosím ref hello část "Spojovací bod služby hostitele režim".</span><span class="sxs-lookup"><span data-stu-id="eaf4d-419">Please ref hello section "SCP Host Mode" for more detailed explanation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eaf4d-420">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eaf4d-420">Next Steps</span></span>
<span data-ttu-id="eaf4d-421">Příklady topologií Storm vytvořený spojovací bod služby najdete v tématu hello následující:</span><span class="sxs-lookup"><span data-stu-id="eaf4d-421">For examples of Storm topologies created using SCP, see hello following:</span></span>

* [<span data-ttu-id="eaf4d-422">Vývoj topologie C# pro Apache Storm v HDInsight pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eaf4d-422">Develop C# topologies for Apache Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [<span data-ttu-id="eaf4d-423">Zpracování událostí z Azure Event Hubs se Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="eaf4d-423">Process events from Azure Event Hubs with Storm on HDInsight</span></span>](hdinsight-storm-develop-csharp-event-hub-topology.md)
* [<span data-ttu-id="eaf4d-424">Vytvoření více datových proudů v topologie C# Storm</span><span class="sxs-lookup"><span data-stu-id="eaf4d-424">Create multiple data streams in a C# Storm topology</span></span>](hdinsight-storm-twitter-trending.md)
* [<span data-ttu-id="eaf4d-425">Použít data toovisualize Power Bi od topologie Storm</span><span class="sxs-lookup"><span data-stu-id="eaf4d-425">Use Power Bi toovisualize data from a Storm topology</span></span>](hdinsight-storm-power-bi-topology.md)
* [<span data-ttu-id="eaf4d-426">Zpracování dat snímačů vehicle ze služby Event Hubs pomocí Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="eaf4d-426">Process vehicle sensor data from Event Hubs using Storm on HDInsight</span></span>](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/IotExample)
* [<span data-ttu-id="eaf4d-427">Extrakce, transformace a načítání (ETL) z Azure Event Hubs tooHBase</span><span class="sxs-lookup"><span data-stu-id="eaf4d-427">Extract, Transform, and Load (ETL) from Azure Event Hubs tooHBase</span></span>](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample)
* [<span data-ttu-id="eaf4d-428">Korelovat události pomocí nástrojů Storm a HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="eaf4d-428">Correlate events using Storm and HBase on HDInsight</span></span>](hdinsight-storm-correlation-topology.md)

