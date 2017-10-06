---
title: "aaaAccess aplikací Hadoop YARN protokoly prostřednictvím kódu programu - Azure | Microsoft Docs"
description: "Přístup k aplikaci prostřednictvím kódu programu přihlásí clusteru Hadoop v HDInsight."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0198d6c9-7767-4682-bd34-42838cf48fc5
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 064efee1ea6a864c29ab897692ead0152c926c0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="access-yarn-application-logs-on-windows-based-hdinsight"></a>Protokoly YARN aplikace přístup na HDInsight se systémem Windows
Toto téma vysvětluje, jak tooaccess hello protokoly YARN (ještě jiný prostředek Vyjednavač) aplikace, které dokončily na cluster Hadoop založených na systému Windows v Azure HDInsight

> [!IMPORTANT]
> Hello informace v tomto dokumentu se vztahuje pouze na základě tooWindows clusterů HDInsight. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Informace o přístupu k YARN přihlásí clustery HDInsight se systémem Linux, najdete v části [YARN přístupu aplikace přihlásí systémem Linux Hadoop v HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md)
>


### <a name="prerequisites"></a>Požadavky
* Cluster HDInsight se systémem Windows.  V tématu [založené na Windows vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="yarn-timeline-server"></a>YARN časová osa serveru
Hello <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">YARN časová osa serveru</a> poskytuje obecné informace o dokončené aplikace také jako framework informace specifické pro aplikaci prostřednictvím dvou různých rozhraní. Zejména:

* Ukládání a načítání informací o obecná aplikace v clusterech prostředí HDInsight byl povolený s verzí 3.1.1.374 nebo vyšší.
* komponenta informace o konkrétní rozhraní aplikace Hello hello časová osa Server není aktuálně k dispozici v clusterech prostředí HDInsight.

Obecné informace o aplikacích v zahrnuje hello následující řazení dat:

* ID aplikace Hello, jedinečný identifikátor aplikace
* Hello uživatele, který spustil aplikace hello
* Informace o pokusy provedené toocomplete hello aplikace
* kontejnery Hello používá jakýkoliv pokus o dané aplikaci

V clusterech služby HDInsight se uloží tyto informace pomocí Azure Resource Manager tooa historie úložiště v hello výchozí kontejner výchozí účet úložiště Azure. Tato obecná data na dokončené aplikace se dají získat pomocí rozhraní REST API:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>Protokoly YARN aplikací a
YARN podporuje více programovacích modelů (MapReduce právě jeden z nich) tím, že odpojí Správa prostředků z plánování/monitorování aplikací. To se provádí prostřednictvím globální konfiguraci *ResourceManager* (RM) uzlu na pracovním *NodeManagers* (NMs) a každou aplikaci *ApplicationMasters* (AMs). Hello každou aplikaci AM vyjedná prostředků (procesoru, paměti, disku, sítě) pro spuštění aplikace s hello RM. Hello RM funguje s NMs toogrant těchto prostředků, které jsou poskytovány jako *kontejnery*. Hello AM zodpovídá za trasování hello průběh tooit hello kontejnery, které jsou přiřazené podle hello RM. Aplikace může vyžadovat mnoho kontejnerů v závislosti na povaze hello aplikace hello.

Kromě toho každá aplikace může obsahovat více *pokusy o aplikace* v pořadí toofinish v přítomnosti hello havárií nebo z důvodu ztráty toohello komunikace mezi AM a RM. Proto jsou kontejnery udělena tooa konkrétní pokus aplikace. V tom smyslu kontejner zajišťuje hello kontext pro základní jednotkou úlohy prováděné YARN aplikace a všechny práci, kterou se provádí v kontextu hello kontejneru se provádí na hello jednoho pracovního uzlu, na které hello byl přidělen kontejneru. V tématu [YARN koncepty] [ YARN-concepts] pro odkaz na další.

Logs (protokoly aplikací a hello přidružené kontejneru) jsou kritické v ladění problematické aplikací Hadoop. YARN poskytuje dobrý rozhraní pro shromažďování, agregace a ukládání protokoly aplikací s hello [protokolu agregace] [ log-aggregation] funkce. Hello protokolu agregace díky funkci přístupem protokoly aplikací determinističtější, jak slučuje protokolů v rámci všech kontejnerů v pracovním uzlu a ukládá je jako jeden agregované soubor protokolu pro pracovního uzlu na výchozí systém souborů hello po dokončení aplikace. Vaše aplikace může používat stovkami nebo tisíci kontejnery, ale protokoly pro všechny kontejnery spustit na jednom pracovního uzlu bude vždy agregované tooa jedním souborem, výsledkem je jeden soubor protokolu pro pracovního uzlu používá vaše aplikace. Agregace protokolu je ve výchozím nastavení v clusterech HDInsight povolené (verze 3.0 a novější), a agregované protokoly lze najít v kontejneru výchozí hello clusteru na hello následující umístění:

    wasb:///app-logs/<user>/logs/<applicationId>

V tomto umístění *uživatele* je název hello hello uživatele, který spustil hello aplikace a *applicationId* je jedinečný identifikátor hello aplikace přiřazené službou hello YARN RM.

Hello agregované protokoly nejsou přímo čitelný, jako jsou zapsány [TFile][T-file], [binární formát] [ binary-format] indexovat pomocí kontejneru. YARN poskytuje rozhraní příkazového řádku nástroje toodump tyto protokoly jako prostý text, pro aplikace nebo kontejnerů, které vás zajímají. Tyto protokoly můžete zobrazit jako prostý text spuštěním jednoho z následujících příkazů YARN přímo na uzlech clusteru hello (po tooit připojují pomocí protokolu RDP) hello:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>YARN ResourceManager uživatelského rozhraní
Hello uživatelském rozhraní YARN ResourceManager běží na clusteru headnode hello a je přístupný prostřednictvím hello řídicí panel portálu Azure:

1. Přihlaste se příliš[portál Azure](https://portal.azure.com/).
2. V levé nabídce hello, klikněte na tlačítko **Procházet**, klikněte na tlačítko **clustery HDInsight**, klikněte na cluster systému Windows, které chcete tooaccess hello protokoly YARN aplikací.
3. V horní nabídce hello, klikněte na tlačítko **řídicí panel**. Zobrazí se stránka, na nové prohlížeči otevřít kartu názvem **HDInsight dotazu konzoly**.
4. Z **HDInsight dotazu konzoly**, klikněte na tlačítko **uživatelském rozhraní Yarn**.

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
