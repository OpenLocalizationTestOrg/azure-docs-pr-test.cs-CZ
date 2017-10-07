---
title: "aaaConfigure Hive zásad v doméně HDInsight - Azure | Microsoft Docs"
description: "Zjistěte..."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3fade1e5-c2e1-4ad5-b371-f95caea23f6d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
ms.openlocfilehash: 56f2bf9d872abc5f772b886fcf91c2e2422092f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hive-policies-in-domain-joined-hdinsight-preview"></a>Konfigurace zásad Hivu ve službě HDInsight připojené k doméně (Preview)
Zjistěte, jak zásady Apache škálu tooconfigure pro Hive. V tomto článku vytvoříte dva škálu zásady toorestrict přístup toohello hivesampletable. Hello hivesampletable se dodává s clustery HDInsight. Po nakonfigurování zásady hello použijete aplikace Excel a rozhraní ODBC ovladač tooconnect tooHive tabulky v HDInsight.

## <a name="prerequisites"></a>Požadavky
* Cluster HDInsight připojený k doméně. Viz [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md).
* Pracovní stanice s Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone nebo Office 2010 Professional Plus.

## <a name="connect-tooapache-ranger-admin-ui"></a>Připojit tooApache škálu správce uživatelského rozhraní
**tooconnect tooRanger uživatelského rozhraní správce**

1. V prohlížeči připojte tooRanger uživatelského rozhraní správce. Adresa URL Hello je https://&lt;ClusterName >.azurehdinsight.net/Ranger/.

   > [!NOTE]
   > Ranger používá jiné přihlašovací údaje než cluster Hadoop. tooprevent prohlížeče pomocí pověření uložených v mezipaměti Hadoop, použijte nové prohlížeče inprivate okno tooconnect toohello škálu správce uživatelského rozhraní.
   >
   >
2. Přihlaste se pomocí hello clusteru správce domény uživatelské jméno a heslo:

    ![Domovská stránka Ranger služby HDInsight připojené k doméně](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-ranger-home-page.png)

    V současné době Ranger funguje pouze s Yarn a Hivem.

## <a name="create-domain-users"></a>Vytvoření uživatelů domén
V tématu [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad) jste vytvořili uživatele hiveuser1 a hiveuser2. V tomto kurzu použijete hello dvě uživatelský účet.

## <a name="create-ranger-policies"></a>Vytvoření zásad Ranger
V této části vytvoříte dvě zásady Ranger pro přistupování k hivesampletable. Udělíte oprávnění Vybrat na různé sady sloupců. Oba uživatelé byli vytvořeni v tématu [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).  V další části hello budete testovat hello dvě zásady v aplikaci Excel.

**toocreate škálu zásady**

