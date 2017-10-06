---
title: "aaaQuery Hive prostřednictvím ovladač JDBC hello - Azure HDInsight | Microsoft Docs"
description: "Použijte ovladač JDBC hello z dotazy tooHadoop aplikace Java aplikace toosubmit Hive v HDInsight. Připojte prostřednictvím kódu programu a z klienta SQuirrel SQL hello."
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
ms.openlocfilehash: 93178da3b8d497faa4c788e91dba89c4e45d3fff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="query-hive-through-hello-jdbc-driver-in-hdinsight"></a><span data-ttu-id="faf0c-104">Dotaz Hive prostřednictvím ovladač JDBC hello v HDInsight</span><span class="sxs-lookup"><span data-stu-id="faf0c-104">Query Hive through hello JDBC driver in HDInsight</span></span>

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

<span data-ttu-id="faf0c-105">Zjistěte, jak ovladač JDBC hello toouse z toosubmit aplikace Java Hive dotazuje tooHadoop v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="faf0c-105">Learn how toouse hello JDBC driver from a Java application toosubmit Hive queries tooHadoop in Azure HDInsight.</span></span> <span data-ttu-id="faf0c-106">Hello informace v tomto dokumentu ukazuje, jak tooconnect prostřednictvím kódu programu a z hello SQuirrel klient SQL.</span><span class="sxs-lookup"><span data-stu-id="faf0c-106">hello information in this document demonstrates how tooconnect programmatically and from hello SQuirrel SQL client.</span></span>

