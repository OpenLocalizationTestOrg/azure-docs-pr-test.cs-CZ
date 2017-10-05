---
title: "Apache Storm příkladu Java topologie – Azure HDInsight | Microsoft Docs"
description: "Naučte se vytvářet topologií Apache Storm v jazyce Java tak, že vytvoříte ukázkové topologie počet aplikace word."
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
ms.openlocfilehash: 36285fbaf1da3c566d338bd5612eebad327eaf50
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-apache-storm-topology-in-java"></a><span data-ttu-id="8035f-104">Vytvoření topologie Apache Storm v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="8035f-104">Create an Apache Storm topology in Java</span></span>

<span data-ttu-id="8035f-105">Naučte se vytvářet topologii založené na jazyce Java pro Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="8035f-105">Learn how to create a Java-based topology for Apache Storm.</span></span> <span data-ttu-id="8035f-106">Můžete vytvořit topologie Storm, který implementuje počtu slov aplikace.</span><span class="sxs-lookup"><span data-stu-id="8035f-106">You create a Storm topology that implements a word-count application.</span></span> <span data-ttu-id="8035f-107">Používáte Maven k sestavení a balíček projektu.</span><span class="sxs-lookup"><span data-stu-id="8035f-107">You use Maven to build and package the project.</span></span> <span data-ttu-id="8035f-108">Pak zjistíte, jak definovat topologie pomocí rozhraní tok.</span><span class="sxs-lookup"><span data-stu-id="8035f-108">Then, you learn how to define the topology using the Flux framework.</span></span>

> [!NOTE]
> <span data-ttu-id="8035f-109">Rozhraní framework tok je k dispozici v Storm 0.10.0 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="8035f-109">The Flux framework is available in Storm 0.10.0 or later.</span></span> <span data-ttu-id="8035f-110">Storm 0.10.0 je k dispozici s HDInsight 3.3 a 3.4.</span><span class="sxs-lookup"><span data-stu-id="8035f-110">Storm 0.10.0 is available with HDInsight 3.3 and 3.4.</span></span>

<span data-ttu-id="8035f-111">Po dokončení kroků v tomto dokumentu, můžete nasadit topologie do Apache Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8035f-111">After completing the steps in this document, you can deploy the topology to Apache Storm on HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="8035f-112">Dokončené verze příkladů topologie Storm vytvořené v tomto dokumentu je k dispozici na [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).</span><span class="sxs-lookup"><span data-stu-id="8035f-112">A completed version of the Storm topology examples created in this document is available at [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8035f-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8035f-113">Prerequisites</span></span>

* [<span data-ttu-id="8035f-114">Java Developer Kit (JDK) verze 7</span><span class="sxs-lookup"><span data-stu-id="8035f-114">Java Developer Kit (JDK) version 7</span></span>](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)

