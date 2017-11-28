---
title: "aaaApache Storm ukázkové topologie Java - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toocreate topologií Apache Storm v jazyce Java vytvořením příklad word počet topologie."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "Apache storm, například apache storm storm java, příklad topologie storm"
ms.assetid: a8838f29-9c08-4fd9-99ef-26655d1bf6d7
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 54fa9dc3c93ddad83ac861f3101f50f80117d804
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-apache-storm-topology-in-java"></a><span data-ttu-id="4e5ff-104">Vytvoření topologie Apache Storm v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="4e5ff-104">Create an Apache Storm topology in Java</span></span>

<span data-ttu-id="4e5ff-105">Zjistěte, jak toocreate topologii založené na jazyce Java pro Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-105">Learn how toocreate a Java-based topology for Apache Storm.</span></span> <span data-ttu-id="4e5ff-106">Můžete vytvořit topologie Storm, který implementuje počtu slov aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-106">You create a Storm topology that implements a word-count application.</span></span> <span data-ttu-id="4e5ff-107">Použijete projekt Maven toobuild a balíček hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-107">You use Maven toobuild and package hello project.</span></span> <span data-ttu-id="4e5ff-108">Potom se dozvíte, jak toodefine hello topologie pomocí hello tok framework.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-108">Then, you learn how toodefine hello topology using hello Flux framework.</span></span>

> [!NOTE]
> <span data-ttu-id="4e5ff-109">Tok framework Hello je k dispozici v Storm 0.10.0 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-109">hello Flux framework is available in Storm 0.10.0 or later.</span></span> <span data-ttu-id="4e5ff-110">Storm 0.10.0 je k dispozici s HDInsight 3.3 a 3.4.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-110">Storm 0.10.0 is available with HDInsight 3.3 and 3.4.</span></span>

<span data-ttu-id="4e5ff-111">Po dokončení kroků hello v tomto dokumentu, můžete nasadit tooApache hello topologie Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-111">After completing hello steps in this document, you can deploy hello topology tooApache Storm on HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="4e5ff-112">Dokončené verzi příkladů topologie Storm hello vytvořené v tomto dokumentu je k dispozici na [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).</span><span class="sxs-lookup"><span data-stu-id="4e5ff-112">A completed version of hello Storm topology examples created in this document is available at [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4e5ff-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4e5ff-113">Prerequisites</span></span>

* [<span data-ttu-id="4e5ff-114">Java Developer Kit (JDK) verze 7</span><span class="sxs-lookup"><span data-stu-id="4e5ff-114">Java Developer Kit (JDK) version 7</span></span>](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)

