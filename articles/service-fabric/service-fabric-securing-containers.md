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
# <a name="container-security"></a><span data-ttu-id="94506-103">Kontejner zabezpečení</span><span class="sxs-lookup"><span data-stu-id="94506-103">Container security</span></span>

<span data-ttu-id="94506-104">Service Fabric poskytuje mechanismus pro služby uvnitř kontejneru pro přístup k certifikátu, který je nainstalován na uzly v clusteru systému Windows nebo Linux (verze 5.7 nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="94506-104">Service Fabric provides a mechanism for services inside a container to access a certificate that is installed on the nodes in a Windows or Linux cluster (version 5.7 or higher).</span></span> <span data-ttu-id="94506-105">Kromě toho Service Fabric také podporuje gMSA (skupinové účty spravované služby) pro Windows kontejnery.</span><span class="sxs-lookup"><span data-stu-id="94506-105">In addition, Service Fabric also supports gMSA (group Managed Service Accounts) for Windows containers.</span></span> 

## <a name="certificate-management-for-containers"></a><span data-ttu-id="94506-106">Správa certifikátů pro kontejnery</span><span class="sxs-lookup"><span data-stu-id="94506-106">Certificate management for containers</span></span>

<span data-ttu-id="94506-107">Zadáním certifikát můžete zabezpečit vaše služby kontejneru.</span><span class="sxs-lookup"><span data-stu-id="94506-107">You can secure your container services by specifying a certificate.</span></span> <span data-ttu-id="94506-108">Certifikát musí být nainstalován na uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="94506-108">The certificate must be installed on the nodes of the cluster.</span></span> <span data-ttu-id="94506-109">Informace o certifikátu je součástí manifest aplikace v rámci `ContainerHostPolicies` značce jako následující fragment kódu ukazuje:</span><span class="sxs-lookup"><span data-stu-id="94506-109">The certificate information is provided in the application manifest under the `ContainerHostPolicies` tag as the following snippet shows:</span></span>

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

<span data-ttu-id="94506-110">Při spouštění aplikace, modulu runtime přečte certifikáty a vygeneruje soubor PFX a heslo pro každý certifikát.</span><span class="sxs-lookup"><span data-stu-id="94506-110">When starting the application, the runtime reads the certificates and generates a PFX file and password for each certificate.</span></span> <span data-ttu-id="94506-111">Tento soubor PFX a heslo jsou přístupné uvnitř kontejneru pomocí následující proměnné prostředí:</span><span class="sxs-lookup"><span data-stu-id="94506-111">This PFX file and password are accessible inside the container using the following environment variables:</span></span> 

* <span data-ttu-id="94506-112">**_PFX _ [CertName] Certificate_ [CodePackageName]**</span><span class="sxs-lookup"><span data-stu-id="94506-112">**Certificate_[CodePackageName]_[CertName]_PFX**</span></span>
* <span data-ttu-id="94506-113">**_Password _ [CertName] Certificate_ [CodePackageName]**</span><span class="sxs-lookup"><span data-stu-id="94506-113">**Certificate_[CodePackageName]_[CertName]_Password**</span></span>

<span data-ttu-id="94506-114">Službu kontejneru nebo procesu je zodpovědná za importu souboru PFX do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="94506-114">The container service or process is responsible for importing the PFX file into the container.</span></span> <span data-ttu-id="94506-115">Chcete-li import certifikátu, můžete použít `setupentrypoint.sh` skriptů nebo provést vlastní kód v rámci procesu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="94506-115">To import the certificate, you can use `setupentrypoint.sh` scripts or executed custom code within the container process.</span></span> <span data-ttu-id="94506-116">Následuje ukázkový kód v jazyce C# pro import souboru PFX:</span><span class="sxs-lookup"><span data-stu-id="94506-116">Sample code in C# for importing the PFX file follows:</span></span>

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
<span data-ttu-id="94506-117">Tento certifikát PFX, který lze použít pro ověření aplikace nebo služby nebo zabezpečený commmunication s jinými službami.</span><span class="sxs-lookup"><span data-stu-id="94506-117">This PFX certificate can be used for authenticate the application or service or secure commmunication with other services.</span></span>


## <a name="set-up-gmsa-for-windows-containers"></a><span data-ttu-id="94506-118">Nastavit gMSA pro kontejnery Windows</span><span class="sxs-lookup"><span data-stu-id="94506-118">Set up gMSA for Windows containers</span></span>

<span data-ttu-id="94506-119">Nastavit gMSA (skupina účty spravované služby), specifikace soubor přihlašovacích údajů (`credspec`) je umístěn na všech uzlech v clusteru.</span><span class="sxs-lookup"><span data-stu-id="94506-119">To set up gMSA (group Managed Service Accounts), a credential specification file (`credspec`) is placed on all nodes in the cluster.</span></span> <span data-ttu-id="94506-120">Soubor můžete zkopírovat na všech uzlech pomocí rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="94506-120">The file can be copied on all nodes using a VM extension.</span></span>  <span data-ttu-id="94506-121">`credspec` Soubor musí obsahovat informace o účtu gMSA.</span><span class="sxs-lookup"><span data-stu-id="94506-121">The `credspec` file must contain the gMSA account information.</span></span> <span data-ttu-id="94506-122">Další informace o `credspec` souborů najdete v tématu [účty služby](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts).</span><span class="sxs-lookup"><span data-stu-id="94506-122">For more information on the `credspec` file, see [Service Accounts](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts).</span></span> <span data-ttu-id="94506-123">Specifikace přihlašovacích údajů a `Hostname` značky jsou určené v manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="94506-123">The credential specification and the `Hostname` tag are specified in the application manifest.</span></span> <span data-ttu-id="94506-124">`Hostname` Značky musí odpovídat názvu účtu gMSA kompatibilní se kontejneru.</span><span class="sxs-lookup"><span data-stu-id="94506-124">The `Hostname` tag must match the gMSA account name that the container runs under.</span></span>  <span data-ttu-id="94506-125">`Hostname` Značka umožňuje kontejneru ke svému ověření do dalších služeb v doméně pomocí ověřování protokolem Kerberos.</span><span class="sxs-lookup"><span data-stu-id="94506-125">The `Hostname` tag allows the container to authenticate itself to other services in the domain using Kerberos authentication.</span></span>  <span data-ttu-id="94506-126">Ukázka pro zadání `Hostname` a `credspec` v aplikaci se zobrazí manifestu v následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="94506-126">A sample for specifying the `Hostname` and the `credspec` in the application manifest is shown in the following snippet:</span></span>

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="next-steps"></a><span data-ttu-id="94506-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="94506-127">Next steps</span></span>

* [<span data-ttu-id="94506-128">Nasazení kontejneru systému Windows pro Service Fabric na Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="94506-128">Deploy a Windows container to Service Fabric on Windows Server 2016</span></span>](service-fabric-get-started-containers.md)
* [<span data-ttu-id="94506-129">Nasadit kontejner Docker do Service Fabric v systému Linux</span><span class="sxs-lookup"><span data-stu-id="94506-129">Deploy a Docker container to Service Fabric on Linux</span></span>](service-fabric-get-started-containers-linux.md)
