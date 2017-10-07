---
title: "aaaAzure Service Fabric kontejneru zabezpečení | Microsoft Docs"
description: "Přečtěte si teď toosecure služby kontejneru."
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
ms.openlocfilehash: 88faf4e8f949c2f7743756b6272ca672d9710630
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="container-security"></a>Kontejner zabezpečení

Service Fabric poskytuje mechanismus pro služby uvnitř kontejneru tooaccess certifikát, který je nainstalován na hello uzlů v clusteru systému Windows nebo Linux (verze 5.7 nebo vyšší). Kromě toho Service Fabric také podporuje gMSA (skupinové účty spravované služby) pro Windows kontejnery. 

## <a name="certificate-management-for-containers"></a>Správa certifikátů pro kontejnery

Zadáním certifikát můžete zabezpečit vaše služby kontejneru. Hello certifikát musí být nainstalovaný na hello uzly clusteru hello. Hello certifikát informace jsou uvedené v manifestu aplikace hello pod hello `ContainerHostPolicies` značce jako hello následující fragment kódu ukazuje:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

Při spouštění aplikace hello, hello runtime přečte hello certifikáty a vygeneruje soubor PFX a heslo pro každý certifikát. Tento soubor PFX a heslo jsou přístupné uvnitř kontejneru hello pomocí hello následující proměnné prostředí: 

* **_PFX _ [CertName] Certificate_ [CodePackageName]**
* **_Password _ [CertName] Certificate_ [CodePackageName]**

pro import souboru PFX hello do kontejneru hello zodpovídá Hello kontejneru služby nebo proces. tooimport hello certifikátu, můžete použít `setupentrypoint.sh` skriptů nebo provést vlastní kód v rámci procesu kontejneru hello. Následuje ukázkový kód v jazyce C# pro import souboru PFX hello:

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
Tento certifikát PFX, který lze ověřit hello aplikace nebo služby nebo zabezpečený commmunication s jinými službami.


## <a name="set-up-gmsa-for-windows-containers"></a>Nastavit gMSA pro kontejnery Windows

tooset až gMSA (skupina účty spravované služby), specifikace soubor přihlašovacích údajů (`credspec`) je umístěn ve všech uzlech v clusteru hello. soubor Hello lze zkopírovat na všech uzlech pomocí rozšíření virtuálního počítače.  Hello `credspec` soubor musí obsahovat informace o účtu gMSA hello. Další informace o hello `credspec` souborů najdete v tématu [účty služby](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts). Specifikace Hello přihlašovacích údajů a hello `Hostname` značky jsou určené v manifestu aplikace hello. Hello `Hostname` značky musí shodovat s názvem účtu gMSA hello, který hello kontejneru běží pod.  Hello `Hostname` značka umožňuje tooauthenticate hello kontejneru, samotné tooother služby v doméně hello pomocí ověřování protokolem Kerberos.  Ukázka pro zadání hello `Hostname` a hello `credspec` v hello manifest aplikace se zobrazí v hello následující fragment kódu:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="next-steps"></a>Další kroky

* [Nasazení tooService kontejneru Windows Fabric na Windows Server 2016](service-fabric-get-started-containers.md)
* [Nasazení tooService kontejner Docker prostředků infrastruktury v systému Linux](service-fabric-get-started-containers-linux.md)