* <span data-ttu-id="4e5ff-115">[Maven (https://maven.apache.org/download.cgi)](https://maven.apache.org/download.cgi): Maven je systém sestavení projektu pro projekty Java.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-115">[Maven (https://maven.apache.org/download.cgi)](https://maven.apache.org/download.cgi): Maven is a project build system for Java projects.</span></span>

* <span data-ttu-id="4e5ff-116">Textového editoru nebo IDE.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-116">A text editor or IDE.</span></span>

## <a name="configure-environment-variables"></a><span data-ttu-id="4e5ff-117">Nakonfigurujte proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="4e5ff-117">Configure environment variables</span></span>

<span data-ttu-id="4e5ff-118">Hello následující proměnné prostředí může být nastaven při instalaci Java a hello JDK.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-118">hello following environment variables may be set when you install Java and hello JDK.</span></span> <span data-ttu-id="4e5ff-119">Ale byste měli zkontrolovat, že existují a že obsahují hello správné hodnoty pro váš systém.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-119">However, you should check that they exist and that they contain hello correct values for your system.</span></span>

* <span data-ttu-id="4e5ff-120">**JAVA_HOME** -by měla odkazovat toohello adresáře, kde je nainstalován hello prostředí Java runtime (JRE).</span><span class="sxs-lookup"><span data-stu-id="4e5ff-120">**JAVA_HOME** - should point toohello directory where hello Java runtime environment (JRE) is installed.</span></span> <span data-ttu-id="4e5ff-121">Například v distribuci systému Unix nebo Linux, musí mít hodnotu podobnou příliš`/usr/lib/jvm/java-7-oracle`.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-121">For example, in a Unix or Linux distribution, it should have a value similar too`/usr/lib/jvm/java-7-oracle`.</span></span> <span data-ttu-id="4e5ff-122">Windows neměl by mít hodnotu podobnou příliš`c:\Program Files (x86)\Java\jre1.7`</span><span class="sxs-lookup"><span data-stu-id="4e5ff-122">In Windows, it would have a value similar too`c:\Program Files (x86)\Java\jre1.7`</span></span>

* <span data-ttu-id="4e5ff-123">**CESTA** -musí obsahovat hello následující cesty:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-123">**PATH** - should contain hello following paths:</span></span>

  * <span data-ttu-id="4e5ff-124">**JAVA_HOME** (nebo ekvivalentní cesta hello)</span><span class="sxs-lookup"><span data-stu-id="4e5ff-124">**JAVA_HOME** (or hello equivalent path)</span></span>

  * <span data-ttu-id="4e5ff-125">**JAVA_HOME\bin** (nebo ekvivalentní cesta hello)</span><span class="sxs-lookup"><span data-stu-id="4e5ff-125">**JAVA_HOME\bin** (or hello equivalent path)</span></span>

  * <span data-ttu-id="4e5ff-126">Hello adresáře, kde je nainstalován Maven</span><span class="sxs-lookup"><span data-stu-id="4e5ff-126">hello directory where Maven is installed</span></span>

## <a name="create-a-maven-project"></a><span data-ttu-id="4e5ff-127">Vytvořte projekt Maven</span><span class="sxs-lookup"><span data-stu-id="4e5ff-127">Create a Maven project</span></span>

<span data-ttu-id="4e5ff-128">Z příkazového řádku hello, použijte následující hello příkaz toocreate projekt Maven s názvem **WordCount**:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-128">From hello command line, use hello following command toocreate a Maven project named **WordCount**:</span></span>

```bash
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false
```

> [!NOTE]
> <span data-ttu-id="4e5ff-129">Pokud používáte prostředí PowerShell, je třeba vložit`-D` parametry s dvojitými uvozovkami.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-129">If you are using PowerShell, you must surround the`-D` parameters with double quotes.</span></span>
>
> `mvn archetype:generate "-DarchetypeArtifactId=maven-archetype-quickstart" "-DgroupId=com.microsoft.example" "-DartifactId=WordCount" "-DinteractiveMode=false"`

<span data-ttu-id="4e5ff-130">Tento příkaz vytvoří adresář s názvem `WordCount` hello aktuálního umístění, která obsahuje základní projekt Maven.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-130">This command creates a directory named `WordCount` at hello current location, which contains a basic Maven project.</span></span> <span data-ttu-id="4e5ff-131">Hello `WordCount` adresář obsahuje hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-131">hello `WordCount` directory contains hello following items:</span></span>

* <span data-ttu-id="4e5ff-132">`pom.xml`: Obsahuje nastavení pro projekt Maven hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-132">`pom.xml`: Contains settings for hello Maven project.</span></span>
* <span data-ttu-id="4e5ff-133">`src\main\java\com\microsoft\example`: Obsahuje kód aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-133">`src\main\java\com\microsoft\example`: Contains your application code.</span></span>
* <span data-ttu-id="4e5ff-134">`src\test\java\com\microsoft\example`: Obsahuje testy pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-134">`src\test\java\com\microsoft\example`: Contains tests for your application.</span></span> 

### <a name="remove-hello-generated-example-code"></a><span data-ttu-id="4e5ff-135">Odebrat hello generované ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="4e5ff-135">Remove hello generated example code</span></span>

<span data-ttu-id="4e5ff-136">Odstraňte testovací hello generované hello souborů a aplikací:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-136">Delete hello generated test and hello application files:</span></span>

* <span data-ttu-id="4e5ff-137">**src\test\java\com\microsoft\example\AppTest.Java**</span><span class="sxs-lookup"><span data-stu-id="4e5ff-137">**src\test\java\com\microsoft\example\AppTest.java**</span></span>
* <span data-ttu-id="4e5ff-138">**src\main\java\com\microsoft\example\App.Java**</span><span class="sxs-lookup"><span data-stu-id="4e5ff-138">**src\main\java\com\microsoft\example\App.java**</span></span>

## <a name="add-maven-repositories"></a><span data-ttu-id="4e5ff-139">Přidání úložiště Maven</span><span class="sxs-lookup"><span data-stu-id="4e5ff-139">Add Maven repositories</span></span>

<span data-ttu-id="4e5ff-140">HDInsight je založena na hello Hortonworks Data Platform (HDP), takže vám doporučujeme používat hello Hortonworks úložiště toodownload závislosti pro projekty Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-140">HDInsight is based on hello Hortonworks Data Platform (HDP), so we recommend using hello Hortonworks repository toodownload dependencies for your Apache Storm projects.</span></span> <span data-ttu-id="4e5ff-141">V hello __pom.xml__ soubor, přidejte následující XML po hello hello `<url>http://maven.apache.org</url>` řádku:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-141">In hello __pom.xml__ file, add hello following XML after hello `<url>http://maven.apache.org</url>` line:</span></span>

```xml
<repositories>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPReleases</id>
        <name>HDP Releases</name>
        <url>http://repo.hortonworks.com/content/repositories/releases/</url>
        <layout>default</layout>
    </repository>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPJetty</id>
        <name>Hadoop Jetty</name>
        <url>http://repo.hortonworks.com/content/repositories/jetty-hadoop/</url>
        <layout>default</layout>
    </repository>
</repositories>
```

## <a name="add-properties"></a><span data-ttu-id="4e5ff-142">Přidání vlastnosti</span><span class="sxs-lookup"><span data-stu-id="4e5ff-142">Add properties</span></span>

<span data-ttu-id="4e5ff-143">Maven vám umožní hodnoty úrovni projektu toodefine názvem vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-143">Maven allows you toodefine project-level values called properties.</span></span> <span data-ttu-id="4e5ff-144">V hello __pom.xml__, přidejte následující text po hello hello `</repositories>` řádku:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-144">In hello __pom.xml__, add hello following text after hello `</repositories>` line:</span></span>

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--
    This is a version of Storm from hello Hortonworks repository that is compatible with HDInsight.
    -->
    <storm.version>1.0.1.2.5.3.0-37</storm.version>
</properties>
```

<span data-ttu-id="4e5ff-145">Teď můžete použít tuto hodnotu v dalších částech hello `pom.xml`.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-145">You can now use this value in other sections of hello `pom.xml`.</span></span> <span data-ttu-id="4e5ff-146">Například při zadávání hello verzi komponenty Storm, můžete použít `${storm.version}` místo pevné kódování hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-146">For example, when specifying hello version of Storm components, you can use `${storm.version}` instead of hard coding a value.</span></span>

## <a name="add-dependencies"></a><span data-ttu-id="4e5ff-147">Přidat závislosti</span><span class="sxs-lookup"><span data-stu-id="4e5ff-147">Add dependencies</span></span>

<span data-ttu-id="4e5ff-148">Přidáte závislost pro komponenty Storm.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-148">Add a dependency for Storm components.</span></span> <span data-ttu-id="4e5ff-149">Otevřete hello `pom.xml` souboru a přidejte následující kód v hello hello `<dependencies>` části:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-149">Open hello `pom.xml` file and add hello following code in hello `<dependencies>` section:</span></span>

```xml
<dependency>
    <groupId>org.apache.storm</groupId>
    <artifactId>storm-core</artifactId>
    <version>${storm.version}</version>
    <!-- keep storm out of hello jar-with-dependencies -->
    <scope>provided</scope>
</dependency>
```

<span data-ttu-id="4e5ff-150">Při kompilaci, Maven používá tato informace toolook `storm-core` v úložišti Maven hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-150">At compile time, Maven uses this information toolook up `storm-core` in hello Maven repository.</span></span> <span data-ttu-id="4e5ff-151">Vypadá to nejdřív v úložišti hello v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-151">It first looks in hello repository on your local computer.</span></span> <span data-ttu-id="4e5ff-152">Nejsou-li hello soubory existuje, Maven je stáhne z veřejného úložiště Maven hello a ukládá je v místním úložišti hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-152">If hello files aren't there, Maven downloads them from hello public Maven repository and stores them in hello local repository.</span></span>

> [!NOTE]
> <span data-ttu-id="4e5ff-153">Všimněte si hello `<scope>provided</scope>` řádek v této části.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-153">Notice hello `<scope>provided</scope>` line in this section.</span></span> <span data-ttu-id="4e5ff-154">Toto nastavení určuje Maven tooexclude **storm základní** ze všech JAR souborů, které jsou vytvořit, protože je poskytované hello systému.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-154">This setting tells Maven tooexclude **storm-core** from any JAR files that are created, because it is provided by hello system.</span></span>

## <a name="build-configuration"></a><span data-ttu-id="4e5ff-155">Konfigurace sestavení</span><span class="sxs-lookup"><span data-stu-id="4e5ff-155">Build configuration</span></span>

<span data-ttu-id="4e5ff-156">Moduly plug-in maven povolit toocustomize hello fáze sestavení projektu hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-156">Maven plug-ins allow you toocustomize hello build stages of hello project.</span></span> <span data-ttu-id="4e5ff-157">Například jak kompilace projektu hello nebo jak toopackage do soubor JAR.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-157">For example, how hello project is compiled or how toopackage it into a JAR file.</span></span> <span data-ttu-id="4e5ff-158">Otevřete hello `pom.xml` souboru a přidejte následující kód přímo nad hello hello `</project>` řádku.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-158">Open hello `pom.xml` file and add hello following code directly above hello `</project>` line.</span></span>

```xml
<build>
    <plugins>
    </plugins>
    <resources>
    </resources>
</build>
```

<span data-ttu-id="4e5ff-159">Tato část je použité tooadd moduly plug-in, prostředky a další možnosti konfigurace sestavení.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-159">This section is used tooadd plug-ins, resources, and other build configuration options.</span></span> <span data-ttu-id="4e5ff-160">Úplný přehled hello **pom.xml** souborů najdete v tématu [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).</span><span class="sxs-lookup"><span data-stu-id="4e5ff-160">For a full reference of hello **pom.xml** file, see [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).</span></span>

### <a name="add-plug-ins"></a><span data-ttu-id="4e5ff-161">Přidat moduly plug-in</span><span class="sxs-lookup"><span data-stu-id="4e5ff-161">Add plug-ins</span></span>

<span data-ttu-id="4e5ff-162">Topologií Apache Storm implementována v jazyce Java, hello [modulu plug-in Maven Exec](http://www.mojohaus.org/exec-maven-plugin/) je užitečné, protože umožňuje tooeasily místní spuštění hello topologie ve vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-162">For Apache Storm topologies implemented in Java, hello [Exec Maven Plugin](http://www.mojohaus.org/exec-maven-plugin/) is useful because it allows you tooeasily run hello topology locally in your development environment.</span></span> <span data-ttu-id="4e5ff-163">Přidejte následující toohello hello `<plugins>` části hello `pom.xml` souboru modulu plug-in Exec Maven tooinclude hello:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-163">Add hello following toohello `<plugins>` section of hello `pom.xml` file tooinclude hello Exec Maven plugin:</span></span>

```xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>exec-maven-plugin</artifactId>
    <version>1.4.0</version>
    <executions>
    <execution>
    <goals>
        <goal>exec</goal>
    </goals>
    </execution>
    </executions>
    <configuration>
    <executable>java</executable>
    <includeProjectDependencies>true</includeProjectDependencies>
    <includePluginDependencies>false</includePluginDependencies>
    <classpathScope>compile</classpathScope>
    <mainClass>${storm.topology}</mainClass>
    <cleanupDaemonThreads>false</cleanupDaemonThreads> 
    </configuration>
</plugin>
```

<span data-ttu-id="4e5ff-164">Další užitečné modul plug-in je hello [Apache Maven kompilátoru modul plug-in](http://maven.apache.org/plugins/maven-compiler-plugin/), což je použité toochange možnosti kompilace.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-164">Another useful plug-in is hello [Apache Maven Compiler Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/), which is used toochange compilation options.</span></span> <span data-ttu-id="4e5ff-165">změny Hello hello verzi Javy, který Maven používá pro hello zdroj a cíl pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-165">hello changes hello Java version that Maven uses for hello source and target for your application.</span></span>

* <span data-ttu-id="4e5ff-166">Pro HDInsight __3.4 nebo starším__, nastavení hello zdroje a cíle too__1.7__ verze Java.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-166">For HDInsight __3.4 or earlier__, set hello source and target Java version too__1.7__.</span></span>

* <span data-ttu-id="4e5ff-167">Pro HDInsight __3.5__, nastavení hello zdroje a cíle too__1.8__ verze Java.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-167">For HDInsight __3.5__, set hello source and target Java version too__1.8__.</span></span>

<span data-ttu-id="4e5ff-168">Přidejte následující text v hello hello `<plugins>` části hello `pom.xml` souboru modulu plug-in Apache Maven kompilátoru tooinclude hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-168">Add hello following text in hello `<plugins>` section of hello `pom.xml` file tooinclude hello Apache Maven Compiler plugin.</span></span> <span data-ttu-id="4e5ff-169">Tento příklad určuje 1.8, tak, aby hello cílová HDInsight verze 3.5.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-169">This example specifies 1.8, so hello target HDInsight version is 3.5.</span></span>

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.3</version>
    <configuration>
    <source>1.8</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

### <a name="configure-resources"></a><span data-ttu-id="4e5ff-170">Konfigurace prostředků</span><span class="sxs-lookup"><span data-stu-id="4e5ff-170">Configure resources</span></span>

<span data-ttu-id="4e5ff-171">Hello oddílu prostředků můžete prostředky bez kódu tooinclude například konfigurační soubory, které jsou potřebné součásti v topologii hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-171">hello resources section allows you tooinclude non-code resources such as configuration files needed by components in hello topology.</span></span> <span data-ttu-id="4e5ff-172">V tomto příkladu přidejte následující text v hello hello `<resources>` části hello se soubor pom.xml.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-172">For this example, add hello following text in hello `<resources>` section of hello \`pom.xml file.</span></span>

```xml
<resource>
    <directory>${basedir}/resources</directory>
    <filtering>false</filtering>
    <includes>
        <include>log4j2.xml</include>
    </includes>
</resource>
```

<span data-ttu-id="4e5ff-173">Tento příklad přidá adresáře hello prostředků v kořenovém hello hello projektu (`${basedir}`) jako umístění, která obsahuje prostředky a zahrnuje hello soubor s názvem `log4j2.xml`.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-173">This example adds hello resources directory in hello root of hello project (`${basedir}`) as a location that contains resources, and includes hello file named `log4j2.xml`.</span></span> <span data-ttu-id="4e5ff-174">Tento soubor je použité tooconfigure, jaké je do něj protokolují informace podle topologie hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-174">This file is used tooconfigure what information is logged by hello topology.</span></span>

## <a name="create-hello-topology"></a><span data-ttu-id="4e5ff-175">Vytvoření topologie hello</span><span class="sxs-lookup"><span data-stu-id="4e5ff-175">Create hello topology</span></span>

<span data-ttu-id="4e5ff-176">Topologie založené na jazyce Java Apache Storm se skládá z tři součásti, které musíte napsat (nebo reference) jako závislost.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-176">A Java-based Apache Storm topology consists of three components that you must author (or reference) as a dependency.</span></span>

* <span data-ttu-id="4e5ff-177">**Spouts**: načte data z externího zdroje a vysílá datových proudů do topologie hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-177">**Spouts**: Reads data from external sources and emits streams of data into hello topology.</span></span>

* <span data-ttu-id="4e5ff-178">**Bolts**: provádí zpracování datových proudů vysílaných funkcích spouts nebo jiné funkce bolts a vysílá jeden nebo více datových proudů.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-178">**Bolts**: Performs processing on streams emitted by spouts or other bolts, and emits one or more streams.</span></span>

* <span data-ttu-id="4e5ff-179">**Topologie**: definuje, jak hello spouts a funkce bolts jsou uspořádány a poskytuje hello vstupní bod pro topologii s hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-179">**Topology**: Defines how hello spouts and bolts are arranged, and provides hello entry point for hello topology.</span></span>

### <a name="create-hello-spout"></a><span data-ttu-id="4e5ff-180">Vytvoření hello spout</span><span class="sxs-lookup"><span data-stu-id="4e5ff-180">Create hello spout</span></span>

<span data-ttu-id="4e5ff-181">tooreduce požadavky pro nastavení externích datových zdrojů, hello následující spout jednoduše vyšle náhodných věty.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-181">tooreduce requirements for setting up external data sources, hello following spout simply emits random sentences.</span></span> <span data-ttu-id="4e5ff-182">Je upravenou verzi spout, který je zadán v rámci hello [počáteční příklady Storm](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).</span><span class="sxs-lookup"><span data-stu-id="4e5ff-182">It is a modified version of a spout that is provided with hello [Storm-Starter examples](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).</span></span>

> [!NOTE]
> <span data-ttu-id="4e5ff-183">Příklad funkcí spout, který čte z externího zdroje dat najdete v jednom z hello následující příklady:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-183">For an example of a spout that reads from an external data source, see one of hello following examples:</span></span>
>
> * <span data-ttu-id="4e5ff-184">[TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): Příklad funkcí spout, který čte ze služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-184">[TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): An example spout that reads from Twitter</span></span>
> * <span data-ttu-id="4e5ff-185">[Storm Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): spout, který čte z Kafka</span><span class="sxs-lookup"><span data-stu-id="4e5ff-185">[Storm-Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): A spout that reads from Kafka</span></span>

<span data-ttu-id="4e5ff-186">Pro hello spout, vytvořte soubor s názvem `RandomSentenceSpout.java` v hello `src\main\java\com\microsoft\example` hello adresáře a použijte následující kód v jazyce Java jako obsah hello:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-186">For hello spout, create a file named `RandomSentenceSpout.java` in hello `src\main\java\com\microsoft\example` directory and use hello following Java code as hello contents:</span></span>

```java
package com.microsoft.example;

import org.apache.storm.spout.SpoutOutputCollector;
import org.apache.storm.task.TopologyContext;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseRichSpout;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Values;
import org.apache.storm.utils.Utils;

import java.util.Map;
import java.util.Random;

//This spout randomly emits sentences
public class RandomSentenceSpout extends BaseRichSpout {
  //Collector used tooemit output
  SpoutOutputCollector _collector;
  //Used toogenerate a random number
  Random _rand;

  //Open is called when an instance of hello class is created
  @Override
  public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
  //Set hello instance collector toohello one passed in
    _collector = collector;
    //For randomness
    _rand = new Random();
  }

  //Emit data toohello stream
  @Override
  public void nextTuple() {
  //Sleep for a bit
    Utils.sleep(100);
    //hello sentences that are randomly emitted
    String[] sentences = new String[]{ "hello cow jumped over hello moon", "an apple a day keeps hello doctor away",
        "four score and seven years ago", "snow white and hello seven dwarfs", "i am at two with nature" };
    //Randomly pick a sentence
    String sentence = sentences[_rand.nextInt(sentences.length)];
    //Emit hello sentence
    _collector.emit(new Values(sentence));
  }

  //Ack is not implemented since this is a basic example
  @Override
  public void ack(Object id) {
  }

  //Fail is not implemented since this is a basic example
  @Override
  public void fail(Object id) {
  }

  //Declare hello output fields. In this case, an sentence
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("sentence"));
  }
}
```

> [!NOTE]
> <span data-ttu-id="4e5ff-187">I když tato topologie používá jenom jeden spout, ostatní může mít několik, které data datového kanálu z různých zdrojů do topologie hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-187">Although this topology uses only one spout, others may have several that feed data from different sources into hello topology.</span></span>

### <a name="create-hello-bolts"></a><span data-ttu-id="4e5ff-188">Vytvoření funkce bolts hello</span><span class="sxs-lookup"><span data-stu-id="4e5ff-188">Create hello bolts</span></span>

<span data-ttu-id="4e5ff-189">Funkce Bolts zpracovávat zpracování dat hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-189">Bolts handle hello data processing.</span></span> <span data-ttu-id="4e5ff-190">Tato topologie používá dvě funkce bolts:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-190">This topology uses two bolts:</span></span>

* <span data-ttu-id="4e5ff-191">**SplitSentence**: rozdělí věty hello vysílaných **RandomSentenceSpout** do jednotlivých slov.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-191">**SplitSentence**: Splits hello sentences emitted by **RandomSentenceSpout** into individual words.</span></span>

* <span data-ttu-id="4e5ff-192">**WordCount**: Spočítá počet opakování jednotlivých slov došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-192">**WordCount**: Counts how many times each word has occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="4e5ff-193">Funkce Bolts dělat cokoliv, například výpočet, trvalost nebo rozhovoru tooexternal součásti.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-193">Bolts can do anything, for example, computation, persistence, or talking tooexternal components.</span></span>

<span data-ttu-id="4e5ff-194">Vytvořte dva nové soubory, `SplitSentence.java` a `WordCount.java` v hello `src\main\java\com\microsoft\example` adresáře.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-194">Create two new files, `SplitSentence.java` and `WordCount.java` in hello `src\main\java\com\microsoft\example` directory.</span></span> <span data-ttu-id="4e5ff-195">Použijte následující text jako hello obsahu pro soubory hello hello:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-195">Use hello following text as hello contents for hello files:</span></span>

#### <a name="splitsentence"></a><span data-ttu-id="4e5ff-196">SplitSentence</span><span class="sxs-lookup"><span data-stu-id="4e5ff-196">SplitSentence</span></span>

```java
package com.microsoft.example;

