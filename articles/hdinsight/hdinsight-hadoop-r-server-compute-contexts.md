---
title: "aaaCompute kontextu možnosti R serverem v HDInsight - Azure | Microsoft Docs"
description: "Další informace o hello různými výpočetními kontextu možnosti k dispozici toousers s R serverem v HDInsight"
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0deb0b1c-4094-459b-94fc-ec9b774c1f8a
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: b3b0d0cc3caa390797dcff8c73d66cd3ad78bcaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="compute-context-options-for-r-server-on-hdinsight"></a>Výpočetní kontextu možnosti pro R Server v HDInsight

Microsoft R serverem v Azure HDInsight řídí, jak jsou volání provedený nastavení hello výpočetní kontextu. Tento článek popisuje hello možnosti, které jsou k dispozici toospecify zda a jak je paralelizovaná spuštění několika málo mezi jader hello hraniční uzel nebo clusteru HDInsight.

Hello hraniční uzel clusteru poskytuje vhodné místo tooconnect toohello clusteru a toorun skripty R. Hraniční uzel máte hello možnost spuštěné hello paralelizovaná málo distribuované funkce ScaleR mezi hello jader hello hraniční uzel serveru. Můžete také spustit je mezi uzly hello hello clusteru pomocí Hadoop mapy snížit nebo Spark na ScaleR výpočetní kontexty.

