---
title: "aaaGet začít s Hadoop a Hive v Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toocreate HDInsight clustery a dotaz na data pomocí Hive."
keywords: "začínáme používat hadoop, hadoop linux, hadoop rychlý start, hive začínáme, hive rychlý start"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6a12ed4c-9d49-4990-abf5-0a79fdfca459
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/23/2017
ms.author: jgao
ms.openlocfilehash: 3d96d78121200ebda3626dd2c3885e3ddacd546d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hadoop-tutorial-get-started-using-hadoop-in-hdinsight"></a>Kurz Hadoopu: Začínáme používat Hadoop v HDInsight

Zjistěte, jak toocreate [Hadoop](http://hadoop.apache.org/) clusterů v HDInsight a jak úlohy toorun Hive v HDInsight. [Apache Hive](https://hive.apache.org/) je nejoblíbenější součástí ekosystému Hadoop hello hello. Aktuálně se HDInsight dodává se [sedmi různými typy clusteru](hdinsight-hadoop-introduction.md#overview). Každý typ clusteru podporuje odlišnou sadu komponent. Všechny typy clusteru podporují Hive. Seznam podporovaných součásti v HDInsight, naleznete v části [co je nového ve verzích clusterů systému Hadoop hello poskytovaných v HDInsight?](hdinsight-component-versioning.md)  

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít:

* **Předplatné Azure**: toocreate Bezplatný zkušební účet jeden měsíc, procházet příliš[azure.microsoft.com/free](https://azure.microsoft.com/free).

## <a name="create-cluster"></a>Vytvoření clusteru

Většina úloh Hadoop jsou dávkové úlohy. Vytvořit cluster, spustíte některé úlohy a pak odstraňte hello clusteru. V této části vytvoříte cluster Hadoop ve službě HDInsight pomocí [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md). Zkušenosti s šablonou Resource Manageru nejsou pro postup dle tohoto kurzu vyžadovány. Další metody vytváření clusterů a principy vlastnosti hello používané v tomto kurzu, najdete v tématu [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Použijte selektor hello na hello horní části stránky toochoose hello možnosti vytvoření clusteru.

Šablona Resource Manager Hello použili v tomto kurzu se nachází v [Githubu](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/). 

1. Klikněte na tlačítko hello toosign bitové kopie v tooAzure a otevřete hello šablony Resource Manageru v hello portálu Azure. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-ssh-password%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. Zadejte nebo vyberte hello následující hodnoty:
   
    ![HDInsight Linux Začínáme šablony Resource Manageru na portálu](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-arm-template-on-portal.png "hello clusteru Hadoop nasazení v HDInsigut pomocí portálu Azure a šablony správce prostředků skupiny").
   
    * **Předplatné**: Vyberte své předplatné Azure.
    * **Skupina prostředků**: Vytvořte skupinu prostředků, nebo vyberte existující.  Skupina prostředků je kontejner komponent Azure.  V takovém případě obsahuje hello skupinu prostředků clusteru HDInsight hello a hello závislý účet úložiště Azure. 
    * **Umístění**: Vyberte místo, kam chcete toocreate, cluster Azure umístění.  Zvolte blíže tooyou pro lepší výkon umístění. 
    * **Typ clusteru**: Pro účely tohoto kurzu vyberte **hadoop**.
    * **Název clusteru**: Zadejte název pro hello Hadoop cluster.
    * **Přihlašovací jméno a heslo clusteru**: hello výchozí přihlašovací jméno je **správce**.
    * **SSH uživatelské jméno a heslo**: výchozí uživatelské jméno hello **sshuser**.  Můžete ho změnit. 
     
    Některé vlastnosti nejsou pevně kódovaný v šabloně hello.  Můžete nakonfigurovat tyto hodnoty z šablony hello.

    * **Umístění**: hello umístění hello clusteru a hello účet závislého úložiště sdílené složky hello stejné umístění jako skupina prostředků hello.
    * **Verze clusteru:** 3.5
    * **Typ operačního systému**: Linux
    * **Počet pracovních uzlů**: 2

     Každý cluster obsahuje závislost [účtu Azure Storage](hdinsight-hadoop-use-blob-storage.md) nebo [účtu Azure Data Lake](hdinsight-hadoop-use-data-lake-store.md). Označuje se jako výchozí účet úložiště hello. HDInsight cluster a jeho výchozí účet úložiště musí být společně umístěny v hello stejné oblasti Azure. Odstraněním clusterů nedojde k odstranění účtu úložiště hello. 
     
     Podrobnější vysvětlení těchto vlastností naleznete v tématu [Vytváření clusterů Hadoop ve službě HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

3. Vyberte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše** a **Pin toodashboard**a potom klikněte na **nákupu**. Zobrazí se nová dlaždice s názvem **nasazení šablony** na řídicí panel portálu hello. Trvá přibližně 20 minut toocreate cluster. Po vytvoření clusteru hello hello titulek hello dlaždice je změněné toohello název skupiny prostředků, které zadáte. A skupina prostředků hello hello portálu automaticky otevře v novém okně. Uvidíte hello clusteru i hello výchozí úložiště uvedené.
   
    ![Počáteční skupina prostředků HDInsight Linux](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-resource-group.png "Skupina prostředků clusteru Azure HDInsight").

4. Klikněte na tlačítko hello clusteru název tooopen hello clusteru v novém okně.

   ![Počáteční nastavení clusteru HDInsight Linux](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-cluster-settings.png "Vlastnosti clusteru HDInsight")


## <a name="run-hive-queries"></a>Spuštění dotazů Hive
[Apache Hive](hdinsight-use-hive.md) je hello nejoblíbenější součástí používanou v HDInsight. Existuje mnoho způsobů toorun úlohy Hive v HDInsight. V tomto kurzu použijete zobrazení Ambari Hive z portálu hello hello. Další metody pro odesílání úloh Hive naleznete v části [Použití Hive v HDInsight](hdinsight-use-hive.md).

1. Z hello předchozím snímku obrazovky, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **řídicí panel clusteru HDInsight**.  Můžete také vyhledat příliš **https://&lt;ClusterName >. azurehdinsight.net**, kde &lt;ClusterName > je hello clusteru, kterou jste vytvořili v předchozí části tooopen hello Ambari.
2. Zadejte hello Hadoop uživatelské jméno a heslo, které jste zadali v předchozí části hello. výchozí uživatelské jméno Hello **správce**.
3. Otevřete **zobrazení Hive** jak ukazuje následující snímek obrazovky hello:
   
    ![Výběr zobrazení Ambari](./media/hdinsight-hadoop-linux-tutorial-get-started/selecthiveview.png "Nabídka zobrazení Hive služby HDInsight").
4. V hello **Editor dotazů** oddílu hello stránky, hello vložte následující příkazy HiveQL do listu hello:
   
        SHOW TABLES;
   
   > [!NOTE]
   > Hive vyžaduje středník.       
   > 
   > 
5. Klikněte na tlačítko **Spustit**. A **výsledky zpracování dotazu** část by měla být pod hello Editor dotazů a zobrazení informací o hello úlohy. 
   
    Po dokončení dotazu hello hello **výsledky zpracování dotazu** části zobrazí výsledky hello hello operace. Zobrazí jedna tabulka s názvem **hivesampletable**. Tato vzorová tabulka Hive obsahuje všechny clustery HDInsight hello.
   
    ![Zobrazení Hive služby HDInsight](./media/hdinsight-hadoop-linux-tutorial-get-started/hiveview.png "Editor dotazů zobrazení Hive služby HDInsight").
6. Opakujte kroky 4 a hello toorun krok 5 následující dotaz:
   
        SELECT * FROM hivesampletable;
   
   > [!TIP]
   > Poznámka: hello **výsledky uložit** rozevírací seznam v horní hello levé hello **výsledky zpracování dotazu** část můžete použít tento výsledky stahování hello tooeither nebo uložíte tooHDInsight úložiště jako soubor CSV.
   > 
   > 
7. Klikněte na tlačítko **historie** tooget seznam úloh hello.

Po dokončení úlohy Hive můžete [exportovat hello výsledky tooAzure SQL database nebo databáze systému SQL Server](hdinsight-use-sqoop-mac-linux.md), můžete také [vizualizace výsledků hello pomocí aplikace Excel](hdinsight-connect-excel-power-query.md). Další informace o používání Hive v HDInsight, naleznete v části [použití Hive a HiveQL s Hadoop v HDInsight tooanalyze ukázkového souboru Apache log4j](hdinsight-use-hive.md).

## <a name="clean-up-hello-tutorial"></a>Vyčistěte kurz hello
Po dokončení kurzu hello můžete toodelete hello clusteru. Pomocí HDInsight jsou vaše data uložena v Azure Storage, takže můžete clusteru bezpečně odstranit, pokud není používán. Za cluster služby HDInsight se účtují poplatky, i když se nepoužívá. Vzhledem k tomu, že hello poplatky za hello clusteru jsou mnohokrát víc než hello poplatky za úložiště, dává ekonomický smysl clustery toodelete které nejsou používána. 

> [!NOTE]
> Pomocí [Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md), můžete vytvořit clustery HDInsight na vyžádání a nakonfigurovat TimeToLive nastavení automaticky příliš odstranění clusterů hello. 
> 
> 

**toodelete hello clusteru a/nebo hello výchozí účet úložiště**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na panelu portálu hello klikněte na dlaždici hello s hello název skupiny prostředků, který jste použili při vytvoření clusteru hello.
3. Klikněte na tlačítko **odstranit** na hello prostředků okno toodelete hello skupinu prostředků, která obsahuje hello clusteru a hello výchozí účet úložiště nebo klikněte na název clusteru hello na hello **prostředky** dlaždici a potom klikněte na **Odstranit** v okně clusteru hello. Poznámka: odstranění skupiny prostředků hello odstraní účet úložiště hello. Pokud chcete účet úložiště hello tookeep, zvolte toodelete hello pouze clusteru.

## <a name="troubleshoot"></a>Řešení potíží

Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili, jak toocreate HDInsight se systémem Linux clusteru pomocí šablony Resource Manageru a tooperform základní dotazy Hive.

toolearn Další informace o analýze dat pomocí HDInsight, najdete v části hello následující články:

* toolearn Další informace o používání Hive s HDInsight, včetně jak dotazuje tooperform Hive ze sady Visual Studio, najdete v části [používání Hive s HDInsight][hdinsight-use-hive].
* toolearn o Pig jazyk používaný datový tootransform najdete v tématu [použijte Pig s HDInsight][hdinsight-use-pig].
* toolearn o MapReduce, způsob, jakým toowrite programy, které zpracovávají data v Hadoop, najdete v části [používání MapReduce s HDInsight][hdinsight-use-mapreduce].
* toolearn o používání hello nástroje HDInsight pro Visual Studio tooanalyze data v HDInsight, najdete v části [začněte používat nástroje Visual Studio Hadoop pro HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md).

Pokud jste připravené toostart práce s vlastními daty a potřebovat další informace o tooknow způsob, jakým HDInsight ukládá data, nebo jak tooget data do HDInsight, najdete v části hello následující:

* Informace o tom, jak HDInsight používá Azure Storage, najdete v tématu [Používání Azure Storage s HDInsight](hdinsight-hadoop-use-blob-storage.md).
* Informace o tom tooupload data tooHDInsight, najdete v části [nahrát data tooHDInsight][hdinsight-upload-data].

Pokud vás zajímají další informace o vytváření a správě clusteru služby HDInsight toolearn, najdete v části hello následující:

* toolearn o správě clusteru HDInsight se systémem Linux, najdete v části [Správa clusterů HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md).
* toolearn Další informace o možnosti hello můžete vybrat při vytváření clusteru HDInsight, najdete v části [vytváření HDInsight v Linuxu pomocí vlastních možností](hdinsight-hadoop-provision-linux-clusters.md).
* Pokud jste obeznámeni s Linux a Hadoop, ale chcete tooknow podrobnosti o Hadoop na hello HDInsight, přečtěte si téma [práce s HDInsight v Linuxu](hdinsight-hadoop-linux-information.md). Tento článek obsahuje informace o:
  
  * Adresách URL služeb hostovaných na hello clusteru, například Ambari a WebHCat
  * Hello umístění souborů Hadoop a příkladech v hello místního systému souborů
  * Hello použití služby Azure Storage (WASB) namísto HDFS jako úložiště dat výchozí hello

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