import java.text.BreakIterator;

import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class SplitSentence extends BaseBasicBolt {

  //Execute is called tooprocess tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //Get hello sentence content from hello tuple
    String sentence = tuple.getString(0);
    //An iterator tooget each word
    BreakIterator boundary=BreakIterator.getWordInstance();
    //Give hello iterator hello sentence
    boundary.setText(sentence);
    //Find hello beginning first word
    int start=boundary.first();
    //Iterate over each word and emit it toohello output stream
    for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
      //get hello word
      String word=sentence.substring(start,end);
      //If a word is whitespace characters, replace it with empty
      word=word.replaceAll("\\s+","");
      //if it's an actual word, emit it
      if (!word.equals("")) {
        collector.emit(new Values(word));
      }
    }
  }

  //Declare that emitted tuples contain a word field
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word"));
  }
}
```

#### <a name="wordcount"></a><span data-ttu-id="4e5ff-197">WordCount</span><span class="sxs-lookup"><span data-stu-id="4e5ff-197">WordCount</span></span>

```java
package com.microsoft.example;

import java.util.HashMap;
import java.util.Map;
import java.util.Iterator;

import org.apache.storm.Constants;
import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;
import org.apache.storm.Config;

// For logging
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.LogManager;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class WordCount extends BaseBasicBolt {
  //Create logger for this class
  private static final Logger logger = LogManager.getLogger(WordCount.class);
  //For holding words and counts
  Map<String, Integer> counts = new HashMap<String, Integer>();
  //How often tooemit a count of words
  private Integer emitFrequency;

  // Default constructor
  public WordCount() {
      emitFrequency=5; // Default too60 seconds
  }

  // Constructor that sets emit frequency
  public WordCount(Integer frequency) {
      emitFrequency=frequency;
  }

  //Configure frequency of tick tuples for this bolt
  //This delivers a 'tick' tuple on a specific interval,
  //which is used tootrigger certain actions
  @Override
  public Map<String, Object> getComponentConfiguration() {
      Config conf = new Config();
      conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
      return conf;
  }

  //execute is called tooprocess tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //If it's a tick tuple, emit all words and counts
    if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
            && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
      for(String word : counts.keySet()) {
        Integer count = counts.get(word);
        collector.emit(new Values(word, count));
        logger.info("Emitting a count of " + count + " for word " + word);
      }
    } else {
      //Get hello word contents from hello tuple
      String word = tuple.getString(0);
      //Have we counted any already?
      Integer count = counts.get(word);
      if (count == null)
        count = 0;
      //Increment hello count and store it
      count++;
      counts.put(word, count);
    }
  }

  //Declare that this emits a tuple containing two fields; word and count
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word", "count"));
  }
}
```

### <a name="define-hello-topology"></a><span data-ttu-id="4e5ff-198">Definovat topologii hello</span><span class="sxs-lookup"><span data-stu-id="4e5ff-198">Define hello topology</span></span>

<span data-ttu-id="4e5ff-199">topologie Hello sváže funkcích spouts hello a bolts společně na graf, který definuje, jak se data proudí mezi součástmi hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-199">hello topology ties hello spouts and bolts together into a graph, which defines how data flows between hello components.</span></span> <span data-ttu-id="4e5ff-200">Nabízí taky paralelismus pomocné parametry, které Storm používá při vytváření instancí komponent hello v rámci clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-200">It also provides parallelism hints that Storm uses when creating instances of hello components within hello cluster.</span></span>

<span data-ttu-id="4e5ff-201">Hello následující obrázek je základní diagram hello grafu součástí této topologii.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-201">hello following image is a basic diagram of hello graph of components for this topology.</span></span>

![Diagram znázorňující hello spouts a bolts uspořádání](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

<span data-ttu-id="4e5ff-203">tooimplement hello topologie, vytvořte soubor s názvem `WordCountTopology.java` v hello `src\main\java\com\microsoft\example` adresáře.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-203">tooimplement hello topology, create a file named `WordCountTopology.java` in hello `src\main\java\com\microsoft\example` directory.</span></span> <span data-ttu-id="4e5ff-204">Použijte následující kód v jazyce Java jako hello obsah souboru hello hello:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-204">Use hello following Java code as hello contents of hello file:</span></span>

```java
package com.microsoft.example;

