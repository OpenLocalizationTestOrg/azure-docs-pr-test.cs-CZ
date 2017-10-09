---
title: "aaaMonitor a spravovat Azure HDInsight pomocí Ambari webového uživatelského rozhraní | Microsoft Docs"
description: "Zjistěte, jak toouse Ambari toomonitor a Správa clusterů HDInsight se systémem Linux. V tomto dokumentu zjistíte, jak toouse hello webové uživatelské rozhraní Ambari součástí HDInsight clustery."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4787f3cc-a650-4dc3-9d96-a19a67aad046
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: d422c40e63345d7054839a625e115c50dad040f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hdinsight-clusters-by-using-hello-ambari-web-ui"></a>Správa clusterů HDInsight pomocí hello webové uživatelské rozhraní Ambari

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari zjednodušuje hello Správa a sledování clusteru Hadoop tím, že poskytuje snadno toouse webového uživatelského rozhraní a REST API. Ambari je zahrnutý v clusterech HDInsight se systémem Linux a je použité toomonitor hello clusteru a ujistěte se změny konfigurace.

V tomto dokumentu zjistíte, jak toouse hello webové uživatelské rozhraní Ambari pomocí clusteru služby HDInsight.

## <a id="whatis"></a>Co je Ambari?

[Apache Ambari](http://ambari.apache.org) zjednodušuje správu Hadoop, tím, že poskytuje snadno použitelné webového uživatelského rozhraní. Můžete použít Ambari vytvářet, spravovat a monitorovat clusterů systému Hadoop. Vývojářům můžete integrovat tyto funkce do svých aplikací s použitím hello [Ambari REST API](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Hello webové uživatelské rozhraní Ambari je dostupné ve výchozím nastavení s clustery HDInsight, které používají operační systém Linux hello.

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). 

## <a name="connectivity"></a>Připojení

Hello webové uživatelské rozhraní Ambari je k dispozici v clusteru HDInsight na HTTPS://CLUSTERNAME.azurehdidnsight.net, kde **CLUSTERNAME** je hello názvem vašeho clusteru.

> [!IMPORTANT]
> Připojení tooAmbari v HDInsight vyžaduje protokol HTTPS. Po zobrazení výzvy k ověření, použijte název účtu správce hello a heslo, které jste zadali při vytvoření clusteru hello.

## <a name="ssh-tunnel-proxy"></a>Tunelové propojení SSH (proxy)

