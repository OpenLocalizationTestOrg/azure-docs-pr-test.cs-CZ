---
title: "zabezpečení aaaHadoop - připojený k doméně clustery HDInsight - Azure | Microsoft Docs"
description: "Zjistěte..."
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
ms.date: 10/31/2016
ms.author: saurinsh
ms.openlocfilehash: 5a9469402a61bcba4017e1ff4bd06dfba23ac963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="an-introduction-toohadoop-security-with-domain-joined-hdinsight-clusters-preview"></a>Zabezpečení služby Úvod tooHadoop s clustery HDInsight připojený k doméně (Preview)

Služba Azure HDInsight až doteď podporovala jako místního správce pouze jednoho uživatele. To dobře fungovalo pro menší aplikace, týmy nebo oddělení. Jako Hadoop založené na úlohy, které získávají další oblíbenosti v hello enterprise sektoru, hello třeba pro podniky se stala stále důležité možnosti úrovni jako prostřednictvím služby active directory, ověřování, podpora více uživatelů a řízení přístupu na základě rolí. Pomocí clusterů HDInsight připojený k doméně, můžete vytvořit domény služby Active Directory tooan připojený clusteru HDInsight, nakonfigurujte seznam zaměstnanci z hello enterprise, kteří můžou ověřovat pomocí služby Azure Active Directory toolog na tooHDInsight clusteru. Každý, kdo mimo hello enterprise nelze přihlásit nebo přístup ke clusteru HDInsight hello. Hello správce podniku můžete konfigurovat řízení přístupu na základě rolí pro používání Hive zabezpečení [Apache škálu](http://hortonworks.com/apache/ranger/), proto omezíte přístup k toodata tooonly tolik, podle potřeby. Nakonec Dobrý den, správce můžete auditovat přístup k datům hello zaměstnanci a všechny změny provést tooaccess řídit zásady, proto dosažení vysoký stupeň zásady správného řízení podnikovým prostředkům.

> [!NOTE]
> Hello nových funkcí popsaných v této verzi preview jsou dostupné pouze pro clustery HDInsight se systémem Linux pro úlohy Hive. Dobrý den jiné úlohy, například HBase, Spark, Storm a Kafka, budou povolené v budoucnosti uvolní.

> [!IMPORTANT]
> Oozie není povoleno v doméně HDInsight.

## <a name="benefits"></a>Výhody
Zabezpečení podniku se skládá ze čtyř hlavních pilířů – Zabezpečení perimetru, Ověřování, Autorizace a Šifrování.

![Pilíře výhod clusterů HDInsight připojených k doméně](./media/hdinsight-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

### <a name="perimeter-security"></a>Zabezpečení perimetru
Služba HDInsight zajišťuje zabezpečení perimetru pomocí virtuálních sítí a služby Gateway. Enterprise Admins v současné době můžete vytvořit cluster služby HDInsight uvnitř virtuální sítě a použít skupiny zabezpečení sítě (příchozí nebo odchozí pravidla brány firewall) toorestrict přístup toohello virtuální síť. Pouze příchozí technologie hello IP adresy, které jsou definované v hello pravidla brány firewall, bude možné toocommunicate s clusterem HDInsight hello, takže zajištění zabezpečení hraniční. Další vrstvu zabezpečení perimetru zajišťuje služba Gateway. Hello brány je hello služby, která funguje jako první linií obrany pro všechny příchozí cluster HDInsight toohello požadavku. Přijme žádost o hello, ověří jeho platnost a pak umožňuje hello požadavek toopass toohello jiné uzly v clusteru, tedy zajištění zabezpečení hraniční tooother název a datové uzly v clusteru hello.

### <a name="authentication"></a>Authentication
V této verzi Public Preview může podnikový správce zřídit cluster HDInsight připojený k doméně ve [virtuální síti](https://azure.microsoft.com/services/virtual-network/). Hello uzly clusteru HDInsight hello bude připojený k toohello domény spravuje hello enterprise. Toho můžete dosáhnout s použitím služby [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md). Všechny hello uzly v clusteru hello jsou tooa připojené k doméně, která spravuje hello enterprise. S tímto nastavením hello enterprise zaměstnanci můžou přihlásit na uzlech clusteru toohello pomocí svých přihlašovacích údajů domény. Může také používat jejich tooauthenticate přihlašovací údaje domény pomocí jiných schválené koncových bodů jako toointeract Hue, zobrazení Ambari, rozhraní ODBC, JDBC, prostředí PowerShell a rozhraní REST API s clusterem hello. Dobrý den, správce má plnou kontrolu nad omezení hello počtu uživatelů interakci s clusterem hello prostřednictvím těchto koncových bodů.

### <a name="authorization"></a>Autorizace
Osvědčeným postupem následuje Většina firem je, že ne každý zaměstnanec má přístup k prostředkům organizace tooall. V této verzi se podobně Dobrý den, správce můžete definovat zásady řízení přístupu na základě rolí pro prostředky clusteru hello. Například můžete nakonfigurovat Dobrý den, správce [Apache škálu](http://hortonworks.com/apache/ranger/) tooset zásady řízení přístupu pro Hive. Tato funkce zajišťuje, že zaměstnanci budou moct tooaccess jenom tolik dat, které potřebovat toobe úspěšné při své práci. Cluster toohello přístup SSH je také s omezeným přístupem toohello pouze správce.

### <a name="auditing"></a>Auditování
Společně s ochrana hello HDInsight prostředky clusteru před neoprávněnými uživateli a zabezpečení dat hello, auditování všech přístup k prostředkům clusteru toohello a hello data je nutné tootrack neoprávněným i neúmyslnému přístupu hello prostředků. Dobrý den, správce s touto verzí preview, můžete zobrazit a sestavy všechny prostředky clusteru HDInsight toohello přístup a data. Dobrý den, správce můžete také zobrazit a sestavy, všechny změny zásady řízení přístupu toohello v Apache škálu podporované koncové body. Cluster HDInsight se připojený k doméně používá protokoly auditu toosearch známé uživatelské rozhraní Apache škálu hello. Na back-end hello, používá škálu [Apache Solr](http://hortonworks.com/apache/solr/) pro ukládání a vyhledávání hello protokoly.

### <a name="encryption"></a>Šifrování
Ochrana dat je důležité pro zabezpečení organizace schůzku a požadavky na dodržování předpisů a společně se omezení přístupu toodata z neoprávněným zaměstnanci, ho musí také být zabezpečený šifrování je. Obě hello datová úložiště pro clustery služby HDInsight, objektu Blob Azure Storage a Azure Data Lake Storage podporují transparentní serverové [šifrování dat](../storage/common/storage-service-encryption.md) v klidovém stavu. Zabezpečené clustery HDInsight budou bez problému fungovat s touto schopností šifrování neaktivních uložených dat na straně serveru.

## <a name="next-steps"></a>Další kroky
* Pokud chcete konfigurovat cluster HDInsight připojený k doméně, přečtěte si téma [Konfigurace clusterů HDInsight připojených k doméně](hdinsight-domain-joined-configure.md).
* Pokud chcete spravovat clustery HDInsight připojené k doméně, přečtěte si téma [Správa clusterů HDInsight připojených k doméně](hdinsight-domain-joined-manage.md).
* Pokud chcete konfigurovat zásady Hivu a spouštět dotazy Hivu, přečtěte si téma [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](hdinsight-domain-joined-run-hive.md).
* Spuštění dotazů Hive pomocí protokolu SSH v clusterech HDInsight připojený k doméně, najdete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
