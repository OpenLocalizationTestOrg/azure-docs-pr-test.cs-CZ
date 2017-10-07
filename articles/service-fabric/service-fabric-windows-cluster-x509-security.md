---
title: "aaaSecure Azure Service Fabric clusteru v systému Windows pomocí certifikátů | Microsoft Docs"
description: "Tento článek popisuje, jak toosecure komunikaci v rámci hello samostatné, nebo privátní clusteru také mezi klienty a hello clusteru."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: fe0ed74c-9af5-44e9-8d62-faf1849af68c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dekapur
ms.openlocfilehash: f0d411963615349a84edfc8125dec4ee5908f146
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a><span data-ttu-id="42d2b-103">Zabezpečení clusteru s podporou samostatné do systému Windows pomocí certifikátů X.509</span><span class="sxs-lookup"><span data-stu-id="42d2b-103">Secure a standalone cluster on Windows using X.509 certificates</span></span>
<span data-ttu-id="42d2b-104">Tento článek popisuje, jak toosecure hello komunikace mezi hello různé uzly clusteru Windows samostatné taky, jak tooauthenticate klienti připojení clusteru toothis pomocí certifikátů X.509.</span><span class="sxs-lookup"><span data-stu-id="42d2b-104">This article describes how toosecure hello communication between hello various nodes of your standalone Windows cluster, as well as how tooauthenticate clients connecting toothis cluster, using X.509 certificates.</span></span> <span data-ttu-id="42d2b-105">To zajišťuje, jenom Autorizovaní uživatelé mají přístup ke clusteru hello hello nasazené aplikace a provádět úlohy správy.</span><span class="sxs-lookup"><span data-stu-id="42d2b-105">This ensures that only authorized users can access hello cluster, hello deployed applications and perform management tasks.</span></span>  <span data-ttu-id="42d2b-106">Certifikát zabezpečení může být povoleno v clusteru hello při vytvoření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="42d2b-106">Certificate security should be enabled on hello cluster when hello cluster is created.</span></span>  

