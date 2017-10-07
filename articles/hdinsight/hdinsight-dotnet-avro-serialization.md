---
title: aaaSerialize data v Hadoop - Microsoft Avro Library - Azure | Microsoft Docs
description: "Zjistěte, jak tooserialize a deserializaci dat v Hadoop v HDInsight pomocí hello Microsoft Avro Library toopersist toomemory, databáze nebo souboru."
keywords: avro hadoop avro
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: c78dc20d-5d8d-4366-94ac-abbe89aaac58
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: jgao
ms.custom: hdiseo17may2017
ms.openlocfilehash: f364f8e855a54c0fc160e9a106ec8d5b30c6db23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="serialize-data-in-hadoop-with-hello-microsoft-avro-library"></a>Serializace dat v Hadoop pomocí hello Microsoft Avro Library

>[!NOTE]
>Hello Avro SDK je podporován společností Microsoft. Knihovna Hello je komunita s otevřeným zdrojem podporována. Hello zdroje pro hello knihovně jsou umístěné v [Githubu](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).

Toto téma ukazuje, jak toouse hello [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) tooserialize objekty a další data struktur do datových proudů toopersist je toomemory, databáze nebo souboru. Také ukazuje, jak toodeserialize je toorecover hello původní objekty.

[!INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

## <a name="apache-avro"></a>Apache Avro
Hello <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> hello implementuje Apache Avro systém serializace dat pro prostředí Microsoft.NET hello. Apache Avro poskytuje formát pro výměnu kompaktní binární data pro serializaci. Používá <a href="http://www.json.org" target="_blank">JSON</a> toodefine jazykově nezávislého schématu, které poskytuje vzájemnou vzájemná funkční spolupráce jazyka. Data serializovaná v jednom jazyce lze číst v jiném. Aktuálně jsou podporované C, C++, C#, Java, PHP, Python a Ruby. Podrobné informace o formátu hello naleznete v hello <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro Specification</a>. 

>[!NOTE]
>Hello Microsoft Avro Library nepodporuje hello vzdálené procedury volání (RPC) součástí téhle specifikaci.
>

Hello serializované reprezentace objektu v hello Avro systému se skládá ze dvou částí: schéma a skutečnou hodnotu. schématu Avro Hello popisuje hello nezávislé na jazyku datový model dat hello serializovat s JSON. Zobrazí se node souběžně s binární reprezentace data. Každý objekt toobe napsané pomocí žádné režijní náklady na hodnotu, provedení serializace rychlého a hello reprezentace malé s hello schématu odděleně od binární reprezentace hello umožňuje.

## <a name="hello-hadoop-scenario"></a>scénář Hadoop Hello
Formát serializace Apache Avro Hello se často používá v Azure HDInsight a dalších prostředích s Apache Hadoop. Avro nabízí pohodlný způsob toorepresent komplexní datové struktury v rámci úlohy Hadoop MapReduce. Hello formát souborů nástroje Avro (soubor kontejneru objektu Avro) byl programovací model MapReduce navrženou toosupport hello distribuován. Hello klíčové funkce, která umožňuje distribuční hello je, že hello soubory jsou "rozdělitelné" hello smysl, která můžete vyhledat libovolný bod v souboru a začít číst od určitého bloku.

## <a name="serialization-in-avro-library"></a>Serializace v knihovně Avro
Hello knihovny .NET pro Avro podporuje dva způsoby serializaci objektů:

* **reflexe** -hello schéma JSON pro typy hello automaticky vytvořen z dat hello kontrakt atributy hello .NET typy toobe serializovat.
* **Obecné záznam** -schématu A JSON je explicitně určena v záznamu reprezentována hello [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) třídy existuje toodescribe hello schéma pro hello data toobe po žádné typy .NET serializovat.

Pokud schéma dat hello se označuje tooboth hello zapisovače a čtečku datového proudu hello, odesláním hello dat bez jeho schématu. V případech, pokud soubor Avro objektu kontejneru se používá, jsou hello schématu uložená v souboru hello. Další parametry, jako je například kodeků hello používá pro kompresi dat lze zadat. Tyto scénáře jsou popsané v další podrobnosti a ukazuje hello následující příklady kódu:

## <a name="install-avro-library"></a>Nainstaluje Avro knihovny
Následující Hello jsou požadovány, než nainstalujete hello knihovně:

* <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Rozhraní Microsoft .NET Framework 4</a>
* <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 nebo novější)

