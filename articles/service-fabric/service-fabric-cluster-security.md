---
title: aaaSecure cluster Service Fabric | Microsoft Docs
description: "Popisuje scénáře zabezpečení hello tooimplement Service Fabric clusteru a hello různých technologií, které používají tyto scénáře."
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
ms.openlocfilehash: 249a9e85b8fbe174e2accee85a94d95b2872a3af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-cluster-security-scenarios"></a>Scénáře zabezpečení clusteru Service Fabric
Cluster Service Fabric je prostředek, který vlastníte. Clustery musí být zabezpečené tooprevent neoprávněného uživatele z připojení clusteru tooyour, zejména v případě, že je v něm spuštěny úlohy v produkčním prostředí. Přestože je možné toocreate zabezpečená clusteru, tak umožňuje anonymní uživatelé tooconnect tooit, pokud vystavuje toohello koncové body správy veřejného Internetu. 

Tento článek obsahuje přehled hello scénáře zabezpečení pro clustery spuštěná v Azure nebo samostatné a hello tooimplement různých technologií, které používají tyto scénáře. scénáře zabezpečení clusteru Hello jsou:

* Zabezpečení – uzly
* Uzel Klient zabezpečení
* Řízení přístupu na základě role (RBAC)

## <a name="node-to-node-security"></a>Zabezpečení – uzly
Zabezpečuje komunikaci mezi hello virtuální počítače nebo počítače v clusteru hello. Tím se zajistí, že pouze počítače, které jsou autorizované toojoin hello clusteru mohou být součástí hostování aplikací a služeb v clusteru hello.

![Diagram komunikaci mezi uzly][Node-to-Node]

