---
title: "Zabezpečení clusteru Service Fabric | Microsoft Docs"
description: "Popisuje scénáře zabezpečení clusteru Service Fabric a různých technologií, použít k implementaci těchto scénářů."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 26b58724-6a43-4f20-b965-2da3f086cf8a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: chackdan
ms.openlocfilehash: 5afbe575a8affc37b8f902c0988585a83921e3d2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="service-fabric-cluster-security-scenarios"></a>Scénáře zabezpečení clusteru Service Fabric
Cluster Service Fabric je prostředek, který vlastníte. Clustery musí být zabezpečená neoprávněným uživatelům zabránit v připojení ke clusteru, zejména v případě, že je v něm spuštěny úlohy v produkčním prostředí. Přestože je možné vytvořit zabezpečená clusteru, tak učiníte, umožníte anonymní uživatelé pro připojení, pokud vystavuje koncové body správy do veřejného Internetu. 

Tento článek obsahuje přehled scénáře zabezpečení pro clustery se systémem na Azure nebo samostatné a různých technologií použít k implementaci těchto scénářů. Scénáře zabezpečení clusteru jsou:

* Zabezpečení – uzly
* Uzel Klient zabezpečení
* Řízení přístupu na základě role (RBAC)

## <a name="node-to-node-security"></a>Zabezpečení – uzly
Zabezpečuje komunikaci mezi virtuální počítače nebo počítače v clusteru. Tím se zajistí, že pouze počítače, které jsou autorizované pro připojení ke clusteru mohou být součástí hostování aplikací a služeb v clusteru.

![Diagram komunikaci mezi uzly][Node-to-Node]