Všimněte si, že hello Newtonsoft.Json.dll závislostí se automaticky stáhne hello instalaci hello Microsoft Avro Library. Postup Hello je součástí hello následující části:

Hello Microsoft Avro Library je distribuován jako balíčku NuGet, který lze nainstalovat ze sady Visual Studio prostřednictvím hello následující postup:

1. Vyberte hello **projektu** -> karta **spravovat balíčky NuGet...**
2. Vyhledejte "Microsoft.Hadoop.Avro" hello **Online hledání** pole.
3. Klikněte na tlačítko hello **nainstalovat** tlačítko vedle příliš**knihovnu Microsoft Azure HDInsight Avro**.

Všimněte si, že hello Newtonsoft.Json.dll (> = 6.0.4) závislostí je také automaticky staženy s hello Microsoft Avro Library.

Hello Microsoft Avro Library zdrojový kód je k dispozici na [Githubu](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).

## <a name="compile-schemas-using-avro-library"></a>Kompilace pomocí knihovny Avro schémata
Hello Microsoft Avro Library obsahuje generování kódu, nástroj, který umožňuje vytváření typy C# automaticky podle hello dříve definované schéma JSON. Nástroj pro generování kódu Hello není distribuován jako binární spustitelný soubor, ale se dají snadno vytvářet prostřednictvím hello následující postup:

1. Stáhněte si soubor .zip hello hello nejnovější verzi sady SDK HDInsight zdrojového kódu z <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK pro Hadoop</a>. (Klikněte na tlačítko hello **Stáhnout** ikonu, není hello **stáhne** karta.)
2. Extrahujte hello adresáře tooa sady SDK HDInsight z počítače hello pomocí rozhraní .NET Framework 4 nainstalovaný a připojený toohello Internetu pro stahování balíčků NuGet potřebné závislosti. Níže předpokládáme, že hello zdrojový kód je extrahované tooC:\SDK.
3. Přejít toohello složku C:\SDK\src\Microsoft.Hadoop.Avro.Tools a spusťte build.bat. (hello volání souboru MSBuild z hello 32-bit distribuce hello rozhraní .NET Framework. Pokud chcete toouse hello 64bitovou verzi, build.bat upravit následující hello komentáře v souboru hello.) Zkontrolujte, zda text hello sestavení úspěšné. (Na některých systémů MSBuild může vytvořit upozornění. Tato upozornění neovlivňují hello nástroj, dokud nedojde k žádným chybám sestavení.)
4. Hello zkompilovat nástroj se nachází v C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.

tooget obeznámeni s hello syntaxe příkazového řádku, spusťte následující příkaz z hello složky, kde se nachází nástroj pro generování kódu hello hello:`Microsoft.Hadoop.Avro.Tools help /c:codegen`

Nástroj hello tootest, můžete z hello ukázka JSON soubor schématu s hello zdrojového kódu vygenerovat třídy jazyka C#. Spusťte následující příkaz hello:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

To by měl tooproduce dva C# soubory v aktuálním adresáři hello: SensorData.cs a Location.cs.

toounderstand hello logiky, která používá nástroj pro generování kódu hello při převodu typů tooC # hello JSON schématu, naleznete v souboru hello, GenerationVerification.feature umístěný v C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.

