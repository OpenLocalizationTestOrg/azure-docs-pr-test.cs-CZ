---
title: "cluster Service Fabric aaaCreate v hello portálu Azure | Microsoft Docs"
description: "Tento článek popisuje, jak hello tooset zabezpečení clusteru Service Fabric v Azure pomocí portálu Azure a Azure Key Vault."
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
ms.openlocfilehash: 045f71b491260e741ce7a54a75c440e1b33059a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-fabric-cluster-in-azure-using-hello-azure-portal"></a><span data-ttu-id="6e573-103">Vytvořit cluster Service Fabric v Azure pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6e573-103">Create a Service Fabric cluster in Azure using hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6e573-104">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6e573-104">Azure Resource Manager</span></span>](service-fabric-cluster-creation-via-arm.md)
> * [<span data-ttu-id="6e573-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6e573-105">Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
> 
> 

<span data-ttu-id="6e573-106">Toto je podrobný průvodce, který vás provede kroky hello nastavení zabezpečení clusteru Service Fabric v Azure pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6e573-106">This is a step-by-step guide that walks you through hello steps of setting up a secure Service Fabric cluster in Azure using hello Azure portal.</span></span> <span data-ttu-id="6e573-107">Tento průvodce vás provede hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6e573-107">This guide walks you through hello following steps:</span></span>

* <span data-ttu-id="6e573-108">Nastavení klíče toomanage Key Vault pro zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e573-108">Set up Key Vault toomanage keys for cluster security.</span></span>
* <span data-ttu-id="6e573-109">Vytvoření zabezpečeného clusteru v Azure prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6e573-109">Create a secured cluster in Azure through hello Azure portal.</span></span>
* <span data-ttu-id="6e573-110">Ověřování pomocí certifikátů správci.</span><span class="sxs-lookup"><span data-stu-id="6e573-110">Authenticate administrators using certificates.</span></span>

> [!NOTE]
> <span data-ttu-id="6e573-111">Pro pokročilejší možnosti zabezpečení, jako je například ověřování uživatelů s Azure Active Directory a nastavení certifikátů pro zabezpečení aplikací [vytvořit cluster pomocí Azure Resource Manager][create-cluster-arm].</span><span class="sxs-lookup"><span data-stu-id="6e573-111">For more advanced security options, such as user authentication with Azure Active Directory and setting up certificates for application security, [create your cluster using Azure Resource Manager][create-cluster-arm].</span></span>
> 
> 

<span data-ttu-id="6e573-112">Cluster s podporou zabezpečení je clusteru, který brání neoprávněným přístupem toomanagement operace, která zahrnuje nasazení, upgrade a odstranění aplikací, služeb a hello data, která obsahují.</span><span class="sxs-lookup"><span data-stu-id="6e573-112">A secure cluster is a cluster that prevents unauthorized access toomanagement operations, which includes deploying, upgrading, and deleting applications, services, and hello data they contain.</span></span> <span data-ttu-id="6e573-113">Nezabezpečená clusteru je cluster, že každý, kdo připojit tooat kdykoli a provádět operace správy.</span><span class="sxs-lookup"><span data-stu-id="6e573-113">An unsecure cluster is a cluster that anyone can connect tooat any time and perform management operations.</span></span> <span data-ttu-id="6e573-114">Přestože je možné toocreate nezabezpečená clusteru, je **důrazně doporučujeme toocreate cluster zabezpečené**.</span><span class="sxs-lookup"><span data-stu-id="6e573-114">Although it is possible toocreate an unsecure cluster, it is **highly recommended toocreate a secure cluster**.</span></span> <span data-ttu-id="6e573-115">Nezabezpečená clusteru **nelze zabezpečit později** -je nutné vytvořit nový cluster.</span><span class="sxs-lookup"><span data-stu-id="6e573-115">An unsecure cluster **cannot be secured later** - a new cluster must be created.</span></span>

<span data-ttu-id="6e573-116">Koncepty Hello jsou hello stejné pro vytvoření zabezpečeného clusterů, ať už hello clustery jsou clusterů systému Linux nebo Windows clusterů.</span><span class="sxs-lookup"><span data-stu-id="6e573-116">hello concepts are hello same for creating secure clusters, whether hello clusters are Linux clusters or Windows clusters.</span></span> <span data-ttu-id="6e573-117">Další informace a pomocné rutiny skripty pro vytvoření zabezpečeného clusterů systému Linux, najdete v tématu [vytváření zabezpečené clusterů v systému Linux](service-fabric-cluster-creation-via-arm.md#secure-linux-clusters).</span><span class="sxs-lookup"><span data-stu-id="6e573-117">For more information and helper scripts for creating secure Linux clusters, please see [Creating secure clusters on Linux](service-fabric-cluster-creation-via-arm.md#secure-linux-clusters).</span></span> <span data-ttu-id="6e573-118">Hello parametry získané skriptem hello Pomocník poskytuje může být vstup přímo na portálu hello jak je popsáno v části hello [vytvořit cluster v hello portál Azure](#create-cluster-portal).</span><span class="sxs-lookup"><span data-stu-id="6e573-118">hello parameters obtained by hello helper script provided can be input directly into hello portal as described in hello section [Create a cluster in hello Azure portal](#create-cluster-portal).</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="6e573-119">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="6e573-119">Log in tooAzure</span></span>
<span data-ttu-id="6e573-120">Tato příručka používá [prostředí Azure PowerShell][azure-powershell].</span><span class="sxs-lookup"><span data-stu-id="6e573-120">This guide uses [Azure PowerShell][azure-powershell].</span></span> <span data-ttu-id="6e573-121">Při spouštění novou relaci prostředí PowerShell, přihlaste se tooyour účet Azure a vybrat své předplatné před provedením Azure příkazy.</span><span class="sxs-lookup"><span data-stu-id="6e573-121">When starting a new PowerShell session, log in tooyour Azure account and select your subscription before executing Azure commands.</span></span>

<span data-ttu-id="6e573-122">Přihlaste se účtem tooyour azure:</span><span class="sxs-lookup"><span data-stu-id="6e573-122">Log in tooyour azure account:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="6e573-123">Vyberte předplatné:</span><span class="sxs-lookup"><span data-stu-id="6e573-123">Select your subscription:</span></span>

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a><span data-ttu-id="6e573-124">Nastavení služby Key Vault</span><span class="sxs-lookup"><span data-stu-id="6e573-124">Set up Key Vault</span></span>
<span data-ttu-id="6e573-125">Tato část hello Průvodce vás provede procesem vytváření Key Vault pro cluster Service Fabric v Azure a aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6e573-125">This part of hello guide walks you through creating a Key Vault for a Service Fabric cluster in Azure and for Service Fabric applications.</span></span> <span data-ttu-id="6e573-126">Dokončení průvodce v Key Vault, najdete v části hello [Key Vault Příručka Začínáme][key-vault-get-started].</span><span class="sxs-lookup"><span data-stu-id="6e573-126">For a complete guide on Key Vault, see hello [Key Vault getting started guide][key-vault-get-started].</span></span>

<span data-ttu-id="6e573-127">Service Fabric používá toosecure certifikáty X.509 clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e573-127">Service Fabric uses X.509 certificates toosecure a cluster.</span></span> <span data-ttu-id="6e573-128">Azure Key Vault je použité toomanage certifikáty pro clusterů Service Fabric v Azure.</span><span class="sxs-lookup"><span data-stu-id="6e573-128">Azure Key Vault is used toomanage certificates for Service Fabric clusters in Azure.</span></span> <span data-ttu-id="6e573-129">Pokud cluster je nasazené v Azure, poskytovatel prostředků Azure hello zodpovědný za vytváření clusterů Service Fabric vyžaduje certifikáty od Key Vault a nainstaluje je hello clusteru virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6e573-129">When a cluster is deployed in Azure, hello Azure resource provider responsible for creating Service Fabric clusters pulls certificates from Key Vault and installs them on hello cluster VMs.</span></span>

<span data-ttu-id="6e573-130">Hello následující diagram znázorňuje hello vztah mezi Key Vault, cluster Service Fabric a hello poskytovatel prostředků Azure, která používá certifikáty uložené v Key Vault, při vytváření clusteru:</span><span class="sxs-lookup"><span data-stu-id="6e573-130">hello following diagram illustrates hello relationship between Key Vault, a Service Fabric cluster, and hello Azure resource provider that uses certificates stored in Key Vault when it creates a cluster:</span></span>

![Instalace certifikátu][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a><span data-ttu-id="6e573-132">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="6e573-132">Create a Resource Group</span></span>
<span data-ttu-id="6e573-133">prvním krokem Hello je toocreate novou skupinu prostředků speciálně pro Key Vault.</span><span class="sxs-lookup"><span data-stu-id="6e573-133">hello first step is toocreate a new resource group specifically for Key Vault.</span></span> <span data-ttu-id="6e573-134">Uvedení Key Vault do vlastní skupiny prostředků se doporučuje, aby můžete odebrat skupiny prostředků výpočetního prostředí a úložiště – například hello skupiny prostředků, který má cluster Service Fabric – bez ztráty klíče a tajné klíče.</span><span class="sxs-lookup"><span data-stu-id="6e573-134">Putting Key Vault into its own resource group is recommended so that you can remove compute and storage resource groups - such as hello resource group that has your Service Fabric cluster - without losing your keys and secrets.</span></span> <span data-ttu-id="6e573-135">Skupina prostředků Hello, který má Key Vault musí být v hello stejné oblasti jako hello clusteru, který se používá.</span><span class="sxs-lookup"><span data-stu-id="6e573-135">hello resource group that has your Key Vault must be in hello same region as hello cluster that is using it.</span></span>

```powershell

    PS C:\Users\vturecek> New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: hello output object type of this cmdlet will be modified in a future release.

    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a><span data-ttu-id="6e573-136">Vytvoření trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="6e573-136">Create Key Vault</span></span>
<span data-ttu-id="6e573-137">Vytvoření trezoru klíč v hello novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6e573-137">Create a Key Vault in hello new resource group.</span></span> <span data-ttu-id="6e573-138">Hello Key Vault **musí být povolen pro nasazení** tooallow hello Service Fabric prostředků zprostředkovatele tooget certifikáty z něj a nainstalujte na uzly clusteru:</span><span class="sxs-lookup"><span data-stu-id="6e573-138">hello Key Vault **must be enabled for deployment** tooallow hello Service Fabric resource provider tooget certificates from it and install on cluster nodes:</span></span>

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
                                       Permissions tooKeys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions tooSecrets   :    all


    Tags                             :
```

<span data-ttu-id="6e573-139">Pokud máte existující Key Vault, můžete ji povolit pro nasazení pomocí rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="6e573-139">If you have an existing Key Vault, you can enable it for deployment using Azure CLI:</span></span>

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```


## <a name="add-certificates-tookey-vault"></a><span data-ttu-id="6e573-140">Přidejte certifikáty tooKey trezoru</span><span class="sxs-lookup"><span data-stu-id="6e573-140">Add certificates tooKey Vault</span></span>
<span data-ttu-id="6e573-141">Certifikáty se používají v Service Fabric tooprovide ověřování a šifrování toosecure různé aspekty cluster a jeho aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e573-141">Certificates are used in Service Fabric tooprovide authentication and encryption toosecure various aspects of a cluster and its applications.</span></span> <span data-ttu-id="6e573-142">Další informace o tom, jak certifikáty se používají v Service Fabric najdete v tématu [scénáře zabezpečení clusteru Service Fabric][service-fabric-cluster-security].</span><span class="sxs-lookup"><span data-stu-id="6e573-142">For more information on how certificates are used in Service Fabric, see [Service Fabric cluster security scenarios][service-fabric-cluster-security].</span></span>

### <a name="cluster-and-server-certificate-required"></a><span data-ttu-id="6e573-143">Cluster a server certifikátu (povinné)</span><span class="sxs-lookup"><span data-stu-id="6e573-143">Cluster and server certificate (required)</span></span>
<span data-ttu-id="6e573-144">Tento certifikát je požadovaná toosecure cluster a zabránit tooit neoprávněného přístupu.</span><span class="sxs-lookup"><span data-stu-id="6e573-144">This certificate is required toosecure a cluster and prevent unauthorized access tooit.</span></span> <span data-ttu-id="6e573-145">Poskytuje zabezpečení clusteru několik způsoby:</span><span class="sxs-lookup"><span data-stu-id="6e573-145">It provides cluster security in a couple ways:</span></span>

* <span data-ttu-id="6e573-146">**Ověřování clusteru:** ověřuje komunikaci mezi uzly pro cluster federaci.</span><span class="sxs-lookup"><span data-stu-id="6e573-146">**Cluster authentication:** Authenticates node-to-node communication for cluster federation.</span></span> <span data-ttu-id="6e573-147">Pouze uzly, které můžete prokázání své identity s tímto certifikátem můžete připojit hello cluster.</span><span class="sxs-lookup"><span data-stu-id="6e573-147">Only nodes that can prove their identity with this certificate can join hello cluster.</span></span>
* <span data-ttu-id="6e573-148">**Ověření serveru:** ověřuje hello clusteru správy koncové body tooa správy klienta, tak, aby hello klienta pro správu zná ho je rozhovoru toohello skutečné clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e573-148">**Server authentication:** Authenticates hello cluster management endpoints tooa management client, so that hello management client knows it is talking toohello real cluster.</span></span> <span data-ttu-id="6e573-149">Tento certifikát také poskytuje protokol SSL pro hello rozhraní API pro správu protokolu HTTPS a pro Service Fabric Explorer přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6e573-149">This certificate also provides SSL for hello HTTPS management API and for Service Fabric Explorer over HTTPS.</span></span>

<span data-ttu-id="6e573-150">tooserve tyto účely, hello certifikátu musí splňovat následující požadavky hello:</span><span class="sxs-lookup"><span data-stu-id="6e573-150">tooserve these purposes, hello certificate must meet hello following requirements:</span></span>

* <span data-ttu-id="6e573-151">Hello certifikát musí obsahovat privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="6e573-151">hello certificate must contain a private key.</span></span>
* <span data-ttu-id="6e573-152">Hello certifikátu musí být vytvořeny pro výměnu klíčů, exportovatelný tooa soubor Personal Information Exchange (.pfx).</span><span class="sxs-lookup"><span data-stu-id="6e573-152">hello certificate must be created for key exchange, exportable tooa Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="6e573-153">Hello názvu subjektu certifikátu musí odpovídat cluster Service Fabric hello domény použít tooaccess hello.</span><span class="sxs-lookup"><span data-stu-id="6e573-153">hello certificate's subject name must match hello domain used tooaccess hello Service Fabric cluster.</span></span> <span data-ttu-id="6e573-154">Toto je požadovaná tooprovide SSL pro koncové body správy protokolu HTTPS a Service Fabric Explorer hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e573-154">This is required tooprovide SSL for hello cluster's HTTPS management endpoints and Service Fabric Explorer.</span></span> <span data-ttu-id="6e573-155">Nelze získat certifikát SSL od certifikační autority (CA) pro hello `.cloudapp.azure.com` domény.</span><span class="sxs-lookup"><span data-stu-id="6e573-155">You cannot obtain an SSL certificate from a certificate authority (CA) for hello `.cloudapp.azure.com` domain.</span></span> <span data-ttu-id="6e573-156">Získejte vlastní název domény pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="6e573-156">Acquire a custom domain name for your cluster.</span></span> <span data-ttu-id="6e573-157">Při žádosti o certifikát od název subjektu certifikátu certifikační Autority hello musí odpovídat názvu vlastní domény hello používá pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="6e573-157">When you request a certificate from a CA hello certificate's subject name must match hello custom domain name used for your cluster.</span></span>

### <a name="client-authentication-certificates"></a><span data-ttu-id="6e573-158">Certifikáty pro ověřování klientů</span><span class="sxs-lookup"><span data-stu-id="6e573-158">Client authentication certificates</span></span>
<span data-ttu-id="6e573-159">Další klientské certifikáty ověřit správci pro úlohy správy clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e573-159">Additional client certificates authenticate administrators for cluster management tasks.</span></span> <span data-ttu-id="6e573-160">Service Fabric má dvě úrovně přístupu: **správce** a **jen pro čtení uživatele**.</span><span class="sxs-lookup"><span data-stu-id="6e573-160">Service Fabric has two access levels: **admin** and **read-only user**.</span></span> <span data-ttu-id="6e573-161">Minimálně je třeba použít jeden certifikát pro přístup pro správu.</span><span class="sxs-lookup"><span data-stu-id="6e573-161">At minimum, a single certificate for administrative access should be used.</span></span> <span data-ttu-id="6e573-162">Pro další přístupu na úrovni uživatele je třeba zadat samostatný certifikát.</span><span class="sxs-lookup"><span data-stu-id="6e573-162">For additional user-level access, a separate certificate must be provided.</span></span> <span data-ttu-id="6e573-163">Další informace o přístupu rolí najdete v tématu [řízení přístupu na základě rolí pro Service Fabric klienty][service-fabric-cluster-security-roles].</span><span class="sxs-lookup"><span data-stu-id="6e573-163">For more information on access roles, see [role-based access control for Service Fabric clients][service-fabric-cluster-security-roles].</span></span>

<span data-ttu-id="6e573-164">Není nutné tooupload klienta ověřování certifikátů tooKey trezoru toowork s Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6e573-164">You do not need tooupload Client authentication certificates tooKey Vault toowork with Service Fabric.</span></span> <span data-ttu-id="6e573-165">Tyto certifikáty stačí toobe poskytuje toousers, kteří mají oprávnění pro správu clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e573-165">These certificates only need toobe provided toousers who are authorized for cluster management.</span></span> 

> [!NOTE]
> <span data-ttu-id="6e573-166">Azure Active Directory je, že hello doporučená způsob tooauthenticate klientů pro cluster operace správy.</span><span class="sxs-lookup"><span data-stu-id="6e573-166">Azure Active Directory is hello recommended way tooauthenticate clients for cluster management operations.</span></span> <span data-ttu-id="6e573-167">toouse Azure Active Directory, musíte [vytvořit cluster pomocí Azure Resource Manager][create-cluster-arm].</span><span class="sxs-lookup"><span data-stu-id="6e573-167">toouse Azure Active Directory, you must [create a cluster using Azure Resource Manager][create-cluster-arm].</span></span>
> 
> 

### <a name="application-certificates-optional"></a><span data-ttu-id="6e573-168">Certifikáty aplikace (volitelné)</span><span class="sxs-lookup"><span data-stu-id="6e573-168">Application certificates (optional)</span></span>
<span data-ttu-id="6e573-169">Libovolný počet dalších certifikátů lze nainstalovat na clusteru pro aplikace z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="6e573-169">Any number of additional certificates can be installed on a cluster for application security purposes.</span></span> <span data-ttu-id="6e573-170">Před vytvořením clusteru, zvažte hello aplikace zabezpečení scénáře, které vyžadují certifikát toobe, nainstalovaná na uzlech hello, jako například:</span><span class="sxs-lookup"><span data-stu-id="6e573-170">Before creating your cluster, consider hello application security scenarios that require a certificate toobe installed on hello nodes, such as:</span></span>

* <span data-ttu-id="6e573-171">Šifrování a dešifrování hodnot konfigurace aplikace</span><span class="sxs-lookup"><span data-stu-id="6e573-171">Encryption and decryption of application configuration values</span></span>
* <span data-ttu-id="6e573-172">Šifrování dat mezi uzly během replikace</span><span class="sxs-lookup"><span data-stu-id="6e573-172">Encryption of data across nodes during replication</span></span> 

<span data-ttu-id="6e573-173">Při vytváření clusteru prostřednictvím hello portál Azure se nedá nakonfigurovat certifikáty k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6e573-173">Application certificates cannot be configured when creating a cluster through hello Azure portal.</span></span> <span data-ttu-id="6e573-174">certifikáty k aplikaci tooconfigure během instalace clusteru, musíte [vytvořit cluster pomocí Azure Resource Manager][create-cluster-arm].</span><span class="sxs-lookup"><span data-stu-id="6e573-174">tooconfigure application certificates at cluster setup time, you must [create a cluster using Azure Resource Manager][create-cluster-arm].</span></span> <span data-ttu-id="6e573-175">Můžete také přidat aplikace certifikáty toohello clusteru po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="6e573-175">You can also add application certificates toohello cluster after it has been created.</span></span>

### <a name="formatting-certificates-for-azure-resource-provider-use"></a><span data-ttu-id="6e573-176">Certifikáty formátování pro použití poskytovatele prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="6e573-176">Formatting certificates for Azure resource provider use</span></span>
<span data-ttu-id="6e573-177">Soubory privátních klíčů (.pfx) můžete přidat a používat přímo přes Key Vault.</span><span class="sxs-lookup"><span data-stu-id="6e573-177">Private key files (.pfx) can be added and used directly through Key Vault.</span></span> <span data-ttu-id="6e573-178">Poskytovatel prostředků Azure hello však vyžaduje toobe klíče uložené ve speciální formátu JSON, který zahrnuje hello .pfx jako kódování base-64 kódovaného řetězec a hello heslo soukromého klíče.</span><span class="sxs-lookup"><span data-stu-id="6e573-178">However, hello Azure resource provider requires keys toobe stored in a special JSON format that includes hello .pfx as a base-64 encoded string and hello private key password.</span></span> <span data-ttu-id="6e573-179">tooaccommodate tyto požadavky klíče musí být umístěn do řetězce formátu JSON a pak uloženy jako *tajné klíče* v Key Vault.</span><span class="sxs-lookup"><span data-stu-id="6e573-179">tooaccommodate these requirements, keys must be placed in a JSON string and then stored as *secrets* in Key Vault.</span></span>

<span data-ttu-id="6e573-180">modul prostředí PowerShell je toomake to zpracovat snadnější, [dostupná na Githubu][service-fabric-rp-helpers].</span><span class="sxs-lookup"><span data-stu-id="6e573-180">toomake this process easier, a PowerShell module is [available on GitHub][service-fabric-rp-helpers].</span></span> <span data-ttu-id="6e573-181">Postupujte podle těchto kroků toouse hello modul:</span><span class="sxs-lookup"><span data-stu-id="6e573-181">Follow these steps toouse hello module:</span></span>

1. <span data-ttu-id="6e573-182">Stáhněte si celý obsah hello hello úložiště do místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="6e573-182">Download hello entire contents of hello repo into a local directory.</span></span> 
2. <span data-ttu-id="6e573-183">Import modulu hello v okně prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6e573-183">Import hello module in your PowerShell window:</span></span>

```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
```

<span data-ttu-id="6e573-184">Hello `Invoke-AddCertToKeyVault` příkaz v tento modul prostředí PowerShell automaticky formáty privátní klíč certifikátu do řetězce formátu JSON a odešle ho tooKey trezoru.</span><span class="sxs-lookup"><span data-stu-id="6e573-184">hello `Invoke-AddCertToKeyVault` command in this PowerShell module automatically formats a certificate private key into a JSON string and uploads it tooKey Vault.</span></span> <span data-ttu-id="6e573-185">Použijte ji tooadd hello clusteru certifikát a všechny další aplikaci certifikáty tooKey trezoru.</span><span class="sxs-lookup"><span data-stu-id="6e573-185">Use it tooadd hello cluster certificate and any additional application certificates tooKey Vault.</span></span> <span data-ttu-id="6e573-186">Tento krok opakujte pro všechny další certifikáty, že chcete tooinstall v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e573-186">Repeat this step for any additional certificates you want tooinstall in your cluster.</span></span>

```powershell
PS C:\Users\vturecek> Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

    Switching context tooSubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: hello output object type of this cmdlet will be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret toomyvault in vault myvault


Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

<span data-ttu-id="6e573-187">Toto jsou všechny požadované součásti hello Key Vault pro konfiguraci šablony správce prostředků clusteru Service Fabric, která nainstaluje certifikáty pro ověřování uzlu, zabezpečení koncového bodu správy a ověření a zabezpečení žádné další aplikace Funkce, které používají certifikáty X.509.</span><span class="sxs-lookup"><span data-stu-id="6e573-187">These are all hello Key Vault prerequisites for configuring a Service Fabric cluster Resource Manager template that installs certificates for node authentication, management endpoint security and authentication, and any additional application security features that use X.509 certificates.</span></span> <span data-ttu-id="6e573-188">V tomto okamžiku teď byste měli mít hello po instalaci v Azure:</span><span class="sxs-lookup"><span data-stu-id="6e573-188">At this point, you should now have hello following setup in Azure:</span></span>

* <span data-ttu-id="6e573-189">Skupina prostředků Key Vault</span><span class="sxs-lookup"><span data-stu-id="6e573-189">Key Vault resource group</span></span>
  * <span data-ttu-id="6e573-190">Key Vault</span><span class="sxs-lookup"><span data-stu-id="6e573-190">Key Vault</span></span>
    * <span data-ttu-id="6e573-191">Ověřovací certifikát serverů clusteru</span><span class="sxs-lookup"><span data-stu-id="6e573-191">Cluster server authentication certificate</span></span>

<span data-ttu-id="6e573-192">< /a "Vytvoření cluster-portal" ></a></span><span class="sxs-lookup"><span data-stu-id="6e573-192"></a "create-cluster-portal" ></a></span></span>

## <a name="create-cluster-in-hello-azure-portal"></a><span data-ttu-id="6e573-193">Vytvoření clusteru v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6e573-193">Create cluster in hello Azure portal</span></span>
### <a name="search-for-hello-service-fabric-cluster-resource"></a><span data-ttu-id="6e573-194">Vyhledejte hello prostředku clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6e573-194">Search for hello Service Fabric cluster resource</span></span>
![Vyhledejte šablony clusteru Service Fabric na hello portálu Azure.][SearchforServiceFabricClusterTemplate]

1. <span data-ttu-id="6e573-196">Přihlaste se toohello [portál Azure][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="6e573-196">Sign in toohello [Azure portal][azure-portal].</span></span>
2. <span data-ttu-id="6e573-197">Klikněte na tlačítko **nový** tooadd novou šablonu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6e573-197">Click **New** tooadd a new resource template.</span></span> <span data-ttu-id="6e573-198">Vyhledejte šablony hello Service Fabric Cluster v hello **Marketplace** pod **všechno, co**.</span><span class="sxs-lookup"><span data-stu-id="6e573-198">Search for hello Service Fabric Cluster template in hello **Marketplace** under **Everything**.</span></span>
3. <span data-ttu-id="6e573-199">Vyberte **Service Fabric Cluster** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="6e573-199">Select **Service Fabric Cluster** from hello list.</span></span>
4. <span data-ttu-id="6e573-200">Přejděte toohello **Service Fabric Cluster** okně klikněte na tlačítko **vytvořit**,</span><span class="sxs-lookup"><span data-stu-id="6e573-200">Navigate toohello **Service Fabric Cluster** blade, click **Create**,</span></span>
5. <span data-ttu-id="6e573-201">Hello **cluster Service Fabric vytvořit** okno obsahuje hello následující čtyři kroky.</span><span class="sxs-lookup"><span data-stu-id="6e573-201">hello **Create Service Fabric cluster** blade has hello following four steps.</span></span>

#### <a name="1-basics"></a><span data-ttu-id="6e573-202">1. Základy</span><span class="sxs-lookup"><span data-stu-id="6e573-202">1. Basics</span></span>
![Snímek obrazovky vytváření nové skupiny prostředků.][CreateRG]

<span data-ttu-id="6e573-204">V okně základy hello musíte tooprovide hello základní informace pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="6e573-204">In hello Basics blade you need tooprovide hello basic details for your cluster.</span></span>

1. <span data-ttu-id="6e573-205">Zadejte název hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e573-205">Enter hello name of your cluster.</span></span>
2. <span data-ttu-id="6e573-206">Zadejte **uživatelské jméno** a **heslo** pro vzdálená plocha pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6e573-206">Enter a **user name** and **password** for Remote Desktop for hello VMs.</span></span>
3. <span data-ttu-id="6e573-207">Ujistěte se, zda text hello tooselect **předplatné** má váš clusteru toobe nasazen, zvláště pokud máte více předplatných.</span><span class="sxs-lookup"><span data-stu-id="6e573-207">Make sure tooselect hello **Subscription** that you want your cluster toobe deployed to, especially if you have multiple subscriptions.</span></span>
4. <span data-ttu-id="6e573-208">Vytvoření **novou skupinu prostředků**.</span><span class="sxs-lookup"><span data-stu-id="6e573-208">Create a **new resource group**.</span></span> <span data-ttu-id="6e573-209">Je nejlepší toogive hello stejný název jako hello clusteru, protože pomáhá při hledání je později, zejména v případě, že se pokoušíte toomake změny tooyour nasazení nebo odstranit cluster.</span><span class="sxs-lookup"><span data-stu-id="6e573-209">It is best toogive it hello same name as hello cluster, since it helps in finding them later, especially when you are trying toomake changes tooyour deployment or delete your cluster.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="6e573-210">I když toouse existující skupinu prostředků, můžete rozhodnout, je vhodné toocreate novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6e573-210">Although you can decide toouse an existing resource group, it is a good practice toocreate a new resource group.</span></span> <span data-ttu-id="6e573-211">Díky tomu snadno toodelete clustery, které nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="6e573-211">This makes it easy toodelete clusters that you do not need.</span></span>
   > 
   > 
5. <span data-ttu-id="6e573-212">Vyberte hello **oblast** ve kterém chcete toocreate hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e573-212">Select hello **region** in which you want toocreate hello cluster.</span></span> <span data-ttu-id="6e573-213">Je nutné použít hello se stejné oblasti, kterou si klíč trezoru.</span><span class="sxs-lookup"><span data-stu-id="6e573-213">You must use hello same region that your Key Vault is in.</span></span>

#### <a name="2-cluster-configuration"></a><span data-ttu-id="6e573-214">2. Konfigurace clusteru</span><span class="sxs-lookup"><span data-stu-id="6e573-214">2. Cluster configuration</span></span>
![Vytvoření typu uzlu][CreateNodeType]

<span data-ttu-id="6e573-216">Nakonfigurujte uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e573-216">Configure your cluster nodes.</span></span> <span data-ttu-id="6e573-217">Typy uzlů definovat velikosti virtuálních počítačů hello hello počet virtuálních počítačů a jejich vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6e573-217">Node types define hello VM sizes, hello number of VMs, and their properties.</span></span> <span data-ttu-id="6e573-218">Cluster může mít více než jeden typ uzlu, ale typ primárního uzlu hello (hello první, kterou definujete na portálu hello) musí mít aspoň pět virtuálních počítačů, protože tento typ uzlu hello umístění Service Fabric systémových služeb.</span><span class="sxs-lookup"><span data-stu-id="6e573-218">Your cluster can have more than one node type, but hello primary node type (hello first one that you define on hello portal) must have at least five VMs, as this is hello node type where Service Fabric system services are placed.</span></span> <span data-ttu-id="6e573-219">Nekonfigurujte **vlastnosti umístění** vzhledem k tomu je automaticky přidá výchozí umístění vlastnost "NodeTypeName".</span><span class="sxs-lookup"><span data-stu-id="6e573-219">Do not configure **Placement Properties** because a default placement property of "NodeTypeName" is added automatically.</span></span>

> [!NOTE]
> <span data-ttu-id="6e573-220">Běžný scénář pro více typů uzlu je aplikace, která obsahuje službu front-endu a back endové službě.</span><span class="sxs-lookup"><span data-stu-id="6e573-220">A common scenario for multiple node types is an application that contains a front-end service and a back-end service.</span></span> <span data-ttu-id="6e573-221">Chcete tooput hello front-endových služeb na menší virtuálních počítačů (velikosti virtuálních počítačů jako D2) s porty otevřené toohello Internet, ale bez internetového portů otevřené, aby tooput hello back-end službu na větší virtuální počítače (s velikostí virtuálního počítače jako D4, D6, D15 a tak dále).</span><span class="sxs-lookup"><span data-stu-id="6e573-221">You want tooput hello front-end service on smaller VMs (VM sizes like D2) with ports open toohello Internet, but you want tooput hello back-end service on larger VMs (with VM sizes like D4, D6, D15, and so on) with no Internet-facing ports open.</span></span>
> 
> 

1. <span data-ttu-id="6e573-222">Zvolte název pro váš typ uzlu (1 too12 znaků obsahující pouze písmena a číslice).</span><span class="sxs-lookup"><span data-stu-id="6e573-222">Choose a name for your node type (1 too12 characters containing only letters and numbers).</span></span>
2. <span data-ttu-id="6e573-223">Hello minimální **velikost** virtuálních počítačů pro primární uzel hello typ doprovází hello **odolnost** vrstvy, které jste vybrali pro hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e573-223">hello minimum **size** of VMs for hello primary node type is driven by hello **durability** tier you choose for hello cluster.</span></span> <span data-ttu-id="6e573-224">Výchozí hodnota Hello pro vrstvu odolnost hello je Bronzová.</span><span class="sxs-lookup"><span data-stu-id="6e573-224">hello default for hello durability tier is bronze.</span></span> <span data-ttu-id="6e573-225">Další informace o odolnost, najdete v části [jak toochoose hello Service Fabric clusteru spolehlivost a odolnost][service-fabric-cluster-capacity].</span><span class="sxs-lookup"><span data-stu-id="6e573-225">For more information on durability, see [how toochoose hello Service Fabric cluster reliability and durability][service-fabric-cluster-capacity].</span></span>
3. <span data-ttu-id="6e573-226">Vyberte velikost virtuálního počítače hello a cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="6e573-226">Select hello VM size and pricing tier.</span></span> <span data-ttu-id="6e573-227">Virtuální počítače D-series jednotek SSD a jsou doporučovány stavových aplikací.</span><span class="sxs-lookup"><span data-stu-id="6e573-227">D-series VMs have SSD drives and are highly recommended for stateful applications.</span></span> <span data-ttu-id="6e573-228">Použít všechny virtuálních počítačů SKU, který má částečné jader nebo mají méně než 7 GB kapacity disku.</span><span class="sxs-lookup"><span data-stu-id="6e573-228">Do not use any VM SKU that has partial cores or have less than 7 GB of available disk capacity.</span></span> 
4. <span data-ttu-id="6e573-229">Hello minimální **číslo** virtuálních počítačů pro primární uzel hello typ doprovází hello **spolehlivost** vrstvy, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="6e573-229">hello minimum **number** of VMs for hello primary node type is driven by hello **reliability** tier you choose.</span></span> <span data-ttu-id="6e573-230">Výchozí hodnota Hello pro úroveň spolehlivosti hello je Silver.</span><span class="sxs-lookup"><span data-stu-id="6e573-230">hello default for hello reliability tier is Silver.</span></span> <span data-ttu-id="6e573-231">Další informace o spolehlivosti, najdete v části [jak toochoose hello Service Fabric clusteru spolehlivost a odolnost][service-fabric-cluster-capacity].</span><span class="sxs-lookup"><span data-stu-id="6e573-231">For more information on reliability, see [how toochoose hello Service Fabric cluster reliability and durability][service-fabric-cluster-capacity].</span></span>
5. <span data-ttu-id="6e573-232">Zvolte hello počet virtuálních počítačů pro typ uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="6e573-232">Choose hello number of VMs for hello node type.</span></span> <span data-ttu-id="6e573-233">Je možné škálovat později na nahoru nebo dolů hello počet virtuálních počítačů v uzlu typu, ale na primárním uzlu, který typ hello, hello minimální doprovází hello úroveň spolehlivosti, která jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="6e573-233">You can scale up or down hello number of VMs in a node type later on, but on hello primary node type, hello minimum is driven by hello reliability level that you have chosen.</span></span> <span data-ttu-id="6e573-234">Ostatní typy uzlů může mít minimálně 1 virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6e573-234">Other node types can have a minimum of 1 VM.</span></span>
6. <span data-ttu-id="6e573-235">Nakonfigurujte vlastní koncové body.</span><span class="sxs-lookup"><span data-stu-id="6e573-235">Configure custom endpoints.</span></span> <span data-ttu-id="6e573-236">Toto pole umožňuje tooenter textový soubor s oddělovači seznam portů, které chcete tooexpose prostřednictvím hello nástroj pro vyrovnávání zatížení Azure toohello veřejného Internetu pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e573-236">This field allows you tooenter a comma-separated list of ports that you want tooexpose through hello Azure Load Balancer toohello public Internet for your applications.</span></span> <span data-ttu-id="6e573-237">Například pokud máte v plánu toodeploy clusteru tooyour webové aplikace, zadejte "80" zde tooallow přenosy na portu 80 do clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e573-237">For example, if you plan toodeploy a web application tooyour cluster, enter "80" here tooallow traffic on port 80 into your cluster.</span></span> <span data-ttu-id="6e573-238">Další informace o koncových bodů najdete v tématu [komunikaci s aplikacemi][service-fabric-connect-and-communicate-with-services]</span><span class="sxs-lookup"><span data-stu-id="6e573-238">For more information on endpoints, see [communicating with applications][service-fabric-connect-and-communicate-with-services]</span></span>
7. <span data-ttu-id="6e573-239">Konfigurace clusteru **diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="6e573-239">Configure cluster **diagnostics**.</span></span> <span data-ttu-id="6e573-240">Ve výchozím nastavení jsou diagnostiky povolené na váš clusteru tooassist s řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="6e573-240">By default, diagnostics are enabled on your cluster tooassist with troubleshooting issues.</span></span> <span data-ttu-id="6e573-241">Pokud chcete změnit toodisable diagnostiky hello **stav** přepnutí příliš**vypnout**.</span><span class="sxs-lookup"><span data-stu-id="6e573-241">If you want toodisable diagnostics change hello **Status** toggle too**Off**.</span></span> <span data-ttu-id="6e573-242">Vypnutí možnosti diagnostiky je **není** nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="6e573-242">Turning off diagnostics is **not** recommended.</span></span>
8. <span data-ttu-id="6e573-243">Vyberte hello Fabric upgradu, které chcete nastavit clusteru režimu.</span><span class="sxs-lookup"><span data-stu-id="6e573-243">Select hello Fabric upgrade mode you want set your cluster to.</span></span> <span data-ttu-id="6e573-244">Vyberte **automatické**, pokud chcete vybrat tooautomatically systému hello až hello nejnovější dostupnou verzi a zkuste tooupgrade tooit vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e573-244">Select **Automatic**, if you want hello system tooautomatically pick up hello latest available version and try tooupgrade your cluster tooit.</span></span> <span data-ttu-id="6e573-245">Nastavit režim hello příliš**ruční**, pokud chcete, aby toochoose na podporovanou verzi.</span><span class="sxs-lookup"><span data-stu-id="6e573-245">Set hello mode too**Manual**, if you want toochoose a supported version.</span></span>

> [!NOTE]
> <span data-ttu-id="6e573-246">Podporujeme pouze clustery, které běží podporovaná verze služby prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="6e573-246">We support only clusters that are running supported versions of service Fabric.</span></span> <span data-ttu-id="6e573-247">Výběrem hello **ruční** režimu přenášíte na hello odpovědnost tooupgrade tooa podporované verze vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e573-247">By selecting hello **Manual** mode, you are taking on hello responsibility tooupgrade your cluster tooa supported version.</span></span> <span data-ttu-id="6e573-248">Další podrobnosti o režimu upgradu hello prostředků infrastruktury, najdete v tématu hello [service fabric clusteru upgrade dokumentu.][service-fabric-cluster-upgrade]</span><span class="sxs-lookup"><span data-stu-id="6e573-248">For more details on hello Fabric upgrade mode see hello [service-fabric-cluster-upgrade document.][service-fabric-cluster-upgrade]</span></span>
> 
> 

#### <a name="3-security"></a><span data-ttu-id="6e573-249">3. Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="6e573-249">3. Security</span></span>
![Snímek obrazovky konfigurace zabezpečení na portálu Azure.][SecurityConfigs]

<span data-ttu-id="6e573-251">posledním krokem Hello je tooprovide certifikátu informace toosecure hello clusteru pomocí hello Key Vault a certifikát informace vytvořený dříve.</span><span class="sxs-lookup"><span data-stu-id="6e573-251">hello final step is tooprovide certificate information toosecure hello cluster using hello Key Vault and certificate information created earlier.</span></span>

* <span data-ttu-id="6e573-252">Naplnění polí hello primární certifikát získaný odesílání hello výstup hello **clusteru certifikát** pomocí trezoru tooKey hello `Invoke-AddCertToKeyVault` příkaz prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6e573-252">Populate hello primary certificate fields with hello output obtained from uploading hello **cluster certificate** tooKey Vault using hello `Invoke-AddCertToKeyVault` PowerShell command.</span></span>

```powershell
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47
```

* <span data-ttu-id="6e573-253">Zkontrolujte hello **konfigurovat upřesňující nastavení** pole tooenter klientské certifikáty pro **správce klienta** a **jen pro čtení klienta**.</span><span class="sxs-lookup"><span data-stu-id="6e573-253">Check hello **Configure advanced settings** box tooenter client certificates for **admin client** and **read-only client**.</span></span> <span data-ttu-id="6e573-254">V těchto polích zadejte hello kryptografický otisk certifikátu klienta správce a hello kryptografický otisk certifikátu klienta uživatele jen pro čtení, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="6e573-254">In these fields, enter hello thumbprint of your admin client certificate and hello thumbprint of your read-only user client certificate, if applicable.</span></span> <span data-ttu-id="6e573-255">Pokud se správce pokusí o tooconnect toohello clusteru, získají přístup pouze v případě, že mají certifikát s kryptografickým otiskem, který odpovídá hodnoty kryptografického otisku hello zadali tady.</span><span class="sxs-lookup"><span data-stu-id="6e573-255">When administrators attempt tooconnect toohello cluster, they are granted access only if they have a certificate with a thumbprint that matches hello thumbprint values entered here.</span></span>  

#### <a name="4-summary"></a><span data-ttu-id="6e573-256">4. Souhrn</span><span class="sxs-lookup"><span data-stu-id="6e573-256">4. Summary</span></span>
![<span data-ttu-id="6e573-257">Snímek obrazovky Tabule start hello zobrazení "Nasazení Cluster Service Fabric."</span><span class="sxs-lookup"><span data-stu-id="6e573-257">Screen shot of hello start board displaying "Deploying Service Fabric Cluster."</span></span> ][Notifications]

<span data-ttu-id="6e573-258">Vytvoření clusteru hello toocomplete, klikněte na tlačítko **Souhrn** toosee hello konfigurace, které jste zadali, nebo stáhnout hello šablony Azure Resource Manageru, který používá toodeploy vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e573-258">toocomplete hello cluster creation, click **Summary** toosee hello configurations that you have provided, or download hello Azure Resource Manager template that that used toodeploy your cluster.</span></span> <span data-ttu-id="6e573-259">Po zadané povinné nastavení hello hello **OK** bude zelené tlačítko a hello procesu vytváření clusteru můžete spustit kliknutím.</span><span class="sxs-lookup"><span data-stu-id="6e573-259">After you have provided hello mandatory settings, hello **OK** button becomes green and you can start hello cluster creation process by clicking it.</span></span>

<span data-ttu-id="6e573-260">Zobrazí se průběh vytváření hello v oznámeních hello.</span><span class="sxs-lookup"><span data-stu-id="6e573-260">You can see hello creation progress in hello notifications.</span></span> <span data-ttu-id="6e573-261">(Klikněte na ikonu zvonku"hello" v blízkosti hello stavového řádku v hello pravé horní části obrazovky.) Pokud jste klikli na **Pin tooStartboard** při vytváření clusteru hello, uvidíte **nasazení Cluster Service Fabric** připnutý toohello **spustit** panelu.</span><span class="sxs-lookup"><span data-stu-id="6e573-261">(Click hello "Bell" icon near hello status bar at hello upper right of your screen.) If you clicked **Pin tooStartboard** while creating hello cluster, you will see **Deploying Service Fabric Cluster** pinned toohello **Start** board.</span></span>

### <a name="view-your-cluster-status"></a><span data-ttu-id="6e573-262">Zobrazení stavu clusteru</span><span class="sxs-lookup"><span data-stu-id="6e573-262">View your cluster status</span></span>
![Snímek obrazovky podrobnosti o clusteru hello řídicím panelu.][ClusterDashboard]

<span data-ttu-id="6e573-264">Po vytvoření clusteru si můžete prohlédnout cluster hello portálu:</span><span class="sxs-lookup"><span data-stu-id="6e573-264">Once your cluster is created, you can inspect your cluster in hello portal:</span></span>

1. <span data-ttu-id="6e573-265">Přejděte příliš**Procházet** a klikněte na tlačítko **clustery infrastruktury služby**.</span><span class="sxs-lookup"><span data-stu-id="6e573-265">Go too**Browse** and click **Service Fabric Clusters**.</span></span>
2. <span data-ttu-id="6e573-266">Vyhledejte cluster a klikněte na něj.</span><span class="sxs-lookup"><span data-stu-id="6e573-266">Locate your cluster and click it.</span></span>
3. <span data-ttu-id="6e573-267">Zobrazí podrobnosti o hello clusteru na řídicím panelu hello, včetně veřejný koncový bod clusteru hello a odkaz tooService Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="6e573-267">You can now see hello details of your cluster in hello dashboard, including hello cluster's public endpoint and a link tooService Fabric Explorer.</span></span>

<span data-ttu-id="6e573-268">Hello **uzlu monitorování** část v okně řídicí panel clusteru hello označuje hello počet virtuálních počítačů, které jsou v pořádku a není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="6e573-268">hello **Node Monitor** section on hello cluster's dashboard blade indicates hello number of VMs that are healthy and not healthy.</span></span> <span data-ttu-id="6e573-269">Můžete najít další podrobnosti o stavu hello clusteru na [Service Fabric stavu modelu ÚVOD][service-fabric-health-introduction].</span><span class="sxs-lookup"><span data-stu-id="6e573-269">You can find more details about hello cluster's health at [Service Fabric health model introduction][service-fabric-health-introduction].</span></span>

> [!NOTE]
> <span data-ttu-id="6e573-270">Clusterů Service Fabric vyžadují určitý počet uzlů toobe dostupnost vždy toomaintain a zachovávají stav – odkazované tooas "Správa kvora".</span><span class="sxs-lookup"><span data-stu-id="6e573-270">Service Fabric clusters require a certain number of nodes toobe up always toomaintain availability and preserve state - referred tooas "maintaining quorum".</span></span> <span data-ttu-id="6e573-271">Proto, je obvykle není bezpečné tooshut dolů všechny počítače v clusteru hello Pokud jste provedli nejprve [úplného zálohování vaší stavu][service-fabric-reliable-services-backup-restore].</span><span class="sxs-lookup"><span data-stu-id="6e573-271">Therfore, it is typically not safe tooshut down all machines in hello cluster unless you have first performed a [full backup of your state][service-fabric-reliable-services-backup-restore].</span></span>
> 
> 

## <a name="remote-connect-tooa-virtual-machine-scale-set-instance-or-a-cluster-node"></a><span data-ttu-id="6e573-272">Vzdálené připojení instance tooa sadu škálování virtuálního počítače nebo uzlu clusteru</span><span class="sxs-lookup"><span data-stu-id="6e573-272">Remote connect tooa Virtual Machine Scale Set instance or a cluster node</span></span>
<span data-ttu-id="6e573-273">Každý z hello NodeTypes zadáte v clusteru výsledky do sady škálování virtuálního počítače získávání nastavení.</span><span class="sxs-lookup"><span data-stu-id="6e573-273">Each of hello NodeTypes you specify in your cluster results in a Virtual Machine Scale Set getting set-up.</span></span> <span data-ttu-id="6e573-274">V tématu [vzdáleného připojení instance sadu škálování virtuálního počítače tooa] [ remote-connect-to-a-vm-scale-set] podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="6e573-274">See [Remote connect tooa Virtual Machine Scale Set instance][remote-connect-to-a-vm-scale-set] for details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e573-275">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6e573-275">Next steps</span></span>
<span data-ttu-id="6e573-276">V tuto chvíli máte zabezpečené cluster pro správu ověřování pomocí certifikátů.</span><span class="sxs-lookup"><span data-stu-id="6e573-276">At this point, you have a secure cluster using certificates for management authentication.</span></span> <span data-ttu-id="6e573-277">Dále [připojení clusteru tooyour](service-fabric-connect-to-secure-cluster.md) a zjistěte, jak příliš[spravovat tajné klíče aplikace](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="6e573-277">Next, [connect tooyour cluster](service-fabric-connect-to-secure-cluster.md) and learn how too[manage application secrets](service-fabric-application-secret-management.md).</span></span>  <span data-ttu-id="6e573-278">Také další informace o [možnosti podpory Service Fabric](service-fabric-support.md).</span><span class="sxs-lookup"><span data-stu-id="6e573-278">Also, learn about [Service Fabric support options](service-fabric-support.md).</span></span>

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
