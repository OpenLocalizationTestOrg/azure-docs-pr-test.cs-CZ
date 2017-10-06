---
title: "aaaCreate clusterů systému Hadoop pomocí webového prohlížeče - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toocreate Hadoop, HBase, Storm a Spark clustery v Linuxu pro HDInsight pomocí webového prohlížeče a hello portál Azure preview."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 697278cf-0032-4f7c-b9b2-a84c4347659e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 63027e35e2d66dd76218aff3e0c085fc811736ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-linux-based-clusters-in-hdinsight-using-hello-azure-portal"></a>Vytvořit clustery se systémem Linux v HDInsight pomocí hello portálu Azure
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Hello portál Azure je nástroj pro správu webových služeb a prostředků, které jsou hostované v cloudu Microsoft Azure hello. V tomto článku se dozvíte, jak toocreate HDInsight se systémem Linux clusterů pomocí portálu hello.

## <a name="prerequisites"></a>Požadavky
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Moderní webový prohlížeč**. Hello portál Azure používá HTML5 a Javascript a nemusí fungovat správně starší verze webových prohlížečů.

## <a name="create-clusters"></a>Vytváření clusterů
Hello portál Azure zpřístupní většinu vlastností clusteru hello. Pomocí šablony Azure Resource Manager, můžete skrýt spoustu dalších podrobností. Další informace najdete v tématu [vytvořit systémem Linux Hadoop clusterů v HDInsight pomocí šablony Azure Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko  **+** , klikněte na tlačítko **Intelligence + analýzy**a potom klikněte na **HDInsight**.
   
    ![Vytvoření nového clusteru v hello portál Azure](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster.png "vytvoření nového clusteru v hello portálu Azure")

3. V hello **HDInsight** okně klikněte na tlačítko **vlastní (velikost, nastavení, aplikace)**, klikněte na tlačítko **Základy**a potom zadejte následující informace hello.

    ![Vytvoření nového clusteru v hello portál Azure](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-basics.png "vytvoření nového clusteru v hello portálu Azure")

    * Zadejte **Název clusteru**: Tento název musí být globálně jedinečný.

    * Z hello **předplatné** hello rozevíracího seznamu, vyberte předplatné Azure, který se použije pro hello cluster.

    * Klikněte na tlačítko **clusteru typu**a potom vyberte:
   
        * **Typ clusteru**: Pokud si nejste jisti, jaké toochoose, vyberte **Hadoop**. Je nejoblíbenější typ clusteru hello.
     
            > [!IMPORTANT]
            > HDInsight clustery mohou mít různé typy, které odpovídají toohello zatížení nebo technologie, která hello clusteru je přizpůsobená pro. Neexistuje žádná podporovaná metoda toocreate clusteru, který kombinuje více typů, jako je například Storm a HBase v jednom clusteru. 
            > 
            > 
        
        * **Operační systém**: Vyberte **Linux**.
        
        * **Verze**: použijte výchozí verze hello, pokud nevíte, jaká toochoose. Další informace najdete v článku [Verze clusterů HDInsight](hdinsight-component-versioning.md).
        * **Cluster vrstvy**: Azure HDInsight nabízí cloud nabídky velkých objemů dat hello ve dvou kategoriích: úrovně Standard a Premium vrstvy. Další informace najdete v článku [Úrovně clusterů](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers).

    * Pro **uživatelské jméno přihlášení clusteru** a **clusteru přihlašovacího hesla**, poskytovat hello uživatelské jméno a heslo pro uživatele správce hello.

    * Zadejte **uživatelské jméno SSH** a pokud chcete, aby heslo SSH hello toohave je stejný jako hello jste dřív zadali heslo správce, vyberte hello **použijte stejné heslo jako přihlašovací údaje clusteru** zaškrtávací políčko. Pokud ne, zadejte buď **heslo** nebo **veřejný klíč**, která bude uživatel SSH použité tooauthenticate hello. Pomocí veřejného klíče je hello doporučenému přístupu. Klikněte na tlačítko **vyberte** v hello dolní toosave hello přihlašovací údaje konfigurace.
   
        Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

    * Pro **skupiny prostředků**, určete, zda chcete toocreate novou skupinu prostředků, nebo použijte existující.

    * Zadejte datového centra **umístění** kde bude vytvořen hello clusteru.

    * Klikněte na **Další**.

4. Na hello **úložiště** okno, zadejte, zda chcete jako výchozí úložiště Azure Storage (WASB) nebo Data Lake Store. Podívejte se na následující další informace o tabulce hello.

    ![Vytvoření nového clusteru v hello portál Azure](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-storage.png "vytvoření nového clusteru v hello portálu Azure")

    | Úložiště                                      | Popis |
    |----------------------------------------------|-------------|
    | **Objektů BLOB služby Azure Storage jako výchozí úložiště**   | <ul><li>Pro **primárního úložiště typu**, vyberte **Azure Storage**. Potom pro **metodu výběru**, můžete zvolit **mé odběry** Pokud budete chtít toospecify účet úložiště, který je součástí vašeho předplatného Azure a pak vyberte hello účet úložiště. Jinak, klikněte na tlačítko **přístupový klíč** a poskytnout informace o hello hello účtu úložiště, které chcete toochoose z mimo vašeho předplatného Azure.</li><li>Pro **výchozí kontejner**, můžete zvolit toogo názvem hello výchozí kontejner navrhované portálem hello nebo zadat vlastní.</li><li>Pokud používáte WASB jako výchozí úložiště, můžete (volitelně) klepnutím **další účty úložiště** tooassociate s clusterem hello účtů toospecify další úložiště. V hello **klíčů k úložišti Azure** okně klikněte na tlačítko **přidejte klíč k úložišti**, a potom můžete zadat účet úložiště z vašich předplatných Azure nebo z jiných předplatných (tím, že účet úložiště hello přístupový klíč).</li><li>Pokud používáte WASB jako výchozí úložiště, můžete (volitelně) klepnutím **Data Lake Store přístup** toospecify Azure Data Lake Store jako další úložiště. Další informace najdete v tématu [vytvoření clusteru HDInsight s Data Lake Store pomocí portálu Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</li></ul> |
    | **Azure Data Lake Store jako výchozí úložiště** | Pro **primárního úložiště typu**, vyberte **Data Lake Store** a pak najdete v článku toohello [vytvoření clusteru HDInsight s Data Lake Store pomocí portálu Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) pro pokyny. |
    | **Externí metaúložiště**                      | Volitelně můžete zadat SQL databázi toosave Hive a Oozie metadata přidružená hello clusteru. Pro **vyberte databázi SQL pro Hive** vyberte databázi SQL a zadejte hello uživatelské jméno a heslo pro databázi hello. Opakujte tyto kroky pro Oozie metadata.<br><br>Některé aspekty při používání Azure SQL database pro metaúložiště. <ul><li>databáze Azure SQL Hello použitý k hello metaúložiště musí umožňovat tooother připojení služby Azure, včetně Azure HDInsight. Na panelu databáze Azure SQL hello, na pravé straně hello klikněte na název serveru hello. Toto je hello serveru, na které hello SQL je spuštěna instance databáze. Po otevření zobrazení hello serveru, klikněte na tlačítko **konfigurace**a pak pro **služeb Azure**, klikněte na tlačítko **Ano**a potom klikněte na **Uložit**.</li><li>Při vytváření metaúložiště, nepoužívejte název databáze, který obsahuje pomlčky nebo spojovníky, protože to může způsobit toofail procesu vytváření clusteru hello.</li></ul>                                                                                                                                                                       |

    Klikněte na **Další**. 

    > [!WARNING]
    > Použití účtu další úložiště v jiném umístění než hello HDInsight cluster není podporováno.

5. Volitelně klikněte na **aplikace** tooinstall aplikací, které fungují s clustery HDInsight. Tyto aplikace mohou být vytvořeny společností Microsoft, nezávislými dodavateli softwaru (ISV) nebo vámi samotnými. Další informace najdete v tématu [aplikace nainstalovat HDInsight](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation).


6. Klikněte na tlačítko **velikost clusteru** toodisplay informace o hello uzly, které budou vytvořeny pro tento cluster. Nastavit hello počet uzlů pracovního procesu, které potřebujete pro hello cluster. Hello odhadované náklady na hello clusteru se zobrazí v okně hello.
   
    ![Uzel cenové úrovně okno](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-nodes.png "zadejte počet uzlů clusteru")
   
   > [!IMPORTANT]
   > Pokud máte v plánu na víc než 32 uzlů pracovního procesu, při vytváření clusteru nebo škálování hello clusteru po vytvoření, pak je nutné vybrat velikost hlavního uzlu s alespoň s 8 jádry a 14GB paměti ram.
   > 
   > Další informace o velikosti uzlu a souvisejících nákladů, najdete v části [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).
   > 
   > 
   
   Klikněte na tlačítko **Další** uzlu hello toosave ceny konfigurace.

7. Klikněte na tlačítko **upřesňující nastavení** tooconfigure další volitelné nastavení, například pomocí **akcí skriptů** toocustomize tooinstall clusteru vlastní součásti nebo připojení **virtuální sítě**. Podívejte se na následující další informace o tabulce hello.

    ![Uzel cenové úrovně okno](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-advanced.png "zadejte počet uzlů clusteru")

    | Možnost | Popis |
    |--------|-------------|
    | **Akce skriptu** | Tuto možnost použijte, pokud chcete toouse toocustomize vlastní skript clusteru, protože vytváří se hello cluster. Další informace o akcí skriptů naleznete v tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md). |
    | **Virtual Network** | Vyberte Azure virtuální podsítě sítě a hello, pokud chcete cluster hello tooplace do virtuální sítě. Informace o používání HDInsight s virtuální síť, včetně určité požadavky na konfiguraci pro hello virtuální sítě, najdete v tématu [HDInsight rozšířit možnosti pomocí Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md). |

    Klikněte na **Další**.

8. Na hello **Souhrn** okno, zkontrolujte informace o hello jste dřív zadali a potom klikněte na **vytvořit**.

    ![Uzel cenové úrovně okno](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-summary.png "zadejte počet uzlů clusteru")
    
    > [!NOTE]
    > To bude trvat nějakou dobu toobe hello clusteru vytvořený, obvykle přibližně 15 minut. Použít hello dlaždici na úvodní panel hello nebo hello **oznámení** položku na hello levé toocheck stránku hello na hello procesu zřizování.
    > 
    > 
12. Po dokončení procesu vytváření hello, klikněte na dlaždici hello hello clusteru z hello úvodní panel toolaunch hello clusteru okno. okno Hello clusteru poskytuje hello následující informace.
    
    ![Okno clusteru](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-completed.png "vlastnosti clusteru")
    
    Použijte následující ikony hello toounderstand hello horní části tohoto okna hello.
    
    * **Přehled** kartě jsou uvedeny všechny hello základní informace o clusteru hello například hello název, skupinu prostředků hello patří do hello umístění hello operačního systému, adresa URL pro řídicí panel clusteru hello atd.
    * **Řídicí panel** vás přesměruje na portál Ambari toohello přidruženého k hello clusteru.
    * **Secure Shell**: informace potřebné tooaccess hello clusteru pomocí protokolu SSH.
    * **Škálování clusteru** umožňuje zvýšit hello počet uzlů pracovního procesu přidruženého k hello clusteru.
    * **Odstranit**: odstranění clusteru HDInsight hello.
    

## <a name="customize-clusters"></a>Přizpůsobení clusterů
* V tématu [clusterů HDInsight přizpůsobit pomocí Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).
* V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-hello-cluster"></a>Odstranění clusteru hello
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Řešení potíží

Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Další kroky
Teď, když jste úspěšně vytvořili clusteru služby HDInsight, použijte následující toolearn jak hello toowork k vašemu clusteru:

### <a name="hadoop-clusters"></a>Clustery Hadoop
* [Použití Hivu se službou HDInsight](hdinsight-use-hive.md)
* [Použití Pigu se službou HDInsight](hdinsight-use-pig.md)
* [Používání nástroje MapReduce s HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>Clustery HBase
* [Začínáme s HBase v HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Vývoj aplikací v jazyce Java pro HBase v HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Clustery Storm
* [Vývoj topologie Java pro Storm v HDInsight](hdinsight-storm-develop-java-topology.md)
* [Použití komponent, Python v Storm v HDInsight](hdinsight-storm-develop-python-topology.md)
* [Nasazení a monitorování topologie se Storm v HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Clustery Spark
* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](hdinsight-apache-spark-eventhub-streaming.md)