Obory názvů se extrahují z hello schématu JSON, pomocí logiky hello popsané v souboru hello zmíněných v předchozím odstavci hello. Obory názvů extrahována ze schématu hello mají přednost před ať se poskytuje s parametrem/n hello v hello nástroj příkazového řádku. Pokud chcete toooverride hello obory názvů obsažené v hello schéma, použijte parametr /nf hello. Například toochange všechny obory názvů v hello SampleJSONSchema.avsc toomy.own.nspace, provést hello následující příkaz:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="about-hello-samples"></a>O ukázky hello
Šest příklady uvedené v tomto tématu ilustrují různé scénáře nepodporuje hello Microsoft Avro Library. Hello Microsoft Avro Library je navrženou toowork s žádné datového proudu. V těchto příkladech je upravit data prostřednictvím datových proudů paměti místo souborů datových proudů nebo databází kvůli jednoduchosti a přehlednosti. Hello přístup použitý v produkčním prostředí závisí na hello požadavků na přesný scénář, zdroj dat a svazek, omezení výkonu a dalších faktorů.

jak Hello první dva příklady zobrazit tooserialize a deserializaci dat do vyrovnávací paměti datového proudu pomocí reflexe a obecné záznamy. Hello schématu v obou případech se předpokládá, že toobe sdílena mezi hello čtení a zápis out-of-band.

Zobrazit Hello třetí a čtvrtý příklady jak tooserialize a deserializaci dat pomocí hello Avro objektu kontejneru souborů. Pokud data uložená v souboru Avro kontejneru, její schéma je vždy uložit s ním, protože hello schématu musí být sdílená pro deserializaci.

Hello obsahující ukázkový text hello první čtyři příklady si můžete stáhnout z hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">ukázky kódu Azure</a> lokality.

Hello páté příklad ukazuje způsob toouse vlastní kompresní kodek pro Avro objektu kontejneru soubory. Ukázka obsahující hello kód pro tento příklad si můžete stáhnout z hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">ukázky kódu Azure</a> lokality.

Hello šesté příklad ukazuje, jak toouse Avro serializace tooupload data tooAzure úložiště objektů Blob a analyzujte ji pomocí clusteru služby HDInsight (Hadoop) Hive. Lze ji stáhnout z hello <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">ukázky kódu Azure</a> lokality.

Tady jsou odkazy toohello šesti ukázky popsané v tématu hello:

* <a href="#Scenario1">**Serializace pomocí reflexe** </a> -hello schéma JSON pro typy toobe serializovat automaticky vytvořen z dat hello kontrakt atributy.
* <a href="#Scenario2">**Serializace s obecné záznam** </a> -schématu JSON hello je explicitně určena v záznamu, když je k dispozici pro reflexi žádný typ rozhraní .NET.
* <a href="#Scenario3">**Serializace objektu kontejneru soubory pomocí reflexe** </a> -schématu JSON hello je automaticky vytvořen a sdílené společně s hello serializovat data prostřednictvím soubor Avro objektu kontejneru.
* <a href="#Scenario4">**Serializace objektu kontejneru soubory pomocí obecné záznam** </a> -schématu JSON hello je explicitně zadaný před hello serializace a sdílet společně s hello dat přes soubor Avro objektu kontejneru.
* <a href="#Scenario5">**Serializace objektu kontejneru soubory pomocí vlastní kompresní kodek** </a> -hello příklad ukazuje, jak toocreate Avro objektu kontejneru soubor s vlastní .NET provádění hello Deflate data kompresní kodek.
* <a href="#Scenario6">**Pomocí Avro tooupload dat pro služby Microsoft Azure HDInsight hello** </a> -hello příklad ukazuje, jak Avro serializace komunikuje s hello služba HDInsight. Active Azure předplatného a přístup tooan cluster Azure HDInsight vyžaduje toorun v tomto příkladu.

