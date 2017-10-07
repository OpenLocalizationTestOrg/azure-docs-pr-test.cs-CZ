---
title: "aaaGet začít s R serverem v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate platformě Apache Spark v clusteru HDInsight, který zahrnuje R Server a odeslat skript v jazyce R na clusteru hello."
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b5e111f3-c029-436c-ba22-c54a4a3016e3
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/14/2017
ms.author: bradsev
ms.openlocfilehash: f7e418bbac48eee080a4b4cfbb33e246324ea5c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-using-r-server-on-hdinsight"></a>Začínáme používat R Server ve službě HDInsight

HDInsight zahrnuje serveru R toobe možnost integrovat do clusteru HDInsight. Tato možnost umožňuje skripty R toouse Spark a MapReduce toorun distribuované výpočty. V tomto dokumentu zjistíte, jak toocreate serveru R na clusteru HDInsight a poté spustit R skript, který ukazuje, jak pomocí Spark pro distribuovaných R výpočtů.


## <a name="prerequisites"></a>Požadavky

* **Předplatné Azure:** Než začnete tento kurz, musíte mít předplatné Azure. Článek přejděte toohello [bezplatná zkušební verze Microsoft Azure získat](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) Další informace.
* **Klient Secure Shell (SSH)**: slouží klienta SSH tooremotely připojení clusteru HDInsight toohello a spusťte příkazy přímo na clusteru hello. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).
* **(Volitelné) klíče SSH**: můžete zabezpečit hello SSH účet používaný tooconnect toohello clusteru pomocí hesla nebo veřejného klíče. Pomocí hesla je jednodušší a umožňuje vám tooget spustit bez nutnosti toocreate pár veřejného a privátního klíče. Použití klíče je ale bezpečnější.

> [!NOTE]
> Hello kroky v tomto dokumentu předpokládají, že používáte heslo.


## <a name="automated-cluster-creation"></a>Automatizované vytváření clusterů

Můžete automatizovat vytváření hello serverů R HDInsight pomocí Azure Resource Manager šablony, hello SDK a také prostředí PowerShell.

* toocreate serveru R pomocí šablony Správa prostředků Azure, najdete v části [nasazení clusteru služby HDInsight serveru R](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).
* toocreate na serveru R pomocí hello .NET SDK najdete v části [vytvořit clustery se systémem Linux v HDInsight pomocí hello .NET SDK.](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)
* toodeploy R Server pomocí prostředí powershell, najdete v článku hello na [vytváření R Server v HDInsight pomocí prostředí PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-hello-cluster-using-hello-azure-portal"></a>Vytvoření clusteru hello pomocí hello portálu Azure

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).

2. Vyberte **NOVÝ** -> **Inteligentní funkce a analýzy** -> **HDInsight**.

    ![Obrázek vytváření nového clusteru](./media/hdinsight-hadoop-r-server-get-started/newcluster.png)

3. V hello **rychle vytvořit** uživatele, zadejte název pro hello cluster v hello **název clusteru** pole. Pokud máte víc předplatných Azure, použijte hello **předplatné** položka tooselect hello jeden chcete toouse.

    ![Výběr názvu clusteru a předplatného](./media/hdinsight-hadoop-r-server-get-started/clustername.png)

4. Vyberte **clusteru typ** tooopen hello **konfigurace clusteru** okno. Na hello **konfigurace clusteru** okně vyberte hello následující možnosti:

    * **Typ clusteru:** R Server
    * **Verze**: Vyberte hello verzi tooinstall R Server v clusteru hello. Hello verzi aktuálně k dispozici je ***R Server 9.1 (HDI 3.6)***. Poznámky k verzi pro hello dostupné verze serveru R jsou k dispozici [zde](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes).
    * **R Studio community edition pro počítače s R Server**: Tento IDE založené na prohlížeči je nainstalována ve výchozím nastavení v uzlu edge hello. Pokud si přejete toonot nainstalováno a potom zrušte zaškrtnutí políčka hello. Pokud se rozhodnete toohave je nainstalovat, hello adresu URL pro přístup k hello přihlášení Rstudia Server se nachází v okně portálu aplikace pro váš cluster, je po vytvoření.
    * Ponechte hello další možnosti hello výchozí hodnoty a použít hello **vyberte** toosave hello clusteru typ tlačítka.

        ![Snímek obrazovky okna Typ clusteru](./media/hdinsight-hadoop-r-server-get-started/clustertypeconfig.png)

