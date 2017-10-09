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
# <a name="scp-programming-guide"></a>Průvodce programováním spojovací bod služby
Spojovací bod služby je platforma toobuild reálném čase, spolehlivé, konzistentní a vysoký výkon zpracování dat aplikace. Je založen na [Apache Storm](http://storm.incubator.apache.org/) – systém navrhovány hello OSS komunit zpracování datového proudu. Storm je určen podle Nathan Marz a otevřete která byla vytvořena pomocí služby Twitter. Využívá [Apache ZooKeeper](http://zookeeper.apache.org/), jiné Apache projektu tooenable vysoce spolehlivé distribuované koordinace a správa stavu. 

Pouze hello spojovací bod služby projektu Storm v systému Windows, která je součástí, ale také hello projektu přidat rozšíření a přizpůsobení pro ekosystém Windows hello. rozšíření Hello zahrnují vývojáře prostředí .NET a knihovny, přizpůsobení hello obsahuje nasazení systému Windows. 

Hello rozšíření a přizpůsobení se provádí tak, že jsme nemusejí toofork hello OSS projekty a jsme může využít odvozené ekosystémů nástavbou Storm.

## <a name="processing-model"></a>Zpracování modelu
Hello data v spojovací bod služby je modelovaná jako nepřetržité proudy řazené kolekce členů. Obvykle hello řazených kolekcí členů toku do některé fronty nejprve pak zachyceny a transformovat obchodní logiky hostovanou v rámci topologie Storm, nakonec výstup hello může předat jako systém tooanother spojovací bod služby řazené kolekce členů, nebo být potvrdit toostores jako systém souborů DFS nebo databáze, jako SQL Server.

![Diagram fronty napájení tooprocessing data, která kanály úložiště dat](media/hdinsight-storm-scp-programming-guide/queue-feeding-data-to-processing-to-data-store.png)

Topologie aplikací v Storm, definuje výpočetní graf. Každý uzel v topologii obsahuje logiku zpracování a odkazů mezi uzly indikují datový tok. vstupní data tooinject Hello uzly do topologie hello se nazývají funkcích Spouts, které můžou být použité toosequence hello data. Hello vstupních dat může být umístěn v protokoly transakcí databáze, čítače výkonu systému atd. hello uzly s obou toky vstupní a výstupní data se označují jako funkce Bolts, které hello filtrování skutečná data a výběry a agregaci.

Spojovací bod služby podporuje snažit v aspoň jednou a přesně-po zpracování dat. V distribuované aplikaci streamování zpracování různých chybám může dojít během zpracování dat, jako je výpadek sítě, selhání počítače nebo Chyba uživatelského kódu atd. Zpracování v aspoň jednou zajistí všechna data zpracují alespoň jednou podle přehrání automaticky hello stejná data při dochází k chybě. Na alespoň jedno zpracování je jednoduché a spolehlivé a vyhovuje dobře v mnoha aplikacích. Pokud aplikace hello vyžaduje přesné počítání, například na alespoň jedno zpracování je však dostatek vzhledem k tomu, že hello stejných dat může potenciálně se přehrávají v topologii aplikace hello. V takovém případě, přesně-po zpracování je navržený tak, že výsledek hello toomake je správný, i v případě, že hello data může být přehrány a zpracování více než jednou..

Spojovací bod služby umožňuje .NET vývojáři toodevelop reálném čase data proces aplikacím při využívání hello Java Virtual Machine (JVM) na základě Storm v části titulní hello. Hello .NET a prostředí Java Virtual Machine komunikovat přes TCP místní soketu. V podstatě každou funkcí Spout/Bolt je proces pár .net nebo Java, kde běží logiku uživatelského hello v rozhraní .net procesu jako o modul plug-in.

toobuild data, zpracování aplikace nad spojovací bod služby, jsou potřeba několik kroků:

* Návrh a implementaci hello funkcích Spouts toopull v datech z fronty.
* Návrh a implementaci funkce Bolts tooprocess hello vstupní data a uložit tooexternal úložiště dat, například databáze.
* Návrh hello topologie, odeslání a spusťte hello topologie. Definuje bodů uchycení a hello data zprostředkovatele Hello topologie toků mezi bodů uchycení hello. Spojovací bod služby bude trvat specifikace hello topologie a nasaďte ji na cluster Storm, kde každý vrchol běží na jednom uzlu logické. Hello převzetí služeb při selhání a škálování se postarat pomocí plánovače úloh Storm hello.

Tento dokument bude používat některé toowalk jednoduché příklady, jak pomocí aplikace toobuild zpracování dat se spojovací bod služby.

## <a name="scp-plugin-interface"></a>Modul plug-in rozhraní spojovací bod služby
Moduly plug-in spojovací bod služby (nebo aplikace) je samostatná souborů EXE, které můžete i spustit v prostředí Visual Studio v průběhu fáze vývoje hello a zapojené do kanálu hello Storm po nasazení v produkčním prostředí. Zápis modulu plug-in hello spojovací bod služby je hello právě stejné jako zápis jakékoli jiné standardní konzolové aplikace systému Windows. Platforma SCP.NET deklaruje některé rozhraní pro funkcí spout/bolt a hello uživatelského modulu plug-in kódu by měla implementovat tato rozhraní. hlavním účelem Hello tohoto návrhu je, že tento uživatel hello se zaměřit na své vlastní obchodní logics a nechá jiných věcí toobe zpracovávaných SCP.NET platformy.

Hello uživatelského modulu plug-in kódu by měla implementovat jednu z těchto hodnot rozhraní hello, závisí na tom, zda text hello topologie je transakční nebo netransakční, a jestli je součást hello funkcích spout nebo bolt.

* ISCPSpout
* ISCPBolt
* ISCPTxSpout
* ISCPBatchBolt

### <a name="iscpplugin"></a>ISCPPlugin
ISCPPlugin je hello společné rozhraní pro všechny typy modulů plug-in. V současné době je fiktivní rozhraní.

    public interface ISCPPlugin 
    {
    }

### <a name="iscpspout"></a>ISCPSpout
ISCPSpout je hello rozhraní pro netransakční funkcí spout.

     public interface ISCPSpout : ISCPPlugin                    
     {
         void NextTuple(Dictionary<string, Object> parms);         
         void Ack(long seqId, Dictionary<string, Object> parms);   
         void Fail(long seqId, Dictionary<string, Object> parms);  
     }

Když `NextTuple()` nazývá hello C\# uživatelského kódu můžete emitování jeden nebo více řazené kolekce členů. Pokud není nic tooemit, tato metoda by měla vrátit bez generování nic. Je potřeba poznamenat, `NextTuple()`, `Ack()`, a `Fail()` se nazývají ve smyčce úzkou v jedno vlákno v jazyce C\# procesu. Pokud neexistují žádné tooemit řazené kolekce členů, je zdvořilý toohave NextTuple spánku pro za krátkou dobu čas (například 10 ms), jako toowaste není příliš mnoho procesoru.

`Ack()`a `Fail()` bude volat pouze v případě potvrzení mechanismus je povolena v specifikace souboru. Hello `seqId` je použité tooidentify hello řazené kolekce členů, které jsou acked nebo se nezdařilo. Takže pokud ack je povoleno v netransakční topologie, je třeba používat následující funkce vysílat hello v Spout:

    public abstract void Emit(string streamId, List<object> values, long seqId); 

Pokud ack není podporován v netransakční topologii, hello `Ack()` a `Fail()` může být ponecháno prázdné funkce.

Hello `parms` vstupní parametry v tyto funkce jsou právě prázdný slovník, jsou vyhrazené pro budoucí použití.

### <a name="iscpbolt"></a>ISCPBolt
ISCPBolt je hello rozhraní pro netransakční funkcí bolt.

    public interface ISCPBolt : ISCPPlugin 
    {
    void Execute(SCPTuple tuple);           
    }

Pokud je k dispozici nové řazené kolekce členů, hello `Execute()` funkce volaná tooprocess ho.

### <a name="iscptxspout"></a>ISCPTxSpout
ISCPTxSpout je hello rozhraní pro transakční funkcí spout.

    public interface ISCPTxSpout : ISCPPlugin
    {
        void NextTx(out long seqId, Dictionary<string, Object> parms);  
        void Ack(long seqId, Dictionary<string, Object> parms);         
        void Fail(long seqId, Dictionary<string, Object> parms);        
    }

Stejně jako netransakční kontrolní část `NextTx()`, `Ack()`, a `Fail()` se nazývají ve smyčce úzkou v jedno vlákno v jazyce C\# procesu. Pokud neexistují žádné tooemit dat, je zdvořilý toohave `NextTx` režimu spánku pro krátkou dobu (10 ms), jako toowaste není příliš mnoho procesoru.

`NextTx()`je volána toostart novou transakci, hello vnější parametr `seqId` je použité tooidentify hello transakce, která je použita také v `Ack()` a `Fail()`. V `NextTx()`, uživatel může emitování straně tooJava data. Hello data budou uložena v ZooKeeper toosupport opakování. Hello kapacitu ZooKeeper je velmi omezená, a proto by měl uživatel emitování pouze metadata, ne hromadné data v transakční spout.

Bude Storm přehráním transakce automaticky, pokud se nezdaří, tak `Fail()` by neměl být volán v případě normální. Ale pokud spojovací bod služby můžete zkontrolovat hello metadata vysílaných transakční spout, můžete volat `Fail()` při hello metadat je neplatná.

Hello `parms` vstupní parametry v tyto funkce jsou právě prázdný slovník, jsou vyhrazené pro budoucí použití.

### <a name="iscpbatchbolt"></a>ISCPBatchBolt
ISCPBatchBolt je hello rozhraní pro transakční funkcí bolt.

    public interface ISCPBatchBolt : ISCPPlugin           
    {
        void Execute(SCPTuple tuple);
        void FinishBatch(Dictionary<string, Object> parms);  
    }

`Execute()`je volána, když je nové řazené kolekce členů přicházejících u hello bolt. `FinishBatch()`je volána, když je tato transakce skončila. Hello `parms` vstupní parametr je vyhrazena pro budoucí použití.

Pro transakční topologie, je důležité koncept – `StormTxAttempt`. Má dvě pole `TxId` a `AttemptId`. `TxId`je použité tooidentify konkrétní transakce a pro dané transakci, může mít několik pokus hello transakce selže a je přehrány. SCP.NET se nový různých ISCPBatchBolt objekt tooprocess každý `StormTxAttempt`, jenom jako proveďte co Storm Java straně. účelem Hello tohoto návrhu je toosupport zpracování paralelní transakce. Uživatel by měl mějte ho, pokud transakce pokus po dokončení, odpovídající objekt ISCPBatchBolt hello budou zničena a uklizeny.

## <a name="object-model"></a>Objektový Model
SCP.NET také poskytuje pro vývojáře tooprogram s jednoduchou sadou objekty klíče. Jsou **kontextu**, **úložiště stavu**, a **SCPRuntime**. Bude se zabývá hello rest součástí této části.

### <a name="context"></a>Kontext
Poskytuje kontext spuštěné aplikace toohello prostředí. Každá instance ISCPPlugin (ISCPSpout/ISCPBolt/ISCPTxSpout/ISCPBatchBolt) má odpovídající instance kontextu. Hello funkce poskytované službou kontextu je možné rozdělit do dvou částí: (1) hello statickou část, která je k dispozici v hello celou C\# zpracovat, (2) hello dynamické část, která je dostupná jenom pro konkrétní instance kontextu hello.

### <a name="static-part"></a>Statické části
    public static ILogger Logger = null;
    public static SCPPluginType pluginType;                      
    public static Config Config { get; set; }                    
    public static TopologyContext TopologyContext { get; set; }  

`Logger`pro účely protokolu poskytována.

`pluginType`je použit typ modulu plug-in hello tooindicate hello C\# procesu. Pokud hello C\# proces běží v režimu místního testovacího (bez Java), typ modulu plug-in hello je `SCP_NET_LOCAL`.

    public enum SCPPluginType 
    {
        SCP_NET_LOCAL = 0,       
        SCP_NET_SPOUT = 1,       
        SCP_NET_BOLT = 2,        
        SCP_NET_TX_SPOUT = 3,   
        SCP_NET_BATCH_BOLT = 4  
    }

`Config`je k dispozici parametry konfigurace tooget ze strany Java. Hello parametry se jí předávají ze strany Java při C\# modul plug-in je inicializován. Hello `Config` parametry jsou rozdělené do dvou částí: `stormConf` a `pluginConf`.

    public Dictionary<string, Object> stormConf { get; set; }  
    public Dictionary<string, Object> pluginConf { get; set; }  

`stormConf`je parametry definované Storm a `pluginConf` je hello parametry definované spojovací bod služby. Například:

    public class Constants
    {
        … …

        // constant string for pluginConf
        public static readonly String NONTRANSACTIONAL_ENABLE_ACK = "nontransactional.ack.enabled";  

        // constant string for stormConf
        public static readonly String STORM_ZOOKEEPER_SERVERS = "storm.zookeeper.servers";           
        public static readonly String STORM_ZOOKEEPER_PORT = "storm.zookeeper.port";                 
    }

`TopologyContext`je zadaný tooget hello topologie kontextu, je nejvhodnější pro komponenty s více stupně paralelního zpracování. Zde naleznete příklad:

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

### <a name="dynamic-part"></a>Dynamické části
Hello následující rozhraní jsou příslušné tooa určité instance kontextu. instance kontextu Hello se vytvoří platformou SCP.NET a předán toohello uživatelského kódu:

    // Declare hello Output and Input Stream Schemas

    public void DeclareComponentSchema(ComponentStreamSchema schema);   

    // Emit tuple toodefault stream.
    public abstract void Emit(List<object> values);                   

    // Emit tuple toohello specific stream.
    public abstract void Emit(string streamId, List<object> values);  

Pro podporu ack netransakční funkcí spout je poskytována hello následující metodu:

    // for non-transactional Spout which supports ack
    public abstract void Emit(string streamId, List<object> values, long seqId);  

Pro podporu ack netransakční bolt, by měl explicitně `Ack()` nebo `Fail()` hello obdržel řazené kolekce členů. A při vytváření nové řazené kolekce členů, musíte také zadat hello kotvy hello nové řazené kolekce členů. Hello následující metody jsou k dispozici.

    public abstract void Emit(string streamId, IEnumerable<SCPTuple> anchors, List<object> values); 
    public abstract void Ack(SCPTuple tuple);
    public abstract void Fail(SCPTuple tuple);

### <a name="statestore"></a>Úložiště stavu
`StateStore`poskytuje metadata služby, generování monotónní pořadí a bez čekání spolupráce. Abstrakce vyšší úrovně distribuované souběžnosti se dají vytvářet `StateStore`, včetně distribuované zámky, distribuované fronty, překážek a transakce služby.

Spojovací bod služby aplikace mohou používat hello `State` objektu toopersist některé informace v ZooKeeper, zejména pro transakční topologii. To proto, pokud transakční spout zhroucení a restartování, může načíst hello nezbytné informace z ZooKeeper a restartujte hello kanálu.

Hello `StateStore` objekt především má tyto metody:

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

Hello `State` objekt především má tyto metody:

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

Pro hello `Commit()` metodu, pokud je nastavena simpleMode tootrue, jednoduše odstraní odpovídající ZNode v ZooKeeper hello. Jinak se odstraní hello aktuální ZNode a přidání nového uzlu v hello potvrzení\_cesta.

### <a name="scpruntime"></a>SCPRuntime
SCPRuntime poskytuje hello následující dvě metody.

    public static void Initialize();

    public static void LaunchPlugin(newSCPPlugin createDelegate);  

`Initialize()`je použité tooinitialize hello spojovací bod služby modulu runtime prostředí. Tato metoda hello C\# proces připojí straně toohello Java a získá konfigurace parametrů a kontextu topologie.

`LaunchPlugin()`použít tookick vypnout uvítací zprávu zpracovává smyčky. V této smyčky hello C\# modulu plug-in se zobrazí formulář zprávy Java straně (včetně řazených kolekcí členů a řízení signály) a pak zadejte zprávy hello procesu, například voláním metody rozhraní hello pomocí hello uživatelského kódu. Hello vstupního parametru pro metodu `LaunchPlugin()` je delegáta, který může vrátit objekt, který ISCPSpout/IScpBolt/ISCPTxSpout/ISCPBatchBolt rozhraní implementovat.

    public delegate ISCPPlugin newSCPPlugin(Context ctx, Dictionary\<string, Object\> parms); 

Pro ISCPBatchBolt, nám získat `StormTxAttempt` z `parms`a použít ho toojudge, jestli je přehraná pokus. To se obvykle provádí na hello potvrzení bolt a je znázorněn v hello `HelloWorldTx` příklad.

Obecně řečeno hello modulů plug-in spojovací bod služby může spustit ve dvou režimech tady:

1. Místní testovací režim: V tomto režimu hello modulů plug-in spojovací bod služby (hello C\# uživatelského kódu) v sadě Visual Studio spustit v průběhu fáze vývoje hello. `LocalContext`můžete použít v tomto režimu, který poskytuje metoda tooserialize hello vygenerované řazených kolekcí členů toolocal soubory a číst jejich zpět toomemory.
   
        public interface ILocalContext
        {
            List\<SCPTuple\> RecvFromMsgQueue();
            void WriteMsgQueueToFile(string filepath, bool append = false);  
            void ReadFromFileToMsgQueue(string filepath);                    
        }
2. Regulární režim: V tomto režimu, moduly plug-in spojovací bod služby hello spustily procesem storm java.
   
    Tady je příklad spuštění modulu plug-in spojovací bod služby:
   
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

## <a name="topology-specification-language"></a>Topologie specifikace jazyka
Určení topologie spojovací bod služby je domény konkrétní jazyk pro popis a konfiguraci topologie spojovací bod služby. Je založena na Storm na Clojure DSL (<http://storm.incubator.apache.org/documentation/Clojure-DSL.html>) a je prodloužena spojovací bod služby.

Specifikace topologie jde odeslat přímo toostorm clusteru pro provedení prostřednictvím hello ***runspec*** příkaz.

SCP.NET má přidejte postupujte podle funkce toodefine hello transakční topologie:

| **Nové funkce** | **Parametry** | **Popis** |
| --- | --- | --- |
| **TX topolopy** |název topologie<br />spout mapy<br />bolt mapy |Definovat topologii transakcí s názvem topologie hello &nbsp;spouts definice mapy a hello funkce bolts definice mapy |
| **spojovací bod služby. tx spout** |Exec – název<br />argumentů<br />Pole |Definujte transakční funkcí spout. Spustí aplikace hello s ***exec název*** pomocí ***argumentů***.<br /><br />Hello ***pole*** je pole výstup hello spout |
| **spojovací bod služby tx-batch-bolt** |Exec – název<br />argumentů<br />Pole |Definujte transakční Bolt dávky. Spustí aplikace hello s ***exec název*** pomocí ***argumentů.***<br /><br />Hello pole je hello výstup polí pro funkcí bolt. |
| **spojovací bod služby tx potvrzení bolt** |Exec – název<br />argumentů<br />Pole |Definujte transakční Committer Bolt. Spustí aplikace hello s ***exec název*** pomocí ***argumentů***.<br /><br />Hello ***pole*** je pole výstup hello bolt |
| **nontx topolopy** |název topologie<br />spout mapy<br />bolt mapy |Definovat topologii netransakční s názvem topologie hello&nbsp; spouts definice mapy a hello funkce bolts definice mapy |
| **spout spojovací bod služby** |Exec – název<br />argumentů<br />Pole<br />parameters |Definujte netransakční spout. Spustí aplikace hello s ***exec název*** pomocí ***argumentů***.<br /><br />Hello ***pole*** je pole výstup hello spout<br /><br />Hello ***parametry*** je volitelná, použití toospecify některé parametry, jako je například "nontransactional.ack.enabled". |
| **bolt spojovací bod služby** |Exec – název<br />argumentů<br />Pole<br />parameters |Definujte netransakční funkcí Bolt. Spustí aplikace hello s ***exec název*** pomocí ***argumentů***.<br /><br />Hello ***pole*** je pole výstup hello bolt<br /><br />Hello ***parametry*** je volitelná, použití toospecify některé parametry, jako je například "nontransactional.ack.enabled". |

SCP.NET má postupujte podle klíče slova definované:

| **Klíčová slova** | **Popis** |
| --- | --- |
| **: název** |Definování hello název topologie |
| **: topologie** |Definování hello topologie pomocí hello výše funkce a sestavení v těch, které jsou. |
| **: p** |Definujte hello paralelismus nápovědu pro každou funkcí spout nebo bolt. |
| **: Konfigurace** |Definování konfigurace parametru nebo aktualizace hello existující |
| **: schéma** |Definujte hello schématu datového proudu. |

A často používané parametry:

| **Parametr** | **Popis** |
| --- | --- |
| **"plugin.name"** |Název souboru EXE modulu plug-in hello C# |
| **"plugin.args"** |argumentů modulu plug-in |
| **"output.schema"** |Schéma výstupu |
| **"nontransactional.ack.enabled"** |Určuje, jestli je povolená ack pro netransakční topologie |

příkaz runspec Hello nasadí společně s hello bits, využití hello je jako:

    .\bin\runSpec.cmd
    usage: runSpec [spec-file target-dir [resource-dir] [-cp classpath]]
    ex: runSpec examples\HelloWorld\HelloWorld.spec specs examples\HelloWorld\Target

Hello ***prostředků dir*** parametr je volitelný, je třeba toospecify ho, když chcete, aby tooplug a C\# aplikace a tento adresář bude obsahovat aplikaci hello hello závislosti a konfigurace.

Hello ***cesty pro třídy*** parametr je nepovinný. Pokud soubor specifikace hello obsahuje Java Spout nebo Bolt je použité toospecify hello Java cesty pro třídy.

## <a name="miscellaneous-features"></a>Různé funkce
### <a name="input-and-output-schema-declaration"></a>Vstup a výstup schématu deklarace
Hello uživatele můžete emitování řazené kolekce členů v jazyce C\# zpracování, hello platformy musí tooserialize hello řazené kolekce členů do byte [] straně tooJava přenos a Storm přenese tento cíle toohello řazené kolekce členů. Mezitím v podřízené součásti hello C\# proces bude přijímat řazené kolekce členů zpět z straně java a převádět je původní typy toohello podle platformy, všechny tyto operace jsou skryt hello platformy.

toosupport hello serializace a deserializace, uživatelský kód musí toodeclare hello schéma hello vstupy a výstupy.

schéma vstupu a výstupu datového proudu Hello je definován jako slovník, hello klíč je hello StreamId a hello hodnota je hello typy sloupců hello. součást Hello může mít víc datových proudů deklarován.

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


V kontextu objektu máme hello přidat následující rozhraní API:

    public void DeclareComponentSchema(ComponentStreamSchema schema)

Uživatelský kód musí zajistit vygenerované řazených kolekcí členů hello orientují hello schéma definované pro tento datový proud nebo hello systému vyvolá výjimku modulu runtime.

### <a name="multi-stream-support"></a>Podpora více datového proudu
Spojovací bod služby podporuje uživatele code tooemit nebo přijímat z několika různých datových proudů v hello stejný čas. Podpora Hello odráží v kontextu objektu hello jako hello vysílat metoda přijímá parametr ID volitelné datového proudu.

Byly přidány dvě metody v hello SCP.NET kontextu objektu. Jsou použité tooemit řazené kolekce členů nebo řazené kolekce členů toospecify StreamId. Hello StreamId je řetězec a je nutné toobe konzistentní v obou C\# a hello specifikace definice topologie.

        /* Emit tuple toohello specific stream. */
        public abstract void Emit(string streamId, List<object> values);

        /* for non-transactional Spout only */
        public abstract void Emit(string streamId, List<object> values, long seqId);

Hello datového proudu neexistující generování tooa způsobí, že výjimky za běhu.

### <a name="fields-grouping"></a>Pole seskupení
Hello sestavení v seskupení pole v Strom nepracuje správně v SCP.NET. Na hello straně Java Proxy všechny hello pole datové typy jsou ve skutečnosti byte [] a seskupení pole hello používá hello byte [] objekt hash kód tooperform hello seskupení. Kód hash pro objekt Hello byte [] je hello adresa tohoto objektu v paměti. Tak bude hello seskupení nesprávný pro dva bajty [] objekty hello této sdílené složky stejný obsah, ale není hello stejnou adresu.

SCP.NET přidá metoda přizpůsobené seskupení a použije obsah hello hello byte [] toodo hello seskupení. V **specifikace** souboru hello syntaxe je jako:

    (bolt-spec
        {
            "spout_test" (scp-field-group :non-tx [0,1])
        }
        …
    )


Tady

1. "spojovací bod služby skupiny polí" znamená "Vlastní pole seskupení implementované spojovací bod služby".
2. ": tx"nebo": bez tx" znamená, pokud je transakční topologie. Tyto informace potřebujeme od hello od indexu se liší v tx oproti bez tx topologie.
3. [0,1] znamená hashset ID pole, počínaje od 0.

### <a name="hybrid-topology"></a>Hybridní topologie
nativní Hello Storm je napsán v jazyce Java. A SCP.Net má rozšířené tooenable naše celní toowrite C\# code toohandle své obchodní logiku. Také podporujeme hybridní topologie, která obsahuje nejen C, ale\# funkcích spouts nebo funkce bolts, ale také Java funkcí Spout/funkce Bolts.

### <a name="specify-java-spoutbolt-in-spec-file"></a>Zadat Java funkcí Spout/Bolt specifikace souboru
V souboru specifikace "spojovací bod služby funkcí spout" a "spojovací bod služby funkcí bolt" může být také použít toospecify Spouts Java a funkce Bolts, tady je příklad:

    (spout-spec 
      (microsoft.scp.example.HybridTopology.Generator.)           
      :p 1)

Zde `microsoft.scp.example.HybridTopology.Generator` je název hello hello Java Spout třídy.

### <a name="specify-java-classpath-in-runspec-command"></a>Zadejte cestu pro třídy Java v runSpec příkaz
Pokud chcete toosubmit topologie obsahující Java Spouts nebo funkce Bolts, potřebovat hello kompilace toofirst Java Spouts nebo funkce Bolts a získání souborů Jar hello. Pak musíte zadat cestě třídy hello java, která obsahuje soubory Jar hello při odesílání topologie. Zde naleznete příklad:

    bin\runSpec.cmd examples\HybridTopology\HybridTopology.spec specs examples\HybridTopology\net\Target -cp examples\HybridTopology\java\target\*

Zde **příklady\\HybridTopology\\java\\cíl\\**  je hello složku obsahující soubor Jar funkcí Spout/Bolt Java hello.

### <a name="serialization-and-deserialization-between-java-and-c"></a>Serializace a deserializace mezi Java a C\
Součásti naší spojovací bod služby zahrnuje straně Java a C\# straně. V pořadí toointeract s nativní Java funkcích Spouts nebo funkce Bolts, musí být provedena serializaci nebo deserializaci mezi straně Java a C\# straně, jak je znázorněno v následujícím grafu hello.

![Diagram součásti java odesílání tooSCP součást odesílání tooJava součásti](media/hdinsight-storm-scp-programming-guide/java-compent-sending-to-scp-component-sending-to-java-component.png)

1. **Serializace v jazyce Java straně a deserializace v jazyce C\# straně**
   
   Nejprve poskytujeme výchozí implementace v jazyce Java straně serializace a deserializace v jazyce C\# straně. Metoda serializace Hello na straně Java lze zadat v souboru specifikace:
   
       (scp-bolt
           {
               "plugin.name" "HybridTopology.exe"
               "plugin.args" ["displayer"]
               "output.schema" {}
               "customized.java.serializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer"]
           })
   
   Hello deserializace metody v jazyce C\# straně musí být zadán v jazyce C\# uživatelského kódu:
   
       Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
       inputSchema.Add("default", new List<Type>() { typeof(Person) });
       this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
       this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());            
   
   Tato výchozí implementace by měla řídit většinou, pokud hello datový typ není příliš složitý. Pro určité případy buď protože hello uživatelský datový typ je příliš složité, nebo protože hello výkon naše výchozí implementace nesplňuje hello požadavek uživatele, uživatel může modul plug-in vlastní implementaci.
   
   Hello serializovat rozhraní v jazyce java straně je definován jako:
   
       public interface ICustomizedInteropJavaSerializer {
           public void prepare(String[] args);
           public List<ByteBuffer> serialize(List<Object> objectList);
       }
   
   Hello deserializovat rozhraní v jazyce C\# straně je definován jako:
   
   veřejné rozhraní ICustomizedInteropCSharpDeserializer
   
       public interface ICustomizedInteropCSharpDeserializer
       {
           List<Object> Deserialize(List<byte[]> dataList, List<Type> targetTypes);
       }
2. **Serializace v jazyce C\# straně a deserializace v jazyce Java souběžný**
   
   Hello metoda serializace v jazyce C\# straně musí být zadán v jazyce C\# uživatelského kódu:
   
       this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer()); 
   
   Hello deserializace metody na straně Java musí být zadán v souboru specifikace:
   
     (spojovací bod služby spout
   
       {
         "plugin.name" "HybridTopology.exe"
         "plugin.args" ["generator"]
         "output.schema" {"default" ["person"]}
         "customized.java.deserializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" "microsoft.scp.example.HybridTopology.Person"]
       })
   
   Následuje název hello deserializátor "microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" a "microsoft.scp.example.HybridTopology.Person" je, že je k deserializaci hello cílová třída hello data.
   
   Uživatel může také modulu plug-in vlastní implementace C\# serializátor a deserializátor Java. Toto je hello rozhraní pro C\# serializátor:
   
       public interface ICustomizedInteropCSharpSerializer
       {
           List<byte[]> Serialize(List<object> dataList);
       }
   
   Toto je hello rozhraní pro deserializátor Java:
   
       public interface ICustomizedInteropJavaDeserializer {
           public void prepare(String[] targetClassNames);
           public List<Object> Deserialize(List<ByteBuffer> dataList);
       }

