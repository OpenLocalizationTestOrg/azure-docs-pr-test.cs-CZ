---
title: "aaaAccess aplikací Hadoop YARN protokolů v HDInsight se systémem Linux - Azure | Microsoft Docs"
description: "Zjistěte, jak aplikace YARN tooaccess přihlášení systémem Linux HDInsight (Hadoop) clusteru pomocí příkazového řádku hello i webový prohlížeč."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 3ec08d20-4f19-4a8e-ac86-639c04d2f12e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 0bab356e3b97114abbb05712c8e7b21a194f2508
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="access-yarn-application-logs-on-linux-based-hdinsight"></a>Protokoly YARN aplikace přístup na HDInsight se systémem Linux

Zjistěte, jak tooaccess hello protokoly YARN (ještě jiný prostředek Vyjednavač) aplikací na clusteru Hadoop v prostředí Azure HDInsight.

> [!IMPORTANT]
> Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="YARNTimelineServer"></a>YARN časová osa serveru

Hello [YARN časová osa serveru](http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) poskytuje obecné informace o dokončené aplikace a informace o konkrétní rozhraní aplikaci prostřednictvím dvou různých rozhraní. Zejména:

* Ukládání a načítání informací o obecná aplikace v clusterech prostředí HDInsight byl povolený s verzí 3.1.1.374 nebo vyšší.
* komponenta informace o konkrétní rozhraní aplikace Hello hello časová osa Server není aktuálně k dispozici v clusterech prostředí HDInsight.

Obecné informace o aplikacích v zahrnuje hello následující datový typ:

* ID aplikace Hello, jedinečný identifikátor aplikace
* Hello uživatele, který spustil aplikace hello
* Informace o pokusy provedené toocomplete hello aplikace
* kontejnery Hello používá jakýkoliv pokus o dané aplikaci

## <a name="YARNAppsAndLogs"></a>Protokoly YARN aplikací a

YARN podporuje více programovacích modelů (MapReduce právě jeden z nich) tím, že odpojí Správa prostředků z plánování/monitorování aplikací. YARN používá globální konfiguraci *ResourceManager* (RM) uzlu na pracovním *NodeManagers* (NMs) a každou aplikaci *ApplicationMasters* (AMs). Hello každou aplikaci AM vyjedná prostředků (procesoru, paměti, disku, sítě) pro spuštění aplikace s hello RM. Hello RM funguje s NMs toogrant těchto prostředků, které jsou poskytovány jako *kontejnery*. Hello AM zodpovídá za trasování hello průběh tooit hello kontejnery, které jsou přiřazené podle hello RM. Aplikace může vyžadovat mnoho kontejnerů v závislosti na povaze hello aplikace hello.

Každá aplikace může obsahovat více *pokusy o aplikace*. Pokud aplikace selže, může pokus jako nový pokus o. Každý pokus o spuštění v kontejneru. V tom smyslu poskytuje kontejner hello kontext pro základní jednotkou úlohy prováděné YARN aplikace. Všechny práci, kterou se provádí v kontextu hello kontejneru se provádí na hello jednoho pracovního uzlu, na které hello byl přidělen kontejneru. V tématu [YARN koncepty] [ YARN-concepts] pro odkaz na další.

Logs (protokoly aplikací a hello přidružené kontejneru) jsou kritické v ladění problematické aplikací Hadoop. YARN poskytuje dobrý rozhraní pro shromažďování, agregace a ukládání protokoly aplikací s hello [protokolu agregace] [ log-aggregation] funkce. Díky funkci agregace protokolu Hello přístupem protokoly aplikací více deterministický. Ho slučuje protokolů v rámci všech kontejnerů v pracovním uzlu a ukládá je jako jeden soubor protokolu agregované pro pracovního uzlu. Hello protokolu jsou uloženy na výchozí systém souborů hello po dokončení aplikace. Vaše aplikace může používat stovkami nebo tisíci kontejnery, ale protokoly pro všechny kontejnery spustit na jednom pracovního uzlu jsou vždy agregované tooa jedním souborem. Proto není pouze 1 protokolu za pracovního uzlu používá vaše aplikace. Agregace protokolu je povoleno ve výchozím nastavení v clusterech HDInsight verze 3.0 nebo novějším. Agregované protokoly jsou umístěny ve výchozím nastavení úložiště pro hello cluster. Následující cesta Hello je hello HDFS cesta toohello protokoly:

    /app-logs/<user>/logs/<applicationId>

V cestě hello `user` je název hello hello uživatele, který spustil aplikace hello. Hello `applicationId` je hello jedinečný identifikátor přiřazený tooan aplikace hello YARN RM.

Hello agregované protokoly nejsou přímo čitelný, jako jsou zapsány [TFile][T-file], [binární formát] [ binary-format] indexovat pomocí kontejneru. Hello protokoly YARN ResourceManager nebo rozhraní příkazového řádku nástroje tooview tyto protokoly použijte jako prostý text pro aplikace nebo kontejnerů, které vás zajímají.

## <a name="yarn-cli-tools"></a>YARN rozhraní příkazového řádku nástroje

nástroje příkazového řádku YARN hello toouse, nejprve je nutné připojit toohello clusteru HDInsight pomocí SSH. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

Tyto protokoly můžete zobrazit jako prostý text spuštěním jednoho z hello následující příkazy:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>

Zadejte hello &lt;applicationId >, &lt;uživatel kdo spuštění--application >, &lt;identifikátor containerId >, a &lt;adresa pracovní node > informace při spuštění těchto příkazů.

## <a name="yarn-resourcemanager-ui"></a>YARN ResourceManager uživatelského rozhraní

Hello uživatelském rozhraní YARN ResourceManager běží na clusteru headnode hello. Je přístupný prostřednictvím webovému uživatelskému rozhraní Ambari hello. Následující kroky tooview hello YARN hello použití protokolů:

1. Ve webovém prohlížeči přejděte toohttps://CLUSTERNAME.azurehdinsight.net. Nahraďte název clusteru s názvem hello clusteru HDInsight.
2. Hello seznamu služeb na levé straně hello vyberte **YARN**.

    ![Yarn služby vybrané](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnservice.png)
3. Z hello **rychlé odkazy** rozevíracího seznamu, vyberte jeden z hlavních uzlech clusteru hello a pak vyberte **ResourceManager protokolu**.

    ![Yarn rychlé odkazy](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnquicklinks.png)

    Zobrazí seznam protokolů tooYARN odkazy.

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
