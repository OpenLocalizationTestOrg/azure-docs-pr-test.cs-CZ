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
# <a name="container-security"></a><span data-ttu-id="db504-103">Kontejner zabezpečení</span><span class="sxs-lookup"><span data-stu-id="db504-103">Container security</span></span>

<span data-ttu-id="db504-104">Service Fabric poskytuje mechanismus pro služby uvnitř kontejneru tooaccess certifikát, který je nainstalován na hello uzlů v clusteru systému Windows nebo Linux (verze 5.7 nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="db504-104">Service Fabric provides a mechanism for services inside a container tooaccess a certificate that is installed on hello nodes in a Windows or Linux cluster (version 5.7 or higher).</span></span> <span data-ttu-id="db504-105">Kromě toho Service Fabric také podporuje gMSA (skupinové účty spravované služby) pro Windows kontejnery.</span><span class="sxs-lookup"><span data-stu-id="db504-105">In addition, Service Fabric also supports gMSA (group Managed Service Accounts) for Windows containers.</span></span> 

## <a name="certificate-management-for-containers"></a><span data-ttu-id="db504-106">Správa certifikátů pro kontejnery</span><span class="sxs-lookup"><span data-stu-id="db504-106">Certificate management for containers</span></span>

<span data-ttu-id="db504-107">Zadáním certifikát můžete zabezpečit vaše služby kontejneru.</span><span class="sxs-lookup"><span data-stu-id="db504-107">You can secure your container services by specifying a certificate.</span></span> <span data-ttu-id="db504-108">Hello certifikát musí být nainstalovaný na hello uzly clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="db504-108">hello certificate must be installed on hello nodes of hello cluster.</span></span> <span data-ttu-id="db504-109">Hello certifikát informace jsou uvedené v manifestu aplikace hello pod hello `ContainerHostPolicies` značce jako hello následující fragment kódu ukazuje:</span><span class="sxs-lookup"><span data-stu-id="db504-109">hello certificate information is provided in hello application manifest under hello `ContainerHostPolicies` tag as hello following snippet shows:</span></span>

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

<span data-ttu-id="db504-110">Při spouštění aplikace hello, hello runtime přečte hello certifikáty a vygeneruje soubor PFX a heslo pro každý certifikát.</span><span class="sxs-lookup"><span data-stu-id="db504-110">When starting hello application, hello runtime reads hello certificates and generates a PFX file and password for each certificate.</span></span> <span data-ttu-id="db504-111">Tento soubor PFX a heslo jsou přístupné uvnitř kontejneru hello pomocí hello následující proměnné prostředí:</span><span class="sxs-lookup"><span data-stu-id="db504-111">This PFX file and password are accessible inside hello container using hello following environment variables:</span></span> 

* <span data-ttu-id="db504-112">**_PFX _ [CertName] Certificate_ [CodePackageName]**</span><span class="sxs-lookup"><span data-stu-id="db504-112">**Certificate_[CodePackageName]_[CertName]_PFX**</span></span>
* <span data-ttu-id="db504-113">**_Password _ [CertName] Certificate_ [CodePackageName]**</span><span class="sxs-lookup"><span data-stu-id="db504-113">**Certificate_[CodePackageName]_[CertName]_Password**</span></span>

<span data-ttu-id="db504-114">pro import souboru PFX hello do kontejneru hello zodpovídá Hello kontejneru služby nebo proces.</span><span class="sxs-lookup"><span data-stu-id="db504-114">hello container service or process is responsible for importing hello PFX file into hello container.</span></span> <span data-ttu-id="db504-115">tooimport hello certifikátu, můžete použít `setupentrypoint.sh` skriptů nebo provést vlastní kód v rámci procesu kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="db504-115">tooimport hello certificate, you can use `setupentrypoint.sh` scripts or executed custom code within hello container process.</span></span> <span data-ttu-id="db504-116">Následuje ukázkový kód v jazyce C# pro import souboru PFX hello:</span><span class="sxs-lookup"><span data-stu-id="db504-116">Sample code in C# for importing hello PFX file follows:</span></span>

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
<span data-ttu-id="db504-117">Tento certifikát PFX, který lze ověřit hello aplikace nebo služby nebo zabezpečený commmunication s jinými službami.</span><span class="sxs-lookup"><span data-stu-id="db504-117">This PFX certificate can be used for authenticate hello application or service or secure commmunication with other services.</span></span>


## <a name="set-up-gmsa-for-windows-containers"></a><span data-ttu-id="db504-118">Nastavit gMSA pro kontejnery Windows</span><span class="sxs-lookup"><span data-stu-id="db504-118">Set up gMSA for Windows containers</span></span>

<span data-ttu-id="db504-119">tooset až gMSA (skupina účty spravované služby), specifikace soubor přihlašovacích údajů (`credspec`) je umístěn ve všech uzlech v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="db504-119">tooset up gMSA (group Managed Service Accounts), a credential specification file (`credspec`) is placed on all nodes in hello cluster.</span></span> <span data-ttu-id="db504-120">soubor Hello lze zkopírovat na všech uzlech pomocí rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="db504-120">hello file can be copied on all nodes using a VM extension.</span></span>  <span data-ttu-id="db504-121">Hello `credspec` soubor musí obsahovat informace o účtu gMSA hello.</span><span class="sxs-lookup"><span data-stu-id="db504-121">hello `credspec` file must contain hello gMSA account information.</span></span> <span data-ttu-id="db504-122">Další informace o hello `credspec` souborů najdete v tématu [účty služby](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts).</span><span class="sxs-lookup"><span data-stu-id="db504-122">For more information on hello `credspec` file, see [Service Accounts](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts).</span></span> <span data-ttu-id="db504-123">Specifikace Hello přihlašovacích údajů a hello `Hostname` značky jsou určené v manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="db504-123">hello credential specification and hello `Hostname` tag are specified in hello application manifest.</span></span> <span data-ttu-id="db504-124">Hello `Hostname` značky musí shodovat s názvem účtu gMSA hello, který hello kontejneru běží pod.</span><span class="sxs-lookup"><span data-stu-id="db504-124">hello `Hostname` tag must match hello gMSA account name that hello container runs under.</span></span>  <span data-ttu-id="db504-125">Hello `Hostname` značka umožňuje tooauthenticate hello kontejneru, samotné tooother služby v doméně hello pomocí ověřování protokolem Kerberos.</span><span class="sxs-lookup"><span data-stu-id="db504-125">hello `Hostname` tag allows hello container tooauthenticate itself tooother services in hello domain using Kerberos authentication.</span></span>  <span data-ttu-id="db504-126">Ukázka pro zadání hello `Hostname` a hello `credspec` v hello manifest aplikace se zobrazí v hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="db504-126">A sample for specifying hello `Hostname` and hello `credspec` in hello application manifest is shown in hello following snippet:</span></span>

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="next-steps"></a><span data-ttu-id="db504-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db504-127">Next steps</span></span>

* [<span data-ttu-id="db504-128">Nasazení tooService kontejneru Windows Fabric na Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="db504-128">Deploy a Windows container tooService Fabric on Windows Server 2016</span></span>](service-fabric-get-started-containers.md)
* [<span data-ttu-id="db504-129">Nasazení tooService kontejner Docker prostředků infrastruktury v systému Linux</span><span class="sxs-lookup"><span data-stu-id="db504-129">Deploy a Docker container tooService Fabric on Linux</span></span>](service-fabric-get-started-containers-linux.md)
