---
title: "návody aaaHDInsight Spark v Azure pomocí PySpark a Scala | Microsoft Docs"
description: "Příklady hello proces vědecké účely Team dat, které provede hello použití PySpark a Scala na Azure HDInsight Spark toodo prediktivní analýzy."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: bradsev
ms.openlocfilehash: f8b41a8cae414586570761ba4b4eb4c239cbb981
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hdinsight-spark-data-science-walkthroughs-using-pyspark-and-scala-on-azure"></a>Návody vědecké účely dat HDInsight Spark v Azure pomocí PySpark a Scala

Tyto postupy použijte PySpark a Scala na toodo clusteru Azure Spark pro prediktivní analýzy. Následují hello kroků uvedených v hello proces vědecké účely dat týmu. Přehled hello proces vědecké účely Team dat najdete v tématu [proces vědecké účely dat](data-science-process-overview.md). Přehled Spark v HDInsight, naleznete v části [Úvod tooSpark v HDInsight](../hdinsight/hdinsight-apache-spark-overview.md).

Návody vědecké účely další data, které jsou spouštěny hello proces vědecké účely Team dat jsou seskupené podle hello **platformy** , které používají: 

[!INCLUDE [tdsp-walkthroughs-by-platform](../../includes/tdsp-walkthroughs-by-platform.md)]

## <a name="predict-taxi-tips-using-pyspark-on-azure-spark"></a>Předpověď taxíkem tipy pomocí PySpark na Azure Spark

Hello [používejte Spark v Azure HDInsight](machine-learning-data-science-spark-overview.md) návod používá data z New Yorku taxislužby toopredict, zda je placené tip a rozsah hello objemu očekává toobe placené. Používá hello proces vědecké účely dat Team scénář pomocí [clusteru Azure HDInsight Spark](https://azure.microsoft.com/services/hdinsight/) toostore, prozkoumat a funkce pracovníka data z hello veřejně dostupné NYC taxíkem cestě a jízdenky datovou sadu. Toto přehledové téma nastaví můžete s clusteru HDInsight Spark a hello Jupyter PySpark poznámkových bloků, které jsou používány hello zbytek návod hello. Tyto poznámkových bloků ukazují, jak tooexplore vaše data a poté jak toocreate a využívat modelů. Hello advanced zkoumání dat a modelování poznámkového bloku ukazuje, jak tooinclude (vymetání) křížového ověřování, technologie hyper parametr komínů a vyhodnocení modelu.

### <a name="data-exploration-and-modeling-with-spark"></a>Zkoumání dat a modelování pomocí Spark 
Prozkoumejte hello datovou sadu a vytvářet, stanovení skóre a vyhodnotit hello machine learning modely projdete hello [vytvořit binární klasifikace a regrese modely pro data s hello Spark MLlib toolkit](machine-learning-data-science-spark-data-exploration-modeling.md) tématu.

### <a name="model-consumption"></a>Spotřeba modelu
toolearn způsobu tooscore hello klasifikace a regrese modely vytvořené v tomto tématu najdete v [skóre a vyhodnocení modelů learning vytvořené Spark počítač](machine-learning-data-science-spark-model-consumption.md).

### <a name="cross-validation-and-hyperparameter-sweeping"></a>Křížové ověření a hyperparameter (vymetání) komínů
V tématu [Advanced zkoumání dat a modelování pomocí Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) na tom, jak může být modely Trénink pomocí sweeping křížové ověření a technologie hyper parametr.


## <a name="predict-taxi-tips-using-scala-on-azure-spark"></a>Předpověď taxíkem tipy pomocí na Azure Spark Scala

Hello [pomocí Scala pomocí Spark v Azure](machine-learning-data-science-process-scala-walkthrough.md) návod používá data z New Yorku taxislužby toopredict, zda je placené tip a rozsah hello objemu očekává toobe placené. Ukazuje, jak toouse Scala pro úkoly pod dohledem strojového učení s hello Spark machine learning knihovny (MLlib) a SparkML balíčky v clusteru Azure HDInsight Spark. Provede vás prostřednictvím hello úloh, které tvoří hello [proces vědecké účely dat](http://aka.ms/datascienceprocess): přijímání dat a zkoumání, vizualizace, funkce analýzy, modelování a spotřeba modelu. modely Hello vytvářeny zahrnují logistic a lineární regrese, náhodné doménové struktury a přechodu boosted stromy.


## <a name="next-steps"></a>Další kroky

Informace hello klíčových komponent, které tvoří hello proces vědecké účely dat týmu, naleznete v [přehled tým datové vědy procesu](data-science-process-overview.md).

Informace životního cyklu hello proces vědecké účely Team dat, které můžete použít toostructure projekty vědecké účely dat, naleznete v [procesu vědecké účely Team datového cyklu](data-science-process-lifecycle.md). životní cyklus Hello popisuje hello kroky, z toofinish spuštění, který projekty obvykle postupujte při jejich spuštění. 