## <a name="Scenario1"></a>Příklad 1: Serializace pomocí reflexe
Hello schéma JSON pro typy hello můžete být automaticky vytvořené hello Microsoft Avro Library prostřednictvím reflexe z dat hello kontrakt atributy hello C# objekty toobe serializovat. vytvoří technologie Hello Microsoft Avro Library [ **IAvroSeralizer<T>**  ](http://msdn.microsoft.com/library/dn627341.aspx) tooidentify hello pole toobe serializovat.

V tomto příkladu objekty ( **SensorData** se členem **umístění** struktura) jsou serializovaných tooa datový proud paměti a tento datový proud je zase deserializovat. Hello výsledkem je, pak porovná s toohello počáteční instance tooconfirm této hello **SensorData** objekt obnovit je identické toohello původní.

schéma Hello v tomto příkladu se předpokládá toobe sdílena mezi hello čtení a zápis, tak formát kontejneru objektů Avro hello se nevyžaduje. Pro příklad tooserialize a deserializaci dat do vyrovnávací paměti pomocí reflexe ve formátu kontejneru objektu hello, pokud hello schématu je potřeba sdílet s hello data, najdete v části <a href="#Scenario3">serializace objektu kontejneru soubory pomocí reflexe</a>.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //hello usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serialize and deserialize sample data set represented as an object using reflection.
            //No explicit schema definition is required - schema of serialized objects is automatically built.
            public void SerializeDeserializeObjectUsingReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION\n");
                Console.WriteLine("Serializing Sample Data Set...");

                //Create a new AvroSerializer instance and specify a custom serialization strategy AvroDataContractResolver
                //for serializing only properties attributed with DataContract/DateMember
                var avroSerializer = AvroSerializer.Create<SensorData>();

                //Create a memory stream buffer
                using (var buffer = new MemoryStream())
                {
                    //Create a data set by using sample class and struct
                    var expected = new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } };

                    //Serialize hello data toohello specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare hello stream for deserializing hello data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from hello stream and cast it toohello same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches hello original one
                    bool isEqual = this.Equal(expected, actual);

                    Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);

                }
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }



            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization toomemory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key tooexit.");
                Console.Read();
            }
        }
    }
    // hello example is expected toodisplay hello following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key tooexit.


## <a name="sample-2-serialization-with-a-generic-record"></a>Příklad 2: Serializace se záznamem obecné
Při reflexe nelze použít, protože není možné vyjádřit hello data pomocí tříd rozhraní .NET pomocí kontraktu dat, lze v záznamu obecné explicitně zadat schématu JSON. Tato metoda je nižší než pomocí reflexe. V takových případech hello schéma pro hello data může být také dynamické, tedy není známý v době kompilace. Data vyjádřené hodnot oddělených čárkami (CSV) soubory, jejichž schématu neznámý, dokud je transformovat formát Avro toohello v době běhu je příklad toto řazení dynamické scénáře.

Tento příklad ukazuje, jak toocreate a používat [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) tooexplicitly zadejte schématu JSON, jak toopopulate ji s daty hello a poté jak tooserialize a deserializaci. výsledek Hello se pak porovná tooconfirm toohello počáteční instance, která hello záznam obnovit se původní identické toohello.

schéma Hello v tomto příkladu se předpokládá toobe sdílena mezi hello čtení a zápis, tak formát kontejneru objektů Avro hello se nevyžaduje. Pro příklad tooserialize a deserializaci dat do vyrovnávací paměti pomocí záznam obecný formát kontejneru objektů hello při hello schématu musí být součástí hello serializovat data, najdete v části hello <a href="#Scenario4">serializace pomocí kontejner objektů soubory s obecné záznam</a> příklad.

    namespace Microsoft.Hadoop.Avro.Sample
    {
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.Serialization;
    using Microsoft.Hadoop.Avro.Container;
    using Microsoft.Hadoop.Avro.Schema;
    using Microsoft.Hadoop.Avro;

    //This class contains all methods demonstrating
    //hello usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with hello schema explicitly defined in JSON.
        //All serialized data should be mapped toohello fields of hello generic record,
        //which in turn is then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining hello Schema and creating Sample Data Set...");

            //Define hello schema in JSON
            const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

            //Create a generic serializer based on hello schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record toorepresent hello data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize hello data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize hello data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify hello results
                bool isEqual = expected.Location.Floor.Equals(actual.Location.Floor);
                isEqual = isEqual && expected.Location.Room.Equals(actual.Location.Room);
                isEqual = isEqual && ((byte[])expected.Value).SequenceEqual((byte[])actual.Value);
                Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);
            }
        }

        static void Main()
        {

            string sectionDivider = "---------------------------------------- ";

            //Create an instance of AvroSample class and invoke methods
            //illustrating different serializing approaches
            AvroSample Sample = new AvroSample();

            //Serialization toomemory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key tooexit.");
            Console.Read();
        }
    }
    }
    // hello example is expected toodisplay hello following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining hello Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key tooexit.