5. Zadejte **Uživatelské jméno přihlášení clusteru** a **Heslo přihlášení clusteru**.

    Zadejte **Uživatelské jméno SSH**. SSH je použité tooremotely toohello clusteru pomocí připojit **Secure Shell (SSH)** klienta. Uživatel SSH hello můžete zadat v tomto dialogovém okně nebo po vytvoření clusteru hello (na kartě Konfigurace hello hello clusteru). R Server je nakonfigurovaný tooexpect **uživatelské jméno SSH** z "remoteuser".  **Pokud chcete použít jiné uživatelské jméno, je nutné provést další krok po vytvoření clusteru hello.**

    Ponechejte pole hello kontrola **použijte stejné heslo jako přihlašovací údaje clusteru** toouse **heslo** psaní hello ověřování Pokud dáváte přednost použití veřejného klíče.  Je nutné pár veřejného a privátního klíče tooaccess R Server v clusteru hello prostřednictvím vzdáleného klienta, jako například RTVS, Rstudia nebo jiné plochy IDE. Pokud instalujete hello edice community Rstudia serveru, je nutné toochoose zadat heslo SSH.     

    toocreate a používání páru veřejného a privátního klíče RSA, zrušte zaškrtnutí políčka **použijte stejné heslo jako přihlašovací údaje clusteru** a pak vyberte **veřejný klíč** a pokračovat následujícím způsobem. Tyto pokyny předpokládají, že máte nainstalované prostředí Cygwin, ve kterém je dostupný příkaz ssh-keygen nebo ekvivalentní příkaz.

    * Generování páru veřejného a privátního klíče RSA z příkazového řádku hello na svém přenosném počítači:

        ssh-keygen -t rsa -b 2048

    * Postupujte podle výzvy tooname hello soubor klíče a potom zadejte přístupové heslo pro zvýšení zabezpečení. Na obrazovce by měl vypadat podobně jako hello následující bitové kopie:

        ![Příkazový řádek SSH v systému Windows](./media/hdinsight-hadoop-r-server-get-started/sshcmdline.png)

    * Tento příkaz vytvoří soubor privátního klíče a soubor veřejného klíče v části hello název < privátní klíč filename > .pub, například furiosa a furiosa.pub.

        ![Adresář SSH](./media/hdinsight-hadoop-r-server-get-started/dir.png)

    * Pak zadejte hello soubor veřejného klíče (&#42;. protokol pub) při přiřazování HDI clusteru přihlašovací údaje a nakonec potvrďte skupinu prostředků a oblasti a vyberte **Další**.

        ![Okno Přihlašovací údaje](./media/hdinsight-hadoop-r-server-get-started/publickeyfile.png)  

   * Změna oprávnění u hello privátní keyfile na svém přenosném počítači:

        chmod 600 <název_souboru_privátního_klíče>

   * Použijte hello soubor privátního klíče pomocí protokolu SSH pro vzdálené přihlášení:

        ssh –i <název_souboru_privátního_klíče> remoteuser@<hostname public ip>

      Nebo jako součást definice hello vaší Hadoop Spark výpočetní kontext pro R Server hello klienta. V tématu hello **pomocí Microsoft R Server jako klienta Hadoop** pododdílu v [vytvoření kontextu výpočetní pro Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark).

6. Hello rychle vytvořit přechody, se kterými toohello **úložiště** okno tooselect hello účet úložiště toobe nastavení používá pro primárního umístění hello hello HDFS souboru systému používaného clusteru hello. Vyberte nový nebo existující účet služby Azure Storage nebo existující účet Data Lake Store.

    - Pokud vyberete účet služby Azure Storage, stávající účet úložiště je vybrána výběrem **vyberte účet úložiště** a potom vybrat příslušný účet hello. Vytvořit nový účet pomocí hello **vytvořit nový** odkaz v hello **vyberte účet úložiště** části.

      > [!NOTE]
      > Pokud vyberete **nový** musíte zadat název nového účtu úložiště hello. Pokud byla přijata hello název se zobrazí zelené zaškrtnutí.

      Hello **výchozí kontejner** výchozí název toohello hello clusteru. Toto výchozí nastavení ponechte jako hodnota hello.

      Pokud jste vybrali novou možnost účet úložiště výzva tooselect **umístění** je daný tooselect které oblasti toocreate hello účet úložiště.  

         ![Okno zdroje dat](./media/hdinsight-getting-started-with-r/datastore.png)  

      > [!IMPORTANT]
      > Výběr hello umístění pro zdroj dat výchozí hello také nastaví hello umístění hello clusteru HDInsight. Hello clusteru a výchozí zdroj dat musí být v hello stejné oblasti.

    - Pokud chcete toouse existující Data Lake Store, pak vyberte hello toouse účtu ADLS úložiště clusteru a ten přidejte hello *přidat* identity tooyour clusteru tooallow přístup toohello úložiště. Další informace o tomto procesu najdete v tématu [Vytvoření clusteru HDInsight se službou Data Lake Store pomocí webu Azure Portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal).

    Použití hello **vyberte** konfigurace zdroje dat hello toosave tlačítko.


7. Hello **Souhrn** okno pak zobrazí toovalidate všechna nastavení. Zde můžete změnit vaše **velikost clusteru** toomodify hello počet serverů v clusteru a také zadat jakýkoli **skript akce** chcete toorun. Pokud si nejste jisti, že je potřeba větší cluster, nechte hello počet uzlů pracovního procesu na výchozí hello `4`. Hello odhadované náklady na hello clusteru se zobrazí v okně hello.

    ![souhrn clusteru](./media/hdinsight-hadoop-r-server-get-started/clustersummary.png)

   > [!NOTE]
   > V případě potřeby můžete změnit velikost clusteru později prostřednictvím hello portálu (**clusteru** -> **nastavení** -> **škálování clusteru**) tooincrease nebo zmenšení hello počet uzlů pracovního procesu.  Tato změna velikosti může být užitečná pro volnoběhu dolů hello clusteru, když není používán, nebo pro přidání hello požadavků na kapacitu toomeet větší úloh.
   >
   >

   Některé faktory tookeep pamatujte při Změna velikosti vašeho clusteru, hello datové uzly a hello hraniční uzel patří:  

   * Hello výkon distribuovaných R Server analýzy na Spark je přímo úměrná toohello počet uzlů pracovního procesu, když hello dat je velká.  

   * výkon Hello R Server analýzy je lineární hello velikost dat probíhá analýza. Například:  

     * Pro malé toomodest data je vhodné, když analyzovat v kontextu místního výpočetního uzlu edge hello výkonu.  Další informace o hello scénáře pod nimiž hello místní a Spark výpočetní kontexty nejlépe fungovat, najdete v části možnosti výpočetní kontext pro R Server v HDInsight.<br>
     * Pokud přihlášení toohello hraniční uzel a spustit R skript, pak všechny ale hello ScaleR rx – funkce provést <strong>místně</strong> na uzlu edge hello. Proto hello paměť a počet jader hello hraniční uzel by měly mít velikost odpovídajícím způsobem. Hello totéž platí, pokud používáte R Server na HDI vzdálené výpočetní kontext z vašeho přenosného počítače.

     ![Okno s cenovými úrovněmi uzlů](./media/hdinsight-hadoop-r-server-get-started/pricingtier.png)

     Použití hello **vyberte** tlačítko toosave hello uzlu ceny konfigurace.

   Je uvedený také odkaz na **stažení šablony a parametrů**. Klikněte na tento odkaz toodisplay skripty, které lze použít tooautomate hello vytvoření clusteru s konfigurací vybrané hello. Tyto skripty jsou dostupné i z hello Azure portálu položka pro váš cluster po jeho vytvoření.

   > [!NOTE]
   > Jak dlouho trvá nějakou dobu toobe hello clusteru vytvořený, obvykle přibližně 20 minut. Použít hello dlaždici na úvodní panel hello nebo hello **oznámení** položku na hello levé toocheck stránku hello v procesu vytváření hello.
   >
   >

<a name="connect-to-rstudio-server"></a>
## <a name="connect-toorstudio-server"></a>Připojit tooRStudio serveru

Pokud jste vybrali tooinclude edice community Rstudia Server v instalaci, můžete přistupovat hello Rstudia přihlášení přes dvě různé metody.

1. Přejděte toohello následující adresu URL (kde **CLUSTERNAME** je název hello hello clusteru, které jste vytvořili):

    https://**NÁZEV_CLUSTERU**.azurehdinsight.net/rstudio/

2. Otevřete položku hello pro váš cluster v hello portál Azure, vyberte hello **řídicí panely serveru R** rychlé odkazu a potom vyberete hello **řídicí panel Studio R**:

     ![Řídicí panel studio hello R přístup](./media/hdinsight-getting-started-with-r/rstudiodashboard1.png)

     ![Řídicí panel studio hello R přístup](./media/hdinsight-getting-started-with-r/rstudiodashboard2.png)

   > [!IMPORTANT]
   > Bez ohledu na použité metodě hello hello při prvním přihlášení je třeba tooauthenticate dvakrát.  Při prvním ověřování hello, zadejte hello *clusteru správce userid* a *heslo*. Hello druhého řádku, zadejte hello *SSH userid* a *heslo*. Následné protokolu in vyžadují jenom hello *heslo SSH* a *userid*.

<a name="connect-to-edge-node"></a>
## <a name="connect-toohello-r-server-edge-node"></a>Připojit okrajovému uzlu serveru R toohello

Připojte okrajovému uzlu serveru tooR hello clusteru HDInsight pomocí protokolu SSH pomocí příkazu hello:

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> Můžete najít hello `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` adresu v hello portálu Azure, pak výběrem clusteru **všechna nastavení** -> **aplikace** -> **RServer**. Zobrazí informace o koncovém SSH pro hello hraniční uzel hello.
>
> ![Obrázek hello SSH Endpoint pro hello hraniční uzel](./media/hdinsight-hadoop-r-server-get-started/sshendpoint.png)
>
>

Pokud jste použili toosecure hesla účtu uživatele SSH, jste výzvami tooenter ho. Pokud jste použili veřejný klíč, můžete mít toouse hello `-i` parametr toospecify hello odpovídající soukromý klíč. Například:

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Další informace najdete v tématu [tooHDInsight (Hadoop) pomocí protokolu SSH připojit](hdinsight-hadoop-linux-use-ssh-unix.md).

Po připojení přijedete do výzva podobné následující toohello:

    sername@ed00-myrser:~$

<a name="enable-concurrent-users"></a>
## <a name="enable-multiple-concurrent-users"></a>Povolení několika souběžných uživatelů

Přidáním více uživatelů pro hello hraniční uzel, na které hello Rstudia komunity běží verze můžete povolit více souběžných uživatelů.

Při vytváření clusteru HDInsight musíte zadat dva uživatele – uživatele HTTP a uživatele SSH:

![Souběžný uživatel 1](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-1.png)

- **Uživatelské jméno přihlášení clusteru**: uživatelé HTTP pro ověřování prostřednictvím brány hello HDInsight, který je použité tooprotect hello HDInsight clustery, kterou jste vytvořili. Tento uživatel HTTP je použité tooaccess hello uživatelského rozhraní Ambari, uživatelském rozhraní YARN, jakož i ostatní součásti uživatelského rozhraní.
- **Uživatelské jméno zabezpečené Shell (SSH)**: SSH uživatele tooaccess hello clusteru prostřednictvím zabezpečeného prostředí. Tento uživatel je uživatelem v systému Linux hello hlavních uzlech, pracovní uzly a uzly okraj hello. Abyste mohli používat zabezpečený prostředí tooaccess hello uzly v clusteru s podporou vzdálené.

Hello R Studio serveru Community verze použitá v hello Microsoft R Server v clusteru HDInsight typ akceptuje pouze Linux uživatelské jméno a heslo jako mechanismus přihlášení. Nepodporuje předávání tokenů. Pokud jste vytvořili nový cluster a chcete toouse R Studio tooaccess, je nutné toolog v dvakrát.

- První přihlášení pomocí přihlašovacích údajů uživatele hello HTTP prostřednictvím hello HDInsight brány: 

    ![Souběžný uživatel 2a](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2a.png)

- Pak použijte toolog přihlašovací údaje uživatele SSH hello tooRStudio:
  
    ![Souběžný uživatel 2b](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2b.png)

V současné době je možné při zřizování clusteru HDInsight vytvořit pouze jeden uživatelský účet SSH. Takže tooenable clusterů více uživatelů tooaccess Microsoft R serverem v HDInsight, potřebujeme toocreate další uživatele v hello systému Linux.

Protože komunity Rstudia Server běží na clusteru hello hraniční uzel, existují zde několik kroků:

1. Použít toolog uživatele SSH hello vytvořit v uzlu edge toohello
2. Přidání dalších uživatelů Linuxu na hraničním uzlu
3. Verze komunity Rstudia použití vytvořit uživateli hello

### <a name="step-1-use-hello-created-ssh-user-toolog-in-toohello-edge-node"></a>Krok 1: Použití toolog uživatele SSH hello vytvořit v uzlu edge toohello

Stáhněte si nástroj žádné SSH (jako je například Putty) a použít hello existující SSH uživatele toolog v. Potom postupujte podle pokynů hello v [tooHDInsight (Hadoop) pomocí protokolu SSH připojit](hdinsight-hadoop-linux-use-ssh-unix.md) tooaccess hello hraniční uzel. Hello adresa edge uzlu serveru R na clusteru HDInsight: *clustername-ed-ssh.azurehdinsight.net*


### <a name="step-2-add-more-linux-users-in-edge-node"></a>Krok 2: Přidání dalších uživatelů Linuxu na hraničním uzlu

tooadd uživatele toohello hraniční uzel, spuštěním příkazů hello:

    sudo useradd yournewusername -m
    sudo passwd yourusername

Měli byste vidět hello položky vrátila následující: 

![Souběžný uživatel 3](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-3.png)

Po zobrazení výzvy k "aktuální heslo protokolu Kerberos:", stačí stisknout klávesu **Enter** tooignore ho. Hello `-m` možnost `useradd` příkaz znamená, že hello systému vytvoří domovské složky pro hello uživatele, který je vyžadován pro verze Rstudia komunity.


### <a name="step-3-use-rstudio-community-version-with-hello-user-created"></a>Krok 3: Použití Rstudia komunity verze vytvořit uživateli hello

Používejte toolog hello uživatelem vytvořené v tooRStudio:

![Souběžný uživatel 4](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-4.png)

Všimněte si, že Rstudia označuje, že používáte hello nového uživatele (například tady *sshuser6*) toolog v clusteru hello: 

![Souběžný uživatel 5](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-5.png)

Také můžete přihlásit pomocí přihlašovacích údajů původní hello (ve výchozím nastavení, je *sshuser*) z jiného okna prohlížeče současně.

Můžete odeslat úlohu pomocí funkcí ScaleR. Tady je příklad hello příkazy používané toorun úlohy:

    # Set hello HDFS (WASB) location of example data.
    bigDataDirRoot <- "/example/data"

    # Create a local folder for storaging data temporarily.
    source <- "/tmp/AirOnTimeCSV2012"
    dir.create(source)

    # Download data toohello tmp folder.
    remoteDir <- "http://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
    download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
    download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
    download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
    download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
    download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
    download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
    download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
    download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
    download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
    download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
    download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
    download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

    # Set directory in bigDataDirRoot tooload hello data.
    inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

    # Create hello directory.
    rxHadoopMakeDir(inputDir)

    # Copy hello data from source tooinput.
    rxHadoopCopyFromLocal(source, bigDataDirRoot)

    # Define hello HDFS (WASB) file system.
    hdfsFS <- RxHdfsFileSystem()

    # Create info list for hello airline data.
    airlineColInfo <- list(
    DAY_OF_WEEK = list(type = "factor"),
    ORIGIN = list(type = "factor"),
    DEST = list(type = "factor"),
    DEP_TIME = list(type = "integer"),
    ARR_DEL15 = list(type = "logical"))

    # Get all hello column names.
    varNames <- names(airlineColInfo)

    # Define hello text data source in HDFS.
    airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

    # Define hello text data source in local system.
    airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

    # Specify hello formula toouse.
    formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

    # Define hello Spark compute context.
    mySparkCluster <- RxSpark()

    # Set hello compute context.
    rxSetComputeContext(mySparkCluster)

    # Run a logistic regression.
    system.time(
        modelSpark <- rxLogit(formula, data = airOnTimeData)
    )

    # Display a summary.
    summary(modelSpark)


Všimněte si, že hello úlohy, odeslané jsou v různých uživatelská jména v uživatelském rozhraní YARN:

![Souběžný uživatel 6](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-6.png)

Všimněte si také že hello nově přidaní uživatelé nemají oprávnění kořenového v systému Linux, ale budou mít hello stejný přístup k souborům hello tooall v hello vzdálené HDFS a WASB úložiště.


<a name="use-r-console"></a>
## <a name="use-hello-r-console"></a>Pomocí konzoly hello R

1. Z relace SSH hello použijte následující příkaz toostart hello R konzoly hello:  

        R

2. Měli byste vidět výstup podobný toohello následující:
    
    R verze 3.2.2 (2015-08-14)--"Bezpečnost ještě efektivněji" Copyright (C) 2015 hello R Foundation pro statistické výpočty platformy: x86_64-pc-linux-gnu (64 bitů)

    R je bezplatný software a nevztahuje se na něj ŽÁDNÁ ZÁRUKA.
    Jsou úvodní tooredistribute ho za určitých podmínek.
    Podrobnosti o distribuci zobrazíte zadáním license() nebo licence().

    Podpora přirozeného jazyka, ale spuštění v anglickém národním prostředí

    R je projekt spolupráce s mnoha přispěvateli.
    Zadejte 'contributors()' Další informace a 'citation()' na tom, jak toocite R nebo R balíčky v publikacích.

    Typ 'demo()' pro některé ukázky, 'help()' online nápovědu nebo 'help.start()' pro toohelp rozhraní prohlížeče HTML.
    Typ 'q()' tooquit R.

    Microsoft R Server verze 8.0: vylepšená distribuce balíčků R Microsoftu – Copyright (C) 2016 Microsoft Corporation

    Pokud chcete zobrazit poznámky k verzi, zadejte readme().
    >

3. Z hello `>` řádek, můžete zadat kód R. R server obsahuje balíčky, které vám umožňují tooeasily interakci s Hadoop a spusťte distribuované výpočty. Například použijte následující příkaz tooview hello kořenovém hello výchozí systém souborů pro hello HDInsight cluster hello:

    rxHadoopListFiles("/")

4. Můžete také použít hello WASB styl adresování.

    rxHadoopListFiles("wasb:///")


## <a name="using-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a>Použití R Serveru ve službě HDInsight ze vzdálené instance Microsoft R Serveru nebo klienta Microsoft R Client

Je možné tooset až přístup toohello HDI Hadoop Spark výpočetní kontextu ze vzdálené instance systému Microsoft R Server nebo klienta Microsoft R systémem stolní nebo přenosný. V tématu **pomocí Microsoft R Server jako klienta Hadoop** část v hello [vytváření kontextu výpočetní pro Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md). toodo tedy potřebujete toospecify hello při definování hello RxSpark výpočetní kontext na svém přenosném počítači následující možnosti: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches a sshProfileScript. Například:


    myNameNode <- "default"
    myPort <- 0

    mySshHostname  <- 'rkrrehdi1-ed-ssh.azurehdinsight.net'  # HDI secure shell hostname
    mySshUsername  <- 'remoteuser'# HDI SSH username
    mySshSwitches  <- '-i /cygdrive/c/Data/R/davec'   # HDI SSH private key

    myhdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep="/")
    myShareDir <- paste("/var/RevoShare" , mySshUsername, sep="/")

    mySparkCluster <- RxSpark(
      hdfsShareDir = myhdfsShareDir,
      shareDir     = myShareDir,
      sshUsername  = mySshUsername,
      sshHostname  = mySshHostname,
      sshSwitches  = mySshSwitches,
      sshProfileScript = '/etc/profile',
      nameNode     = myNameNode,
      port         = myPort,
      consoleOutput= TRUE
    )