## <a name="scp-host-mode"></a>Spojovací bod služby hostitele režimu
V tomto režimu můžete uživatele jejich kódy tooDLL zkompilovat a použít SCPHost.exe poskytované topologie toosubmit spojovací bod služby. Specifikace souboru Hello vypadá takto:

    (scp-spout
      {
        "plugin.name" "SCPHost.exe"
        "plugin.args" ["HelloWorld.dll" "Scp.App.HelloWorld.Generator" "Get"]
        "output.schema" {"default" ["sentence"]}
      })

Zde `plugin.name` je zadán jako `SCPHost.exe` poskytované spojovací bod služby SDK. SCPHost.exe které přijímá přesně tři parametry:

1. Hello nejprve jeden je hello DLL název, který je `"HelloWorld.dll"` v tomto příkladu.
2. Hello druhá je hello názvu třídy, což je `"Scp.App.HelloWorld.Generator"` v tomto příkladu.
3. Hello třetí jeden je hello název veřejné statické metody, které může být vyvolaná tooget instanci ISCPPlugin.

V režimu hostitele uživatelský kód kompiluje jako knihovny DLL a je vyvolána platformou spojovací bod služby. Spojovací bod služby platformy, můžete získat úplné řízení pro logiku celou zpracování hello. Proto doporučujeme naše zákazníky toosubmit topologii spojovací bod služby hostitele režimu vzhledem k tomu, že ho můžete zjednodušit hello vývojového prostředí a přineste nám větší flexibilitu a zpětnou kompatibilitu pro také novější verzi.

