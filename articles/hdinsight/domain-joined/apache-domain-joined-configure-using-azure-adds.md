---
title: "Konfigurace clusterů HDInsight připojený k doméně pomocí nástroje Azure Active Directory Domain Services – Azure | Microsoft Docs"
description: "Zjistěte, jak připravit a nakonfigurovat clusterů HDInsight připojený k doméně pomocí nástroje Azure Active Directory Domain Services"
services: hdinsight
documentationcenter: 
author: bprakash
manager: jhubbard
editor: cgronlun
tags: 
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/10/2017
ms.author: bhanupr
ms.openlocfilehash: 77478616eae27828a57a36dc0aaf3884e80ce403
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="configure-domain-joined-hdinsight-clusters-using-azure-active-directory-domain-services"></a>Konfigurace clusterů HDInsight připojený k doméně pomocí nástroje Azure Active Directory Domain Services

Připojené k doméně clustery poskytují více uživateli enterprise funkcích zabezpečení v HDInsight. Clustery HDInsight připojený k doméně jsou připojené k domén služby active directory tak, aby uživatelé domény můžete používat přihlašovací údaje domény k ověření s clustery a spouštění úloh big data. 

K nastavení řadiče domény, aby cluster připojený k doméně HDInsight se můžete připojit k dvěma způsoby:

- Azure Active Directory Domain Services (Azure AD DS)
- Řadič služby Active Directory Domain na virtuálních počítačích Azure IaaS

V tomto článku zjistěte, jak nakonfigurovat cluster HDInsight připojený k doméně pomocí Azure Active Directory Domain Services.

## <a name="create-azure-adds"></a>Vytvoření Azure přidá

Budete muset vytvořit Azure AD DS před vytvořením clusteru služby HDInsight. Vytvoření služby Azure přidá naleznete v tématu [povolit Azure Active Directory Domain Services pomocí webu Azure portal](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Pouze správci klientů mají oprávnění k vytvoření služby domain services. Pokud používáte Azure Data Lake Storage (ADLS) jako výchozí úložiště pro HDInsight, pak zajistěte, aby že výchozí klient Azure AD pro ADLS je stejná jako doména clusteru HDInsight. 

Po zřídil službu domény musíte vytvořit účet služby v **správci řadič domény služby Azure AD** skupiny k vytvoření clusteru HDInsight. Účet služby musí být globální správce na Azure AD.

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Vytvoření clusteru HDInsight se připojený k doméně

Dalším krokem je vytvoření clusteru HDInsight pomocí AAD DS a účet služby vytvořené v předchozí části.

Je jednodušší umístit ve stejném Azure virtuální network(VNet) služba Azure AD domain Services a clusteru HDInsight. V případě, že jsou v různých virtuálních sítí, musí peer obě virtuální sítě. Další informace najdete v tématu [partnerský vztah virtuální sítě](../../virtual-network/virtual-network-peering-overview.md).

Když vytvoříte cluster HDInsight připojený k doméně, je nutné zadat následující parametry:

- **Název domény**: název domény přidružený k Azure AD DS. Například contoso.onmicrosoft.com
- **Uživatelské jméno domény**: účet služby ve skupině Správci řadič domény služby Azure AD, který je vytvořen v předchozí části. Například, hdiadmin@contoso.onmicrosoft.com. Tento uživatel domény je správce tento cluster HDInsight připojený k doméně.
- **Heslo domény**: heslo účtu služby.
- **Organizační jednotka**: rozlišující název organizační jednotky, kterou chcete použít s clusterem HDInsight. Například: OU = HDInsightOU, DC = contoso, DC = onmicrosohift, DC = com. Pokud tuto organizační jednotku neexistuje, pokusí se HDInsight cluster vytvořit tuto organizační jednotku. 
- **Adresa URL LDAPS**: například ldaps://contoso.onmicrosoft.com:636
- **Skupina uživatelů přístup**: skupiny zabezpečení, jejichž uživatelé, které chcete synchronizovat do clusteru. Například HiveUsers. Pokud chcete zadat víc skupin uživatelů, oddělte je čárkou ','.
 
Následující snímek obrazovky ukazuje konfigurace na portálu Azure:

![Konfigurace Azure HDInsight připojený k doméně Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Další postup
* Pokud chcete konfigurovat zásady Hivu a spouštět dotazy Hivu, přečtěte si téma [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](apache-domain-joined-run-hive.md).
* Pro připojení ke clusterům HDInsight připojený k doméně pomocí protokolu SSH, najdete v části [použití SSH se systémem Linux Hadoop v HDInsight ze systému Linux, Unix nebo OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