## <a name="use-a-compute-context"></a>Použití výpočetního kontextu

Výpočetní kontext umožňuje toocontrol zda výpočtu je provést lokálně na uzlu edge hello nebo rozdělené mezi hello uzly v clusteru HDInsight hello.

1. Z Rstudia serveru nebo konzoly hello R (v relaci SSH) použijte následující kód tooload příklad dat do hello výchozí úložiště pro HDInsight hello:

        # Set hello HDFS (WASB) location of example data
        bigDataDirRoot <- "/example/data"

        # create a local folder for storaging data temporarily
        source <- "/tmp/AirOnTimeCSV2012"
        dir.create(source)

        # Download data toohello tmp folder
        remoteDir <- "http://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
        download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
        download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
        download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
        download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
        download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
        download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
        download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
        download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
        download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
        download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
        download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
        download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

        # Set directory in bigDataDirRoot tooload hello data into
        inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

        # Make hello directory
        rxHadoopMakeDir(inputDir)

        # Copy hello data from source tooinput
        rxHadoopCopyFromLocal(source, bigDataDirRoot)

2. Dále umožňuje vytvořit některé informace data a definovat dvou zdrojů dat, aby jsme můžete pracovat s daty hello.

        # Define hello HDFS (WASB) file system
        hdfsFS <- RxHdfsFileSystem()

        # Create info list for hello airline data
        airlineColInfo <- list(
             DAY_OF_WEEK = list(type = "factor"),
             ORIGIN = list(type = "factor"),
             DEST = list(type = "factor"),
             DEP_TIME = list(type = "integer"),
             ARR_DEL15 = list(type = "logical"))

        # get all hello column names
        varNames <- names(airlineColInfo)

        # Define hello text data source in hdfs
        airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

        # Define hello text data source in local system
        airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

        # formula toouse
        formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

