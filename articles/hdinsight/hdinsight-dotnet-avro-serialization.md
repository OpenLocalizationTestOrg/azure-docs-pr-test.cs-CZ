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
# <a name="serialize-data-in-hadoop-with-hello-microsoft-avro-library"></a><span data-ttu-id="5d26a-104">Serializace dat v Hadoop pomocí hello Microsoft Avro Library</span><span class="sxs-lookup"><span data-stu-id="5d26a-104">Serialize data in Hadoop with hello Microsoft Avro Library</span></span>

>[!NOTE]
><span data-ttu-id="5d26a-105">Hello Avro SDK je podporován společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="5d26a-105">hello Avro SDK is no longer supported by Microsoft.</span></span> <span data-ttu-id="5d26a-106">Knihovna Hello je komunita s otevřeným zdrojem podporována.</span><span class="sxs-lookup"><span data-stu-id="5d26a-106">hello library is open source community supported.</span></span> <span data-ttu-id="5d26a-107">Hello zdroje pro hello knihovně jsou umístěné v [Githubu](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span><span class="sxs-lookup"><span data-stu-id="5d26a-107">hello sources for hello library are located in [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span></span>

<span data-ttu-id="5d26a-108">Toto téma ukazuje, jak toouse hello [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) tooserialize objekty a další data struktur do datových proudů toopersist je toomemory, databáze nebo souboru.</span><span class="sxs-lookup"><span data-stu-id="5d26a-108">This topic shows how toouse hello [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) tooserialize objects and other data structures into streams toopersist them toomemory, a database, or a file.</span></span> <span data-ttu-id="5d26a-109">Také ukazuje, jak toodeserialize je toorecover hello původní objekty.</span><span class="sxs-lookup"><span data-stu-id="5d26a-109">It also shows how toodeserialize them toorecover hello original objects.</span></span>

[!INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

## <a name="apache-avro"></a><span data-ttu-id="5d26a-110">Apache Avro</span><span class="sxs-lookup"><span data-stu-id="5d26a-110">Apache Avro</span></span>
<span data-ttu-id="5d26a-111">Hello <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> hello implementuje Apache Avro systém serializace dat pro prostředí Microsoft.NET hello.</span><span class="sxs-lookup"><span data-stu-id="5d26a-111">hello <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> implements hello Apache Avro data serialization system for hello Microsoft.NET environment.</span></span> <span data-ttu-id="5d26a-112">Apache Avro poskytuje formát pro výměnu kompaktní binární data pro serializaci.</span><span class="sxs-lookup"><span data-stu-id="5d26a-112">Apache Avro provides a compact binary data interchange format for serialization.</span></span> <span data-ttu-id="5d26a-113">Používá <a href="http://www.json.org" target="_blank">JSON</a> toodefine jazykově nezávislého schématu, které poskytuje vzájemnou vzájemná funkční spolupráce jazyka.</span><span class="sxs-lookup"><span data-stu-id="5d26a-113">It uses <a href="http://www.json.org" target="_blank">JSON</a> toodefine a language-agnostic schema that underwrites language interoperability.</span></span> <span data-ttu-id="5d26a-114">Data serializovaná v jednom jazyce lze číst v jiném.</span><span class="sxs-lookup"><span data-stu-id="5d26a-114">Data serialized in one language can be read in another.</span></span> <span data-ttu-id="5d26a-115">Aktuálně jsou podporované C, C++, C#, Java, PHP, Python a Ruby.</span><span class="sxs-lookup"><span data-stu-id="5d26a-115">Currently C, C++, C#, Java, PHP, Python, and Ruby are supported.</span></span> <span data-ttu-id="5d26a-116">Podrobné informace o formátu hello naleznete v hello <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro Specification</a>.</span><span class="sxs-lookup"><span data-stu-id="5d26a-116">Detailed information on hello format can be found in hello <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro Specification</a>.</span></span> 

>[!NOTE]
><span data-ttu-id="5d26a-117">Hello Microsoft Avro Library nepodporuje hello vzdálené procedury volání (RPC) součástí téhle specifikaci.</span><span class="sxs-lookup"><span data-stu-id="5d26a-117">hello Microsoft Avro Library does not support hello remote procedure calls (RPCs) part of this specification.</span></span>
>

<span data-ttu-id="5d26a-118">Hello serializované reprezentace objektu v hello Avro systému se skládá ze dvou částí: schéma a skutečnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5d26a-118">hello serialized representation of an object in hello Avro system consists of two parts: schema and actual value.</span></span> <span data-ttu-id="5d26a-119">schématu Avro Hello popisuje hello nezávislé na jazyku datový model dat hello serializovat s JSON.</span><span class="sxs-lookup"><span data-stu-id="5d26a-119">hello Avro schema describes hello language-independent data model of hello serialized data with JSON.</span></span> <span data-ttu-id="5d26a-120">Zobrazí se node souběžně s binární reprezentace data.</span><span class="sxs-lookup"><span data-stu-id="5d26a-120">It is presented side by side with a binary representation of data.</span></span> <span data-ttu-id="5d26a-121">Každý objekt toobe napsané pomocí žádné režijní náklady na hodnotu, provedení serializace rychlého a hello reprezentace malé s hello schématu odděleně od binární reprezentace hello umožňuje.</span><span class="sxs-lookup"><span data-stu-id="5d26a-121">Having hello schema separate from hello binary representation permits each object toobe written with no per-value overheads, making serialization fast, and hello representation small.</span></span>

## <a name="hello-hadoop-scenario"></a><span data-ttu-id="5d26a-122">scénář Hadoop Hello</span><span class="sxs-lookup"><span data-stu-id="5d26a-122">hello Hadoop scenario</span></span>
<span data-ttu-id="5d26a-123">Formát serializace Apache Avro Hello se často používá v Azure HDInsight a dalších prostředích s Apache Hadoop.</span><span class="sxs-lookup"><span data-stu-id="5d26a-123">hello Apache Avro serialization format is widely used in Azure HDInsight and other Apache Hadoop environments.</span></span> <span data-ttu-id="5d26a-124">Avro nabízí pohodlný způsob toorepresent komplexní datové struktury v rámci úlohy Hadoop MapReduce.</span><span class="sxs-lookup"><span data-stu-id="5d26a-124">Avro provides a convenient way toorepresent complex data structures within a Hadoop MapReduce job.</span></span> <span data-ttu-id="5d26a-125">Hello formát souborů nástroje Avro (soubor kontejneru objektu Avro) byl programovací model MapReduce navrženou toosupport hello distribuován.</span><span class="sxs-lookup"><span data-stu-id="5d26a-125">hello format of Avro files (Avro object container file) has been designed toosupport hello distributed MapReduce programming model.</span></span> <span data-ttu-id="5d26a-126">Hello klíčové funkce, která umožňuje distribuční hello je, že hello soubory jsou "rozdělitelné" hello smysl, která můžete vyhledat libovolný bod v souboru a začít číst od určitého bloku.</span><span class="sxs-lookup"><span data-stu-id="5d26a-126">hello key feature that enables hello distribution is that hello files are “splittable” in hello sense that one can seek any point in a file and start reading from a particular block.</span></span>

## <a name="serialization-in-avro-library"></a><span data-ttu-id="5d26a-127">Serializace v knihovně Avro</span><span class="sxs-lookup"><span data-stu-id="5d26a-127">Serialization in Avro Library</span></span>
<span data-ttu-id="5d26a-128">Hello knihovny .NET pro Avro podporuje dva způsoby serializaci objektů:</span><span class="sxs-lookup"><span data-stu-id="5d26a-128">hello .NET Library for Avro supports two ways of serializing objects:</span></span>

* <span data-ttu-id="5d26a-129">**reflexe** -hello schéma JSON pro typy hello automaticky vytvořen z dat hello kontrakt atributy hello .NET typy toobe serializovat.</span><span class="sxs-lookup"><span data-stu-id="5d26a-129">**reflection** - hello JSON schema for hello types is automatically built from hello data contract attributes of hello .NET types toobe serialized.</span></span>
* <span data-ttu-id="5d26a-130">**Obecné záznam** -schématu A JSON je explicitně určena v záznamu reprezentována hello [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) třídy existuje toodescribe hello schéma pro hello data toobe po žádné typy .NET serializovat.</span><span class="sxs-lookup"><span data-stu-id="5d26a-130">**generic record** - A JSON schema is explicitly specified in a record represented by hello [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) class when no .NET types are present toodescribe hello schema for hello data toobe serialized.</span></span>

<span data-ttu-id="5d26a-131">Pokud schéma dat hello se označuje tooboth hello zapisovače a čtečku datového proudu hello, odesláním hello dat bez jeho schématu.</span><span class="sxs-lookup"><span data-stu-id="5d26a-131">When hello data schema is known tooboth hello writer and reader of hello stream, hello data can be sent without its schema.</span></span> <span data-ttu-id="5d26a-132">V případech, pokud soubor Avro objektu kontejneru se používá, jsou hello schématu uložená v souboru hello.</span><span class="sxs-lookup"><span data-stu-id="5d26a-132">In cases when an Avro object container file is used, hello schema is stored within hello file.</span></span> <span data-ttu-id="5d26a-133">Další parametry, jako je například kodeků hello používá pro kompresi dat lze zadat.</span><span class="sxs-lookup"><span data-stu-id="5d26a-133">Other parameters, such as hello codec used for data compression, can be specified.</span></span> <span data-ttu-id="5d26a-134">Tyto scénáře jsou popsané v další podrobnosti a ukazuje hello následující příklady kódu:</span><span class="sxs-lookup"><span data-stu-id="5d26a-134">These scenarios are outlined in more detail and illustrated in hello following code examples:</span></span>

## <a name="install-avro-library"></a><span data-ttu-id="5d26a-135">Nainstaluje Avro knihovny</span><span class="sxs-lookup"><span data-stu-id="5d26a-135">Install Avro Library</span></span>
<span data-ttu-id="5d26a-136">Následující Hello jsou požadovány, než nainstalujete hello knihovně:</span><span class="sxs-lookup"><span data-stu-id="5d26a-136">hello following are required before you install hello library:</span></span>

* <span data-ttu-id="5d26a-137"><a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Rozhraní Microsoft .NET Framework 4</a></span><span class="sxs-lookup"><span data-stu-id="5d26a-137"><a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a></span></span>
* <span data-ttu-id="5d26a-138"><a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="5d26a-138"><a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 or later)</span></span>

<span data-ttu-id="5d26a-139">Všimněte si, že hello Newtonsoft.Json.dll závislostí se automaticky stáhne hello instalaci hello Microsoft Avro Library.</span><span class="sxs-lookup"><span data-stu-id="5d26a-139">Note that hello Newtonsoft.Json.dll dependency is downloaded automatically with hello installation of hello Microsoft Avro Library.</span></span> <span data-ttu-id="5d26a-140">Postup Hello je součástí hello následující části:</span><span class="sxs-lookup"><span data-stu-id="5d26a-140">hello procedure is provided in hello following section:</span></span>

<span data-ttu-id="5d26a-141">Hello Microsoft Avro Library je distribuován jako balíčku NuGet, který lze nainstalovat ze sady Visual Studio prostřednictvím hello následující postup:</span><span class="sxs-lookup"><span data-stu-id="5d26a-141">hello Microsoft Avro Library is distributed as a NuGet package that can be installed from Visual Studio via hello following procedure:</span></span>

1. <span data-ttu-id="5d26a-142">Vyberte hello **projektu** -> karta **spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="5d26a-142">Select hello **Project** tab -> **Manage NuGet Packages...**</span></span>
2. <span data-ttu-id="5d26a-143">Vyhledejte "Microsoft.Hadoop.Avro" hello **Online hledání** pole.</span><span class="sxs-lookup"><span data-stu-id="5d26a-143">Search for "Microsoft.Hadoop.Avro" in hello **Search Online** box.</span></span>
3. <span data-ttu-id="5d26a-144">Klikněte na tlačítko hello **nainstalovat** tlačítko vedle příliš**knihovnu Microsoft Azure HDInsight Avro**.</span><span class="sxs-lookup"><span data-stu-id="5d26a-144">Click hello **Install** button next too**Microsoft Azure HDInsight Avro Library**.</span></span>

<span data-ttu-id="5d26a-145">Všimněte si, že hello Newtonsoft.Json.dll (> = 6.0.4) závislostí je také automaticky staženy s hello Microsoft Avro Library.</span><span class="sxs-lookup"><span data-stu-id="5d26a-145">Note that hello Newtonsoft.Json.dll (>=6.0.4) dependency is also downloaded automatically with hello Microsoft Avro Library.</span></span>

<span data-ttu-id="5d26a-146">Hello Microsoft Avro Library zdrojový kód je k dispozici na [Githubu](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span><span class="sxs-lookup"><span data-stu-id="5d26a-146">hello Microsoft Avro Library source code is available at [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span></span>

## <a name="compile-schemas-using-avro-library"></a><span data-ttu-id="5d26a-147">Kompilace pomocí knihovny Avro schémata</span><span class="sxs-lookup"><span data-stu-id="5d26a-147">Compile schemas using Avro Library</span></span>
<span data-ttu-id="5d26a-148">Hello Microsoft Avro Library obsahuje generování kódu, nástroj, který umožňuje vytváření typy C# automaticky podle hello dříve definované schéma JSON.</span><span class="sxs-lookup"><span data-stu-id="5d26a-148">hello Microsoft Avro Library contains a code generation utility that allows creating C# types automatically based on hello previously defined JSON schema.</span></span> <span data-ttu-id="5d26a-149">Nástroj pro generování kódu Hello není distribuován jako binární spustitelný soubor, ale se dají snadno vytvářet prostřednictvím hello následující postup:</span><span class="sxs-lookup"><span data-stu-id="5d26a-149">hello code generation utility is not distributed as a binary executable, but can be easily built via hello following procedure:</span></span>

1. <span data-ttu-id="5d26a-150">Stáhněte si soubor .zip hello hello nejnovější verzi sady SDK HDInsight zdrojového kódu z <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK pro Hadoop</a>.</span><span class="sxs-lookup"><span data-stu-id="5d26a-150">Download hello .zip file with hello latest version of HDInsight SDK source code from <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK For Hadoop</a>.</span></span> <span data-ttu-id="5d26a-151">(Klikněte na tlačítko hello **Stáhnout** ikonu, není hello **stáhne** karta.)</span><span class="sxs-lookup"><span data-stu-id="5d26a-151">(Click hello **Download** icon, not hello **Downloads** tab.)</span></span>
2. <span data-ttu-id="5d26a-152">Extrahujte hello adresáře tooa sady SDK HDInsight z počítače hello pomocí rozhraní .NET Framework 4 nainstalovaný a připojený toohello Internetu pro stahování balíčků NuGet potřebné závislosti.</span><span class="sxs-lookup"><span data-stu-id="5d26a-152">Extract hello HDInsight SDK tooa directory on hello machine with .NET Framework 4 installed and connected toohello Internet for downloading necessary dependency NuGet packages.</span></span> <span data-ttu-id="5d26a-153">Níže předpokládáme, že hello zdrojový kód je extrahované tooC:\SDK.</span><span class="sxs-lookup"><span data-stu-id="5d26a-153">Below, we assume that hello source code is extracted tooC:\SDK.</span></span>
3. <span data-ttu-id="5d26a-154">Přejít toohello složku C:\SDK\src\Microsoft.Hadoop.Avro.Tools a spusťte build.bat.</span><span class="sxs-lookup"><span data-stu-id="5d26a-154">Go toohello folder C:\SDK\src\Microsoft.Hadoop.Avro.Tools and run build.bat.</span></span> <span data-ttu-id="5d26a-155">(hello volání souboru MSBuild z hello 32-bit distribuce hello rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5d26a-155">(hello file calls MSBuild from hello 32-bit distribution of hello .NET Framework.</span></span> <span data-ttu-id="5d26a-156">Pokud chcete toouse hello 64bitovou verzi, build.bat upravit následující hello komentáře v souboru hello.) Zkontrolujte, zda text hello sestavení úspěšné.</span><span class="sxs-lookup"><span data-stu-id="5d26a-156">If you would like toouse hello 64-bit version, edit build.bat, following hello comments inside hello file.) Ensure that hello build is successful.</span></span> <span data-ttu-id="5d26a-157">(Na některých systémů MSBuild může vytvořit upozornění.</span><span class="sxs-lookup"><span data-stu-id="5d26a-157">(On some systems, MSBuild may produce warnings.</span></span> <span data-ttu-id="5d26a-158">Tato upozornění neovlivňují hello nástroj, dokud nedojde k žádným chybám sestavení.)</span><span class="sxs-lookup"><span data-stu-id="5d26a-158">These warnings do not affect hello utility as long as there are no build errors.)</span></span>
4. <span data-ttu-id="5d26a-159">Hello zkompilovat nástroj se nachází v C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.</span><span class="sxs-lookup"><span data-stu-id="5d26a-159">hello compiled utility is located in C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.</span></span>

<span data-ttu-id="5d26a-160">tooget obeznámeni s hello syntaxe příkazového řádku, spusťte následující příkaz z hello složky, kde se nachází nástroj pro generování kódu hello hello:`Microsoft.Hadoop.Avro.Tools help /c:codegen`</span><span class="sxs-lookup"><span data-stu-id="5d26a-160">tooget familiar with hello command-line syntax, execute hello following command from hello folder where hello code generation utility is located: `Microsoft.Hadoop.Avro.Tools help /c:codegen`</span></span>

<span data-ttu-id="5d26a-161">Nástroj hello tootest, můžete z hello ukázka JSON soubor schématu s hello zdrojového kódu vygenerovat třídy jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="5d26a-161">tootest hello utility, you can generate C# classes from hello sample JSON schema file provided with hello source code.</span></span> <span data-ttu-id="5d26a-162">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="5d26a-162">Execute hello following command:</span></span>

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

<span data-ttu-id="5d26a-163">To by měl tooproduce dva C# soubory v aktuálním adresáři hello: SensorData.cs a Location.cs.</span><span class="sxs-lookup"><span data-stu-id="5d26a-163">This is supposed tooproduce two C# files in hello current directory: SensorData.cs and Location.cs.</span></span>

<span data-ttu-id="5d26a-164">toounderstand hello logiky, která používá nástroj pro generování kódu hello při převodu typů tooC # hello JSON schématu, naleznete v souboru hello, GenerationVerification.feature umístěný v C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.</span><span class="sxs-lookup"><span data-stu-id="5d26a-164">toounderstand hello logic that hello code generation utility is using while converting hello JSON schema tooC# types, see hello file GenerationVerification.feature located in C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.</span></span>

<span data-ttu-id="5d26a-165">Obory názvů se extrahují z hello schématu JSON, pomocí logiky hello popsané v souboru hello zmíněných v předchozím odstavci hello.</span><span class="sxs-lookup"><span data-stu-id="5d26a-165">Namespaces are extracted from hello JSON schema, using hello logic described in hello file mentioned in hello previous paragraph.</span></span> <span data-ttu-id="5d26a-166">Obory názvů extrahována ze schématu hello mají přednost před ať se poskytuje s parametrem/n hello v hello nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="5d26a-166">Namespaces extracted from hello schema take precedence over whatever is provided with hello /n parameter in hello utility command line.</span></span> <span data-ttu-id="5d26a-167">Pokud chcete toooverride hello obory názvů obsažené v hello schéma, použijte parametr /nf hello.</span><span class="sxs-lookup"><span data-stu-id="5d26a-167">If you want toooverride hello namespaces contained within hello schema, use hello /nf parameter.</span></span> <span data-ttu-id="5d26a-168">Například toochange všechny obory názvů v hello SampleJSONSchema.avsc toomy.own.nspace, provést hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5d26a-168">For example, toochange all namespaces from hello SampleJSONSchema.avsc toomy.own.nspace, execute hello following command:</span></span>

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="about-hello-samples"></a><span data-ttu-id="5d26a-169">O ukázky hello</span><span class="sxs-lookup"><span data-stu-id="5d26a-169">About hello samples</span></span>
<span data-ttu-id="5d26a-170">Šest příklady uvedené v tomto tématu ilustrují různé scénáře nepodporuje hello Microsoft Avro Library.</span><span class="sxs-lookup"><span data-stu-id="5d26a-170">Six examples provided in this topic illustrate different scenarios supported by hello Microsoft Avro Library.</span></span> <span data-ttu-id="5d26a-171">Hello Microsoft Avro Library je navrženou toowork s žádné datového proudu.</span><span class="sxs-lookup"><span data-stu-id="5d26a-171">hello Microsoft Avro Library is designed toowork with any stream.</span></span> <span data-ttu-id="5d26a-172">V těchto příkladech je upravit data prostřednictvím datových proudů paměti místo souborů datových proudů nebo databází kvůli jednoduchosti a přehlednosti.</span><span class="sxs-lookup"><span data-stu-id="5d26a-172">In these examples, data is manipulated via memory streams rather than file streams or databases for simplicity and consistency.</span></span> <span data-ttu-id="5d26a-173">Hello přístup použitý v produkčním prostředí závisí na hello požadavků na přesný scénář, zdroj dat a svazek, omezení výkonu a dalších faktorů.</span><span class="sxs-lookup"><span data-stu-id="5d26a-173">hello approach taken in a production environment depends on hello exact scenario requirements, data source and volume, performance constraints, and other factors.</span></span>

<span data-ttu-id="5d26a-174">jak Hello první dva příklady zobrazit tooserialize a deserializaci dat do vyrovnávací paměti datového proudu pomocí reflexe a obecné záznamy.</span><span class="sxs-lookup"><span data-stu-id="5d26a-174">hello first two examples show how tooserialize and deserialize data into memory stream buffers by using reflection and generic records.</span></span> <span data-ttu-id="5d26a-175">Hello schématu v obou případech se předpokládá, že toobe sdílena mezi hello čtení a zápis out-of-band.</span><span class="sxs-lookup"><span data-stu-id="5d26a-175">hello schema in these two cases is assumed toobe shared between hello readers and writers out-of-band.</span></span>

<span data-ttu-id="5d26a-176">Zobrazit Hello třetí a čtvrtý příklady jak tooserialize a deserializaci dat pomocí hello Avro objektu kontejneru souborů.</span><span class="sxs-lookup"><span data-stu-id="5d26a-176">hello third and fourth examples show how tooserialize and deserialize data by using hello Avro object container files.</span></span> <span data-ttu-id="5d26a-177">Pokud data uložená v souboru Avro kontejneru, její schéma je vždy uložit s ním, protože hello schématu musí být sdílená pro deserializaci.</span><span class="sxs-lookup"><span data-stu-id="5d26a-177">When data is stored in an Avro container file, its schema is always stored with it because hello schema must be shared for deserialization.</span></span>

<span data-ttu-id="5d26a-178">Hello obsahující ukázkový text hello první čtyři příklady si můžete stáhnout z hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">ukázky kódu Azure</a> lokality.</span><span class="sxs-lookup"><span data-stu-id="5d26a-178">hello sample containing hello first four examples can be downloaded from hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="5d26a-179">Hello páté příklad ukazuje způsob toouse vlastní kompresní kodek pro Avro objektu kontejneru soubory.</span><span class="sxs-lookup"><span data-stu-id="5d26a-179">hello fifth example shows how toouse a custom compression codec for Avro object container files.</span></span> <span data-ttu-id="5d26a-180">Ukázka obsahující hello kód pro tento příklad si můžete stáhnout z hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">ukázky kódu Azure</a> lokality.</span><span class="sxs-lookup"><span data-stu-id="5d26a-180">A sample containing hello code for this example can be downloaded from hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="5d26a-181">Hello šesté příklad ukazuje, jak toouse Avro serializace tooupload data tooAzure úložiště objektů Blob a analyzujte ji pomocí clusteru služby HDInsight (Hadoop) Hive.</span><span class="sxs-lookup"><span data-stu-id="5d26a-181">hello sixth sample shows how toouse Avro serialization tooupload data tooAzure Blob storage and then analyze it by using Hive with an HDInsight (Hadoop) cluster.</span></span> <span data-ttu-id="5d26a-182">Lze ji stáhnout z hello <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">ukázky kódu Azure</a> lokality.</span><span class="sxs-lookup"><span data-stu-id="5d26a-182">It can be downloaded from hello <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="5d26a-183">Tady jsou odkazy toohello šesti ukázky popsané v tématu hello:</span><span class="sxs-lookup"><span data-stu-id="5d26a-183">Here are links toohello six samples discussed in hello topic:</span></span>

* <span data-ttu-id="5d26a-184"><a href="#Scenario1">**Serializace pomocí reflexe** </a> -hello schéma JSON pro typy toobe serializovat automaticky vytvořen z dat hello kontrakt atributy.</span><span class="sxs-lookup"><span data-stu-id="5d26a-184"><a href="#Scenario1">**Serialization with reflection**</a> - hello JSON schema for types toobe serialized is automatically built from hello data contract attributes.</span></span>
* <span data-ttu-id="5d26a-185"><a href="#Scenario2">**Serializace s obecné záznam** </a> -schématu JSON hello je explicitně určena v záznamu, když je k dispozici pro reflexi žádný typ rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="5d26a-185"><a href="#Scenario2">**Serialization with generic record**</a> - hello JSON schema is explicitly specified in a record when no .NET type is available for reflection.</span></span>
* <span data-ttu-id="5d26a-186"><a href="#Scenario3">**Serializace objektu kontejneru soubory pomocí reflexe** </a> -schématu JSON hello je automaticky vytvořen a sdílené společně s hello serializovat data prostřednictvím soubor Avro objektu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5d26a-186"><a href="#Scenario3">**Serialization using object container files with reflection**</a> - hello JSON schema is automatically built and shared together with hello serialized data via an Avro object container file.</span></span>
* <span data-ttu-id="5d26a-187"><a href="#Scenario4">**Serializace objektu kontejneru soubory pomocí obecné záznam** </a> -schématu JSON hello je explicitně zadaný před hello serializace a sdílet společně s hello dat přes soubor Avro objektu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5d26a-187"><a href="#Scenario4">**Serialization using object container files with generic record**</a> - hello JSON schema is explicitly specified before hello serialization and shared together with hello data via an Avro object container file.</span></span>
* <span data-ttu-id="5d26a-188"><a href="#Scenario5">**Serializace objektu kontejneru soubory pomocí vlastní kompresní kodek** </a> -hello příklad ukazuje, jak toocreate Avro objektu kontejneru soubor s vlastní .NET provádění hello Deflate data kompresní kodek.</span><span class="sxs-lookup"><span data-stu-id="5d26a-188"><a href="#Scenario5">**Serialization using object container files with a custom compression codec**</a> - hello example shows how toocreate an Avro object container file with a customized .NET implementation of hello Deflate data compression codec.</span></span>
* <span data-ttu-id="5d26a-189"><a href="#Scenario6">**Pomocí Avro tooupload dat pro služby Microsoft Azure HDInsight hello** </a> -hello příklad ukazuje, jak Avro serializace komunikuje s hello služba HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5d26a-189"><a href="#Scenario6">**Using Avro tooupload data for hello Microsoft Azure HDInsight service**</a> - hello example illustrates how Avro serialization interacts with hello HDInsight service.</span></span> <span data-ttu-id="5d26a-190">Active Azure předplatného a přístup tooan cluster Azure HDInsight vyžaduje toorun v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="5d26a-190">An active Azure subscription and access tooan Azure HDInsight cluster are required toorun this example.</span></span>

## <span data-ttu-id="5d26a-191"><a name="Scenario1"></a>Příklad 1: Serializace pomocí reflexe</span><span class="sxs-lookup"><span data-stu-id="5d26a-191"><a name="Scenario1"></a>Sample 1: Serialization with reflection</span></span>
<span data-ttu-id="5d26a-192">Hello schéma JSON pro typy hello můžete být automaticky vytvořené hello Microsoft Avro Library prostřednictvím reflexe z dat hello kontrakt atributy hello C# objekty toobe serializovat.</span><span class="sxs-lookup"><span data-stu-id="5d26a-192">hello JSON schema for hello types can be automatically built by hello Microsoft Avro Library via reflection from hello data contract attributes of hello C# objects toobe serialized.</span></span> <span data-ttu-id="5d26a-193">vytvoří technologie Hello Microsoft Avro Library [ **IAvroSeralizer<T>**  ](http://msdn.microsoft.com/library/dn627341.aspx) tooidentify hello pole toobe serializovat.</span><span class="sxs-lookup"><span data-stu-id="5d26a-193">hello Microsoft Avro Library creates an [**IAvroSeralizer<T>**](http://msdn.microsoft.com/library/dn627341.aspx) tooidentify hello fields toobe serialized.</span></span>

<span data-ttu-id="5d26a-194">V tomto příkladu objekty ( **SensorData** se členem **umístění** struktura) jsou serializovaných tooa datový proud paměti a tento datový proud je zase deserializovat.</span><span class="sxs-lookup"><span data-stu-id="5d26a-194">In this example, objects (a **SensorData** class with a member **Location** struct) are serialized tooa memory stream, and this stream is in turn deserialized.</span></span> <span data-ttu-id="5d26a-195">Hello výsledkem je, pak porovná s toohello počáteční instance tooconfirm této hello **SensorData** objekt obnovit je identické toohello původní.</span><span class="sxs-lookup"><span data-stu-id="5d26a-195">hello result is then compared toohello initial instance tooconfirm that hello **SensorData** object recovered is identical toohello original.</span></span>

<span data-ttu-id="5d26a-196">schéma Hello v tomto příkladu se předpokládá toobe sdílena mezi hello čtení a zápis, tak formát kontejneru objektů Avro hello se nevyžaduje.</span><span class="sxs-lookup"><span data-stu-id="5d26a-196">hello schema in this example is assumed toobe shared between hello readers and writers, so hello Avro object container format is not required.</span></span> <span data-ttu-id="5d26a-197">Pro příklad tooserialize a deserializaci dat do vyrovnávací paměti pomocí reflexe ve formátu kontejneru objektu hello, pokud hello schématu je potřeba sdílet s hello data, najdete v části <a href="#Scenario3">serializace objektu kontejneru soubory pomocí reflexe</a>.</span><span class="sxs-lookup"><span data-stu-id="5d26a-197">For an example of how tooserialize and deserialize data into memory buffers by using reflection with hello object container format when hello schema must be shared with hello data, see <a href="#Scenario3">Serialization using object container files with reflection</a>.</span></span>

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


## <a name="sample-2-serialization-with-a-generic-record"></a><span data-ttu-id="5d26a-198">Příklad 2: Serializace se záznamem obecné</span><span class="sxs-lookup"><span data-stu-id="5d26a-198">Sample 2: Serialization with a generic record</span></span>
<span data-ttu-id="5d26a-199">Při reflexe nelze použít, protože není možné vyjádřit hello data pomocí tříd rozhraní .NET pomocí kontraktu dat, lze v záznamu obecné explicitně zadat schématu JSON.</span><span class="sxs-lookup"><span data-stu-id="5d26a-199">A JSON schema can be explicitly specified in a generic record when reflection cannot be used because hello data cannot be represented via .NET classes with a data contract.</span></span> <span data-ttu-id="5d26a-200">Tato metoda je nižší než pomocí reflexe.</span><span class="sxs-lookup"><span data-stu-id="5d26a-200">This method is slower than using reflection.</span></span> <span data-ttu-id="5d26a-201">V takových případech hello schéma pro hello data může být také dynamické, tedy není známý v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="5d26a-201">In such cases, hello schema for hello data may also be dynamic, that is, not known at compile time.</span></span> <span data-ttu-id="5d26a-202">Data vyjádřené hodnot oddělených čárkami (CSV) soubory, jejichž schématu neznámý, dokud je transformovat formát Avro toohello v době běhu je příklad toto řazení dynamické scénáře.</span><span class="sxs-lookup"><span data-stu-id="5d26a-202">Data represented as comma-separated values (CSV) files whose schema is unknown until it is transformed toohello Avro format at run time is an example of this sort of dynamic scenario.</span></span>

<span data-ttu-id="5d26a-203">Tento příklad ukazuje, jak toocreate a používat [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) tooexplicitly zadejte schématu JSON, jak toopopulate ji s daty hello a poté jak tooserialize a deserializaci.</span><span class="sxs-lookup"><span data-stu-id="5d26a-203">This example shows how toocreate and use an [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) tooexplicitly specify a JSON schema, how toopopulate it with hello data, and then how tooserialize and deserialize it.</span></span> <span data-ttu-id="5d26a-204">výsledek Hello se pak porovná tooconfirm toohello počáteční instance, která hello záznam obnovit se původní identické toohello.</span><span class="sxs-lookup"><span data-stu-id="5d26a-204">hello result is then compared toohello initial instance tooconfirm that hello record recovered is identical toohello original.</span></span>

<span data-ttu-id="5d26a-205">schéma Hello v tomto příkladu se předpokládá toobe sdílena mezi hello čtení a zápis, tak formát kontejneru objektů Avro hello se nevyžaduje.</span><span class="sxs-lookup"><span data-stu-id="5d26a-205">hello schema in this example is assumed toobe shared between hello readers and writers, so hello Avro object container format is not required.</span></span> <span data-ttu-id="5d26a-206">Pro příklad tooserialize a deserializaci dat do vyrovnávací paměti pomocí záznam obecný formát kontejneru objektů hello při hello schématu musí být součástí hello serializovat data, najdete v části hello <a href="#Scenario4">serializace pomocí kontejner objektů soubory s obecné záznam</a> příklad.</span><span class="sxs-lookup"><span data-stu-id="5d26a-206">For an example of how tooserialize and deserialize data into memory buffers by using a generic record with hello object container format when hello schema must be included with hello serialized data, see hello <a href="#Scenario4">Serialization using object container files with generic record</a> example.</span></span>

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


## <a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a><span data-ttu-id="5d26a-207">Ukázka 3: Serializace soubory kontejneru objektů a serializace pomocí reflexe</span><span class="sxs-lookup"><span data-stu-id="5d26a-207">Sample 3: Serialization using object container files and serialization with reflection</span></span>
<span data-ttu-id="5d26a-208">Tato ukázka je podobném scénáři toohello v hello <a href="#Scenario1"> prvním příkladu</a>, kde je implicitně zadané schéma hello pomocí reflexe.</span><span class="sxs-lookup"><span data-stu-id="5d26a-208">This example is similar toohello scenario in hello <a href="#Scenario1"> first example</a>, where hello schema is implicitly specified with reflection.</span></span> <span data-ttu-id="5d26a-209">Hello rozdílem je, že zde není hello schématu předpokládá toobe známé toohello čtečka, která deserializuje ho.</span><span class="sxs-lookup"><span data-stu-id="5d26a-209">hello difference is that here, hello schema is not assumed toobe known toohello reader that deserializes it.</span></span> <span data-ttu-id="5d26a-210">Hello **SensorData** serializovat toobe objekty a jejich implicitně určené schéma uložené v souboru kontejneru objektu Avro reprezentována hello [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) Třída.</span><span class="sxs-lookup"><span data-stu-id="5d26a-210">hello **SensorData** objects toobe serialized and their implicitly specified schema are stored in an Avro object container file represented by hello [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) class.</span></span>

<span data-ttu-id="5d26a-211">je v tomto příkladu se serializovat Hello data [ **SequentialWriter<SensorData>**  ](http://msdn.microsoft.com/library/dn627340.aspx) a deserializovat s [ **SequentialReader<SensorData>** ](http://msdn.microsoft.com/library/dn627340.aspx).</span><span class="sxs-lookup"><span data-stu-id="5d26a-211">hello data is serialized in this example with [**SequentialWriter<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx) and deserialized with [**SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx).</span></span> <span data-ttu-id="5d26a-212">výsledek Hello je porovnání toohello počáteční instancí tooensure identity.</span><span class="sxs-lookup"><span data-stu-id="5d26a-212">hello result then is compared toohello initial instances tooensure identity.</span></span>

<span data-ttu-id="5d26a-213">Vítejte v hello objekt dat prostřednictvím výchozí hello je komprimovaný soubor kontejneru [ **Deflate** ] [ deflate-100] kompresní kodek z rozhraní .NET Framework 4.</span><span class="sxs-lookup"><span data-stu-id="5d26a-213">hello data in hello object container file is compressed via hello default [**Deflate**][deflate-100] compression codec from .NET Framework 4.</span></span> <span data-ttu-id="5d26a-214">V tématu hello <a href="#Scenario5"> páté příklad</a> v této toolearn téma Jak toouse novější a vyšší verzi hello [ **Deflate** ] [ deflate-110] komprese kodek dostupný v rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="5d26a-214">See hello <a href="#Scenario5"> fifth example</a> in this topic toolearn how toouse a more recent and superior version of hello [**Deflate**][deflate-110] compression codec available in .NET Framework 4.5.</span></span>

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


## <a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a><span data-ttu-id="5d26a-215">Ukázka 4: Serializace pomocí obecné záznam soubory kontejneru objektů a serializace</span><span class="sxs-lookup"><span data-stu-id="5d26a-215">Sample 4: Serialization using object container files and serialization with generic record</span></span>
<span data-ttu-id="5d26a-216">Tato ukázka je podobném scénáři toohello v hello <a href="#Scenario2"> druhém příkladu</a>, kde je explicitně zadané schéma hello s JSON.</span><span class="sxs-lookup"><span data-stu-id="5d26a-216">This example is similar toohello scenario in hello <a href="#Scenario2"> second example</a>, where hello schema is explicitly specified with JSON.</span></span> <span data-ttu-id="5d26a-217">Hello rozdílem je, že zde není hello schématu předpokládá toobe známé toohello čtečka, která deserializuje ho.</span><span class="sxs-lookup"><span data-stu-id="5d26a-217">hello difference is that here, hello schema is not assumed toobe known toohello reader that deserializes it.</span></span>

<span data-ttu-id="5d26a-218">Hello testovací datové sady se shromažďují do seznamu [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objektů prostřednictvím explicitně definované schéma JSON a pak uloženy v objektu kontejneru souboru parametrem hello [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="5d26a-218">hello test data set is collected into a list of [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objects via an explicitly defined JSON schema and then stored in an object container file represented by hello [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) class.</span></span> <span data-ttu-id="5d26a-219">Tento soubor kontejner vytvoří zapisovač, který je použité tooserialize hello data, nekomprimovaným, tooa datový proud paměti, který je pak uložený v souboru tooa.</span><span class="sxs-lookup"><span data-stu-id="5d26a-219">This container file creates a writer that is used tooserialize hello data, uncompressed, tooa memory stream that is then saved tooa file.</span></span> <span data-ttu-id="5d26a-220">Hello [ **Codec.Null** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) parametr použitý k vytvoření čtečky hello Určuje, že není komprimována tato data.</span><span class="sxs-lookup"><span data-stu-id="5d26a-220">hello [**Codec.Null**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) parameter used for creating hello reader specifies that this data is not compressed.</span></span>

<span data-ttu-id="5d26a-221">Hello data se pak čtení ze souboru hello a deserializovat do kolekce objektů.</span><span class="sxs-lookup"><span data-stu-id="5d26a-221">hello data is then read from hello file and deserialized into a collection of objects.</span></span> <span data-ttu-id="5d26a-222">Tato kolekce je porovnání toohello počáteční seznam tooconfirm Avro zaznamenává, jestli se shodují.</span><span class="sxs-lookup"><span data-stu-id="5d26a-222">This collection is compared toohello initial list of Avro records tooconfirm that they are identical.</span></span>

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




## <a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a><span data-ttu-id="5d26a-223">Ukázka 5: Serializace objektu kontejneru soubory pomocí vlastní kompresní kodek</span><span class="sxs-lookup"><span data-stu-id="5d26a-223">Sample 5: Serialization using object container files with a custom compression codec</span></span>
<span data-ttu-id="5d26a-224">Hello páté příklad ukazuje způsob toouse vlastní kompresní kodek pro Avro objektu kontejneru soubory.</span><span class="sxs-lookup"><span data-stu-id="5d26a-224">hello fifth example shows how toouse a custom compression codec for Avro object container files.</span></span> <span data-ttu-id="5d26a-225">Ukázka obsahující hello kód pro tento příklad si můžete stáhnout z hello [ukázky kódu Azure](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) lokality.</span><span class="sxs-lookup"><span data-stu-id="5d26a-225">A sample containing hello code for this example can be downloaded from hello [Azure code samples](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) site.</span></span>

<span data-ttu-id="5d26a-226">Hello [Avro Specification](http://avro.apache.org/docs/current/spec.html#Required+Codecs) umožňuje využití používá volitelné kompresní kodek (kromě příliš**Null** a **Deflate** výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="5d26a-226">hello [Avro Specification](http://avro.apache.org/docs/current/spec.html#Required+Codecs) allows usage of an optional compression codec (in addition too**Null** and **Deflate** defaults).</span></span> <span data-ttu-id="5d26a-227">V tomto příkladu není implementace nové kodeků například Snappy (uvedené jako podporované volitelné kodeků v hello [Avro Specification](http://avro.apache.org/docs/current/spec.html#snappy)).</span><span class="sxs-lookup"><span data-stu-id="5d26a-227">This example is not implementing a new codec such as Snappy (mentioned as a supported optional codec in hello [Avro Specification](http://avro.apache.org/docs/current/spec.html#snappy)).</span></span> <span data-ttu-id="5d26a-228">Ukazuje, jak toouse hello implementace rozhraní .NET Framework 4.5 hello [ **Deflate** ] [ deflate-110] kodeků, které poskytuje lepší algoritmus komprese podle hello [zlib](http://zlib.net/) knihovnu komprese než verze rozhraní .NET Framework 4 výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="5d26a-228">It shows how toouse hello .NET Framework 4.5 implementation of hello [**Deflate**][deflate-110] codec, which provides a better compression algorithm based on hello [zlib](http://zlib.net/) compression library than hello default .NET Framework 4 version.</span></span>

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

## <a name="sample-6-using-avro-tooupload-data-for-hello-microsoft-azure-hdinsight-service"></a><span data-ttu-id="5d26a-229">Ukázka 6: Používající Avro tooupload dat pro hello služby Microsoft Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="5d26a-229">Sample 6: Using Avro tooupload data for hello Microsoft Azure HDInsight service</span></span>
<span data-ttu-id="5d26a-230">Hello šesté příklad ukazuje některé programovací techniky související toointeracting pomocí služby Azure HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="5d26a-230">hello sixth example illustrates some programming techniques related toointeracting with hello Azure HDInsight service.</span></span> <span data-ttu-id="5d26a-231">Ukázka obsahující hello kód pro tento příklad si můžete stáhnout z hello [ukázky kódu Azure](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) lokality.</span><span class="sxs-lookup"><span data-stu-id="5d26a-231">A sample containing hello code for this example can be downloaded from hello [Azure code samples](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) site.</span></span>

<span data-ttu-id="5d26a-232">Ukázka Hello hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="5d26a-232">hello sample does hello following tasks:</span></span>

* <span data-ttu-id="5d26a-233">Připojí tooan stávajícího clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5d26a-233">Connects tooan existing HDInsight service cluster.</span></span>
* <span data-ttu-id="5d26a-234">Serializuje několik souborů CSV a odesílá úložiště objektů Blob tooAzure výsledek hello.</span><span class="sxs-lookup"><span data-stu-id="5d26a-234">Serializes several CSV files and uploads hello result tooAzure Blob storage.</span></span> <span data-ttu-id="5d26a-235">(soubory CSV hello distribuují společně s hello ukázka a představují výpis z příloze Stock historická data distribuovat [Infochimps](http://www.infochimps.com/) dobu hello pod hodnotou 1970 2010.</span><span class="sxs-lookup"><span data-stu-id="5d26a-235">(hello CSV files are distributed together with hello sample and represent an extract from AMEX Stock historical data distributed by [Infochimps](http://www.infochimps.com/) for hello period 1970-2010.</span></span> <span data-ttu-id="5d26a-236">Ukázka Hello čte data souboru CSV, převede hello tooinstances záznamy o hello **Stock** třídy a pak je serializuje pomocí reflexe.</span><span class="sxs-lookup"><span data-stu-id="5d26a-236">hello sample reads CSV file data, converts hello records tooinstances of hello **Stock** class, and then serializes them by using reflection.</span></span> <span data-ttu-id="5d26a-237">Definice uložených typu je vytvořený z schématu JSON prostřednictvím hello nástroj pro generování kódu Microsoft Avro Library.</span><span class="sxs-lookup"><span data-stu-id="5d26a-237">Stock type definition is created from a JSON schema via hello Microsoft Avro Library code generation utility.</span></span>
* <span data-ttu-id="5d26a-238">Vytvoří novou tabulku externí názvem **akcií** v Hive a propojí ji toohello data odeslali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="5d26a-238">Creates a new external table called **Stocks** in Hive, and links it toohello data uploaded in hello previous step.</span></span>
* <span data-ttu-id="5d26a-239">Provede dotaz pomocí Hive prostřednictvím hello **akcií** tabulky.</span><span class="sxs-lookup"><span data-stu-id="5d26a-239">Executes a query by using Hive over hello **Stocks** table.</span></span>

<span data-ttu-id="5d26a-240">Kromě toho hello ukázka provádí postup pro čištění před a po provedení hlavních operací.</span><span class="sxs-lookup"><span data-stu-id="5d26a-240">In addition, hello sample performs a clean-up procedure before and after performing major operations.</span></span> <span data-ttu-id="5d26a-241">Během hello čištění všechny hello související data objektů Blob v Azure a složky, se odeberou a tabulku Hive hello je odstranit.</span><span class="sxs-lookup"><span data-stu-id="5d26a-241">During hello clean-up, all of hello related Azure Blob data and folders are removed, and hello Hive table is dropped.</span></span> <span data-ttu-id="5d26a-242">Můžete také vyvolat hello postup vyčištění z hello ukázka příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="5d26a-242">You can also invoke hello clean-up procedure from hello sample command line.</span></span>

<span data-ttu-id="5d26a-243">Ukázka Hello má hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="5d26a-243">hello sample has hello following prerequisites:</span></span>

* <span data-ttu-id="5d26a-244">Aktivní předplatné Microsoft Azure a jeho ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="5d26a-244">An active Microsoft Azure subscription and its subscription ID.</span></span>
* <span data-ttu-id="5d26a-245">Certifikát správy pro předplatné hello s hello odpovídající privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="5d26a-245">A management certificate for hello subscription with hello corresponding private key.</span></span> <span data-ttu-id="5d26a-246">Hello certifikátu musí být nainstalován v hello aktuální privátní úložiště uživatele na hello počítači toorun hello vzorku.</span><span class="sxs-lookup"><span data-stu-id="5d26a-246">hello certificate should be installed in hello current user private storage on hello machine used toorun hello sample.</span></span>
* <span data-ttu-id="5d26a-247">Aktivní clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5d26a-247">An active HDInsight cluster.</span></span>
* <span data-ttu-id="5d26a-248">Účet úložiště Azure propojené toohello clusteru HDInsight z předchozí součásti hello, společně s hello odpovídající primární nebo sekundární přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="5d26a-248">An Azure Storage account linked toohello HDInsight cluster from hello previous prerequisite, together with hello corresponding primary, or secondary access key.</span></span>

<span data-ttu-id="5d26a-249">Před spuštěním hello ukázka všechny hello informace z požadovaných součástí hello musí být zadaná toohello vzorový konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="5d26a-249">All of hello information from hello prerequisites should be entered toohello sample configuration file before hello sample is run.</span></span> <span data-ttu-id="5d26a-250">Existují dvě možné způsoby, jak toodo ho:</span><span class="sxs-lookup"><span data-stu-id="5d26a-250">There are two possible ways toodo it:</span></span>

* <span data-ttu-id="5d26a-251">Upravte soubor app.config hello v hello ukázka kořenový adresář a následně vytvořit ukázkové hello</span><span class="sxs-lookup"><span data-stu-id="5d26a-251">Edit hello app.config file in hello sample root directory and then build hello sample</span></span>
* <span data-ttu-id="5d26a-252">Nejprve sestavení ukázkový text hello a pak upravte AvroHDISample.exe.config v adresáři hello sestavení</span><span class="sxs-lookup"><span data-stu-id="5d26a-252">First build hello sample, and then edit AvroHDISample.exe.config in hello build directory</span></span>

<span data-ttu-id="5d26a-253">V obou případech by mělo být provedeno všechny úpravy v hello  **<appSettings>**  v oddílu nastavení.</span><span class="sxs-lookup"><span data-stu-id="5d26a-253">In both cases, all edits should be done in hello **<appSettings>** settings section.</span></span> <span data-ttu-id="5d26a-254">Postupujte podle hello komentáře v souboru hello.</span><span class="sxs-lookup"><span data-stu-id="5d26a-254">Follow hello comments in hello file.</span></span>
<span data-ttu-id="5d26a-255">Hello ukázka spuštění z příkazového řádku hello spuštěním hello následující příkaz (kde hello soubor .zip s ukázkou hello se předpokládá, že tooC:\AvroHDISample toobe extrahovat; Pokud hello relevantní, jinak použijte cesta k souboru):</span><span class="sxs-lookup"><span data-stu-id="5d26a-255">hello sample is run from hello command line by executing hello following command (where hello .zip file with hello sample was assumed toobe extracted tooC:\AvroHDISample; if otherwise, use hello relevant file path):</span></span>

    AvroHDISample run C:\AvroHDISample\Data

<span data-ttu-id="5d26a-256">tooclean až hello clusteru, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="5d26a-256">tooclean up hello cluster, run hello following command:</span></span>

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
