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
# <a name="query-hive-through-hello-jdbc-driver-in-hdinsight"></a>Dotaz Hive prostřednictvím ovladač JDBC hello v HDInsight

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Zjistěte, jak ovladač JDBC hello toouse z toosubmit aplikace Java Hive dotazuje tooHadoop v Azure HDInsight. Hello informace v tomto dokumentu ukazuje, jak tooconnect prostřednictvím kódu programu a z hello SQuirrel klient SQL.

Další informace o hello rozhraní JDBC Hive naleznete v tématu [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).

## <a name="prerequisites"></a>Požadavky

* Hadoop v clusteru HDInsight. Clustery se systémem Linux nebo systému Windows fungovat.

  > [!IMPORTANT]
  > Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [HDInsight 3.3 vyřazení](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [SQuirreL SQL](http://squirrel-sql.sourceforge.net/). SQuirreL je JDBC klientské aplikace.

* Hello [Java Developer Kit (JDK) verze 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) nebo vyšší.

* [Apache Maven](https://maven.apache.org). Maven je systém pro projekty Java, který se používá hello projektu spojené s tímto článkem sestavení projektu.

## <a name="jdbc-connection-string"></a>Připojovací řetězec JDBC

Cluster HDInsight tooan JDBC připojení v Azure jsou vytvářeny více než 443 a hello provoz je zabezpečené pomocí protokolu SSL. Hello veřejné brány, kterou hello clustery nacházejí za přesměruje hello provoz toohello port, který HiveServer2 ve skutečnosti naslouchá na. Hello následuje příklad řetězce připojení:

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

Nahraďte `CLUSTERNAME` s názvem hello clusteru HDInsight.

## <a name="authentication"></a>Authentication

Při navazování připojení hello, je nutné použít hello HDInsight clusteru správce jméno a heslo tooauthenticate toohello clusteru brány. Při připojení z klientů JDBC, jako například SQuirreL SQL, musíte zadat hello správce jméno a heslo v nastavení klienta.

Z aplikace Java musíte použít hello jméno a heslo při navazování připojení. Například hello následující kód Java otevře nové připojení pomocí hello připojovací řetězec, jméno správce a heslo:

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a>Spojte se s klienta SQuirreL SQL

SQuirreL SQL je klient JDBC, který lze použít tooremotely spouštět dotazy Hive k vašemu clusteru HDInsight. Hello následující postup předpokládá, že jste již nainstalovali SQuirreL SQL.

1. Zkopírujte hello Hive JDBC ovladače z clusteru HDInsight.

    * Pro **HDInsight se systémem Linux**, použijte hello následující kroky toodownload hello požadované jar soubory.

        1. Vytvořte adresář, který obsahuje soubory hello. Například, `mkdir hivedriver`.

        2. Z příkazového řádku použijte hello následující příkazy toocopy hello soubory z clusteru HDInsight hello:

            ```bash
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/hive-jdbc*standalone.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-common.jar .
            scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-auth.jar .
            ```

            Nahraďte `USERNAME` s názvem účtu uživatele SSH hello hello clusteru. Nahraďte `CLUSTERNAME` s názvem clusteru HDInsight hello.

    * Pro **HDInsight se systémem Windows**, použijte hello následující kroky souborů jar toodownload hello.

        1. Z hello portálu Azure, vyberte HDInsight cluster a pak vyberte hello **vzdálené plochy** ikonu.

            ![Ikona vzdálené plochy](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopicon.png)

        2. V okně hello vzdálené plochy, použijte hello **Connect** tlačítko tooconnect toohello clusteru. Pokud není povoleno hello vzdálené plochy, použijte hello formuláře tooprovide uživatelské jméno a heslo a potom vyberte **povolit** tooenable vzdálené plochy pro hello cluster.

            ![Okno Vzdálené plochy](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopblade.png)

            Po výběru **Connect**, stáhne se soubor RDP. Pomocí klienta vzdálené plochy hello toolaunch tento soubor. Pokud budete vyzváni, použijte hello uživatelské jméno a heslo, které jste zadali pro přístup ke vzdálené ploše.

        3. Po připojení, zkopírujte následující soubory z místního počítače hello vzdálené plochy relace tooyour hello. Umístí je do místního adresáře s názvem `hivedriver`.

            * C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-JDBC-0.14.0.2.2.9.1-7-Standalone.JAR
            * C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-Common-2.6.0.2.2.9.1-7.JAR
            * C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.JAR

            > [!NOTE]
            > verze Hello čísla součástí hello cesty a názvy souborů mohou být různé pro váš cluster.

        4. Odpojení relace vzdálené plochy hello po dokončení kopírování souborů hello.

2. Spusťte aplikaci SQuirreL SQL hello. Zleva hello okna hello vyberte **ovladače**.

    ![Karta ovladače na levé straně hello okna hello](./media/hdinsight-connect-hive-jdbc-driver/squirreldrivers.png)

3. Z hello ikony na začátku hello hello **ovladače** dialogové okno, vyberte hello  **+**  toocreate ikonu ovladač.

    ![Ikony ovladače](./media/hdinsight-connect-hive-jdbc-driver/driversicons.png)

4. V dialogovém okně Přidat ovladač hello přidejte hello následující informace:

    * **Název**: Hive
    * **Příklad adresy URL**:`jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`
    * **Navíc cesty tříd**: předtím stáhli pomocí hello přidat tlačítko tooadd hello jar soubory
    * **Název třídy**: org.apache.hive.jdbc.HiveDriver

   ![Přidat dialogové okno ovladače](./media/hdinsight-connect-hive-jdbc-driver/adddriver.png)

   Klikněte na tlačítko **OK** toosave tato nastavení.

5. Na levé straně hello okna SQuirreL SQL hello vyberte **aliasy**. Pak klikněte na tlačítko hello  **+**  ikonu toocreate alias připojení.

    ![Přidat nový alias](./media/hdinsight-connect-hive-jdbc-driver/aliases.png)

6. Použití hello následující hodnoty pro hello **přidat Alias** dialogové okno.

    * **Název**: Hive v HDInsight

    * **Ovladač**: použití hello rozevírací tooselect hello **Hive** ovladačů

    * **Adresa URL**: jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

        Nahraďte **CLUSTERNAME** s názvem hello clusteru HDInsight.

    * **Uživatelské jméno**: název účtu přihlášení hello clusteru pro váš cluster HDInsight. Výchozí hodnota Hello je `admin`.

    * **Heslo**: hello heslo pro účet přihlášení hello clusteru.

 ![alias dialogové okno Přidání](./media/hdinsight-connect-hive-jdbc-driver/addalias.png)

    Použití hello **Test** tooverify tlačítko, která hello připojení funguje. Když **připojit k: Hive v HDInsight** otevře se dialogové okno, vyberte **připojit** tooperform hello testu. Pokud hello test úspěšný, zobrazí **úspěšné připojení** dialogové okno.

    připojení hello toosave alias, použijte hello **Ok** tlačítko dole hello hello **přidat Alias** dialogové okno.

7. Z hello **připojit k** vyberte rozevírací seznam v horní části hello SQL SQuirreL **Hive v HDInsight**. Po zobrazení výzvy vyberte **Connect**.

    ![Dialogové okno připojení](./media/hdinsight-connect-hive-jdbc-driver/connect.png)

8. Po připojení, zadejte následující dotaz do hello dialog dotazu SQL a pak vyberte hello hello **spustit** ikonu. Hello výsledky oblasti by měl zobrazit hello výsledky dotazu hello.

        select * from hivesampletable limit 10;

    ![Dialogové okno dotazu SQL, včetně výsledků](./media/hdinsight-connect-hive-jdbc-driver/sqlquery.png)

## <a name="connect-from-an-example-java-application"></a>Připojení z příklad aplikace Java

Příklad použití Java klienta tooquery Hive v HDInsight je k dispozici na [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc). Postupujte podle pokynů hello v toobuild hello úložiště a spusťte ukázkové hello.

## <a name="troubleshooting"></a>Řešení potíží

### <a name="unexpected-error-occurred-attempting-tooopen-an-sql-connection"></a>Došlo k neočekávané chybě při pokusu tooopen připojení k SQL

**Příznaky**: při připojování tooan clusteru HDInsight, který je verze 3.3 nebo 3.4, zobrazí chybu, ke které došlo k neočekávané chybě. Hello trasování zásobníku pro tuto chybu začíná textem hello následující řádky:

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

**Příčina**: Tato chyba je způsobená neshodou hello verzi souboru commons codec.jar hello používané SQuirreL a hello jeden požadované součásti Hive JDBC hello.

**Řešení**: toofix tato chyba, hello použijte následující kroky:

1. Stáhněte si soubor jar commons kodeků hello z clusteru HDInsight.

        scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-codec*.jar ./commons-codec.jar

2. Ukončete SQuirreL a potom přejděte toohello directory nainstalovanou SQuirreL ve vašem systému. V adresáři SquirreL hello, v části hello `lib` adresář, nahraďte hello existující commons-codec.jar s hello jeden stažený z clusteru HDInsight hello.

3. Restartujte SQuirreL. Chyba Hello provedeno už při připojování tooHive v HDInsight.

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili, jak toouse toowork JDBC s Hive, hello použijte následující odkazy tooexplore jiné způsoby toowork s Azure HDInsight.

* [Nahrání dat tooHDInsight](hdinsight-upload-data.md)
* [Použití Hivu se službou HDInsight](hdinsight-use-hive.md)
* [Použití Pigu se službou HDInsight](hdinsight-use-pig.md)
* [Použití úloh MapReduce se službou HDInsight](hdinsight-use-mapreduce.md)