3. Umožňuje spustit logistic regression nad hello dat pomocí kontextu místní výpočetní hello.

        # Set a local compute context
        rxSetComputeContext("local")

        # Run a logistic regression
        system.time(
           modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
        )

        # Display a summary
        summary(modelLocal)

    Měli byste vidět výstup, který končí toohello podobné řádky následující:

        Data: airOnTimeDataLocal (RxTextData Data Source)
        File name: /tmp/AirOnTimeCSV2012
        Dependent variable(s): ARR_DEL15
        Total independent variables: 634 (Including number dropped: 3)
        Number of valid observations: 6005381
        Number of missing observations: 91381
        -2*LogLikelihood: 5143814.1504 (Residual deviance on 6004750 degrees of freedom)

        Coefficients:
                         Estimate Std. Error z value Pr(>|z|)
         (Intercept)   -3.370e+00  1.051e+00  -3.208  0.00134 **
         ORIGIN=JFK     4.549e-01  7.915e-01   0.575  0.56548
         ORIGIN=LAX     5.265e-01  7.915e-01   0.665  0.50590
         ......
         DEST=SHD       5.975e-01  9.371e-01   0.638  0.52377
         DEST=TTN       4.563e-01  9.520e-01   0.479  0.63172
         DEST=LAR      -1.270e+00  7.575e-01  -1.676  0.09364 .
         DEST=BPT         Dropped    Dropped Dropped  Dropped

         ---

         Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

         Condition number of final variance-covariance matrix: 11904202
         Number of iterations: 7

