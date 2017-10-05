---
title: "Zabezpečení Azure container Service Fabric | Microsoft Docs"
description: "Naučte se zabezpečení služby kontejneru."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 75faca1e827a0eca6b97adcb2e1c6ca72b3364d6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="container-security"></a>Kontejner zabezpečení

Service Fabric poskytuje mechanismus pro služby uvnitř kontejneru pro přístup k certifikátu, který je nainstalován na uzly v clusteru systému Windows nebo Linux (verze 5.7 nebo vyšší). Kromě toho Service Fabric také podporuje gMSA (skupinové účty spravované služby) pro Windows kontejnery. 

## <a name="certificate-management-for-containers"></a>Správa certifikátů pro kontejnery

Zadáním certifikát můžete zabezpečit vaše služby kontejneru. Certifikát musí být nainstalován na uzlech clusteru. Informace o certifikátu je součástí manifest aplikace v rámci `ContainerHostPolicies` značce jako následující fragment kódu ukazuje:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

Při spouštění aplikace, modulu runtime přečte certifikáty a vygeneruje soubor PFX a heslo pro každý certifikát. Tento soubor PFX a heslo jsou přístupné uvnitř kontejneru pomocí následující proměnné prostředí: 

* **_PFX _ [CertName] Certificate_ [CodePackageName]**
* **_Password _ [CertName] Certificate_ [CodePackageName]**

Službu kontejneru nebo procesu je zodpovědná za importu souboru PFX do kontejneru. Chcete-li import certifikátu, můžete použít `setupentrypoint.sh` skriptů nebo provést vlastní kód v rámci procesu kontejneru. Následuje ukázkový kód v jazyce C# pro import souboru PFX:

```c#
    string certificateFilePath = Environment.GetEnvironmentVariable("Certificate_NodeContainerService.Code_MyCert1_PFX");
    string passwordFilePath = Environment.GetEnvironmentVariable("Certificate_NodeContainerService.Code_MyCert1_Password");
    X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
    string password = File.ReadAllLines(passwordFilePath, Encoding.Default)[0];
    password = password.Replace("\0", string.Empty);
    X509Certificate2 cert = new X509Certificate2(certificateFilePath, password, X509KeyStorageFlags.MachineKeySet | X509KeyStorageFlags.PersistKeySet);
    store.Open(OpenFlags.ReadWrite);
    store.Add(cert);
    store.Close();
```
Tento certifikát PFX, který lze použít pro ověření aplikace nebo služby nebo zabezpečený commmunication s jinými službami.


## <a name="set-up-gmsa-for-windows-containers"></a>Nastavit gMSA pro kontejnery Windows

Nastavit gMSA (skupina účty spravované služby), specifikace soubor přihlašovacích údajů (`credspec`) je umístěn na všech uzlech v clusteru. Soubor můžete zkopírovat na všech uzlech pomocí rozšíření virtuálního počítače.  `credspec` Soubor musí obsahovat informace o účtu gMSA. Další informace o `credspec` souborů najdete v tématu [účty služby](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts). Specifikace přihlašovacích údajů a `Hostname` značky jsou určené v manifestu aplikace. `Hostname` Značky musí odpovídat názvu účtu gMSA kompatibilní se kontejneru.  `Hostname` Značka umožňuje kontejneru ke svému ověření do dalších služeb v doméně pomocí ověřování protokolem Kerberos.  Ukázka pro zadání `Hostname` a `credspec` v aplikaci se zobrazí manifestu v následující fragment kódu:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="next-steps"></a>Další kroky

* [Nasazení kontejneru systému Windows pro Service Fabric na Windows Server 2016](service-fabric-get-started-containers.md)
* [Nasadit kontejner Docker do Service Fabric v systému Linux](service-fabric-get-started-containers-linux.md)
