---
title: "Architektura Azure HDInsight připojený k doméně | Microsoft Docs"
description: "Naučte se plánovat službu HDInsight připojenou k doméně."
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
ms.openlocfilehash: 7e34f47f09466a40993b4cc797ff1cad2bdaeafe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Plánování clusterů Azure Hadoop připojených k doméně ve službě HDInsight

Tradiční Hadoop je jednouživatelský cluster. Je vhodný pro většinu společností, které mají menší aplikační týmy sestavující úlohy velkých objemů dat. Jak Hadoop získává na oblíbenosti, mnoho podniků přechází k modelu, kdy clustery spravují IT týmy a sdílí je několik aplikačních týmů. Proto jsou funkce zahrnující víceuživatelské clustery mezi nejžádanějšími funkcemi ve službě Azure HDInsight.

Místo vytváření vlastní víceuživatelská ověřování a autorizaci, HDInsight využívá nejoblíbenější zprostředkovatele identity – Active Directory (AD). Funkci efektivní zabezpečení ve službě Active Directory lze použít ke správě více uživatelů autorizace v HDInsight. Integrací HDInsight s AD může komunikovat s clustery pomocí svých přihlašovacích údajů AD. HDInsight mapuje Active Directory na místní uživatel Hadoop, takže všechny služby spuštěné v HDInsight (Ambari, Hive serveru škálu, Spark thrift serveru a dalších) pracovní bezproblémově pro ověřené uživatele.

## <a name="integrate-hdinsight-with-ad-and-ad-on-iaas-vm"></a>HDInsight integrovat AD a služby AD na virtuální počítač IaaS

Díky integraci s Azure AD ani AD pro virtuální počítač Iaas HDInsight, jsou uzly clusteru HDInsight, připojený k doméně. HDInsight vytvoří objekty služby pro služby Hadoop běžící v clusteru a umístí se do zadané organizační jednotce (OU) v Azure AD ani AD pro virtuální počítač IaaS. HDInsight vytvoří také zpětné mapování DNS v doméně pro IP adresy uzly, které jsou připojené k doméně.

Tohoto uspořádání můžete docílit pomocí několika architektur. Můžete si vybrat z následujících architektur.

**Integrované s AD běžících na Azure IaaS HDInsight**

Toto je nejjednodušší architektura pro integraci služby HDInsight s Active Directory. Řadič domény AD běží na jednu (nebo víc) virtuálních počítačů (VM) v Azure. Tyto virtuální počítače se obvykle nacházejí v rámci virtuální sítě. Vytvoříte další virtuální síť pro cluster HDInsight. Pro HDInsight tak, aby měl směrem pohledu do služby Active Directory, budete muset peer tyto virtuální sítě pomocí [VNet-to-VNet peering](../virtual-network/virtual-network-create-peering.md). Pokud vytvoříte služby Active Directory v ARM, potom můžete vytvořit služby Active Directory a prostředí HDInsight ve stejné virtuální síti a vy nemusíte dělat partnerský vztah. 

![Topologie clusteru HDInsight připojeného k doméně](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> V této architektuře nemůžete použít Azure Data Lake Store s clusterem HDInsight.


Předpoklady pro službu Active Directory:

* Musí být vytvořená [organizační jednotka](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), ve které umístíte virtuální počítače clusteru HDInsight a instanční objekty používané clusterem.
* [Protokoly přístup Lightweight Directory](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) (LDAPs) musí být nastavení pro komunikaci se službou AD. Certifikát použitý k nastavení protokolu LDAPS musí být skutečný certifikát (ne certifikát podepsaný svým držitelem).
* V doméně musí být vytvořené reverzní zóny DNS pro rozsah IP adres podsítě HDInsight (například 10.2.0.0/24 na předchozím obrázku).
* Je potřeba účet služby nebo účet uživatele. Pomocí tohoto účtu vytvoříte cluster HDInsight. Tento účet musí mít následující oprávnění:

    - Oprávnění k vytvoření instančních objektů a objektů počítačů v rámci organizační jednotky
    - Oprávnění k vytvoření pravidel reverzního proxy serveru DNS
    - Oprávnění k připojení počítačů k doméně služby Active Directory

**Služba HDInsight integrovaná s výhradně cloudovou službou Azure AD**

Pro výhradně cloudovou službu Azure AD nakonfigurujte řadič domény tak, aby HDInsight bylo možné integrovat s Azure AD. Toho dosáhnete pomocí [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) (Azure AD DS). Azure AD DS vytvoří počítače řadiče domény v cloudu a poskytne vám jejich IP adresy. Kvůli vyšší dostupnosti vytvoří dva řadiče domény.

Azure AD DS v současné době existuje pouze v klasických virtuálních sítích. Je dostupná jenom pomocí portálu Azure Classic. Virtuální síť HDInsight existuje na webu Azure Portal, takže bude potřeba vytvořit partnerský vztah dvou virtuálních sítí s klasickou virtuální sítí.

> [!NOTE]
> Partnerský vztah mezi klasickou virtuální sítí a virtuální sítí Azure Resource Manageru vyžaduje, aby obě virtuální sítě byly ve stejné oblasti a obě spadaly pod stejné předplatné Azure.

![Topologie clusteru HDInsight připojeného k doméně](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Požadavky pro Azure AD:

* Musí být vytvořená [organizační jednotka](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), ve které umístíte virtuální počítače clusteru HDInsight a instanční objekty používané clusterem.
* Při konfiguraci služby Azure AD DS musí být nastavený protokol [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md). Certifikát použitý k nastavení protokolu LDAPS musí být skutečný certifikát (ne certifikát podepsaný svým držitelem).
* V doméně musí být vytvořené reverzní zóny DNS pro rozsah IP adres podsítě HDInsight (například 10.2.0.0/24 na předchozím obrázku).
* Musí se synchronizovat [hodnoty hash hesel](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) ze služby Azure AD do Azure AD DS.
* Je potřeba účet služby nebo účet uživatele. Pomocí tohoto účtu vytvoříte cluster HDInsight. Tento účet musí mít následující oprávnění:

    - Oprávnění k vytvoření instančních objektů a objektů počítačů v rámci organizační jednotky
    - Oprávnění k vytvoření pravidel reverzního proxy serveru DNS
    - Oprávnění k připojení počítačů k doméně služby Azure AD

## <a name="next-steps"></a>Další kroky
* Pokud chcete konfigurovat cluster HDInsight připojený k doméně, přečtěte si téma [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md).
* Pokud chcete spravovat clustery HDInsight připojené k doméně, přečtěte si téma [Správa clusterů HDInsight připojených k doméně](hdinsight-domain-joined-manage.md).
* Pokud chcete konfigurovat zásady Hivu a spouštět dotazy Hivu, přečtěte si téma [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](hdinsight-domain-joined-run-hive.md).
* Ke spouštění dotazů Hive pomocí protokolu SSH na doméně clusterů HDInsight, najdete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).