## <a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>Ukázka 3: Serializace soubory kontejneru objektů a serializace pomocí reflexe
Tato ukázka je podobném scénáři toohello v hello <a href="#Scenario1"> prvním příkladu</a>, kde je implicitně zadané schéma hello pomocí reflexe. Hello rozdílem je, že zde není hello schématu předpokládá toobe známé toohello čtečka, která deserializuje ho. Hello **SensorData** serializovat toobe objekty a jejich implicitně určené schéma uložené v souboru kontejneru objektu Avro reprezentována hello [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) Třída.

je v tomto příkladu se serializovat Hello data [ **SequentialWriter<SensorData>**  ](http://msdn.microsoft.com/library/dn627340.aspx) a deserializovat s [ **SequentialReader<SensorData>** ](http://msdn.microsoft.com/library/dn627340.aspx). výsledek Hello je porovnání toohello počáteční instancí tooensure identity.

Vítejte v hello objekt dat prostřednictvím výchozí hello je komprimovaný soubor kontejneru [ **Deflate** ] [ deflate-100] kompresní kodek z rozhraní .NET Framework 4. V tématu hello <a href="#Scenario5"> páté příklad</a> v této toolearn téma Jak toouse novější a vyšší verzi hello [ **Deflate** ] [ deflate-110] komprese kodek dostupný v rozhraní .NET Framework 4.5.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //hello usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes hello sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with hello Deflate codec.
            public void SerializeDeserializeUsingObjectContainersReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data toofile.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects toostream.
                    //Data is compressed using hello Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize hello data toostream by using hello sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream toofile
                    Console.WriteLine("Saving serialized data toofile...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare hello stream for deserializing hello data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from hello given stream.
                    //It allows iterating over hello deserialized objects because it implements hello IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches hello original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            bool isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual);
                            count++;
                        }
                    }
                }

                //Delete hello file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream tooa new file with hello given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("hello following exception was thrown during creation and writing toohello file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using hello given path tooa memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("hello following exception was thrown during reading from hello file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("hello following exception was thrown during deleting hello file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection tooAvro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key tooexit.");
                Console.Read();
            }
        }
    }
    // hello example is expected toodisplay hello following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data toofile...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key tooexit.


## <a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>Ukázka 4: Serializace pomocí obecné záznam soubory kontejneru objektů a serializace
Tato ukázka je podobném scénáři toohello v hello <a href="#Scenario2"> druhém příkladu</a>, kde je explicitně zadané schéma hello s JSON. Hello rozdílem je, že zde není hello schématu předpokládá toobe známé toohello čtečka, která deserializuje ho.