import org.apache.storm.Config;
import org.apache.storm.LocalCluster;
import org.apache.storm.StormSubmitter;
import org.apache.storm.topology.TopologyBuilder;
import org.apache.storm.tuple.Fields;

import com.microsoft.example.RandomSentenceSpout;

public class WordCountTopology {

  //Entry point for hello topology
  public static void main(String[] args) throws Exception {
  //Used toobuild hello topology
    TopologyBuilder builder = new TopologyBuilder();
    //Add hello spout, with a name of 'spout'
    //and parallelism hint of 5 executors
    builder.setSpout("spout", new RandomSentenceSpout(), 5);
    //Add hello SplitSentence bolt, with a name of 'split'
    //and parallelism hint of 8 executors
    //shufflegrouping subscribes toohello spout, and equally distributes
    //tuples (sentences) across instances of hello SplitSentence bolt
    builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
    //Add hello counter, with a name of 'count'
    //and parallelism hint of 12 executors
    //fieldsgrouping subscribes toohello split bolt, and
    //ensures that hello same word is sent toohello same instance (group by field 'word')
    builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

    //new configuration
    Config conf = new Config();
    //Set toofalse toodisable debug information when
    // running in production on a cluster
    conf.setDebug(false);

    //If there are arguments, we are running on a cluster
    if (args != null && args.length > 0) {
      //parallelism hint tooset hello number of workers
      conf.setNumWorkers(3);
      //submit hello topology
      StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
    }
    //Otherwise, we are running locally
    else {
      //Cap hello maximum number of executors that can be spawned
      //for a component too3
      conf.setMaxTaskParallelism(3);
      //LocalCluster is used toorun locally
      LocalCluster cluster = new LocalCluster();
      //submit hello topology
      cluster.submitTopology("word-count", conf, builder.createTopology());
      //sleep
      Thread.sleep(10000);
      //shut down hello cluster
      cluster.shutdown();
    }
  }
}
```

### <a name="configure-logging"></a><span data-ttu-id="4e5ff-205">Konfigurace protokolování</span><span class="sxs-lookup"><span data-stu-id="4e5ff-205">Configure logging</span></span>

<span data-ttu-id="4e5ff-206">Storm používá Apache Log4j toolog informace.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-206">Storm uses Apache Log4j toolog information.</span></span> <span data-ttu-id="4e5ff-207">Pokud neprovedete konfiguraci protokolování, hello topologie vysílá diagnostické informace.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-207">If you do not configure logging, hello topology emits diagnostic information.</span></span> <span data-ttu-id="4e5ff-208">Co je protokolováno, toocontrol vytvořte soubor s názvem `log4j2.xml` v hello `resources` adresáře.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-208">toocontrol what is logged, create a file named `log4j2.xml` in hello `resources` directory.</span></span> <span data-ttu-id="4e5ff-209">Použijte následující XML jako hello obsah souboru hello hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-209">Use hello following XML as hello contents of hello file.</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
<Appenders>
    <Console name="STDOUT" target="SYSTEM_OUT">
        <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
</Appenders>
<Loggers>
    <Logger name="com.microsoft.example" level="trace" additivity="false">
        <AppenderRef ref="STDOUT"/>
    </Logger>
    <Root level="error">
        <Appender-Ref ref="STDOUT"/>
    </Root>
</Loggers>
</Configuration>
```

