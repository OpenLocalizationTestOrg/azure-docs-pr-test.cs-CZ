---
title: "topologie Storm aaaApache sadou Visual Studio a C# – Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toocreate topologie Storm v jazyce C#. Vytvořte topologii počtu jednoduché aplikace word v sadě Visual Studio pomocí nástroje hello Hadoop pro sadu Visual Studio."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 380d804f-a8c5-4b20-9762-593ec4da5a0d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/02/2017
ms.author: larryfr
ms.openlocfilehash: b3fb01a4dda144fd7fb4141e624e31e667f93753
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-c-topologies-for-apache-storm-by-using-hello-data-lake-tools-for-visual-studio"></a>Vývoj topologie C# pro Apache Storm pomocí nástrojů hello Data Lake pro Visual Studio

Zjistěte, jak toocreate topologie C# Storm pomocí hello Azure Data Lake (Hadoop) nástroje pro sadu Visual Studio. Tento dokument vás provede procesem vytvoření projektu Storm v sadě Visual Studio, místní testování a nasazení zásady tooan Apache Storm v clusteru Azure HDInsight hello.

Také zjistíte, jak toocreate hybridní topologie, které využívají součásti C# nebo Java.

> [!NOTE]
> Při hello kroky v tomto dokumentu se spoléhají na vývojového prostředí systému Windows pomocí sady Visual Studio, může být zkompilovaný projekt hello odeslaná tooeither cluster Linux nebo HDInsight se systémem Windows. Clustery se systémem Linux vytvořené po 28 říjnu 2016 se podporují jenom SCP.NET topologie.

topologie toouse C# s clusterem se systémem Linux, je nutné aktualizovat hello balíček Microsoft.SCP.Net.SDK NuGet používá ve vašem projektu tooversion 0.10.0.6 nebo novější. Hello verzi balíčku hello se taky musí shodovat hello hlavní verzi Storm v HDInsight nainstalována.

| HDInsight verze | Storm verze | SCP.NET verze | Výchozí Mono verze |
|:-----------------:|:-------------:|:---------------:|:--------------------:|
| 3.3 |0.10.x |0.10.x.x</br>(jenom na HDInsight se systémem Windows) | Není k dispozici |
| 3.4 | 0.10.0.x | 0.10.0.x | 3.2.8 |
| 3,5 | 1.0.2.x | 1.0.0.x | 4.2.1 |
| 3.6 | 1.1.0.x | 1.0.0.x | 4.2.8 |

> [!IMPORTANT]
> C# topologií v clusterech se systémem Linux musí používat rozhraní .NET 4.5 a používat Mono toorun na clusteru HDInsight hello. Zkontrolujte [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/) pro potenciální nekompatibility.

## <a name="install-visual-studio"></a>Instalace sady Visual Studio

Topologie C# s SCP.NET můžete vyvíjet pomocí jedné z následujících verzí sady Visual Studio hello:

* Visual Studio 2012 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=39305)

* Visual Studio 2013 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=44921) nebo [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)