Hello testovací datové sady se shromažďují do seznamu [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objektů prostřednictvím explicitně definované schéma JSON a pak uloženy v objektu kontejneru souboru parametrem hello [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) třídy. Tento soubor kontejner vytvoří zapisovač, který je použité tooserialize hello data, nekomprimovaným, tooa datový proud paměti, který je pak uložený v souboru tooa. Hello [ **Codec.Null** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) parametr použitý k vytvoření čtečky hello Určuje, že není komprimována tato data.

Hello data se pak čtení ze souboru hello a deserializovat do kolekce objektů. Tato kolekce je porovnání toohello počáteční seznam tooconfirm Avro zaznamenává, jestli se shodují.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro.Schema;
        using Microsoft.Hadoop.Avro;

        //This class contains all methods demonstrating
        //hello usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining hello Schema and creating Sample Data Set...");

                //Define hello schema in JSON
                const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

                //Create a generic serializer based on hello schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record toorepresent hello data
                var testData = new List<AvroRecord>();

                dynamic expected1 = new AvroRecord(rootSchema);
                dynamic location1 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location1.Floor = 1;
                location1.Room = 243;
                expected1.Location = location1;
                expected1.Value = new byte[] { 1, 2, 3, 4, 5 };
                testData.Add(expected1);

                dynamic expected2 = new AvroRecord(rootSchema);
                dynamic location2 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location2.Floor = 1;
                location2.Room = 244;
                expected2.Location = location2;
                expected2.Value = new byte[] { 6, 7, 8, 9 };
                testData.Add(expected2);

                //Serializing and saving data toofile.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects toostream.
                    //Data is not compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize hello data toostream by using hello sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data toofile...");

                    //Save stream toofile
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing hello data.
                //Create a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare hello stream for deserializing hello data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from hello given stream.
                    //It allows iterating over hello deserialized objects because it implements hello IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify hello results
                            var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = (dynamic)serialized, actual = (dynamic)deserialized });
                            int count = 1;
                            foreach (var pair in pairs)
                            {
                                bool isEqual = pair.expected.Location.Floor.Equals(pair.actual.Location.Floor);
                                isEqual = isEqual && pair.expected.Location.Room.Equals(pair.actual.Location.Room);
                                isEqual = isEqual && ((byte[])pair.expected.Value).SequenceEqual((byte[])pair.actual.Value);
                                Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                                count++;
                            }
                        }
                    }
                }

                //Delete hello file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream tooa new file with hello given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("hello following exception was thrown during creation and writing toohello file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using hello given path tooa memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("hello following exception was thrown during reading from hello file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using hello given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("hello following exception was thrown during deleting hello file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of hello AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record tooAvro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key tooexit.");
                Console.Read();
            }
        }
    }
    // hello example is expected toodisplay hello following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining hello Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data toofile...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key tooexit.




## <a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Ukázka 5: Serializace objektu kontejneru soubory pomocí vlastní kompresní kodek
Hello páté příklad ukazuje způsob toouse vlastní kompresní kodek pro Avro objektu kontejneru soubory. Ukázka obsahující hello kód pro tento příklad si můžete stáhnout z hello [ukázky kódu Azure](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) lokality.