<span data-ttu-id="4e5ff-210">Tato konfigurace XML nakonfiguruje nové protokoly pro hello `com.microsoft.example` třída, která obsahuje součásti hello v této topologii příklad.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-210">This XML configures a new logger for hello `com.microsoft.example` class, which includes hello components in this example topology.</span></span> <span data-ttu-id="4e5ff-211">úroveň Hello nastavena tootrace pro tohoto protokolovacího nástroje, které jsou zaznamenány žádné informace o protokolování vygenerované součásti v této topologii.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-211">hello level is set tootrace for this logger, which captures any logging information emitted by components in this topology.</span></span>

<span data-ttu-id="4e5ff-212">Hello `<Root level="error">` části nakonfiguruje hello kořenové úrovně protokolování (vše, co není ve `com.microsoft.example`) informace o chybě tooonly protokolu.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-212">hello `<Root level="error">` section configures hello root level of logging (everything not in `com.microsoft.example`) tooonly log error information.</span></span>

<span data-ttu-id="4e5ff-213">Další informace o konfiguraci protokolování pro Log4j najdete v tématu [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).</span><span class="sxs-lookup"><span data-stu-id="4e5ff-213">For more information on configuring logging for Log4j, see [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).</span></span>

> [!NOTE]
> <span data-ttu-id="4e5ff-214">Storm verze 0.10.0 a vyšší využití Log4j 2.x.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-214">Storm version 0.10.0 and higher use Log4j 2.x.</span></span> <span data-ttu-id="4e5ff-215">Starší verze storm použít Log4j 1.x, který používá jiný formát pro konfiguraci protokolu.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-215">Older versions of storm used Log4j 1.x, which used a different format for log configuration.</span></span> <span data-ttu-id="4e5ff-216">Informace o konfiguraci starší hello, najdete v části [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).</span><span class="sxs-lookup"><span data-stu-id="4e5ff-216">For information on hello older configuration, see [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).</span></span>