Při Ambari pro váš cluster je přístupný přímo prostřednictvím hello Internet, hello některé odkazy z hello webové uživatelské rozhraní Ambari (například toohello JobTracker) se nezobrazí na Internetu. tooaccess těchto služeb, je nutné vytvořit tunelového propojení SSH. Další informace najdete v tématu [používání tunelového propojení SSH s HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="ambari-web-ui"></a>Webovému uživatelskému rozhraní Ambari

Při připojování toohello webové uživatelské rozhraní Ambari, jste výzvami tooauthenticate toohello stránky. Použijte hello uživatel s oprávněními správce clusteru (výchozí správce) a heslo použité při vytváření clusteru.

Po otevření stránky hello Poznámka: hello panelu v horní části hello. Tento panel obsahuje hello následující informace a ovládací prvky:

![ambari nav](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

* **Ambari logo** – řídicí panel hello otevře, který lze použít toomonitor hello clusteru.

* **Název ops # clusteru** -zobrazí hello počet probíhajících operacích Ambari. Výběr názvu clusteru hello nebo **# ops** zobrazí seznam operace na pozadí.

* **výstrahy #** -zobrazí varování nebo kritické výstrahy, pokud existuje, které pro hello cluster.

* **Řídicí panel** -zobrazí hello řídicího panelu.

* **Služby** – informace a konfigurace nastavení pro hello služby v clusteru hello.

* **Hostitelé** – informace a konfigurace nastavení pro hello uzly v clusteru hello.

* **Výstrahy** -protokolu informace, upozornění a kritické výstrahy.

* **Správce** -softwaru zásobníku nebo služby, které jsou nainstalované na hello clusteru, informace o účtu služby a zabezpečení protokolu Kerberos.

* **Tlačítko Správce** -Ambari správy, uživatelská nastavení a odhlášení.

## <a name="monitoring"></a>Monitorování

### <a name="alerts"></a>Výstrahy

Hello následující seznam obsahuje společné výstrahy stavy hello používá Ambari:

* **OK**
* **Upozornění**
* **KRITICKÉ**
* **NEZNÁMÝ**

Výstrahy jiné než **OK** způsobit hello **# výstrahy** položku v horní části hello hello stránky toodisplay hello počtu výstrah. Výběrem této položky zobrazí hello výstrahy a jejich stav.

Výstrahy jsou uspořádány do několik výchozích skupin, které lze zobrazit z hello **výstrahy** stránky.

![stránka výstrah](./media/hdinsight-hadoop-manage-ambari/alerts.png)

Hello skupin můžete spravovat pomocí hello **akce** nabídky a výběrem **Spravovat výstrahy skupiny**.

![Dialogové okno upozornění skupin spravovat](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

Můžete také spravovat výstrahy metody a vytvořit oznámení o výstrahách z hello **akce** nabídky výběrem __Spravovat výstrahy oznámení__. Zobrazí se všechny aktuální oznámení. Odsud můžete také vytvořit oznámení. Může být zasláno oznámení **e-MAILU** nebo **SNMP** dojít, když konkrétní závažnost výstrahy nebo kombinace. Například můžete odesílat e-mailové zprávy při jejich hello výstrahy v hello **YARN výchozí** skupiny je nastaven příliš**kritický**.

![Vytvoření dialogového okna výstrah](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

Nakonec výběr __spravovat nastavení upozornění__ z hello __akce__ nabídky vám umožní tooset hello počet opakování výstrahy se musí vyskytovat předtím, než odešle oznámení. Toto nastavení může být použité tooprevent oznámení pro přechodné chyby.

### <a name="cluster"></a>Cluster

Hello **metriky** karta hello řídicí panel obsahuje řadu pomůcek, které umožňují snadno toomonitor hello stav clusteru na první pohled. Několik widgetů, například **využití procesoru**, poskytují další informace, při kliknutí na.

![řídicí panel se metriky](./media/hdinsight-hadoop-manage-ambari/metrics.png)

Hello **Heatmaps** karta zobrazuje jako barevnou heatmaps, budete z zelená toored metriky.

![řídicí panel se heatmaps](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

Další informace o hello uzlů v clusteru hello vyberte **hostitele**. Pak vyberte hello konkrétním uzlu, které vás zajímají.

![Podrobnosti o hostiteli](./media/hdinsight-hadoop-manage-ambari/host-details.png)

### <a name="services"></a>Služby

Hello **služby** bočním panelu na hello řídicí panel poskytuje rychlý přehled o stavu hello hello služeb spuštěných v clusteru hello. Jsou různé ikony používané tooindicate stavu nebo akce, které by měla být provedena. Například symbol žlutý recyklaci se zobrazí, pokud služba potřebuje toobe recyklují.

![straně řádku služeb](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

> [!NOTE]
> Hello služeb, zobrazí se liší mezi typy clusteru HDInsight a verze. Hello služby zobrazí tady může být jiný než services hello zobrazené u vašeho clusteru.

Výběr služby zobrazí podrobnější informace ve službě hello.

![souhrnné informace o služby](./media/hdinsight-hadoop-manage-ambari/service-details.png)

#### <a name="quick-links"></a>Rychlé odkazy

Zobrazení některé služby **rychlé odkazy** odkaz hello horní části stránky hello. To může být použité tooaccess specifickou pro službu web uživatelská rozhraní, jako například:

* **Historie úlohy** -historie úlohy MapReduce.
* **Správce prostředků** -YARN ResourceManager uživatelského rozhraní.
* **NameNode** -Hadoop Distributed soubor System (HDFS) NameNode uživatelského rozhraní.
* **Uživatelské rozhraní webu Oozie** -Oozie uživatelského rozhraní.

V prohlížeči, který zobrazí stránku hello vybrané výběrem libovolné z těchto odkazů otevře novou kartu.

> [!NOTE]
> Výběr hello **rychlé odkazy** záznam pro službu může vrátit chyba "server nebyl nalezen". Pokud dojde k této chybě, musíte použít tunelového propojení SSH při použití hello **rychlé odkazy** položka pro tuto službu. Informace najdete v tématu [používání tunelového propojení SSH s HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)

## <a name="management"></a>Správa

### <a name="ambari-users-groups-and-permissions"></a>Ambari uživatele, skupiny a oprávnění

Práce se uživatelé, skupiny a oprávnění jsou podporovány při použití [připojený k doméně](hdinsight-domain-joined-introduction.md) clusteru HDInsight. Informace o používání hello uživatelského rozhraní Ambari správy na cluster připojený k doméně, najdete v části [Správa clusterů HDInsight připojený k doméně](hdinsight-domain-joined-introduction.md).

> [!WARNING]
> Neměňte heslo hello sledovací hello Ambari zařízení (hdinsightwatchdog) v clusteru HDInsight se systémem Linux. Změna hesla zalomení hello hello možnost toouse skript akce nebo provádět operace škálování k vašemu clusteru.

### <a name="hosts"></a>Hostitelé

Hello **hostitele** stránka obsahuje seznam všech hostitelích v clusteru hello. toomanage hostitele, postupujte podle těchto kroků.

![stránka hostitele](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [!NOTE]
> Přidání, vyřazení z provozu a recommissioning hostitele nepoužívejte s clustery HDInsight.

1. Vyberte hostitele hello chcete toomanage.

2. Použití hello **akce** nabídky tooselect hello akce chcete tooperform:

   * **Spustit všechny součásti** – spustit všechny součásti hello hostiteli.

   * **Zastavit všechny součásti** -ukončit všechny součásti na hostiteli hello.

   * **Restartujte všechny součásti** – zastavení a spuštění všech součástí na hostiteli hello.

   * **Zapnout režim údržby** -potlačí výstrah pro hostitele hello. Tento režim by měl povolit, pokud jsou provádění akcí, které generují výstrahy. Například ukončení a opětovném spuštění služby.

   * **Zapnutí režimu údržby** – vrátí text hello hostitele toonormal výstrahy.

   * **Zastavit** -zastaví DataNode nebo NodeManagers na hostiteli hello.

   * **Spustit** -spustí DataNode nebo NodeManagers na hostiteli hello.

   * **Restartujte** -zastaví a spustí DataNode nebo NodeManagers na hostiteli hello.

   * **Vyřadit z provozu** – odebere hostitele z clusteru hello.

     > [!NOTE]
     > Nepoužívejte tuto akci v clusterech prostředí HDInsight.

   * **Recommission** -přidá toohello dříve vyřazení hostitelského clusteru.

     > [!NOTE]
     > Nepoužívejte tuto akci v clusterech prostředí HDInsight.

### <a id="service"></a>Služby

Z hello **řídicí panel** nebo **služby** stránky, použijte hello **akce** tlačítko dole hello hello seznam služeb toostop a spuštění všech služeb.

![Akce služby](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

> [!WARNING]
> Při **přidat službu** je uveden v této nabídce, neměl by být clusteru HDInsight toohello tooadd používaných služeb. Nové služby musí být přidaní pomocí akce skriptu při zřizování clusteru. Další informace o použití akce skriptu najdete v tématu [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).

Při hello **akce** tlačítko můžete restartovat všechny služby, často se má toostart, zastavit nebo restartovat určitou službu. Použijte následující postup tooperform akce na jednotlivé služby hello:

1. Z hello **řídicí panel** nebo **služby** vyberte možnost služby.

2. Z hello horní části hello **Souhrn** pomocí hello **služby akce** tlačítkem a vyberte hello tootake akce. Tím dojde k restartování služby hello na všech uzlech.

    ![Akce služby](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

   > [!NOTE]
   > Restartování některé služby, když běží hello clusteru může generovat výstrahy. tooavoid výstrahy, můžete použít hello **služby akce** tlačítko tooenable **režimu údržby** služby hello před provedením hello restartování.

3. Jakmile akci byla vybrána, hello **# op** položka hello horní části hello stránky přírůstcích tooshow, ke kterému dochází operaci na pozadí. Nakonfigurované toodisplay, zobrazí se seznam hello operace na pozadí.

   > [!NOTE]
   > Pokud jste povolili **režimu údržby** hello služby, mějte na paměti, toodisable ho pomocí hello **služby akce** tlačítko po dokončení operace hello.

tooconfigure služby, použijte hello následující kroky:

1. Z hello **řídicí panel** nebo **služby** vyberte možnost služby.

2. Vyberte hello **konfigurací** se zobrazí kartu hello aktuální konfiguraci. Zobrazí se také seznam předchozí konfigurace.

    ![Konfigurace](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. Použití hello pole zobrazí toomodify hello konfigurace a pak vyberte **Uložit**. Nebo vyberte předchozí konfiguraci a pak vyberte **změnit na aktuální** tooroll zpět toohello předchozí nastavení.

## <a name="ambari-views"></a>Zobrazení Ambari

Zobrazení Ambari umožňují vývojářům prvky uživatelského rozhraní tooplug do hello webové uživatelské rozhraní Ambari pomocí hello [Framework zobrazení Ambari](https://cwiki.apache.org/confluence/display/AMBARI/Views). HDInsight poskytuje hello následující zobrazení s typy clusteru Hadoop:

* Správce front yarn: správce front hello poskytuje jednoduché uživatelského rozhraní pro zobrazení a úprava fronty YARN.

* Zobrazení Hive: hello zobrazení Hive můžete toorun dotazů Hive přímo z webového prohlížeče. Můžete uložit dotazy, zobrazení výsledků, uložte výsledky úložiště clusteru toohello nebo stáhnout výsledky tooyour místní systém. Další informace o používání Hive zobrazení najdete v tématu [použijte zobrazení Hive s HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).

* Zobrazení tez: hello Tez zobrazení můžete toobetter pochopit a optimalizovat úlohy. Můžete zobrazit informace na tom, jak jsou vykonány úlohách Tez a jaké prostředky jsou používány.
