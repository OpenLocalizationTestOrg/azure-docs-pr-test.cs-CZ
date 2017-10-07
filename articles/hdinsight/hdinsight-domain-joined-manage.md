---
title: "clustery HDInsight připojený k doméně aaaManage - Azure | Microsoft Docs"
description: "Zjistěte, jak toomanage připojený k doméně clusterů HDInsight"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 6ebc4d2f-2f6a-4e1e-ab6d-af4db6b4c87c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
ms.openlocfilehash: 233ddf0953e981f9a24b77d9dde194d590e5e6d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-domain-joined-hdinsight-clusters-preview"></a>Správa clusterů HDInsight připojený k doméně (Preview)
Další informace hello uživatelů a rolí hello v doméně HDInsight a jak toomanage připojený k doméně clusterů HDInsight.

## <a name="users-of-domain-joined-hdinsight-clusters"></a>Uživatelé clustery HDInsight připojený k doméně
Cluster služby HDInsight, který není připojený k doméně má dva uživatelské účty, které jsou vytvořené během vytváření clusteru hello:

* **Správce Ambari**: Tento účet je také označován jako *Hadoop uživatele* nebo *HTTP uživatele*. Tento účet lze použít toolog na tooAmbari na https://&lt;clustername >. azurehdinsight.net. Můžete také být použité toorun dotazy na zobrazení Ambari, spusťte úloh prostřednictvím externích nástrojů (tj. prostředí PowerShell, Templeton, Visual Studio) a ověřit pomocí ovladače Hive ODBC hello a nástrojů BI (tj. aplikace Excel, PowerBI nebo Tableau).
* **Uživatel SSH**: Tento účet je možné pomocí protokolu SSH a spuštěním příkazu "sudo" příkazů. Má toohello oprávnění kořenové virtuální počítače s Linuxem.

Cluster HDInsight připojený k doméně má tři nové uživatele v přidání tooAmbari správce a SSH uživatele.

* **Správce škálu**: Tento účet je hello místní účet správce Apache škálu. Není uživatele domény služby active directory. Tento účet můžete být použité toosetup zásady a provádět další uživatelé, správci nebo delegovaní správci (tak, aby tito uživatelé mohou spravovat zásady). Ve výchozím nastavení, je uživatelské jméno hello *správce* a hello heslo je hello stejná hodnota jako hello Ambari heslo správce. Hello heslo lze aktualizovat ze stránky nastavení hello škálu.
* **Uživatel domény s oprávněními správce clusteru**: Tento účet je označen jako hello Správce clusteru Hadoop včetně Ambari a škálu uživatele domény služby active directory. Při vytváření clusteru, je třeba zadat pověření uživatele. Tento uživatel má hello následující oprávnění:

  * Připojení počítače toohello doméně a umístěte je v rámci hello organizační jednotky, které zadáte během vytváření clusteru.
  * Vytvořte objekty služby v rámci hello organizační jednotky, které zadáte během vytváření clusteru.
  * Vytvořte reverzní záznamy DNS.

    Poznámka: hello jiných uživatelů AD také mít tato oprávnění.

    Existují některé koncové body v rámci clusteru hello (například Templeton), které nejsou spravovány nástrojem škálu a proto nejsou zabezpečené. Tyto koncové body jsou uzamčené pro všechny uživatele kromě správce domény hello clusteru.
* **Regulární**: při vytváření clusteru, můžete zadat více skupin služby active directory. Hello uživatelé v těchto skupinách nebudou synchronizovaná tooRanger a Ambari. Tito uživatelé jsou uživatelé domény a budou mít přístup tooonly škálu spravované koncové body (například Hiveserver2). Všechny hello RBAC zásady a auditování bude toothese příslušné uživatele.

## <a name="roles-of-domain-joined-hdinsight-clusters"></a>Role clusterů HDInsight připojený k doméně
HDInsight připojený k doméně mají hello následující role:

* Správce clusteru
* Operátor clusteru
* Správce služeb
* Operátor služby
* Uživatel clusteru

**oprávnění hello toosee z těchto rolí**