Clustery se systémem na Azure nebo samostatné clustery spuštěná v systému Windows můžete použít buď [zabezpečení Certificate](https://msdn.microsoft.com/library/ff649801.aspx) nebo [zabezpečení systému Windows](https://msdn.microsoft.com/library/ff649396.aspx) pro počítače, Windows Server.

### <a name="node-to-node-certificate-security"></a>Zabezpečení certificate – uzly
Service Fabric používá certifikáty X.509 serveru, které zadáte jako součást konfigurace typ uzlu při vytváření clusteru. Rychlý přehled toho, co jsou tyto certifikáty a jak můžete získat nebo jejich vytvoření je k dispozici na konci tohoto článku.

Při vytváření clusteru buď prostřednictvím portálu Azure, šablon Azure Resource Manageru nebo šablonu JSON samostatné konfigurace certifikát zabezpečení. Můžete určit primární certifikát a volitelné sekundární certifikát, který se používá pro certifikát efekty přechodu. Primární a sekundární certifikáty zadáte by měla být jiná než správce klienta a jen pro čtení klientské certifikáty, které zadáte pro [klientský uzel zabezpečení](#client-to-node-security).

Pro čtení Azure [nastavení clusteru pomocí šablony Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) se dozvíte, jak nakonfigurovat certifikát zabezpečení v clusteru.

Pro samostatnou službu Windows Server číst [zabezpečení samostatné clusteru v systému Windows pomocí certifikátů X.509](service-fabric-windows-cluster-x509-security.md)

### <a name="node-to-node-windows-security"></a>Zabezpečení systému windows pro uzel
Pro samostatnou službu Windows Server číst [zabezpečení samostatné clusteru v systému Windows pomocí zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md)

## <a name="client-to-node-security"></a>Uzel Klient zabezpečení
Ověřuje klienty, kteří a zabezpečuje komunikaci mezi klientem a jednotlivé uzly v clusteru. Tento typ zabezpečení ověřuje a zabezpečuje komunikaci klienta, který zajišťuje přístup jenom autorizovaným uživatelům cluster a aplikace nasazené na clusteru. Klienti jsou jednoznačně identifikuje prostřednictvím jejich přihlašovacích údajů zabezpečení systému Windows nebo přihlašovacích certifikátů zabezpečení.

![Diagram komunikace klienta uzlu][Client-to-Node]

Clustery se systémem na Azure nebo samostatné clustery spuštěná v systému Windows můžete použít buď [zabezpečení Certificate](https://msdn.microsoft.com/library/ff649801.aspx) nebo [zabezpečení systému Windows](https://msdn.microsoft.com/library/ff649396.aspx).

### <a name="client-to-node-certificate-security"></a>Certifikát klienta uzel zabezpečení
 Při vytváření clusteru, buď prostřednictvím portálu Azure Resource Manager šablony nebo šablony JSON samostatné zadáním certifikát klienta správce nebo klientský certifikát uživatele konfigurace uzlu klientů certifikát zabezpečení.  Správce klienta a uživatelské certifikáty klienta zadáte by měla být jiné než primární a sekundární certifikáty pro zadáte [-uzly zabezpečení](#node-to-node-security) jako osvědčený postup. Ve výchozím nastavení certifikáty clusteru mezi uzly zabezpečení se přidají do seznamu povolených klienta správce certifikátů.

Klienti připojení ke clusteru pomocí Správce certifikátů mají plný přístup k možnosti správy.  Klienti připojení ke clusteru pomocí klientský certifikát uživatele jen pro čtení mít přístup jenom pro čtení k možnosti správy. Jinými slovy tyto certifikáty se používají pro základů přístup řízení rolí (RBAC), který je popsáno dále v tomto článku.

Pro čtení Azure [nastavení clusteru pomocí šablony Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) se dozvíte, jak nakonfigurovat certifikát zabezpečení v clusteru.

Pro samostatnou službu Windows Server číst [zabezpečení samostatné clusteru v systému Windows pomocí certifikátů X.509](service-fabric-windows-cluster-x509-security.md)

### <a name="client-to-node-azure-active-directory-aad-security-on-azure"></a>Uzel Klient zabezpečení Azure Active Directory (AAD) v Azure
Clustery se systémem na platformě Azure můžete také zabezpečený přístup ke koncovým bodům správy pomocí Azure Active Directory (AAD). V tématu [nastavení clusteru pomocí šablony Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) informace o vytvoření artefakty potřebné AAD, jak naplnit při vytváření clusteru a jak se připojit k tyto clustery později.

## <a name="security-recommendations"></a>Doporučení zabezpečení
Pro Azure clusterů se doporučuje použít k ověřování klientů a certifikáty pro zabezpečení – uzly zabezpečení AAD.

Samostatný Windows Server clustery se doporučuje používat zabezpečení systému Windows se skupiny účtech spravované (GMA) Pokud máte systém Windows Server 2012 R2 a služby Active Directory. Zabezpečení systému Windows stále jinak pomocí účtů systému Windows.

## <a name="role-based-access-control-rbac"></a>Řízení přístupu na základě role (RBAC)
Řízení přístupu umožňuje omezit přístup k určité operace clusteru pro různé skupiny uživatelů, lepší zabezpečení clusteru pomocí Správce clusteru. Podporuje dva typy různý přístup řízení pro klienty připojující se ke clusteru: role správce a role uživatele.

Správci mají plný přístup k funkcím správy (včetně možnosti pro čtení i zápis). Uživatelé, ve výchozím nastavení, mají pouze pro čtení přístup k možnosti správy (například možnosti dotazu) a možnost řešení aplikace a služby.

Zadáte správce a uživatelských rolí klienta v době vytváření clusteru poskytnutím oddělené identity (certifikáty, AAD atd.) pro každý. Další informace o výchozí nastavení řízení přístupu a jak změnit výchozí nastavení, najdete v části [řízení přístupu podle rolí pro Service Fabric klienty](service-fabric-cluster-security-roles.md).

## <a name="x509-certificates-and-service-fabric"></a>X.509 – certifikáty a Service Fabric
Digitální certifikáty X.509 běžně se používají k ověřování klientů a serverů a k šifrování a digitálnímu podepisování zpráv. Další informace o tyto certifikáty, přejděte na [práce s certifikáty](http://msdn.microsoft.com/library/ms731899.aspx).

Vzít v úvahu některé důležité věci:

* Certifikáty používané v clusterech spuštění úlohy v produkčním prostředí by měla být vytvořen pomocí správně nakonfigurovaných certifikát služby Windows Server nebo získané z schválené [certifikační autoritou (CA)](https://en.wikipedia.org/wiki/Certificate_authority).
* Nikdy nepoužívejte žádné dočasných nebo testovací certifikáty v produkčním prostředí, které jsou vytvořeny pomocí nástrojů, jako je MakeCert.exe.
* Můžete použít certifikát podepsaný svým držitelem, ale měli tak učinit pouze pro testovací clustery a ne v produkčním prostředí.

### <a name="server-x509-certificates"></a>Certifikáty X.509 serveru
Certifikáty serveru mít úlohu primární ověřování serveru (uzel) pro klienty nebo ověřování serveru (uzel) na server (uzel). Jedním počáteční kontrol, když se klient nebo uzel ověřuje uzlu je zkontrolujte hodnotu běžný název v poli pro předmět. Tento běžný název nebo jeden z alternativní názvy předmětu certifikátů se musí být uvedena v seznamu povolených běžných názvů.

V následujícím článku popisuje, jak vygenerovat certifikáty s alternativní názvy subjektu (SAN): [postup přidání alternativní název subjektu certifikát zabezpečený LDAP](http://support.microsoft.com/kb/931351).

Pole Předmět může obsahovat několik hodnot, každý s předponou inicializaci označující typ hodnoty. Inicializace nejčastěji, je "CN" pro běžný název; například "CN = www.contoso.com". Je také možné pro pole Předmět být prázdné. Pokud se vyplní volitelné pole alternativní název subjektu, musí obsahovat běžný název certifikátu a jeden záznam za alternativní název subjektu. Tyto jsou vloženy hodnoty názvu DNS.

Hodnota určená pro účely pole certifikátu by měla obsahovat odpovídající hodnotu, jako je například "Ověřování serveru" nebo "Ověření klienta".

### <a name="client-x509-certificates"></a>Klientské certifikáty X.509
Klientské certifikáty nejsou obvykle vystavované certifikační autority. Místo toho osobním úložišti aktuální umístění uživatele obvykle obsahuje klientské certifikáty umísťují kořenovou autoritou, s zamýšlený účel "Ověření klienta". Klienta můžete použít tento certifikát, pokud se vyžaduje vzájemné ověření.

> [!NOTE]
> Všechny operace správy na cluster Service Fabric vyžadují certifikáty serveru. Klientské certifikáty nelze použít pro správu.
> 
> 

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->


## <a name="next-steps"></a>Další kroky
Tento článek obsahuje koncepční informace o zabezpečení clusteru. Další,


1.  [Vytvoření clusteru s podporou v Azure pomocí šablony Resource Manageru](service-fabric-cluster-creation-via-arm.md) 
2.  [Portál Azure](service-fabric-cluster-creation-via-portal.md).

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
