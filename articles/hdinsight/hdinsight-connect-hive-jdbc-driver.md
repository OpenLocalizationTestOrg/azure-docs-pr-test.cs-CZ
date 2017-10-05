---
title: "Dotaz Hive prostřednictvím ovladač JDBC - Azure HDInsight | Microsoft Docs"
description: "Ovladač JDBC z aplikace Java použijte k odesílání dotazů Hive k systému Hadoop v HDInsight. Připojte prostřednictvím kódu programu a z klienta SQuirrel SQL."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 928f8d2a-684d-48cb-894c-11c59a5599ae
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: f2de58c2763b2ddd0926336164738929c477c4ae
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="query-hive-through-the-jdbc-driver-in-hdinsight"></a><span data-ttu-id="66c84-104">Dotaz Hive prostřednictvím ovladač JDBC v HDInsight</span><span class="sxs-lookup"><span data-stu-id="66c84-104">Query Hive through the JDBC driver in HDInsight</span></span>

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

<span data-ttu-id="66c84-105">Naučte se používat k odesílání dotazů Hive k systému Hadoop v prostředí Azure HDInsight ovladač JDBC z aplikace Java.</span><span class="sxs-lookup"><span data-stu-id="66c84-105">Learn how to use the JDBC driver from a Java application to submit Hive queries to Hadoop in Azure HDInsight.</span></span> <span data-ttu-id="66c84-106">Informace v tomto dokumentu ukazuje, jak se připojit prostřednictvím kódu programu a z klienta SQuirrel SQL.</span><span class="sxs-lookup"><span data-stu-id="66c84-106">The information in this document demonstrates how to connect programmatically and from the SQuirrel SQL client.</span></span>

