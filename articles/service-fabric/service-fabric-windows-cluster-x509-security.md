---
title: "Zabezpečení clusteru služby Azure Service Fabric v systému Windows pomocí certifikátů | Microsoft Docs"
description: "Tento článek popisuje, jak zabezpečit komunikaci v rámci samostatné nebo privátní clusteru a také mezi klienty a cluster."
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
ms.openlocfilehash: 71ece1e43cc3c4ac3350cd59633065de06672420
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a><span data-ttu-id="43fa4-103">Zabezpečení clusteru s podporou samostatné do systému Windows pomocí certifikátů X.509</span><span class="sxs-lookup"><span data-stu-id="43fa4-103">Secure a standalone cluster on Windows using X.509 certificates</span></span>
<span data-ttu-id="43fa4-104">Tento článek popisuje, jak k zabezpečení komunikace mezi různými uzly clusteru Windows samostatné také jak k ověřování klientů, připojení k tomuto clusteru pomocí X.509 certifikáty.</span><span class="sxs-lookup"><span data-stu-id="43fa4-104">This article describes how to secure the communication between the various nodes of your standalone Windows cluster, as well as how to authenticate clients connecting to this cluster, using X.509 certificates.</span></span> <span data-ttu-id="43fa4-105">To zajišťuje, že můžete jenom autorizovaným uživatelům přístup ke clusteru, nasazené aplikace a provádět úlohy správy.</span><span class="sxs-lookup"><span data-stu-id="43fa4-105">This ensures that only authorized users can access the cluster, the deployed applications and perform management tasks.</span></span>  <span data-ttu-id="43fa4-106">Certifikát zabezpečení by měly být povoleny v clusteru při vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-106">Certificate security should be enabled on the cluster when the cluster is created.</span></span>  