1. Otevřete hello uživatelského rozhraní Ambari správy.  V tématu [otevřete hello uživatelského rozhraní Ambari správu](#open-the-ambari-management-ui).
2. V levé nabídce hello, klikněte na **role**.
3. Klikněte na tlačítko hello blue otazník toosee hello oprávnění:

    ![Připojené k doméně oprávnění role HDInsight](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-hello-ambari-management-ui"></a>Otevřete hello Ambari správu uživatelského rozhraní
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Otevřete v okně clusteru HDInsight. V tématu [seznamu a zobrazit clustery](hdinsight-administer-use-management-portal.md#list-and-show-clusters).
3. Klikněte na tlačítko **řídicí panel** z hlavní nabídky tooopen hello Ambari.
4. Přihlaste se tooAmbari pomocí hello clusteru správce domény uživatelské jméno a heslo.
5. Klikněte na tlačítko hello **správce** z hello pravém horním rohu a pak klikněte na rozevírací nabídku **spravovat Ambari**.

    ![Připojené k doméně HDInsight spravovat Ambari](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    Hello uživatelského rozhraní vypadá takto:

    ![Připojené k doméně HDInsight Ambari uživatelské rozhraní pro správu](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-hello-domain-users-synchronized-from-your-active-directory"></a>Seznam uživatelů domény hello synchronizované z Active Directory
1. Otevřete hello uživatelského rozhraní Ambari správy.  V tématu [otevřete hello uživatelského rozhraní Ambari správu](#open-the-ambari-management-ui).
2. V levé nabídce hello, klikněte na **uživatelé**. Zobrazí se všichni uživatelé hello synchronizované z vašeho clusteru HDInsight toohello služby Active Directory.

    ![Připojené k doméně seznamu uživatelé uživatelské rozhraní HDInsight Ambari správy](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-hello-domain-groups-synchronized-from-your-active-directory"></a>Seznam hello doménové skupiny synchronizovány ze služby Active Directory
1. Otevřete hello uživatelského rozhraní Ambari správy.  V tématu [otevřete hello uživatelského rozhraní Ambari správu](#open-the-ambari-management-ui).
2. V levé nabídce hello, klikněte na **skupiny**. Zobrazí se všechny skupiny hello synchronizované z vašeho clusteru HDInsight toohello služby Active Directory.

    ![Připojené k doméně seznam skupin pro HDInsight Ambari správu uživatelského rozhraní](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a>Konfigurace zobrazení Hive oprávnění
1. Otevřete hello uživatelského rozhraní Ambari správy.  V tématu [otevřete hello uživatelského rozhraní Ambari správu](#open-the-ambari-management-ui).
2. V levé nabídce hello, klikněte na **zobrazení**.
3. Klikněte na tlačítko **HIVE** tooshow hello podrobnosti.

    ![Připojené k doméně správy HDInsight Ambari Hive zobrazení uživatelského rozhraní](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. Klikněte na tlačítko hello **zobrazení Hive** propojit zobrazení Hive tooconfigure.
5. Projděte dolů toohello **oprávnění** části.

    ![Konfigurace oprávnění připojený k doméně správy HDInsight Ambari Hive zobrazení uživatelského rozhraní](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. Klikněte na tlačítko **přidat uživatele** nebo **přidat skupinu**a pak zadejte hello uživatele nebo skupiny, které můžete použít zobrazení Hive.

## <a name="configure-users-for-hello-roles"></a>Konfigurovat uživatele pro role hello
 toosee seznam rolí a jejich oprávnění, najdete v části [clustery HDInsight role z doméně](#roles-of-domain---joined-hdinsight-clusters).

1. Otevřete hello uživatelského rozhraní Ambari správy.  V tématu [otevřete hello uživatelského rozhraní Ambari správu](#open-the-ambari-management-ui).
2. V levé nabídce hello, klikněte na **role**.
3. Klikněte na tlačítko **přidat uživatele** nebo **přidat skupinu** tooassign uživatelů a skupin toodifferent role.

## <a name="next-steps"></a>Další kroky
* Pokud chcete konfigurovat cluster HDInsight připojený k doméně, přečtěte si téma [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md).
* Pokud chcete konfigurovat zásady Hivu a spouštět dotazy Hivu, přečtěte si téma [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](hdinsight-domain-joined-run-hive.md).
* Spuštění dotazů Hive pomocí protokolu SSH v clusterech HDInsight připojený k doméně, najdete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