4. Dále umožňuje spustit hello stejné logistic regression pomocí kontextu Spark hello. kontext Spark Hello distribuuje hello zpracování přes všechny uzly pracovního procesu hello v clusteru HDInsight hello.

        # Define hello Spark compute context
        mySparkCluster <- RxSpark()

        # Set hello compute context
        rxSetComputeContext(mySparkCluster)

        # Run a logistic regression
        system.time(  
           modelSpark <- rxLogit(formula, data = airOnTimeData)
        )
        
        # Display a summary
        summary(modelSpark)


   > [!NOTE]
   > Můžete taky MapReduce toodistribute výpočetní mezi uzly clusteru. Další informace o výpočetním kontextu najdete v tématu [Možnosti výpočetního kontextu pro R Server ve službě HDInsight](hdinsight-hadoop-r-server-compute-contexts.md).


## <a name="distribute-r-code-toomultiple-nodes"></a>Distribuovat uzly toomultiple kód R

S R Server můžete snadno využít existující kód R a spusťte napříč několika uzly v clusteru hello pomocí `rxExec`. Tato funkce je užitečná při uklízení parametrů nebo provádění simulací. Hello následující kód představuje příklad toouse `rxExec`:

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

Pokud stále používáte hello Spark nebo kontextu MapReduce, tento příkaz vrátí hodnotu hello nodename pro uzly pracovního procesu hello tento kód hello `(Sys.info()["nodename"])` běží na. Například na cluster se čtyřmi uzly očekáváte, že tooreceive výstup podobný toohello následující:

    $rxElem1
        nodename
    "wn3-myrser"

    $rxElem2
        nodename
    "wn0-myrser"

    $rxElem3
        nodename
    "wn3-myrser"

    $rxElem4
        nodename
    "wn3-myrser"