<span data-ttu-id="faf0c-107">Další informace o hello rozhraní JDBC Hive naleznete v tématu [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).</span><span class="sxs-lookup"><span data-stu-id="faf0c-107">For more information on hello Hive JDBC Interface, see [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="faf0c-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="faf0c-108">Prerequisites</span></span>

* <span data-ttu-id="faf0c-109">Hadoop v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="faf0c-109">A Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="faf0c-110">Clustery se systémem Linux nebo systému Windows fungovat.</span><span class="sxs-lookup"><span data-stu-id="faf0c-110">Either Linux-based or Windows-based clusters work.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="faf0c-111">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="faf0c-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="faf0c-112">Další informace najdete v tématu [HDInsight 3.3 vyřazení](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="faf0c-112">For more information, see [HDInsight 3.3 retirement](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="faf0c-113">[SQuirreL SQL](http://squirrel-sql.sourceforge.net/).</span><span class="sxs-lookup"><span data-stu-id="faf0c-113">[SQuirreL SQL](http://squirrel-sql.sourceforge.net/).</span></span> <span data-ttu-id="faf0c-114">SQuirreL je JDBC klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="faf0c-114">SQuirreL is a JDBC client application.</span></span>

* <span data-ttu-id="faf0c-115">Hello [Java Developer Kit (JDK) verze 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="faf0c-115">hello [Java Developer Kit (JDK) version 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or higher.</span></span>

* <span data-ttu-id="faf0c-116">[Apache Maven](https://maven.apache.org).</span><span class="sxs-lookup"><span data-stu-id="faf0c-116">[Apache Maven](https://maven.apache.org).</span></span> <span data-ttu-id="faf0c-117">Maven je systém pro projekty Java, který se používá hello projektu spojené s tímto článkem sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="faf0c-117">Maven is a project build system for Java projects that is used by hello project associated with this article.</span></span>

## <a name="jdbc-connection-string"></a><span data-ttu-id="faf0c-118">Připojovací řetězec JDBC</span><span class="sxs-lookup"><span data-stu-id="faf0c-118">JDBC connection string</span></span>

<span data-ttu-id="faf0c-119">Cluster HDInsight tooan JDBC připojení v Azure jsou vytvářeny více než 443 a hello provoz je zabezpečené pomocí protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="faf0c-119">JDBC connections tooan HDInsight cluster on Azure are made over 443, and hello traffic is secured using SSL.</span></span> <span data-ttu-id="faf0c-120">Hello veřejné brány, kterou hello clustery nacházejí za přesměruje hello provoz toohello port, který HiveServer2 ve skutečnosti naslouchá na.</span><span class="sxs-lookup"><span data-stu-id="faf0c-120">hello public gateway that hello clusters sit behind redirects hello traffic toohello port that HiveServer2 is actually listening on.</span></span> <span data-ttu-id="faf0c-121">Hello následuje příklad řetězce připojení:</span><span class="sxs-lookup"><span data-stu-id="faf0c-121">hello following is an example connection string:</span></span>

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

<span data-ttu-id="faf0c-122">Nahraďte `CLUSTERNAME` s názvem hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="faf0c-122">Replace `CLUSTERNAME` with hello name of your HDInsight cluster.</span></span>

## <a name="authentication"></a><span data-ttu-id="faf0c-123">Authentication</span><span class="sxs-lookup"><span data-stu-id="faf0c-123">Authentication</span></span>

<span data-ttu-id="faf0c-124">Při navazování připojení hello, je nutné použít hello HDInsight clusteru správce jméno a heslo tooauthenticate toohello clusteru brány.</span><span class="sxs-lookup"><span data-stu-id="faf0c-124">When establishing hello connection, you must use hello HDInsight cluster admin name and password tooauthenticate toohello cluster gateway.</span></span> <span data-ttu-id="faf0c-125">Při připojení z klientů JDBC, jako například SQuirreL SQL, musíte zadat hello správce jméno a heslo v nastavení klienta.</span><span class="sxs-lookup"><span data-stu-id="faf0c-125">When connecting from JDBC clients such as SQuirreL SQL, you must enter hello admin name and password in client settings.</span></span>

<span data-ttu-id="faf0c-126">Z aplikace Java musíte použít hello jméno a heslo při navazování připojení.</span><span class="sxs-lookup"><span data-stu-id="faf0c-126">From a Java application, you must use hello name and password when establishing a connection.</span></span> <span data-ttu-id="faf0c-127">Například hello následující kód Java otevře nové připojení pomocí hello připojovací řetězec, jméno správce a heslo:</span><span class="sxs-lookup"><span data-stu-id="faf0c-127">For example, hello following Java code opens a new connection using hello connection string, admin name, and password:</span></span>

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a><span data-ttu-id="faf0c-128">Spojte se s klienta SQuirreL SQL</span><span class="sxs-lookup"><span data-stu-id="faf0c-128">Connect with SQuirreL SQL client</span></span>

<span data-ttu-id="faf0c-129">SQuirreL SQL je klient JDBC, který lze použít tooremotely spouštět dotazy Hive k vašemu clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="faf0c-129">SQuirreL SQL is a JDBC client that can be used tooremotely run Hive queries with your HDInsight cluster.</span></span> <span data-ttu-id="faf0c-130">Hello následující postup předpokládá, že jste již nainstalovali SQuirreL SQL.</span><span class="sxs-lookup"><span data-stu-id="faf0c-130">hello following steps assume that you have already installed SQuirreL SQL.</span></span>

1. <span data-ttu-id="faf0c-131">Zkopírujte hello Hive JDBC ovladače z clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="faf0c-131">Copy hello Hive JDBC drivers from your HDInsight cluster.</span></span>

    * <span data-ttu-id="faf0c-132">Pro **HDInsight se systémem Linux**, použijte hello následující kroky toodownload hello požadované jar soubory.</span><span class="sxs-lookup"><span data-stu-id="faf0c-132">For **Linux-based HDInsight**, use hello following steps toodownload hello required jar files.</span></span>

        1. <span data-ttu-id="faf0c-133">Vytvořte adresář, který obsahuje soubory hello.</span><span class="sxs-lookup"><span data-stu-id="faf0c-133">Create a directory that contains hello files.</span></span> <span data-ttu-id="faf0c-134">Například, `mkdir hivedriver`.</span><span class="sxs-lookup"><span data-stu-id="faf0c-134">For example, `mkdir hivedriver`.</span></span>

        2. <span data-ttu-id="faf0c-135">Z příkazového řádku použijte hello následující příkazy toocopy hello soubory z clusteru HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="faf0c-135">From a command line, use hello following commands toocopy hello files from hello HDInsight cluster:</span></span>

            ```bash
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/hive-jdbc*standalone.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-common.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-auth.jar .
            ```

            <span data-ttu-id="faf0c-136">Nahraďte `USERNAME` s názvem účtu uživatele SSH hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="faf0c-136">Replace `USERNAME` with hello SSH user account name for hello cluster.</span></span> <span data-ttu-id="faf0c-137">Nahraďte `CLUSTERNAME` s názvem clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="faf0c-137">Replace `CLUSTERNAME` with hello HDInsight cluster name.</span></span>

    * <span data-ttu-id="faf0c-138">Pro **HDInsight se systémem Windows**, použijte hello následující kroky souborů jar toodownload hello.</span><span class="sxs-lookup"><span data-stu-id="faf0c-138">For **Windows-based HDInsight**, use hello following steps toodownload hello jar files.</span></span>

        1. <span data-ttu-id="faf0c-139">Z hello portálu Azure, vyberte HDInsight cluster a pak vyberte hello **vzdálené plochy** ikonu.</span><span class="sxs-lookup"><span data-stu-id="faf0c-139">From hello Azure portal, select your HDInsight cluster, and then select hello **Remote Desktop** icon.</span></span>

            ![Ikona vzdálené plochy](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopicon.png)

        2. <span data-ttu-id="faf0c-141">V okně hello vzdálené plochy, použijte hello **Connect** tlačítko tooconnect toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="faf0c-141">On hello Remote Desktop blade, use hello **Connect** button tooconnect toohello cluster.</span></span> <span data-ttu-id="faf0c-142">Pokud není povoleno hello vzdálené plochy, použijte hello formuláře tooprovide uživatelské jméno a heslo a potom vyberte **povolit** tooenable vzdálené plochy pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="faf0c-142">If hello Remote Desktop is not enabled, use hello form tooprovide a user name and password, then select **Enable** tooenable Remote Desktop for hello cluster.</span></span>

            ![Okno Vzdálené plochy](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopblade.png)

            <span data-ttu-id="faf0c-144">Po výběru **Connect**, stáhne se soubor RDP.</span><span class="sxs-lookup"><span data-stu-id="faf0c-144">After selecting **Connect**, a .rdp file is downloaded.</span></span> <span data-ttu-id="faf0c-145">Pomocí klienta vzdálené plochy hello toolaunch tento soubor.</span><span class="sxs-lookup"><span data-stu-id="faf0c-145">Use this file toolaunch hello Remote Desktop client.</span></span> <span data-ttu-id="faf0c-146">Pokud budete vyzváni, použijte hello uživatelské jméno a heslo, které jste zadali pro přístup ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="faf0c-146">When prompted, use hello user name and password you entered for Remote Desktop access.</span></span>

        3. <span data-ttu-id="faf0c-147">Po připojení, zkopírujte následující soubory z místního počítače hello vzdálené plochy relace tooyour hello.</span><span class="sxs-lookup"><span data-stu-id="faf0c-147">Once connected, copy hello following files from hello Remote Desktop session tooyour local machine.</span></span> <span data-ttu-id="faf0c-148">Umístí je do místního adresáře s názvem `hivedriver`.</span><span class="sxs-lookup"><span data-stu-id="faf0c-148">Put them in a local directory named `hivedriver`.</span></span>

            * <span data-ttu-id="faf0c-149">C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-JDBC-0.14.0.2.2.9.1-7-Standalone.JAR</span><span class="sxs-lookup"><span data-stu-id="faf0c-149">C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-jdbc-0.14.0.2.2.9.1-7-standalone.jar</span></span>
            * <span data-ttu-id="faf0c-150">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-Common-2.6.0.2.2.9.1-7.JAR</span><span class="sxs-lookup"><span data-stu-id="faf0c-150">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-common-2.6.0.2.2.9.1-7.jar</span></span>
            * <span data-ttu-id="faf0c-151">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.JAR</span><span class="sxs-lookup"><span data-stu-id="faf0c-151">C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.jar</span></span>

            > [!NOTE]
            > <span data-ttu-id="faf0c-152">verze Hello čísla součástí hello cesty a názvy souborů mohou být různé pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="faf0c-152">hello version numbers included in hello paths and file names may be different for your cluster.</span></span>

        4. <span data-ttu-id="faf0c-153">Odpojení relace vzdálené plochy hello po dokončení kopírování souborů hello.</span><span class="sxs-lookup"><span data-stu-id="faf0c-153">Disconnect hello Remote Desktop session once you have finished copying hello files.</span></span>

2. <span data-ttu-id="faf0c-154">Spusťte aplikaci SQuirreL SQL hello.</span><span class="sxs-lookup"><span data-stu-id="faf0c-154">Start hello SQuirreL SQL application.</span></span> <span data-ttu-id="faf0c-155">Zleva hello okna hello vyberte **ovladače**.</span><span class="sxs-lookup"><span data-stu-id="faf0c-155">From hello left of hello window, select **Drivers**.</span></span>

    ![Karta ovladače na levé straně hello okna hello](./media/hdinsight-connect-hive-jdbc-driver/squirreldrivers.png)

3. <span data-ttu-id="faf0c-157">Z hello ikony na začátku hello hello **ovladače** dialogové okno, vyberte hello  **+**  toocreate ikonu ovladač.</span><span class="sxs-lookup"><span data-stu-id="faf0c-157">From hello icons at hello top of hello **Drivers** dialog, select hello **+** icon toocreate a driver.</span></span>

    ![Ikony ovladače](./media/hdinsight-connect-hive-jdbc-driver/driversicons.png)

4. <span data-ttu-id="faf0c-159">V dialogovém okně Přidat ovladač hello přidejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="faf0c-159">In hello Add Driver dialog, add hello following information:</span></span>

    * <span data-ttu-id="faf0c-160">**Název**: Hive</span><span class="sxs-lookup"><span data-stu-id="faf0c-160">**Name**: Hive</span></span>
    * <span data-ttu-id="faf0c-161">**Příklad adresy URL**:`jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`</span><span class="sxs-lookup"><span data-stu-id="faf0c-161">**Example URL**: `jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`</span></span>
    * <span data-ttu-id="faf0c-162">**Navíc cesty tříd**: předtím stáhli pomocí hello přidat tlačítko tooadd hello jar soubory</span><span class="sxs-lookup"><span data-stu-id="faf0c-162">**Extra Class Path**: Use hello Add button tooadd hello jar files downloaded earlier</span></span>
    * <span data-ttu-id="faf0c-163">**Název třídy**: org.apache.hive.jdbc.HiveDriver</span><span class="sxs-lookup"><span data-stu-id="faf0c-163">**Class Name**: org.apache.hive.jdbc.HiveDriver</span></span>

   ![Přidat dialogové okno ovladače](./media/hdinsight-connect-hive-jdbc-driver/adddriver.png)

   <span data-ttu-id="faf0c-165">Klikněte na tlačítko **OK** toosave tato nastavení.</span><span class="sxs-lookup"><span data-stu-id="faf0c-165">Click **OK** toosave these settings.</span></span>

5. <span data-ttu-id="faf0c-166">Na levé straně hello okna SQuirreL SQL hello vyberte **aliasy**.</span><span class="sxs-lookup"><span data-stu-id="faf0c-166">On hello left of hello SQuirreL SQL window, select **Aliases**.</span></span> <span data-ttu-id="faf0c-167">Pak klikněte na tlačítko hello  **+**  ikonu toocreate alias připojení.</span><span class="sxs-lookup"><span data-stu-id="faf0c-167">Then click hello **+** icon toocreate a connection alias.</span></span>

    ![Přidat nový alias](./media/hdinsight-connect-hive-jdbc-driver/aliases.png)

6. <span data-ttu-id="faf0c-169">Použití hello následující hodnoty pro hello **přidat Alias** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="faf0c-169">Use hello following values for hello **Add Alias** dialog.</span></span>

    * <span data-ttu-id="faf0c-170">**Název**: Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="faf0c-170">**Name**: Hive on HDInsight</span></span>

    * <span data-ttu-id="faf0c-171">**Ovladač**: použití hello rozevírací tooselect hello **Hive** ovladačů</span><span class="sxs-lookup"><span data-stu-id="faf0c-171">**Driver**: Use hello dropdown tooselect hello **Hive** driver</span></span>

    * <span data-ttu-id="faf0c-172">**Adresa URL**: jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2</span><span class="sxs-lookup"><span data-stu-id="faf0c-172">**URL**: jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2</span></span>

        <span data-ttu-id="faf0c-173">Nahraďte **CLUSTERNAME** s názvem hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="faf0c-173">Replace **CLUSTERNAME** with hello name of your HDInsight cluster.</span></span>

    * <span data-ttu-id="faf0c-174">**Uživatelské jméno**: název účtu přihlášení hello clusteru pro váš cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="faf0c-174">**User Name**: hello cluster login account name for your HDInsight cluster.</span></span> <span data-ttu-id="faf0c-175">Výchozí hodnota Hello je `admin`.</span><span class="sxs-lookup"><span data-stu-id="faf0c-175">hello default is `admin`.</span></span>

    * <span data-ttu-id="faf0c-176">**Heslo**: hello heslo pro účet přihlášení hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="faf0c-176">**Password**: hello password for hello cluster login account.</span></span>

 ![alias dialogové okno Přidání](./media/hdinsight-connect-hive-jdbc-driver/addalias.png)

    <span data-ttu-id="faf0c-178">Použití hello **Test** tooverify tlačítko, která hello připojení funguje.</span><span class="sxs-lookup"><span data-stu-id="faf0c-178">Use hello **Test** button tooverify that hello connection works.</span></span> <span data-ttu-id="faf0c-179">Když **připojit k: Hive v HDInsight** otevře se dialogové okno, vyberte **připojit** tooperform hello testu.</span><span class="sxs-lookup"><span data-stu-id="faf0c-179">When **Connect to: Hive on HDInsight** dialog appears, select **Connect** tooperform hello test.</span></span> <span data-ttu-id="faf0c-180">Pokud hello test úspěšný, zobrazí **úspěšné připojení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="faf0c-180">If hello test succeeds, you see a **Connection successful** dialog.</span></span>

    <span data-ttu-id="faf0c-181">připojení hello toosave alias, použijte hello **Ok** tlačítko dole hello hello **přidat Alias** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="faf0c-181">toosave hello connection alias, use hello **Ok** button at hello bottom of hello **Add Alias** dialog.</span></span>

7. <span data-ttu-id="faf0c-182">Z hello **připojit k** vyberte rozevírací seznam v horní části hello SQL SQuirreL **Hive v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="faf0c-182">From hello **Connect to** dropdown at hello top of SQuirreL SQL, select **Hive on HDInsight**.</span></span> <span data-ttu-id="faf0c-183">Po zobrazení výzvy vyberte **Connect**.</span><span class="sxs-lookup"><span data-stu-id="faf0c-183">When prompted, select **Connect**.</span></span>

    ![Dialogové okno připojení](./media/hdinsight-connect-hive-jdbc-driver/connect.png)

8. <span data-ttu-id="faf0c-185">Po připojení, zadejte následující dotaz do hello dialog dotazu SQL a pak vyberte hello hello **spustit** ikonu.</span><span class="sxs-lookup"><span data-stu-id="faf0c-185">Once connected, enter hello following query into hello SQL query dialog, and then select hello **Run** icon.</span></span> <span data-ttu-id="faf0c-186">Hello výsledky oblasti by měl zobrazit hello výsledky dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="faf0c-186">hello results area should show hello results of hello query.</span></span>

        select * from hivesampletable limit 10;

    ![Dialogové okno dotazu SQL, včetně výsledků](./media/hdinsight-connect-hive-jdbc-driver/sqlquery.png)

## <a name="connect-from-an-example-java-application"></a><span data-ttu-id="faf0c-188">Připojení z příklad aplikace Java</span><span class="sxs-lookup"><span data-stu-id="faf0c-188">Connect from an example Java application</span></span>

<span data-ttu-id="faf0c-189">Příklad použití Java klienta tooquery Hive v HDInsight je k dispozici na [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc).</span><span class="sxs-lookup"><span data-stu-id="faf0c-189">An example of using a Java client tooquery Hive on HDInsight is available at [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc).</span></span> <span data-ttu-id="faf0c-190">Postupujte podle pokynů hello v toobuild hello úložiště a spusťte ukázkové hello.</span><span class="sxs-lookup"><span data-stu-id="faf0c-190">Follow hello instructions in hello repository toobuild and run hello sample.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="faf0c-191">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="faf0c-191">Troubleshooting</span></span>

### <a name="unexpected-error-occurred-attempting-tooopen-an-sql-connection"></a><span data-ttu-id="faf0c-192">Došlo k neočekávané chybě při pokusu tooopen připojení k SQL</span><span class="sxs-lookup"><span data-stu-id="faf0c-192">Unexpected Error occurred attempting tooopen an SQL connection</span></span>

<span data-ttu-id="faf0c-193">**Příznaky**: při připojování tooan clusteru HDInsight, který je verze 3.3 nebo 3.4, zobrazí chybu, ke které došlo k neočekávané chybě.</span><span class="sxs-lookup"><span data-stu-id="faf0c-193">**Symptoms**: When connecting tooan HDInsight cluster that is version 3.3 or 3.4, you may receive an error that an unexpected error occurred.</span></span> <span data-ttu-id="faf0c-194">Hello trasování zásobníku pro tuto chybu začíná textem hello následující řádky:</span><span class="sxs-lookup"><span data-stu-id="faf0c-194">hello stack trace for this error begins with hello following lines:</span></span>

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

<span data-ttu-id="faf0c-195">**Příčina**: Tato chyba je způsobená neshodou hello verzi souboru commons codec.jar hello používané SQuirreL a hello jeden požadované součásti Hive JDBC hello.</span><span class="sxs-lookup"><span data-stu-id="faf0c-195">**Cause**: This error is caused by a mismatch in hello version of hello commons-codec.jar file used by SQuirreL and hello one required by hello Hive JDBC components.</span></span>

<span data-ttu-id="faf0c-196">**Řešení**: toofix tato chyba, hello použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="faf0c-196">**Resolution**: toofix this error, use hello following steps:</span></span>

1. <span data-ttu-id="faf0c-197">Stáhněte si soubor jar commons kodeků hello z clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="faf0c-197">Download hello commons-codec jar file from your HDInsight cluster.</span></span>

        scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-codec*.jar ./commons-codec.jar

2. <span data-ttu-id="faf0c-198">Ukončete SQuirreL a potom přejděte toohello directory nainstalovanou SQuirreL ve vašem systému.</span><span class="sxs-lookup"><span data-stu-id="faf0c-198">Exit SQuirreL, and then go toohello directory where SQuirreL is installed on your system.</span></span> <span data-ttu-id="faf0c-199">V adresáři SquirreL hello, v části hello `lib` adresář, nahraďte hello existující commons-codec.jar s hello jeden stažený z clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="faf0c-199">In hello SquirreL directory, under hello `lib` directory, replace hello existing commons-codec.jar with hello one downloaded from hello HDInsight cluster.</span></span>

3. <span data-ttu-id="faf0c-200">Restartujte SQuirreL.</span><span class="sxs-lookup"><span data-stu-id="faf0c-200">Restart SQuirreL.</span></span> <span data-ttu-id="faf0c-201">Chyba Hello provedeno už při připojování tooHive v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="faf0c-201">hello error should no longer occur when connecting tooHive on HDInsight.</span></span>

## <a name="next-steps"></a><span data-ttu-id="faf0c-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="faf0c-202">Next steps</span></span>

<span data-ttu-id="faf0c-203">Teď, když jste se naučili, jak toouse toowork JDBC s Hive, hello použijte následující odkazy tooexplore jiné způsoby toowork s Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="faf0c-203">Now that you have learned how toouse JDBC toowork with Hive, use hello following links tooexplore other ways toowork with Azure HDInsight.</span></span>

* [<span data-ttu-id="faf0c-204">Nahrání dat tooHDInsight</span><span class="sxs-lookup"><span data-stu-id="faf0c-204">Upload data tooHDInsight</span></span>](hdinsight-upload-data.md)
* [<span data-ttu-id="faf0c-205">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="faf0c-205">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="faf0c-206">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="faf0c-206">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="faf0c-207">Použití úloh MapReduce se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="faf0c-207">Use MapReduce jobs with HDInsight</span></span>](hdinsight-use-mapreduce.md)