## <a name="test-hello-topology-locally"></a><span data-ttu-id="4e5ff-217">Test hello místně topologie</span><span class="sxs-lookup"><span data-stu-id="4e5ff-217">Test hello topology locally</span></span>

<span data-ttu-id="4e5ff-218">Po uložení hello soubory, použijte následující příkaz tootest hello topologie místně hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-218">After you save hello files, use hello following command tootest hello topology locally.</span></span>

```bash
mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology
```

<span data-ttu-id="4e5ff-219">Při jeho spuštění, hello topologie zobrazí informace o spuštění.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-219">As it runs, hello topology displays startup information.</span></span> <span data-ttu-id="4e5ff-220">Hello následující text je příklad výstupu počet word hello:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-220">hello following text is an example of hello word count output:</span></span>

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

<span data-ttu-id="4e5ff-221">Tento příklad protokol Určuje, že word hello "a" má byla vygenerované 113 časy.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-221">This example log indicates that hello word 'and' has been emitted 113 times.</span></span> <span data-ttu-id="4e5ff-222">Hello počet i nadále toogo nahoru, dokud hello topologie běží, protože hello spout nepřetržitě vysílá hello stejné věty.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-222">hello count continues toogo up as long as hello topology runs because hello spout continuously emits hello same sentences.</span></span>

<span data-ttu-id="4e5ff-223">Mezi emisí slova a počty je interval 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-223">There is a 5-second interval between emission of words and counts.</span></span> <span data-ttu-id="4e5ff-224">Hello **WordCount** nakonfigurována součást tooonly generuje informace, pokud dorazí značek řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-224">hello **WordCount** component is configured tooonly emit information when a tick tuple arrives.</span></span> <span data-ttu-id="4e5ff-225">Požaduje této značky řazených kolekcí členů jsou pouze doručovány každých pět sekund.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-225">It requests that tick tuples are only delivered every five seconds.</span></span>

## <a name="convert-hello-topology-tooflux"></a><span data-ttu-id="4e5ff-226">Převést tooFlux topologie hello</span><span class="sxs-lookup"><span data-stu-id="4e5ff-226">Convert hello topology tooFlux</span></span>

<span data-ttu-id="4e5ff-227">Tok je nové rozhraní Storm 0.10.0 k dispozici a vyšší, což vám umožní tooseparate konfiguraci z implementace.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-227">Flux is a new framework available with Storm 0.10.0 and higher, which allows you tooseparate configuration from implementation.</span></span> <span data-ttu-id="4e5ff-228">Vaše komponenty jsou stále definována v jazyce Java, ale hello topologie je definována pomocí souboru YAML.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-228">Your components are still defined in Java, but hello topology is defined using a YAML file.</span></span> <span data-ttu-id="4e5ff-229">Můžete balíček definice výchozí topologie s projektem nebo používat samostatný soubor při odesílání topologie hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-229">You can package a default topology definition with your project, or use a standalone file when submitting hello topology.</span></span> <span data-ttu-id="4e5ff-230">Při odesílání topologie tooStorm hello, můžete použít proměnné prostředí nebo konfigurační soubory toopopulate hodnoty v hello YAML topologie definice.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-230">When submitting hello topology tooStorm, you can use environment variables or configuration files toopopulate values in hello YAML topology definition.</span></span>

<span data-ttu-id="4e5ff-231">soubor YAML Hello definuje hello součásti toouse hello topologie a hello tok dat mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-231">hello YAML file defines hello components toouse for hello topology and hello data flow between them.</span></span> <span data-ttu-id="4e5ff-232">Můžete zahrnout soubor YAML jako součást soubor jar hello nebo externí soubor YAML můžete použít.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-232">You can include a YAML file as part of hello jar file or you can use an external YAML file.</span></span>