## <a name="scp-programming-examples"></a>Příklady programování spojovací bod služby
### <a name="helloworld"></a>Hello World
**Hello World** je velmi jednoduchý příklad tooshow chuť SCP.Net. Používá topologii netransakční s spout, nazývá **generátor**a dvě funkce bolts názvem **rozdělovače** a **čítač**. Hello spout **generátor** se náhodně generovat některé věty a příliš emitování tyto věty**rozdělovače**. Hello bolt **rozdělovače** bude rozdělení toowords věty hello a emitování tato slova příliš**čítač** funkcí bolt. Čítač"Hello bolt" používá slovník toorecord hello výskyt počet jednotlivých slov.

Existují dva soubory specifikace, **HelloWorld.spec** a **HelloWorld\_EnableAck.spec** v tomto příkladu. V hello C\# kódu, ho můžete zjistit, zda je povoleno potvrzení získáním hello pluginConf ze strany Java.

    /* demo how tooget pluginConf info */
    if (Context.Config.pluginConf.ContainsKey(Constants.NONTRANSACTIONAL_ENABLE_ACK))
    {
        enableAck = (bool)(Context.Config.pluginConf[Constants.NONTRANSACTIONAL_ENABLE_ACK]);
    }
    Context.Logger.Info("enableAck: {0}", enableAck);