<span data-ttu-id="43fa4-107">Další informace o zabezpečení clusteru, jako jsou například – uzly zabezpečení, klientský uzel zabezpečení a řízení přístupu na základě rolí najdete v tématu [clusteru scénáře zabezpečení](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="43fa4-107">For more information on cluster security such as node-to-node security, client-to-node security, and role-based access control, see [Cluster security scenarios](service-fabric-cluster-security.md).</span></span>

## <a name="which-certificates-will-you-need"></a><span data-ttu-id="43fa4-108">Certifikáty, které budete potřebovat?</span><span class="sxs-lookup"><span data-stu-id="43fa4-108">Which certificates will you need?</span></span>
<span data-ttu-id="43fa4-109">Abyste mohli začít [Stáhnout balíček clusteru samostatné](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) s jedním z uzlů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-109">To start with, [download the standalone cluster package](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) to one of the nodes in your cluster.</span></span> <span data-ttu-id="43fa4-110">V staženého balíčku naleznete **ClusterConfig.X509.MultiMachine.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-110">In the downloaded package, you will find a **ClusterConfig.X509.MultiMachine.json** file.</span></span> <span data-ttu-id="43fa4-111">Otevřete soubor a projděte si část pro **zabezpečení** pod **vlastnosti** části:</span><span class="sxs-lookup"><span data-stu-id="43fa4-111">Open the file and review the section for **security** under the **properties** section:</span></span>

```JSON
"security": {
    "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
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

<span data-ttu-id="43fa4-112">Tato část popisuje certifikáty, které je třeba pro zabezpečení samostatný cluster Windows.</span><span class="sxs-lookup"><span data-stu-id="43fa4-112">This section describes the certificates that you need for securing your standalone Windows cluster.</span></span> <span data-ttu-id="43fa4-113">Pokud zadáte certifikát clusteru, nastavte hodnotu **ClusterCredentialType** k  _**X509**_.</span><span class="sxs-lookup"><span data-stu-id="43fa4-113">If you are specifying a cluster certificate, set the value of **ClusterCredentialType** to _**X509**_.</span></span> <span data-ttu-id="43fa4-114">Pokud zadáte certifikát serveru pro připojení mimo, nastavte **ServerCredentialType** k  _**X509**_.</span><span class="sxs-lookup"><span data-stu-id="43fa4-114">If you are specifying server certificate for outside connections, set the **ServerCredentialType** to _**X509**_.</span></span> <span data-ttu-id="43fa4-115">I když není povinné, doporučujeme mít oba tyto certifikáty pro cluster s podporou správně zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="43fa4-115">Although not mandatory, we recommend to have both these certificates for a properly secured cluster.</span></span> <span data-ttu-id="43fa4-116">Pokud nastavíte tyto hodnoty na *X509* pak je nutné také zadat odpovídající certifikáty nebo Service Fabric vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="43fa4-116">If you set these values to *X509* then you must also specify the corresponding certificates or Service Fabric will throw an exception.</span></span> <span data-ttu-id="43fa4-117">V některých scénářích pouze můžete zadat _ClientCertificateThumbprints_ nebo _ReverseProxyCertificate_.</span><span class="sxs-lookup"><span data-stu-id="43fa4-117">In some scenarios, you may only want to specify the _ClientCertificateThumbprints_ or _ReverseProxyCertificate_.</span></span> <span data-ttu-id="43fa4-118">V těchto scénářích, není nutné nastavovat _ClusterCredentialType_ nebo _ServerCredentialType_ k _X509_.</span><span class="sxs-lookup"><span data-stu-id="43fa4-118">In those scenarios, you need not set _ClusterCredentialType_ or _ServerCredentialType_ to _X509_.</span></span>


> [!NOTE]
> <span data-ttu-id="43fa4-119">A [kryptografický otisk](https://en.wikipedia.org/wiki/Public_key_fingerprint) je primární Identita certifikátu.</span><span class="sxs-lookup"><span data-stu-id="43fa4-119">A [thumbprint](https://en.wikipedia.org/wiki/Public_key_fingerprint) is the primary identity of a certificate.</span></span> <span data-ttu-id="43fa4-120">Čtení [postup načtení kryptografického otisku certifikátu](https://msdn.microsoft.com/library/ms734695.aspx) a zjistěte, kryptografický otisk certifikáty, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="43fa4-120">Read [How to retrieve thumbprint of a certificate](https://msdn.microsoft.com/library/ms734695.aspx) to find out the thumbprint of the certificates that you create.</span></span>
> 
> 

<span data-ttu-id="43fa4-121">Následující tabulka uvádí certifikáty, které budete potřebovat na vaše nastavení clusteru:</span><span class="sxs-lookup"><span data-stu-id="43fa4-121">The following table lists the certificates that you will need on your cluster setup:</span></span>

| <span data-ttu-id="43fa4-122">**Nastavení CertificateInformation**</span><span class="sxs-lookup"><span data-stu-id="43fa4-122">**CertificateInformation Setting**</span></span> | <span data-ttu-id="43fa4-123">**Popis**</span><span class="sxs-lookup"><span data-stu-id="43fa4-123">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="43fa4-124">ClusterCertificate</span><span class="sxs-lookup"><span data-stu-id="43fa4-124">ClusterCertificate</span></span> |<span data-ttu-id="43fa4-125">Vhodné pro testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="43fa4-125">Recommended for test environment.</span></span> <span data-ttu-id="43fa4-126">Tento certifikát je vyžadován k zabezpečení komunikace mezi uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-126">This certificate is required to secure the communication between the nodes on a cluster.</span></span> <span data-ttu-id="43fa4-127">Můžete použít dvě různé certifikáty, primární a sekundární pro upgrade.</span><span class="sxs-lookup"><span data-stu-id="43fa4-127">You can use two different certificates, a primary and a secondary for upgrade.</span></span> <span data-ttu-id="43fa4-128">Kryptografický otisk certifikátu primární v nastavení **kryptografický otisk** části a u sekundární v **ThumbprintSecondary** proměnné.</span><span class="sxs-lookup"><span data-stu-id="43fa4-128">Set the thumbprint of the primary certificate in the **Thumbprint** section and that of the secondary in the **ThumbprintSecondary** variables.</span></span> |
| <span data-ttu-id="43fa4-129">ClusterCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="43fa4-129">ClusterCertificateCommonNames</span></span> |<span data-ttu-id="43fa4-130">Nedoporučuje pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="43fa4-130">Recommended for production environment.</span></span> <span data-ttu-id="43fa4-131">Tento certifikát je vyžadován k zabezpečení komunikace mezi uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-131">This certificate is required to secure the communication between the nodes on a cluster.</span></span> <span data-ttu-id="43fa4-132">Můžete použít jednu nebo dvě clusteru běžné názvy certifikátů.</span><span class="sxs-lookup"><span data-stu-id="43fa4-132">You can use one or two cluster certificate common names.</span></span> |
| <span data-ttu-id="43fa4-133">ServerCertificate</span><span class="sxs-lookup"><span data-stu-id="43fa4-133">ServerCertificate</span></span> |<span data-ttu-id="43fa4-134">Vhodné pro testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="43fa4-134">Recommended for test environment.</span></span> <span data-ttu-id="43fa4-135">Tento certifikát je pro klienta zobrazí při pokusu o připojení k tomuto clusteru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-135">This certificate is presented to the client when it tries to connect to this cluster.</span></span> <span data-ttu-id="43fa4-136">Pro usnadnění práce, můžete použít stejný certifikát pro *ClusterCertificate* a *ServerCertificate*.</span><span class="sxs-lookup"><span data-stu-id="43fa4-136">For convenience, you can choose to use the same certificate for *ClusterCertificate* and *ServerCertificate*.</span></span> <span data-ttu-id="43fa4-137">Dva certifikáty jiný server, primární a sekundární můžete použít pro upgrade.</span><span class="sxs-lookup"><span data-stu-id="43fa4-137">You can use two different server certificates, a primary and a secondary for upgrade.</span></span> <span data-ttu-id="43fa4-138">Kryptografický otisk certifikátu primární v nastavení **kryptografický otisk** části a u sekundární v **ThumbprintSecondary** proměnné.</span><span class="sxs-lookup"><span data-stu-id="43fa4-138">Set the thumbprint of the primary certificate in the **Thumbprint** section and that of the secondary in the **ThumbprintSecondary** variables.</span></span> |
| <span data-ttu-id="43fa4-139">ServerCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="43fa4-139">ServerCertificateCommonNames</span></span> |<span data-ttu-id="43fa4-140">Nedoporučuje pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="43fa4-140">Recommended for production environment.</span></span> <span data-ttu-id="43fa4-141">Tento certifikát je pro klienta zobrazí při pokusu o připojení k tomuto clusteru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-141">This certificate is presented to the client when it tries to connect to this cluster.</span></span> <span data-ttu-id="43fa4-142">Pro usnadnění práce, můžete použít stejný certifikát pro *ClusterCertificateCommonNames* a *ServerCertificateCommonNames*.</span><span class="sxs-lookup"><span data-stu-id="43fa4-142">For convenience, you can choose to use the same certificate for *ClusterCertificateCommonNames* and *ServerCertificateCommonNames*.</span></span> <span data-ttu-id="43fa4-143">Můžete použít jednu nebo dvě certifikát běžné názvy serverů.</span><span class="sxs-lookup"><span data-stu-id="43fa4-143">You can use one or two server certificate common names.</span></span> |
| <span data-ttu-id="43fa4-144">ClientCertificateThumbprints</span><span class="sxs-lookup"><span data-stu-id="43fa4-144">ClientCertificateThumbprints</span></span> |<span data-ttu-id="43fa4-145">To je sada certifikáty, které chcete instalovat na ověřené klienty.</span><span class="sxs-lookup"><span data-stu-id="43fa4-145">This is a set of certificates that you want to install on the authenticated clients.</span></span> <span data-ttu-id="43fa4-146">Může mít řadu různých klientské certifikáty, které jsou nainstalované v počítačích, které chcete povolit přístup ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-146">You can have a number of different client certificates installed on the machines that you want to allow access to the cluster.</span></span> <span data-ttu-id="43fa4-147">Kryptografický otisk každý certifikát v nastavení **CertificateThumbprint** proměnné.</span><span class="sxs-lookup"><span data-stu-id="43fa4-147">Set the thumbprint of each certificate in the **CertificateThumbprint** variable.</span></span> <span data-ttu-id="43fa4-148">Pokud nastavíte **IsAdmin** k *true*, pak klienta se tento certifikát nainstalován, můžete provést správce správy aktivit v clusteru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-148">If you set the **IsAdmin** to *true*, then the client with this certificate installed on it can do administrator management activities on the cluster.</span></span> <span data-ttu-id="43fa4-149">Pokud **IsAdmin** je *false*, klient s tímto certifikátem můžete provádět jenom akce povolené pro uživatelská přístupová práva, obvykle jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="43fa4-149">If the **IsAdmin** is *false*, the client with this certificate can only perform the actions allowed for user access rights, typically read-only.</span></span> <span data-ttu-id="43fa4-150">Další informace o rolích najdete v tématu [řízení přístupu (RBAC) na základě Role](service-fabric-cluster-security.md#role-based-access-control-rbac)</span><span class="sxs-lookup"><span data-stu-id="43fa4-150">For more information on roles read [Role based access control (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac)</span></span> |
| <span data-ttu-id="43fa4-151">ClientCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="43fa4-151">ClientCertificateCommonNames</span></span> |<span data-ttu-id="43fa4-152">Běžný název první klientský certifikát pro nastavit **CertificateCommonName**.</span><span class="sxs-lookup"><span data-stu-id="43fa4-152">Set the common name of the first client certificate for the **CertificateCommonName**.</span></span> <span data-ttu-id="43fa4-153">**CertificateIssuerThumbprint** je kryptografický otisk pro vystavitele tohoto certifikátu.</span><span class="sxs-lookup"><span data-stu-id="43fa4-153">The **CertificateIssuerThumbprint** is the thumbprint for the issuer of this certificate.</span></span> <span data-ttu-id="43fa4-154">Čtení [práce s certifikáty](https://msdn.microsoft.com/library/ms731899.aspx) Další informace o běžné názvy a vystavitele.</span><span class="sxs-lookup"><span data-stu-id="43fa4-154">Read [Working with certificates](https://msdn.microsoft.com/library/ms731899.aspx) to know more about common names and the issuer.</span></span> |
| <span data-ttu-id="43fa4-155">ReverseProxyCertificate</span><span class="sxs-lookup"><span data-stu-id="43fa4-155">ReverseProxyCertificate</span></span> |<span data-ttu-id="43fa4-156">Vhodné pro testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="43fa4-156">Recommended for test environment.</span></span> <span data-ttu-id="43fa4-157">Toto je volitelné certifikát, který může být zadán, pokud chcete zabezpečit vaše [Reverse Proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="43fa4-157">This is an optional certificate that can be specified if you want to secure your [Reverse Proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="43fa4-158">Ujistěte se, že reverseProxyEndpointPort je nastaven v nodeTypes, pokud používáte tento certifikát.</span><span class="sxs-lookup"><span data-stu-id="43fa4-158">Make sure reverseProxyEndpointPort is set in nodeTypes if you are using this certificate.</span></span> |
| <span data-ttu-id="43fa4-159">ReverseProxyCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="43fa4-159">ReverseProxyCertificateCommonNames</span></span> |<span data-ttu-id="43fa4-160">Nedoporučuje pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="43fa4-160">Recommended for production environment.</span></span> <span data-ttu-id="43fa4-161">Toto je volitelné certifikát, který může být zadán, pokud chcete zabezpečit vaše [Reverse Proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="43fa4-161">This is an optional certificate that can be specified if you want to secure your [Reverse Proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="43fa4-162">Ujistěte se, že reverseProxyEndpointPort je nastaven v nodeTypes, pokud používáte tento certifikát.</span><span class="sxs-lookup"><span data-stu-id="43fa4-162">Make sure reverseProxyEndpointPort is set in nodeTypes if you are using this certificate.</span></span> |

<span data-ttu-id="43fa4-163">Tady je příklad konfigurace clusteru kde byly zadány clusteru, serveru a klientské certifikáty.</span><span class="sxs-lookup"><span data-stu-id="43fa4-163">Here is example cluster configuration where the Cluster, Server, and Client certificates have been provided.</span></span> <span data-ttu-id="43fa4-164">Pamatujte, že pro cluster nebo server / reverseProxy certifikáty, kryptografický otisk a běžný název nejsou povoleny společně nakonfigurovat pro stejný typ certifikátu.</span><span class="sxs-lookup"><span data-stu-id="43fa4-164">Please note that for cluster/ server/ reverseProxy certificates, thumbprint and common name are not allowed to be configured together for the same cert type.</span></span>

 ```JSON
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "10.7.0.6",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
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

## <a name="certificate-roll-over"></a><span data-ttu-id="43fa4-165">Převrácení certifikátu</span><span class="sxs-lookup"><span data-stu-id="43fa4-165">Certificate roll over</span></span>
<span data-ttu-id="43fa4-166">Při použití běžný název certifikátu místo kryptografický otisk, certifikát kumulativní přes nevyžaduje upgrade konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-166">When using certificate common name instead of thumbprint, certificate roll over doesn't require cluster configuration upgrade.</span></span>
<span data-ttu-id="43fa4-167">Pokud certifikát kumulativní přes zahrnuje vystavitele mění, uložte zachovat původní Vystavitel certifikátu v certifikát minimálně 2 hodiny po instalaci nového vystavitele certifikátu.</span><span class="sxs-lookup"><span data-stu-id="43fa4-167">If certificate roll over involves issuer roll over, please keep the old issuer cert in the cert store at least 2 hours after installing the new issuer cert.</span></span>

## <a name="acquire-the-x509-certificates"></a><span data-ttu-id="43fa4-168">Získat certifikáty X.509</span><span class="sxs-lookup"><span data-stu-id="43fa4-168">Acquire the X.509 certificates</span></span>
<span data-ttu-id="43fa4-169">Pro zabezpečení komunikace v rámci clusteru, bude nejprve nutné k získání certifikátů X.509 pro uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-169">To secure communication within the cluster, you will first need to obtain X.509 certificates for your cluster nodes.</span></span> <span data-ttu-id="43fa4-170">Kromě toho pokud chcete omezit připojení k tomuto clusteru na autorizované počítače nebo uživatele, musíte získat a nainstalovat certifikáty pro klientské počítače.</span><span class="sxs-lookup"><span data-stu-id="43fa4-170">Additionally, to limit connection to this cluster to authorized machines/users, you will need to obtain and install certificates for the client machines.</span></span>

<span data-ttu-id="43fa4-171">Pro clustery, které jsou spuštěné úlohy v produkčním prostředí, měli byste použít [certifikační autoritou (CA)](https://en.wikipedia.org/wiki/Certificate_authority) podepsaný certifikát X.509 k zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-171">For clusters that are running production workloads, you should use a [Certificate Authority (CA)](https://en.wikipedia.org/wiki/Certificate_authority) signed X.509 certificate to secure the cluster.</span></span> <span data-ttu-id="43fa4-172">Podrobnosti o získání těchto certifikátů, přejděte na [postupy: získání certifikátu](http://msdn.microsoft.com/library/aa702761.aspx).</span><span class="sxs-lookup"><span data-stu-id="43fa4-172">For details on obtaining these certificates, go to [How to: Obtain a Certificate](http://msdn.microsoft.com/library/aa702761.aspx).</span></span>

<span data-ttu-id="43fa4-173">Pro clustery, které používáte pro testovací účely můžete použít certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="43fa4-173">For clusters that you use for test purposes, you can choose to use a self-signed certificate.</span></span>

## <a name="optional-create-a-self-signed-certificate"></a><span data-ttu-id="43fa4-174">Volitelné: Vytvořit certifikát podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="43fa4-174">Optional: Create a self-signed certificate</span></span>
<span data-ttu-id="43fa4-175">Vytvoření certifikátu podepsaného svým držitelem, který může být správně zabezpečená jedním ze způsobů je používat *CertSetup.ps1* skript ve složce Service Fabric SDK v adresáři *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*.</span><span class="sxs-lookup"><span data-stu-id="43fa4-175">One way to create a self-signed cert that can be secured correctly is to use the *CertSetup.ps1* script in the Service Fabric SDK folder in the directory *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*.</span></span> <span data-ttu-id="43fa4-176">Upravit tento soubor a změnit výchozí název certifikátu (vyhledejte hodnotu *CN = ServiceFabricDevClusterCert*).</span><span class="sxs-lookup"><span data-stu-id="43fa4-176">Edit this file to change the default name of the certificate (look for the value *CN=ServiceFabricDevClusterCert*).</span></span> <span data-ttu-id="43fa4-177">Spusťte tento skript jako `.\CertSetup.ps1 -Install`.</span><span class="sxs-lookup"><span data-stu-id="43fa4-177">Run this script as `.\CertSetup.ps1 -Install`.</span></span>

<span data-ttu-id="43fa4-178">Nyní exportujte certifikát do souboru PFX chráněný heslem.</span><span class="sxs-lookup"><span data-stu-id="43fa4-178">Now export the certificate to a PFX file with a protected password.</span></span> <span data-ttu-id="43fa4-179">Nejdřív získáte kryptografický otisk certifikátu.</span><span class="sxs-lookup"><span data-stu-id="43fa4-179">First get the thumbprint of the certificate.</span></span> <span data-ttu-id="43fa4-180">Z *spustit* nabídky, spusťte *spravovat certifikáty počítače*.</span><span class="sxs-lookup"><span data-stu-id="43fa4-180">From the *Start* menu, run the *Manage computer certificates*.</span></span> <span data-ttu-id="43fa4-181">Přejděte na **Místní počítač\Osobní** složky a najít certifikát, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="43fa4-181">Navigate to the **Local Computer\Personal** folder and find the certificate you just created.</span></span> <span data-ttu-id="43fa4-182">Dvakrát klikněte na certifikát, který chcete otevřít, vyberte *podrobnosti* kartě a přejděte dolů k položce *kryptografický otisk* pole.</span><span class="sxs-lookup"><span data-stu-id="43fa4-182">Double-click the certificate to open it, select the *Details* tab and scroll down to the *Thumbprint* field.</span></span> <span data-ttu-id="43fa4-183">Zkopírujte hodnotu kryptografického otisku do příkaz prostředí PowerShell následující po odebrání prostory.</span><span class="sxs-lookup"><span data-stu-id="43fa4-183">Copy the thumbprint value into the PowerShell command below, after removing the spaces.</span></span>  <span data-ttu-id="43fa4-184">Změna `String` hodnotu na vhodný zabezpečené heslo, aby ho ochránil a spusťte následující příkazy v prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="43fa4-184">Change the `String` value to a suitable secure password to protect it and run the following in PowerShell:</span></span>

```powershell   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

<span data-ttu-id="43fa4-185">Chcete-li zobrazit podrobnosti o certifikát nainstalovaný na počítači spuštěním následujícího příkazu Powershellu:</span><span class="sxs-lookup"><span data-stu-id="43fa4-185">To see the details of a certificate installed on the machine you can run the following PowerShell command:</span></span>

```powershell
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

<span data-ttu-id="43fa4-186">Případně, pokud máte předplatné Azure, postupujte podle kroků v části [přidejte certifikáty do Key Vault](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).</span><span class="sxs-lookup"><span data-stu-id="43fa4-186">Alternatively, if you have an Azure subscription, follow the section [Add certificates to Key Vault](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).</span></span>

## <a name="install-the-certificates"></a><span data-ttu-id="43fa4-187">Nainstalujte certifikáty</span><span class="sxs-lookup"><span data-stu-id="43fa4-187">Install the certificates</span></span>
<span data-ttu-id="43fa4-188">Jakmile máte autority, můžete je nainstalovat na uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-188">Once you have certificate(s), you can install them on the cluster nodes.</span></span> <span data-ttu-id="43fa4-189">Uzly musí mít nejnovější prostředí Windows PowerShell 3.x na nich být nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="43fa4-189">Your nodes need to have the latest Windows PowerShell 3.x installed on them.</span></span> <span data-ttu-id="43fa4-190">Musíte opakujte tyto kroky na každém uzlu clusteru a Server certifikátů a všechny sekundární certifikátů.</span><span class="sxs-lookup"><span data-stu-id="43fa4-190">You will need to repeat these steps on each node, for both Cluster and Server certificates and any secondary certificates.</span></span>

1. <span data-ttu-id="43fa4-191">Zkopírujte soubory .pfx do uzlu.</span><span class="sxs-lookup"><span data-stu-id="43fa4-191">Copy the .pfx file(s) to the node.</span></span>
2. <span data-ttu-id="43fa4-192">Otevřete okno prostředí PowerShell jako správce a zadejte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="43fa4-192">Open a PowerShell window as an administrator and enter the following commands.</span></span> <span data-ttu-id="43fa4-193">Nahraďte *$pswd* s heslem, které jste použili k vytvoření tohoto certifikátu.</span><span class="sxs-lookup"><span data-stu-id="43fa4-193">Replace the *$pswd* with the password that you used to create this certificate.</span></span> <span data-ttu-id="43fa4-194">Nahraďte *$PfxFilePath* s úplnou cestou .pfx zkopíruje na tento uzel.</span><span class="sxs-lookup"><span data-stu-id="43fa4-194">Replace the *$PfxFilePath* with the full path of the .pfx copied to this node.</span></span>
   
    ```powershell
    $pswd = "1234"
    $PfxFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```
3. <span data-ttu-id="43fa4-195">Teď nastavte řízení přístupu na tento certifikát tak, aby Service Fabric procesu, který běží pod účtem Network Service, můžete použít tak, že spustíte následující skript.</span><span class="sxs-lookup"><span data-stu-id="43fa4-195">Now set the access control on this certificate so that the Service Fabric process, which runs under the Network Service account, can use it by running the following script.</span></span> <span data-ttu-id="43fa4-196">Zadejte kryptografický otisk certifikátu a "Síťová služba" pro účet služby.</span><span class="sxs-lookup"><span data-stu-id="43fa4-196">Provide the thumbprint of the certificate and "NETWORK SERVICE" for the service account.</span></span> <span data-ttu-id="43fa4-197">Můžete zkontrolovat, zda jsou seznamy ACL v certifikátu správné otevřením certifikátu v *spustit* > *spravovat certifikáty počítače* a prohlížení *všechny úlohy* > *spravovat privátní klíče*.</span><span class="sxs-lookup"><span data-stu-id="43fa4-197">You can check that the ACLs on the certificate are correct by opening the certificate in *Start* > *Manage computer certificates* and looking at *All Tasks* > *Manage Private Keys*.</span></span>
   
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
   
    # Specify the user, the permissions and the permission type
    $permission = "$($serviceAccount)","FullControl","Allow"
    $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission
   
    # Location of the machine related keys
    $keyPath = Join-Path -Path $env:ProgramData -ChildPath "\Microsoft\Crypto\RSA\MachineKeys"
    $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
    $keyFullPath = Join-Path -Path $keyPath -ChildPath $keyName
   
    # Get the current acl of the private key
    $acl = (Get-Item $keyFullPath).GetAccessControl('Access')
   
    # Add the new ace to the acl of the private key
    $acl.SetAccessRule($accessRule)
   
    # Write back the new acl
    Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop
   
    # Observe the access rights currently assigned to this certificate.
    get-acl $keyFullPath| fl
    ```
4. <span data-ttu-id="43fa4-198">Opakujte předchozí kroky pro každý certifikát serveru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-198">Repeat the steps above for each server certificate.</span></span> <span data-ttu-id="43fa4-199">Tyto kroky můžete použít také k instalaci klientské certifikáty na počítačích, které chcete povolit přístup ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-199">You can also use these steps to install the client certificates on the machines that you want to allow access to the cluster.</span></span>

## <a name="create-the-secure-cluster"></a><span data-ttu-id="43fa4-200">Vytvoření zabezpečeného clusteru</span><span class="sxs-lookup"><span data-stu-id="43fa4-200">Create the secure cluster</span></span>
<span data-ttu-id="43fa4-201">Po dokončení konfigurace **zabezpečení** části **ClusterConfig.X509.MultiMachine.json** souboru, můžete přejít k [vytvořit cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) oddílu konfigurace uzlů a vytvořit samostatný cluster.</span><span class="sxs-lookup"><span data-stu-id="43fa4-201">After configuring the **security** section of the **ClusterConfig.X509.MultiMachine.json** file, you can proceed to [Create your cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) section to configure the nodes and create the standalone cluster.</span></span> <span data-ttu-id="43fa4-202">Nezapomeňte použít **ClusterConfig.X509.MultiMachine.json** souboru při vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-202">Remember to use the **ClusterConfig.X509.MultiMachine.json** file while creating the cluster.</span></span> <span data-ttu-id="43fa4-203">Například váš příkaz může vypadat následovně:</span><span class="sxs-lookup"><span data-stu-id="43fa4-203">For example, your command might look like the following:</span></span>

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

<span data-ttu-id="43fa4-204">Jakmile máte zabezpečené samostatné Windows clusteru úspěšně spuštěna, a nastavili ověřené klientům připojit se k němu, postupujte podle části [připojení ke clusteru zabezpečené pomocí prostředí PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) připojení k tomuto.</span><span class="sxs-lookup"><span data-stu-id="43fa4-204">Once you have the secure standalone Windows cluster successfully running, and have setup the authenticated clients to connect to it, follow the section [Connect to a secure cluster using PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) to connect to it.</span></span> <span data-ttu-id="43fa4-205">Například:</span><span class="sxs-lookup"><span data-stu-id="43fa4-205">For example:</span></span>

```powershell
$ConnectArgs = @{  ConnectionEndpoint = '10.7.0.5:19000';  X509Credential = $True;  StoreLocation = 'LocalMachine';  StoreName = "MY";  ServerCertThumbprint = "057b9544a6f2733e0c8d3a60013a58948213f551";  FindType = 'FindByThumbprint';  FindValue = "057b9544a6f2733e0c8d3a60013a58948213f551"   }
Connect-ServiceFabricCluster $ConnectArgs
```

<span data-ttu-id="43fa4-206">Potom můžete spustit další příkazy prostředí PowerShell pro práci s tímto clusterem.</span><span class="sxs-lookup"><span data-stu-id="43fa4-206">You can then run other PowerShell commands to work with this cluster.</span></span> <span data-ttu-id="43fa4-207">Například [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps) zobrazíte seznam uzlů na tomto zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-207">For example, [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps) to show a list of nodes on this secure cluster.</span></span>


<span data-ttu-id="43fa4-208">Odebrat cluster, připojit k uzlu v clusteru, kam jste stáhli balíček Service Fabric, otevřete příkazový řádek a přejděte do složky balíčku.</span><span class="sxs-lookup"><span data-stu-id="43fa4-208">To remove the cluster, connect to the node on the cluster where you downloaded the Service Fabric package, open a command line and navigate to the package folder.</span></span> <span data-ttu-id="43fa4-209">Nyní spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="43fa4-209">Now run the following command:</span></span>

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

> [!NOTE]
> <span data-ttu-id="43fa4-210">Nesprávný certifikát konfigurace mohou zabránit objevuje během nasazení clusteru.</span><span class="sxs-lookup"><span data-stu-id="43fa4-210">Incorrect certificate configuration may prevent the cluster from coming up during deployment.</span></span> <span data-ttu-id="43fa4-211">Samoobslužné diagnostikovat problémy se zabezpečením, podívejte se prosím ve skupině Prohlížeč událostí *protokoly aplikací a služeb* > *Microsoft Service Fabric*.</span><span class="sxs-lookup"><span data-stu-id="43fa4-211">To self-diagnose security issues, please look in event viewer group *Applications and Services Logs* > *Microsoft-Service Fabric*.</span></span>
> 
> 