1. Otevřete uživatelské rozhraní správce Ranger. V tématu [připojit tooApache uživatelského rozhraní správce škálu](#connect-to-apache-ranager-admin-ui).
2. V části **Hive** klikněte na **&lt;název_clusteru>_hive**. Měly by se zobrazit dvě předem nakonfigurované zásady.
3. Klikněte na tlačítko **přidat nové zásady**a pak zadejte hello následující hodnoty:

   * Policy name (Název zásady): read-hivesampletable-all
   * Hive Database (Databáze Hivu): default (výchozí)
   * table (tabulka): hivesampletable
   * Hive column (Sloupec Hivu):*
   * Select User (Vybrat uživatele): hiveuser1
   * Permissions (Oprávnění): select (vybrat)

     ![Konfigurace zásady Hivu v Ranger služby HDInsight připojené k doméně](./media/hdinsight-domain-joined-run-hive/hdinsight-domain-joined-configure-ranger-policy.png).

     > [!NOTE]
     > Pokud uživatel domény není vyplněný v vyberte uživatele, počkejte několik minut pro škálu toosync v AAD.
     >
     >
4. Klikněte na tlačítko **přidat** toosave hello zásad.
5. Zopakujte poslední dva kroky toocreate hello jiné zásady s hello následující vlastnosti:

   * Policy name (Název zásady): read-hivesampletable-devicemake
   * Hive Database (Databáze Hivu): default (výchozí)
   * table (tabulka): hivesampletable
   * Hive column (Sloupec Hivu): clientid, devicemake
   * Select User (Vybrat uživatele): hiveuser2
   * Permissions (Oprávnění): select (vybrat)

## <a name="create-hive-odbc-data-source"></a>Vytvoření zdroje dat Hive ODBC
Hello pokyny naleznete v [zdroje dat ODBC Hive vytvořit](hdinsight-connect-excel-hive-odbc-driver.md).  

    Vlastnost|Popis
    ---|---
    Název zdroje dat|Zadejte název zdroje dat tooyour
    Hostitel|Zadejte &lt;název_clusteru_HDInsight>.azurehdinsight.net. Například mujHDICluster.azurehdinsight.net.
    Port|Použijte <strong>443</strong>. (Tento port se změnil z 563 too443.)
    Databáze|Použijte <strong>Výchozí</strong>.
    Typ serveru Hive|Vyberte <strong>Hive Server 2</strong>.
    Mechanismus|Vyberte <strong>Služba Azure HDInsight</strong>
    Cesta HTTP|Ponechte prázdné.
    Uživatelské jméno|Zadejte hiveuser1@contoso158.onmicrosoft.com. Aktualizujte název domény hello, pokud se liší.
    Heslo|Zadejte heslo hello hiveuser1.
    </table>

Ujistěte se, že tooclick **Test** před uložením zdroj dat hello.

## <a name="import-data-into-excel-from-hdinsight"></a>Import dat do Excelu ze služby HDInsight
V poslední části hello jste nakonfigurovali dvě zásady.  hiveuser1 má hello vyberte oprávnění pro všechny sloupce hello a hiveuser2 má hello vyberte oprávnění na dva sloupce. V této části zosobnit hello dva uživatelé tooimport dat do aplikace Excel.

1. V Excelu otevřete nový nebo existující sešit.
2. Z hello **Data** , klikněte na **z jiných zdrojů dat**a potom klikněte na **z Průvodce datovým připojením** toolaunch hello **Průvodce datovým připojením**.

    ![Otevřete Průvodce připojením dat][img-hdi-simbahiveodbc.excel.dataconnection]
3. Vyberte **název DSN rozhraní ODBC** jako zdroj dat hello a pak klikněte na tlačítko **Další**.
4. Ze zdroje dat ODBC, název, který jste vytvořili v předchozím kroku hello zdroje dat vyberte hello a pak klikněte na **Další**.
5. Znovu zadejte heslo hello hello clusteru v Průvodci hello a pak klikněte na tlačítko **OK**. Počkejte hello **vybrat databázi a tabulku** tooopen dialogové okno. Může to trvat několik sekund.
6. Vyberte **hivesampletable** a pak klikněte na **Další**.
7. Klikněte na **Dokončit**.
8. V hello **importovat Data** dialogové okno, můžete změnit nebo zadejte dotaz hello. Ano, klikněte na tlačítko toodo **vlastnosti**. Může to trvat několik sekund.
9. Klikněte na tlačítko hello **definice** text příkazu kartě hello je:

       SELECT * FROM "HIVE"."default"."hivesampletable"

   Hiveuser1 zásadami hello škálu definovaných, má oprávnění select pro všechny sloupce hello.  Takže tento dotaz funguje s přihlašovacími údaji uživatele hiveuser1, ale nefunguje s přihlašovacími údaji uživatele hiveuser2.

   ![Vlastnosti připojení][img-hdi-simbahiveodbc-excel-connectionproperties]
10. Klikněte na tlačítko **OK** dialogové okno Vlastnosti připojení tooclose hello.
11. Klikněte na tlačítko **OK** tooclose hello **importovat Data** dialogové okno.  
12. Zadejte znovu heslo hello hiveuser1 a potom klikněte na **OK**. Jak dlouho trvá několik sekund, než získá importované tooExcel data. Po dokončení importu byste měli vidět 11 sloupců dat.

Druhá zásada tootest hello (čtení devicemake hivesampletable), který jste vytvořili v poslední části hello

1. Přidejte v Excelu nový list.
2. Postupujte podle hello poslední postup tooimport hello data.  Hello pouze změny, které provedete je toouse hiveuser2 pověření místo hiveuser1 společnosti. To se nezdaří, protože hiveuser2 má pouze oprávnění toosee dva sloupce. Získáte hello následující chybě:

        [Microsoft][HiveODBC] (35) Error from Hive: error code: '40000' error message: 'Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hiveuser2] does not have [SELECT] privilege on [default/hivesampletable/clientid,country ...]'.
3. Postupujte podle hello stejný postup tooimport data. Tentokrát použijte přihlašovací údaje pro hiveuser2 a také upravit příkaz select hello z:

        SELECT * FROM "HIVE"."default"."hivesampletable"

    na:

        SELECT clientid, devicemake FROM "HIVE"."default"."hivesampletable"

    Po dokončení importu byste měli vidět naimportované dva sloupce dat.

## <a name="next-steps"></a>Další kroky
* Pokud chcete konfigurovat cluster HDInsight připojený k doméně, přečtěte si téma [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md).
* Pokud chcete spravovat clustery HDInsight připojené k doméně, přečtěte si téma [Správa clusterů HDInsight připojených k doméně](hdinsight-domain-joined-manage.md).
* Spuštění dotazů Hive pomocí protokolu SSH v clusterech HDInsight připojený k doméně, najdete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
* Připojení Hive pomocí Hive JDBC, najdete v části [připojit tooHive v Azure HDInsight pomocí ovladače Hive JDBC hello](hdinsight-connect-hive-jdbc-driver.md)
* Připojování tooHadoop aplikace Excel pomocí Hive ODBC, najdete v části [tooHadoop připojení aplikace Excel s hello jednotky Microsoft Hive ODBC](hdinsight-connect-excel-hive-odbc-driver.md)
* Připojování tooHadoop aplikace Excel pomocí doplňku Power Query, najdete v části [tooHadoop připojení aplikace Excel pomocí doplňku Power Query](hdinsight-connect-excel-power-query.md)