## <a name="accessing-data-in-hive-and-parquet"></a>Přístup k datům v Hive a Parquet

K dispozici v R Server 9.1 funkce umožňuje přímý přístup toodata v Hive a Parquet pro použití funkce ScaleR v kontextu výpočetní hello Spark. Tyto možnosti jsou k dispozici prostřednictvím nové ScaleR datového zdroje funkce volané RxHiveData a RxParquetData které fungovat prostřednictvím použití Spark SQL tooload dat přímo do DataFrame Spark pro analýzu ScaleR.  

Následující kód Hello poskytuje ukázkový kód při použití nových funkcí hello:

    #Create a Spark compute context:
    myHadoopCluster <- rxSparkConnect(reset = TRUE)

    #Retrieve some sample data from Hive and run a model:
    hiveData <- RxHiveData("select * from hivesampletable",
                     colInfo = list(devicemake = list(type = "factor")))
    rxGetInfo(hiveData, getVarInfo = TRUE)

    rxLinMod(querydwelltime ~ devicemake, data=hiveData)

    #Retrieve some sample data from Parquet and run a model:
    rxHadoopMakeDir('/share')
    rxHadoopCopyFromLocal(file.path(rxGetOption('sampleDataDir'), 'claimsParquet/'), '/share/')
    pqData <- RxParquetData('/share/claimsParquet',
                     colInfo = list(
                age    = list(type = "factor"),
               car.age = list(type = "factor"),
                  type = list(type = "factor")
             ) )
    rxGetInfo(pqData, getVarInfo = TRUE)

    rxNaiveBayes(type ~ age + cost, data = pqData)

    #Check on Spark data objects, cleanup, and close hello Spark session:
    lsObj <- rxSparkListData() # two data objs are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() # it should show empty list
    rxSparkDisconnect(myHadoopCluster)


Další informace o těchto nových funkcí naleznete v online nápovědě hello v R Server prostřednictvím použití hello `?RxHivedata` a `?RxParquetData` příkazy.  


## <a name="install-additional-r-packages-on-hello-edge-node"></a>Nainstalujte další balíčky R na uzlu edge hello

Pokud chcete tooinstall další balíčky R na hello hraniční uzel, můžete použít `install.packages()` přímo z uvnitř hello konzoly R při připojené toohello okraj uzlu prostřednictvím SSH. Pokud potřebujete balíčky R tooinstall na uzly pracovního procesu hello hello clusteru, ale musí používat akce skriptu.

Akce skriptů jsou skripty Bash, které jsou používané toomake konfigurace změny toohello HDInsight clusteru nebo tooinstall další software, například další balíčky R. Další balíčky tooinstall pomocí akce skriptu, použijte hello následující kroky:

> [!IMPORTANT]
> Pomocí skriptových akcí tooinstall další balíčky R dá použít jenom po vytvoření clusteru hello. Tento postup nepoužívejte při vytváření clusteru, protože skript hello používá tento údaj R Server úplně nainstalována a nakonfigurována.
>
>