* <span data-ttu-id="8035f-115">[Maven (https://maven.apache.org/download.cgi)](https://maven.apache.org/download.cgi): Maven je systém sestavení projektu pro projekty Java.</span><span class="sxs-lookup"><span data-stu-id="8035f-115">[Maven (https://maven.apache.org/download.cgi)](https://maven.apache.org/download.cgi): Maven is a project build system for Java projects.</span></span>

* <span data-ttu-id="8035f-116">Textového editoru nebo IDE.</span><span class="sxs-lookup"><span data-stu-id="8035f-116">A text editor or IDE.</span></span>

## <a name="configure-environment-variables"></a><span data-ttu-id="8035f-117">Nakonfigurujte proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="8035f-117">Configure environment variables</span></span>

<span data-ttu-id="8035f-118">Následující proměnné prostředí může být nastaven při instalaci Java a sadu JDK.</span><span class="sxs-lookup"><span data-stu-id="8035f-118">The following environment variables may be set when you install Java and the JDK.</span></span> <span data-ttu-id="8035f-119">Nicméně byste měli zkontrolovat, že existují a že obsahují správné hodnoty pro váš systém.</span><span class="sxs-lookup"><span data-stu-id="8035f-119">However, you should check that they exist and that they contain the correct values for your system.</span></span>

* <span data-ttu-id="8035f-120">**JAVA_HOME** -by měla odkazovat na adresář, kam nainstalovat prostředí Java runtime (JRE).</span><span class="sxs-lookup"><span data-stu-id="8035f-120">**JAVA_HOME** - should point to the directory where the Java runtime environment (JRE) is installed.</span></span> <span data-ttu-id="8035f-121">Například v distribuci systému Unix nebo Linux, musí mít hodnotu podobnou `/usr/lib/jvm/java-7-oracle`.</span><span class="sxs-lookup"><span data-stu-id="8035f-121">For example, in a Unix or Linux distribution, it should have a value similar to `/usr/lib/jvm/java-7-oracle`.</span></span> <span data-ttu-id="8035f-122">V systému Windows má hodnotu podobnou`c:\Program Files (x86)\Java\jre1.7`</span><span class="sxs-lookup"><span data-stu-id="8035f-122">In Windows, it would have a value similar to `c:\Program Files (x86)\Java\jre1.7`</span></span>

* <span data-ttu-id="8035f-123">**CESTA** -musí obsahovat následující cesty:</span><span class="sxs-lookup"><span data-stu-id="8035f-123">**PATH** - should contain the following paths:</span></span>

  * <span data-ttu-id="8035f-124">**JAVA_HOME** (nebo ekvivalentní cesta)</span><span class="sxs-lookup"><span data-stu-id="8035f-124">**JAVA_HOME** (or the equivalent path)</span></span>

  * <span data-ttu-id="8035f-125">**JAVA_HOME\bin** (nebo ekvivalentní cesta)</span><span class="sxs-lookup"><span data-stu-id="8035f-125">**JAVA_HOME\bin** (or the equivalent path)</span></span>

  * <span data-ttu-id="8035f-126">Adresář, kde je nainstalován Maven</span><span class="sxs-lookup"><span data-stu-id="8035f-126">The directory where Maven is installed</span></span>

## <a name="create-a-maven-project"></a><span data-ttu-id="8035f-127">Vytvořte projekt Maven</span><span class="sxs-lookup"><span data-stu-id="8035f-127">Create a Maven project</span></span>

<span data-ttu-id="8035f-128">Z příkazového řádku, použijte následující příkaz k vytvoření Maven projektu s názvem **WordCount**:</span><span class="sxs-lookup"><span data-stu-id="8035f-128">From the command line, use the following command to create a Maven project named **WordCount**:</span></span>

```bash
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false
```

> [!NOTE]
> <span data-ttu-id="8035f-129">Pokud používáte prostředí PowerShell, je třeba vložit`-D` parametry s dvojitými uvozovkami.</span><span class="sxs-lookup"><span data-stu-id="8035f-129">If you are using PowerShell, you must surround the`-D` parameters with double quotes.</span></span>
>
> `mvn archetype:generate "-DarchetypeArtifactId=maven-archetype-quickstart" "-DgroupId=com.microsoft.example" "-DartifactId=WordCount" "-DinteractiveMode=false"`

<span data-ttu-id="8035f-130">Tento příkaz vytvoří adresář s názvem `WordCount` do aktuálního umístění, která obsahuje základní projekt Maven.</span><span class="sxs-lookup"><span data-stu-id="8035f-130">This command creates a directory named `WordCount` at the current location, which contains a basic Maven project.</span></span> <span data-ttu-id="8035f-131">`WordCount` Adresář obsahuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="8035f-131">The `WordCount` directory contains the following items:</span></span>

* <span data-ttu-id="8035f-132">`pom.xml`: Obsahuje nastavení pro projekt Maven.</span><span class="sxs-lookup"><span data-stu-id="8035f-132">`pom.xml`: Contains settings for the Maven project.</span></span>
* <span data-ttu-id="8035f-133">`src\main\java\com\microsoft\example`: Obsahuje kód aplikace.</span><span class="sxs-lookup"><span data-stu-id="8035f-133">`src\main\java\com\microsoft\example`: Contains your application code.</span></span>
* <span data-ttu-id="8035f-134">`src\test\java\com\microsoft\example`: Obsahuje testy pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8035f-134">`src\test\java\com\microsoft\example`: Contains tests for your application.</span></span> 

### <a name="remove-the-generated-example-code"></a><span data-ttu-id="8035f-135">Odebrat generovaný ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="8035f-135">Remove the generated example code</span></span>

<span data-ttu-id="8035f-136">Odstraňte generovaný test a soubory aplikace:</span><span class="sxs-lookup"><span data-stu-id="8035f-136">Delete the generated test and the application files:</span></span>

* <span data-ttu-id="8035f-137">**src\test\java\com\microsoft\example\AppTest.Java**</span><span class="sxs-lookup"><span data-stu-id="8035f-137">**src\test\java\com\microsoft\example\AppTest.java**</span></span>
* <span data-ttu-id="8035f-138">**src\main\java\com\microsoft\example\App.Java**</span><span class="sxs-lookup"><span data-stu-id="8035f-138">**src\main\java\com\microsoft\example\App.java**</span></span>

## <a name="add-maven-repositories"></a><span data-ttu-id="8035f-139">Přidání úložiště Maven</span><span class="sxs-lookup"><span data-stu-id="8035f-139">Add Maven repositories</span></span>

<span data-ttu-id="8035f-140">HDInsight je založena na Hortonworks Data Platform (HDP), takže vám doporučujeme používat úložiště Hortonworks ke stažení závislostí pro projekty Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="8035f-140">HDInsight is based on the Hortonworks Data Platform (HDP), so we recommend using the Hortonworks repository to download dependencies for your Apache Storm projects.</span></span> <span data-ttu-id="8035f-141">V __pom.xml__ soubor, přidejte následující kód XML po `<url>http://maven.apache.org</url>` řádku:</span><span class="sxs-lookup"><span data-stu-id="8035f-141">In the __pom.xml__ file, add the following XML after the `<url>http://maven.apache.org</url>` line:</span></span>

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

## <a name="add-properties"></a><span data-ttu-id="8035f-142">Přidání vlastnosti</span><span class="sxs-lookup"><span data-stu-id="8035f-142">Add properties</span></span>

<span data-ttu-id="8035f-143">Maven umožňuje definovat hodnoty úrovni projektu názvem vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8035f-143">Maven allows you to define project-level values called properties.</span></span> <span data-ttu-id="8035f-144">V __pom.xml__, přidejte následující text po `</repositories>` řádku:</span><span class="sxs-lookup"><span data-stu-id="8035f-144">In the __pom.xml__, add the following text after the `</repositories>` line:</span></span>

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--
    This is a version of Storm from the Hortonworks repository that is compatible with HDInsight.
    -->
    <storm.version>1.0.1.2.5.3.0-37</storm.version>
</properties>
```

<span data-ttu-id="8035f-145">Teď můžete použít tuto hodnotu v dalších částech `pom.xml`.</span><span class="sxs-lookup"><span data-stu-id="8035f-145">You can now use this value in other sections of the `pom.xml`.</span></span> <span data-ttu-id="8035f-146">Například při určení verze součástí Storm, můžete použít `${storm.version}` místo pevné kódování hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8035f-146">For example, when specifying the version of Storm components, you can use `${storm.version}` instead of hard coding a value.</span></span>

## <a name="add-dependencies"></a><span data-ttu-id="8035f-147">Přidat závislosti</span><span class="sxs-lookup"><span data-stu-id="8035f-147">Add dependencies</span></span>

<span data-ttu-id="8035f-148">Přidáte závislost pro komponenty Storm.</span><span class="sxs-lookup"><span data-stu-id="8035f-148">Add a dependency for Storm components.</span></span> <span data-ttu-id="8035f-149">Otevřete `pom.xml` souboru a přidejte následující kód `<dependencies>` části:</span><span class="sxs-lookup"><span data-stu-id="8035f-149">Open the `pom.xml` file and add the following code in the `<dependencies>` section:</span></span>

```xml
<dependency>
    <groupId>org.apache.storm</groupId>
    <artifactId>storm-core</artifactId>
    <version>${storm.version}</version>
    <!-- keep storm out of the jar-with-dependencies -->
    <scope>provided</scope>
</dependency>
```

<span data-ttu-id="8035f-150">Při kompilaci, Maven používá tuto informaci k vyhledání `storm-core` v úložišti Maven.</span><span class="sxs-lookup"><span data-stu-id="8035f-150">At compile time, Maven uses this information to look up `storm-core` in the Maven repository.</span></span> <span data-ttu-id="8035f-151">Nejprve hledá v úložišti místního počítače.</span><span class="sxs-lookup"><span data-stu-id="8035f-151">It first looks in the repository on your local computer.</span></span> <span data-ttu-id="8035f-152">Nejsou-li soubory existuje, Maven je stáhne z veřejného úložiště Maven a ukládá je v místním úložišti.</span><span class="sxs-lookup"><span data-stu-id="8035f-152">If the files aren't there, Maven downloads them from the public Maven repository and stores them in the local repository.</span></span>

> [!NOTE]
> <span data-ttu-id="8035f-153">Upozornění `<scope>provided</scope>` řádek v této části.</span><span class="sxs-lookup"><span data-stu-id="8035f-153">Notice the `<scope>provided</scope>` line in this section.</span></span> <span data-ttu-id="8035f-154">Toto nastavení určuje Maven vyloučit **storm základní** z všechny JAR soubory, které jsou vytvořeny, protože je k dispozici v systému.</span><span class="sxs-lookup"><span data-stu-id="8035f-154">This setting tells Maven to exclude **storm-core** from any JAR files that are created, because it is provided by the system.</span></span>

## <a name="build-configuration"></a><span data-ttu-id="8035f-155">Konfigurace sestavení</span><span class="sxs-lookup"><span data-stu-id="8035f-155">Build configuration</span></span>

<span data-ttu-id="8035f-156">Moduly plug-in maven umožňují přizpůsobit fáze sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="8035f-156">Maven plug-ins allow you to customize the build stages of the project.</span></span> <span data-ttu-id="8035f-157">Například jak kompilace projektu nebo jak zabalit do soubor JAR.</span><span class="sxs-lookup"><span data-stu-id="8035f-157">For example, how the project is compiled or how to package it into a JAR file.</span></span> <span data-ttu-id="8035f-158">Otevřete `pom.xml` souboru a přidejte následující kód přímo výše `</project>` řádku.</span><span class="sxs-lookup"><span data-stu-id="8035f-158">Open the `pom.xml` file and add the following code directly above the `</project>` line.</span></span>

```xml
<build>
    <plugins>
    </plugins>
    <resources>
    </resources>
</build>
```

<span data-ttu-id="8035f-159">V této části se používá k přidání modulů plug-in, prostředky a další možnosti konfigurace sestavení.</span><span class="sxs-lookup"><span data-stu-id="8035f-159">This section is used to add plug-ins, resources, and other build configuration options.</span></span> <span data-ttu-id="8035f-160">Úplný přehled o **pom.xml** souborů najdete v tématu [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).</span><span class="sxs-lookup"><span data-stu-id="8035f-160">For a full reference of the **pom.xml** file, see [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).</span></span>

### <a name="add-plug-ins"></a><span data-ttu-id="8035f-161">Přidat moduly plug-in</span><span class="sxs-lookup"><span data-stu-id="8035f-161">Add plug-ins</span></span>

<span data-ttu-id="8035f-162">Pro topologií Apache Storm implementována v jazyce Java [modulu plug-in Maven Exec](http://www.mojohaus.org/exec-maven-plugin/) je užitečné, protože umožňuje snadno topologii místní spuštění ve vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="8035f-162">For Apache Storm topologies implemented in Java, the [Exec Maven Plugin](http://www.mojohaus.org/exec-maven-plugin/) is useful because it allows you to easily run the topology locally in your development environment.</span></span> <span data-ttu-id="8035f-163">Přidejte následující `<plugins>` části `pom.xml` souboru modulu plug-in Exec Maven:</span><span class="sxs-lookup"><span data-stu-id="8035f-163">Add the following to the `<plugins>` section of the `pom.xml` file to include the Exec Maven plugin:</span></span>

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

<span data-ttu-id="8035f-164">Další užitečné modul plug-in je [Apache Maven kompilátoru modul plug-in](http://maven.apache.org/plugins/maven-compiler-plugin/), který se používá k změnit možnosti kompilace.</span><span class="sxs-lookup"><span data-stu-id="8035f-164">Another useful plug-in is the [Apache Maven Compiler Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/), which is used to change compilation options.</span></span> <span data-ttu-id="8035f-165">Změny jazyce Java verze, která používá Maven pro zdroje a cíle pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8035f-165">The changes the Java version that Maven uses for the source and target for your application.</span></span>

* <span data-ttu-id="8035f-166">Pro HDInsight __3.4 nebo starším__, nastavte zdroj a cíl verzi Javy k __1.7__.</span><span class="sxs-lookup"><span data-stu-id="8035f-166">For HDInsight __3.4 or earlier__, set the source and target Java version to __1.7__.</span></span>

* <span data-ttu-id="8035f-167">Pro HDInsight __3.5__, nastavte zdroj a cíl verzi Javy k __1.8__.</span><span class="sxs-lookup"><span data-stu-id="8035f-167">For HDInsight __3.5__, set the source and target Java version to __1.8__.</span></span>

<span data-ttu-id="8035f-168">Přidejte následující text do `<plugins>` části `pom.xml` souboru modulu plug-in Apache Maven kompilátoru.</span><span class="sxs-lookup"><span data-stu-id="8035f-168">Add the following text in the `<plugins>` section of the `pom.xml` file to include the Apache Maven Compiler plugin.</span></span> <span data-ttu-id="8035f-169">Tento příklad určuje 1.8, tak, aby cílová HDInsight verze 3.5.</span><span class="sxs-lookup"><span data-stu-id="8035f-169">This example specifies 1.8, so the target HDInsight version is 3.5.</span></span>

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

### <a name="configure-resources"></a><span data-ttu-id="8035f-170">Konfigurace prostředků</span><span class="sxs-lookup"><span data-stu-id="8035f-170">Configure resources</span></span>

<span data-ttu-id="8035f-171">V části prostředky umožňuje zahrnout jiný kód prostředkům, například konfigurační soubory, které jsou potřebné součásti v topologii.</span><span class="sxs-lookup"><span data-stu-id="8035f-171">The resources section allows you to include non-code resources such as configuration files needed by components in the topology.</span></span> <span data-ttu-id="8035f-172">V tomto příkladu přidejte následující text do `<resources>` oddílu se soubor pom.xml.</span><span class="sxs-lookup"><span data-stu-id="8035f-172">For this example, add the following text in the `<resources>` section of the \`pom.xml file.</span></span>

```xml
<resource>
    <directory>${basedir}/resources</directory>
    <filtering>false</filtering>
    <includes>
        <include>log4j2.xml</include>
    </includes>
</resource>
```

<span data-ttu-id="8035f-173">Tento příklad přidá adresáře prostředků v kořenovém adresáři projektu (`${basedir}`) jako umístění, která obsahuje prostředky a obsahuje soubor s názvem `log4j2.xml`.</span><span class="sxs-lookup"><span data-stu-id="8035f-173">This example adds the resources directory in the root of the project (`${basedir}`) as a location that contains resources, and includes the file named `log4j2.xml`.</span></span> <span data-ttu-id="8035f-174">Tento soubor se používá ke konfiguraci, které informace se v topologii protokolu.</span><span class="sxs-lookup"><span data-stu-id="8035f-174">This file is used to configure what information is logged by the topology.</span></span>

## <a name="create-the-topology"></a><span data-ttu-id="8035f-175">Vytvoření topologie</span><span class="sxs-lookup"><span data-stu-id="8035f-175">Create the topology</span></span>

<span data-ttu-id="8035f-176">Topologie založené na jazyce Java Apache Storm se skládá z tři součásti, které musíte napsat (nebo reference) jako závislost.</span><span class="sxs-lookup"><span data-stu-id="8035f-176">A Java-based Apache Storm topology consists of three components that you must author (or reference) as a dependency.</span></span>

* <span data-ttu-id="8035f-177">**Spouts**: načte data z externího zdroje a vysílá datových proudů do topologie.</span><span class="sxs-lookup"><span data-stu-id="8035f-177">**Spouts**: Reads data from external sources and emits streams of data into the topology.</span></span>

* <span data-ttu-id="8035f-178">**Bolts**: provádí zpracování datových proudů vysílaných funkcích spouts nebo jiné funkce bolts a vysílá jeden nebo více datových proudů.</span><span class="sxs-lookup"><span data-stu-id="8035f-178">**Bolts**: Performs processing on streams emitted by spouts or other bolts, and emits one or more streams.</span></span>

* <span data-ttu-id="8035f-179">**Topologie**: definuje, jak funkcích spouts a funkce bolts jsou uspořádány a představuje vstupní bod pro topologii.</span><span class="sxs-lookup"><span data-stu-id="8035f-179">**Topology**: Defines how the spouts and bolts are arranged, and provides the entry point for the topology.</span></span>

### <a name="create-the-spout"></a><span data-ttu-id="8035f-180">Vytvořte spout</span><span class="sxs-lookup"><span data-stu-id="8035f-180">Create the spout</span></span>

<span data-ttu-id="8035f-181">Ke snížení požadavků pro nastavení externích zdrojů dat, následující spout jednoduše vyšle náhodných věty.</span><span class="sxs-lookup"><span data-stu-id="8035f-181">To reduce requirements for setting up external data sources, the following spout simply emits random sentences.</span></span> <span data-ttu-id="8035f-182">Je upravenou verzi spout, který je zadán v rámci [počáteční příklady Storm](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).</span><span class="sxs-lookup"><span data-stu-id="8035f-182">It is a modified version of a spout that is provided with the [Storm-Starter examples](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).</span></span>

> [!NOTE]
> <span data-ttu-id="8035f-183">Příklad funkcí spout, který čte z externího zdroje dat najdete v následujících příkladech:</span><span class="sxs-lookup"><span data-stu-id="8035f-183">For an example of a spout that reads from an external data source, see one of the following examples:</span></span>
>
> * <span data-ttu-id="8035f-184">[TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): Příklad funkcí spout, který čte ze služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="8035f-184">[TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): An example spout that reads from Twitter</span></span>
> * <span data-ttu-id="8035f-185">[Storm Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): spout, který čte z Kafka</span><span class="sxs-lookup"><span data-stu-id="8035f-185">[Storm-Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): A spout that reads from Kafka</span></span>

<span data-ttu-id="8035f-186">Pro spout, vytvořte soubor s názvem `RandomSentenceSpout.java` v `src\main\java\com\microsoft\example` adresáře a použijte následující Java kód jako obsah:</span><span class="sxs-lookup"><span data-stu-id="8035f-186">For the spout, create a file named `RandomSentenceSpout.java` in the `src\main\java\com\microsoft\example` directory and use the following Java code as the contents:</span></span>

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
  //Collector used to emit output
  SpoutOutputCollector _collector;
  //Used to generate a random number
  Random _rand;

  //Open is called when an instance of the class is created
  @Override
  public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
  //Set the instance collector to the one passed in
    _collector = collector;
    //For randomness
    _rand = new Random();
  }

  //Emit data to the stream
  @Override
  public void nextTuple() {
  //Sleep for a bit
    Utils.sleep(100);
    //The sentences that are randomly emitted
    String[] sentences = new String[]{ "the cow jumped over the moon", "an apple a day keeps the doctor away",
        "four score and seven years ago", "snow white and the seven dwarfs", "i am at two with nature" };
    //Randomly pick a sentence
    String sentence = sentences[_rand.nextInt(sentences.length)];
    //Emit the sentence
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

  //Declare the output fields. In this case, an sentence
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("sentence"));
  }
}
```

> [!NOTE]
> <span data-ttu-id="8035f-187">I když tato topologie používá jenom jeden spout, ostatní může mít několik, které data datového kanálu z různých zdrojů do topologie.</span><span class="sxs-lookup"><span data-stu-id="8035f-187">Although this topology uses only one spout, others may have several that feed data from different sources into the topology.</span></span>

### <a name="create-the-bolts"></a><span data-ttu-id="8035f-188">Vytvořte funkce bolts</span><span class="sxs-lookup"><span data-stu-id="8035f-188">Create the bolts</span></span>

<span data-ttu-id="8035f-189">Funkce Bolts zpracovávat data zpracování.</span><span class="sxs-lookup"><span data-stu-id="8035f-189">Bolts handle the data processing.</span></span> <span data-ttu-id="8035f-190">Tato topologie používá dvě funkce bolts:</span><span class="sxs-lookup"><span data-stu-id="8035f-190">This topology uses two bolts:</span></span>

* <span data-ttu-id="8035f-191">**SplitSentence**: rozdělí věty vysílaných **RandomSentenceSpout** do jednotlivých slov.</span><span class="sxs-lookup"><span data-stu-id="8035f-191">**SplitSentence**: Splits the sentences emitted by **RandomSentenceSpout** into individual words.</span></span>

* <span data-ttu-id="8035f-192">**WordCount**: Spočítá počet opakování jednotlivých slov došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="8035f-192">**WordCount**: Counts how many times each word has occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="8035f-193">Funkce Bolts dělat cokoliv, například výpočet, trvalost nebo rozhovoru externích součástí.</span><span class="sxs-lookup"><span data-stu-id="8035f-193">Bolts can do anything, for example, computation, persistence, or talking to external components.</span></span>

<span data-ttu-id="8035f-194">Vytvořte dva nové soubory, `SplitSentence.java` a `WordCount.java` v `src\main\java\com\microsoft\example` adresáře.</span><span class="sxs-lookup"><span data-stu-id="8035f-194">Create two new files, `SplitSentence.java` and `WordCount.java` in the `src\main\java\com\microsoft\example` directory.</span></span> <span data-ttu-id="8035f-195">Použijte následující text jako obsah pro soubory:</span><span class="sxs-lookup"><span data-stu-id="8035f-195">Use the following text as the contents for the files:</span></span>

#### <a name="splitsentence"></a><span data-ttu-id="8035f-196">SplitSentence</span><span class="sxs-lookup"><span data-stu-id="8035f-196">SplitSentence</span></span>

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

  //Execute is called to process tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //Get the sentence content from the tuple
    String sentence = tuple.getString(0);
    //An iterator to get each word
    BreakIterator boundary=BreakIterator.getWordInstance();
    //Give the iterator the sentence
    boundary.setText(sentence);
    //Find the beginning first word
    int start=boundary.first();
    //Iterate over each word and emit it to the output stream
    for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
      //get the word
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

#### <a name="wordcount"></a><span data-ttu-id="8035f-197">WordCount</span><span class="sxs-lookup"><span data-stu-id="8035f-197">WordCount</span></span>

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
  //How often to emit a count of words
  private Integer emitFrequency;

  // Default constructor
  public WordCount() {
      emitFrequency=5; // Default to 60 seconds
  }

  // Constructor that sets emit frequency
  public WordCount(Integer frequency) {
      emitFrequency=frequency;
  }

  //Configure frequency of tick tuples for this bolt
  //This delivers a 'tick' tuple on a specific interval,
  //which is used to trigger certain actions
  @Override
  public Map<String, Object> getComponentConfiguration() {
      Config conf = new Config();
      conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
      return conf;
  }

  //execute is called to process tuples
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
      //Get the word contents from the tuple
      String word = tuple.getString(0);
      //Have we counted any already?
      Integer count = counts.get(word);
      if (count == null)
        count = 0;
      //Increment the count and store it
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

### <a name="define-the-topology"></a><span data-ttu-id="8035f-198">Definovat topologii</span><span class="sxs-lookup"><span data-stu-id="8035f-198">Define the topology</span></span>

<span data-ttu-id="8035f-199">Topologie sváže funkcích spouts a bolts společně na graf, který definuje, jak se data proudí mezi součástmi.</span><span class="sxs-lookup"><span data-stu-id="8035f-199">The topology ties the spouts and bolts together into a graph, which defines how data flows between the components.</span></span> <span data-ttu-id="8035f-200">Nabízí taky paralelismus pomocné parametry, které Storm používá při vytváření instancí komponent v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="8035f-200">It also provides parallelism hints that Storm uses when creating instances of the components within the cluster.</span></span>

<span data-ttu-id="8035f-201">Na následujícím obrázku je základní diagram grafu součástí této topologii.</span><span class="sxs-lookup"><span data-stu-id="8035f-201">The following image is a basic diagram of the graph of components for this topology.</span></span>

![Diagram zobrazující uspořádání funkcích spouts a funkce bolts](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

<span data-ttu-id="8035f-203">Pokud chcete implementovat topologii, vytvořte soubor s názvem `WordCountTopology.java` v `src\main\java\com\microsoft\example` adresáře.</span><span class="sxs-lookup"><span data-stu-id="8035f-203">To implement the topology, create a file named `WordCountTopology.java` in the `src\main\java\com\microsoft\example` directory.</span></span> <span data-ttu-id="8035f-204">Použijte následující kód Java jako obsah souboru:</span><span class="sxs-lookup"><span data-stu-id="8035f-204">Use the following Java code as the contents of the file:</span></span>

```java
package com.microsoft.example;

import org.apache.storm.Config;
import org.apache.storm.LocalCluster;
import org.apache.storm.StormSubmitter;
import org.apache.storm.topology.TopologyBuilder;
import org.apache.storm.tuple.Fields;

import com.microsoft.example.RandomSentenceSpout;

public class WordCountTopology {

  //Entry point for the topology
  public static void main(String[] args) throws Exception {
  //Used to build the topology
    TopologyBuilder builder = new TopologyBuilder();
    //Add the spout, with a name of 'spout'
    //and parallelism hint of 5 executors
    builder.setSpout("spout", new RandomSentenceSpout(), 5);
    //Add the SplitSentence bolt, with a name of 'split'
    //and parallelism hint of 8 executors
    //shufflegrouping subscribes to the spout, and equally distributes
    //tuples (sentences) across instances of the SplitSentence bolt
    builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
    //Add the counter, with a name of 'count'
    //and parallelism hint of 12 executors
    //fieldsgrouping subscribes to the split bolt, and
    //ensures that the same word is sent to the same instance (group by field 'word')
    builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

    //new configuration
    Config conf = new Config();
    //Set to false to disable debug information when
    // running in production on a cluster
    conf.setDebug(false);

    //If there are arguments, we are running on a cluster
    if (args != null && args.length > 0) {
      //parallelism hint to set the number of workers
      conf.setNumWorkers(3);
      //submit the topology
      StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
    }
    //Otherwise, we are running locally
    else {
      //Cap the maximum number of executors that can be spawned
      //for a component to 3
      conf.setMaxTaskParallelism(3);
      //LocalCluster is used to run locally
      LocalCluster cluster = new LocalCluster();
      //submit the topology
      cluster.submitTopology("word-count", conf, builder.createTopology());
      //sleep
      Thread.sleep(10000);
      //shut down the cluster
      cluster.shutdown();
    }
  }
}
```

### <a name="configure-logging"></a><span data-ttu-id="8035f-205">Konfigurace protokolování</span><span class="sxs-lookup"><span data-stu-id="8035f-205">Configure logging</span></span>

<span data-ttu-id="8035f-206">Storm používá Apache Log4j k ukládání informací.</span><span class="sxs-lookup"><span data-stu-id="8035f-206">Storm uses Apache Log4j to log information.</span></span> <span data-ttu-id="8035f-207">Pokud neprovedete konfiguraci protokolování, topologii vysílá diagnostické informace.</span><span class="sxs-lookup"><span data-stu-id="8035f-207">If you do not configure logging, the topology emits diagnostic information.</span></span> <span data-ttu-id="8035f-208">Pokud chcete řídit, co je protokolováno, vytvořte soubor s názvem `log4j2.xml` v `resources` adresáře.</span><span class="sxs-lookup"><span data-stu-id="8035f-208">To control what is logged, create a file named `log4j2.xml` in the `resources` directory.</span></span> <span data-ttu-id="8035f-209">Použijte následující kód XML jako obsah souboru.</span><span class="sxs-lookup"><span data-stu-id="8035f-209">Use the following XML as the contents of the file.</span></span>

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

<span data-ttu-id="8035f-210">Tato konfigurace XML nakonfiguruje nové protokoly pro `com.microsoft.example` třída, která obsahuje součásti v této topologii příklad.</span><span class="sxs-lookup"><span data-stu-id="8035f-210">This XML configures a new logger for the `com.microsoft.example` class, which includes the components in this example topology.</span></span> <span data-ttu-id="8035f-211">Pro tohoto protokolovacího nástroje, které jsou zaznamenány žádné informace o protokolování vygenerované součásti v této topologii je nastavena úroveň pro trasování.</span><span class="sxs-lookup"><span data-stu-id="8035f-211">The level is set to trace for this logger, which captures any logging information emitted by components in this topology.</span></span>

<span data-ttu-id="8035f-212">`<Root level="error">` Části nakonfiguruje kořenové úrovni protokolování (vše, co není ve `com.microsoft.example`) se protokolovat jenom informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="8035f-212">The `<Root level="error">` section configures the root level of logging (everything not in `com.microsoft.example`) to only log error information.</span></span>

<span data-ttu-id="8035f-213">Další informace o konfiguraci protokolování pro Log4j najdete v tématu [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).</span><span class="sxs-lookup"><span data-stu-id="8035f-213">For more information on configuring logging for Log4j, see [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).</span></span>

> [!NOTE]
> <span data-ttu-id="8035f-214">Storm verze 0.10.0 a vyšší využití Log4j 2.x.</span><span class="sxs-lookup"><span data-stu-id="8035f-214">Storm version 0.10.0 and higher use Log4j 2.x.</span></span> <span data-ttu-id="8035f-215">Starší verze storm použít Log4j 1.x, který používá jiný formát pro konfiguraci protokolu.</span><span class="sxs-lookup"><span data-stu-id="8035f-215">Older versions of storm used Log4j 1.x, which used a different format for log configuration.</span></span> <span data-ttu-id="8035f-216">Informace o konfiguraci starší, najdete v části [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).</span><span class="sxs-lookup"><span data-stu-id="8035f-216">For information on the older configuration, see [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).</span></span>

## <a name="test-the-topology-locally"></a><span data-ttu-id="8035f-217">Testování topologie místně</span><span class="sxs-lookup"><span data-stu-id="8035f-217">Test the topology locally</span></span>

<span data-ttu-id="8035f-218">Po uložení souborů, použijte následující příkaz k testování topologie místně.</span><span class="sxs-lookup"><span data-stu-id="8035f-218">After you save the files, use the following command to test the topology locally.</span></span>

```bash
mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology
```

<span data-ttu-id="8035f-219">Při jeho spuštění, topologii zobrazí informace o spuštění.</span><span class="sxs-lookup"><span data-stu-id="8035f-219">As it runs, the topology displays startup information.</span></span> <span data-ttu-id="8035f-220">Tento text je příklad výstupu, počet word:</span><span class="sxs-lookup"><span data-stu-id="8035f-220">The following text is an example of the word count output:</span></span>

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

<span data-ttu-id="8035f-221">Tento příklad protokol znamená, že slovo 'a' má byla vygenerované 113 časy.</span><span class="sxs-lookup"><span data-stu-id="8035f-221">This example log indicates that the word 'and' has been emitted 113 times.</span></span> <span data-ttu-id="8035f-222">Počet i nadále přejít, dokud topologie běží, protože funkcí spout nepřetržitě vysílá stejné věty.</span><span class="sxs-lookup"><span data-stu-id="8035f-222">The count continues to go up as long as the topology runs because the spout continuously emits the same sentences.</span></span>

<span data-ttu-id="8035f-223">Mezi emisí slova a počty je interval 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="8035f-223">There is a 5-second interval between emission of words and counts.</span></span> <span data-ttu-id="8035f-224">**WordCount** součást nakonfigurovaná pro vydávání pouze informace, pokud dorazí značek řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="8035f-224">The **WordCount** component is configured to only emit information when a tick tuple arrives.</span></span> <span data-ttu-id="8035f-225">Požaduje této značky řazených kolekcí členů jsou pouze doručovány každých pět sekund.</span><span class="sxs-lookup"><span data-stu-id="8035f-225">It requests that tick tuples are only delivered every five seconds.</span></span>

## <a name="convert-the-topology-to-flux"></a><span data-ttu-id="8035f-226">Převést topologii tok</span><span class="sxs-lookup"><span data-stu-id="8035f-226">Convert the topology to Flux</span></span>

<span data-ttu-id="8035f-227">Tok je nové rozhraní Storm 0.10.0 k dispozici a vyšší, což umožňuje oddělit konfiguraci z implementace.</span><span class="sxs-lookup"><span data-stu-id="8035f-227">Flux is a new framework available with Storm 0.10.0 and higher, which allows you to separate configuration from implementation.</span></span> <span data-ttu-id="8035f-228">Vaše komponenty jsou stále definována v jazyce Java, ale topologii je definována pomocí souboru YAML.</span><span class="sxs-lookup"><span data-stu-id="8035f-228">Your components are still defined in Java, but the topology is defined using a YAML file.</span></span> <span data-ttu-id="8035f-229">Můžete balíček definice výchozí topologie s projektu, nebo použijte samostatný soubor při odesílání topologie.</span><span class="sxs-lookup"><span data-stu-id="8035f-229">You can package a default topology definition with your project, or use a standalone file when submitting the topology.</span></span> <span data-ttu-id="8035f-230">Při odesílání topologie do Storm, můžete použít k naplnění hodnoty v definici topologie YAML proměnné prostředí nebo konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="8035f-230">When submitting the topology to Storm, you can use environment variables or configuration files to populate values in the YAML topology definition.</span></span>

<span data-ttu-id="8035f-231">Soubor YAML definuje součásti, které budou používat pro topologii a data tok mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="8035f-231">The YAML file defines the components to use for the topology and the data flow between them.</span></span> <span data-ttu-id="8035f-232">Jako součást na soubor jar můžete zahrnout soubor YAML nebo můžete použít externí soubor YAML.</span><span class="sxs-lookup"><span data-stu-id="8035f-232">You can include a YAML file as part of the jar file or you can use an external YAML file.</span></span>

<span data-ttu-id="8035f-233">Další informace o toku najdete v tématu [tok framework (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="8035f-233">For more information on Flux, see [Flux framework (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span></span>

> [!WARNING]
> <span data-ttu-id="8035f-234">Z důvodu [chyb (https://issues.apache.org/jira/browse/STORM-2055)](https://issues.apache.org/jira/browse/STORM-2055) s Storm 1.0.1, budete muset nainstalovat [Storm vývojového prostředí](https://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) tok topologie spouštět místně.</span><span class="sxs-lookup"><span data-stu-id="8035f-234">Due to a [bug (https://issues.apache.org/jira/browse/STORM-2055)](https://issues.apache.org/jira/browse/STORM-2055) with Storm 1.0.1, you may need to install a [Storm development environment](https://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) to run Flux topologies locally.</span></span>

1. <span data-ttu-id="8035f-235">Přesunout `WordCountTopology.java` souboru mimo projekt.</span><span class="sxs-lookup"><span data-stu-id="8035f-235">Move the `WordCountTopology.java` file out of the project.</span></span> <span data-ttu-id="8035f-236">Tento soubor dříve, definované topologii, ale není potřeba s tokem.</span><span class="sxs-lookup"><span data-stu-id="8035f-236">Previously, this file defined the topology, but isn't needed with Flux.</span></span>

2. <span data-ttu-id="8035f-237">V `resources` adresáře, vytvořte soubor s názvem `topology.yaml`.</span><span class="sxs-lookup"><span data-stu-id="8035f-237">In the `resources` directory, create a file named `topology.yaml`.</span></span> <span data-ttu-id="8035f-238">Použijte následující text jako obsah tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="8035f-238">Use the following text as the contents of this file.</span></span>

        name: "wordcount"       # friendly name for the topology
        
        config:                 # Topology configuration
        topology.workers: 1     # Hint for the number of workers to create
        
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
            from: "sentence-spout"       # The stream emitter
            to: "splitter-bolt"          # The stream consumer
            grouping:                    # Grouping type
                type: SHUFFLE
          
            - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
                args: ["word"]           # field(s) to group on

3. <span data-ttu-id="8035f-239">Proveďte následující změny na `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="8035f-239">Make the following changes to the `pom.xml` file.</span></span>
   
   * <span data-ttu-id="8035f-240">Přidejte následující závislost nové v `<dependencies>` části:</span><span class="sxs-lookup"><span data-stu-id="8035f-240">Add the following new dependency in the `<dependencies>` section:</span></span>
     
        ```xml
        <!-- Add a dependency on the Flux framework -->
        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>flux-core</artifactId>
            <version>${storm.version}</version>
        </dependency>
        ```
   * <span data-ttu-id="8035f-241">Přidejte následující modul plug-in, který `<plugins>` části.</span><span class="sxs-lookup"><span data-stu-id="8035f-241">Add the following plugin to the `<plugins>` section.</span></span> <span data-ttu-id="8035f-242">Tento modul plug-in zpracovává vytvoření balíčku (soubor jar) pro projekt a platí některé transformace, které jsou specifické pro tok při vytváření balíčku.</span><span class="sxs-lookup"><span data-stu-id="8035f-242">This plugin handles the creation of a package (jar file) for the project, and applies some transformations specific to Flux when creating the package.</span></span>
     
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
                    <!-- We're using Flux, so refer to it as main -->
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

   * <span data-ttu-id="8035f-243">V **modulu plug-in exec maven** `<configuration>` část, změňte hodnotu `<mainClass>` k `org.apache.storm.flux.Flux`.</span><span class="sxs-lookup"><span data-stu-id="8035f-243">In the **exec-maven-plugin** `<configuration>` section, change the value for `<mainClass>` to `org.apache.storm.flux.Flux`.</span></span> <span data-ttu-id="8035f-244">Toto nastavení umožňuje tok pro zpracování topologii spuštěn místně v vývoj.</span><span class="sxs-lookup"><span data-stu-id="8035f-244">This setting allows Flux to handle running the topology locally in development.</span></span>

   * <span data-ttu-id="8035f-245">V `<resources>` přidejte následující příkaz a `<includes>`.</span><span class="sxs-lookup"><span data-stu-id="8035f-245">In the `<resources>` section, add the following to the `<includes>`.</span></span> <span data-ttu-id="8035f-246">Tato konfigurace XML obsahuje YAML soubor, který definuje topologii v rámci projektu.</span><span class="sxs-lookup"><span data-stu-id="8035f-246">This XML includes the YAML file that defines the topology as part of the project.</span></span>

        ```xml
        <include>topology.yaml</include>
        ```

## <a name="test-the-flux-topology-locally"></a><span data-ttu-id="8035f-247">Testování topologii tok místně</span><span class="sxs-lookup"><span data-stu-id="8035f-247">Test the flux topology locally</span></span>

1. <span data-ttu-id="8035f-248">Pro zkompilování a spuštění tok topologie pomocí nástroje Maven, použijte následující:</span><span class="sxs-lookup"><span data-stu-id="8035f-248">Use the following to compile and execute the Flux topology using Maven:</span></span>

    ```bash
    mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    ```

    <span data-ttu-id="8035f-249">Pokud používáte prostředí PowerShell, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8035f-249">If you are using PowerShell, use the following command:</span></span>

    ```bash
    mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"
    ```

    > [!WARNING]
    > <span data-ttu-id="8035f-250">Pokud vaše topologie používá Storm 1.0.1 bits, tento příkaz se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="8035f-250">If your topology uses Storm 1.0.1 bits, this command fails.</span></span> <span data-ttu-id="8035f-251">Tato chyba je způsobená [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055).</span><span class="sxs-lookup"><span data-stu-id="8035f-251">This failure is caused by [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055).</span></span> <span data-ttu-id="8035f-252">Místo toho [nainstalovat Storm ve vašem vývojovém prostředí](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html) a použijte následující informace.</span><span class="sxs-lookup"><span data-stu-id="8035f-252">Instead, [install Storm in your development environment](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html) and use the following information.</span></span>

    <span data-ttu-id="8035f-253">Pokud máte [nainstalovaný ve vašem vývojovém prostředí Storm](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), můžete místo toho použít následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="8035f-253">If you have [installed Storm in your development environment](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), you can use the following commands instead:</span></span>

    ```bash
    mvn compile package
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml
    ```

    <span data-ttu-id="8035f-254">`--local` Parametr spustí topologii v místním režimu ve vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="8035f-254">The `--local` parameter runs the topology in local mode on your development environment.</span></span> <span data-ttu-id="8035f-255">`-R /topology.yaml` Parametr používá `topology.yaml` souborů prostředků z na soubor jar definovat topologii.</span><span class="sxs-lookup"><span data-stu-id="8035f-255">The `-R /topology.yaml` parameter uses the `topology.yaml` file resource from the jar file to define the topology.</span></span>

    <span data-ttu-id="8035f-256">Při jeho spuštění, topologii zobrazí informace o spuštění.</span><span class="sxs-lookup"><span data-stu-id="8035f-256">As it runs, the topology displays startup information.</span></span> <span data-ttu-id="8035f-257">Tento text je příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="8035f-257">The following text is an example of the output:</span></span>

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs

    <span data-ttu-id="8035f-258">Mezi listy protokolovaných informací je prodlevu 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="8035f-258">There is a 10-second delay between batches of logged information.</span></span>

2. <span data-ttu-id="8035f-259">Vytvořit kopii `topology.yaml` souboru z projektu.</span><span class="sxs-lookup"><span data-stu-id="8035f-259">Make a copy of the `topology.yaml` file from the project.</span></span> <span data-ttu-id="8035f-260">Název nového souboru `newtopology.yaml`.</span><span class="sxs-lookup"><span data-stu-id="8035f-260">Name the new file `newtopology.yaml`.</span></span> <span data-ttu-id="8035f-261">V `newtopology.yaml` souboru, vyhledejte následující oddíl a změňte hodnotu `10` k `5`.</span><span class="sxs-lookup"><span data-stu-id="8035f-261">In the `newtopology.yaml` file, find the following section and change the value of `10` to `5`.</span></span> <span data-ttu-id="8035f-262">Tato úprava změní interval mezi generování dávky počty slov z 10 sekund. 5.</span><span class="sxs-lookup"><span data-stu-id="8035f-262">This modification changes the interval between emitting batches of word counts from 10 seconds to 5.</span></span>

    ```yaml
    - id: "counter-bolt"
    className: "com.microsoft.example.WordCount"
    constructorArgs:
    - 5
    parallelism: 1
    ```yaml

3. To run the topology, use the following command:

    ```bash
    mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"
    ```

    <span data-ttu-id="8035f-263">Nebo, pokud máte Storm na vašem vývojovém prostředí:</span><span class="sxs-lookup"><span data-stu-id="8035f-263">Or, if you have Storm on your development environment:</span></span>

    ```bash
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml
    ```

    <span data-ttu-id="8035f-264">Změna `/path/to/newtopology.yaml` na cestu k souboru newtopology.yaml jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="8035f-264">Change the `/path/to/newtopology.yaml` to the path to the newtopology.yaml file you created in the previous step.</span></span> <span data-ttu-id="8035f-265">Tento příkaz používá newtopology.yaml jako definice topologie.</span><span class="sxs-lookup"><span data-stu-id="8035f-265">This command uses the newtopology.yaml as the topology definition.</span></span> <span data-ttu-id="8035f-266">Vzhledem k tomu, že jsme nezahrnuli `compile` parametr, Maven používá verzi projektu vytvořené v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="8035f-266">Since we didn't include the `compile` parameter, Maven uses the version of the project built in previous steps.</span></span>

    <span data-ttu-id="8035f-267">Jakmile se spustí topologii, měli byste zaznamenat, že čas mezi emitovaného dávky se změnila, aby odrážela hodnotu v newtopology.yaml.</span><span class="sxs-lookup"><span data-stu-id="8035f-267">Once the topology starts, you should notice that the time between emitted batches has changed to reflect the value in newtopology.yaml.</span></span> <span data-ttu-id="8035f-268">Abyste viděli, že můžete změnit konfiguraci prostřednictvím soubor YAML bez nutnosti její kompilace topologii.</span><span class="sxs-lookup"><span data-stu-id="8035f-268">So you can see that you can change your configuration through a YAML file without having to recompile the topology.</span></span>

<span data-ttu-id="8035f-269">Další informace o těchto a dalších funkcí rozhraní tok najdete v tématu [tok (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="8035f-269">For more information on these and other features of the Flux framework, see [Flux (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span></span>

## <a name="trident"></a><span data-ttu-id="8035f-270">Trident</span><span class="sxs-lookup"><span data-stu-id="8035f-270">Trident</span></span>

<span data-ttu-id="8035f-271">Trident má vysokou úroveň abstrakce, která je poskytována Storm.</span><span class="sxs-lookup"><span data-stu-id="8035f-271">Trident is a high-level abstraction that is provided by Storm.</span></span> <span data-ttu-id="8035f-272">Podporuje stavová zpracování.</span><span class="sxs-lookup"><span data-stu-id="8035f-272">It supports stateful processing.</span></span> <span data-ttu-id="8035f-273">Primární výhodou Trident je, že ho může zaručit, že každou zprávu, která vstupuje do topologie je zpracovány pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="8035f-273">The primary advantage of Trident is that it can guarantee that every message that enters the topology is processed only once.</span></span> <span data-ttu-id="8035f-274">Bez použití Trident, topologie pouze zaručit alespoň jednou zpracování zpráv.</span><span class="sxs-lookup"><span data-stu-id="8035f-274">Without using Trident, your topology can only guarantee that messages are processed at least once.</span></span> <span data-ttu-id="8035f-275">Existují také další rozdíly, jako je například integrované komponenty, které lze použít místo vytvoření funkce bolts.</span><span class="sxs-lookup"><span data-stu-id="8035f-275">There are also other differences, such as built-in components that can be used instead of creating bolts.</span></span> <span data-ttu-id="8035f-276">Funkce bolts jsou ve skutečnosti nahrazovány obecného méně součástí, např. filtry, projekce a funkcí.</span><span class="sxs-lookup"><span data-stu-id="8035f-276">In fact, bolts are replaced by less-generic components, such as filters, projections, and functions.</span></span>

<span data-ttu-id="8035f-277">Trident aplikace lze vytvořit pomocí projekty Maven.</span><span class="sxs-lookup"><span data-stu-id="8035f-277">Trident applications can be created by using Maven projects.</span></span> <span data-ttu-id="8035f-278">Použijte stejný základní postup uvedenou výše v tomto článku – pouze kód se liší.</span><span class="sxs-lookup"><span data-stu-id="8035f-278">You use the same basic steps as presented earlier in this article—only the code is different.</span></span> <span data-ttu-id="8035f-279">Trident také (aktuálně) nelze zadat s tokem framework.</span><span class="sxs-lookup"><span data-stu-id="8035f-279">Trident also cannot (currently) be used with the Flux framework.</span></span>

<span data-ttu-id="8035f-280">Další informace o Trident naleznete v tématu [přehled API Trident](http://storm.apache.org/documentation/Trident-API-Overview.html).</span><span class="sxs-lookup"><span data-stu-id="8035f-280">For more information about Trident, see the [Trident API Overview](http://storm.apache.org/documentation/Trident-API-Overview.html).</span></span>

<span data-ttu-id="8035f-281">Příklad aplikace Trident naleznete v části [Twitter trendů témata s Apache Storm v HDInsight](hdinsight-storm-twitter-trending.md).</span><span class="sxs-lookup"><span data-stu-id="8035f-281">For an example of a Trident application, see [Twitter trending topics with Apache Storm on HDInsight](hdinsight-storm-twitter-trending.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8035f-282">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8035f-282">Next Steps</span></span>

<span data-ttu-id="8035f-283">Jste se naučili vytváření topologie Storm pomocí Java.</span><span class="sxs-lookup"><span data-stu-id="8035f-283">You have learned how to create a Storm topology by using Java.</span></span> <span data-ttu-id="8035f-284">Teď další postup:</span><span class="sxs-lookup"><span data-stu-id="8035f-284">Now learn how to:</span></span>

* [<span data-ttu-id="8035f-285">Nasazení a správa topologií Apache Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8035f-285">Deploy and manage Apache Storm topologies on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology.md)

* [<span data-ttu-id="8035f-286">Vývoj topologie C# pro Apache Storm v HDInsight pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8035f-286">Develop C# topologies for Apache Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)

<span data-ttu-id="8035f-287">Můžete najít další příklad topologií Storm navštivte stránky [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="8035f-287">You can find more example Storm topologies by visiting [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>

