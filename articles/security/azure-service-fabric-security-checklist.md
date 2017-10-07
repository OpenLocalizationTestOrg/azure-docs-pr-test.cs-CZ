---
title: "kontrolní seznam zabezpečení aaaAzure service fabric | Microsoft Docs"
description: "Tento článek obsahuje sadu kontrolní seznam pro zabezpečení zabezpečení prostředků infrastruktury Azure."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.openlocfilehash: 10ffaea9e7e4de6d758b0a57a79e269c87bfd14d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-security-checklist"></a>Kontrolní seznam zabezpečení Azure Service Fabric
Tento článek poskytuje snadno použitelnou kontrolní seznam, který vám pomůže zabezpečit vaše prostředí Azure Service Fabric.

## <a name="introduction"></a>Úvod
Azure Service Fabric je platforma distribuovaných systémů, která umožňuje snadno toopackage, nasadit a spravovat škálovatelného a spolehlivého mikroslužeb. Service Fabric také řeší hello významné problémy ve vývoji a správě cloudových aplikací. Vývojáři a správci se můžou vyhnout problémům se složitou infrastrukturou a místo toho se soustředit na implementaci zásadních a náročných úloh, které jsou škálovatelné, spolehlivé a spravovatelné.

## <a name="checklist"></a>Kontrolní seznam
Použijte následující kontrolní seznam toohelp, ujistěte se, že nebyly přehlížen jakékoli závažné potíže v Správa a konfigurace zabezpečeného řešení Azure Service Fabric hello.


|Kontrolní seznam kategorie| Popis |
| ------------ | -------- |
|[Řízení přístupu na základě role (RBAC)](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security-roles) | <ul><li>Řízení přístupu umožňuje hello toolimit clusteru správce přístup toocertain operace na clusteru pro různé skupiny uživatelů, lepší zabezpečení hello clusteru.</li><li>Správci mají plný přístup toomanagement možnosti (včetně možnosti pro čtení i zápis). </li><li>   Uživatelé, ve výchozím nastavení, mají pouze možnosti toomanagement přístup pro čtení (například použití dotazů) a hello možnost tooresolve aplikací a služeb.</li></ul>|
|[X.509 – certifikáty a Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security) | <ul><li>[Certifikáty](https://docs.microsoft.com/en-us/dotnet/framework/wcf/feature-details/working-with-certificates) použít v clusterech spuštěné úlohy v produkčním prostředí by měla být vytvořen pomocí správně nakonfigurovaných certifikát služby Windows Server nebo získané z schválené [certifikační autoritou (CA)](https://en.wikipedia.org/wiki/Certificate_authority).</li><li>Nikdy používat [dočasné nebo testovací certifikáty](https://docs.microsoft.com/en-us/dotnet/framework/wcf/feature-details/how-to-create-temporary-certificates-for-use-during-development) v produkčním prostředí, které jsou vytvořené s nástroji, jako [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968.aspx). </li><li>Můžete použít [certifikát podepsaný svým držitelem](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-x509-security) ale by tak učinit pouze pro testovací clustery a ne v produkčním prostředí.</li></ul>|
|[Zabezpečení clusteru](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security) | <ul><li>scénáře zabezpečení clusteru Hello patří mezi uzly zabezpečení, zabezpečení klienta k uzlu, [řízení přístupu na základě Role (RBAC)](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-security-roles).</li></ul>|
|[Ověření clusteru](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm) | <ul><li>Ověřuje [komunikaci mezi uzly](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/service-fabric/service-fabric-cluster-security.md) pro federaci clusteru. </li></ul>|
|[Ověření serveru](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm) | <ul><li>Ověřuje hello [koncových bodů pro správu clusteru](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-portal) tooa klienta pro správu.</li></ul>|
|[Zabezpečení aplikací](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm)| <ul><li>Šifrování a dešifrování hodnot konfigurace aplikace.</li><li> Šifrování dat mezi uzly během replikace.</li></ul>|
|[Certifikát clusteru](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-x509-security) | <ul><li>Tento certifikát se vyžaduje toosecure hello komunikaci mezi hello uzly v clusteru.</li><li>  Nastavte hello kryptografický otisk hello primární certifikát v hello části kryptografický otisk a který hello sekundární v proměnné ThumbprintSecondary hello.</li></ul>|
|[ServerCertificate](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-windows-cluster-x509-security)| <ul><li>Tento certifikát se zobrazí toohello klienta, když se ho pokusí tooconnect toothis clusteru. Dva certifikáty jiný server, primární a sekundární můžete použít pro upgrade.</li></ul>|
|ClientCertificateThumbprints| <ul><li>To je sada certifikáty, které chcete tooinstall na klientech hello ověření. </li></ul>|
|ClientCertificateCommonNames| <ul><li>Nastavit hello běžný název hello první klientský certifikát pro hello CertificateCommonName. Hello CertificateIssuerThumbprint je hello kryptografický otisk pro hello vystavitele tohoto certifikátu. </li></ul>|
|ReverseProxyCertificate| <ul><li>Toto je volitelné certifikát, který může být zadán, pokud chcete, aby toosecure vaše [Reverse Proxy](https://docs.microsoft.com/en-in/azure/service-fabric/service-fabric-reverseproxy). </li></ul>|
|Key Vault| <ul><li>Toomanage certifikáty se používají pro clusterů Service Fabric v Azure.  </li></ul>|


## <a name="next-steps"></a>Další kroky
- [Proces upgradu Service Fabric Cluster a očekávání od vás](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-upgrade)
- [Správu aplikací Service Fabric v sadě Visual Studio](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-manage-application-in-visual-studio).
- [Stav služby Fabric modelu ÚVOD](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-health-introduction).
