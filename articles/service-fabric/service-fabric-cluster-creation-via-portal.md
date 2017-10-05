---
title: "Vytvořit cluster Service Fabric na portálu Azure | Microsoft Docs"
description: "Tento článek popisuje postup nastavení zabezpečení clusteru Service Fabric v Azure pomocí portálu Azure a Azure Key Vault."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: vturecek
ms.assetid: 426c3d13-127a-49eb-a54c-6bde7c87a83b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/21/2017
ms.author: chackdan
ms.openlocfilehash: 7dda9520ce3d93bf0e86bd2481ad06c268d087c7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-service-fabric-cluster-in-azure-using-the-azure-portal"></a><span data-ttu-id="64727-103">Vytvořit cluster Service Fabric v Azure pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="64727-103">Create a Service Fabric cluster in Azure using the Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="64727-104">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="64727-104">Azure Resource Manager</span></span>](service-fabric-cluster-creation-via-arm.md)
> * [<span data-ttu-id="64727-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="64727-105">Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
> 
> 

<span data-ttu-id="64727-106">Toto je podrobný průvodce, který vás provede kroky nastavení zabezpečení clusteru Service Fabric v Azure pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="64727-106">This is a step-by-step guide that walks you through the steps of setting up a secure Service Fabric cluster in Azure using the Azure portal.</span></span> <span data-ttu-id="64727-107">Tento průvodce vás provede následující kroky:</span><span class="sxs-lookup"><span data-stu-id="64727-107">This guide walks you through the following steps:</span></span>

* <span data-ttu-id="64727-108">Nastavte Key Vault pro správu klíčů pro zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="64727-108">Set up Key Vault to manage keys for cluster security.</span></span>
* <span data-ttu-id="64727-109">Vytvoření zabezpečeného clusteru v Azure prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="64727-109">Create a secured cluster in Azure through the Azure portal.</span></span>
* <span data-ttu-id="64727-110">Ověřování pomocí certifikátů správci.</span><span class="sxs-lookup"><span data-stu-id="64727-110">Authenticate administrators using certificates.</span></span>

> [!NOTE]
> <span data-ttu-id="64727-111">Pro pokročilejší možnosti zabezpečení, jako je například ověřování uživatelů s Azure Active Directory a nastavení certifikátů pro zabezpečení aplikací [vytvořit cluster pomocí Azure Resource Manager][create-cluster-arm].</span><span class="sxs-lookup"><span data-stu-id="64727-111">For more advanced security options, such as user authentication with Azure Active Directory and setting up certificates for application security, [create your cluster using Azure Resource Manager][create-cluster-arm].</span></span>
> 
> 

<span data-ttu-id="64727-112">Cluster s podporou zabezpečení je clusteru, který brání neoprávněným přístupem k operace správy, která zahrnuje nasazení, upgrade a odstranění aplikací, služeb a data, která obsahují.</span><span class="sxs-lookup"><span data-stu-id="64727-112">A secure cluster is a cluster that prevents unauthorized access to management operations, which includes deploying, upgrading, and deleting applications, services, and the data they contain.</span></span> <span data-ttu-id="64727-113">Nezabezpečená clusteru je cluster každý, kdo může připojit k kdykoli a provádět operace správy.</span><span class="sxs-lookup"><span data-stu-id="64727-113">An unsecure cluster is a cluster that anyone can connect to at any time and perform management operations.</span></span> <span data-ttu-id="64727-114">Přestože je možné vytvořit nezabezpečená clusteru, je **důrazně doporučujeme vytvořit cluster zabezpečené**.</span><span class="sxs-lookup"><span data-stu-id="64727-114">Although it is possible to create an unsecure cluster, it is **highly recommended to create a secure cluster**.</span></span> <span data-ttu-id="64727-115">Nezabezpečená clusteru **nelze zabezpečit později** -je nutné vytvořit nový cluster.</span><span class="sxs-lookup"><span data-stu-id="64727-115">An unsecure cluster **cannot be secured later** - a new cluster must be created.</span></span>

<span data-ttu-id="64727-116">Koncepty jsou stejné pro vytvoření zabezpečeného clusterů, jestli jsou clusterů systému Windows nebo Linux clusterů.</span><span class="sxs-lookup"><span data-stu-id="64727-116">The concepts are the same for creating secure clusters, whether the clusters are Linux clusters or Windows clusters.</span></span> <span data-ttu-id="64727-117">Další informace a pomocné rutiny skripty pro vytvoření zabezpečeného clusterů systému Linux, najdete v tématu [vytváření zabezpečené clusterů v systému Linux](service-fabric-cluster-creation-via-arm.md#secure-linux-clusters).</span><span class="sxs-lookup"><span data-stu-id="64727-117">For more information and helper scripts for creating secure Linux clusters, please see [Creating secure clusters on Linux](service-fabric-cluster-creation-via-arm.md#secure-linux-clusters).</span></span> <span data-ttu-id="64727-118">Parametry získané zadané pomocné rutiny skript můžete zadat přímo do portálu, jak je popsáno v části [na portálu Azure vytvořit cluster](#create-cluster-portal).</span><span class="sxs-lookup"><span data-stu-id="64727-118">The parameters obtained by the helper script provided can be input directly into the portal as described in the section [Create a cluster in the Azure portal](#create-cluster-portal).</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="64727-119">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="64727-119">Log in to Azure</span></span>
<span data-ttu-id="64727-120">Tato příručka používá [prostředí Azure PowerShell][azure-powershell].</span><span class="sxs-lookup"><span data-stu-id="64727-120">This guide uses [Azure PowerShell][azure-powershell].</span></span> <span data-ttu-id="64727-121">Při spouštění novou relaci prostředí PowerShell, přihlaste se k účtu Azure a vybrat své předplatné před provedením Azure příkazy.</span><span class="sxs-lookup"><span data-stu-id="64727-121">When starting a new PowerShell session, log in to your Azure account and select your subscription before executing Azure commands.</span></span>

<span data-ttu-id="64727-122">Přihlaste se k účtu azure:</span><span class="sxs-lookup"><span data-stu-id="64727-122">Log in to your azure account:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="64727-123">Vyberte předplatné:</span><span class="sxs-lookup"><span data-stu-id="64727-123">Select your subscription:</span></span>

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a><span data-ttu-id="64727-124">Nastavení služby Key Vault</span><span class="sxs-lookup"><span data-stu-id="64727-124">Set up Key Vault</span></span>
<span data-ttu-id="64727-125">Tato část průvodce vás provede procesem vytvoření trezoru klíč pro cluster Service Fabric v Azure a aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="64727-125">This part of the guide walks you through creating a Key Vault for a Service Fabric cluster in Azure and for Service Fabric applications.</span></span> <span data-ttu-id="64727-126">Dokončení průvodce v Key Vault, najdete v článku [Key Vault Příručka Začínáme][key-vault-get-started].</span><span class="sxs-lookup"><span data-stu-id="64727-126">For a complete guide on Key Vault, see the [Key Vault getting started guide][key-vault-get-started].</span></span>

<span data-ttu-id="64727-127">Service Fabric používá certifikáty X.509 k zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="64727-127">Service Fabric uses X.509 certificates to secure a cluster.</span></span> <span data-ttu-id="64727-128">Azure Key Vault se používá ke správě certifikátů pro clusterů Service Fabric v Azure.</span><span class="sxs-lookup"><span data-stu-id="64727-128">Azure Key Vault is used to manage certificates for Service Fabric clusters in Azure.</span></span> <span data-ttu-id="64727-129">Pokud cluster je nasazené v Azure, poskytovatel prostředků Azure, který je zodpovědný za vytváření clusterů Service Fabric vyžaduje certifikáty od Key Vault a nainstaluje je v clusteru virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="64727-129">When a cluster is deployed in Azure, the Azure resource provider responsible for creating Service Fabric clusters pulls certificates from Key Vault and installs them on the cluster VMs.</span></span>

<span data-ttu-id="64727-130">Následující diagram znázorňuje vztah mezi Key Vault, cluster Service Fabric a poskytovatel prostředků Azure, která používá certifikáty uložené v Key Vault, při vytváření clusteru:</span><span class="sxs-lookup"><span data-stu-id="64727-130">The following diagram illustrates the relationship between Key Vault, a Service Fabric cluster, and the Azure resource provider that uses certificates stored in Key Vault when it creates a cluster:</span></span>

![Instalace certifikátu][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a><span data-ttu-id="64727-132">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="64727-132">Create a Resource Group</span></span>
<span data-ttu-id="64727-133">Prvním krokem je vytvoření nové skupiny prostředků speciálně pro Key Vault.</span><span class="sxs-lookup"><span data-stu-id="64727-133">The first step is to create a new resource group specifically for Key Vault.</span></span> <span data-ttu-id="64727-134">Uvedení Key Vault do vlastní skupiny prostředků se doporučuje, aby odeberete skupiny prostředků výpočetního prostředí a úložiště – například skupiny prostředků, který má cluster Service Fabric – bez ztráty klíče a tajné klíče.</span><span class="sxs-lookup"><span data-stu-id="64727-134">Putting Key Vault into its own resource group is recommended so that you can remove compute and storage resource groups - such as the resource group that has your Service Fabric cluster - without losing your keys and secrets.</span></span> <span data-ttu-id="64727-135">Skupinu prostředků, která má Key Vault musí být ve stejné oblasti jako cluster, který se používá.</span><span class="sxs-lookup"><span data-stu-id="64727-135">The resource group that has your Key Vault must be in the same region as the cluster that is using it.</span></span>

```powershell

    PS C:\Users\vturecek> New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet will be modified in a future release.

    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a><span data-ttu-id="64727-136">Vytvoření trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="64727-136">Create Key Vault</span></span>
<span data-ttu-id="64727-137">Vytvoření trezoru klíč do nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="64727-137">Create a Key Vault in the new resource group.</span></span> <span data-ttu-id="64727-138">Key Vault **musí být povolen pro nasazení** umožňující poskytovateli prostředků Service Fabric získat certifikáty z něj a nainstalujte na uzly clusteru:</span><span class="sxs-lookup"><span data-stu-id="64727-138">The Key Vault **must be enabled for deployment** to allow the Service Fabric resource provider to get certificates from it and install on cluster nodes:</span></span>

```powershell

    PS C:\Users\vturecek> New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment


    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all


    Tags                             :
```

<span data-ttu-id="64727-139">Pokud máte existující Key Vault, můžete ji povolit pro nasazení pomocí rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="64727-139">If you have an existing Key Vault, you can enable it for deployment using Azure CLI:</span></span>

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```


## <a name="add-certificates-to-key-vault"></a><span data-ttu-id="64727-140">Přidejte certifikáty do Key Vault</span><span class="sxs-lookup"><span data-stu-id="64727-140">Add certificates to Key Vault</span></span>
<span data-ttu-id="64727-141">Ve službě Service Fabric se k ověřování a šifrování pro zabezpečení různých aspektů clusteru a jeho aplikací využívají certifikáty.</span><span class="sxs-lookup"><span data-stu-id="64727-141">Certificates are used in Service Fabric to provide authentication and encryption to secure various aspects of a cluster and its applications.</span></span> <span data-ttu-id="64727-142">Další informace o tom, jak certifikáty se používají v Service Fabric najdete v tématu [scénáře zabezpečení clusteru Service Fabric][service-fabric-cluster-security].</span><span class="sxs-lookup"><span data-stu-id="64727-142">For more information on how certificates are used in Service Fabric, see [Service Fabric cluster security scenarios][service-fabric-cluster-security].</span></span>

### <a name="cluster-and-server-certificate-required"></a><span data-ttu-id="64727-143">Cluster a server certifikátu (povinné)</span><span class="sxs-lookup"><span data-stu-id="64727-143">Cluster and server certificate (required)</span></span>
<span data-ttu-id="64727-144">Tento certifikát je vyžadován k zabezpečení clusteru a zabránit neoprávněnému přístupu k němu.</span><span class="sxs-lookup"><span data-stu-id="64727-144">This certificate is required to secure a cluster and prevent unauthorized access to it.</span></span> <span data-ttu-id="64727-145">Poskytuje zabezpečení clusteru několik způsoby:</span><span class="sxs-lookup"><span data-stu-id="64727-145">It provides cluster security in a couple ways:</span></span>

* <span data-ttu-id="64727-146">**Ověřování clusteru:** ověřuje komunikaci mezi uzly pro cluster federaci.</span><span class="sxs-lookup"><span data-stu-id="64727-146">**Cluster authentication:** Authenticates node-to-node communication for cluster federation.</span></span> <span data-ttu-id="64727-147">Pouze uzly, které můžete prokázání své identity s tímto certifikátem může připojit ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="64727-147">Only nodes that can prove their identity with this certificate can join the cluster.</span></span>
* <span data-ttu-id="64727-148">**Ověření serveru:** ověřuje koncové body správy clusteru klient pro správu, tak, aby klient správy zná je rozhovoru s skutečné clusteru.</span><span class="sxs-lookup"><span data-stu-id="64727-148">**Server authentication:** Authenticates the cluster management endpoints to a management client, so that the management client knows it is talking to the real cluster.</span></span> <span data-ttu-id="64727-149">Tento certifikát také poskytuje SSL pro rozhraní API pro správu protokolu HTTPS a pro Service Fabric Explorer přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="64727-149">This certificate also provides SSL for the HTTPS management API and for Service Fabric Explorer over HTTPS.</span></span>

<span data-ttu-id="64727-150">Poskytovat tyto účely, certifikát musí splňovat následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="64727-150">To serve these purposes, the certificate must meet the following requirements:</span></span>

* <span data-ttu-id="64727-151">Certifikát musí obsahovat privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="64727-151">The certificate must contain a private key.</span></span>
* <span data-ttu-id="64727-152">Certifikát se musí vytvořit pro výměnu klíčů, exportovat do souboru Personal Information Exchange (.pfx).</span><span class="sxs-lookup"><span data-stu-id="64727-152">The certificate must be created for key exchange, exportable to a Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="64727-153">Název předmětu certifikátu musí odpovídat doménu použitou pro přístup ke clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="64727-153">The certificate's subject name must match the domain used to access the Service Fabric cluster.</span></span> <span data-ttu-id="64727-154">To je potřeba zajistit SSL pro koncové body správy protokolu HTTPS a Service Fabric Explorer clusteru.</span><span class="sxs-lookup"><span data-stu-id="64727-154">This is required to provide SSL for the cluster's HTTPS management endpoints and Service Fabric Explorer.</span></span> <span data-ttu-id="64727-155">Nelze získat certifikát SSL od certifikační autority (CA) pro `.cloudapp.azure.com` domény.</span><span class="sxs-lookup"><span data-stu-id="64727-155">You cannot obtain an SSL certificate from a certificate authority (CA) for the `.cloudapp.azure.com` domain.</span></span> <span data-ttu-id="64727-156">Získejte vlastní název domény pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="64727-156">Acquire a custom domain name for your cluster.</span></span> <span data-ttu-id="64727-157">Při žádosti o certifikát od certifikační Autority název subjektu certifikátu musí odpovídat vlastní název domény pro váš cluster používá.</span><span class="sxs-lookup"><span data-stu-id="64727-157">When you request a certificate from a CA the certificate's subject name must match the custom domain name used for your cluster.</span></span>

### <a name="client-authentication-certificates"></a><span data-ttu-id="64727-158">Certifikáty pro ověřování klientů</span><span class="sxs-lookup"><span data-stu-id="64727-158">Client authentication certificates</span></span>
<span data-ttu-id="64727-159">Další klientské certifikáty ověřit správci pro úlohy správy clusteru.</span><span class="sxs-lookup"><span data-stu-id="64727-159">Additional client certificates authenticate administrators for cluster management tasks.</span></span> <span data-ttu-id="64727-160">Service Fabric má dvě úrovně přístupu: **správce** a **jen pro čtení uživatele**.</span><span class="sxs-lookup"><span data-stu-id="64727-160">Service Fabric has two access levels: **admin** and **read-only user**.</span></span> <span data-ttu-id="64727-161">Minimálně je třeba použít jeden certifikát pro přístup pro správu.</span><span class="sxs-lookup"><span data-stu-id="64727-161">At minimum, a single certificate for administrative access should be used.</span></span> <span data-ttu-id="64727-162">Pro další přístupu na úrovni uživatele je třeba zadat samostatný certifikát.</span><span class="sxs-lookup"><span data-stu-id="64727-162">For additional user-level access, a separate certificate must be provided.</span></span> <span data-ttu-id="64727-163">Další informace o přístupu rolí najdete v tématu [řízení přístupu na základě rolí pro Service Fabric klienty][service-fabric-cluster-security-roles].</span><span class="sxs-lookup"><span data-stu-id="64727-163">For more information on access roles, see [role-based access control for Service Fabric clients][service-fabric-cluster-security-roles].</span></span>

<span data-ttu-id="64727-164">Není nutné nahrát certifikáty pro ověřování klientů pro Key Vault pro práci s Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="64727-164">You do not need to upload Client authentication certificates to Key Vault to work with Service Fabric.</span></span> <span data-ttu-id="64727-165">Tyto certifikáty pouze musí být zadané pro uživatele, kteří mají oprávnění pro správu clusteru.</span><span class="sxs-lookup"><span data-stu-id="64727-165">These certificates only need to be provided to users who are authorized for cluster management.</span></span> 

> [!NOTE]
> <span data-ttu-id="64727-166">Azure Active Directory je doporučeným způsobem k ověřování klientů pro operace správy clusterů.</span><span class="sxs-lookup"><span data-stu-id="64727-166">Azure Active Directory is the recommended way to authenticate clients for cluster management operations.</span></span> <span data-ttu-id="64727-167">Chcete-li používat Azure Active Directory, je potřeba [vytvořit cluster pomocí Azure Resource Manager][create-cluster-arm].</span><span class="sxs-lookup"><span data-stu-id="64727-167">To use Azure Active Directory, you must [create a cluster using Azure Resource Manager][create-cluster-arm].</span></span>
> 
> 

### <a name="application-certificates-optional"></a><span data-ttu-id="64727-168">Certifikáty aplikace (volitelné)</span><span class="sxs-lookup"><span data-stu-id="64727-168">Application certificates (optional)</span></span>
<span data-ttu-id="64727-169">Libovolný počet dalších certifikátů lze nainstalovat na clusteru pro aplikace z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="64727-169">Any number of additional certificates can be installed on a cluster for application security purposes.</span></span> <span data-ttu-id="64727-170">Před vytvořením clusteru, vezměte v úvahu scénáře zabezpečení aplikací, které vyžadují certifikát nainstalovaný na uzlech, například:</span><span class="sxs-lookup"><span data-stu-id="64727-170">Before creating your cluster, consider the application security scenarios that require a certificate to be installed on the nodes, such as:</span></span>

* <span data-ttu-id="64727-171">Šifrování a dešifrování hodnot konfigurace aplikace</span><span class="sxs-lookup"><span data-stu-id="64727-171">Encryption and decryption of application configuration values</span></span>
* <span data-ttu-id="64727-172">Šifrování dat mezi uzly během replikace</span><span class="sxs-lookup"><span data-stu-id="64727-172">Encryption of data across nodes during replication</span></span> 

<span data-ttu-id="64727-173">Při vytváření clusteru prostřednictvím portálu Azure se nedá nakonfigurovat certifikáty k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="64727-173">Application certificates cannot be configured when creating a cluster through the Azure portal.</span></span> <span data-ttu-id="64727-174">Konfigurace aplikace certifikáty během instalace clusteru, musíte [vytvořit cluster pomocí Azure Resource Manager][create-cluster-arm].</span><span class="sxs-lookup"><span data-stu-id="64727-174">To configure application certificates at cluster setup time, you must [create a cluster using Azure Resource Manager][create-cluster-arm].</span></span> <span data-ttu-id="64727-175">Certifikáty aplikace můžete také přidat do clusteru po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="64727-175">You can also add application certificates to the cluster after it has been created.</span></span>

### <a name="formatting-certificates-for-azure-resource-provider-use"></a><span data-ttu-id="64727-176">Certifikáty formátování pro použití poskytovatele prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="64727-176">Formatting certificates for Azure resource provider use</span></span>
<span data-ttu-id="64727-177">Soubory privátních klíčů (.pfx) můžete přidat a používat přímo přes Key Vault.</span><span class="sxs-lookup"><span data-stu-id="64727-177">Private key files (.pfx) can be added and used directly through Key Vault.</span></span> <span data-ttu-id="64727-178">Však poskytovatel prostředků Azure vyžaduje klíče ukládaly ve speciální formátu JSON, který zahrnuje .pfx jako base-64 kódovaný řetězec a heslo soukromého klíče.</span><span class="sxs-lookup"><span data-stu-id="64727-178">However, the Azure resource provider requires keys to be stored in a special JSON format that includes the .pfx as a base-64 encoded string and the private key password.</span></span> <span data-ttu-id="64727-179">Abychom vyhověli tyto požadavky, klíče musí být umístěn do řetězce formátu JSON a pak uloženy jako *tajné klíče* v Key Vault.</span><span class="sxs-lookup"><span data-stu-id="64727-179">To accommodate these requirements, keys must be placed in a JSON string and then stored as *secrets* in Key Vault.</span></span>

<span data-ttu-id="64727-180">Pro usnadnění tohoto procesu je modul prostředí PowerShell [dostupná na Githubu][service-fabric-rp-helpers].</span><span class="sxs-lookup"><span data-stu-id="64727-180">To make this process easier, a PowerShell module is [available on GitHub][service-fabric-rp-helpers].</span></span> <span data-ttu-id="64727-181">Postupujte podle těchto kroků, které chcete použít modul:</span><span class="sxs-lookup"><span data-stu-id="64727-181">Follow these steps to use the module:</span></span>

1. <span data-ttu-id="64727-182">Stáhněte si celý obsah úložiště do místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="64727-182">Download the entire contents of the repo into a local directory.</span></span> 
2. <span data-ttu-id="64727-183">Naimportujte modul v okně prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="64727-183">Import the module in your PowerShell window:</span></span>

```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
```

<span data-ttu-id="64727-184">`Invoke-AddCertToKeyVault` Příkaz v tento modul prostředí PowerShell automaticky formáty privátní klíč certifikátu do řetězce formátu JSON a odešle ji do služby Key Vault.</span><span class="sxs-lookup"><span data-stu-id="64727-184">The `Invoke-AddCertToKeyVault` command in this PowerShell module automatically formats a certificate private key into a JSON string and uploads it to Key Vault.</span></span> <span data-ttu-id="64727-185">Ho použijte k přidání certifikátu clusteru a všechny další aplikaci certifikáty do Key Vault.</span><span class="sxs-lookup"><span data-stu-id="64727-185">Use it to add the cluster certificate and any additional application certificates to Key Vault.</span></span> <span data-ttu-id="64727-186">Tento krok opakujte pro všechny další certifikáty, které chcete nainstalovat v clusteru.</span><span class="sxs-lookup"><span data-stu-id="64727-186">Repeat this step for any additional certificates you want to install in your cluster.</span></span>

```powershell
PS C:\Users\vturecek> Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet will be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault


Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

<span data-ttu-id="64727-187">Toto jsou všechny předpoklady Key Vault pro konfiguraci šablony správce prostředků clusteru Service Fabric, která nainstaluje certifikáty pro ověřování uzlu, zabezpečení koncového bodu správy a ověřování a funkcí zabezpečení další aplikace, které používají certifikáty X.509.</span><span class="sxs-lookup"><span data-stu-id="64727-187">These are all the Key Vault prerequisites for configuring a Service Fabric cluster Resource Manager template that installs certificates for node authentication, management endpoint security and authentication, and any additional application security features that use X.509 certificates.</span></span> <span data-ttu-id="64727-188">V tomto okamžiku teď byste měli mít následující nastavení v Azure:</span><span class="sxs-lookup"><span data-stu-id="64727-188">At this point, you should now have the following setup in Azure:</span></span>

* <span data-ttu-id="64727-189">Skupina prostředků Key Vault</span><span class="sxs-lookup"><span data-stu-id="64727-189">Key Vault resource group</span></span>
  * <span data-ttu-id="64727-190">Key Vault</span><span class="sxs-lookup"><span data-stu-id="64727-190">Key Vault</span></span>
    * <span data-ttu-id="64727-191">Ověřovací certifikát serverů clusteru</span><span class="sxs-lookup"><span data-stu-id="64727-191">Cluster server authentication certificate</span></span>

<span data-ttu-id="64727-192">< /a "Vytvoření cluster-portal" ></a></span><span class="sxs-lookup"><span data-stu-id="64727-192"></a "create-cluster-portal" ></a></span></span>

## <a name="create-cluster-in-the-azure-portal"></a><span data-ttu-id="64727-193">Vytvoření clusteru na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="64727-193">Create cluster in the Azure portal</span></span>
### <a name="search-for-the-service-fabric-cluster-resource"></a><span data-ttu-id="64727-194">Vyhledejte prostředek clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="64727-194">Search for the Service Fabric cluster resource</span></span>
![vyhledávání pro šablonu clusteru Service Fabric na portálu Azure.][SearchforServiceFabricClusterTemplate]

1. <span data-ttu-id="64727-196">Přihlaste se na web [Azure Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="64727-196">Sign in to the [Azure portal][azure-portal].</span></span>
2. <span data-ttu-id="64727-197">Klikněte na tlačítko **nový** o přidání nového prostředku šablony.</span><span class="sxs-lookup"><span data-stu-id="64727-197">Click **New** to add a new resource template.</span></span> <span data-ttu-id="64727-198">Vyhledejte šablony Service Fabric Cluster **Marketplace** pod **všechno, co**.</span><span class="sxs-lookup"><span data-stu-id="64727-198">Search for the Service Fabric Cluster template in the **Marketplace** under **Everything**.</span></span>
3. <span data-ttu-id="64727-199">Vyberte **Service Fabric Cluster** ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="64727-199">Select **Service Fabric Cluster** from the list.</span></span>
4. <span data-ttu-id="64727-200">Přejděte na **Service Fabric Cluster** okně klikněte na tlačítko **vytvořit**,</span><span class="sxs-lookup"><span data-stu-id="64727-200">Navigate to the **Service Fabric Cluster** blade, click **Create**,</span></span>
5. <span data-ttu-id="64727-201">**Cluster Service Fabric vytvořit** okno obsahuje následující čtyři kroky.</span><span class="sxs-lookup"><span data-stu-id="64727-201">The **Create Service Fabric cluster** blade has the following four steps.</span></span>

#### <a name="1-basics"></a><span data-ttu-id="64727-202">1. Základy</span><span class="sxs-lookup"><span data-stu-id="64727-202">1. Basics</span></span>
![Snímek obrazovky vytváření nové skupiny prostředků.][CreateRG]

<span data-ttu-id="64727-204">V okně základy budete muset zadat základní informace pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="64727-204">In the Basics blade you need to provide the basic details for your cluster.</span></span>

1. <span data-ttu-id="64727-205">Zadejte název clusteru.</span><span class="sxs-lookup"><span data-stu-id="64727-205">Enter the name of your cluster.</span></span>
2. <span data-ttu-id="64727-206">Zadejte **uživatelské jméno** a **heslo** pro vzdálené plochy pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="64727-206">Enter a **user name** and **password** for Remote Desktop for the VMs.</span></span>
3. <span data-ttu-id="64727-207">Je nutné vybrat **předplatné** má cluster pro nasazení do, zvláště pokud máte více předplatných.</span><span class="sxs-lookup"><span data-stu-id="64727-207">Make sure to select the **Subscription** that you want your cluster to be deployed to, especially if you have multiple subscriptions.</span></span>
4. <span data-ttu-id="64727-208">Vytvoření **novou skupinu prostředků**.</span><span class="sxs-lookup"><span data-stu-id="64727-208">Create a **new resource group**.</span></span> <span data-ttu-id="64727-209">Je nejlepší a použít stejný název jako cluster, protože pomáhá při hledání je později, zejména v případě, že se pokoušíte provést změny vašeho nasazení nebo odstranit cluster.</span><span class="sxs-lookup"><span data-stu-id="64727-209">It is best to give it the same name as the cluster, since it helps in finding them later, especially when you are trying to make changes to your deployment or delete your cluster.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="64727-210">I když můžete se rozhodnete použít existující skupinu prostředků, je vhodné vytvořit novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="64727-210">Although you can decide to use an existing resource group, it is a good practice to create a new resource group.</span></span> <span data-ttu-id="64727-211">To usnadňuje odstranění clusterů, které nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="64727-211">This makes it easy to delete clusters that you do not need.</span></span>
   > 
   > 
5. <span data-ttu-id="64727-212">Vyberte **oblast** ve kterém chcete vytvořit cluster.</span><span class="sxs-lookup"><span data-stu-id="64727-212">Select the **region** in which you want to create the cluster.</span></span> <span data-ttu-id="64727-213">Je nutné použít stejné oblasti, kterou Key Vault je v.</span><span class="sxs-lookup"><span data-stu-id="64727-213">You must use the same region that your Key Vault is in.</span></span>

#### <a name="2-cluster-configuration"></a><span data-ttu-id="64727-214">2. Konfigurace clusteru</span><span class="sxs-lookup"><span data-stu-id="64727-214">2. Cluster configuration</span></span>
![Vytvoření typu uzlu][CreateNodeType]

<span data-ttu-id="64727-216">Nakonfigurujte uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="64727-216">Configure your cluster nodes.</span></span> <span data-ttu-id="64727-217">Typy uzlů definovat velikosti virtuálních počítačů, počet virtuálních počítačů a jejich vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="64727-217">Node types define the VM sizes, the number of VMs, and their properties.</span></span> <span data-ttu-id="64727-218">Cluster může mít více než jeden typ uzlu, ale typ primárního uzlu (první, kterou definujete na portálu) musí mít aspoň pět virtuálních počítačů, jedná se o typ uzlu, kde jsou umístěny Service Fabric systémových služeb.</span><span class="sxs-lookup"><span data-stu-id="64727-218">Your cluster can have more than one node type, but the primary node type (the first one that you define on the portal) must have at least five VMs, as this is the node type where Service Fabric system services are placed.</span></span> <span data-ttu-id="64727-219">Nekonfigurujte **vlastnosti umístění** vzhledem k tomu je automaticky přidá výchozí umístění vlastnost "NodeTypeName".</span><span class="sxs-lookup"><span data-stu-id="64727-219">Do not configure **Placement Properties** because a default placement property of "NodeTypeName" is added automatically.</span></span>

> [!NOTE]
> <span data-ttu-id="64727-220">Běžný scénář pro více typů uzlu je aplikace, která obsahuje službu front-endu a back endové službě.</span><span class="sxs-lookup"><span data-stu-id="64727-220">A common scenario for multiple node types is an application that contains a front-end service and a back-end service.</span></span> <span data-ttu-id="64727-221">Chcete umístit front-endová služba menší virtuálních počítačů (velikosti virtuálních počítačů jako D2) s porty otevřené k Internetu, ale chcete umístit na větší virtuální počítače (s velikostí virtuálního počítače jako D4, D6, D15 a tak dále) bez internetového portů otevřete back endové službě.</span><span class="sxs-lookup"><span data-stu-id="64727-221">You want to put the front-end service on smaller VMs (VM sizes like D2) with ports open to the Internet, but you want to put the back-end service on larger VMs (with VM sizes like D4, D6, D15, and so on) with no Internet-facing ports open.</span></span>
> 
> 

1. <span data-ttu-id="64727-222">Zvolte název pro váš typ uzlu (obsahující pouze písmena a čísla 1 až 12 znaků).</span><span class="sxs-lookup"><span data-stu-id="64727-222">Choose a name for your node type (1 to 12 characters containing only letters and numbers).</span></span>
2. <span data-ttu-id="64727-223">Minimální **velikost** virtuálních počítačů pro primární uzel typu vycházejí z **odolnost** vrstvy, které zvolíte pro cluster.</span><span class="sxs-lookup"><span data-stu-id="64727-223">The minimum **size** of VMs for the primary node type is driven by the **durability** tier you choose for the cluster.</span></span> <span data-ttu-id="64727-224">Výchozí hodnota pro úroveň odolnosti je Bronzová.</span><span class="sxs-lookup"><span data-stu-id="64727-224">The default for the durability tier is bronze.</span></span> <span data-ttu-id="64727-225">Další informace o odolnost, najdete v části [jak zvolit spolehlivost clusteru Service Fabric a odolnost][service-fabric-cluster-capacity].</span><span class="sxs-lookup"><span data-stu-id="64727-225">For more information on durability, see [how to choose the Service Fabric cluster reliability and durability][service-fabric-cluster-capacity].</span></span>
3. <span data-ttu-id="64727-226">Vyberte velikost virtuálního počítače a cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="64727-226">Select the VM size and pricing tier.</span></span> <span data-ttu-id="64727-227">Virtuální počítače D-series jednotek SSD a jsou doporučovány stavových aplikací.</span><span class="sxs-lookup"><span data-stu-id="64727-227">D-series VMs have SSD drives and are highly recommended for stateful applications.</span></span> <span data-ttu-id="64727-228">Použít všechny virtuálních počítačů SKU, který má částečné jader nebo mají méně než 7 GB kapacity disku.</span><span class="sxs-lookup"><span data-stu-id="64727-228">Do not use any VM SKU that has partial cores or have less than 7 GB of available disk capacity.</span></span> 
4. <span data-ttu-id="64727-229">Minimální **číslo** virtuálních počítačů pro primární uzel typu vycházejí z **spolehlivost** vrstvy, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="64727-229">The minimum **number** of VMs for the primary node type is driven by the **reliability** tier you choose.</span></span> <span data-ttu-id="64727-230">Výchozí hodnota pro úroveň spolehlivosti je Silver.</span><span class="sxs-lookup"><span data-stu-id="64727-230">The default for the reliability tier is Silver.</span></span> <span data-ttu-id="64727-231">Další informace o spolehlivosti, najdete v části [jak zvolit spolehlivost clusteru Service Fabric a odolnost][service-fabric-cluster-capacity].</span><span class="sxs-lookup"><span data-stu-id="64727-231">For more information on reliability, see [how to choose the Service Fabric cluster reliability and durability][service-fabric-cluster-capacity].</span></span>
5. <span data-ttu-id="64727-232">Zvolte počet virtuálních počítačů pro typ uzlu.</span><span class="sxs-lookup"><span data-stu-id="64727-232">Choose the number of VMs for the node type.</span></span> <span data-ttu-id="64727-233">Je možné škálovat později na nahoru nebo dolů počet virtuálních počítačů v uzlu typu, ale na primárním uzlu, který typ minimální doprovází úroveň spolehlivosti, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="64727-233">You can scale up or down the number of VMs in a node type later on, but on the primary node type, the minimum is driven by the reliability level that you have chosen.</span></span> <span data-ttu-id="64727-234">Ostatní typy uzlů může mít minimálně 1 virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="64727-234">Other node types can have a minimum of 1 VM.</span></span>
6. <span data-ttu-id="64727-235">Nakonfigurujte vlastní koncové body.</span><span class="sxs-lookup"><span data-stu-id="64727-235">Configure custom endpoints.</span></span> <span data-ttu-id="64727-236">Toto pole umožňuje zadat seznam portů, které chcete zpřístupnit prostřednictvím nástroje pro vyrovnávání zatížení Azure do veřejného Internetu pro vaše aplikace oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="64727-236">This field allows you to enter a comma-separated list of ports that you want to expose through the Azure Load Balancer to the public Internet for your applications.</span></span> <span data-ttu-id="64727-237">Například pokud plánujete nasadit webovou aplikaci pro váš cluster, zadejte "80" zde Pokud chcete povolit přenosy na portu 80 do clusteru.</span><span class="sxs-lookup"><span data-stu-id="64727-237">For example, if you plan to deploy a web application to your cluster, enter "80" here to allow traffic on port 80 into your cluster.</span></span> <span data-ttu-id="64727-238">Další informace o koncových bodů najdete v tématu [komunikaci s aplikacemi][service-fabric-connect-and-communicate-with-services]</span><span class="sxs-lookup"><span data-stu-id="64727-238">For more information on endpoints, see [communicating with applications][service-fabric-connect-and-communicate-with-services]</span></span>
7. <span data-ttu-id="64727-239">Konfigurace clusteru **diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="64727-239">Configure cluster **diagnostics**.</span></span> <span data-ttu-id="64727-240">Ve výchozím nastavení jsou diagnostiky povolené v clusteru, vám pomůže při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="64727-240">By default, diagnostics are enabled on your cluster to assist with troubleshooting issues.</span></span> <span data-ttu-id="64727-241">Pokud chcete zakázat diagnostiky změnu **stav** přepnutím **vypnout**.</span><span class="sxs-lookup"><span data-stu-id="64727-241">If you want to disable diagnostics change the **Status** toggle to **Off**.</span></span> <span data-ttu-id="64727-242">Vypnutí možnosti diagnostiky je **není** nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="64727-242">Turning off diagnostics is **not** recommended.</span></span>
8. <span data-ttu-id="64727-243">Vyberte režim upgradu prostředků infrastruktury, které chcete nastavit váš cluster.</span><span class="sxs-lookup"><span data-stu-id="64727-243">Select the Fabric upgrade mode you want set your cluster to.</span></span> <span data-ttu-id="64727-244">Vyberte **automatické**, pokud chcete, aby systém automaticky vyzvedne, až na nejnovější dostupnou verzi a pokuste se upgrade clusteru na ni.</span><span class="sxs-lookup"><span data-stu-id="64727-244">Select **Automatic**, if you want the system to automatically pick up the latest available version and try to upgrade your cluster to it.</span></span> <span data-ttu-id="64727-245">Nastavit režim **ruční**, pokud chcete zvolit podporovanou verzi.</span><span class="sxs-lookup"><span data-stu-id="64727-245">Set the mode to **Manual**, if you want to choose a supported version.</span></span>

> [!NOTE]
> <span data-ttu-id="64727-246">Podporujeme pouze clustery, které běží podporovaná verze služby prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="64727-246">We support only clusters that are running supported versions of service Fabric.</span></span> <span data-ttu-id="64727-247">Výběrem **ruční** režimu přenášíte na odpovědnost cluster upgradovat na podporovanou verzi.</span><span class="sxs-lookup"><span data-stu-id="64727-247">By selecting the **Manual** mode, you are taking on the responsibility to upgrade your cluster to a supported version.</span></span> <span data-ttu-id="64727-248">Pro další informace o prostředcích infrastruktury upgradu naleznete v režimu [service fabric clusteru upgrade dokumentu.][service-fabric-cluster-upgrade]</span><span class="sxs-lookup"><span data-stu-id="64727-248">For more details on the Fabric upgrade mode see the [service-fabric-cluster-upgrade document.][service-fabric-cluster-upgrade]</span></span>
> 
> 

#### <a name="3-security"></a><span data-ttu-id="64727-249">3. Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="64727-249">3. Security</span></span>
![Snímek obrazovky konfigurace zabezpečení na portálu Azure.][SecurityConfigs]

<span data-ttu-id="64727-251">Posledním krokem je poskytnout informace o certifikátu k zabezpečení clusteru pomocí Key Vault a certifikát informace vytvořený dříve.</span><span class="sxs-lookup"><span data-stu-id="64727-251">The final step is to provide certificate information to secure the cluster using the Key Vault and certificate information created earlier.</span></span>

* <span data-ttu-id="64727-252">Vyplňte pole primární certifikát s výstupem získané z odesílání **clusteru certifikát** pomocí Key Vault `Invoke-AddCertToKeyVault` příkaz prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="64727-252">Populate the primary certificate fields with the output obtained from uploading the **cluster certificate** to Key Vault using the `Invoke-AddCertToKeyVault` PowerShell command.</span></span>

```powershell
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47
```

* <span data-ttu-id="64727-253">Zkontrolujte **konfigurovat upřesňující nastavení** pole k zadání klientské certifikáty pro **správce klienta** a **jen pro čtení klienta**.</span><span class="sxs-lookup"><span data-stu-id="64727-253">Check the **Configure advanced settings** box to enter client certificates for **admin client** and **read-only client**.</span></span> <span data-ttu-id="64727-254">V těchto polích zadejte kryptografický otisk certifikátu klienta správce a kryptografický otisk certifikátu klienta uživatele jen pro čtení, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="64727-254">In these fields, enter the thumbprint of your admin client certificate and the thumbprint of your read-only user client certificate, if applicable.</span></span> <span data-ttu-id="64727-255">Když se správce pokusí připojit ke clusteru, získají přístup pouze v případě, že mají certifikát s kryptografickým otiskem, který odpovídá hodnoty kryptografického otisku zadali tady.</span><span class="sxs-lookup"><span data-stu-id="64727-255">When administrators attempt to connect to the cluster, they are granted access only if they have a certificate with a thumbprint that matches the thumbprint values entered here.</span></span>  

#### <a name="4-summary"></a><span data-ttu-id="64727-256">4. Souhrn</span><span class="sxs-lookup"><span data-stu-id="64727-256">4. Summary</span></span>
![<span data-ttu-id="64727-257">Snímek obrazovky úvodní panel zobrazení "Nasazení Cluster Service Fabric."</span><span class="sxs-lookup"><span data-stu-id="64727-257">Screen shot of the start board displaying "Deploying Service Fabric Cluster."</span></span> ][Notifications]

<span data-ttu-id="64727-258">Chcete-li dokončit vytvoření clusteru, klikněte na tlačítko **Souhrn** najdete v části konfigurace, které jste zadali, nebo stáhnout šablony Azure Resource Manager, který používá k nasazení clusteru.</span><span class="sxs-lookup"><span data-stu-id="64727-258">To complete the cluster creation, click **Summary** to see the configurations that you have provided, or download the Azure Resource Manager template that that used to deploy your cluster.</span></span> <span data-ttu-id="64727-259">Poté, co jste zadali povinná nastavení **OK** bude zelené tlačítko a procesu vytváření clusteru můžete spustit kliknutím.</span><span class="sxs-lookup"><span data-stu-id="64727-259">After you have provided the mandatory settings, the **OK** button becomes green and you can start the cluster creation process by clicking it.</span></span>

<span data-ttu-id="64727-260">Průběh vytváření můžete sledovat v oznámeních.</span><span class="sxs-lookup"><span data-stu-id="64727-260">You can see the creation progress in the notifications.</span></span> <span data-ttu-id="64727-261">(Klikněte na ikonu zvonku u stavového řádku v pravém horním rohu obrazovky.) Pokud jste klikli na **připnout na úvodní panel** při vytváření clusteru, zobrazí se **nasazení Cluster Service Fabric** připnuli k **spustit** panelu.</span><span class="sxs-lookup"><span data-stu-id="64727-261">(Click the "Bell" icon near the status bar at the upper right of your screen.) If you clicked **Pin to Startboard** while creating the cluster, you will see **Deploying Service Fabric Cluster** pinned to the **Start** board.</span></span>

### <a name="view-your-cluster-status"></a><span data-ttu-id="64727-262">Zobrazení stavu clusteru</span><span class="sxs-lookup"><span data-stu-id="64727-262">View your cluster status</span></span>
![Snímek obrazovky podrobnosti o clusteru v řídicím panelu.][ClusterDashboard]

<span data-ttu-id="64727-264">Po vytvoření clusteru si můžete prohlédnout cluster v portálu:</span><span class="sxs-lookup"><span data-stu-id="64727-264">Once your cluster is created, you can inspect your cluster in the portal:</span></span>

1. <span data-ttu-id="64727-265">Přejděte na **Procházet** a klikněte na tlačítko **clustery infrastruktury služby**.</span><span class="sxs-lookup"><span data-stu-id="64727-265">Go to **Browse** and click **Service Fabric Clusters**.</span></span>
2. <span data-ttu-id="64727-266">Vyhledejte cluster a klikněte na něj.</span><span class="sxs-lookup"><span data-stu-id="64727-266">Locate your cluster and click it.</span></span>
3. <span data-ttu-id="64727-267">Na řídicím panelu se zobrazí podrobnosti o clusteru, včetně veřejného koncového bodu clusteru a odkaz na Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="64727-267">You can now see the details of your cluster in the dashboard, including the cluster's public endpoint and a link to Service Fabric Explorer.</span></span>

<span data-ttu-id="64727-268">**Uzlu monitorování** část v okně řídicí panel clusteru označuje počet virtuálních počítačů, které jsou v pořádku a není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="64727-268">The **Node Monitor** section on the cluster's dashboard blade indicates the number of VMs that are healthy and not healthy.</span></span> <span data-ttu-id="64727-269">Můžete najít další podrobnosti o stavu clusteru na [Service Fabric stavu modelu ÚVOD][service-fabric-health-introduction].</span><span class="sxs-lookup"><span data-stu-id="64727-269">You can find more details about the cluster's health at [Service Fabric health model introduction][service-fabric-health-introduction].</span></span>

> [!NOTE]
> <span data-ttu-id="64727-270">Clusterů Service Fabric vyžadovat určité počet uzlů na být až vždycky zachování dostupnosti a zároveň zachovat stav – označuje jako "Správa kvora".</span><span class="sxs-lookup"><span data-stu-id="64727-270">Service Fabric clusters require a certain number of nodes to be up always to maintain availability and preserve state - referred to as "maintaining quorum".</span></span> <span data-ttu-id="64727-271">Proto, je obvykle není bezpečné vypnout všechny počítače v clusteru, pokud jste provedli nejprve [úplného zálohování vaší stavu][service-fabric-reliable-services-backup-restore].</span><span class="sxs-lookup"><span data-stu-id="64727-271">Therfore, it is typically not safe to shut down all machines in the cluster unless you have first performed a [full backup of your state][service-fabric-reliable-services-backup-restore].</span></span>
> 
> 

## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a><span data-ttu-id="64727-272">Vzdálené připojení k instanci a sadu škálování virtuálního počítače nebo uzlu clusteru</span><span class="sxs-lookup"><span data-stu-id="64727-272">Remote connect to a Virtual Machine Scale Set instance or a cluster node</span></span>
<span data-ttu-id="64727-273">Každý z NodeTypes zadáte v clusteru výsledky do sady škálování virtuálního počítače získávání nastavení.</span><span class="sxs-lookup"><span data-stu-id="64727-273">Each of the NodeTypes you specify in your cluster results in a Virtual Machine Scale Set getting set-up.</span></span> <span data-ttu-id="64727-274">V tématu [vzdálené připojení k instanci sadu škálování virtuálního počítače] [ remote-connect-to-a-vm-scale-set] podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="64727-274">See [Remote connect to a Virtual Machine Scale Set instance][remote-connect-to-a-vm-scale-set] for details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="64727-275">Další kroky</span><span class="sxs-lookup"><span data-stu-id="64727-275">Next steps</span></span>
<span data-ttu-id="64727-276">V tuto chvíli máte zabezpečené cluster pro správu ověřování pomocí certifikátů.</span><span class="sxs-lookup"><span data-stu-id="64727-276">At this point, you have a secure cluster using certificates for management authentication.</span></span> <span data-ttu-id="64727-277">Dále [připojit ke clusteru](service-fabric-connect-to-secure-cluster.md) a zjistěte, jak [spravovat tajné klíče aplikace](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="64727-277">Next, [connect to your cluster](service-fabric-connect-to-secure-cluster.md) and learn how to [manage application secrets](service-fabric-application-secret-management.md).</span></span>  <span data-ttu-id="64727-278">Také další informace o [možnosti podpory Service Fabric](service-fabric-support.md).</span><span class="sxs-lookup"><span data-stu-id="64727-278">Also, learn about [Service Fabric support options](service-fabric-support.md).</span></span>

<!-- Links -->
[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[azure-portal]: https://portal.azure.com/
[key-vault-get-started]: ../key-vault/key-vault-get-started.md
[create-cluster-arm]: service-fabric-cluster-creation-via-arm.md
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-cluster-security-roles]: service-fabric-cluster-security-roles.md
[service-fabric-cluster-capacity]: service-fabric-cluster-capacity.md
[service-fabric-connect-and-communicate-with-services]: service-fabric-connect-and-communicate-with-services.md
[service-fabric-health-introduction]: service-fabric-health-introduction.md
[service-fabric-reliable-services-backup-restore]: service-fabric-reliable-services-backup-restore.md
[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md#remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node
[service-fabric-cluster-upgrade]: service-fabric-cluster-upgrade.md

<!--Image references-->
[SearchforServiceFabricClusterTemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png
[CreateRG]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png
[CreateNodeType]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