<span data-ttu-id="4e5ff-233">Další informace o toku najdete v tématu [tok framework (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="4e5ff-233">For more information on Flux, see [Flux framework (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span></span>

> [!WARNING]
> <span data-ttu-id="4e5ff-234">Kvůli tooa [chyb (https://issues.apache.org/jira/browse/STORM-2055)](https://issues.apache.org/jira/browse/STORM-2055) s Storm 1.0.1, může být nutné tooinstall [Storm vývojového prostředí](https://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) toorun tok místně topologie.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-234">Due tooa [bug (https://issues.apache.org/jira/browse/STORM-2055)](https://issues.apache.org/jira/browse/STORM-2055) with Storm 1.0.1, you may need tooinstall a [Storm development environment](https://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) toorun Flux topologies locally.</span></span>

1. <span data-ttu-id="4e5ff-235">Přesunout hello `WordCountTopology.java` souboru mimo projekt hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-235">Move hello `WordCountTopology.java` file out of hello project.</span></span> <span data-ttu-id="4e5ff-236">Tento soubor dříve, definované hello topologie, ale není potřeba s tokem.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-236">Previously, this file defined hello topology, but isn't needed with Flux.</span></span>

2. <span data-ttu-id="4e5ff-237">V hello `resources` adresáře, vytvořte soubor s názvem `topology.yaml`.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-237">In hello `resources` directory, create a file named `topology.yaml`.</span></span> <span data-ttu-id="4e5ff-238">Použijte hello následující text jako hello obsah tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-238">Use hello following text as hello contents of this file.</span></span>

        name: "wordcount"       # friendly name for hello topology
        
        config:                 # Topology configuration
        topology.workers: 1     # Hint for hello number of workers toocreate
        
        spouts:                 # Spout definitions
        - id: "sentence-spout"
            className: "com.microsoft.example.RandomSentenceSpout"
            parallelism: 1      # parallelism hint
        
        bolts:                  # Bolt definitions
        - id: "splitter-bolt"
            className: "com.microsoft.example.SplitSentence"
            parallelism: 1
         
        - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
                - 10
            parallelism: 1
        
        streams:                # Stream definitions
            - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
            from: "sentence-spout"       # hello stream emitter
            to: "splitter-bolt"          # hello stream consumer
            grouping:                    # Grouping type
                type: SHUFFLE
          
            - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
                args: ["word"]           # field(s) toogroup on

3. <span data-ttu-id="4e5ff-239">Ujistěte se, hello následující změny toohello `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-239">Make hello following changes toohello `pom.xml` file.</span></span>
   
   * <span data-ttu-id="4e5ff-240">Přidejte následující nové závislosti v hello hello `<dependencies>` části:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-240">Add hello following new dependency in hello `<dependencies>` section:</span></span>
     
        ```xml
        <!-- Add a dependency on hello Flux framework -->
        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>flux-core</artifactId>
            <version>${storm.version}</version>
        </dependency>
        ```
   * <span data-ttu-id="4e5ff-241">Přidejte následující modul plug-in toohello hello `<plugins>` části.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-241">Add hello following plugin toohello `<plugins>` section.</span></span> <span data-ttu-id="4e5ff-242">Tento modul plug-in zpracovává hello vytvoření balíčku (soubor jar) pro projekt hello a platí konkrétní tooFlux některé transformace při vytváření balíčku hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-242">This plugin handles hello creation of a package (jar file) for hello project, and applies some transformations specific tooFlux when creating hello package.</span></span>
     
        ```xml
        <!-- build an uber jar -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <configuration>
                <transformers>
                    <!-- Keep us from getting a "can't overwrite file error" -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                    <!-- We're using Flux, so refer tooit as main -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <mainClass>org.apache.storm.flux.Flux</mainClass>
                    </transformer>
                </transformers>
                <!-- Keep us from getting a bad signature error -->
                <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                            <exclude>META-INF/*.SF</exclude>
                            <exclude>META-INF/*.DSA</exclude>
                            <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                </filters>
            </configuration>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
        ```

   * <span data-ttu-id="4e5ff-243">V hello **modulu plug-in exec maven** `<configuration>` změňte hodnotu hello `<mainClass>` příliš`org.apache.storm.flux.Flux`.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-243">In hello **exec-maven-plugin** `<configuration>` section, change hello value for `<mainClass>` too`org.apache.storm.flux.Flux`.</span></span> <span data-ttu-id="4e5ff-244">Toto nastavení umožňuje tok toohandle hello topologie spuštěn místně v vývoj.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-244">This setting allows Flux toohandle running hello topology locally in development.</span></span>

   * <span data-ttu-id="4e5ff-245">V hello `<resources>` přidejte následující toohello hello `<includes>`.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-245">In hello `<resources>` section, add hello following toohello `<includes>`.</span></span> <span data-ttu-id="4e5ff-246">Tato konfigurace XML obsahuje hello YAML soubor, který definuje hello topologie v rámci projektu hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-246">This XML includes hello YAML file that defines hello topology as part of hello project.</span></span>

        ```xml
        <include>topology.yaml</include>
        ```

## <a name="test-hello-flux-topology-locally"></a><span data-ttu-id="4e5ff-247">Test hello tok topologie místně</span><span class="sxs-lookup"><span data-stu-id="4e5ff-247">Test hello flux topology locally</span></span>

1. <span data-ttu-id="4e5ff-248">Použijte následující toocompile hello a provést hello tok topologie pomocí nástroje Maven:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-248">Use hello following toocompile and execute hello Flux topology using Maven:</span></span>

    ```bash
    mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    ```

    <span data-ttu-id="4e5ff-249">Pokud používáte prostředí PowerShell, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-249">If you are using PowerShell, use hello following command:</span></span>

    ```bash
    mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"
    ```

    > [!WARNING]
    > <span data-ttu-id="4e5ff-250">Pokud vaše topologie používá Storm 1.0.1 bits, tento příkaz se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-250">If your topology uses Storm 1.0.1 bits, this command fails.</span></span> <span data-ttu-id="4e5ff-251">Tato chyba je způsobená [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055).</span><span class="sxs-lookup"><span data-stu-id="4e5ff-251">This failure is caused by [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055).</span></span> <span data-ttu-id="4e5ff-252">Místo toho [nainstalovat Storm ve vašem vývojovém prostředí](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html) a hello použijte následující informace.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-252">Instead, [install Storm in your development environment](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html) and use hello following information.</span></span>

    <span data-ttu-id="4e5ff-253">Pokud máte [nainstalovaný ve vašem vývojovém prostředí Storm](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), můžete použít následující příkazy místo hello:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-253">If you have [installed Storm in your development environment](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), you can use hello following commands instead:</span></span>

    ```bash
    mvn compile package
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml
    ```

    <span data-ttu-id="4e5ff-254">Hello `--local` parametr spustí hello topologie v místním režimu ve vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-254">hello `--local` parameter runs hello topology in local mode on your development environment.</span></span> <span data-ttu-id="4e5ff-255">Hello `-R /topology.yaml` parametr používá hello `topology.yaml` souborů prostředků z hello jar souboru toodefine hello topologie.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-255">hello `-R /topology.yaml` parameter uses hello `topology.yaml` file resource from hello jar file toodefine hello topology.</span></span>

    <span data-ttu-id="4e5ff-256">Při jeho spuštění, hello topologie zobrazí informace o spuštění.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-256">As it runs, hello topology displays startup information.</span></span> <span data-ttu-id="4e5ff-257">Následující text Hello je příklad výstupu hello:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-257">hello following text is an example of hello output:</span></span>

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs

    <span data-ttu-id="4e5ff-258">Mezi listy protokolovaných informací je prodlevu 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-258">There is a 10-second delay between batches of logged information.</span></span>

2. <span data-ttu-id="4e5ff-259">Vytvořit kopii hello `topology.yaml` souboru z projektu hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-259">Make a copy of hello `topology.yaml` file from hello project.</span></span> <span data-ttu-id="4e5ff-260">Název hello nový soubor `newtopology.yaml`.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-260">Name hello new file `newtopology.yaml`.</span></span> <span data-ttu-id="4e5ff-261">V hello `newtopology.yaml` souboru, najít hello následující části a změnit hodnotu hello `10` příliš`5`.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-261">In hello `newtopology.yaml` file, find hello following section and change hello value of `10` too`5`.</span></span> <span data-ttu-id="4e5ff-262">Tato změna změny hello interval mezi generování dávky aplikace word počty z too5 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-262">This modification changes hello interval between emitting batches of word counts from 10 seconds too5.</span></span>

    ```yaml
    - id: "counter-bolt"
    className: "com.microsoft.example.WordCount"
    constructorArgs:
    - 5
    parallelism: 1
    ```yaml

3. toorun hello topology, use hello following command:

    ```bash
    mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"
    ```

    <span data-ttu-id="4e5ff-263">Nebo, pokud máte Storm na vašem vývojovém prostředí:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-263">Or, if you have Storm on your development environment:</span></span>

    ```bash
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml
    ```

    <span data-ttu-id="4e5ff-264">Změna hello `/path/to/newtopology.yaml` toohello cestě toohello newtopology.yaml souboru jste vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-264">Change hello `/path/to/newtopology.yaml` toohello path toohello newtopology.yaml file you created in hello previous step.</span></span> <span data-ttu-id="4e5ff-265">Tento příkaz používá hello newtopology.yaml jako definice topologie hello.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-265">This command uses hello newtopology.yaml as hello topology definition.</span></span> <span data-ttu-id="4e5ff-266">Vzhledem k tomu, že jsme nezahrnuli hello `compile` parametr Maven používá verzi hello projektu hello vytvořené v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-266">Since we didn't include hello `compile` parameter, Maven uses hello version of hello project built in previous steps.</span></span>

    <span data-ttu-id="4e5ff-267">Jednou hello topologie spustí, měli byste zaznamenat, že hello čas mezi emitovaného dávky se změnila hodnota hello tooreflect v newtopology.yaml.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-267">Once hello topology starts, you should notice that hello time between emitted batches has changed tooreflect hello value in newtopology.yaml.</span></span> <span data-ttu-id="4e5ff-268">Abyste viděli, že můžete změnit konfiguraci prostřednictvím soubor YAML bez nutnosti toorecompile hello topologie.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-268">So you can see that you can change your configuration through a YAML file without having toorecompile hello topology.</span></span>

<span data-ttu-id="4e5ff-269">Další informace o těchto a dalších funkcí hello tok framework najdete v tématu [tok (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="4e5ff-269">For more information on these and other features of hello Flux framework, see [Flux (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span></span>

## <a name="trident"></a><span data-ttu-id="4e5ff-270">Trident</span><span class="sxs-lookup"><span data-stu-id="4e5ff-270">Trident</span></span>

<span data-ttu-id="4e5ff-271">Trident má vysokou úroveň abstrakce, která je poskytována Storm.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-271">Trident is a high-level abstraction that is provided by Storm.</span></span> <span data-ttu-id="4e5ff-272">Podporuje stavová zpracování.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-272">It supports stateful processing.</span></span> <span data-ttu-id="4e5ff-273">Primární výhodou Trident Hello je, že ho může zaručit, že každou zprávu, která vstupuje do topologie hello je zpracovány pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-273">hello primary advantage of Trident is that it can guarantee that every message that enters hello topology is processed only once.</span></span> <span data-ttu-id="4e5ff-274">Bez použití Trident, topologie pouze zaručit alespoň jednou zpracování zpráv.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-274">Without using Trident, your topology can only guarantee that messages are processed at least once.</span></span> <span data-ttu-id="4e5ff-275">Existují také další rozdíly, jako je například integrované komponenty, které lze použít místo vytvoření funkce bolts.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-275">There are also other differences, such as built-in components that can be used instead of creating bolts.</span></span> <span data-ttu-id="4e5ff-276">Funkce bolts jsou ve skutečnosti nahrazovány obecného méně součástí, např. filtry, projekce a funkcí.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-276">In fact, bolts are replaced by less-generic components, such as filters, projections, and functions.</span></span>

<span data-ttu-id="4e5ff-277">Trident aplikace lze vytvořit pomocí projekty Maven.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-277">Trident applications can be created by using Maven projects.</span></span> <span data-ttu-id="4e5ff-278">Použít hello basic stejné kroky uvedenou výše v tomto článku – pouze hello kód se liší.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-278">You use hello same basic steps as presented earlier in this article—only hello code is different.</span></span> <span data-ttu-id="4e5ff-279">Trident také (aktuálně) nelze zadat s hello tok framework.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-279">Trident also cannot (currently) be used with hello Flux framework.</span></span>

<span data-ttu-id="4e5ff-280">Další informace o Trident naleznete v tématu hello [přehled API Trident](http://storm.apache.org/documentation/Trident-API-Overview.html).</span><span class="sxs-lookup"><span data-stu-id="4e5ff-280">For more information about Trident, see hello [Trident API Overview](http://storm.apache.org/documentation/Trident-API-Overview.html).</span></span>

<span data-ttu-id="4e5ff-281">Příklad aplikace Trident naleznete v části [Twitter trendů témata s Apache Storm v HDInsight](hdinsight-storm-twitter-trending.md).</span><span class="sxs-lookup"><span data-stu-id="4e5ff-281">For an example of a Trident application, see [Twitter trending topics with Apache Storm on HDInsight](hdinsight-storm-twitter-trending.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4e5ff-282">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4e5ff-282">Next Steps</span></span>

<span data-ttu-id="4e5ff-283">Jste se naučili, jak toocreate topologie Storm pomocí Java.</span><span class="sxs-lookup"><span data-stu-id="4e5ff-283">You have learned how toocreate a Storm topology by using Java.</span></span> <span data-ttu-id="4e5ff-284">Teď další postup:</span><span class="sxs-lookup"><span data-stu-id="4e5ff-284">Now learn how to:</span></span>

* [<span data-ttu-id="4e5ff-285">Nasazení a správa topologií Apache Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="4e5ff-285">Deploy and manage Apache Storm topologies on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology.md)

* [<span data-ttu-id="4e5ff-286">Vývoj topologie C# pro Apache Storm v HDInsight pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4e5ff-286">Develop C# topologies for Apache Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)

<span data-ttu-id="4e5ff-287">Můžete najít další příklad topologií Storm navštivte stránky [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="4e5ff-287">You can find more example Storm topologies by visiting [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>