Clustery se systémem na Azure nebo samostatné clustery spuštěná v systému Windows můžete použít buď [zabezpečení Certificate](https://msdn.microsoft.com/library/ff649801.aspx) nebo [zabezpečení systému Windows](https://msdn.microsoft.com/library/ff649396.aspx) pro počítače, Windows Server.

### <a name="node-to-node-certificate-security"></a>Zabezpečení certificate – uzly
Service Fabric používá certifikáty X.509 serveru, které zadáte jako součást konfigurace hello typ uzlu při vytváření clusteru. Rychlý přehled toho, co jsou tyto certifikáty a jak můžete získat nebo jejich vytvoření je k dispozici na konci hello tohoto článku.

Při vytváření clusteru hello buď prostřednictvím hello portálu Azure, šablon Azure Resource Manageru nebo šablonu JSON samostatné konfigurace certifikát zabezpečení. Můžete určit primární certifikát a volitelné sekundární certifikát, který se používá pro certifikát efekty přechodu. Hello primární a sekundární certifikáty zadáte by měla být jiná než hello správce klienta a jen pro čtení klientských certifikátů, který určíte v parametru [klientský uzel zabezpečení](#client-to-node-security).

Pro čtení Azure [nastavení clusteru pomocí šablony Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) toolearn jak tooconfigure certifikátu zabezpečení v clusteru.

Pro samostatnou službu Windows Server číst [zabezpečení samostatné clusteru v systému Windows pomocí certifikátů X.509](service-fabric-windows-cluster-x509-security.md)

### <a name="node-to-node-windows-security"></a>Zabezpečení systému windows pro uzel
Pro samostatnou službu Windows Server číst [zabezpečení samostatné clusteru v systému Windows pomocí zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md)

## <a name="client-to-node-security"></a>Uzel Klient zabezpečení
Ověřuje klienty, kteří a zabezpečuje komunikaci mezi klientem a jednotlivé uzly v clusteru hello. Tento typ zabezpečení ověřuje a zabezpečuje komunikaci klienta, který zajišťuje přístup jenom autorizovaným uživatelům hello clusteru a hello aplikace nasazené na clusteru hello. Klienti jsou jednoznačně identifikuje prostřednictvím jejich přihlašovacích údajů zabezpečení systému Windows nebo přihlašovacích certifikátů zabezpečení.

![Diagram komunikace klienta uzlu][Client-to-Node]

Clustery se systémem na Azure nebo samostatné clustery spuštěná v systému Windows můžete použít buď [zabezpečení Certificate](https://msdn.microsoft.com/library/ff649801.aspx) nebo [zabezpečení systému Windows](https://msdn.microsoft.com/library/ff649396.aspx).

### <a name="client-to-node-certificate-security"></a>Certifikát klienta uzel zabezpečení
 Při vytváření clusteru hello buď prostřednictvím hello portálu Azure, šablony Resource Manageru nebo šablonu JSON samostatné zadáním certifikát klienta správce nebo klientský certifikát uživatele konfigurace uzlu klientů certifikát zabezpečení.  Hello klientů a uživatelů klientské certifikáty správce zadáte by měla být jiná než hello primární a sekundární certifikáty zadáte pro [-uzly zabezpečení](#node-to-node-security) jako osvědčený postup. Ve výchozím nastavení certifikáty clusteru hello zabezpečení – uzly jsou přidány toohello povolené klienta seznamu Správce certifikátů.

Klienti připojení toohello clusteru pomocí Správce certifikátů hello mají plný přístup toomanagement funkce.  Klienti připojení clusteru toohello pomocí hello jen pro čtení uživatele klientský certifikát mají jenom funkce, toomanagement přístup pro čtení. Jinými slovy tyto certifikáty se používají pro hello základů řízení přístupu rolí (RBAC) popsanou dále v tomto článku.

Pro čtení Azure [nastavení clusteru pomocí šablony Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) toolearn jak tooconfigure certifikátu zabezpečení v clusteru.

Pro samostatnou službu Windows Server číst [zabezpečení samostatné clusteru v systému Windows pomocí certifikátů X.509](service-fabric-windows-cluster-x509-security.md)

### <a name="client-to-node-azure-active-directory-aad-security-on-azure"></a>Uzel Klient zabezpečení Azure Active Directory (AAD) v Azure
Clustery se systémem na platformě Azure také můžete zabezpečit přístup koncových bodů pro správu toohello pomocí Azure Active Directory (AAD). V tématu [nastavení clusteru pomocí šablony Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) informace o tom, jak toocreate hello artefakty potřebné AAD, jak toopopulate je během clusteru vytváření a jak tooconnect toothose clusterů později.

## <a name="security-recommendations"></a>Doporučení zabezpečení
Pro Azure clusterů se doporučuje použít AAD zabezpečení tooauthenticate klientů a certifikáty pro zabezpečení mezi uzly.

Samostatný Windows Server clustery se doporučuje používat zabezpečení systému Windows se skupiny účtech spravované (GMA) Pokud máte systém Windows Server 2012 R2 a služby Active Directory. Zabezpečení systému Windows stále jinak pomocí účtů systému Windows.

## <a name="role-based-access-control-rbac"></a>Řízení přístupu na základě role (RBAC)
Řízení přístupu umožňuje hello toolimit clusteru správce přístup toocertain operace na clusteru pro různé skupiny uživatelů, lepší zabezpečení hello clusteru. Podporuje dva typy různý přístup řízení pro klienty připojení clusteru tooa: role správce a role uživatele.

Správci mají plný přístup toomanagement možnosti (včetně možnosti pro čtení i zápis). Uživatelé, ve výchozím nastavení, mají pouze možnosti toomanagement přístup pro čtení (například použití dotazů) a hello možnost tooresolve aplikací a služeb.

Zadáte hello správce a uživatelských rolí klienta v době vytváření clusteru hello tím, že poskytuje samostatné identity (certifikáty, AAD atd.) pro každý. Další informace o nastavení řízení přístupu výchozí hello a jak toochange hello výchozí nastavení, najdete v části [řízení přístupu podle rolí pro Service Fabric klienty](service-fabric-cluster-security-roles.md).

## <a name="x509-certificates-and-service-fabric"></a>X.509 – certifikáty a Service Fabric
Digitální certifikáty X.509 jsou běžně používané tooauthenticate klienty a servery a tooencrypt a digitálnímu podepisování zpráv. Další informace o tyto certifikáty, přejděte příliš[práce s certifikáty](http://msdn.microsoft.com/library/ms731899.aspx).

Tooconsider některé důležité věci:

* Certifikáty používané v clusterech spuštění úlohy v produkčním prostředí by měla být vytvořen pomocí správně nakonfigurovaných certifikát služby Windows Server nebo získané z schválené [certifikační autoritou (CA)](https://en.wikipedia.org/wiki/Certificate_authority).
* Nikdy nepoužívejte žádné dočasných nebo testovací certifikáty v produkčním prostředí, které jsou vytvořeny pomocí nástrojů, jako je MakeCert.exe.
* Můžete použít certifikát podepsaný svým držitelem, ale měli tak učinit pouze pro testovací clustery a ne v produkčním prostředí.

### <a name="server-x509-certificates"></a>Certifikáty X.509 serveru
Certifikáty serveru mají hello primární úlohy ověřování tooclients serveru (uzel) nebo ověřování (uzel) tooa serveru (uzel). Jeden z hello počáteční kontroly, když se klient nebo uzel ověřuje uzlu je hodnota hello toocheck hello běžný název v poli subjektu hello. Tento běžný název nebo jeden z alternativní názvy subjektu hello certifikáty musí být součástí hello seznam povolených běžných názvů.

Hello následující článek popisuje, jak toogenerate certifikáty s alternativní názvy subjektu (SAN): [jak zabezpečit tooadd tooa alternativní název subjektu certifikátu LDAP](http://support.microsoft.com/kb/931351).

pole Subject Hello může obsahovat několik hodnot, každý s předponou typ inicializace hello tooindicate hodnotu. Nejčastěji inicializace hello je "CN" pro běžný název; například "CN = www.contoso.com". Je také možné pro toobe pole Předmět hello prázdné. Pokud se vyplní hello volitelné pole alternativní název subjektu, musí obsahovat hello běžný název certifikátu hello a jeden záznam za alternativní název subjektu. Tyto jsou vloženy hodnoty názvu DNS.

Hello hodnotu pole určená pro účely hello hello certifikátu by měla obsahovat odpovídající hodnotu, jako je například "Ověřování serveru" nebo "Ověření klienta".

### <a name="client-x509-certificates"></a>Klientské certifikáty X.509
Klientské certifikáty nejsou obvykle vystavované certifikační autority. Místo toho hello osobní úložiště hello aktuální umístění uživatele obvykle obsahuje klientské certifikáty umísťují kořenovou autoritou, s zamýšlený účel "Ověření klienta". Hello klienta můžete použít tento certifikát, pokud se vyžaduje vzájemné ověření.

> [!NOTE]
> Všechny operace správy na cluster Service Fabric vyžadují certifikáty serveru. Klientské certifikáty nelze použít pro správu.
> 
> 

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->


## <a name="next-steps"></a>Další kroky
Tento článek obsahuje koncepční informace o zabezpečení clusteru. Další,


1.  [Vytvoření clusteru s podporou v Azure pomocí šablony Resource Manageru](service-fabric-cluster-creation-via-arm.md) 
2.  [Portál Azure](service-fabric-cluster-creation-via-portal.md).

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