Hello [Avro Specification](http://avro.apache.org/docs/current/spec.html#Required+Codecs) umožňuje využití používá volitelné kompresní kodek (kromě příliš**Null** a **Deflate** výchozí nastavení). V tomto příkladu není implementace nové kodeků například Snappy (uvedené jako podporované volitelné kodeků v hello [Avro Specification](http://avro.apache.org/docs/current/spec.html#snappy)). Ukazuje, jak toouse hello implementace rozhraní .NET Framework 4.5 hello [ **Deflate** ] [ deflate-110] kodeků, které poskytuje lepší algoritmus komprese podle hello [zlib](http://zlib.net/) knihovnu komprese než verze rozhraní .NET Framework 4 výchozí hello.

    //
    // This code needs toobe compiled with hello parameter Target Framework set as ".NET Framework 4.5"
    // tooensure hello desired implementation of hello Deflate compression algorithm is used.
    // Ensure your C# project is set up accordingly.
    //

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.IO;
        using System.IO.Compression;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        #region Defining objects for serialization
        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }
        #endregion

        #region Defining custom codec based on .NET Framework V.4.5 Deflate
        //Avro.NET codec class contains two methods,
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed),
        //which are hello key ones for data compression.
        //tooenable a custom codec, one needs tooimplement these methods for hello required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs tooimplement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed toobe null.");

                this.compressionStream = new DeflateStream(buffer, CompressionLevel.Fastest, true);
                this.buffer = buffer;
            }

            public override bool CanRead
            {
                get { return this.buffer.CanRead; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return this.buffer.CanWrite; }
            }

            public override void Flush()
            {
                this.compressionStream.Close();
            }

            public override long Length
            {
                get { return this.buffer.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.buffer.Position;
                }

                set
                {
                    this.buffer.Position = value;
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.buffer.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                return this.buffer.Seek(offset, origin);
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                this.compressionStream.Write(buffer, offset, count);
            }

            protected override void Dispose(bool disposed)
            {
                base.Dispose(disposed);

                if (disposed)
                {
                    this.compressionStream.Dispose();
                    this.compressionStream = null;
                }
            }
        }

        internal sealed class DecompressionStreamDeflate45 : Stream
        {
            private readonly DeflateStream decompressed;

            public DecompressionStreamDeflate45(Stream compressed)
            {
                this.decompressed = new DeflateStream(compressed, CompressionMode.Decompress, true);
            }

            public override bool CanRead
            {
                get { return true; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return false; }
            }

            public override void Flush()
            {
                this.decompressed.Close();
            }

            public override long Length
            {
                get { return this.decompressed.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.decompressed.Position;
                }

                set
                {
                    throw new NotSupportedException();
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.decompressed.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                throw new NotSupportedException();
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                throw new NotSupportedException();
            }

            protected override void Dispose(bool disposing)
            {
                base.Dispose(disposing);

                if (disposing)
                {
                    this.decompressed.Dispose();
                }
            }
        }
        #endregion

        #region Define Codec
        //Define hello actual codec class containing hello required methods for manipulating streams:
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed).
        //Codec class uses classes for compressed and decompressed streams defined above.
        internal sealed class DeflateCodec45 : Codec
        {

            //We merely use different IMPLEMENTATIONS of Deflate, so CodecName remains "deflate"
            public static readonly string CodecName = "deflate";

            public DeflateCodec45()
                : base(CodecName)
            {
            }

            public override Stream GetCompressedStreamOver(Stream decompressed)
            {
                if (decompressed == null)
                {
                    throw new ArgumentNullException("decompressed");
                }

                return new CompressionStreamDeflate45(decompressed);
            }

            public override Stream GetDecompressedStreamOver(Stream compressed)
            {
                if (compressed == null)
                {
                    throw new ArgumentNullException("compressed");
                }

                return new DecompressionStreamDeflate45(compressed);
            }
        }
        #endregion

        #region Define modified Codec Factory
        //Define modified codec factory toobe used in hello reader.
        //It catches hello attempt toouse "Deflate" and provide  a custom codec.
        //For all other cases, it relies on hello base class (CodecFactory).
        internal sealed class CodecFactoryDeflate45 : CodecFactory
        {

            public override Codec Create(string codecName)
            {
                if (codecName == DeflateCodec45.CodecName)
                    return new DeflateCodec45();
                else
                    return base.Create(codecName);
            }
        }
        #endregion

        #endregion

        #region Sample Class with demonstration methods
        //This class contains methods demonstrating
        //hello usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with hello custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related tooserialization, deserialization and
            //object container manipulation, though file stream could be easily used.
            public void SerializeDeserializeUsingObjectContainersReflectionCustomCodec()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate45.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data toofile.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects toostream.
                    //Here hello custom codec is introduced. For convenience, hello next commented code line shows how toouse built-in Deflate.
                    //Note that because hello sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize hello data toostream using hello sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream toofile
                    Console.WriteLine("Saving serialized data toofile...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare hello stream for deserializing hello data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment hello line below if you want toouse built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from hello given stream.
                    //It allows iterating over hello deserialized objects because it implements hello IEnumerable<T> interface.
                    //Here hello custom codec factory is introduced.
                    //For convenience, hello next commented code line shows how toouse built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because hello sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches hello original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        bool isEqual;
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                            count++;
                        }
                    }
                }

                //Delete hello file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream tooa new file with hello given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("hello following exception was thrown during creation and writing toohello file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading file content by using hello given path tooa memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("hello following exception was thrown during reading from hello file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("hello following exception was thrown during deleting hello file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }
            #endregion

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection tooAvro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key tooexit.");
                Console.Read();
            }
        }
    }
    // hello example is expected toodisplay hello following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data toofile...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key tooexit.

