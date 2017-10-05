---
title: "Serializaci dat ve službě Azure - Microsoft Avro Library - Hadoop | Microsoft Docs"
description: "Zjistěte, jak k serializaci a deserializaci dat v Hadoop v HDInsight pomocí Microsoft Avro Library k uchovávání paměť, databáze nebo souboru."
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
ms.openlocfilehash: d06bf8ff4a21e4f4b29593bac32bfa2b32601fc4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a><span data-ttu-id="07db5-104">Serializovat data v Hadoop pomocí Microsoft Avro Library</span><span class="sxs-lookup"><span data-stu-id="07db5-104">Serialize data in Hadoop with the Microsoft Avro Library</span></span>

>[!NOTE]
><span data-ttu-id="07db5-105">Avro SDK je podporován společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="07db5-105">The Avro SDK is no longer supported by Microsoft.</span></span> <span data-ttu-id="07db5-106">Knihovna je podporován komunity s otevřeným zdrojem.</span><span class="sxs-lookup"><span data-stu-id="07db5-106">The library is open source community supported.</span></span> <span data-ttu-id="07db5-107">Zdroje pro knihovnu jsou umístěné v [Githubu](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span><span class="sxs-lookup"><span data-stu-id="07db5-107">The sources for the library are located in [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span></span>

<span data-ttu-id="07db5-108">Toto téma ukazuje, jak používat [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) k serializaci objektů a jiné datové struktury do datových proudů, aby je uchoval do paměti, databáze nebo souboru.</span><span class="sxs-lookup"><span data-stu-id="07db5-108">This topic shows how to use the [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) to serialize objects and other data structures into streams to persist them to memory, a database, or a file.</span></span> <span data-ttu-id="07db5-109">Také ukazuje, jak k deserializaci je obnovit původní objekty.</span><span class="sxs-lookup"><span data-stu-id="07db5-109">It also shows how to deserialize them to recover the original objects.</span></span>

[!INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

## <a name="apache-avro"></a><span data-ttu-id="07db5-110">Apache Avro</span><span class="sxs-lookup"><span data-stu-id="07db5-110">Apache Avro</span></span>
<span data-ttu-id="07db5-111"><a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> implementuje Apache Avro systému serializace dat pro prostředí rozhraní Microsoft.NET.</span><span class="sxs-lookup"><span data-stu-id="07db5-111">The <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> implements the Apache Avro data serialization system for the Microsoft.NET environment.</span></span> <span data-ttu-id="07db5-112">Apache Avro poskytuje formát pro výměnu kompaktní binární data pro serializaci.</span><span class="sxs-lookup"><span data-stu-id="07db5-112">Apache Avro provides a compact binary data interchange format for serialization.</span></span> <span data-ttu-id="07db5-113">Používá <a href="http://www.json.org" target="_blank">JSON</a> k definování jazykově nezávislého schématu, které poskytuje vzájemnou vzájemná funkční spolupráce jazyka.</span><span class="sxs-lookup"><span data-stu-id="07db5-113">It uses <a href="http://www.json.org" target="_blank">JSON</a> to define a language-agnostic schema that underwrites language interoperability.</span></span> <span data-ttu-id="07db5-114">Data serializovaná v jednom jazyce lze číst v jiném.</span><span class="sxs-lookup"><span data-stu-id="07db5-114">Data serialized in one language can be read in another.</span></span> <span data-ttu-id="07db5-115">Aktuálně jsou podporované C, C++, C#, Java, PHP, Python a Ruby.</span><span class="sxs-lookup"><span data-stu-id="07db5-115">Currently C, C++, C#, Java, PHP, Python, and Ruby are supported.</span></span> <span data-ttu-id="07db5-116">Podrobné informace o formátu lze nalézt v <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro Specification</a>.</span><span class="sxs-lookup"><span data-stu-id="07db5-116">Detailed information on the format can be found in the <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro Specification</a>.</span></span> 

>[!NOTE]
><span data-ttu-id="07db5-117">Microsoft Avro Library nepodporuje vzdálené procedury volání (RPC) součástí téhle specifikaci.</span><span class="sxs-lookup"><span data-stu-id="07db5-117">The Microsoft Avro Library does not support the remote procedure calls (RPCs) part of this specification.</span></span>
>

<span data-ttu-id="07db5-118">Serializovaná reprezentace objektu v systému Avro se skládá ze dvou částí: schéma a skutečnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="07db5-118">The serialized representation of an object in the Avro system consists of two parts: schema and actual value.</span></span> <span data-ttu-id="07db5-119">Schématu Avro popisuje model nezávislé na jazyku data serializovaná data s JSON.</span><span class="sxs-lookup"><span data-stu-id="07db5-119">The Avro schema describes the language-independent data model of the serialized data with JSON.</span></span> <span data-ttu-id="07db5-120">Zobrazí se node souběžně s binární reprezentace data.</span><span class="sxs-lookup"><span data-stu-id="07db5-120">It is presented side by side with a binary representation of data.</span></span> <span data-ttu-id="07db5-121">Každý objekt k zapsání pomocí žádné režijní náklady na hodnotu, provedení serializace rychlé a reprezentaci malé s odděleně od binární reprezentace schématu umožňuje.</span><span class="sxs-lookup"><span data-stu-id="07db5-121">Having the schema separate from the binary representation permits each object to be written with no per-value overheads, making serialization fast, and the representation small.</span></span>

## <a name="the-hadoop-scenario"></a><span data-ttu-id="07db5-122">Scénář Hadoop</span><span class="sxs-lookup"><span data-stu-id="07db5-122">The Hadoop scenario</span></span>
<span data-ttu-id="07db5-123">V Azure HDInsight a dalších prostředích s Apache Hadoop se často používá formát serializace Apache Avro.</span><span class="sxs-lookup"><span data-stu-id="07db5-123">The Apache Avro serialization format is widely used in Azure HDInsight and other Apache Hadoop environments.</span></span> <span data-ttu-id="07db5-124">Avro nabízí pohodlný způsob, jak představují komplexní datové struktury v rámci úlohy Hadoop MapReduce.</span><span class="sxs-lookup"><span data-stu-id="07db5-124">Avro provides a convenient way to represent complex data structures within a Hadoop MapReduce job.</span></span> <span data-ttu-id="07db5-125">Formát Avro soubory (soubor kontejneru objektu Avro) má byly navrženy pro podporu Distribuovaný programovací model MapReduce.</span><span class="sxs-lookup"><span data-stu-id="07db5-125">The format of Avro files (Avro object container file) has been designed to support the distributed MapReduce programming model.</span></span> <span data-ttu-id="07db5-126">Klíčové funkce, která umožní, že distribuce je, že soubory jsou "rozdělitelné" v tom smyslu, že jeden vyhledat libovolný bod v souboru a začít číst od určitého bloku.</span><span class="sxs-lookup"><span data-stu-id="07db5-126">The key feature that enables the distribution is that the files are “splittable” in the sense that one can seek any point in a file and start reading from a particular block.</span></span>

## <a name="serialization-in-avro-library"></a><span data-ttu-id="07db5-127">Serializace v knihovně Avro</span><span class="sxs-lookup"><span data-stu-id="07db5-127">Serialization in Avro Library</span></span>
<span data-ttu-id="07db5-128">Knihovny .NET pro Avro podporuje dva způsoby serializaci objektů:</span><span class="sxs-lookup"><span data-stu-id="07db5-128">The .NET Library for Avro supports two ways of serializing objects:</span></span>

* <span data-ttu-id="07db5-129">**reflexe** – schéma JSON pro typy automaticky vychází z dat kontrakt atributy typy .NET, které mají být serializován.</span><span class="sxs-lookup"><span data-stu-id="07db5-129">**reflection** - The JSON schema for the types is automatically built from the data contract attributes of the .NET types to be serialized.</span></span>
* <span data-ttu-id="07db5-130">**Obecné záznam** -schématu A JSON je explicitně určena v záznamu reprezentována [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) třídy, pokud žádné typy .NET existuje popisují schéma pro data, která mají být serializován.</span><span class="sxs-lookup"><span data-stu-id="07db5-130">**generic record** - A JSON schema is explicitly specified in a record represented by the [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) class when no .NET types are present to describe the schema for the data to be serialized.</span></span>

<span data-ttu-id="07db5-131">Pokud schéma dat je znám zapisovače i čtečku datového proudu, data lze odeslat bez jeho schématu.</span><span class="sxs-lookup"><span data-stu-id="07db5-131">When the data schema is known to both the writer and reader of the stream, the data can be sent without its schema.</span></span> <span data-ttu-id="07db5-132">V případech, pokud se používá soubor Avro objektu kontejneru, schéma je uloženo v souboru.</span><span class="sxs-lookup"><span data-stu-id="07db5-132">In cases when an Avro object container file is used, the schema is stored within the file.</span></span> <span data-ttu-id="07db5-133">Další parametry, například kodek použitý pro kompresi dat lze zadat.</span><span class="sxs-lookup"><span data-stu-id="07db5-133">Other parameters, such as the codec used for data compression, can be specified.</span></span> <span data-ttu-id="07db5-134">Tyto scénáře jsou popsané v další podrobnosti a ukazuje následující příklady kódu:</span><span class="sxs-lookup"><span data-stu-id="07db5-134">These scenarios are outlined in more detail and illustrated in the following code examples:</span></span>

## <a name="install-avro-library"></a><span data-ttu-id="07db5-135">Nainstaluje Avro knihovny</span><span class="sxs-lookup"><span data-stu-id="07db5-135">Install Avro Library</span></span>
<span data-ttu-id="07db5-136">Je potřeba splnit následující před instalací knihovny:</span><span class="sxs-lookup"><span data-stu-id="07db5-136">The following are required before you install the library:</span></span>

* <span data-ttu-id="07db5-137"><a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Rozhraní Microsoft .NET Framework 4</a></span><span class="sxs-lookup"><span data-stu-id="07db5-137"><a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a></span></span>
* <span data-ttu-id="07db5-138"><a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="07db5-138"><a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 or later)</span></span>

<span data-ttu-id="07db5-139">Všimněte si, že závislost Newtonsoft.Json.dll automaticky stažena v instalaci Microsoft Avro Library.</span><span class="sxs-lookup"><span data-stu-id="07db5-139">Note that the Newtonsoft.Json.dll dependency is downloaded automatically with the installation of the Microsoft Avro Library.</span></span> <span data-ttu-id="07db5-140">Postup je uvedené v následující části:</span><span class="sxs-lookup"><span data-stu-id="07db5-140">The procedure is provided in the following section:</span></span>

<span data-ttu-id="07db5-141">Microsoft Avro Library je distribuován jako balíčku NuGet, který lze nainstalovat ze sady Visual Studio pomocí následujícího postupu:</span><span class="sxs-lookup"><span data-stu-id="07db5-141">The Microsoft Avro Library is distributed as a NuGet package that can be installed from Visual Studio via the following procedure:</span></span>

1. <span data-ttu-id="07db5-142">Vyberte **projektu** -> karta **spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="07db5-142">Select the **Project** tab -> **Manage NuGet Packages...**</span></span>
2. <span data-ttu-id="07db5-143">Vyhledejte "Microsoft.Hadoop.Avro" v **Online hledání** pole.</span><span class="sxs-lookup"><span data-stu-id="07db5-143">Search for "Microsoft.Hadoop.Avro" in the **Search Online** box.</span></span>
3. <span data-ttu-id="07db5-144">Klikněte **nainstalovat** vedle položky **knihovnu Microsoft Azure HDInsight Avro**.</span><span class="sxs-lookup"><span data-stu-id="07db5-144">Click the **Install** button next to **Microsoft Azure HDInsight Avro Library**.</span></span>

<span data-ttu-id="07db5-145">Všimněte si, že Newtonsoft.Json.dll (> = 6.0.4) závislostí je také automaticky staženy s Microsoft Avro Library.</span><span class="sxs-lookup"><span data-stu-id="07db5-145">Note that the Newtonsoft.Json.dll (>=6.0.4) dependency is also downloaded automatically with the Microsoft Avro Library.</span></span>

<span data-ttu-id="07db5-146">Microsoft Avro Library zdrojový kód je k dispozici na [Githubu](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span><span class="sxs-lookup"><span data-stu-id="07db5-146">The Microsoft Avro Library source code is available at [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span></span>

## <a name="compile-schemas-using-avro-library"></a><span data-ttu-id="07db5-147">Kompilace pomocí knihovny Avro schémata</span><span class="sxs-lookup"><span data-stu-id="07db5-147">Compile schemas using Avro Library</span></span>
<span data-ttu-id="07db5-148">Microsoft Avro Library obsahuje nástroj pro generování kódu, který umožňuje vytváření typy C# automaticky v závislosti na dříve definované schéma JSON.</span><span class="sxs-lookup"><span data-stu-id="07db5-148">The Microsoft Avro Library contains a code generation utility that allows creating C# types automatically based on the previously defined JSON schema.</span></span> <span data-ttu-id="07db5-149">Nástroj pro generování kódu není distribuován jako binární spustitelný soubor, ale je možné snadno sestavit pomocí následujícího postupu:</span><span class="sxs-lookup"><span data-stu-id="07db5-149">The code generation utility is not distributed as a binary executable, but can be easily built via the following procedure:</span></span>

1. <span data-ttu-id="07db5-150">Stáhněte si soubor .zip na nejnovější verzi sady SDK HDInsight zdrojového kódu z <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK pro Hadoop</a>.</span><span class="sxs-lookup"><span data-stu-id="07db5-150">Download the .zip file with the latest version of HDInsight SDK source code from <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK For Hadoop</a>.</span></span> <span data-ttu-id="07db5-151">(Klikněte na tlačítko **Stáhnout** ikonu, není **stáhne** karta.)</span><span class="sxs-lookup"><span data-stu-id="07db5-151">(Click the **Download** icon, not the **Downloads** tab.)</span></span>
2. <span data-ttu-id="07db5-152">Extrahujte sady SDK HDInsight do adresáře na počítači s rozhraní .NET Framework 4 nainstalovaný a připojený k Internetu pro stahování balíčků NuGet potřebné závislosti.</span><span class="sxs-lookup"><span data-stu-id="07db5-152">Extract the HDInsight SDK to a directory on the machine with .NET Framework 4 installed and connected to the Internet for downloading necessary dependency NuGet packages.</span></span> <span data-ttu-id="07db5-153">Níže předpokládáme, že zdrojový kód je extrahovány do C:\SDK.</span><span class="sxs-lookup"><span data-stu-id="07db5-153">Below, we assume that the source code is extracted to C:\SDK.</span></span>
3. <span data-ttu-id="07db5-154">Přejděte do složky C:\SDK\src\Microsoft.Hadoop.Avro.Tools a spusťte build.bat.</span><span class="sxs-lookup"><span data-stu-id="07db5-154">Go to the folder C:\SDK\src\Microsoft.Hadoop.Avro.Tools and run build.bat.</span></span> <span data-ttu-id="07db5-155">(Soubor volá MSBuild z distribučního 32bitová verze rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="07db5-155">(The file calls MSBuild from the 32-bit distribution of the .NET Framework.</span></span> <span data-ttu-id="07db5-156">Pokud chcete použít 64bitová verze, upravte build.bat, následující komentáře v souboru.) Ujistěte se, že sestavení byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="07db5-156">If you would like to use the 64-bit version, edit build.bat, following the comments inside the file.) Ensure that the build is successful.</span></span> <span data-ttu-id="07db5-157">(Na některých systémů MSBuild může vytvořit upozornění.</span><span class="sxs-lookup"><span data-stu-id="07db5-157">(On some systems, MSBuild may produce warnings.</span></span> <span data-ttu-id="07db5-158">Tato upozornění nemají vliv na nástroj, dokud nedojde k žádným chybám sestavení.)</span><span class="sxs-lookup"><span data-stu-id="07db5-158">These warnings do not affect the utility as long as there are no build errors.)</span></span>
4. <span data-ttu-id="07db5-159">Kompilované nástroj se nachází v C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.</span><span class="sxs-lookup"><span data-stu-id="07db5-159">The compiled utility is located in C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.</span></span>

<span data-ttu-id="07db5-160">Abyste se seznámili s syntaxe příkazového řádku, spusťte následující příkaz ze složky, kde se nachází nástroj pro generování kódu:`Microsoft.Hadoop.Avro.Tools help /c:codegen`</span><span class="sxs-lookup"><span data-stu-id="07db5-160">To get familiar with the command-line syntax, execute the following command from the folder where the code generation utility is located: `Microsoft.Hadoop.Avro.Tools help /c:codegen`</span></span>

<span data-ttu-id="07db5-161">K testování nástroj, můžete vygenerovat třídy jazyka C# ze souboru schématu JSON ukázka součástí zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="07db5-161">To test the utility, you can generate C# classes from the sample JSON schema file provided with the source code.</span></span> <span data-ttu-id="07db5-162">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="07db5-162">Execute the following command:</span></span>

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

<span data-ttu-id="07db5-163">To by měl vytvořit dvě C# soubory v aktuálním adresáři: SensorData.cs a Location.cs.</span><span class="sxs-lookup"><span data-stu-id="07db5-163">This is supposed to produce two C# files in the current directory: SensorData.cs and Location.cs.</span></span>

<span data-ttu-id="07db5-164">Abyste pochopili logiky, která používá nástroj pro generování kódu při převodu schéma JSON pro typy C#, naleznete v souboru GenerationVerification.feature umístěný v C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.</span><span class="sxs-lookup"><span data-stu-id="07db5-164">To understand the logic that the code generation utility is using while converting the JSON schema to C# types, see the file GenerationVerification.feature located in C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.</span></span>

<span data-ttu-id="07db5-165">Obory názvů se extrahují z schématu JSON pomocí logiku popsané v souboru uveden v předchozím odstavci.</span><span class="sxs-lookup"><span data-stu-id="07db5-165">Namespaces are extracted from the JSON schema, using the logic described in the file mentioned in the previous paragraph.</span></span> <span data-ttu-id="07db5-166">Obory názvů extrahována ze schématu mají přednost před ať se poskytuje s parametrem/n v příkazovém řádku nástroj.</span><span class="sxs-lookup"><span data-stu-id="07db5-166">Namespaces extracted from the schema take precedence over whatever is provided with the /n parameter in the utility command line.</span></span> <span data-ttu-id="07db5-167">Pokud chcete přepsat obory názvů obsažené ve schématu, použijte parametr /nf.</span><span class="sxs-lookup"><span data-stu-id="07db5-167">If you want to override the namespaces contained within the schema, use the /nf parameter.</span></span> <span data-ttu-id="07db5-168">Například pokud chcete změnit všechny obory názvů z SampleJSONSchema.avsc my.own.nspace, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="07db5-168">For example, to change all namespaces from the SampleJSONSchema.avsc to my.own.nspace, execute the following command:</span></span>

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="about-the-samples"></a><span data-ttu-id="07db5-169">O ukázky</span><span class="sxs-lookup"><span data-stu-id="07db5-169">About the samples</span></span>
<span data-ttu-id="07db5-170">Šest příklady uvedené v tomto tématu ilustrují Microsoft Avro Library podporuje různé scénáře.</span><span class="sxs-lookup"><span data-stu-id="07db5-170">Six examples provided in this topic illustrate different scenarios supported by the Microsoft Avro Library.</span></span> <span data-ttu-id="07db5-171">Microsoft Avro Library je navržen pro práci s žádné datového proudu.</span><span class="sxs-lookup"><span data-stu-id="07db5-171">The Microsoft Avro Library is designed to work with any stream.</span></span> <span data-ttu-id="07db5-172">V těchto příkladech je upravit data prostřednictvím datových proudů paměti místo souborů datových proudů nebo databází kvůli jednoduchosti a přehlednosti.</span><span class="sxs-lookup"><span data-stu-id="07db5-172">In these examples, data is manipulated via memory streams rather than file streams or databases for simplicity and consistency.</span></span> <span data-ttu-id="07db5-173">Přístup použitý v produkčním prostředí závisí na požadavky na přesný scénář, zdroj dat a svazek, omezení výkonu a dalších faktorů.</span><span class="sxs-lookup"><span data-stu-id="07db5-173">The approach taken in a production environment depends on the exact scenario requirements, data source and volume, performance constraints, and other factors.</span></span>

<span data-ttu-id="07db5-174">První dva příklady ukazují, jak k serializaci a deserializaci dat do vyrovnávací paměti datového proudu pomocí reflexe a obecné záznamy.</span><span class="sxs-lookup"><span data-stu-id="07db5-174">The first two examples show how to serialize and deserialize data into memory stream buffers by using reflection and generic records.</span></span> <span data-ttu-id="07db5-175">Schéma v obou případech se předpokládá, že se dělí čtení a zápis out-of-band.</span><span class="sxs-lookup"><span data-stu-id="07db5-175">The schema in these two cases is assumed to be shared between the readers and writers out-of-band.</span></span>

<span data-ttu-id="07db5-176">Třetí a čtvrtý příklady ukazují, jak k serializaci a deserializaci dat pomocí souborů Avro objektu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="07db5-176">The third and fourth examples show how to serialize and deserialize data by using the Avro object container files.</span></span> <span data-ttu-id="07db5-177">Pokud data uložená v souboru Avro kontejneru, jeho schématu je vždy uložit s ním, protože schéma musí být sdílená pro deserializaci.</span><span class="sxs-lookup"><span data-stu-id="07db5-177">When data is stored in an Avro container file, its schema is always stored with it because the schema must be shared for deserialization.</span></span>

<span data-ttu-id="07db5-178">Vzorku, který obsahuje první čtyři příklady si můžete stáhnout z <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">ukázky kódu Azure</a> lokality.</span><span class="sxs-lookup"><span data-stu-id="07db5-178">The sample containing the first four examples can be downloaded from the <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="07db5-179">Páté příklad ukazuje, jak používat vlastní kompresní kodek pro soubory kontejneru objektů Avro.</span><span class="sxs-lookup"><span data-stu-id="07db5-179">The fifth example shows how to use a custom compression codec for Avro object container files.</span></span> <span data-ttu-id="07db5-180">Ukázka obsahující kód pro tento příklad si můžete stáhnout z <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">ukázky kódu Azure</a> lokality.</span><span class="sxs-lookup"><span data-stu-id="07db5-180">A sample containing the code for this example can be downloaded from the <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="07db5-181">Šesté příklad ukazuje použití Avro serializace k odesílání dat do úložiště objektů Blob v Azure a analyzujte ji pomocí clusteru služby HDInsight (Hadoop) Hive.</span><span class="sxs-lookup"><span data-stu-id="07db5-181">The sixth sample shows how to use Avro serialization to upload data to Azure Blob storage and then analyze it by using Hive with an HDInsight (Hadoop) cluster.</span></span> <span data-ttu-id="07db5-182">Lze ji stáhnout z <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">ukázky kódu Azure</a> lokality.</span><span class="sxs-lookup"><span data-stu-id="07db5-182">It can be downloaded from the <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="07db5-183">Tady jsou odkazy na šesti ukázky popsané v tématu:</span><span class="sxs-lookup"><span data-stu-id="07db5-183">Here are links to the six samples discussed in the topic:</span></span>

* <span data-ttu-id="07db5-184"><a href="#Scenario1">**Serializace pomocí reflexe** </a> – schéma JSON pro typy k serializaci automaticky vychází z dat kontrakt atributy.</span><span class="sxs-lookup"><span data-stu-id="07db5-184"><a href="#Scenario1">**Serialization with reflection**</a> - The JSON schema for types to be serialized is automatically built from the data contract attributes.</span></span>
* <span data-ttu-id="07db5-185"><a href="#Scenario2">**Serializace s obecné záznam** </a> -schématu JSON je explicitně určena v záznamu, když je k dispozici pro reflexi žádný typ rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="07db5-185"><a href="#Scenario2">**Serialization with generic record**</a> - The JSON schema is explicitly specified in a record when no .NET type is available for reflection.</span></span>
* <span data-ttu-id="07db5-186"><a href="#Scenario3">**Serializace objektu kontejneru soubory pomocí reflexe** </a> -schématu JSON je automaticky vytvořen a sdílet společně s serializovaná data prostřednictvím soubor Avro objektu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="07db5-186"><a href="#Scenario3">**Serialization using object container files with reflection**</a> - The JSON schema is automatically built and shared together with the serialized data via an Avro object container file.</span></span>
* <span data-ttu-id="07db5-187"><a href="#Scenario4">**Serializace objektu kontejneru soubory pomocí obecné záznam** </a> -schématu JSON jsou explicitně zadaný před serializaci a sdílet společně s dat přes soubor Avro objektu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="07db5-187"><a href="#Scenario4">**Serialization using object container files with generic record**</a> - The JSON schema is explicitly specified before the serialization and shared together with the data via an Avro object container file.</span></span>
* <span data-ttu-id="07db5-188"><a href="#Scenario5">**Serializace objektu kontejneru soubory pomocí vlastní kompresní kodek** </a> – příklad ukazuje, jak vytvořit soubor Avro objektu kontejneru s vlastní .NET provádění kodeků komprese dat Deflate.</span><span class="sxs-lookup"><span data-stu-id="07db5-188"><a href="#Scenario5">**Serialization using object container files with a custom compression codec**</a> - The example shows how to create an Avro object container file with a customized .NET implementation of the Deflate data compression codec.</span></span>
* <span data-ttu-id="07db5-189"><a href="#Scenario6">**Nahrání dat do služby Microsoft Azure HDInsight pomocí Avro** </a> – příklad ukazuje, jak Avro serializace komunikuje se službou HDInsight.</span><span class="sxs-lookup"><span data-stu-id="07db5-189"><a href="#Scenario6">**Using Avro to upload data for the Microsoft Azure HDInsight service**</a> - The example illustrates how Avro serialization interacts with the HDInsight service.</span></span> <span data-ttu-id="07db5-190">Aktivní předplatné Azure a přístup ke clusteru Azure HDInsight jsou nutné ke spuštění v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="07db5-190">An active Azure subscription and access to an Azure HDInsight cluster are required to run this example.</span></span>

## <span data-ttu-id="07db5-191"><a name="Scenario1"></a>Příklad 1: Serializace pomocí reflexe</span><span class="sxs-lookup"><span data-stu-id="07db5-191"><a name="Scenario1"></a>Sample 1: Serialization with reflection</span></span>
<span data-ttu-id="07db5-192">Schéma JSON pro typy můžete být automaticky vytvořené Microsoft Library Avro prostřednictvím reflexe z dat kontrakt atributy objektů jazyka C# k serializaci.</span><span class="sxs-lookup"><span data-stu-id="07db5-192">The JSON schema for the types can be automatically built by the Microsoft Avro Library via reflection from the data contract attributes of the C# objects to be serialized.</span></span> <span data-ttu-id="07db5-193">Vytvoří Microsoft Avro Library [ **IAvroSeralizer<T>**  ](http://msdn.microsoft.com/library/dn627341.aspx) k identifikaci pole k serializaci.</span><span class="sxs-lookup"><span data-stu-id="07db5-193">The Microsoft Avro Library creates an [**IAvroSeralizer<T>**](http://msdn.microsoft.com/library/dn627341.aspx) to identify the fields to be serialized.</span></span>

<span data-ttu-id="07db5-194">V tomto příkladu objekty ( **SensorData** se členem **umístění** struktura) jsou serializovat na datový proud paměti a tento datový proud je zase deserializovat.</span><span class="sxs-lookup"><span data-stu-id="07db5-194">In this example, objects (a **SensorData** class with a member **Location** struct) are serialized to a memory stream, and this stream is in turn deserialized.</span></span> <span data-ttu-id="07db5-195">Výsledkem je pak porovnána s počáteční instance potvrdit, že **SensorData** je stejný jako původní objekt obnovit.</span><span class="sxs-lookup"><span data-stu-id="07db5-195">The result is then compared to the initial instance to confirm that the **SensorData** object recovered is identical to the original.</span></span>

<span data-ttu-id="07db5-196">Schéma v tomto příkladu se předpokládá, že sdílet mezi čtení a zápis, tak formát Avro objektu kontejneru se nevyžaduje.</span><span class="sxs-lookup"><span data-stu-id="07db5-196">The schema in this example is assumed to be shared between the readers and writers, so the Avro object container format is not required.</span></span> <span data-ttu-id="07db5-197">Příklad toho, jak k serializaci a deserializaci dat do vyrovnávací paměti pomocí reflexe ve formátu kontejneru objektu, pokud schéma musí být sdílená s daty v tématu <a href="#Scenario3">serializace objektu kontejneru soubory pomocí reflexe</a>.</span><span class="sxs-lookup"><span data-stu-id="07db5-197">For an example of how to serialize and deserialize data into memory buffers by using reflection with the object container format when the schema must be shared with the data, see <a href="#Scenario3">Serialization using object container files with reflection</a>.</span></span>

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
        //the usage of Microsoft Avro Library
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

                    //Serialize the data to the specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from the stream and cast it to the same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches the original one
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

                //Serialization to memory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-2-serialization-with-a-generic-record"></a><span data-ttu-id="07db5-198">Příklad 2: Serializace se záznamem obecné</span><span class="sxs-lookup"><span data-stu-id="07db5-198">Sample 2: Serialization with a generic record</span></span>
<span data-ttu-id="07db5-199">Při reflexe nelze použít, protože data nelze reprezentovat prostřednictvím rozhraní .NET třídy s kontraktu dat, lze v záznamu obecné explicitně zadat schématu JSON.</span><span class="sxs-lookup"><span data-stu-id="07db5-199">A JSON schema can be explicitly specified in a generic record when reflection cannot be used because the data cannot be represented via .NET classes with a data contract.</span></span> <span data-ttu-id="07db5-200">Tato metoda je nižší než pomocí reflexe.</span><span class="sxs-lookup"><span data-stu-id="07db5-200">This method is slower than using reflection.</span></span> <span data-ttu-id="07db5-201">V takových případech schéma pro data mohou být také dynamické, tedy není známý v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="07db5-201">In such cases, the schema for the data may also be dynamic, that is, not known at compile time.</span></span> <span data-ttu-id="07db5-202">Data reprezentován jako soubory hodnot oddělených čárkami (CSV), jejichž schématu neznámý, dokud se převede do formátu Avro v době běhu je příklad toto řazení dynamické scénáře.</span><span class="sxs-lookup"><span data-stu-id="07db5-202">Data represented as comma-separated values (CSV) files whose schema is unknown until it is transformed to the Avro format at run time is an example of this sort of dynamic scenario.</span></span>

<span data-ttu-id="07db5-203">Tento příklad ukazuje, jak vytvořit a používat [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) k explicitnímu zadání schématu JSON, postup naplnění s daty a pak k serializaci a deserializaci.</span><span class="sxs-lookup"><span data-stu-id="07db5-203">This example shows how to create and use an [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) to explicitly specify a JSON schema, how to populate it with the data, and then how to serialize and deserialize it.</span></span> <span data-ttu-id="07db5-204">Výsledkem je pak porovnána s počáteční instance potvrďte, že záznam obnovit je stejný jako původní.</span><span class="sxs-lookup"><span data-stu-id="07db5-204">The result is then compared to the initial instance to confirm that the record recovered is identical to the original.</span></span>

<span data-ttu-id="07db5-205">Schéma v tomto příkladu se předpokládá, že sdílet mezi čtení a zápis, tak formát Avro objektu kontejneru se nevyžaduje.</span><span class="sxs-lookup"><span data-stu-id="07db5-205">The schema in this example is assumed to be shared between the readers and writers, so the Avro object container format is not required.</span></span> <span data-ttu-id="07db5-206">Příklad toho, jak k serializaci a deserializaci dat do vyrovnávací paměti pomocí obecné záznam ve formátu kontejneru objektu při schématu musí být součástí serializovaná data, naleznete v části <a href="#Scenario4">serializace objektu kontejneru soubory pomocí obecné záznam</a> příklad.</span><span class="sxs-lookup"><span data-stu-id="07db5-206">For an example of how to serialize and deserialize data into memory buffers by using a generic record with the object container format when the schema must be included with the serialized data, see the <a href="#Scenario4">Serialization using object container files with generic record</a> example.</span></span>

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
    //the usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with the schema explicitly defined in JSON.
        //All serialized data should be mapped to the fields of the generic record,
        //which in turn is then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining the Schema and creating Sample Data Set...");

            //Define the schema in JSON
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

            //Create a generic serializer based on the schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record to represent the data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize the data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize the data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify the results
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

            //Serialization to memory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key to exit.");
            Console.Read();
        }
    }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a><span data-ttu-id="07db5-207">Ukázka 3: Serializace soubory kontejneru objektů a serializace pomocí reflexe</span><span class="sxs-lookup"><span data-stu-id="07db5-207">Sample 3: Serialization using object container files and serialization with reflection</span></span>
<span data-ttu-id="07db5-208">Je podobná scénář v tomto příkladu <a href="#Scenario1"> prvním příkladu</a>, tam, kde je schéma implicitně zadán pomocí reflexe.</span><span class="sxs-lookup"><span data-stu-id="07db5-208">This example is similar to the scenario in the <a href="#Scenario1"> first example</a>, where the schema is implicitly specified with reflection.</span></span> <span data-ttu-id="07db5-209">Rozdíl je, že tady, schéma se předpokládá, že být známé čtečce deserializuje ho.</span><span class="sxs-lookup"><span data-stu-id="07db5-209">The difference is that here, the schema is not assumed to be known to the reader that deserializes it.</span></span> <span data-ttu-id="07db5-210">**SensorData** objekty k serializaci a jejich implicitně určené schéma uložené v souboru kontejneru objektu Avro reprezentována [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="07db5-210">The **SensorData** objects to be serialized and their implicitly specified schema are stored in an Avro object container file represented by the [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) class.</span></span>

<span data-ttu-id="07db5-211">Je v tomto příkladu se serializovat data [ **SequentialWriter<SensorData>**  ](http://msdn.microsoft.com/library/dn627340.aspx) a deserializovat s [ **SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx).</span><span class="sxs-lookup"><span data-stu-id="07db5-211">The data is serialized in this example with [**SequentialWriter<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx) and deserialized with [**SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx).</span></span> <span data-ttu-id="07db5-212">Výsledkem je pak porovnána s počáteční instancí zajistit identity.</span><span class="sxs-lookup"><span data-stu-id="07db5-212">The result then is compared to the initial instances to ensure identity.</span></span>

<span data-ttu-id="07db5-213">Data do souboru objektu kontejneru se komprimují prostřednictvím výchozí [ **Deflate** ] [ deflate-100] kompresní kodek z rozhraní .NET Framework 4.</span><span class="sxs-lookup"><span data-stu-id="07db5-213">The data in the object container file is compressed via the default [**Deflate**][deflate-100] compression codec from .NET Framework 4.</span></span> <span data-ttu-id="07db5-214">Najdete v článku <a href="#Scenario5"> páté příklad</a> v tomto tématu se dozvíte, jak používat novější a vyšší verze [ **Deflate** ] [ deflate-110] komprese kodeků, které jsou k dispozici v rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="07db5-214">See the <a href="#Scenario5"> fifth example</a> in this topic to learn how to use a more recent and superior version of the [**Deflate**][deflate-110] compression codec available in .NET Framework 4.5.</span></span>

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
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes the sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the Deflate codec.
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

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data is compressed using the Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
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

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
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

                //Delete the file
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

            //Saving memory stream to a new file with the given path
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
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
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

            //Reading a file content by using the given path to a memory stream
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
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
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
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
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

                //Serialization using reflection to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a><span data-ttu-id="07db5-215">Ukázka 4: Serializace pomocí obecné záznam soubory kontejneru objektů a serializace</span><span class="sxs-lookup"><span data-stu-id="07db5-215">Sample 4: Serialization using object container files and serialization with generic record</span></span>
<span data-ttu-id="07db5-216">Je podobná scénář v tomto příkladu <a href="#Scenario2"> druhém příkladu</a>, kde je explicitně zadat schématu s JSON.</span><span class="sxs-lookup"><span data-stu-id="07db5-216">This example is similar to the scenario in the <a href="#Scenario2"> second example</a>, where the schema is explicitly specified with JSON.</span></span> <span data-ttu-id="07db5-217">Rozdíl je, že tady, schéma se předpokládá, že být známé čtečce deserializuje ho.</span><span class="sxs-lookup"><span data-stu-id="07db5-217">The difference is that here, the schema is not assumed to be known to the reader that deserializes it.</span></span>

<span data-ttu-id="07db5-218">Sada testovacích dat. se shromažďují do seznamu [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objektů prostřednictvím explicitně definované schéma JSON a pak uloženy v souboru kontejneru objektu reprezentována [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="07db5-218">The test data set is collected into a list of [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objects via an explicitly defined JSON schema and then stored in an object container file represented by the [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) class.</span></span> <span data-ttu-id="07db5-219">Tento soubor kontejner vytvoří zapisovač, který se používá k serializaci dat, nekomprimovaným paměti datový proud, který je pak uloží do souboru.</span><span class="sxs-lookup"><span data-stu-id="07db5-219">This container file creates a writer that is used to serialize the data, uncompressed, to a memory stream that is then saved to a file.</span></span> <span data-ttu-id="07db5-220">[ **Codec.Null** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) parametr použitý k vytvoření čtečky Určuje, že není komprimována tato data.</span><span class="sxs-lookup"><span data-stu-id="07db5-220">The [**Codec.Null**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) parameter used for creating the reader specifies that this data is not compressed.</span></span>

<span data-ttu-id="07db5-221">Data se pak čtení ze souboru a deserializovat do kolekce objektů.</span><span class="sxs-lookup"><span data-stu-id="07db5-221">The data is then read from the file and deserialized into a collection of objects.</span></span> <span data-ttu-id="07db5-222">Tato kolekce je ve srovnání s počáteční seznam záznamů Avro potvrďte, že jsou identické.</span><span class="sxs-lookup"><span data-stu-id="07db5-222">This collection is compared to the initial list of Avro records to confirm that they are identical.</span></span>

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
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining the Schema and creating Sample Data Set...");

                //Define the schema in JSON
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

                //Create a generic serializer based on the schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record to represent the data
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

                //Serializing and saving data to file.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data is not compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data to file...");

                    //Save stream to file
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing the data.
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

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify the results
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

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream to a new file with the given path
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
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
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

            //Reading a file content by using the given path to a memory stream
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
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using the given path
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
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
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

                //Create an instance of the AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.




## <a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a><span data-ttu-id="07db5-223">Ukázka 5: Serializace objektu kontejneru soubory pomocí vlastní kompresní kodek</span><span class="sxs-lookup"><span data-stu-id="07db5-223">Sample 5: Serialization using object container files with a custom compression codec</span></span>
<span data-ttu-id="07db5-224">Páté příklad ukazuje, jak používat vlastní kompresní kodek pro soubory kontejneru objektů Avro.</span><span class="sxs-lookup"><span data-stu-id="07db5-224">The fifth example shows how to use a custom compression codec for Avro object container files.</span></span> <span data-ttu-id="07db5-225">Ukázka obsahující kód pro tento příklad si můžete stáhnout z [ukázky kódu Azure](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) lokality.</span><span class="sxs-lookup"><span data-stu-id="07db5-225">A sample containing the code for this example can be downloaded from the [Azure code samples](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) site.</span></span>

<span data-ttu-id="07db5-226">[Avro Specification](http://avro.apache.org/docs/current/spec.html#Required+Codecs) umožňuje využití používá volitelné kompresní kodek (kromě **Null** a **Deflate** výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="07db5-226">The [Avro Specification](http://avro.apache.org/docs/current/spec.html#Required+Codecs) allows usage of an optional compression codec (in addition to **Null** and **Deflate** defaults).</span></span> <span data-ttu-id="07db5-227">V tomto příkladu není implementace nové kodeků například Snappy (uvedené jako podporované volitelné kodeků v [Avro Specification](http://avro.apache.org/docs/current/spec.html#snappy)).</span><span class="sxs-lookup"><span data-stu-id="07db5-227">This example is not implementing a new codec such as Snappy (mentioned as a supported optional codec in the [Avro Specification](http://avro.apache.org/docs/current/spec.html#snappy)).</span></span> <span data-ttu-id="07db5-228">Ukazuje, jak používat implementace rozhraní .NET Framework 4.5 [ **Deflate** ] [ deflate-110] kodeků, které poskytuje lepší algoritmus komprese na základě [zlib](http://zlib.net/) knihovnu komprese, než je výchozí verze rozhraní .NET Framework 4.</span><span class="sxs-lookup"><span data-stu-id="07db5-228">It shows how to use the .NET Framework 4.5 implementation of the [**Deflate**][deflate-110] codec, which provides a better compression algorithm based on the [zlib](http://zlib.net/) compression library than the default .NET Framework 4 version.</span></span>

    //
    // This code needs to be compiled with the parameter Target Framework set as ".NET Framework 4.5"
    // to ensure the desired implementation of the Deflate compression algorithm is used.
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
        //which are the key ones for data compression.
        //To enable a custom codec, one needs to implement these methods for the required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs to implement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed to be null.");

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
        //Define the actual codec class containing the required methods for manipulating streams:
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
        //Define modified codec factory to be used in the reader.
        //It catches the attempt to use "Deflate" and provide  a custom codec.
        //For all other cases, it relies on the base class (CodecFactory).
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
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related to serialization, deserialization and
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

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Here the custom codec is introduced. For convenience, the next commented code line shows how to use built-in Deflate.
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
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

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment the line below if you want to use built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    //Here the custom codec factory is introduced.
                    //For convenience, the next commented code line shows how to use built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
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

                //Delete the file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
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
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
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

            //Reading file content by using the given path to a memory stream
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
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
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
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
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

                //Serialization using reflection to Avro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.

## <a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a><span data-ttu-id="07db5-229">Ukázka 6: Pomocí Avro odesílat data do služby Microsoft Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="07db5-229">Sample 6: Using Avro to upload data for the Microsoft Azure HDInsight service</span></span>
<span data-ttu-id="07db5-230">Šesté příklad ukazuje některé programátorských technik související s interakci s služby Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="07db5-230">The sixth example illustrates some programming techniques related to interacting with the Azure HDInsight service.</span></span> <span data-ttu-id="07db5-231">Ukázka obsahující kód pro tento příklad si můžete stáhnout z [ukázky kódu Azure](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) lokality.</span><span class="sxs-lookup"><span data-stu-id="07db5-231">A sample containing the code for this example can be downloaded from the [Azure code samples](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) site.</span></span>

<span data-ttu-id="07db5-232">Ukázka provede následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="07db5-232">The sample does the following tasks:</span></span>

* <span data-ttu-id="07db5-233">Připojí se k existujícímu clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="07db5-233">Connects to an existing HDInsight service cluster.</span></span>
* <span data-ttu-id="07db5-234">Serializuje několik souborů CSV a odesílá výsledek do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="07db5-234">Serializes several CSV files and uploads the result to Azure Blob storage.</span></span> <span data-ttu-id="07db5-235">(Soubory CSV jsou distribuovány spolu s vzorku a představují výpis z příloze Stock historická data distribuovat [Infochimps](http://www.infochimps.com/) dobu pod hodnotou 1970 2010.</span><span class="sxs-lookup"><span data-stu-id="07db5-235">(The CSV files are distributed together with the sample and represent an extract from AMEX Stock historical data distributed by [Infochimps](http://www.infochimps.com/) for the period 1970-2010.</span></span> <span data-ttu-id="07db5-236">Ukázka čte data souboru CSV, převede záznamy na instance **Stock** třídy a pak je serializuje pomocí reflexe.</span><span class="sxs-lookup"><span data-stu-id="07db5-236">The sample reads CSV file data, converts the records to instances of the **Stock** class, and then serializes them by using reflection.</span></span> <span data-ttu-id="07db5-237">Definice uložených typu je vytvořený z prostřednictvím nástroje Microsoft Avro Library kódu generování schématu JSON.</span><span class="sxs-lookup"><span data-stu-id="07db5-237">Stock type definition is created from a JSON schema via the Microsoft Avro Library code generation utility.</span></span>
* <span data-ttu-id="07db5-238">Vytvoří novou tabulku externí názvem **akcií** v Hive a odkazy na jeho data nahráli v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="07db5-238">Creates a new external table called **Stocks** in Hive, and links it to the data uploaded in the previous step.</span></span>
* <span data-ttu-id="07db5-239">Provede dotaz pomocí Hive prostřednictvím **akcií** tabulky.</span><span class="sxs-lookup"><span data-stu-id="07db5-239">Executes a query by using Hive over the **Stocks** table.</span></span>

<span data-ttu-id="07db5-240">Kromě toho ukázku provádí postup pro čištění před a po provedení hlavních operací.</span><span class="sxs-lookup"><span data-stu-id="07db5-240">In addition, the sample performs a clean-up procedure before and after performing major operations.</span></span> <span data-ttu-id="07db5-241">Během vyčištění budou odebrána všechna související data objektů Blob v Azure a složky a je vyřadit tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="07db5-241">During the clean-up, all of the related Azure Blob data and folders are removed, and the Hive table is dropped.</span></span> <span data-ttu-id="07db5-242">Můžete také vyvolat postup vyčištění z příkazového řádku ukázka.</span><span class="sxs-lookup"><span data-stu-id="07db5-242">You can also invoke the clean-up procedure from the sample command line.</span></span>

<span data-ttu-id="07db5-243">Ukázka má následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="07db5-243">The sample has the following prerequisites:</span></span>

* <span data-ttu-id="07db5-244">Aktivní předplatné Microsoft Azure a jeho ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="07db5-244">An active Microsoft Azure subscription and its subscription ID.</span></span>
* <span data-ttu-id="07db5-245">Certifikát správy pro předplatné s odpovídající privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="07db5-245">A management certificate for the subscription with the corresponding private key.</span></span> <span data-ttu-id="07db5-246">Certifikát musí být nainstalován v aktuální uživatel privátní úložiště na počítači, který slouží ke spuštění ukázky.</span><span class="sxs-lookup"><span data-stu-id="07db5-246">The certificate should be installed in the current user private storage on the machine used to run the sample.</span></span>
* <span data-ttu-id="07db5-247">Aktivní clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="07db5-247">An active HDInsight cluster.</span></span>
* <span data-ttu-id="07db5-248">Účet úložiště Azure odkazované ke clusteru HDInsight z předchozí součásti, společně s odpovídající primární nebo sekundární přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="07db5-248">An Azure Storage account linked to the HDInsight cluster from the previous prerequisite, together with the corresponding primary, or secondary access key.</span></span>

<span data-ttu-id="07db5-249">Všechny informace z požadavků musí být zadán pro vzorový konfigurační soubor, před spuštěním ukázky.</span><span class="sxs-lookup"><span data-stu-id="07db5-249">All of the information from the prerequisites should be entered to the sample configuration file before the sample is run.</span></span> <span data-ttu-id="07db5-250">Existují dvě možné způsoby, jak to udělat:</span><span class="sxs-lookup"><span data-stu-id="07db5-250">There are two possible ways to do it:</span></span>

* <span data-ttu-id="07db5-251">Upravte soubor app.config v kořenovém adresáři ukázka a potom sestavit ukázku</span><span class="sxs-lookup"><span data-stu-id="07db5-251">Edit the app.config file in the sample root directory and then build the sample</span></span>
* <span data-ttu-id="07db5-252">Nejprve sestavit ukázku a pak upravte AvroHDISample.exe.config v adresáři sestavení</span><span class="sxs-lookup"><span data-stu-id="07db5-252">First build the sample, and then edit AvroHDISample.exe.config in the build directory</span></span>

<span data-ttu-id="07db5-253">V obou případech by mělo být provedeno všechny úpravy  **<appSettings>**  v oddílu nastavení.</span><span class="sxs-lookup"><span data-stu-id="07db5-253">In both cases, all edits should be done in the **<appSettings>** settings section.</span></span> <span data-ttu-id="07db5-254">Postupujte podle komentáře v souboru.</span><span class="sxs-lookup"><span data-stu-id="07db5-254">Follow the comments in the file.</span></span>
<span data-ttu-id="07db5-255">Ukázka spuštění z příkazového řádku tak, že spustíte následující příkaz (kde soubor .zip s ukázkou se předpokládá, že extrahovány do C:\AvroHDISample; Pokud, jinak použijte cestu k souboru relevantní):</span><span class="sxs-lookup"><span data-stu-id="07db5-255">The sample is run from the command line by executing the following command (where the .zip file with the sample was assumed to be extracted to C:\AvroHDISample; if otherwise, use the relevant file path):</span></span>

    AvroHDISample run C:\AvroHDISample\Data

<span data-ttu-id="07db5-256">Vyčistěte clusteru, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="07db5-256">To clean up the cluster, run the following command:</span></span>

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