Ve funkcích spout hello Pokud je povoleno potvrzení, slovník je použité toocache hello záznamů, které nebyly acked. Pokud je volána Fail(), hello selhání řazené kolekce členů budou přehrány:

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

### <a name="helloworldtx"></a>HelloWorldTx
Hello **HelloWorldTx** příklad ukazuje, jak tooimplement transakční topologie. Neměl mít jeden spout názvem **generátor**, funkce bolts batch názvem **partial počet**, potvrzení bolt s názvem **počet součet**. Existují také tři soubory txt předem vytvořené: **DataSource0.txt**, **DataSource1.txt** a **DataSource2.txt**.

V každou transakci, hello spout **generátor** náhodně zvolím dva soubory z hello předem vytvořené tři soubory a emitování hello dva souboru názvy toohello **partial počet** funkcí bolt. Hello bolt **partial počet** nejdřív získat název souboru hello z hello přijatých řazené kolekce členů a pak otevřete hello souboru a počet hello počtu slov v tomto souboru a nakonec emitování hello word číslo toohello **počet – součet**funkcí bolt. Hello **počet součet** bolt shrnuje celkový počet hello.

tooachieve **právě jednou** sémantiku, hello potvrzení bolt **počet součet** potřebovat toojudge toho, jestli je přehraná transakce. V tomto příkladu má proměnná statický člen:

    public static long lastCommittedTxId = -1; 