## <a name="sample-6-using-avro-tooupload-data-for-hello-microsoft-azure-hdinsight-service"></a>Ukázka 6: Používající Avro tooupload dat pro hello služby Microsoft Azure HDInsight
Hello šesté příklad ukazuje některé programovací techniky související toointeracting pomocí služby Azure HDInsight hello. Ukázka obsahující hello kód pro tento příklad si můžete stáhnout z hello [ukázky kódu Azure](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) lokality.

Ukázka Hello hello následující úlohy:

* Připojí tooan stávajícího clusteru služby HDInsight.
* Serializuje několik souborů CSV a odesílá úložiště objektů Blob tooAzure výsledek hello. (soubory CSV hello distribuují společně s hello ukázka a představují výpis z příloze Stock historická data distribuovat [Infochimps](http://www.infochimps.com/) dobu hello pod hodnotou 1970 2010. Ukázka Hello čte data souboru CSV, převede hello tooinstances záznamy o hello **Stock** třídy a pak je serializuje pomocí reflexe. Definice uložených typu je vytvořený z schématu JSON prostřednictvím hello nástroj pro generování kódu Microsoft Avro Library.
* Vytvoří novou tabulku externí názvem **akcií** v Hive a propojí ji toohello data odeslali v předchozím kroku hello.
* Provede dotaz pomocí Hive prostřednictvím hello **akcií** tabulky.

Kromě toho hello ukázka provádí postup pro čištění před a po provedení hlavních operací. Během hello čištění všechny hello související data objektů Blob v Azure a složky, se odeberou a tabulku Hive hello je odstranit. Můžete také vyvolat hello postup vyčištění z hello ukázka příkazového řádku.

Ukázka Hello má hello následující požadavky:

* Aktivní předplatné Microsoft Azure a jeho ID předplatného.
* Certifikát správy pro předplatné hello s hello odpovídající privátní klíč. Hello certifikátu musí být nainstalován v hello aktuální privátní úložiště uživatele na hello počítači toorun hello vzorku.
* Aktivní clusteru HDInsight.
* Účet úložiště Azure propojené toohello clusteru HDInsight z předchozí součásti hello, společně s hello odpovídající primární nebo sekundární přístupový klíč.

Před spuštěním hello ukázka všechny hello informace z požadovaných součástí hello musí být zadaná toohello vzorový konfigurační soubor. Existují dvě možné způsoby, jak toodo ho:

* Upravte soubor app.config hello v hello ukázka kořenový adresář a následně vytvořit ukázkové hello
* Nejprve sestavení ukázkový text hello a pak upravte AvroHDISample.exe.config v adresáři hello sestavení

V obou případech by mělo být provedeno všechny úpravy v hello  **<appSettings>**  v oddílu nastavení. Postupujte podle hello komentáře v souboru hello.
Hello ukázka spuštění z příkazového řádku hello spuštěním hello následující příkaz (kde hello soubor .zip s ukázkou hello se předpokládá, že tooC:\AvroHDISample toobe extrahovat; Pokud hello relevantní, jinak použijte cesta k souboru):

    AvroHDISample run C:\AvroHDISample\Data

tooclean až hello clusteru, spusťte následující příkaz hello:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