1. Z hello [portál Azure](https://portal.azure.com), vyberte svůj Server R na clusteru HDInsight.

2. Z hello **nastavení** vyberte **akcí skriptů** a potom **odeslání nové** toosubmit nové akce skriptu.

   ![Obrázek okna Akce skriptů](./media/hdinsight-hadoop-r-server-get-started/scriptaction.png)

3. Z hello **odeslat akci se skripty** okno, zadejte hello následující informace:

   * **Název**: popisný název tooidentify tento skript

   * **Identifikátor URI skriptu Bash:**`http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`

   * **Hlavní:** Tato položka by měla být **nezaškrtnutá**.

   * **Pracovní:** Tato položka by měla být **zaškrtnutá**.

   * **Hraniční uzly:** Tato položka by měla být **nezaškrtnutá**.

   * **Zookeeper:** Tato položka by měla být **nezaškrtnutá**.

   * **Parametry**: hello R balíčky toobe nainstalována. Například `bitops stringr arules`.

   * **Zachovat tento skript...:** Tato položka by měla být **zaškrtnutá**.  

   > [!NOTE]
   > 1. Ve výchozím nastavení nejsou nainstalovány všechny balíčky R ze snímku úložiště Microsoft MRAN hello konzistentní s verzí hello R Server, který byl nainstalován. Pokud chcete tooinstall novější verze balíčků, je některé riziko nekompatibilita. Tento typ instalace je ale možné zadáním `useCRAN` jako hello hello balíčku první prvek seznamu, například `useCRAN bitops, stringr, arules`.  
   > 2. Některé balíčky R vyžadují další linuxové systémové knihovny. Pro usnadnění práce jsme nainstalovali předem hello závislosti nutné hello nejvyšší 100 nejoblíbenější R balíčky. Ale pokud balíčky R hello, který nainstalujete vyžadují knihovny nad rámec těchto pak musíte stáhnout hello základní skriptu použít se zde a přidat kroky tooinstall hello systému knihovny. Je nutné nahrávání hello upravit skript tooa veřejné kontejner v úložišti Azure objektů blob a používat hello upravit skript tooinstall hello balíčky.
   >    Další informace o vývoji akcí skriptů najdete v tématu [Vývoj akcí skriptů](hdinsight-hadoop-script-actions-linux.md).  
   >
   >

   ![Přidání akce skriptu](./media/hdinsight-getting-started-with-r/submitscriptaction.png)

4. Vyberte **vytvořit** toorun hello skriptu. Po dokončení skriptu hello hello R balíčky jsou k dispozici na všechny uzly pracovního procesu.


## <a name="using-microsoft-r-server-operationalization"></a>Použití operacionalizace Microsoft R Serveru

Po dokončení modelování vaše data můžete zprovoznit hello modelu toomake předpovědi. tooconfigure pro operationalization Microsoft R Server, proveďte následující kroky hello:

První, ssh do uzlu Edge hello. Například: 

    ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Po použití ssh, změňte adresář pro příslušné verze hello a soubor dll dotnet sudo hello: 

- Pro Microsoft R Server 9.1:

    cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0 sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll

- Pro Microsoft R Server 9.0:

    cd /usr/lib64/microsoft-deployr/9.0.1 sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

Microsoft R Server operationalization tooconfigure s konfigurací jedné pole hello následující:

1. Vyberte „Configure R Server for Operationalization“ (Konfigurovat R Server pro operacionalizaci).
2. Vyberte „A. One-box (web + compute nodes)“ (Jednotná konfigurace webu a výpočetních uzlů).
3. Zadejte heslo pro hello **správce** uživatele

![jednotná konfigurace operacionalizace](./media/hdinsight-hadoop-r-server-get-started/admin-util-one-box-.png)

Volitelně můžete provést diagnostické kontroly spuštěním diagnostického testu následujícím způsobem:

1. Vyberte „6. Run diagnostic tests“ (Spustit diagnostické testy).
2. Vyberte „A. Test configuration“ (Testovat konfiguraci).
3. Zadejte uživatelské jméno admin a heslo, které jste zadali v předchozím kroku konfigurace.
4. Ověřte, že se zobrazí „Overall Health: pass“ (Celkový stav: v pořádku)
5. Ukončení hello nástroj pro správu
6. Ukončete připojení SSH

![Diagnostika operacionalizace](./media/hdinsight-hadoop-r-server-get-started/admin-util-diagnostics.png)


>[!NOTE]
>**Dlouhá zpoždění při využívání webové služby ve Sparku**
>
>Pokud dojde k prodlevám při pokusu o tooconsume webové služby vytvořené pomocí funkce mrsdeploy v kontextu výpočtů Spark, musíte tooadd některé chybějící složky. Hello aplikací Spark patří tooa uživatele volat '*rserve2*se vždy, když je volána z webové služby pomocí mrsdeploy funkcí. toowork chcete tento problém vyřešit:

    # Create these required folders for user 'rserve2' in local and hdfs:

    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    # Next, create a new Spark compute context:
 
    rxSparkConnect(reset = TRUE)


V této fázi hello konfigurace pro Operationalization byla dokončena. Nyní můžete pomocí hello mrsdeploy balíček na vaše toohello tooconnect RClient Operationalization na hraniční uzel a začít používat jeho funkce, jako jsou [vzdálené spuštění](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) a [webové služby](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette). V závislosti na tom, jestli váš cluster je nastavený na virtuální síť nebo ne může být nutné tooset až port dopředného tunelové propojení prostřednictvím přihlašování přes SSH. Hello následující oddíly popisují, jak tooset až toto tunelové propojení.

### <a name="rserver-cluster-on-virtual-network"></a>Cluster R Serveru ve virtuální síti

Ujistěte se, že povolíte provoz přes port 12800 toohello hraniční uzel. Tímto způsobem můžete hello hraniční uzel tooconnect toohello Operationalization funkce.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


Pokud hello `remoteLogin()` možné připojit toohello hraniční uzel, ale můžete SSH toohello hraniční uzel, a potom je nutné tooverify, zda text hello pravidlo tooallow přenosy na portu 12800 byla nastavena správně nebo není. Pokud budete pokračovat tooface hello problém, můžete je vyřešit nastavením portu dopředného tunelové propojení prostřednictvím SSH. Pokyny najdete v tématu hello následující části.

### <a name="rserver-cluster-not-set-up-on-virtual-network"></a>Cluster R Serveru nastavený mimo virtuální síť

Pokud váš cluster není nastavený ve virtuální síti nebo máte potíže s připojením přes virtuální síť, můžete použít přesměrování portu tunelovým propojením přes SSH:

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

V PuTTY to také můžete nastavit.

![připojení ssh v putty](./media/hdinsight-hadoop-r-server-get-started/putty.png)

Po relace SSH je aktivní, se předají hello provoz z vašeho počítače port 12800 toohello hraniční uzel port 12800 prostřednictvím relace SSH. Nezapomeňte v metodě `remoteLogin()` použít adresu `127.0.0.1:12800`. Přihlásí toohello hraniční uzel operationalization prostřednictvím portu předávání.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="how-tooscale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a>Jak tooscale Microsoft R Server Operationalization výpočetní uzly na HDInsight uzlů pracovního procesu

### <a name="decommission-hello-worker-nodes"></a>Vyřadit z provozu hello pracovní uzly

Microsoft R Server v současné době není spravován přes YARN. Pokud hello pracovní uzly nejsou vyřazení, hello Yarn Resource Manager nebude fungovat podle očekávání, protože nebude vědět hello prostředků se zabírá serverem hello. V pořadí tooavoid této situaci, doporučujeme před škálovat výpočetní uzly hello vyřazení z provozu hello uzlů pracovního procesu.

Kroky toodecommissioning pracovní uzly:

* Přihlaste se v konzole Ambari tooHDI clusteru a klikněte na kartě "hostitele"
* Vyberte pracovní uzly (toobe vyřazena z provozu), klikněte na "Akce" > "Vybrané hostitele" > "Hostitele" > klikněte na "Zapnout ON režimu údržby". Například v hello následující obrázek jsme vybrali wn3 a wn4 toodecommission.  

   ![vyřazení pracovních uzlů z provozu](./media/hdinsight-hadoop-r-server-get-started/get-started-operationalization.png)  

* Vyberte **Actions** (Akce) > **Selected Hosts** (Vybraní hostitelé) > **DataNodes** (Datové uzly) a klikněte na **Decommission** (Vyřadit z provozu).
* Vyberte **Actions** (Akce) > **Selected Hosts** (Vybraní hostitelé) > **NodeManagers** (Správci uzlů) a klikněte na **Decommission** (Vyřadit z provozu).
* Vyberte **Actions** (Akce) > **Selected Hosts** (Vybraní hostitelé) > **DataNodes** (Datové uzly) a klikněte na **Stop** (Zastavit).
* Vyberte **Actions** (Akce) > **Selected Hosts** (Vybraní hostitelé) > **NodeManagers** (Správci uzlů) a klikněte na **Stop** (Zastavit).
* Vyberte **Actions** (Akce) > **Selected Hosts** (Vybraní hostitelé) > **Hosts** (Hostitelé) a klikněte na **Stop All Components** (Zastavit všechny komponenty).
* Zrušte výběr uzlů pracovního procesu hello a vyberte hlavních uzlech hello
* Vyberte **Actions** (Akce) > **Selected Hosts** (Vybraní hostitelé) > **Hosts** (Hostitelé) > **Restart All Components** (Restartovat všechny komponenty).

### <a name="configure-compute-nodes-on-each-decommissioned-worker-nodes"></a>Konfigurace výpočetních uzlů na všech vyřazených pracovních uzlech

1. Přihlaste se přes SSH do každého vyřazeného pracovního uzlu.
2. Spusťte nástroj pro správu pomocí příkazu `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.
3. Zadejte "1" tooselect možnost "Konfigurace R Server pro Operationalization".
4. Zadejte možnost "c" tooselect "C. Compute node“ (Výpočetní uzel). Tím se nakonfiguruje hello výpočetním uzlu na hello pracovního uzlu.
5. Ukončení hello nástroj pro správu.

### <a name="add-compute-nodes-details-on-web-node"></a>Přidání podrobností o výpočetních uzlech do webového uzlu

Po byly nakonfigurovány všechny uzly pracovního procesu vyřazení toorun výpočetního uzlu, vraťte se na hello hraniční uzel a přidat vyřazení pracovní uzly IP adresy v konfiguraci hello Microsoft uzlu serveru R web na:

* SSH do uzlu edge hello.
* Spusťte `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`.
* Vyhledejte část "Identifikátory URI" hello a přidejte IP pracovního uzlu a podrobnosti o portu.

    ![příkazový řádek vyřazení pracovních uzlů z provozu](./media/hdinsight-hadoop-r-server-get-started/get-started-op-cmd.png)


## <a name="troubleshoot"></a>Řešení potíží

Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).


## <a name="next-steps"></a>Další kroky

Nyní byste měli porozumět, jak toocreate nové clusteru HDInsight, která zahrnuje hello R Server a hello základy používání hello R konzoly z relace SSH. Hello následující témata popisují jiné způsoby správy a práce s R serverem v HDInsight:

* [Přidat Server Rstudia tooHDInsight (Pokud není nainstalován během vytváření clusteru)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Možnosti výpočetního kontextu pro R Server ve službě HDInsight](hdinsight-hadoop-r-server-compute-contexts.md)
* [Možnosti služby Azure Storage pro R Server ve službě HDInsight](hdinsight-hadoop-r-server-storage.md)