<span data-ttu-id="42d2b-107">Další informace o zabezpečení clusteru, jako jsou například – uzly zabezpečení, klientský uzel zabezpečení a řízení přístupu na základě rolí najdete v tématu [clusteru scénáře zabezpečení](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="42d2b-107">For more information on cluster security such as node-to-node security, client-to-node security, and role-based access control, see [Cluster security scenarios](service-fabric-cluster-security.md).</span></span>

## <a name="which-certificates-will-you-need"></a><span data-ttu-id="42d2b-108">Certifikáty, které budete potřebovat?</span><span class="sxs-lookup"><span data-stu-id="42d2b-108">Which certificates will you need?</span></span>
<span data-ttu-id="42d2b-109">toostart, [Stáhnout balíček clusteru samostatné hello](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) tooone hello uzlů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="42d2b-109">toostart with, [download hello standalone cluster package](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) tooone of hello nodes in your cluster.</span></span> <span data-ttu-id="42d2b-110">V hello stáhli balíček, najdete **ClusterConfig.X509.MultiMachine.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="42d2b-110">In hello downloaded package, you will find a **ClusterConfig.X509.MultiMachine.json** file.</span></span> <span data-ttu-id="42d2b-111">Otevřete soubor hello a projděte si část hello pro **zabezpečení** pod hello **vlastnosti** části:</span><span class="sxs-lookup"><span data-stu-id="42d2b-111">Open hello file and review hello section for **security** under hello **properties** section:</span></span>

```JSON
"security": {
    "metadata": "hello Credential type X509 indicates this is cluster is secured using X509 Certificates. hello thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
    "ClusterCredentialType": "X509",
    "ServerCredentialType": "X509",
    "CertificateInformation": {
        "ClusterCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },        
        "ClusterCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]"
            }
            ],
            "X509StoreName": "My"
        },
        "ServerCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ServerCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]"
            }
            ],
            "X509StoreName": "My"
        },
        "ClientCertificateThumbprints": [
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            },
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
        "ClientCertificateCommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
        "ReverseProxyCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ReverseProxyCertificateCommonNames": {
            "CommonNames": [
                {
                "CertificateCommonName": "[CertificateCommonName]"
                }
            ],
            "X509StoreName": "My"
        }
    }
},
```

<span data-ttu-id="42d2b-112">Tato část popisuje hello certifikáty, které je třeba pro zabezpečení samostatný cluster Windows.</span><span class="sxs-lookup"><span data-stu-id="42d2b-112">This section describes hello certificates that you need for securing your standalone Windows cluster.</span></span> <span data-ttu-id="42d2b-113">Pokud zadáte certifikát clusteru, nastavte hodnotu hello **ClusterCredentialType** too_**X509**_. Pokud zadáte certifikát serveru pro připojení mimo, nastavte hello **ServerCredentialType** příliš_**X509**_. I když není povinné, doporučujeme toohave oba tyto certifikáty pro cluster s podporou správně zabezpečené. Pokud nastavíte tyto hodnoty příliš*X509* pak musíte zadat také hello odpovídající certifikáty nebo Service Fabric vyvolá výjimku. V některých případech je vhodné pouze toospecify hello _ClientCertificateThumbprints_ nebo _ReverseProxyCertificate_. V těchto scénářích, není nutné nastavovat _ClusterCredentialType_ nebo _ServerCredentialType_ too_X509_.</span><span class="sxs-lookup"><span data-stu-id="42d2b-113">If you are specifying a cluster certificate, set hello value of **ClusterCredentialType** too_**X509**_. If you are specifying server certificate for outside connections, set hello **ServerCredentialType** too_**X509**_. Although not mandatory, we recommend toohave both these certificates for a properly secured cluster. If you set these values too*X509* then you must also specify hello corresponding certificates or Service Fabric will throw an exception. In some scenarios, you may only want toospecify hello _ClientCertificateThumbprints_ or _ReverseProxyCertificate_. In those scenarios, you need not set _ClusterCredentialType_ or _ServerCredentialType_ too_X509_.</span></span>


> [!NOTE]
> <span data-ttu-id="42d2b-114">A [kryptografický otisk](https://en.wikipedia.org/wiki/Public_key_fingerprint) je primární identita hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="42d2b-114">A [thumbprint](https://en.wikipedia.org/wiki/Public_key_fingerprint) is hello primary identity of a certificate.</span></span> <span data-ttu-id="42d2b-115">Čtení [jak tooretrieve kryptografického otisku certifikátu](https://msdn.microsoft.com/library/ms734695.aspx) toofind out hello kryptografický otisk hello certifikáty, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="42d2b-115">Read [How tooretrieve thumbprint of a certificate](https://msdn.microsoft.com/library/ms734695.aspx) toofind out hello thumbprint of hello certificates that you create.</span></span>
> 
> 

<span data-ttu-id="42d2b-116">Hello následující tabulka uvádí hello certifikáty, které budete potřebovat na vaše nastavení clusteru:</span><span class="sxs-lookup"><span data-stu-id="42d2b-116">hello following table lists hello certificates that you will need on your cluster setup:</span></span>

| <span data-ttu-id="42d2b-117">**Nastavení CertificateInformation**</span><span class="sxs-lookup"><span data-stu-id="42d2b-117">**CertificateInformation Setting**</span></span> | <span data-ttu-id="42d2b-118">**Popis**</span><span class="sxs-lookup"><span data-stu-id="42d2b-118">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="42d2b-119">ClusterCertificate</span><span class="sxs-lookup"><span data-stu-id="42d2b-119">ClusterCertificate</span></span> |<span data-ttu-id="42d2b-120">Vhodné pro testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="42d2b-120">Recommended for test environment.</span></span> <span data-ttu-id="42d2b-121">Tento certifikát se vyžaduje toosecure hello komunikaci mezi hello uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="42d2b-121">This certificate is required toosecure hello communication between hello nodes on a cluster.</span></span> <span data-ttu-id="42d2b-122">Můžete použít dvě různé certifikáty, primární a sekundární pro upgrade.</span><span class="sxs-lookup"><span data-stu-id="42d2b-122">You can use two different certificates, a primary and a secondary for upgrade.</span></span> <span data-ttu-id="42d2b-123">Nastavit hello kryptografický otisk certifikátu primární hello v hello **kryptografický otisk** části a který hello sekundární v hello **ThumbprintSecondary** proměnné.</span><span class="sxs-lookup"><span data-stu-id="42d2b-123">Set hello thumbprint of hello primary certificate in hello **Thumbprint** section and that of hello secondary in hello **ThumbprintSecondary** variables.</span></span> |
| <span data-ttu-id="42d2b-124">ClusterCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="42d2b-124">ClusterCertificateCommonNames</span></span> |<span data-ttu-id="42d2b-125">Nedoporučuje pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="42d2b-125">Recommended for production environment.</span></span> <span data-ttu-id="42d2b-126">Tento certifikát se vyžaduje toosecure hello komunikaci mezi hello uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="42d2b-126">This certificate is required toosecure hello communication between hello nodes on a cluster.</span></span> <span data-ttu-id="42d2b-127">Můžete použít jednu nebo dvě clusteru běžné názvy certifikátů.</span><span class="sxs-lookup"><span data-stu-id="42d2b-127">You can use one or two cluster certificate common names.</span></span> |
| <span data-ttu-id="42d2b-128">ServerCertificate</span><span class="sxs-lookup"><span data-stu-id="42d2b-128">ServerCertificate</span></span> |<span data-ttu-id="42d2b-129">Vhodné pro testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="42d2b-129">Recommended for test environment.</span></span> <span data-ttu-id="42d2b-130">Tento certifikát se zobrazí toohello klienta, když se ho pokusí tooconnect toothis clusteru.</span><span class="sxs-lookup"><span data-stu-id="42d2b-130">This certificate is presented toohello client when it tries tooconnect toothis cluster.</span></span> <span data-ttu-id="42d2b-131">Pro usnadnění práce, můžete toouse hello stejný certifikát pro *ClusterCertificate* a *ServerCertificate*.</span><span class="sxs-lookup"><span data-stu-id="42d2b-131">For convenience, you can choose toouse hello same certificate for *ClusterCertificate* and *ServerCertificate*.</span></span> <span data-ttu-id="42d2b-132">Dva certifikáty jiný server, primární a sekundární můžete použít pro upgrade.</span><span class="sxs-lookup"><span data-stu-id="42d2b-132">You can use two different server certificates, a primary and a secondary for upgrade.</span></span> <span data-ttu-id="42d2b-133">Nastavit hello kryptografický otisk certifikátu primární hello v hello **kryptografický otisk** části a který hello sekundární v hello **ThumbprintSecondary** proměnné.</span><span class="sxs-lookup"><span data-stu-id="42d2b-133">Set hello thumbprint of hello primary certificate in hello **Thumbprint** section and that of hello secondary in hello **ThumbprintSecondary** variables.</span></span> |
| <span data-ttu-id="42d2b-134">ServerCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="42d2b-134">ServerCertificateCommonNames</span></span> |<span data-ttu-id="42d2b-135">Nedoporučuje pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="42d2b-135">Recommended for production environment.</span></span> <span data-ttu-id="42d2b-136">Tento certifikát se zobrazí toohello klienta, když se ho pokusí tooconnect toothis clusteru.</span><span class="sxs-lookup"><span data-stu-id="42d2b-136">This certificate is presented toohello client when it tries tooconnect toothis cluster.</span></span> <span data-ttu-id="42d2b-137">Pro usnadnění práce, můžete toouse hello stejný certifikát pro *ClusterCertificateCommonNames* a *ServerCertificateCommonNames*.</span><span class="sxs-lookup"><span data-stu-id="42d2b-137">For convenience, you can choose toouse hello same certificate for *ClusterCertificateCommonNames* and *ServerCertificateCommonNames*.</span></span> <span data-ttu-id="42d2b-138">Můžete použít jednu nebo dvě certifikát běžné názvy serverů.</span><span class="sxs-lookup"><span data-stu-id="42d2b-138">You can use one or two server certificate common names.</span></span> |
| <span data-ttu-id="42d2b-139">ClientCertificateThumbprints</span><span class="sxs-lookup"><span data-stu-id="42d2b-139">ClientCertificateThumbprints</span></span> |<span data-ttu-id="42d2b-140">To je sada certifikáty, které chcete tooinstall na klientech hello ověření.</span><span class="sxs-lookup"><span data-stu-id="42d2b-140">This is a set of certificates that you want tooinstall on hello authenticated clients.</span></span> <span data-ttu-id="42d2b-141">Může mít řadu různých klientských certifikátů nainstalovaných na hello počítače, které chcete clusteru toohello tooallow přístupu.</span><span class="sxs-lookup"><span data-stu-id="42d2b-141">You can have a number of different client certificates installed on hello machines that you want tooallow access toohello cluster.</span></span> <span data-ttu-id="42d2b-142">Nastaví kryptografický otisk hello každý certifikát v hello **CertificateThumbprint** proměnné.</span><span class="sxs-lookup"><span data-stu-id="42d2b-142">Set hello thumbprint of each certificate in hello **CertificateThumbprint** variable.</span></span> <span data-ttu-id="42d2b-143">Pokud nastavíte hello **IsAdmin** příliš*true*, a poté klienta hello se tento certifikát nainstalován můžete proveďte správce správy aktivit v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="42d2b-143">If you set hello **IsAdmin** too*true*, then hello client with this certificate installed on it can do administrator management activities on hello cluster.</span></span> <span data-ttu-id="42d2b-144">Pokud hello **IsAdmin** je *false*, hello klienta s tímto certifikátem můžete provádět jenom akce hello povolené pro uživatelská přístupová práva, obvykle jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="42d2b-144">If hello **IsAdmin** is *false*, hello client with this certificate can only perform hello actions allowed for user access rights, typically read-only.</span></span> <span data-ttu-id="42d2b-145">Další informace o rolích najdete v tématu [řízení přístupu (RBAC) na základě Role](service-fabric-cluster-security.md#role-based-access-control-rbac)</span><span class="sxs-lookup"><span data-stu-id="42d2b-145">For more information on roles read [Role based access control (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac)</span></span> |
| <span data-ttu-id="42d2b-146">ClientCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="42d2b-146">ClientCertificateCommonNames</span></span> |<span data-ttu-id="42d2b-147">Sada hello běžný název hello první klientský certifikát pro hello **CertificateCommonName**.</span><span class="sxs-lookup"><span data-stu-id="42d2b-147">Set hello common name of hello first client certificate for hello **CertificateCommonName**.</span></span> <span data-ttu-id="42d2b-148">Hello **CertificateIssuerThumbprint** je hello kryptografický otisk pro hello vystavitele tohoto certifikátu.</span><span class="sxs-lookup"><span data-stu-id="42d2b-148">hello **CertificateIssuerThumbprint** is hello thumbprint for hello issuer of this certificate.</span></span> <span data-ttu-id="42d2b-149">Čtení [práce s certifikáty](https://msdn.microsoft.com/library/ms731899.aspx) tooknow více informací o běžné názvy a hello vystavitele.</span><span class="sxs-lookup"><span data-stu-id="42d2b-149">Read [Working with certificates](https://msdn.microsoft.com/library/ms731899.aspx) tooknow more about common names and hello issuer.</span></span> |
| <span data-ttu-id="42d2b-150">ReverseProxyCertificate</span><span class="sxs-lookup"><span data-stu-id="42d2b-150">ReverseProxyCertificate</span></span> |<span data-ttu-id="42d2b-151">Vhodné pro testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="42d2b-151">Recommended for test environment.</span></span> <span data-ttu-id="42d2b-152">Toto je volitelné certifikát, který může být zadán, pokud chcete, aby toosecure vaše [Reverse Proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="42d2b-152">This is an optional certificate that can be specified if you want toosecure your [Reverse Proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="42d2b-153">Ujistěte se, že reverseProxyEndpointPort je nastaven v nodeTypes, pokud používáte tento certifikát.</span><span class="sxs-lookup"><span data-stu-id="42d2b-153">Make sure reverseProxyEndpointPort is set in nodeTypes if you are using this certificate.</span></span> |
| <span data-ttu-id="42d2b-154">ReverseProxyCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="42d2b-154">ReverseProxyCertificateCommonNames</span></span> |<span data-ttu-id="42d2b-155">Nedoporučuje pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="42d2b-155">Recommended for production environment.</span></span> <span data-ttu-id="42d2b-156">Toto je volitelné certifikát, který může být zadán, pokud chcete, aby toosecure vaše [Reverse Proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="42d2b-156">This is an optional certificate that can be specified if you want toosecure your [Reverse Proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="42d2b-157">Ujistěte se, že reverseProxyEndpointPort je nastaven v nodeTypes, pokud používáte tento certifikát.</span><span class="sxs-lookup"><span data-stu-id="42d2b-157">Make sure reverseProxyEndpointPort is set in nodeTypes if you are using this certificate.</span></span> |

<span data-ttu-id="42d2b-158">Tady je příklad konfigurace clusteru kde byly zadány hello clusteru, serveru a klientské certifikáty.</span><span class="sxs-lookup"><span data-stu-id="42d2b-158">Here is example cluster configuration where hello Cluster, Server, and Client certificates have been provided.</span></span> <span data-ttu-id="42d2b-159">Pamatujte, že pro cluster nebo server / reverseProxy certifikáty, kryptografický otisk a běžný název nejsou povoleny toobe nakonfigurované společně pro hello stejný typ certifikátu.</span><span class="sxs-lookup"><span data-stu-id="42d2b-159">Please note that for cluster/ server/ reverseProxy certificates, thumbprint and common name are not allowed toobe configured together for hello same cert type.</span></span>

 ```JSON
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace hello localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "metadata": "Replace hello localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "10.7.0.6",
        "metadata": "Replace hello localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace hello diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "hello Credential type X509 indicates this is cluster is secured using X509 Certificates. hello thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myClusterCertCommonName"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ServerCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myServerCertCommonName"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpointPort": "19001",
            "leaseDriverEndpointPort": "19002",
            "serviceConnectionEndpointPort": "19003",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="certificate-roll-over"></a><span data-ttu-id="42d2b-160">Převrácení certifikátu</span><span class="sxs-lookup"><span data-stu-id="42d2b-160">Certificate roll over</span></span>
<span data-ttu-id="42d2b-161">Při použití běžný název certifikátu místo kryptografický otisk, certifikát kumulativní přes nevyžaduje upgrade konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="42d2b-161">When using certificate common name instead of thumbprint, certificate roll over doesn't require cluster configuration upgrade.</span></span>
<span data-ttu-id="42d2b-162">Pokud certifikát kumulativní přes zahrnuje vystavitele mění, prosím zachovat hello staré vystavitele certifikátu v úložišti certifikátů hello minimálně 2 hodiny po instalaci nového vystavitele certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="42d2b-162">If certificate roll over involves issuer roll over, please keep hello old issuer cert in hello cert store at least 2 hours after installing hello new issuer cert.</span></span>

## <a name="acquire-hello-x509-certificates"></a><span data-ttu-id="42d2b-163">Získat certifikáty X.509 hello</span><span class="sxs-lookup"><span data-stu-id="42d2b-163">Acquire hello X.509 certificates</span></span>
<span data-ttu-id="42d2b-164">toosecure komunikaci v rámci hello clusteru, musíte nejdřív tooobtain certifikátů X.509 pro uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="42d2b-164">toosecure communication within hello cluster, you will first need tooobtain X.509 certificates for your cluster nodes.</span></span> <span data-ttu-id="42d2b-165">Kromě toho toothis toolimit připojení clusteru tooauthorized počítačů nebo uživatelů, budete potřebovat tooobtain a nainstalovat certifikáty pro hello klientské počítače.</span><span class="sxs-lookup"><span data-stu-id="42d2b-165">Additionally, toolimit connection toothis cluster tooauthorized machines/users, you will need tooobtain and install certificates for hello client machines.</span></span>

<span data-ttu-id="42d2b-166">Pro clustery, které jsou spuštěné úlohy v produkčním prostředí, měli byste použít [certifikační autoritou (CA)](https://en.wikipedia.org/wiki/Certificate_authority) podepsané clusteru hello toosecure certifikát X.509.</span><span class="sxs-lookup"><span data-stu-id="42d2b-166">For clusters that are running production workloads, you should use a [Certificate Authority (CA)](https://en.wikipedia.org/wiki/Certificate_authority) signed X.509 certificate toosecure hello cluster.</span></span> <span data-ttu-id="42d2b-167">Podrobnosti o získání těchto certifikátů najdete příliš[postupy: získání certifikátu](http://msdn.microsoft.com/library/aa702761.aspx).</span><span class="sxs-lookup"><span data-stu-id="42d2b-167">For details on obtaining these certificates, go too[How to: Obtain a Certificate](http://msdn.microsoft.com/library/aa702761.aspx).</span></span>

<span data-ttu-id="42d2b-168">Pro clustery, které používáte pro testovací účely můžete toouse certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="42d2b-168">For clusters that you use for test purposes, you can choose toouse a self-signed certificate.</span></span>

## <a name="optional-create-a-self-signed-certificate"></a><span data-ttu-id="42d2b-169">Volitelné: Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="42d2b-169">Optional: Create a self-signed certificate</span></span>
<span data-ttu-id="42d2b-170">Jedním ze způsobů toocreate certifikátu podepsaného svým držitelem, který může být správně zabezpečená je toouse hello *CertSetup.ps1* skript ve složce Service Fabric SDK hello v adresáři hello *C:\Program Files\Microsoft SDKs\Service Fabric\ ClusterSetup\Secure*.</span><span class="sxs-lookup"><span data-stu-id="42d2b-170">One way toocreate a self-signed cert that can be secured correctly is toouse hello *CertSetup.ps1* script in hello Service Fabric SDK folder in hello directory *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*.</span></span> <span data-ttu-id="42d2b-171">Upravit tento toochange hello výchozí název souboru certifikátu hello (vyhledejte hodnotu hello *CN = ServiceFabricDevClusterCert*).</span><span class="sxs-lookup"><span data-stu-id="42d2b-171">Edit this file toochange hello default name of hello certificate (look for hello value *CN=ServiceFabricDevClusterCert*).</span></span> <span data-ttu-id="42d2b-172">Spusťte tento skript jako `.\CertSetup.ps1 -Install`.</span><span class="sxs-lookup"><span data-stu-id="42d2b-172">Run this script as `.\CertSetup.ps1 -Install`.</span></span>

<span data-ttu-id="42d2b-173">Nyní exportujte soubor PFX tooa hello certifikátu chráněný heslem.</span><span class="sxs-lookup"><span data-stu-id="42d2b-173">Now export hello certificate tooa PFX file with a protected password.</span></span> <span data-ttu-id="42d2b-174">Nejdřív získáte hello kryptografický otisk certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="42d2b-174">First get hello thumbprint of hello certificate.</span></span> <span data-ttu-id="42d2b-175">Z hello *spustit* nabídce spustit hello *spravovat certifikáty počítače*.</span><span class="sxs-lookup"><span data-stu-id="42d2b-175">From hello *Start* menu, run hello *Manage computer certificates*.</span></span> <span data-ttu-id="42d2b-176">Přejděte toohello **Místní počítač\Osobní** složky a najít hello certifikátu jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="42d2b-176">Navigate toohello **Local Computer\Personal** folder and find hello certificate you just created.</span></span> <span data-ttu-id="42d2b-177">Dvakrát klikněte na certifikát tooopen hello ho vyberte hello *podrobnosti* a přejděte dolů toohello *kryptografický otisk* pole.</span><span class="sxs-lookup"><span data-stu-id="42d2b-177">Double-click hello certificate tooopen it, select hello *Details* tab and scroll down toohello *Thumbprint* field.</span></span> <span data-ttu-id="42d2b-178">Zkopírujte hodnotu kryptografického otisku hello do příkazu prostředí PowerShell hello níže, po odebrání hello prostory.</span><span class="sxs-lookup"><span data-stu-id="42d2b-178">Copy hello thumbprint value into hello PowerShell command below, after removing hello spaces.</span></span>  <span data-ttu-id="42d2b-179">Změna hello `String` hodnotu tooa vhodný zabezpečeného hesla tooprotect ji a spusťte hello následující v prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="42d2b-179">Change hello `String` value tooa suitable secure password tooprotect it and run hello following in PowerShell:</span></span>

```powershell   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

<span data-ttu-id="42d2b-180">toosee hello podrobnosti certifikátu nainstalovaného na hello počítač můžete spustit následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="42d2b-180">toosee hello details of a certificate installed on hello machine you can run hello following PowerShell command:</span></span>

```powershell
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

<span data-ttu-id="42d2b-181">Případně, pokud máte předplatné Azure, postupujte podle části hello [přidat certifikáty tooKey trezoru](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).</span><span class="sxs-lookup"><span data-stu-id="42d2b-181">Alternatively, if you have an Azure subscription, follow hello section [Add certificates tooKey Vault](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).</span></span>

## <a name="install-hello-certificates"></a><span data-ttu-id="42d2b-182">Instalace certifikátů hello</span><span class="sxs-lookup"><span data-stu-id="42d2b-182">Install hello certificates</span></span>
<span data-ttu-id="42d2b-183">Jakmile máte autority, můžete je nainstalovat na uzlech clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="42d2b-183">Once you have certificate(s), you can install them on hello cluster nodes.</span></span> <span data-ttu-id="42d2b-184">Uzly potřebovat toohave hello nejnovější prostředí Windows PowerShell 3.x na nich být nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="42d2b-184">Your nodes need toohave hello latest Windows PowerShell 3.x installed on them.</span></span> <span data-ttu-id="42d2b-185">Budete potřebovat toorepeat tyto kroky na každém uzlu clusteru a Server certifikátů a všechny sekundární certifikátů.</span><span class="sxs-lookup"><span data-stu-id="42d2b-185">You will need toorepeat these steps on each node, for both Cluster and Server certificates and any secondary certificates.</span></span>

1. <span data-ttu-id="42d2b-186">Zkopírujte hello .pfx soubory toohello uzlu.</span><span class="sxs-lookup"><span data-stu-id="42d2b-186">Copy hello .pfx file(s) toohello node.</span></span>
2. <span data-ttu-id="42d2b-187">Otevřete okno prostředí PowerShell jako správce a zadejte následující příkazy hello.</span><span class="sxs-lookup"><span data-stu-id="42d2b-187">Open a PowerShell window as an administrator and enter hello following commands.</span></span> <span data-ttu-id="42d2b-188">Nahraďte hello *$pswd* heslem hello používá toocreate tento certifikát.</span><span class="sxs-lookup"><span data-stu-id="42d2b-188">Replace hello *$pswd* with hello password that you used toocreate this certificate.</span></span> <span data-ttu-id="42d2b-189">Nahraďte hello *$PfxFilePath* s úplnou cestou hello hello .pfx zkopírovaný toothis uzlu.</span><span class="sxs-lookup"><span data-stu-id="42d2b-189">Replace hello *$PfxFilePath* with hello full path of hello .pfx copied toothis node.</span></span>
   
    ```powershell
    $pswd = "1234"
    $PfxFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```
3. <span data-ttu-id="42d2b-190">Nyní nastavte hello řízení přístupu na tento certifikát tak, aby hello Service Fabric procesu, který běží v rámci hello účet Network Service, můžete použít tak, že spustíte následující skript hello.</span><span class="sxs-lookup"><span data-stu-id="42d2b-190">Now set hello access control on this certificate so that hello Service Fabric process, which runs under hello Network Service account, can use it by running hello following script.</span></span> <span data-ttu-id="42d2b-191">Zadejte hello kryptografický otisk certifikátu hello a "Síťová služba" pro účet služby hello.</span><span class="sxs-lookup"><span data-stu-id="42d2b-191">Provide hello thumbprint of hello certificate and "NETWORK SERVICE" for hello service account.</span></span> <span data-ttu-id="42d2b-192">Můžete zkontrolovat, že hello seznamy ACL na certifikátu hello správnost otevřením hello certifikátu v *spustit* > *spravovat certifikáty počítače* a prohlížení *všechny úlohy*  >  *Spravovat soukromé klíče*.</span><span class="sxs-lookup"><span data-stu-id="42d2b-192">You can check that hello ACLs on hello certificate are correct by opening hello certificate in *Start* > *Manage computer certificates* and looking at *All Tasks* > *Manage Private Keys*.</span></span>
   
    ```powershell
    param
    (
    [Parameter(Position=1, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$pfxThumbPrint,
   
    [Parameter(Position=2, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$serviceAccount
    )
   
    $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; }
   
    # Specify hello user, hello permissions and hello permission type
    $permission = "$($serviceAccount)","FullControl","Allow"
    $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission
   
    # Location of hello machine related keys
    $keyPath = Join-Path -Path $env:ProgramData -ChildPath "\Microsoft\Crypto\RSA\MachineKeys"
    $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
    $keyFullPath = Join-Path -Path $keyPath -ChildPath $keyName
   
    # Get hello current acl of hello private key
    $acl = (Get-Item $keyFullPath).GetAccessControl('Access')
   
    # Add hello new ace toohello acl of hello private key
    $acl.SetAccessRule($accessRule)
   
    # Write back hello new acl
    Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop
   
    # Observe hello access rights currently assigned toothis certificate.
    get-acl $keyFullPath| fl
    ```
4. <span data-ttu-id="42d2b-193">Opakujte kroky hello výše pro každý certifikát serveru.</span><span class="sxs-lookup"><span data-stu-id="42d2b-193">Repeat hello steps above for each server certificate.</span></span> <span data-ttu-id="42d2b-194">Můžete také použít tyto kroky tooinstall hello klientské certifikáty na hello počítače, které chcete clusteru toohello tooallow přístupu.</span><span class="sxs-lookup"><span data-stu-id="42d2b-194">You can also use these steps tooinstall hello client certificates on hello machines that you want tooallow access toohello cluster.</span></span>

## <a name="create-hello-secure-cluster"></a><span data-ttu-id="42d2b-195">Vytvoření zabezpečeného clusteru hello</span><span class="sxs-lookup"><span data-stu-id="42d2b-195">Create hello secure cluster</span></span>
<span data-ttu-id="42d2b-196">Po dokončení konfigurace hello **zabezpečení** části hello **ClusterConfig.X509.MultiMachine.json** soubor, abyste mohli pokračovat příliš[vytvořit cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) tooconfigure části Hello uzly a vytvoření clusteru samostatné hello.</span><span class="sxs-lookup"><span data-stu-id="42d2b-196">After configuring hello **security** section of hello **ClusterConfig.X509.MultiMachine.json** file, you can proceed too[Create your cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) section tooconfigure hello nodes and create hello standalone cluster.</span></span> <span data-ttu-id="42d2b-197">Mějte na paměti, toouse hello **ClusterConfig.X509.MultiMachine.json** souboru při vytvoření clusteru s podporou hello.</span><span class="sxs-lookup"><span data-stu-id="42d2b-197">Remember toouse hello **ClusterConfig.X509.MultiMachine.json** file while creating hello cluster.</span></span> <span data-ttu-id="42d2b-198">Váš příkaz může například vypadat hello následující:</span><span class="sxs-lookup"><span data-stu-id="42d2b-198">For example, your command might look like hello following:</span></span>

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

<span data-ttu-id="42d2b-199">Jakmile máte hello zabezpečené clusteru úspěšném spuštění samostatné Windows a nastavili hello tooit tooconnect ověření klientů, postupujte podle části hello [tooa zabezpečení clusteru připojit pomocí prostředí PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="42d2b-199">Once you have hello secure standalone Windows cluster successfully running, and have setup hello authenticated clients tooconnect tooit, follow hello section [Connect tooa secure cluster using PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) tooconnect tooit.</span></span> <span data-ttu-id="42d2b-200">Například:</span><span class="sxs-lookup"><span data-stu-id="42d2b-200">For example:</span></span>

```powershell
$ConnectArgs = @{  ConnectionEndpoint = '10.7.0.5:19000';  X509Credential = $True;  StoreLocation = 'LocalMachine';  StoreName = "MY";  ServerCertThumbprint = "057b9544a6f2733e0c8d3a60013a58948213f551";  FindType = 'FindByThumbprint';  FindValue = "057b9544a6f2733e0c8d3a60013a58948213f551"   }
Connect-ServiceFabricCluster $ConnectArgs
```

<span data-ttu-id="42d2b-201">Když pak spustíte další toowork příkazy prostředí PowerShell s tímto clusterem.</span><span class="sxs-lookup"><span data-stu-id="42d2b-201">You can then run other PowerShell commands toowork with this cluster.</span></span> <span data-ttu-id="42d2b-202">Například [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps) tooshow seznam uzlů na tomto zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="42d2b-202">For example, [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps) tooshow a list of nodes on this secure cluster.</span></span>


<span data-ttu-id="42d2b-203">tooremove hello clusteru, připojit toohello uzlu v clusteru hello, kam jste stáhli balíček Service Fabric hello, otevřete příkazový řádek a přejděte toohello složky balíčku.</span><span class="sxs-lookup"><span data-stu-id="42d2b-203">tooremove hello cluster, connect toohello node on hello cluster where you downloaded hello Service Fabric package, open a command line and navigate toohello package folder.</span></span> <span data-ttu-id="42d2b-204">Nyní spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="42d2b-204">Now run hello following command:</span></span>

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

> [!NOTE]
> <span data-ttu-id="42d2b-205">Nesprávný certifikát konfigurace mohou zabránit objevuje během nasazení clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="42d2b-205">Incorrect certificate configuration may prevent hello cluster from coming up during deployment.</span></span> <span data-ttu-id="42d2b-206">tooself-diagnostikovat problémy se zabezpečením, podívejte se prosím ve skupině Prohlížeč událostí *protokoly aplikací a služeb* > *Microsoft Service Fabric*.</span><span class="sxs-lookup"><span data-stu-id="42d2b-206">tooself-diagnose security issues, please look in event viewer group *Applications and Services Logs* > *Microsoft-Service Fabric*.</span></span>
> 
> 

