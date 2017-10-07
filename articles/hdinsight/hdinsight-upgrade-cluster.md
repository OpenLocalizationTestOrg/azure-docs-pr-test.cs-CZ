---
title: "novější verze aaaUpgrade HDInsight clusteru tooa-Azure | Microsoft Docs"
description: "Zjistěte, jak tooUpgrade HDInsight clusteru tooa novější verze."
services: hdinsight
documentationcenter: 
author: bhanupr
manager: asadk
editor: bhanupr
ms.assetid: 60eb573c-e639-4815-9fc6-ea8b106d8dbc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/04/2017
ms.author: bhanupr
ms.openlocfilehash: 5fff3c9bc88dfbcbc1ccb0188accdfbbec3a62f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-hdinsight-cluster-tooa-newer-version"></a>Upgrade clusteru HDInsight tooa novější verze
tootake výhod hello nejnovější funkce HDInsight, doporučujeme, aby clustery HDInsight se toolatest upgradovaná verze. Postupujte podle hello níže pokyny tooupgrade vaší verze clusteru HDInsight.

> [!NOTE]
> Clustery HDInsight verze 3.2 nebo 3.3 se blíží datum vyřazení. Informace o podporovanou verzi HDInsight, naleznete v části [HDInsight verze součástí](hdinsight-component-versioning.md#supported-hdinsight-versions).
>
>

## <a name="upgrade-tasks"></a>Úlohy upgradu
pracovní postup tooupgrade Hello clusteru HDInsight je následující.

![Diagram pracovní postup upgradu](./media/hdinsight-upgrade-cluster/upgrade-workflow.png)

1. Číst každá část tohoto dokumentu toounderstand změny, které mohou být vyžadovány při upgradu clusteru HDInsight.
2. Vytvořte cluster s podporou prostředí testovací a kvalita záruky. Další informace týkající se vytvoření clusteru najdete v tématu [zjistěte, jak toocreate HDInsight se systémem Linux clusterů](hdinsight-hadoop-provision-linux-clusters.md)
3. Kopie existující úlohy, zdroje dat a jímky toohello nové prostředí. V tématu [kopírovat Data tooTest prostředí](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) další podrobnosti.
4. Provedení ověření testování toomake jistotu, že vaše úlohy fungují podle očekávání na novém clusteru hello.


Jakmile si ověříte, že vše funguje podle očekávání, naplánujte dobu odstávky hello migrace. Během této výpadků hello následující akce:

1.  Zálohujte všechna přechodný data uložená místně na uzlech clusteru hello. Například pokud máte data uložená přímo na hlavního uzlu.
2.  Odstraňte existující cluster hello.
3.  Vytvoření clusteru s podporou v hello stejné sítě VNET podsíť s nejnovější (nebo podporovaných) HDI verze pomocí hello stejné výchozí datové úložiště, které hello předchozí cluster používá. To umožňuje hello nového clusteru toocontinue pracovat se vaše stávající provozními daty.
4.  Importujte přechodný data, která jste zálohovali.
5.  Spuštění úlohy nebo pokračovat ve zpracovávání pomocí hello nového clusteru.

## <a name="next-steps"></a>Další kroky
* [Zjistěte, jak toocreate HDInsight se systémem Linux clusterů](hdinsight-hadoop-provision-linux-clusters.md)
* [Připojit tooHDInsight pomocí protokolu SSH](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Spravovat cluster se systémem Linux pomocí nástroje Ambari](hdinsight-hadoop-manage-ambari.md)

