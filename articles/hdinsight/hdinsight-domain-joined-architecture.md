---
title: "připojené k aaaDomain architektura Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak tooplan doméně HDInsight."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/03/2017
ms.author: saurinsh
ms.openlocfilehash: 1c3ecedf3739b4f8fa54160225be9c1d6e2ca6cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Plánování clusterů Azure Hadoop připojených k doméně ve službě HDInsight

Hello tradiční Hadoop je cluster jednoho uživatele. Je vhodný pro většinu společností, které mají menší aplikační týmy sestavující úlohy velkých objemů dat. Jak Hadoop získává na oblíbenosti, mnoho podniků přechází k modelu, kdy clustery spravují IT týmy a sdílí je několik aplikačních týmů. Proto funkce, které zahrnující clustery s více uživateli patří mezi hello nejvíce požadované funkce v Azure HDInsight.

Místo vytváření vlastní víceuživatelská ověřování a autorizaci, HDInsight využívá hello nejoblíbenější zprostředkovatele identity – Active Directory (AD). Funkce Hello efektivní zabezpečení ve službě AD může být použité toomanage víceuživatelská autorizace v HDInsight. Integrací HDInsight s AD může komunikovat s clustery hello pomocí svých přihlašovacích údajů AD. HDInsight mapuje uživatele tooa místní Hadoop uživatel, takže všechny hello služby spuštěné v HDInsight (Ambari, Hive serveru škálu, Spark thrift serveru a dalších) pracovní bezproblémově pro hello ověřeného uživatele.

## <a name="integrate-hdinsight-with-ad-and-ad-on-iaas-vm"></a>HDInsight integrovat AD a služby AD na virtuální počítač IaaS

Díky integraci s Azure AD ani AD pro virtuální počítač Iaas HDInsight, jsou uzly clusteru HDInsight hello domény tooa připojený k doméně. HDInsight vytvoří objekty služby pro hello Hadoop běžící v clusteru hello a umístí se do zadané organizační jednotce (OU) v Azure AD ani AD pro virtuální počítač IaaS. HDInsight vytvoří také zpětné mapování DNS v doméně hello hello IP adresy hello uzlů, které jsou připojené k toohello domény.

Tohoto uspořádání můžete docílit pomocí několika architektur. Můžete zvolit z následujících architektury hello.

**Integrované s AD běžících na Azure IaaS HDInsight**

Toto je nejjednodušší architektura hello pro HDInsight integraci se službou Active Directory. Hello řadič domény AD se spouští na jednu (nebo víc) virtuálních počítačů (VM) v Azure. Tyto virtuální počítače se obvykle nacházejí v rámci virtuální sítě. Nastavit jinou virtuální sítí pro hello HDInsight cluster. Pro HDInsight toohave směrem pohledu tooActive Directory, je třeba toopeer tyto virtuální sítě pomocí [VNet-to-VNet peering](../virtual-network/virtual-network-create-peering.md). Pokud vytvoříte hello služby Active Directory v ARM, pak můžete vytvořit hello služby Active Directory a HDInsight v hello stejnou virtuální síť a nepotřebujete toodo partnerský vztah. 

![Topologie clusteru HDInsight připojeného k doméně](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> V této architektuře nemůžete použít Azure Data Lake Store s clusterem HDInsight hello.


Předpoklady pro službu Active Directory:

* [Organizační jednotky](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) musí být vytvořen, v rámci které umístíte hello virtuální počítače clusteru HDInsight a hello objekty služby používá hello cluster.
* [Protokoly přístup Lightweight Directory](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) (LDAPs) musí být nastavení pro komunikaci se službou AD. certifikát používaný tooset Hello až LDAPS musí být skutečné certifikátu (ne certifikát podepsaný svým držitelem).
* Zpětné zóny DNS musí být vytvořeny na hello domény pro rozsah IP adres hello hello HDInsight podsítě (například 10.2.0.0/24 na předchozím obrázku hello).
* Je potřeba účet služby nebo účet uživatele. Použijte tento cluster HDInsight hello toocreate účtu. Tento účet musí mít hello následující oprávnění:

    - Hlavní objekty služby toocreate oprávnění a objekty počítačů v rámci hello organizační jednotky
    - Oprávnění toocreate zpětné DNS proxy pravidla
    - Doménu služby Active Directory toohello počítače toojoin oprávnění

**Služba HDInsight integrovaná s výhradně cloudovou službou Azure AD**

Pro výhradně cloudovou službu Azure AD nakonfigurujte řadič domény tak, aby HDInsight bylo možné integrovat s Azure AD. Toho dosáhnete pomocí [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) (Azure AD DS). Azure AD DS vytvoří počítače řadiče domény v cloudu hello a poskytuje IP adresy pro ně. Kvůli vyšší dostupnosti vytvoří dva řadiče domény.

Azure AD DS v současné době existuje pouze v klasických virtuálních sítích. Jenom je přístupné pomocí hello portál Azure classic. Hello HDInsight virtuální sítě existuje v hello portál Azure, který se musí toobe peered s hello klasickou virtuální síť pomocí VNet-to-VNet partnerský vztah.

> [!NOTE]
> Partnerský vztah mezi klasickou virtuální síť a virtuální síť vyžaduje, že jsou obě virtuální sítě v Azure Resource Manageru hello stejné oblasti a v části hello stejné předplatné Azure.

![Topologie clusteru HDInsight připojeného k doméně](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Požadavky pro Azure AD:

* [Organizační jednotky](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) musí být vytvořen v rámci které umístíte hello virtuální počítače clusteru HDInsight a hello objekty služby používá hello cluster.
* Při konfiguraci služby Azure AD DS musí být nastavený protokol [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md). certifikát používaný tooset Hello až LDAPS musí být skutečné certifikátu (ne certifikát podepsaný svým držitelem).
* Zpětné zóny DNS musí být vytvořeny na hello domény pro rozsah IP adres hello hello HDInsight podsítě (například 10.2.0.0/24 na předchozím obrázku hello).
* [Hodnoty hash hesla](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) musí být synchronizované z Azure AD tooAzure služby AD DS.
* Je potřeba účet služby nebo účet uživatele. Použijte tento cluster HDInsight hello toocreate účtu. Tento účet musí mít hello následující oprávnění:

    - Hlavní objekty služby toocreate oprávnění a objekty počítačů v rámci hello organizační jednotky
    - Oprávnění toocreate zpětné DNS proxy pravidla
    - Oprávnění toojoin počítače toohello Azure AD domény

## <a name="next-steps"></a>Další kroky
* tooconfigure cluster HDInsight připojený k doméně, najdete v části [nakonfigurovat připojený k doméně clusterů HDInsight](hdinsight-domain-joined-configure.md).
* toomanage připojený k doméně clusterů HDInsight, najdete v části [Správa clusterů HDInsight připojený k doméně](hdinsight-domain-joined-manage.md).
* zásady tooconfigure Hive a spouštění dotazů Hive, najdete v části [konfigurace Hive zásady pro připojené k doméně clustery služby HDInsight](hdinsight-domain-joined-run-hive.md).
* toorun dotazů Hive pomocí protokolu SSH na clusterech HDInsight připojený k doméně, najdete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).