Když je vytvořena ISCPBatchBolt instance, se budou získávat hello `txAttempt` ze vstupní parametry:

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

Když `FinishBatch()` je volána, hello `lastCommittedTxId` bude aktualizace, pokud není přehraná transakce.

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


### <a name="hybridtopology"></a>HybridTopology
Tato topologie obsahuje Java Spout a a C\# funkcí Bolt. Používá hello výchozí serializace a deserializace implementace poskytovaná spojovací bod služby platformy. Prosím ref hello **HybridTopology.spec** v **příklady\\HybridTopology** složku podrobnosti hello specifikace souboru, a **SubmitTopology.bat** jak Cesta pro třídy toospecify Java.

### <a name="scphostdemo"></a>SCPHostDemo
V tomto příkladu je hello v podstatě stejné jako Hello World. Hello pouze rozdílem je, že hello uživatele kódu jako knihovny DLL a odeslání hello topologie pomocí SCPHost.exe. Podrobnější vysvětlení prosím ref hello část "Spojovací bod služby hostitele režim".

## <a name="next-steps"></a>Další kroky
Příklady topologií Storm vytvořený spojovací bod služby najdete v tématu hello následující:

* [Vývoj topologie C# pro Apache Storm v HDInsight pomocí sady Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [Zpracování událostí z Azure Event Hubs se Storm v HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md)
* [Vytvoření více datových proudů v topologie C# Storm](hdinsight-storm-twitter-trending.md)
* [Použít data toovisualize Power Bi od topologie Storm](hdinsight-storm-power-bi-topology.md)
* [Zpracování dat snímačů vehicle ze služby Event Hubs pomocí Storm v HDInsight](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/IotExample)
* [Extrakce, transformace a načítání (ETL) z Azure Event Hubs tooHBase](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample)
* [Korelovat události pomocí nástrojů Storm a HBase v HDInsight](hdinsight-storm-correlation-topology.md)