<span data-ttu-id="66c84-107">Další informace o rozhraní JDBC Hive naleznete v tématu [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).</span><span class="sxs-lookup"><span data-stu-id="66c84-107">For more information on the Hive JDBC Interface, see [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66c84-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="66c84-108">Prerequisites</span></span>

* <span data-ttu-id="66c84-109">Hadoop v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="66c84-109">A Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="66c84-110">Clustery se systémem Linux nebo systému Windows fungovat.</span><span class="sxs-lookup"><span data-stu-id="66c84-110">Either Linux-based or Windows-based clusters work.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="66c84-111">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="66c84-111">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="66c84-112">Další informace najdete v tématu [HDInsight 3.3 vyřazení](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="66c84-112">For more information, see [HDInsight 3.3 retirement](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="66c84-113">[SQuirreL SQL](http://squirrel-sql.sourceforge.net/).</span><span class="sxs-lookup"><span data-stu-id="66c84-113">[SQuirreL SQL](http://squirrel-sql.sourceforge.net/).</span></span> <span data-ttu-id="66c84-114">SQuirreL je JDBC klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="66c84-114">SQuirreL is a JDBC client application.</span></span>

* <span data-ttu-id="66c84-115">[Java Developer Kit (JDK) verze 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="66c84-115">The [Java Developer Kit (JDK) version 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or higher.</span></span>

* <span data-ttu-id="66c84-116">[Apache Maven](https://maven.apache.org).</span><span class="sxs-lookup"><span data-stu-id="66c84-116">[Apache Maven](https://maven.apache.org).</span></span> <span data-ttu-id="66c84-117">Maven je systém pro projekty Java, který se používá v projektu spojené s tímto článkem sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="66c84-117">Maven is a project build system for Java projects that is used by the project associated with this article.</span></span>

## <a name="jdbc-connection-string"></a><span data-ttu-id="66c84-118">Připojovací řetězec JDBC</span><span class="sxs-lookup"><span data-stu-id="66c84-118">JDBC connection string</span></span>

<span data-ttu-id="66c84-119">JDBC připojení ke clusteru služby HDInsight v Azure jsou vytvářeny více než 443, a provoz se zabezpečené pomocí protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="66c84-119">JDBC connections to an HDInsight cluster on Azure are made over 443, and the traffic is secured using SSL.</span></span> <span data-ttu-id="66c84-120">Veřejné brány, kterou clustery nacházejí za přesměruje na port, který HiveServer2 ve skutečnosti naslouchá na provoz.</span><span class="sxs-lookup"><span data-stu-id="66c84-120">The public gateway that the clusters sit behind redirects the traffic to the port that HiveServer2 is actually listening on.</span></span> <span data-ttu-id="66c84-121">Následuje příklad řetězce připojení:</span><span class="sxs-lookup"><span data-stu-id="66c84-121">The following is an example connection string:</span></span>

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

<span data-ttu-id="66c84-122">Parametr `CLUSTERNAME` nahraďte názvem vašeho clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="66c84-122">Replace `CLUSTERNAME` with the name of your HDInsight cluster.</span></span>

## <a name="authentication"></a><span data-ttu-id="66c84-123">Authentication</span><span class="sxs-lookup"><span data-stu-id="66c84-123">Authentication</span></span>

<span data-ttu-id="66c84-124">Při vytváření připojení, musíte použít název správce clusteru HDInsight a heslo k ověření clusteru brány.</span><span class="sxs-lookup"><span data-stu-id="66c84-124">When establishing the connection, you must use the HDInsight cluster admin name and password to authenticate to the cluster gateway.</span></span> <span data-ttu-id="66c84-125">Při připojení z klientů JDBC, jako je SQuirreL SQL, musíte zadat jméno správce a heslo v nastavení klienta.</span><span class="sxs-lookup"><span data-stu-id="66c84-125">When connecting from JDBC clients such as SQuirreL SQL, you must enter the admin name and password in client settings.</span></span>

<span data-ttu-id="66c84-126">Z aplikace Java musíte použít přihlašovací jméno a heslo při navazování připojení.</span><span class="sxs-lookup"><span data-stu-id="66c84-126">From a Java application, you must use the name and password when establishing a connection.</span></span> <span data-ttu-id="66c84-127">Například následující kód Java otevře nové připojení pomocí připojovacího řetězce, jméno správce a heslo:</span><span class="sxs-lookup"><span data-stu-id="66c84-127">For example, the following Java code opens a new connection using the connection string, admin name, and password:</span></span>

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a><span data-ttu-id="66c84-128">Spojte se s klienta SQuirreL SQL</span><span class="sxs-lookup"><span data-stu-id="66c84-128">Connect with SQuirreL SQL client</span></span>

<span data-ttu-id="66c84-129">SQuirreL SQL je klient JDBC, který umožňuje vzdáleně spouštět dotazy Hive k vašemu clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="66c84-129">SQuirreL SQL is a JDBC client that can be used to remotely run Hive queries with your HDInsight cluster.</span></span> <span data-ttu-id="66c84-130">Následující postup předpokládá, že jste již nainstalovali SQuirreL SQL.</span><span class="sxs-lookup"><span data-stu-id="66c84-130">The following steps assume that you have already installed SQuirreL SQL.</span></span>

1. <span data-ttu-id="66c84-131">Zkopírujte z clusteru HDInsight Hive JDBC ovladače.</span><span class="sxs-lookup"><span data-stu-id="66c84-131">Copy the Hive JDBC drivers from your HDInsight cluster.</span></span>

    * <span data-ttu-id="66c84-132">Pro **HDInsight se systémem Linux**, použijte následující postup ke stažení souborů požadovaných jar.</span><span class="sxs-lookup"><span data-stu-id="66c84-132">For **Linux-based HDInsight**, use the following steps to download the required jar files.</span></span>

        1. <span data-ttu-id="66c84-133">Vytvořte adresář, který obsahuje soubory.</span><span class="sxs-lookup"><span data-stu-id="66c84-133">Create a directory that contains the files.</span></span> <span data-ttu-id="66c84-134">Například, `mkdir hivedriver`.</span><span class="sxs-lookup"><span data-stu-id="66c84-134">For example, `mkdir hivedriver`.</span></span>

        2. <span data-ttu-id="66c84-135">Z příkazového řádku použijte následující příkazy pro kopírování souborů z clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="66c84-135">From a command line, use the following commands to copy the files from the HDInsight cluster:</span></span>

            ```bash
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/hive-jdbc*standalone.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-common.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-auth.jar .
            ```

            <span data-ttu-id="66c84-136">Nahraďte `USERNAME` s názvem účtu uživatele SSH pro cluster.</span><span class="sxs-lookup"><span data-stu-id="66c84-136">Replace `USERNAME` with the SSH user account name for the cluster.</span></span> <span data-ttu-id="66c84-137">Nahraďte `CLUSTERNAME` s názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="66c84-137">Replace `CLUSTERNAME` with the HDInsight cluster name.</span></span>

    * <span data-ttu-id="66c84-138">Pro **HDInsight se systémem Windows**, použijte následující postup ke stažení souborů jar.</span><span class="sxs-lookup"><span data-stu-id="66c84-138">For **Windows-based HDInsight**, use the following steps to download the jar files.</span></span>

        1. <span data-ttu-id="66c84-139">Z portálu Azure vyberte HDInsight cluster a pak vyberte **vzdálené plochy** ikonu.</span><span class="sxs-lookup"><span data-stu-id="66c84-139">From the Azure portal, select your HDInsight cluster, and then select the **Remote Desktop** icon.</span></span>

            ![Ikona vzdálené plochy](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopicon.png)

        2. <span data-ttu-id="66c84-141">V okně připojení ke vzdálené ploše pomocí **Connect** tlačítko Připojit ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="66c84-141">On the Remote Desktop blade, use the **Connect** button to connect to the cluster.</span></span> <span data-ttu-id="66c84-142">Pokud Vzdálená plocha není povolená, pomocí formuláře zadejte uživatelské jméno a heslo a pak vyberte **povolit** k povolení služby Vzdálená plocha pro cluster.</span><span class="sxs-lookup"><span data-stu-id="66c84-142">If the Remote Desktop is not enabled, use the form to provide a user name and password, then select **Enable** to enable Remote Desktop for the cluster.</span></span>

            ![Okno Vzdálené plochy](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopblade.png)

            <span data-ttu-id="66c84-144">Po výběru **Connect**, stáhne se soubor RDP.</span><span class="sxs-lookup"><span data-stu-id="66c84-144">After selecting **Connect**, a .rdp file is downloaded.</span></span> <span data-ttu-id="66c84-145">Tento soubor lze použijte ke spuštění klienta vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="66c84-145">Use this file to launch the Remote Desktop client.</span></span> <span data-ttu-id="66c84-146">Pokud budete vyzváni, použijte uživatelské jméno a heslo, které jste zadali pro přístup ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="66c84-146">When prompted, use the user name and password you entered for Remote Desktop access.</span></span>

        3. <span data-ttu-id="66c84-147">Po připojení, zkopírujte následující soubory z relace vzdálené plochy do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="66c84-147">Once connected, copy the following files from the Remote Desktop session to your local machine.</span></span> <span data-ttu-id="66c84-148">Umístí je do místního adresáře s názvem `hivedriver`.</span><span class="sxs-lookup"><span data-stu-id="66c84-148">Put them in a local directory named `hivedriver`.</span></span>

            * <span data-ttu-id="66c84-149">C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-JDBC-0.14.0.2.2.9.1-7-Standalone.JAR</span><span class="sxs-lookup"><span data-stu-id="66c84-149">C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-jdbc-0.14.0.2.2.9.1-7-standalone.jar</span></span>
            * <span data-ttu-id="66c84-150">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-Common-2.6.0.2.2.9.1-7.JAR</span><span class="sxs-lookup"><span data-stu-id="66c84-150">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-common-2.6.0.2.2.9.1-7.jar</span></span>
            * <span data-ttu-id="66c84-151">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.JAR</span><span class="sxs-lookup"><span data-stu-id="66c84-151">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.jar</span></span>

            > [!NOTE]
            > <span data-ttu-id="66c84-152">Čísla verzí, které jsou součástí cesty a názvy souborů se může lišit pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="66c84-152">The version numbers included in the paths and file names may be different for your cluster.</span></span>

        4. <span data-ttu-id="66c84-153">Odpojení relace vzdálené plochy po dokončení kopírování souborů.</span><span class="sxs-lookup"><span data-stu-id="66c84-153">Disconnect the Remote Desktop session once you have finished copying the files.</span></span>

2. <span data-ttu-id="66c84-154">Spusťte aplikaci SQuirreL SQL.</span><span class="sxs-lookup"><span data-stu-id="66c84-154">Start the SQuirreL SQL application.</span></span> <span data-ttu-id="66c84-155">V levé části okna vyberte **ovladače**.</span><span class="sxs-lookup"><span data-stu-id="66c84-155">From the left of the window, select **Drivers**.</span></span>

    ![Karta ovladače na levé straně okna](./media/hdinsight-connect-hive-jdbc-driver/squirreldrivers.png)

3. <span data-ttu-id="66c84-157">Z ikony v horní části **ovladače** dialogovém okně, vyberte  **+**  vytvořte ovladač.</span><span class="sxs-lookup"><span data-stu-id="66c84-157">From the icons at the top of the **Drivers** dialog, select the **+** icon to create a driver.</span></span>

    ![Ikony ovladače](./media/hdinsight-connect-hive-jdbc-driver/driversicons.png)

4. <span data-ttu-id="66c84-159">V dialogovém okně Přidat ovladač přidejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="66c84-159">In the Add Driver dialog, add the following information:</span></span>

    * <span data-ttu-id="66c84-160">**Název**: Hive</span><span class="sxs-lookup"><span data-stu-id="66c84-160">**Name**: Hive</span></span>
    * <span data-ttu-id="66c84-161">**Příklad adresy URL**:`jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`</span><span class="sxs-lookup"><span data-stu-id="66c84-161">**Example URL**: `jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`</span></span>
    * <span data-ttu-id="66c84-162">**Navíc cesty tříd**: použijte tlačítko Přidat na přidání souborů jar předtím stáhli</span><span class="sxs-lookup"><span data-stu-id="66c84-162">**Extra Class Path**: Use the Add button to add the jar files downloaded earlier</span></span>
    * <span data-ttu-id="66c84-163">**Název třídy**: org.apache.hive.jdbc.HiveDriver</span><span class="sxs-lookup"><span data-stu-id="66c84-163">**Class Name**: org.apache.hive.jdbc.HiveDriver</span></span>

   ![Přidat dialogové okno ovladače](./media/hdinsight-connect-hive-jdbc-driver/adddriver.png)

   <span data-ttu-id="66c84-165">Klikněte na tlačítko **OK** uložit tato nastavení.</span><span class="sxs-lookup"><span data-stu-id="66c84-165">Click **OK** to save these settings.</span></span>

5. <span data-ttu-id="66c84-166">Na levé straně okna SQuirreL SQL, vyberte **aliasy**.</span><span class="sxs-lookup"><span data-stu-id="66c84-166">On the left of the SQuirreL SQL window, select **Aliases**.</span></span> <span data-ttu-id="66c84-167">Klikněte  **+**  ikonu vytvořit alias připojení.</span><span class="sxs-lookup"><span data-stu-id="66c84-167">Then click the **+** icon to create a connection alias.</span></span>

    ![Přidat nový alias](./media/hdinsight-connect-hive-jdbc-driver/aliases.png)

6. <span data-ttu-id="66c84-169">Použijte následující hodnoty pro **přidat Alias** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="66c84-169">Use the following values for the **Add Alias** dialog.</span></span>

    * <span data-ttu-id="66c84-170">**Název**: Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="66c84-170">**Name**: Hive on HDInsight</span></span>

    * <span data-ttu-id="66c84-171">**Ovladač**: pomocí rozevíracího seznamu vyberte **Hive** ovladačů</span><span class="sxs-lookup"><span data-stu-id="66c84-171">**Driver**: Use the dropdown to select the **Hive** driver</span></span>

    * <span data-ttu-id="66c84-172">**Adresa URL**: jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2</span><span class="sxs-lookup"><span data-stu-id="66c84-172">**URL**: jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2</span></span>

        <span data-ttu-id="66c84-173">Nahraďte **CLUSTERNAME** názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="66c84-173">Replace **CLUSTERNAME** with the name of your HDInsight cluster.</span></span>

    * <span data-ttu-id="66c84-174">**Uživatelské jméno**: název clusteru přihlašovací účet pro váš cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="66c84-174">**User Name**: The cluster login account name for your HDInsight cluster.</span></span> <span data-ttu-id="66c84-175">Výchozí hodnota je `admin`.</span><span class="sxs-lookup"><span data-stu-id="66c84-175">The default is `admin`.</span></span>

    * <span data-ttu-id="66c84-176">**Heslo**: heslo pro účet přihlášení clusteru.</span><span class="sxs-lookup"><span data-stu-id="66c84-176">**Password**: The password for the cluster login account.</span></span>

 ![alias dialogové okno Přidání](./media/hdinsight-connect-hive-jdbc-driver/addalias.png)

    <span data-ttu-id="66c84-178">Použití **Test** tlačítko ověřte, že připojení funguje.</span><span class="sxs-lookup"><span data-stu-id="66c84-178">Use the **Test** button to verify that the connection works.</span></span> <span data-ttu-id="66c84-179">Když **připojit k: Hive v HDInsight** otevře se dialogové okno, vyberte **Connect** k provedení testu.</span><span class="sxs-lookup"><span data-stu-id="66c84-179">When **Connect to: Hive on HDInsight** dialog appears, select **Connect** to perform the test.</span></span> <span data-ttu-id="66c84-180">Pokud je test úspěšný, zobrazí **úspěšné připojení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="66c84-180">If the test succeeds, you see a **Connection successful** dialog.</span></span>

    <span data-ttu-id="66c84-181">Chcete-li uložit alias připojení, použijte **Ok** tlačítko v dolní části **přidat Alias** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="66c84-181">To save the connection alias, use the **Ok** button at the bottom of the **Add Alias** dialog.</span></span>

7. <span data-ttu-id="66c84-182">Z **připojit k** rozevírací seznam v horní části SQuirreL SQL, vyberte **Hive v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="66c84-182">From the **Connect to** dropdown at the top of SQuirreL SQL, select **Hive on HDInsight**.</span></span> <span data-ttu-id="66c84-183">Po zobrazení výzvy vyberte **Connect**.</span><span class="sxs-lookup"><span data-stu-id="66c84-183">When prompted, select **Connect**.</span></span>

    ![Dialogové okno připojení](./media/hdinsight-connect-hive-jdbc-driver/connect.png)

8. <span data-ttu-id="66c84-185">Po připojení, zadejte následující dotaz do dialogu dotaz SQL a potom vyberte **spustit** ikonu.</span><span class="sxs-lookup"><span data-stu-id="66c84-185">Once connected, enter the following query into the SQL query dialog, and then select the **Run** icon.</span></span> <span data-ttu-id="66c84-186">Oblasti výsledky by měl zobrazit výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="66c84-186">The results area should show the results of the query.</span></span>

        select * from hivesampletable limit 10;

    ![Dialogové okno dotazu SQL, včetně výsledků](./media/hdinsight-connect-hive-jdbc-driver/sqlquery.png)

## <a name="connect-from-an-example-java-application"></a><span data-ttu-id="66c84-188">Připojení z příklad aplikace Java</span><span class="sxs-lookup"><span data-stu-id="66c84-188">Connect from an example Java application</span></span>

<span data-ttu-id="66c84-189">Příklad použití Java klienta tak, aby dotaz Hive v HDInsight je k dispozici na [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc).</span><span class="sxs-lookup"><span data-stu-id="66c84-189">An example of using a Java client to query Hive on HDInsight is available at [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc).</span></span> <span data-ttu-id="66c84-190">Postupujte podle pokynů v úložišti sestavení a spuštění ukázky.</span><span class="sxs-lookup"><span data-stu-id="66c84-190">Follow the instructions in the repository to build and run the sample.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="66c84-191">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="66c84-191">Troubleshooting</span></span>

### <a name="unexpected-error-occurred-attempting-to-open-an-sql-connection"></a><span data-ttu-id="66c84-192">Došlo k neočekávané chybě při pokusu o otevření připojení k SQL došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="66c84-192">Unexpected Error occurred attempting to open an SQL connection</span></span>

<span data-ttu-id="66c84-193">**Příznaky**: při připojování ke clusteru služby HDInsight, který je verze 3.3 nebo 3.4, obdržíte chybu, ke které došlo k neočekávané chybě.</span><span class="sxs-lookup"><span data-stu-id="66c84-193">**Symptoms**: When connecting to an HDInsight cluster that is version 3.3 or 3.4, you may receive an error that an unexpected error occurred.</span></span> <span data-ttu-id="66c84-194">Trasování zásobníku pro tuto chybu začíná následující řádky:</span><span class="sxs-lookup"><span data-stu-id="66c84-194">The stack trace for this error begins with the following lines:</span></span>

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

<span data-ttu-id="66c84-195">**Příčina**: Tato chyba je způsobená neshoda ve verzi souboru commons codec.jar používané SQuirreL a jeden požadované součásti Hive JDBC.</span><span class="sxs-lookup"><span data-stu-id="66c84-195">**Cause**: This error is caused by a mismatch in the version of the commons-codec.jar file used by SQuirreL and the one required by the Hive JDBC components.</span></span>

<span data-ttu-id="66c84-196">**Řešení**: odstranění této chyby, použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="66c84-196">**Resolution**: To fix this error, use the following steps:</span></span>

1. <span data-ttu-id="66c84-197">Stáhněte si soubor jar commons kodeků z clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="66c84-197">Download the commons-codec jar file from your HDInsight cluster.</span></span>

        scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-codec*.jar ./commons-codec.jar

2. <span data-ttu-id="66c84-198">Ukončete SQuirreL a pak přejděte do adresáře, kde je nainstalován SQuirreL ve vašem systému.</span><span class="sxs-lookup"><span data-stu-id="66c84-198">Exit SQuirreL, and then go to the directory where SQuirreL is installed on your system.</span></span> <span data-ttu-id="66c84-199">V adresáři SquirreL pod `lib` directory nahradit existující codec.jar commons s ten stažený z clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="66c84-199">In the SquirreL directory, under the `lib` directory, replace the existing commons-codec.jar with the one downloaded from the HDInsight cluster.</span></span>

3. <span data-ttu-id="66c84-200">Restartujte SQuirreL.</span><span class="sxs-lookup"><span data-stu-id="66c84-200">Restart SQuirreL.</span></span> <span data-ttu-id="66c84-201">Chyba provedeno už při připojování k Hive v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="66c84-201">The error should no longer occur when connecting to Hive on HDInsight.</span></span>

## <a name="next-steps"></a><span data-ttu-id="66c84-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="66c84-202">Next steps</span></span>

<span data-ttu-id="66c84-203">Teď, když jste se naučili použití JDBC pro práci s Hive, pomocí následujících odkazů a prozkoumejte další způsoby, jak pracovat s Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="66c84-203">Now that you have learned how to use JDBC to work with Hive, use the following links to explore other ways to work with Azure HDInsight.</span></span>

* [<span data-ttu-id="66c84-204">Nahrání dat do HDInsight</span><span class="sxs-lookup"><span data-stu-id="66c84-204">Upload data to HDInsight</span></span>](hdinsight-upload-data.md)
* [<span data-ttu-id="66c84-205">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="66c84-205">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="66c84-206">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="66c84-206">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="66c84-207">Použití úloh MapReduce se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="66c84-207">Use MapReduce jobs with HDInsight</span></span>](hdinsight-use-mapreduce.md)