## <a name="microsoft-r-server-on-azure-hdinsight"></a>Microsoft R serverem v Azure HDInsight
[Microsoft R serverem v Azure HDInsight](hdinsight-hadoop-r-server-overview.md) nabízí nejnovější funkce hello R na základě analýzy. Může používat data, která je uložená v kontejneru HDFS ve vaší [objektů Blob v Azure](../storage/common/storage-introduction.md "úložiště objektů Azure Blob") účet úložiště, Data Lake store nebo hello místní systém Linux souborů. Vzhledem k tomu, že R Server je založený na R s otevřeným zdrojem, můžete použít hello R aplikacím, které vytvoříte, žádný z hello 8000 + R s otevřeným zdrojem balíčků. Může také používat rutiny hello v [RevoScaleR](https://msdn.microsoft.com/microsoft-r/scaler/scaler), balíček analýzy velkých objemů dat společnosti Microsoft, který je součástí R Server.  

## <a name="compute-contexts-for-an-edge-node"></a>Výpočetní kontexty pro hraniční uzel
Obecně platí R skript, který běží v R Server na uzlu edge hello běží v rámci překladač hello R v tomto uzlu. Hello výjimky jsou tyto kroky, které volají funkce ScaleR. volání ScaleR Hello spustit ve výpočetním prostředí, který je určen podle nastavení hello ScaleR výpočetní kontextu.  Když spustíte R skript z hraniční uzel, hello možných hodnot hello výpočetní kontextu jsou:

- místní sekvenční (*'local'*)
- místní paralelní (*'localpar'*)
- Snižte mapy
- Spark

Hello *'local'* a *'localpar'* možnosti se liší pouze v tom **rxExec** provedení volání. Obě provést další volání funkce rx paralelní způsobem mezi všechny dostupné jader není uvedeno jinak prostřednictvím použití hello ScaleR **numCoresToUse** volby, například `rxOptions(numCoresToUse=6)`. Paralelní provádění možnosti nabízejí optimální výkon.

Hello následující tabulka shrnuje hello různé výpočetní tooset kontextu možnosti, jak jsou vykonány volání:

| Výpočetní kontextu  | Jak tooset                      | Kontext spuštění                        |
| ---------------- | ------------------------------- | ---------------------------------------- |
| Místní sekvenčních | rxSetComputeContext('local')    | Paralelizovaná málo provádění mezi hello jader hello hraniční uzel serveru, s výjimkou rxExec volání, které jsou prováděny sériově |
| Místní paralelní   | rxSetComputeContext('localpar') | Paralelizovaná málo provádění mezi hello jader hello hraničního uzlu serveru |
| Spark            | RxSpark()                       | Paralelizovaná málo distribuované provádění prostřednictvím Spark mezi hello uzly clusteru HDI hello |
| Snižte mapy       | RxHadoopMR()                    | Paralelizovaná málo distribuované provádění prostřednictvím mapy snížit mezi hello uzly clusteru HDI hello |

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Pokyny pro rozhodování v kontextu výpočetní

Tři možnosti hello zvolíte, které poskytují paralelizovaná málo provádění závisí na povaze hello analytics pracovní, hello velikost a umístění hello vaše data. Neexistuje žádný jednoduchý vzorec, který zjistíte, které výpočetní toouse kontextu. Existují však některé základní zásady, které umožňují Zkontrolujte správnou volbou hello nebo alespoň, můžete zúžit vaše volby před spuštěním testu výkonnosti. Tyto zásady patří:

- místní systém souborů Linux Hello je rychlejší než HDFS.
- Opakované analýzy je rychlejší, pokud jsou hello data místní, a pokud se nachází v XDF.
- Je vhodnější toostream malé množství dat ze zdroje dat text. Pokud hello množství dat je větší, převeďte tooXDF před analýzou.
- nezvladatelné pro velmi velké objemy dat se stane Hello režii streamování hello data toohello hraniční uzel pro analýzu nebo kopírování.
- Spark je rychlejší než mapy snížit pro analýzu v Hadoop.

Zadané tyto zásady, hello následující části nabízí některé zásady pro výběr výpočetní kontextu.

### <a name="local"></a>Místní
* Pokud hello množství dat tooanalyze je malá a nevyžaduje opakovanou analýzu, pak Streamovat ho přímo do aplikace pomocí běžných analysis hello *'local'* nebo *'localpar'*.
* Pokud hello množství dat tooanalyze je malá nebo středně velký a vyžaduje opakovanou analýzu, pak zkopírujte jej toohello místního systému souborů, importujte ji tooXDF a analyzujte ji prostřednictvím *'local'* nebo *'localpar'*.

### <a name="hadoop-spark"></a>Hadoop, Spark
* Pokud hello množství dat tooanalyze velká, pak jej importovat tooa Spark DataFrame pomocí **RxHiveData** nebo **RxParquetData**, nebo tooXDF v HDFS (Pokud úložiště je problém) a analyzujte ji pomocí hello Spark výpočetní kontextu.

### <a name="hadoop-map-reduce"></a>Snižte Hadoop mapy
* Použijte hello mapy snížit výpočetní kontextu pouze v případě, že narazíte na problém s nepřekonatelnou s kontextem výpočtů Spark hello vzhledem k tomu, že je obecně pomalejší.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Vložené nápovědu k rxSetComputeContext
Další informace a příklady ScaleR výpočetní kontexty najdete v tématu nápovědy v R na hello rxSetComputeContext metoda, například vložené hello:

    > ?rxSetComputeContext

Můžete se také podívat toohello "[ScaleR distribuované Průvodce Computing](https://msdn.microsoft.com/microsoft-r/scaler-distributed-computing)", je k dispozici z hello [R Server MSDN](https://msdn.microsoft.com/library/mt674634.aspx "R Server na webu MSDN") knihovny.

## <a name="next-steps"></a>Další kroky
V tomto článku jste se dozvěděli o hello možnosti, které jsou k dispozici toospecify zda a jak je paralelizovaná provádění málo mezi jader hello hraniční uzel nebo clusteru HDInsight. toolearn Další informace o tom, jak clusterů toouse R Server s HDInsight, najdete v části hello následující témata:

* [Přehled R Server pro Hadoop](hdinsight-hadoop-r-server-overview.md)
* [Začínáme s R Server pro Hadoop](hdinsight-hadoop-r-server-get-started.md)
* [Přidat Server Rstudia tooHDInsight (Pokud není přidaný při vytváření clusteru)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Možnosti služby Azure Storage pro R Server ve službě HDInsight](hdinsight-hadoop-r-server-storage.md)

