---
title: "aaaUse clusterů více HDInsight pomocí účtu Azure Data Lake Store - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse více než jeden HDInsight clusteru pomocí jednoho účtu Data Lake Store"
keywords: "úložiště hdinsight, hdfs, strukturovaných dat, nestrukturovaných dat, data lake store"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: nitinme
ms.openlocfilehash: 3a125b91fcc1e436270a1bd349f7a2d8f27e06fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-multiple-hdinsight-clusters-with-an-azure-data-lake-store-account"></a>Použití více clusterů HDInsight pomocí účtu Azure Data Lake Store

Počínaje HDInsight verze 3.5, můžete vytvořit clustery HDInsight pomocí účtů Azure Data Lake Store jako hello výchozí systém souborů.
Data Lake Store podporuje neomezené úložiště, která usnadňuje ideální nejen pro hostování velké objemy dat; Můžete ale také pro hostování clusterů HDInsight více tuto sdílenou složku jeden účet Data Lake Store. Instructionson jak clusteru toocreate HDInsight s Data Lake Store jako hello úložiště, najdete v části [Tvorba clusterů HDInsight s Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

Tento článek obsahuje doporučení toohello Data Lake store správce pro nastavení jednoho a sdílené Data Lake ukládání účet, který lze použít v rámci více **active** clusterů HDInsight. Tato doporučení se týkají toohosting více zabezpečené a také nezabezpečené Hadoop clusterů v sdíleného účtu Data Lake store.


## <a name="data-lake-store-file-and-folder-level-acls"></a>Data Lake Store souborů a složek úroveň seznamy ACL

Hello zbývající části tohoto článku předpokládá, že máte dobrou znalost souborů a složek úroveň seznamy ACL v Azure Data Lake Store, který je popsán v podrobností v [řízení přístupu v Azure Data Lake Store](../data-lake-store/data-lake-store-access-control.md).

## <a name="data-lake-store-setup-for-multiple-hdinsight-clusters"></a>Data Lake Store nastavení pro více clusterů HDInsight
Dejte nám trvat složky dvojúrovňové hierarchii tooexplain hello doporučení pro používání více clusterů HDInsight pomocí účtu Data Lake Store. Vezměte v úvahu máte účet Data Lake Store s struktura složek hello **nebo clustery, finance**. Tato struktura všechny clustery hello hello finanční organizace vyžaduje pomocí /clusters/finance jako umístění úložiště hello. V budoucnu, pokud jiné organizaci, Marketing, že chce hello toocreate clusterů HDInsight pomocí stejného účtu Data Lake Store, uživatel může vytvořit /clusters/marketing hello. Prozatím se právě použijeme **nebo clustery, finance**.

tooenable této složky struktura toobe efektivně používané clustery HDInsight, hello Data Lake Store musí správce příslušná oprávnění, jak je popsáno v tabulce hello. oprávnění Hello uvedené v tabulce hello odpovídají tooAccess seznamy řízení přístupu a výchozí-ACL. 


|Složka  |Oprávnění  |Vlastnící uživatel  |Vlastnící skupina  | Jmenovaný uživatel | Jmenovaný uživatel oprávnění | S názvem skupiny | Oprávnění skupiny s názvem |
|---------|---------|---------|---------|---------|---------|---------|---------|
|/ | rwxr-x – x  |Správce |Správce  |Instanční objekt |– x  |FINGRP   |r – x         |
|/Clusters | rwxr-x – x |Správce |Správce |Instanční objekt |– x  |FINGRP |r – x         |
|nebo clustery, finance | rwxr-x – t |Správce |FINGRP  |Instanční objekt |rwx  |-  |-     |

V tabulce hello

- **správce** hello creator a správce hello účtu Data Lake Store.
- **Instanční objekt** je hello Azure Active Directory (AAD) instanční objekt spojené s účtem hello.
- **FINGRP** je skupina uživatelů vytvořené v AAD, která obsahuje uživatele z hello finanční organizace.

Pokyny, jak zjistit, toocreate AAD aplikace, (která také vytvoří objekt služby), [vytvořit aplikaci AAD](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application). Pokyny, jak toocreate skupinu uživatelů v AAD, najdete v části [Správa skupin v Azure Active Directory](../active-directory/active-directory-accessmanagement-manage-groups.md).

Některé klíčové body tooconsider.

- Struktura Hello dvě úrovně složek (**/clusterů nebo finanční nebo**) musí být vytvořen a vytváří Data Lake Store Dobrý den, správce s příslušnými oprávněními **před** účtem úložiště pro clustery hello. Tato struktura není vytvořena automaticky při vytváření clusterů.
- výše uvedený příklad Hello doporučuje nastavení hello vlastnící skupinu **nebo clustery, finance** jako **FINGRP** a jejímu **r-x** přístup tooFINGRP toohello celou složku hierarchie spouštění z kořenového hello. Tím se zajistí, že hello členy FINGRP můžete přejít od kořenové struktura složek hello.
- V případě hello při různé objekty služby AAD můžete vytvořit clustery v **nebo clustery, finance**, hello trvalé bit (Pokud nastavíte na hello **finanční** složky) zajišťuje, že složek vytvořených službou jeden Objekt nelze odstranit, hello jiné.
- Jakmile hello struktury složek a oprávnění jsou na místě, procesu vytváření clusteru HDInsight vytvoří místo specifických pro cluster úložiště v rámci **/clusterů nebo finanční nebo**. Například může být hello úložiště pro cluster s názvem fincluster01 hello **/clusters/finance/fincluster01**. Hello vlastnictví a oprávnění ke složkám hello vytvořené clusteru HDInsight se zobrazí v tabulce hello sem.

    |Složka  |Oprávnění  |Vlastnící uživatel  |Vlastnící skupina  | Jmenovaný uživatel | Jmenovaný uživatel oprávnění | S názvem skupiny | Oprávnění skupiny s názvem |
    |---------|---------|---------|---------|---------|---------|---------|---------|
    |/Clusters/finanace/fincluster01 | rwxr-x---  |Instanční objekt |FINGRP  |- |-  |-   |-  | 
   


## <a name="recommendations-for-job-input-and-output-data"></a>Doporučení pro úlohy vstupní a výstupní data

Jsme doporučujeme vstupní data tooa úlohy a hello výstupy z úlohy být uložena ve složce mimo **/clusterů**. Tím se zajistí i v případě, že složka specifických pro cluster hello je odstraněný tooreclaim některé prostor úložiště, hello úlohy vstupy a výstupy stále k dispozici pro budoucí použití. V takovém případě zajistěte, aby hierarchie hello složku pro ukládání hello úlohy vstupy a výstupy povolovalo odpovídající úroveň přístupu pro hello instanční objekt.

## <a name="limit-on-clusters-sharing-a-single-storage-account"></a>Omezit na clusterech sdílení účet jednoho úložiště

limit Hello hello počtu clusterů, které můžete sdílet jeden účet Data Lake Store, které závisí na pracovním vytížení hello spuštěn na těchto clusterů. S příliš mnoha clustery nebo velmi náročných úloh na hello clustery, které sdílejí účet úložiště může způsobit hello úložiště účet vstupní/výstupní tooget omezeny.

## <a name="support-for-default-acls"></a>Podpora pro výchozí seznamy řízení přístupu

Při vytváření objektu služby pomocí přístupu s názvem uživatele (jak je uvedené v předchozí tabulce hello), doporučujeme **není** přidání hello s názvem uživatelem s seznamu ACL výchozí. Zřizování přístupu s názvem uživatele pomocí výchozí ACL výsledky v přiřazení hello 770 oprávnění pro vlastnícího uživatele, který je vlastníkem skupiny a dalších. Při této výchozí hodnotu 770 rychle nevyžaduje oprávnění z vlastnícího uživatele (7) nebo vlastnící group (7), trvá rychle všechna oprávnění pro ostatní (0). Výsledkem je známý problém s jeden konkrétní případ použití, je podrobněji v hello [známé problémy a řešení](#known-issues-and-workarounds) části.

## <a name="known-issues-and-workarounds"></a>Známé problémy a řešení

Tato část uvádí hello známé problémy při použití HDInsight s Data Lake Store a jejich řešení.

### <a name="publicly-visible-localized-yarn-resources"></a>Veřejně viditelný lokalizované prostředky YARN

Když je vytvořen nový účet Azure Data Lake store, hello kořenový adresář automaticky zřízena too770 sadu bitů oprávnění přístupu ACL. Hello kořenové složky vlastnícího uživatele je sada toohello uživatel, který vytvořil účet hello (Data Lake Store Dobrý den, správce) a skupiny vlastnícím hello je nastavena toohello primární skupiny uživatele hello, který vytvořili účet hello. Žádný přístup se poskytuje "ostatní".

Tato nastavení se ví, že tooaffect jeden konkrétní HDInsight případ použití zaznamenané v [YARN 247](https://hwxmonarch.atlassian.net/browse/YARN-247). Odesílání úloh může selhat s podobné toothis k chybě zpráva:

    Resource XXXX is not publicly accessible and as such cannot be part of hello public cache.

Jak je uvedeno v hello YARN JIRA spojena dříve, při lokalizace veřejných prostředků hello, že lokalizátora ověří, že všechny hello požadované prostředky jsou skutečně veřejné kontrolou jejich oprávnění v systému vzdáleného souborů hello. Všechny LocalResource, který se nevejde této podmínky je odmítnutých pro lokalizaci. Zkontrolujte oprávnění Dobrý den, zahrnuje toohello přístup pro čtení souboru pro "ostatní". Tento scénář při hostování clusterů HDInsight v Azure Data Lake, protože Azure Data Lake odepře přístup všem příliš nefunguje se na pole "ostatní" na kořenové úrovni složky.

#### <a name="workaround"></a>Alternativní řešení
Sada čtení-oprávnění ke spouštění pro **ostatní** prostřednictvím hello hierarchie, například na  **/** , **/clusterů** a **nebo clustery, finance**  jak je uvedeno v předchozí tabulce hello.

## <a name="see-also"></a>Viz také

* [Vytvoření clusteru HDInsight s Data Lake Store jako úložiště](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)