* Visual Studio 2015 nebo [Visual Studio 2015 Community](https://go.microsoft.com/fwlink/?LinkId=532606)

* Visual Studio 2017 (všechny edice)

## <a name="install-data-lake-tools-for-visual-studio"></a>Nástroje pro instalaci Data Lake pro Visual Studio

tooinstall nástroje Data Lake pro Visual Studio, postupujte podle kroků hello v [Začínáme pomocí nástrojů Data Lake pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

## <a name="install-java"></a>Instalace Javy

Při odesílání topologie Storm ze sady Visual Studio SCP.NET generuje soubor zip, který obsahuje hello topologie a závislosti. Java je použité toocreate tyto zip souborů, protože používá formátu, který je více kompatibilní s clustery se systémem Linux.

1. Nainstalujte hello Java Developer Kit (JDK) 7 nebo novější na vašem vývojovém prostředí. Můžete získat hello Oracle JDK z [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html). Můžete také použít [jiných distribuce Java](http://openjdk.java.net/).

2. Hello `JAVA_HOME` prostředí proměnné musí bodu toohello adresář, který obsahuje Java.

3. Hello `PATH` proměnnou prostředí musí obsahovat hello `%JAVA_HOME%\bin` adresáře.

Můžete použít následující C# konzole aplikace tooverify správně nainstalována Java a hello JDK hello:

```csharp
using System;
using System.IO;
namespace ConsoleApplication2
{
   class Program
   {
       static void Main(string[] args)
       {
           string javaHome = Environment.GetEnvironmentVariable(“JAVA_HOME”);
           if (!string.IsNullOrEmpty(javaHome))
           {
               string jarExe = Path.Combine(javaHome + @”\bin”, “jar.exe”);
               if (File.Exists(jarExe))
               {
                   Console.WriteLine(“JAVA Is Installed properly”);
                    return;
               }
               else
               {
                   Console.WriteLine(“A valid JAVA JDK is not found. Looks like JRE is installed instead of JDK.”);
               }
           }
           else
           {
             Console.WriteLine(“A valid JAVA JDK is not found. JAVA_HOME environment variable is not set.”);
           }
       }  
   }
}
```

## <a name="storm-templates"></a>Šablony Storm

Hello nástroje Data Lake pro Visual Studio poskytují hello následující šablony:

| Typ projektu | Demonstruje |
| --- | --- |
| Aplikace Storm |Prázdný projekt topologie Storm. |
| Ukázka zapisovače Azure SQL Storm |Jak toowrite tooAzure databáze SQL. |
| Ukázka čtečky Azure Cosmos DB Storm |Jak tooread z Azure Cosmos DB. |
| Ukázka zapisovače Azure Cosmos DB Storm |Jak toowrite tooAzure Cosmos DB. |
| Ukázka EventHub čtečky Storm |Jak tooread z Azure Event Hubs. |
| Ukázka EventHub zapisovače Storm |Jak toowrite tooAzure Event Hubs. |
| Ukázka HBase čtečky Storm |Jak tooread z HBase v HDInsight clustery. |
| Ukázka HBase zapisovače Storm |Jak toowrite tooHBase v HDInsight clustery. |
| Ukázka hybridní Storm |Jak toouse součásti Java. |
| Ukázka Storm |Počet topologii základní aplikace word. |

> [!WARNING]
> Ne všechny šablony, bude fungovat s HDInsight se systémem Linux. Balíčky Nuget použité šablony hello nemusí být kompatibilní s Mono. Zkontrolujte hello [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentu a použít hello [.NET přenositelnost analyzátor](hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis) tooidentify potenciální problémy.

V hello kroky v tomto dokumentu můžete použít hello základní aplikace Storm projektu typu toocreate topologii.

### <a name="hbase-templates-notes"></a>Poznámky k šablony HBase

Hello čtečky HBase a zapisovače šablony pomocí hello HBase REST API, není hello HBase Java API, toocommunicate s HBase v clusteru HDInsight.

### <a name="eventhub-templates-notes"></a>Poznámky k EventHub šablony

> [!IMPORTANT]
> Hello založené na jazyce Java EventHub spout součásti, které jsou součástí hello EventHub čtečky šablony nemusí pracovat se Storm v HDInsight verze 3.5 nebo novější. Aktualizovanou verzi této součásti je k dispozici na [Githubu](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib).

Ukázkové topologie, který používá tato součást a spolupracuje s Storm v HDInsight 3.5, najdete v části [Githubu](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).

## <a name="create-a-c-topology"></a>Vytvoření topologie C#

1. Otevřete Visual Studio, vyberte **soubor** > **nový**a potom vyberte **projektu**.

2. Z hello **nový projekt** okně rozbalte **nainstalovaná** > **šablony**a vyberte **Azure Data Lake**. Hello seznam šablon, vyberte **aplikace Storm**. V hello dolní části obrazovky hello, zadejte **WordCount** jako název hello aplikace hello.

    ![Snímek obrazovky nový projekt – okno](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

3. Po vytvoření projektu hello byste měli mít hello následující soubory:

   * **Program.cs**: Tento soubor definuje hello topologie pro váš projekt. Ve výchozím nastavení se vytvoří výchozí topologie, která se skládá z jedné spout a jeden bolt.

   * **Spout.cs**: Příklad funkcí spout, který vysílá náhodných čísel.

   * **Bolt.cs**: Příklad funkcí bolt, který uchovává počet čísla vysílaných hello spout.

     Při vytváření projektu hello NuGet stahování hello nejnovější [SCP.NET balíček](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/).

     [!INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

### <a name="implement-hello-spout"></a>Implementace hello spout

1. Otevřete **Spout.cs**. Funkcích spouts jsou použité tooread data v topologii z externího zdroje. Hello hlavní komponenty pro funkcí spout jsou:

   * **NextTuple**: volány Storm, pokud hello spout je povoleno tooemit nové řazené kolekce členů.

   * **Potvrzení** (pouze pro transakční topologie): zpracovává potvrzení iniciovaná jiné komponenty v hello topologie pro odeslaný hello spout řazené kolekce členů. To v úvahu řazené kolekce členů umožňuje hello spout vědět, že byl úspěšně zpracován podřízené součásti.

   * **Selhání** (pouze pro transakční topologie): zpracovává řazené kolekce členů, které jsou selhání zpracování ostatní součásti v topologii hello. Implementace selhání metoda vám umožní toore-emitování hello řazené kolekce členů, takže může být znovu zpracována.

2. Nahraďte obsah hello hello **Spout** se hello následující text. Tato spout náhodně vysílá věty do topologie hello.

    ```csharp
    private Context ctx;
    private Random r = new Random();
    string[] sentences = new string[] {
        "hello cow jumped over hello moon",
        "an apple a day keeps hello doctor away",
        "four score and seven years ago",
        "snow white and hello seven dwarfs",
        "i am at two with nature"
    };

    public Spout(Context ctx)
    {
        // Set hello instance context
        this.ctx = ctx;

        Context.Logger.Info("Generator constructor called");

        // Declare Output schema
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // hello schema for hello default output stream is
        // a tuple that contains a string field
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
    }

    // Get an instance of hello spout
    public static Spout Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Spout(ctx);
    }

    public void NextTuple(Dictionary<string, Object> parms)
    {
        Context.Logger.Info("NextTuple enter");
        // hello sentence toobe emitted
        string sentence;

        // Get a random sentence
        sentence = sentences[r.Next(0, sentences.Length - 1)];
        Context.Logger.Info("Emit: {0}", sentence);
        // Emit it
        this.ctx.Emit(new Values(sentence));

        Context.Logger.Info("NextTuple exit");
    }

    public void Ack(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }
    ```

### <a name="implement-hello-bolts"></a>Implementace funkce bolts hello

1. Odstraňte existující hello **Bolt.cs** souboru z projektu hello.

2. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **přidat** > **novou položku**. Hello seznamu, vyberte **Storm Bolt**a zadejte **Splitter.cs** jako název hello. Opakujte tento proces toocreate s názvem druhý bolt **Counter.cs**.

   * **Splitter.cs**: implementuje funkce bolt, které rozdělí věty do jednotlivých slov a vydá nové proud slova.

   * **Counter.cs**: implementuje funkce bolt, které počty jednotlivých slov a vysílá nového datového proudu slova a hello počtu pro každou aplikaci word.

     > [!NOTE]
     > Tyto funkce bolts číst a zapisovat toostreams, ale můžete použít také bolt toocommunicate s zdroje jako databázi nebo službě.

3. Otevřete **Splitter.cs**. Ve výchozím nastavení obsahuje pouze jednu metodu: **Execute**. Hello Execute metoda je volána, když hello bolt obdrží řazené kolekce členů pro zpracování. Zde si můžete přečíst zpracovat příchozí řazených kolekcí členů a vydávání odchozích řazené kolekce členů.

4. Nahraďte obsah hello hello **rozdělovače** se hello následující kód:

    ```csharp
    private Context ctx;

    // Constructor
    public Splitter(Context ctx)
    {
        Context.Logger.Info("Splitter constructor called");
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // Input contains a tuple with a string field (hello sentence)
        inputSchema.Add("default", new List<Type>() { typeof(string) });
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // Outbound contains a tuple with a string field (hello word)
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance of hello bolt
    public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Splitter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get hello sentence from hello tuple
        string sentence = tuple.GetString(0);
        // Split at space characters
        foreach (string word in sentence.Split(' '))
        {
            Context.Logger.Info("Emit: {0}", word);
            //Emit each word
            this.ctx.Emit(new Values(word));
        }

        Context.Logger.Info("Execute exit");
    }
    ```

5. Otevřete **Counter.cs**a nahraďte obsah třída hello hello následující:

    ```csharp
    private Context ctx;

    // Dictionary for holding words and counts
    private Dictionary<string, int> counts = new Dictionary<string, int>();

    // Constructor
    public Counter(Context ctx)
    {
        Context.Logger.Info("Counter constructor called");
        // Set instance context
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string field - hello word
        inputSchema.Add("default", new List<Type>() { typeof(string) });

        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string and integer field - hello word and hello word count
        outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance
    public static Counter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Counter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get hello word from hello tuple
        string word = tuple.GetString(0);
        // Do we already have an entry for hello word in hello dictionary?
        // If no, create one with a count of 0
        int count = counts.ContainsKey(word) ? counts[word] : 0;
        // Increment hello count
        count++;
        // Update hello count in hello dictionary
        counts[word] = count;

        Context.Logger.Info("Emit: {0}, count: {1}", word, count);
        // Emit hello word and count information
        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
        Context.Logger.Info("Execute exit");
    }
    ```

### <a name="define-hello-topology"></a>Definovat topologii hello

Funkcích spouts a funkce bolts jsou uspořádány v grafu, který definuje, jak hello data proudí mezi součástmi. Pro tuto topologii hello grafu vypadá takto:

![Diagram jak jsou uspořádány součásti](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

Věty jsou nevydává hello spout a jsou distribuované tooinstances bolt rozdělovače hello. bolt rozdělovače Hello dělí do slova, které jsou distribuované toohello čítač bolt hello věty.

Vzhledem k tomu, že počet slov v instanci čítače hello trvá místně, chceme toomake jistotu, že určitá slova toku toohello stejnou instanci bolt čítače. Každá instance uchovává informace o konkrétní slova. Vzhledem k tomu, že hello rozdělovače bolt udržuje bez stavu, skutečně nezávisle na tom, kterou instanci hello rozdělovače obdrží které věty.

Otevřete **Program.cs**. je důležité metoda Hello **GetTopologyBuilder**, což je použité toodefine hello topologie, která je odeslána tooStorm. Nahraďte obsah hello **GetTopologyBuilder** s hello následující kód tooimplement hello topologie popsali výš:

```csharp
// Create a new topology named 'WordCount'
TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

// Add hello spout toohello topology.
// Name hello component 'sentences'
// Name hello field that is emitted as 'sentence'
topologyBuilder.SetSpout(
    "sentences",
    Spout.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
    },
    1);
// Add hello splitter bolt toohello topology.
// Name hello component 'splitter'
// Name hello field that is emitted 'word'
// Use suffleGrouping toodistribute incoming tuples
//   from hello 'sentences' spout across instances
//   of hello splitter
topologyBuilder.SetBolt(
    "splitter",
    Splitter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
    },
    1).shuffleGrouping("sentences");

// Add hello counter bolt toohello topology.
// Name hello component 'counter'
// Name hello fields that are emitted 'word' and 'count'
// Use fieldsGrouping tooensure that tuples are routed
//   toocounter instances based on hello contents of field
//   position 0 (hello word). This could also have been
//   List<string>(){"word"}.
//   This ensures that hello word 'jumped', for example, will always
//   go toohello same instance
topologyBuilder.SetBolt(
    "counter",
    Counter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
    },
    1).fieldsGrouping("splitter", new List<int>() { 0 });

// Add topology config
topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
{
    {"topology.kryo.register","[\"[B\"]"}
});

return topologyBuilder;
```

## <a name="submit-hello-topology"></a>Odeslání hello topologie

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **odeslání tooStorm v HDInsight**.

   > [!NOTE]
   > Pokud se zobrazí výzva, zadejte přihlašovací údaje hello vašeho předplatného Azure. Pokud máte více než jedno předplatné, přihlaste se toohello, která obsahuje váš cluster Storm v HDInsight.

2. Vyberte váš cluster Storm v HDInsight z hello **Storm Cluster** rozevíracího seznamu a potom vyberte **odeslání**. Můžete sledovat, pokud je pomocí hello úspěšné odeslání hello **výstup** okno.

3. Když hello topologie byl úspěšně odeslán, hello **topologie Storm** pro hello clusteru by se měla objevit. Vyberte hello **WordCount** topologie z hello seznamu tooview informace o hello spuštěná topologie.

   > [!NOTE]
   > Můžete také zobrazit **topologie Storm** z **Průzkumníka serveru**. Rozbalte položku **Azure** > **HDInsight**, klikněte pravým tlačítkem myši cluster Storm v HDInsight a pak vyberte **topologie Storm zobrazení**.

    tooview informace o komponentách hello v topologii hello, dvakrát klikněte na součást hello v diagramu hello.

4. Z hello **souhrn topologie** zobrazit, klikněte na tlačítko **Kill** toostop hello topologie.

   > [!NOTE]
   > Topologie Storm pokračovat toorun, dokud se deaktivovala nebo odstranění clusteru hello.

## <a name="transactional-topology"></a>Transakční topologie

předchozí topologie Hello je netransakční. Hello součásti v topologii hello neimplementuje funkci tooreplaying zprávy. Příklad transakční topologie vytvořte projekt a vyberte **Storm ukázka** jako typ projektu hello.

Transakční topologie implementovat hello následující toosupport opakování dat:

* **Ukládání do mezipaměti metadat**: hello spout musí ukládat metadata o datech hello vygenerované, tak, aby hello dat můžete načíst a vygenerované znovu, pokud dojde k chybě. Protože vysílaných hello ukázková data hello je malá, je hello nezpracovaná data pro každý záznam uloženy ve slovníku pro opakování.

* **Potvrzení**: každý bolt v topologii hello můžete volat `this.ctx.Ack(tuple)` tooacknowledge, že je úspěšně zpracovala řazené kolekce členů. Pokud všechny funkce bolts acked hello řazené kolekce členů, hello `Ack` je volána metoda hello spout. Hello `Ack` metoda umožňuje hello spout tooremove data, která se ukládá do mezipaměti pro opakování.

* **Selhání**: můžete volat každý bolt `this.ctx.Fail(tuple)` tooindicate toto zpracování došlo k chybě na řazené kolekce členů. selhání Hello rozšíří toohello `Fail` metoda hello spout, kde můžete s použitím přehrány hello řazené kolekce členů v mezipaměti metadat.

* **Pořadí ID**: při generování řazené kolekce členů, můžete zadat ID jedinečný pořadí. Tato hodnota identifikuje hello řazené kolekce členů pro zpracování opětovného přehrání (Ack a selhání). Například hello spout v hello **Storm ukázka** projektu používá následující hello při generování dat:

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    Tento kód vysílá řazené kolekce členů obsahující větu toohello výchozí datový proud, se hodnota ID pořadí hello obsažené v **lastSeqId**. V tomto příkladu **lastSeqId** se zvýší při každé řazené kolekce členů vygenerované.

Jak je předvedeno v hello **Storm ukázka** projektu, zda je součást transakční lze nastavit v době běhu v závislosti na konfiguraci.

## <a name="hybrid-topology-with-c-and-java"></a>Hybridní topologie s C# a Java

Můžete taky nástrojů Data Lake pro Visual Studio toocreate hybridní topologie, kde jsou některé součásti C# a jiné jsou Java.

Příklad hybridní topologie, vytvořte projekt a vyberte **Storm hybridní ukázka**. Tento typ ukázka ukazuje hello následující koncepty:

* **Java spout** a **C# bolt**: definovaná v **HybridTopology_javaSpout_csharpBolt**.

    * Transakční verze je definována v **HybridTopologyTx_javaSpout_csharpBolt**.

* **C# spout** a **Java bolt**: definovaná v **HybridTopology_csharpSpout_javaBolt**.

    * Transakční verze je definována v **HybridTopologyTx_csharpSpout_javaBolt**.

  > [!NOTE]
  > Tato verze také ukazuje, jak toouse Clojure kód z textového souboru jako součást Java.


topologie hello tooswitch, který se používá při odeslání projektu hello jednoduše přesunout hello `[Active(true)]` topologie toohello příkaz toouse, chcete před odesláním ji toohello clusteru.

> [!NOTE]
> Všechny soubory hello Java, které jsou požadovány jsou uvedeny jako součást tohoto projektu v hello **JavaDependency** složky.

Při vytváření a odesílání hybridní topologie, zvažte následující hello:

* Je nutné použít **JavaComponentConstructor** toocreate instanci hello třída jazyka Java pro funkcích spout nebo bolt.

* Měli byste použít **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** tooserialize data do nebo z komponent v jazyce Java z prostředí Java objekty tooJSON.

* Při odesílání hello topologie toohello serveru, je nutné použít hello **další konfigurace** možnost toospecify hello **cesty k souborům Java**. Zadaná cesta Hello by měl být hello adresář, který obsahuje hello JAR soubory, které obsahují třídy Java.

### <a name="azure-event-hubs"></a>Azure Event Hubs

Verze SCP.NET 0.9.4.203 zavádí nové třídy a metody speciálně pro práci s funkcí spout centra událostí hello (spout Java, který čte ze služby Event Hubs). Když vytvoříte topologii, která se používá spout centra událostí, použijte hello následující metody:

* **EventHubSpoutConfig** – třída: vytvoří objekt, který obsahuje hello konfiguraci pro součást spout hello.

* **TopologyBuilder.SetEventHubSpout** metoda: Přidá hello Event Hub spout součást toohello topologie.

> [!NOTE]
> Stále je nutné použít hello **CustomizedInteropJSONSerializer** tooserialize data vytvořená pomocí funkcí spout hello.

## <a id="configurationmanager"></a>Použití ConfigurationManager

Nepoužívejte **ConfigurationManager** tooretrieve konfigurační hodnoty z funkce bolt a spout součásti. Díky tomu může způsobit výjimku ukazatele null. Místo toho hello konfigurace pro váš projekt je předán do topologie Storm hello jako dvojice klíč a hodnotu v kontextu topologie hello. Jednotlivé komponenty, které jsou závislé na hodnoty konfigurace musíte je znovu načíst z kontextu hello během inicializace.

Hello následující kód ukazuje, jak tooretrieve tyto hodnoty:

```csharp
public class MyComponent : ISCPBolt
{
    // toohold configuration information loaded from context
    Configuration configuration;
    ...
    public MyComponent(Context ctx, Dictionary<string, Object> parms)
    {
        // Save a copy of hello context for this component instance
        this.ctx = ctx;
        // If it exists, load hello configuration for hello component
        if(parms.ContainsKey(Constants.USER_CONFIG))
        {
            this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
        }
        // Retrieve hello value of "Foo" from configuration
        var foo = this.configuration.AppSettings.Settings["Foo"].Value;
    }
    ...
}
```

Pokud používáte `Get` metoda tooreturn na instanci příslušné součásti, ujistěte se, že předává obou hello `Context` a `Dictionary<string, Object>` konstruktor toohello parametry. Hello následující příklad je základní `Get` metoda, která správně předává tyto hodnoty:

```csharp
public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
{
    return new MyComponent(ctx, parms);
}
```

## <a name="how-tooupdate-scpnet"></a>Jak tooupdate SCP.NET

Poslední verze SCP.NET podporovat upgrade balíčku prostřednictvím balíčku NuGet. Když je k dispozici nové aktualizace, zobrazí se oznámení o upgradu. Kontrola toomanually pro upgrade, postupujte takto:

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **spravovat balíčky NuGet**.

2. Správce balíčků hello, vyberte **aktualizace**. Pokud je k dispozici aktualizace, je uvedena. Klikněte na tlačítko **aktualizace** pro balíček tooinstall hello ho.

> [!IMPORTANT]
> Pokud váš projekt byl vytvořen z předchozích verzí SCP.NET, který NuGet nepoužili, je třeba provést následující kroky tooupdate tooa novější verze hello:
>
> 1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **spravovat balíčky NuGet**.
> 2. Pomocí hello **vyhledávání** pole, vyhledejte a pak přidejte, **Microsoft.SCP.Net.SDK** toohello projektu.

## <a name="troubleshoot-common-issues-with-topologies"></a>Řešení běžných problémů s topologie

### <a name="null-pointer-exceptions"></a>Výjimky ukazatele Null.

Při použití topologie C# s clusterem HDInsight se systémem Linux, funkce bolt a spout součásti, které používají **ConfigurationManager** tooread nastavení konfigurace v době běhu může vrátit výjimky ukazatele null.

Hello konfigurace pro váš projekt je předán do topologie Storm hello jako dvojice klíč a hodnotu v kontextu topologie hello. Mohou být načteny z hello objektu slovník, který je předán tooyour komponenty, při jejich inicializaci.

Další informace najdete v tématu hello [ConfigurationManager](#configurationmanager) část tohoto dokumentu.

### <a name="systemtypeloadexception"></a>System.TypeLoadException

Při použití topologie C# s clusterem HDInsight se systémem Linux, se můžete setkat hello následující chybě:

    System.TypeLoadException: Failure has occurred while loading a type.

K této chybě dojde, pokud použijete binární soubor, který není kompatibilní s verzí rozhraní .NET, která podporuje Mono hello.

Pro clustery HDInsight se systémem Linux Ujistěte se, že váš projekt používá binární soubory zkompilovaném pro rozhraní .NET 4.5.

### <a name="test-a-topology-locally"></a>Testování topologii místně

I když je snadno toodeploy cluster tooa topologie, v některých případech může být nutné tootest topologii místně. Použijte následující postup toorun hello a testování ukázkové topologie hello v tomto kurzu místně ve vašem vývojovém prostředí.

> [!WARNING]
> Místní testování funguje výhradně u basic, C# – pouze topologie. Nemůžete použít místní testování pro hybridní topologie nebo topologie, které používají víc datových proudů.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **vlastnosti**. V okně Vlastnosti projektu hello změnit hello **výstupní typ** příliš**konzolové aplikace**.

    ![Snímek obrazovky vlastností projektu, se zvýrazněným typem výstupu](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

   > [!NOTE]
   > Mějte na paměti, toochange hello **výstupní typ** zpět příliš**knihovny tříd** před nasazením hello topologie tooa clusteru.

2. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a pak vyberte **přidat** > **novou položku**. Vyberte **třída**a zadejte **LocalTest.cs** jako název třídy hello. Nakonec klikněte na **přidat**.

3. Otevřete **LocalTest.cs**a přidejte následující hello **pomocí** příkaz v horní části hello:

    ```csharp
    using Microsoft.SCP;
    ```

4. Použití hello následující kód jako obsah hello hello **LocalTest** třídy:

    ```csharp
    // Drives hello topology components
    public void RunTestCase()
    {
        // An empty dictionary for use when creating components
        Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

        #region Test hello spout
        {
            Console.WriteLine("Starting spout");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext spoutCtx = LocalContext.Get();
            // Get a new instance of hello spout, using hello local context
            Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

            // Emit 10 tuples
            for (int i = 0; i < 10; i++)
            {
                sentences.NextTuple(emptyDictionary);
            }
            // Use LocalContext toopersist hello data stream toofile
            spoutCtx.WriteMsgQueueToFile("sentences.txt");
            Console.WriteLine("Spout finished");
        }
        #endregion

        #region Test hello splitter bolt
        {
            Console.WriteLine("Starting splitter bolt");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext splitterCtx = LocalContext.Get();
            // Get a new instance of hello bolt
            Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);

            // Set hello data stream toohello data created by hello spout
            splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
            // Get a batch of tuples from hello stream
            List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
            // Process each tuple in hello batch
            foreach (SCPTuple tuple in batch)
            {
                splitter.Execute(tuple);
            }
            // Use LocalContext toopersist hello data stream toofile
            splitterCtx.WriteMsgQueueToFile("splitter.txt");
            Console.WriteLine("Splitter bolt finished");
        }
        #endregion

        #region Test hello counter bolt
        {
            Console.WriteLine("Starting counter bolt");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext counterCtx = LocalContext.Get();
            // Get a new instance of hello bolt
            Counter counter = Counter.Get(counterCtx, emptyDictionary);

            // Set hello data stream toohello data created by splitter bolt
            counterCtx.ReadFromFileToMsgQueue("splitter.txt");
            // Get a batch of tuples from hello stream
            List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
            // Process each tuple in hello batch
            foreach (SCPTuple tuple in batch)
            {
                counter.Execute(tuple);
            }
            // Use LocalContext toopersist hello data stream toofile
            counterCtx.WriteMsgQueueToFile("counter.txt");
            Console.WriteLine("Counter bolt finished");
        }
        #endregion
    }
    ```

    Trvat chvíli tooread prostřednictvím komentáře kódu hello. Tento kód používá **LocalContext** toorun hello součásti v hello vývojového prostředí a přetrvává hello datový proud mezi komponenty tootext soubory na místní disk hello.

1. Otevřete **Program.cs**a přidejte následující toohello hello **hlavní** metoda:

    ```csharp
    Console.WriteLine("Starting tests");
    System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
    // Initialize hello runtime
    SCPRuntime.Initialize();

    //If we are not running under hello local context, throw an error
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
    {
        throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
    }
    // Create test instance
    LocalTest tests = new LocalTest();
    // Run tests
    tests.RunTestCase();
    Console.WriteLine("Tests finished");
    Console.ReadKey();
    ```

2. Uložte změny hello a pak klikněte na tlačítko **F5** nebo vyberte **ladění** > **spustit ladění** toostart hello projektu. Okno konzoly by měla zobrazovat a stavu protokolu jako průběhu testů hello. Když **dokončení testů** se zobrazí, stiskněte klávesu žádné klíče tooclose hello okno.

3. Použití **Průzkumníka Windows** toolocate hello adresář, který obsahuje projektu. Příklad: **C:\Users\<vaše_uživatelské_jméno > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**. V tomto adresáři otevřete **Bin**a potom klikněte na **ladění**. Měli byste vidět hello textové soubory, které byly vytvořeny při hello testy byly spuštěny: sentences.txt, counter.txt a splitter.txt. Otevřete každý textový soubor a zkontrolujte hello data.

   > [!NOTE]
   > Řetězec dat trvá jako pole desetinných míst v těchto souborech. Například \[[97,103,111]] v hello **splitter.txt** soubor je slovo hello *a*.

> [!NOTE]
> Se, zda text hello tooset **typ projektu** zpět příliš**knihovny tříd** před nasazením tooa Storm v clusteru HDInsight.

### <a name="log-information"></a>Informace o protokolu

Můžete snadno protokolovat informace ze součásti vaší topologie pomocí `Context.Logger`. Například následující hello vytvoří položku informační protokolu:

```csharp
Context.Logger.Info("Component started");
```

Zaznamenané informace lze zobrazit z hello **protokol služby Hadoop**, který se nachází v **Průzkumníka serveru**. Rozbalte položku hello pro váš cluster Storm v HDInsight a pak rozbalte **protokol služby Hadoop**. Nakonec vyberte tooview souboru protokolu hello.

> [!NOTE]
> Hello protokoly jsou uloženy v hello účtu úložiště Azure, který je používán clusteru. protokoly hello tooview v sadě Visual Studio, musíte se přihlásit toohello předplatné Azure, který vlastní účet úložiště hello.

### <a name="view-error-information"></a>Informace o chybě zobrazení

tooview chybách, ke kterým došlo v topologii spuštěné, použijte hello následující kroky:

1. Z **Průzkumníka serveru**, klikněte pravým tlačítkem na hello Storm v clusteru HDInsight a vyberte **topologie Storm zobrazení**.

2. Pro hello **Spout** a **Bolts**, hello **poslední chyba** sloupec obsahuje informace o poslední chybě hello.

3. Vyberte hello **Spout Id** nebo **Bolt Id** pro hello komponenty, která obsahuje chybu uvedené. Na stránce s podrobnostmi o hello, který je zobrazený, další chyba je uvedena informace ve hello **chyby** oddíl hello dolní části stránky hello.

4. tooobtain Další informace, vyberte **Port** z hello **vykonavatelů** oddílu hello stránky, toosee hello Storm pracovní protokolu hello posledních několik minut.

### <a name="errors-submitting-topologies"></a>Chyby odesílání topologie

Pokud narazíte na chyby odesílání topologie tooHDInsight, můžete najít protokoly pro hello serverové komponenty, které zpracovávají topologie odeslání na clusteru HDInsight. tooretrieve tyto protokoly, hello použijte následující příkaz z příkazového řádku:

    scp sshuser@clustername-ssh.azurehdinsight.net:/var/log/hdinsight-scpwebapi/hdinsight-scpwebapi.out .

Nahraďte __sshuser__ s hello SSH uživatelský účet pro hello cluster. Nahraďte __clustername__ s názvem hello hello clusteru HDInsight. Další informace o používání `scp` a `ssh` s HDInsight, najdete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

Odesílání může selhat z několika důvodů:

* JDK není nainstalována nebo není v cestě hello.
* Požadované závislosti Java nejsou součástí hello odeslání.
* Nekompatibilní závislosti.
* Duplicitní názvy topologie.

Pokud hello `hdinsight-scpwebapi.out` protokol obsahuje `FileNotFoundException`, může to být způsobeno hello následující podmínky:

* Hello JDK není v cestě hello na hello vývojové prostředí. Ověřte, že hello JDK je nainstalován v hello vývojového prostředí a že `%JAVA_HOME%/bin` se nachází na cestě hello.
* Chybí závislost Java. Ujistěte se, že všechny požadované .jar soubory jsou včetně jako součást hello odeslání.

## <a name="next-steps"></a>Další kroky

Příklad zpracování dat ze služby Event Hubs naleznete v části [zpracovat události z Azure Event Hubs se Storm v HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md).

Příklad topologie C#, která rozdělí datový proud do různých datových proudů, naleznete v části [C# Storm příklad](https://github.com/Blackmist/csharp-storm-example).

Další informace o vytváření topologie C#, toodiscover naleznete v [Githubu](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

Další způsoby toowork s HDInsight a další Storm v HDInsight ukázky najdete v tématu hello následující dokumenty:

**Microsoft SCP.NET**

* [Průvodce programováním spojovací bod služby](hdinsight-storm-scp-programming-guide.md)

**Apache Storm v HDInsight**

* [Nasazení a monitorování topologie s Apache Storm v HDInsight](hdinsight-storm-deploy-monitor-topology.md)
* [Příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md)

**Apache Hadoop v HDInsight**

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)
* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)
* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)

**Apache HBase v HDInsight**

* [Začínáme s HBase v HDInsight](hdinsight-hbase-tutorial-get-started.md)
