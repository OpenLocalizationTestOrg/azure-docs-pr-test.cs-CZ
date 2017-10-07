---
title: "aaaCreate Azure Service Fabric clusteru ze šablony | Microsoft Docs"
description: "Tento článek popisuje, jak clusteru tooset až zabezpečené Service Fabric v Azure pomocí Azure Resource Manager, Azure Key Vault a Azure Active Directory (Azure AD) pro ověřování klientů."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: chackdan
ms.openlocfilehash: a4563c58a68127720a8290c3be0df9d026833eb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-fabric-cluster-by-using-azure-resource-manager"></a><span data-ttu-id="d524c-103">Vytvořit cluster Service Fabric pomocí Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="d524c-103">Create a Service Fabric cluster by using Azure Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d524c-104">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="d524c-104">Azure Resource Manager</span></span>](service-fabric-cluster-creation-via-arm.md)
> * [<span data-ttu-id="d524c-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d524c-105">Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
>
>

<span data-ttu-id="d524c-106">Tento podrobný průvodce vás provede procesem nastavení zabezpečení clusteru Azure Service Fabric v Azure pomocí Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d524c-106">This step-by-step guide walks you through setting up a secure Azure Service Fabric cluster in Azure by using Azure Resource Manager.</span></span> <span data-ttu-id="d524c-107">Jsme na vědomí, že tento článek hello je dlouhá.</span><span class="sxs-lookup"><span data-stu-id="d524c-107">We acknowledge that hello article is long.</span></span> <span data-ttu-id="d524c-108">Nicméně pokud jste už znát hello obsah, být zda toofollow každý krok pečlivě.</span><span class="sxs-lookup"><span data-stu-id="d524c-108">Nevertheless, unless you are already thoroughly familiar with hello content, be sure toofollow each step carefully.</span></span>

<span data-ttu-id="d524c-109">Průvodce Hello popisuje hello následující postupy:</span><span class="sxs-lookup"><span data-stu-id="d524c-109">hello guide covers hello following procedures:</span></span>

* <span data-ttu-id="d524c-110">Nastavení služby Azure trezoru klíčů tooupload certifikátů pro zabezpečení clusteru a aplikace</span><span class="sxs-lookup"><span data-stu-id="d524c-110">Setting up an Azure key vault tooupload certificates for cluster and application security</span></span>
* <span data-ttu-id="d524c-111">Vytvoření clusteru s podporou zabezpečené v Azure pomocí Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="d524c-111">Creating a secured cluster in Azure by using Azure Resource Manager</span></span>
* <span data-ttu-id="d524c-112">Ověřování uživatelů pomocí služby Azure Active Directory (Azure AD) pro správu clusteru</span><span class="sxs-lookup"><span data-stu-id="d524c-112">Authenticating users by using Azure Active Directory (Azure AD) for cluster management</span></span>

<span data-ttu-id="d524c-113">Cluster s podporou zabezpečení je cluster, který brání neoprávněným přístupem toomanagement operace.</span><span class="sxs-lookup"><span data-stu-id="d524c-113">A secure cluster is a cluster that prevents unauthorized access toomanagement operations.</span></span> <span data-ttu-id="d524c-114">To zahrnuje nasazení, upgrade a odstraňování aplikace, služby a hello data, která obsahují.</span><span class="sxs-lookup"><span data-stu-id="d524c-114">This includes deploying, upgrading, and deleting applications, services, and hello data they contain.</span></span> <span data-ttu-id="d524c-115">Nezabezpečená clusteru je cluster, že každý, kdo připojit tooat kdykoli a provádět operace správy.</span><span class="sxs-lookup"><span data-stu-id="d524c-115">An unsecure cluster is a cluster that anyone can connect tooat any time and perform management operations.</span></span> <span data-ttu-id="d524c-116">Přestože je možné toocreate nezabezpečená clusteru, důrazně doporučujeme vytvořit cluster s podporou zabezpečení z hello outset.</span><span class="sxs-lookup"><span data-stu-id="d524c-116">Although it is possible toocreate an unsecure cluster, we highly recommend that you create a secure cluster from hello outset.</span></span> <span data-ttu-id="d524c-117">Protože nezabezpečená clusteru nelze zabezpečit později, je nutné vytvořit nový cluster.</span><span class="sxs-lookup"><span data-stu-id="d524c-117">Because an unsecure cluster cannot be secured later, a new cluster must be created.</span></span>

<span data-ttu-id="d524c-118">Hello konceptu vytváření zabezpečené clustery je hello stejné, jestli jsou v systému Linux nebo Windows clusterů.</span><span class="sxs-lookup"><span data-stu-id="d524c-118">hello concept of creating secure clusters is hello same, whether they are Linux or Windows clusters.</span></span> <span data-ttu-id="d524c-119">Další informace a pomocné rutiny skripty pro vytvoření zabezpečeného clusterů systému Linux, najdete v části [vytváření zabezpečené clusterů v systému Linux](#secure-linux-clusters).</span><span class="sxs-lookup"><span data-stu-id="d524c-119">For more information and helper scripts for creating secure Linux clusters, see [Creating secure clusters on Linux](#secure-linux-clusters).</span></span>

## <a name="sign-in-tooyour-azure-account"></a><span data-ttu-id="d524c-120">Přihlaste se tooyour účet Azure</span><span class="sxs-lookup"><span data-stu-id="d524c-120">Sign in tooyour Azure account</span></span>
<span data-ttu-id="d524c-121">Tato příručka používá [prostředí Azure PowerShell][azure-powershell].</span><span class="sxs-lookup"><span data-stu-id="d524c-121">This guide uses [Azure PowerShell][azure-powershell].</span></span> <span data-ttu-id="d524c-122">Po spuštění novou relaci prostředí PowerShell se přihlaste tooyour účet Azure a vybrat své předplatné před spuštěním příkazů Azure.</span><span class="sxs-lookup"><span data-stu-id="d524c-122">When you start a new PowerShell session, sign in tooyour Azure account and select your subscription before you execute Azure commands.</span></span>

<span data-ttu-id="d524c-123">Přihlaste se tooyour účet Azure:</span><span class="sxs-lookup"><span data-stu-id="d524c-123">Sign in tooyour Azure account:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="d524c-124">Vyberte předplatné:</span><span class="sxs-lookup"><span data-stu-id="d524c-124">Select your subscription:</span></span>

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-a-key-vault"></a><span data-ttu-id="d524c-125">Nastavení trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="d524c-125">Set up a key vault</span></span>
<span data-ttu-id="d524c-126">Tato část obsahuje informace o vytvoření trezoru klíčů pro cluster Service Fabric v Azure a aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d524c-126">This section discusses creating a key vault for a Service Fabric cluster in Azure and for Service Fabric applications.</span></span> <span data-ttu-id="d524c-127">Dokončení Průvodce tooAzure Key Vault, najdete v části toohello [Key Vault Příručka Začínáme][key-vault-get-started].</span><span class="sxs-lookup"><span data-stu-id="d524c-127">For a complete guide tooAzure Key Vault, refer toohello [Key Vault getting started guide][key-vault-get-started].</span></span>

<span data-ttu-id="d524c-128">Service Fabric používá toosecure certifikáty X.509 cluster a poskytují funkce zabezpečení aplikací.</span><span class="sxs-lookup"><span data-stu-id="d524c-128">Service Fabric uses X.509 certificates toosecure a cluster and provide application security features.</span></span> <span data-ttu-id="d524c-129">Používáte certifikáty toomanage Key Vault pro clusterů Service Fabric v Azure.</span><span class="sxs-lookup"><span data-stu-id="d524c-129">You use Key Vault toomanage certificates for Service Fabric clusters in Azure.</span></span> <span data-ttu-id="d524c-130">Pokud cluster je nasazené v Azure, vyžaduje certifikáty od Key Vault hello poskytovatel prostředků Azure, která je odpovědná za vytváření clusterů Service Fabric a nainstaluje je hello clusteru virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d524c-130">When a cluster is deployed in Azure, hello Azure resource provider that's responsible for creating Service Fabric clusters pulls certificates from Key Vault and installs them on hello cluster VMs.</span></span>

<span data-ttu-id="d524c-131">Hello následující diagram znázorňuje hello vztah mezi Azure Key Vault, cluster Service Fabric a hello poskytovatel prostředků Azure, která používá certifikáty uložené v trezoru klíčů při vytváření clusteru:</span><span class="sxs-lookup"><span data-stu-id="d524c-131">hello following diagram illustrates hello relationship between Azure Key Vault, a Service Fabric cluster, and hello Azure resource provider that uses certificates stored in a key vault when it creates a cluster:</span></span>

![Diagram instalace certifikátu][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a><span data-ttu-id="d524c-133">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="d524c-133">Create a resource group</span></span>
<span data-ttu-id="d524c-134">prvním krokem Hello je toocreate skupinu prostředků speciálně pro váš trezor klíčů.</span><span class="sxs-lookup"><span data-stu-id="d524c-134">hello first step is toocreate a resource group specifically for your key vault.</span></span> <span data-ttu-id="d524c-135">Doporučujeme převést trezoru klíčů hello do vlastní skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="d524c-135">We recommend that you put hello key vault into its own resource group.</span></span> <span data-ttu-id="d524c-136">Tato akce umožňuje odstranit hello výpočetního prostředí a úložiště skupiny prostředků, včetně hello skupinu prostředků, která obsahuje daný cluster Service Fabric bez ztráty klíče a tajné klíče.</span><span class="sxs-lookup"><span data-stu-id="d524c-136">This action lets you remove hello compute and storage resource groups, including hello resource group that contains your Service Fabric cluster, without losing your keys and secrets.</span></span> <span data-ttu-id="d524c-137">Hello skupinu prostředků, která obsahuje váš trezor klíčů _musí být v hello stejné oblasti_ jako hello cluster, který se používá.</span><span class="sxs-lookup"><span data-stu-id="d524c-137">hello resource group that contains your key vault _must be in hello same region_ as hello cluster that is using it.</span></span>

<span data-ttu-id="d524c-138">Pokud máte v plánu toodeploy clustery v několika oblastech, doporučujeme pojmenovat hello skupinu prostředků a trezoru klíčů hello způsobem, který určuje, které oblasti patří do.</span><span class="sxs-lookup"><span data-stu-id="d524c-138">If you plan toodeploy clusters in multiple regions, we suggest that you name hello resource group and hello key vault in a way that indicates which region it belongs to.</span></span>  

```powershell

    New-AzureRmResourceGroup -Name westus-mykeyvault -Location 'West US'
```
<span data-ttu-id="d524c-139">výstup Hello by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="d524c-139">hello output should look like this:</span></span>

```powershell

    WARNING: hello output object type of this cmdlet is going toobe modified in a future release.

    ResourceGroupName : westus-mykeyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/westus-mykeyvault

```
<a id="new-key-vault"></a>

### <a name="create-a-key-vault-in-hello-new-resource-group"></a><span data-ttu-id="d524c-140">Vytvoření trezoru klíčů v hello novou skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="d524c-140">Create a key vault in hello new resource group</span></span>
<span data-ttu-id="d524c-141">Trezor klíčů Hello _musí být povolen pro nasazení_ tooallow hello výpočetních prostředků zprostředkovatele tooget certifikáty z něj a nainstalujte ji na instancí virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="d524c-141">hello key vault _must be enabled for deployment_ tooallow hello compute resource provider tooget certificates from it and install it on virtual machine instances:</span></span>

```powershell

    New-AzureRmKeyVault -VaultName 'mywestusvault' -ResourceGroupName 'westus-mykeyvault' -Location 'West US' -EnabledForDeployment

```

<span data-ttu-id="d524c-142">výstup Hello by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="d524c-142">hello output should look like this:</span></span>

```powershell

    Vault Name                       : mywestusvault
    Resource Group Name              : westus-mykeyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault
    Vault URI                        : https://mywestusvault.vault.azure.net
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
<a id="existing-key-vault"></a>

## <a name="use-an-existing-key-vault"></a><span data-ttu-id="d524c-143">Použít existující trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="d524c-143">Use an existing key vault</span></span>

<span data-ttu-id="d524c-144">toouse existující trezor klíčů, můžete _ji musíte povolit pro nasazení_ tooallow hello výpočetních prostředků zprostředkovatele tooget certifikáty z něj a nainstalujte ji na uzlech clusteru:</span><span class="sxs-lookup"><span data-stu-id="d524c-144">toouse an existing key vault, you _must enable it for deployment_ tooallow hello compute resource provider tooget certificates from it and install it on cluster nodes:</span></span>

```powershell

Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

```

<a id="add-certificate-to-key-vault"></a>

## <a name="add-certificates-tooyour-key-vault"></a><span data-ttu-id="d524c-145">Přidejte certifikáty tooyour klíče trezoru</span><span class="sxs-lookup"><span data-stu-id="d524c-145">Add certificates tooyour key vault</span></span>

<span data-ttu-id="d524c-146">Certifikáty se používají v Service Fabric tooprovide ověřování a šifrování toosecure různé aspekty cluster a jeho aplikace.</span><span class="sxs-lookup"><span data-stu-id="d524c-146">Certificates are used in Service Fabric tooprovide authentication and encryption toosecure various aspects of a cluster and its applications.</span></span> <span data-ttu-id="d524c-147">Další informace o tom, jak certifikáty se používají v Service Fabric najdete v tématu [scénáře zabezpečení clusteru Service Fabric][service-fabric-cluster-security].</span><span class="sxs-lookup"><span data-stu-id="d524c-147">For more information on how certificates are used in Service Fabric, see [Service Fabric cluster security scenarios][service-fabric-cluster-security].</span></span>

### <a name="cluster-and-server-certificate-required"></a><span data-ttu-id="d524c-148">Cluster a server certifikátu (povinné)</span><span class="sxs-lookup"><span data-stu-id="d524c-148">Cluster and server certificate (required)</span></span>
<span data-ttu-id="d524c-149">Tento certifikát je požadovaná toosecure cluster a zabránit tooit neoprávněného přístupu.</span><span class="sxs-lookup"><span data-stu-id="d524c-149">This certificate is required toosecure a cluster and prevent unauthorized access tooit.</span></span> <span data-ttu-id="d524c-150">Poskytuje zabezpečení clusteru dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="d524c-150">It provides cluster security in two ways:</span></span>

* <span data-ttu-id="d524c-151">Ověřování clusteru: ověřuje komunikaci mezi uzly pro cluster federaci.</span><span class="sxs-lookup"><span data-stu-id="d524c-151">Cluster authentication: Authenticates node-to-node communication for cluster federation.</span></span> <span data-ttu-id="d524c-152">Pouze uzly, které můžete prokázání své identity s tímto certifikátem můžete připojit hello cluster.</span><span class="sxs-lookup"><span data-stu-id="d524c-152">Only nodes that can prove their identity with this certificate can join hello cluster.</span></span>
* <span data-ttu-id="d524c-153">Ověření serveru: ověřuje hello clusteru správy koncové body tooa správy klienta, tak, aby hello klienta pro správu zná ho je rozhovoru toohello skutečné clusteru.</span><span class="sxs-lookup"><span data-stu-id="d524c-153">Server authentication: Authenticates hello cluster management endpoints tooa management client, so that hello management client knows it is talking toohello real cluster.</span></span> <span data-ttu-id="d524c-154">Tento certifikát také poskytuje protokolem SSL pro hello rozhraní API pro správu protokolu HTTPS a pro Service Fabric Explorer přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d524c-154">This certificate also provides an SSL for hello HTTPS management API and for Service Fabric Explorer over HTTPS.</span></span>

<span data-ttu-id="d524c-155">tooserve tyto účely, hello certifikátu musí splňovat následující požadavky hello:</span><span class="sxs-lookup"><span data-stu-id="d524c-155">tooserve these purposes, hello certificate must meet hello following requirements:</span></span>

* <span data-ttu-id="d524c-156">Hello certifikát musí obsahovat privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="d524c-156">hello certificate must contain a private key.</span></span>
* <span data-ttu-id="d524c-157">Hello certifikátu musí být vytvořeny pro výměnu klíčů, což je exportovatelný tooa soubor Personal Information Exchange (.pfx).</span><span class="sxs-lookup"><span data-stu-id="d524c-157">hello certificate must be created for key exchange, which is exportable tooa Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="d524c-158">Název subjektu certifikátu Hello musí odpovídat hello domény, že používáte cluster Service Fabric tooaccess hello.</span><span class="sxs-lookup"><span data-stu-id="d524c-158">hello certificate's subject name must match hello domain that you use tooaccess hello Service Fabric cluster.</span></span> <span data-ttu-id="d524c-159">Toto porovnání se vyžaduje tooprovide protokolem SSL pro koncové body správy protokolu HTTPS hello clusteru a Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="d524c-159">This matching is required tooprovide an SSL for hello cluster's HTTPS management endpoints and Service Fabric Explorer.</span></span> <span data-ttu-id="d524c-160">Nelze získat certifikát SSL od certifikační autority (CA) pro hello. cloudapp.azure.com domény.</span><span class="sxs-lookup"><span data-stu-id="d524c-160">You cannot obtain an SSL certificate from a certificate authority (CA) for hello .cloudapp.azure.com domain.</span></span> <span data-ttu-id="d524c-161">Je nutné získat vlastní název domény pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="d524c-161">You must obtain a custom domain name for your cluster.</span></span> <span data-ttu-id="d524c-162">Pokud budete požadovat certifikát od certifikační Autority, hello názvu subjektu certifikátu musí odpovídat hello název vlastní domény, který používáte pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="d524c-162">When you request a certificate from a CA, hello certificate's subject name must match hello custom domain name that you use for your cluster.</span></span>

### <a name="application-certificates-optional"></a><span data-ttu-id="d524c-163">Certifikáty aplikace (volitelné)</span><span class="sxs-lookup"><span data-stu-id="d524c-163">Application certificates (optional)</span></span>
<span data-ttu-id="d524c-164">Libovolný počet dalších certifikátů lze nainstalovat na clusteru pro aplikace z bezpečnostních důvodů.</span><span class="sxs-lookup"><span data-stu-id="d524c-164">Any number of additional certificates can be installed on a cluster for application security purposes.</span></span> <span data-ttu-id="d524c-165">Před vytvořením clusteru, zvažte hello aplikace zabezpečení scénáře, které vyžadují certifikát toobe, nainstalovaná na uzlech hello, jako například:</span><span class="sxs-lookup"><span data-stu-id="d524c-165">Before creating your cluster, consider hello application security scenarios that require a certificate toobe installed on hello nodes, such as:</span></span>

* <span data-ttu-id="d524c-166">Šifrování a dešifrování hodnot konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="d524c-166">Encryption and decryption of application configuration values.</span></span>
* <span data-ttu-id="d524c-167">Šifrování dat mezi uzly během replikace.</span><span class="sxs-lookup"><span data-stu-id="d524c-167">Encryption of data across nodes during replication.</span></span>

### <a name="formatting-certificates-for-azure-resource-provider-use"></a><span data-ttu-id="d524c-168">Certifikáty formátování pro použití poskytovatele prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="d524c-168">Formatting certificates for Azure resource provider use</span></span>
<span data-ttu-id="d524c-169">Můžete přidat a používat soubory privátních klíčů (.pfx) přímo prostřednictvím trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="d524c-169">You can add and use private key files (.pfx) directly through your key vault.</span></span> <span data-ttu-id="d524c-170">Poskytovatel prostředků Azure výpočetní hello však vyžaduje toobe klíče uloženy v speciální formátu JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="d524c-170">However, hello Azure compute resource provider requires keys toobe stored in a special JavaScript Object Notation (JSON) format.</span></span> <span data-ttu-id="d524c-171">Formát Hello obsahuje soubor .pfx hello jako základní řetězec s kódováním base64 a hello heslo soukromého klíče.</span><span class="sxs-lookup"><span data-stu-id="d524c-171">hello format includes hello .pfx file as a base 64-encoded string and hello private key password.</span></span> <span data-ttu-id="d524c-172">tooaccommodate tyto požadavky hello klíče musí být umístěn do řetězce formátu JSON a pak uloženy jako "tajemství" v klíči hello trezoru.</span><span class="sxs-lookup"><span data-stu-id="d524c-172">tooaccommodate these requirements, hello keys must be placed in a JSON string and then stored as "secrets" in hello key vault.</span></span>

<span data-ttu-id="d524c-173">toomake to zpracovat snadnější, [modul prostředí PowerShell je dostupná na Githubu][service-fabric-rp-helpers].</span><span class="sxs-lookup"><span data-stu-id="d524c-173">toomake this process easier, a [PowerShell module is available on GitHub][service-fabric-rp-helpers].</span></span> <span data-ttu-id="d524c-174">modul hello toouse, hello následující:</span><span class="sxs-lookup"><span data-stu-id="d524c-174">toouse hello module, do hello following:</span></span>

1. <span data-ttu-id="d524c-175">Stáhněte si celý obsah hello hello úložiště do místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="d524c-175">Download hello entire contents of hello repo into a local directory.</span></span>
2. <span data-ttu-id="d524c-176">Přejděte toohello místní adresář.</span><span class="sxs-lookup"><span data-stu-id="d524c-176">Go toohello local directory.</span></span>
2. <span data-ttu-id="d524c-177">Importujte modulu ServiceFabricRPHelpers hello v okně prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="d524c-177">Import hello ServiceFabricRPHelpers module in your PowerShell window:</span></span>

```powershell

 Import-Module "C:\..\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

```

<span data-ttu-id="d524c-178">Hello `Invoke-AddCertToKeyVault` příkaz v tento modul prostředí PowerShell automaticky formáty privátní klíč certifikátu do řetězce formátu JSON a odešle ho toohello trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="d524c-178">hello `Invoke-AddCertToKeyVault` command in this PowerShell module automatically formats a certificate private key into a JSON string and uploads it toohello key vault.</span></span> <span data-ttu-id="d524c-179">Použít hello příkaz tooadd hello clusteru certifikát a všechny další aplikaci certifikáty toohello trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="d524c-179">Use hello command tooadd hello cluster certificate and any additional application certificates toohello key vault.</span></span> <span data-ttu-id="d524c-180">Tento krok opakujte pro všechny další certifikáty, že chcete tooinstall v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d524c-180">Repeat this step for any additional certificates you want tooinstall in your cluster.</span></span>

#### <a name="uploading-an-existing-certificate"></a><span data-ttu-id="d524c-181">Odesílání existující certifikát</span><span class="sxs-lookup"><span data-stu-id="d524c-181">Uploading an existing certificate</span></span>

```powershell

 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName westus-mykeyvault -Location "West US" -VaultName mywestusvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

```

<span data-ttu-id="d524c-182">Pokud dojde k chybě, jako je například hello té, kterou tady vidíte obvykle to znamená, že máte konflikt adresy URL prostředku.</span><span class="sxs-lookup"><span data-stu-id="d524c-182">If you get an error, such as hello one shown here, it usually means that you have a resource URL conflict.</span></span> <span data-ttu-id="d524c-183">tooresolve hello konflikt, změnit název trezoru klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="d524c-183">tooresolve hello conflict, change hello key vault name.</span></span>

```
Set-AzureKeyVaultSecret : hello remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

<span data-ttu-id="d524c-184">Po vyřešení konfliktu hello hello výstup by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="d524c-184">After hello conflict is resolved, hello output should look like this:</span></span>

```

    Switching context tooSubscriptionId <guid>
    Ensuring ResourceGroup westus-mykeyvault in West US
    WARNING: hello output object type of this cmdlet is going toobe modified in a future release.
    Using existing value mywestusvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret toomywestusvault in vault mywestusvault


Name  : CertificateThumbprint
Value : E21DBC64B183B5BF355C34C46E03409FEEAEF58D

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault

Name  : CertificateURL
Value : https://mywestusvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

>[!NOTE]
><span data-ttu-id="d524c-185">Je nutné hello tři předchozí řetězce, CertificateThumbprint SourceVault a CertificateURL, tooset zabezpečení clusteru Service Fabric a tooobtain všechny certifikáty aplikace, které používáte pro zabezpečení aplikací.</span><span class="sxs-lookup"><span data-stu-id="d524c-185">You need hello three preceding strings, CertificateThumbprint, SourceVault, and CertificateURL, tooset up a secure Service Fabric cluster and tooobtain any application certificates that you might be using for application security.</span></span> <span data-ttu-id="d524c-186">Pokud uložíte není hello řetězce, může být obtížné tooretrieve je dotazováním hello klíč trezoru později.</span><span class="sxs-lookup"><span data-stu-id="d524c-186">If you do not save hello strings, it can be difficult tooretrieve them by querying hello key vault later.</span></span>

<a id="add-self-signed-certificate-to-key-vault"></a>

#### <a name="creating-a-self-signed-certificate-and-uploading-it-toohello-key-vault"></a><span data-ttu-id="d524c-187">Vytvořit certifikát podepsaný svým držitelem a pak ho nahrát toohello trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="d524c-187">Creating a self-signed certificate and uploading it toohello key vault</span></span>

<span data-ttu-id="d524c-188">Pokud jste již odeslali trezoru klíčů toohello certifikáty, tento krok přeskočte.</span><span class="sxs-lookup"><span data-stu-id="d524c-188">If you have already uploaded your certificates toohello key vault, skip this step.</span></span> <span data-ttu-id="d524c-189">Tento krok je pro generování nový certifikát podepsaný svým držitelem a pak ho nahrát tooyour trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="d524c-189">This step is for generating a new self-signed certificate and uploading it tooyour key vault.</span></span> <span data-ttu-id="d524c-190">Jakmile změňte parametry hello v hello následující skript a potom ho spusťte, vám měla zobrazit výzva k zadání hesla certifikátu.</span><span class="sxs-lookup"><span data-stu-id="d524c-190">After you change hello parameters in hello following script and then run it, you should be prompted for a certificate password.</span></span>  

```powershell

$ResourceGroup = "chackowestuskv"
$VName = "chackokv2"
$SubID = "6c653126-e4ba-42cd-a1dd-f7bf96ae7a47"
$locationRegion = "westus"
$newCertName = "chackotestcertificate1"
$dnsName = "www.mycluster.westus.mydomain.com" #hello certificate's subject name must match hello domain used tooaccess hello Service Fabric cluster.
$localCertPath = "C:\MyCertificates" # location where you want hello .PFX toobe stored

 Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResourceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath

```

<span data-ttu-id="d524c-191">Pokud dojde k chybě, jako je například hello té, kterou tady vidíte obvykle to znamená, že máte konflikt adresy URL prostředku.</span><span class="sxs-lookup"><span data-stu-id="d524c-191">If you get an error, such as hello one shown here, it usually means that you have a resource URL conflict.</span></span> <span data-ttu-id="d524c-192">konflikt hello tooresolve, název trezoru klíčů hello změnu, název RG a tak dále.</span><span class="sxs-lookup"><span data-stu-id="d524c-192">tooresolve hello conflict, change hello key vault name, RG name, and so forth.</span></span>

```
Set-AzureKeyVaultSecret : hello remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

<span data-ttu-id="d524c-193">Po vyřešení konfliktu hello hello výstup by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="d524c-193">After hello conflict is resolved, hello output should look like this:</span></span>

```
PS C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers> Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResouceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -Password $certPassword -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath
Switching context tooSubscriptionId 6c343126-e4ba-52cd-a1dd-f8bf96ae7a47
Ensuring ResourceGroup chackowestuskv in westus
WARNING: hello output object type of this cmdlet will be modified in a future release.
Creating new vault westuskv1 in westus
Creating new self signed certificate at C:\MyCertificates\chackonewcertificate1.pfx
Reading pfx file from C:\MyCertificates\chackonewcertificate1.pfx
Writing secret toochackonewcertificate1 in vault westuskv1


Name  : CertificateThumbprint
Value : 96BB3CC234F9D43C25D4B547sd8DE7B569F413EE

Name  : SourceVault
Value : /subscriptions/6c653126-e4ba-52cd-a1dd-f8bf96ae7a47/resourceGroups/chackowestuskv/providers/Microsoft.KeyVault/vaults/westuskv1

Name  : CertificateURL
Value : https://westuskv1.vault.azure.net:443/secrets/chackonewcertificate1/ee247291e45d405b8c8bbf81782d12bd

```

>[!NOTE]
><span data-ttu-id="d524c-194">Je nutné hello tři předchozí řetězce, CertificateThumbprint SourceVault a CertificateURL, tooset zabezpečení clusteru Service Fabric a tooobtain všechny certifikáty aplikace, které používáte pro zabezpečení aplikací.</span><span class="sxs-lookup"><span data-stu-id="d524c-194">You need hello three preceding strings, CertificateThumbprint, SourceVault, and CertificateURL, tooset up a secure Service Fabric cluster and tooobtain any application certificates that you might be using for application security.</span></span> <span data-ttu-id="d524c-195">Pokud uložíte není hello řetězce, může být obtížné tooretrieve je dotazováním hello klíč trezoru později.</span><span class="sxs-lookup"><span data-stu-id="d524c-195">If you do not save hello strings, it can be difficult tooretrieve them by querying hello key vault later.</span></span>

 <span data-ttu-id="d524c-196">V tomto okamžiku byste měli mít následující prvky zavedené hello:</span><span class="sxs-lookup"><span data-stu-id="d524c-196">At this point, you should have hello following elements in place:</span></span>

* <span data-ttu-id="d524c-197">Skupina prostředků Hello trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="d524c-197">hello key vault resource group.</span></span>
* <span data-ttu-id="d524c-198">Hello trezoru klíčů a jeho adresa URL (nazývané SourceVault v hello předcházející výstup prostředí PowerShell).</span><span class="sxs-lookup"><span data-stu-id="d524c-198">hello key vault and its URL (called SourceVault in hello preceding PowerShell output).</span></span>
* <span data-ttu-id="d524c-199">ověřovací certifikát serverů clusteru Hello a jeho adresu URL v trezoru klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="d524c-199">hello cluster server authentication certificate and its URL in hello key vault.</span></span>
* <span data-ttu-id="d524c-200">Hello aplikace certifikáty a jejich adresy URL v trezoru klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="d524c-200">hello application certificates and their URLs in hello key vault.</span></span>


<a id="add-AAD-for-client"></a>

## <a name="set-up-azure-active-directory-for-client-authentication"></a><span data-ttu-id="d524c-201">Nastavení Azure Active Directory pro ověřování klientů</span><span class="sxs-lookup"><span data-stu-id="d524c-201">Set up Azure Active Directory for client authentication</span></span>

<span data-ttu-id="d524c-202">Azure AD umožňuje tooapplications přístup uživatele toomanage organizace (známé jako klienty).</span><span class="sxs-lookup"><span data-stu-id="d524c-202">Azure AD enables organizations (known as tenants) toomanage user access tooapplications.</span></span> <span data-ttu-id="d524c-203">Aplikace se dál dělí na ty s webové přihlášení uživatelského rozhraní a ty prostředí nativního klienta.</span><span class="sxs-lookup"><span data-stu-id="d524c-203">Applications are divided into those with a web-based sign-in UI and those with a native client experience.</span></span> <span data-ttu-id="d524c-204">V tomto článku předpokládáme, že jste již vytvořili klienta.</span><span class="sxs-lookup"><span data-stu-id="d524c-204">In this article, we assume that you have already created a tenant.</span></span> <span data-ttu-id="d524c-205">Pokud máte není, spusťte načtením [jak tooget Azure Active Directory klienta][active-directory-howto-tenant].</span><span class="sxs-lookup"><span data-stu-id="d524c-205">If you have not, start by reading [How tooget an Azure Active Directory tenant][active-directory-howto-tenant].</span></span>

<span data-ttu-id="d524c-206">Cluster Service Fabric nabízí několik vstupní body tooits funkce správy, včetně hello webové [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] a [Visual Studio] [service-fabric-manage-application-in-visual-studio].</span><span class="sxs-lookup"><span data-stu-id="d524c-206">A Service Fabric cluster offers several entry points tooits management functionality, including hello web-based [Service Fabric Explorer][service-fabric-visualizing-your-cluster] and [Visual Studio][service-fabric-manage-application-in-visual-studio].</span></span> <span data-ttu-id="d524c-207">V důsledku toho vytvoříte dva Azure AD aplikace toocontrol přístup toohello cluster, jednu webovou aplikaci a jeden nativní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d524c-207">As a result, you create two Azure AD applications toocontrol access toohello cluster, one web application and one native application.</span></span>

<span data-ttu-id="d524c-208">toosimplify některé z kroků hello součástí konfigurace služby Azure AD se cluster Service Fabric, jsme vytvořili sadu skriptů prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d524c-208">toosimplify some of hello steps involved in configuring Azure AD with a Service Fabric cluster, we have created a set of Windows PowerShell scripts.</span></span>

> [!NOTE]
> <span data-ttu-id="d524c-209">Je třeba provést následující kroky před vytvořením clusteru hello hello.</span><span class="sxs-lookup"><span data-stu-id="d524c-209">You must complete hello following steps before you create hello cluster.</span></span> <span data-ttu-id="d524c-210">Skripty hello očekává koncové body a názvy clusterů, hello hodnoty by měla být plánované a není hodnoty, že jste již vytvořili.</span><span class="sxs-lookup"><span data-stu-id="d524c-210">Because hello scripts expect cluster names and endpoints, hello values should be planned and not values that you have already created.</span></span>

1. <span data-ttu-id="d524c-211">[Stáhněte si skripty hello] [ sf-aad-ps-script-download] tooyour počítače.</span><span class="sxs-lookup"><span data-stu-id="d524c-211">[Download hello scripts][sf-aad-ps-script-download] tooyour computer.</span></span>
2. <span data-ttu-id="d524c-212">Klikněte pravým tlačítkem na hello soubor zip, vyberte **vlastnosti**, vyberte hello **Odblokovat** zaškrtněte políčko a potom klikněte na **použít**.</span><span class="sxs-lookup"><span data-stu-id="d524c-212">Right-click hello zip file, select **Properties**, select hello **Unblock** check box, and then click **Apply**.</span></span>
3. <span data-ttu-id="d524c-213">Extrahujte soubor zip hello.</span><span class="sxs-lookup"><span data-stu-id="d524c-213">Extract hello zip file.</span></span>
4. <span data-ttu-id="d524c-214">Spustit `SetupApplications.ps1`a zadejte hello TenantId, název clusteru a WebApplicationReplyUrl jako parametry.</span><span class="sxs-lookup"><span data-stu-id="d524c-214">Run `SetupApplications.ps1`, and provide hello TenantId, ClusterName, and WebApplicationReplyUrl as parameters.</span></span> <span data-ttu-id="d524c-215">Například:</span><span class="sxs-lookup"><span data-stu-id="d524c-215">For example:</span></span>

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    <span data-ttu-id="d524c-216">Vaše ID Tenanta můžete najít spuštěním příkazu Powershellu hello `Get-AzureSubscription`.</span><span class="sxs-lookup"><span data-stu-id="d524c-216">You can find your TenantId by executing hello PowerShell command `Get-AzureSubscription`.</span></span> <span data-ttu-id="d524c-217">Provádění tento příkaz zobrazí hello TenantId pro každé předplatné.</span><span class="sxs-lookup"><span data-stu-id="d524c-217">Executing this command displays hello TenantId for every subscription.</span></span>

    <span data-ttu-id="d524c-218">Název clusteru je použité tooprefix hello Azure AD aplikace, které jsou vytvořeny skriptem hello.</span><span class="sxs-lookup"><span data-stu-id="d524c-218">ClusterName is used tooprefix hello Azure AD applications that are created by hello script.</span></span> <span data-ttu-id="d524c-219">Přesně nepotřebuje toomatch hello skutečný název clusteru.</span><span class="sxs-lookup"><span data-stu-id="d524c-219">It does not need toomatch hello actual cluster name exactly.</span></span> <span data-ttu-id="d524c-220">Je určený jenom toomake je snazší toomap Azure AD artefakty toohello Service Fabric clusteru, který se používá s.</span><span class="sxs-lookup"><span data-stu-id="d524c-220">It is intended only toomake it easier toomap Azure AD artifacts toohello Service Fabric cluster that they're being used with.</span></span>

    <span data-ttu-id="d524c-221">WebApplicationReplyUrl je výchozí koncový bod hello vracející Azure AD Uživatelé tooyour po jejich dokončení přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d524c-221">WebApplicationReplyUrl is hello default endpoint that Azure AD returns tooyour users after they finish signing in.</span></span> <span data-ttu-id="d524c-222">Nastavte tento koncový bod jako koncový bod Service Fabric Explorer hello pro váš cluster, který ve výchozím nastavení je:</span><span class="sxs-lookup"><span data-stu-id="d524c-222">Set this endpoint as hello Service Fabric Explorer endpoint for your cluster, which by default is:</span></span>

    <span data-ttu-id="d524c-223">https://&lt;cluster_domain&gt;: 19080/Explorer</span><span class="sxs-lookup"><span data-stu-id="d524c-223">https://&lt;cluster_domain&gt;:19080/Explorer</span></span>

    <span data-ttu-id="d524c-224">Jste výzvami toosign v tooan účtu, který má oprávnění správce pro tenanta hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d524c-224">You are prompted toosign in tooan account that has administrative privileges for hello Azure AD tenant.</span></span> <span data-ttu-id="d524c-225">Po přihlášení, hello skript vytvoří hello web a nativní aplikace toorepresent cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d524c-225">After you sign in, hello script creates hello web and native applications toorepresent your Service Fabric cluster.</span></span> <span data-ttu-id="d524c-226">Pokud si prohlédnete hello klienta aplikace v hello [portál Azure classic][azure-classic-portal], měli byste vidět dvě nové položky:</span><span class="sxs-lookup"><span data-stu-id="d524c-226">If you look at hello tenant's applications in hello [Azure classic portal][azure-classic-portal], you should see two new entries:</span></span>

   * <span data-ttu-id="d524c-227">*Název clusteru*\_clusteru</span><span class="sxs-lookup"><span data-stu-id="d524c-227">*ClusterName*\_Cluster</span></span>
   * <span data-ttu-id="d524c-228">*Název clusteru*\_klienta</span><span class="sxs-lookup"><span data-stu-id="d524c-228">*ClusterName*\_Client</span></span>

   <span data-ttu-id="d524c-229">skript Hello vytiskne hello JSON vyžadují šablony Azure Resource Manageru hello při vytváření clusteru hello v další části hello, takže je vhodné okno PowerShell hello tookeep otevřete.</span><span class="sxs-lookup"><span data-stu-id="d524c-229">hello script prints hello JSON required by hello Azure Resource Manager template when you create hello cluster in hello next section, so it's a good idea tookeep hello PowerShell window open.</span></span>

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a><span data-ttu-id="d524c-230">Vytvoření šablony správce prostředků clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="d524c-230">Create a Service Fabric cluster Resource Manager template</span></span>
<span data-ttu-id="d524c-231">V této části hello výstupy z hello předchozí příkazy prostředí PowerShell se používají v šabloně Resource Manager clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d524c-231">In this section, hello outputs of hello preceding PowerShell commands are used in a Service Fabric cluster Resource Manager template.</span></span>

<span data-ttu-id="d524c-232">Ukázka Resource Manager šablony jsou dostupné v hello [šablony Azure úvodní Galerie na Githubu][azure-quickstart-templates].</span><span class="sxs-lookup"><span data-stu-id="d524c-232">Sample Resource Manager templates are available in hello [Azure quick-start template gallery on GitHub][azure-quickstart-templates].</span></span> <span data-ttu-id="d524c-233">Tyto šablony slouží jako výchozí bod pro šablonu clusteru.</span><span class="sxs-lookup"><span data-stu-id="d524c-233">These templates can be used as a starting point for your cluster template.</span></span>

### <a name="create-hello-resource-manager-template"></a><span data-ttu-id="d524c-234">Vytvoření šablony Resource Manageru hello</span><span class="sxs-lookup"><span data-stu-id="d524c-234">Create hello Resource Manager template</span></span>
<span data-ttu-id="d524c-235">Tato příručka používá hello [5 uzlu clusteru zabezpečené] [ service-fabric-secure-cluster-5-node-1-nodetype] příklad šablony a parametry šablony.</span><span class="sxs-lookup"><span data-stu-id="d524c-235">This guide uses hello [5-node secure cluster][service-fabric-secure-cluster-5-node-1-nodetype] example template and template parameters.</span></span> <span data-ttu-id="d524c-236">Stáhněte si `azuredeploy.json` a `azuredeploy.parameters.json` tooyour počítače a otevřete oba soubory ve svém oblíbeném textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="d524c-236">Download `azuredeploy.json` and `azuredeploy.parameters.json` tooyour computer and open both files in your favorite text editor.</span></span>

### <a name="add-certificates"></a><span data-ttu-id="d524c-237">Přidat certifikáty</span><span class="sxs-lookup"><span data-stu-id="d524c-237">Add certificates</span></span>
<span data-ttu-id="d524c-238">Pod položkou hello trezoru klíčů, který obsahuje klíče certifikátu hello přidáte šablony správce prostředků clusteru tooa certifikáty.</span><span class="sxs-lookup"><span data-stu-id="d524c-238">You add certificates tooa cluster Resource Manager template by referencing hello key vault that contains hello certificate keys.</span></span> <span data-ttu-id="d524c-239">Doporučujeme umístit hello klíč trezoru hodnoty v souboru parametrů šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="d524c-239">We recommend that you place hello key-vault values in a Resource Manager template parameters file.</span></span> <span data-ttu-id="d524c-240">Tím zajistí, že budou hello Resource Manager soubor šablony opakovaně použitelné volné nasazení konkrétní tooa hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d524c-240">Doing so keeps hello Resource Manager template file reusable and free of values specific tooa deployment.</span></span>

#### <a name="add-all-certificates-toohello-virtual-machine-scale-set-osprofile"></a><span data-ttu-id="d524c-241">Přidejte že všechny certifikáty toohello škálovací sady virtuálních počítačů osProfile</span><span class="sxs-lookup"><span data-stu-id="d524c-241">Add all certificates toohello virtual machine scale set osProfile</span></span>
<span data-ttu-id="d524c-242">Každý certifikát, který je nainstalován v clusteru hello musí být nakonfigurované v části osProfile hello hello škálování sadu prostředků (Microsoft.Compute/virtualMachineScaleSets).</span><span class="sxs-lookup"><span data-stu-id="d524c-242">Every certificate that's installed in hello cluster must be configured in hello osProfile section of hello scale set resource (Microsoft.Compute/virtualMachineScaleSets).</span></span> <span data-ttu-id="d524c-243">Tato akce nastaví hello hello certifikát tooinstall poskytovatele prostředků na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d524c-243">This action instructs hello resource provider tooinstall hello certificate on hello VMs.</span></span> <span data-ttu-id="d524c-244">Tato instalace zahrnuje hello clusteru certifikátů a certifikáty zabezpečení jakékoli aplikace, abyste naplánovali toouse pro vaše aplikace:</span><span class="sxs-lookup"><span data-stu-id="d524c-244">This installation includes both hello cluster certificate and any application security certificates that you plan toouse for your applications:</span></span>

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-hello-service-fabric-cluster-certificate"></a><span data-ttu-id="d524c-245">Konfigurace certifikátu clusteru Service Fabric hello</span><span class="sxs-lookup"><span data-stu-id="d524c-245">Configure hello Service Fabric cluster certificate</span></span>
<span data-ttu-id="d524c-246">certifikát pro ověřování Hello clusteru musí být konfigurované v prostředku clusteru Service Fabric obou hello (Microsoft.ServiceFabric/clusters) a nastaví hello Service Fabric rozšíření pro škálování virtuálních počítačů v prostředek sady škálování virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="d524c-246">hello cluster authentication certificate must be configured in both hello Service Fabric cluster resource (Microsoft.ServiceFabric/clusters) and hello Service Fabric extension for virtual machine scale sets in hello virtual machine scale set resource.</span></span> <span data-ttu-id="d524c-247">Toto uspořádání umožňuje tooconfigure zprostředkovatele prostředků Service Fabric hello ho použít pro ověření clusteru a ověření serveru pro správu koncové body.</span><span class="sxs-lookup"><span data-stu-id="d524c-247">This arrangement allows hello Service Fabric resource provider tooconfigure it for use for cluster authentication and server authentication for management endpoints.</span></span>

##### <a name="virtual-machine-scale-set-resource"></a><span data-ttu-id="d524c-248">Škálovací sady virtuálních počítačů prostředků:</span><span class="sxs-lookup"><span data-stu-id="d524c-248">Virtual machine scale set resource:</span></span>
```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a><span data-ttu-id="d524c-249">Prostředek infrastruktury služby:</span><span class="sxs-lookup"><span data-stu-id="d524c-249">Service Fabric resource:</span></span>
```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-azure-ad-configuration"></a><span data-ttu-id="d524c-250">Vložit konfigurace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d524c-250">Insert Azure AD configuration</span></span>
<span data-ttu-id="d524c-251">Konfigurace Hello Azure AD, kterou jste vytvořili dříve lze vložit přímo do vaší šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="d524c-251">hello Azure AD configuration that you created earlier can be inserted directly into your Resource Manager template.</span></span> <span data-ttu-id="d524c-252">Jsme ale doporučujeme, abyste nejdřív extrahovat hello hodnoty do s parametry souboru tookeep hello Resource Manager šablony opakovaně použitelného a bez nasazení konkrétní tooa hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d524c-252">However, we recommended that you first extract hello values into a parameters file tookeep hello Resource Manager template reusable and free of values specific tooa deployment.</span></span>

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### <span data-ttu-id="d524c-253">< "Konfigurace arm" ></a>parametry šablony konfigurace správce prostředků</span><span class="sxs-lookup"><span data-stu-id="d524c-253"><a "configure-arm" ></a>Configure Resource Manager template parameters</span></span>
<!--- Loc Comment: It seems that <a "configure-arm" > must be replaced with <a name="configure-arm"></a> since hello link seems not toobe redirecting correctly --->
<span data-ttu-id="d524c-254">Nakonec použijte hodnoty výstup hello z trezoru klíčů hello a souboru parametrů Azure AD PowerShell příkazy toopopulate hello:</span><span class="sxs-lookup"><span data-stu-id="d524c-254">Finally, use hello output values from hello key vault and Azure AD PowerShell commands toopopulate hello parameters file:</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
<span data-ttu-id="d524c-255">V tomto okamžiku byste měli mít následující prvky zavedené hello:</span><span class="sxs-lookup"><span data-stu-id="d524c-255">At this point, you should have hello following elements in place:</span></span>

* <span data-ttu-id="d524c-256">Skupina prostředků trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="d524c-256">Key vault resource group</span></span>
  * <span data-ttu-id="d524c-257">Trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="d524c-257">Key vault</span></span>
  * <span data-ttu-id="d524c-258">Ověřovací certifikát serverů clusteru</span><span class="sxs-lookup"><span data-stu-id="d524c-258">Cluster server authentication certificate</span></span>
  * <span data-ttu-id="d524c-259">Certifikát pro šifrování dat</span><span class="sxs-lookup"><span data-stu-id="d524c-259">Data encipherment certificate</span></span>
* <span data-ttu-id="d524c-260">Klientovi služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d524c-260">Azure Active Directory tenant</span></span>
  * <span data-ttu-id="d524c-261">Aplikace Azure AD pro správu webových a Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="d524c-261">Azure AD application for web-based management and Service Fabric Explorer</span></span>
  * <span data-ttu-id="d524c-262">Aplikace pro správu nativního klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d524c-262">Azure AD application for native client management</span></span>
  * <span data-ttu-id="d524c-263">Uživatelé a jejich přiřazené role</span><span class="sxs-lookup"><span data-stu-id="d524c-263">Users and their assigned roles</span></span>
* <span data-ttu-id="d524c-264">Šablony Resource Manageru clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="d524c-264">Service Fabric cluster Resource Manager template</span></span>
  * <span data-ttu-id="d524c-265">Certifikáty nakonfigurovaný pomocí trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="d524c-265">Certificates configured through key vault</span></span>
  * <span data-ttu-id="d524c-266">Azure Active Directory, které jsou nakonfigurované</span><span class="sxs-lookup"><span data-stu-id="d524c-266">Azure Active Directory configured</span></span>

<span data-ttu-id="d524c-267">Hello následující diagram znázorňuje, kde trezoru klíčů a konfigurace Azure AD zapadnou do vaší šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="d524c-267">hello following diagram illustrates where your key vault and Azure AD configuration fit into your Resource Manager template.</span></span>

![Mapa závislostí Resource Manager][cluster-security-arm-dependency-map]

## <a name="create-hello-cluster"></a><span data-ttu-id="d524c-269">Vytvoření clusteru hello</span><span class="sxs-lookup"><span data-stu-id="d524c-269">Create hello cluster</span></span>
<span data-ttu-id="d524c-270">Nyní jste připravené toocreate hello clusteru pomocí [nasazení šablony Azure resource][resource-group-template-deploy].</span><span class="sxs-lookup"><span data-stu-id="d524c-270">You are now ready toocreate hello cluster by using [Azure resource template deployment][resource-group-template-deploy].</span></span>

#### <a name="test-it"></a><span data-ttu-id="d524c-271">otestovat</span><span class="sxs-lookup"><span data-stu-id="d524c-271">Test it</span></span>
<span data-ttu-id="d524c-272">V souboru parametrů použijte šablony správce prostředků hello následující tootest příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="d524c-272">Use hello following PowerShell command tootest your Resource Manager template with a parameters file:</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a><span data-ttu-id="d524c-273">nasazení</span><span class="sxs-lookup"><span data-stu-id="d524c-273">Deploy it</span></span>
<span data-ttu-id="d524c-274">Pokud testovací šablony Resource Manageru hello úspěšně projde, použijte hello následující toodeploy příkaz prostředí PowerShell vaše šablony Resource Manageru soubor parametrů:</span><span class="sxs-lookup"><span data-stu-id="d524c-274">If hello Resource Manager template test passes, use hello following PowerShell command toodeploy your Resource Manager template with a parameters file:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>

## <a name="assign-users-tooroles"></a><span data-ttu-id="d524c-275">Přiřazení uživatelů tooroles</span><span class="sxs-lookup"><span data-stu-id="d524c-275">Assign users tooroles</span></span>
<span data-ttu-id="d524c-276">Po vytvoření aplikace toorepresent hello cluster, přiřadit uživatelům role toohello nepodporuje Service Fabric: jen pro čtení a správce. Můžete přiřadit role hello pomocí hello [portál Azure classic][azure-classic-portal].</span><span class="sxs-lookup"><span data-stu-id="d524c-276">After you have created hello applications toorepresent your cluster, assign your users toohello roles supported by Service Fabric: read-only and admin. You can assign hello roles by using hello [Azure classic portal][azure-classic-portal].</span></span>

1. <span data-ttu-id="d524c-277">V hello portálu Azure, přejděte tooyour klienta a potom vyberte **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d524c-277">In hello Azure portal, go tooyour tenant, and then select **Applications**.</span></span>
2. <span data-ttu-id="d524c-278">Vyberte hello webové aplikace, který má název, jako je `myTestCluster_Cluster`.</span><span class="sxs-lookup"><span data-stu-id="d524c-278">Select hello web application, which has a name like `myTestCluster_Cluster`.</span></span>
3. <span data-ttu-id="d524c-279">Klikněte na tlačítko hello **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="d524c-279">Click hello **Users** tab.</span></span>
4. <span data-ttu-id="d524c-280">Vyberte uživatele tooassign a potom klikněte na hello **přiřadit** tlačítko dole hello úvodní obrazovka.</span><span class="sxs-lookup"><span data-stu-id="d524c-280">Select a user tooassign, and then click hello **Assign** button at hello bottom of hello screen.</span></span>

    ![Přiřazení uživatelů tooroles tlačítko][assign-users-to-roles-button]
5. <span data-ttu-id="d524c-282">Vyberte hello role tooassign toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="d524c-282">Select hello role tooassign toohello user.</span></span>

    ![Dialogové okno "Přiřazení uživatelů"][assign-users-to-roles-dialog]

> [!NOTE]
> <span data-ttu-id="d524c-284">Další informace o rolích v Service Fabric najdete v tématu [řízení přístupu na základě rolí pro Service Fabric klienty](service-fabric-cluster-security-roles.md).</span><span class="sxs-lookup"><span data-stu-id="d524c-284">For more information about roles in Service Fabric, see [Role-based access control for Service Fabric clients](service-fabric-cluster-security-roles.md).</span></span>
>
>

 <a name="secure-linux-clusters"></a>
 <!--- Loc Comment: It seems that letter S in cluster was missing, which caused hello wrong redirection of hello link --->

## <a name="create-secure-clusters-on-linux"></a><span data-ttu-id="d524c-285">Vytvoření zabezpečeného clusterů v systému Linux</span><span class="sxs-lookup"><span data-stu-id="d524c-285">Create secure clusters on Linux</span></span>
<span data-ttu-id="d524c-286">proces toomake hello jednodušší, uvádíme [pomocné rutiny skriptu](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux).</span><span class="sxs-lookup"><span data-stu-id="d524c-286">toomake hello process easier, we have provided a [helper script](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux).</span></span> <span data-ttu-id="d524c-287">Než použijete tento skript pomocné rutiny, ujistěte se, že už máte rozhraní příkazového řádku Azure (CLI) nainstalovaná a je v cestě.</span><span class="sxs-lookup"><span data-stu-id="d524c-287">Before you use this helper script, ensure that you already have Azure command-line interface (CLI) installed, and it is in your path.</span></span> <span data-ttu-id="d524c-288">Ujistěte se, že hello skript má oprávnění tooexecute spuštěním `chmod +x cert_helper.py` po stáhnout.</span><span class="sxs-lookup"><span data-stu-id="d524c-288">Make sure that hello script has permissions tooexecute by running `chmod +x cert_helper.py` after downloading it.</span></span> <span data-ttu-id="d524c-289">prvním krokem Hello je toosign v tooyour účet Azure pomocí rozhraní příkazového řádku s hello `azure login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="d524c-289">hello first step is toosign in tooyour Azure account by using CLI with hello `azure login` command.</span></span> <span data-ttu-id="d524c-290">Po přihlášení tooyour účet Azure, použijte hello pomocné rutiny, skript se certifikační Autorita certifikátu podepsaného držitelem, jako hello následující příkaz ukazuje:</span><span class="sxs-lookup"><span data-stu-id="d524c-290">After signing in tooyour Azure account, use hello helper script with your CA signed certificate, as hello following command shows:</span></span>

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]
```

<span data-ttu-id="d524c-291">Parametr - ifile Hello může trvat soubor .pfx nebo soubor .pem jako vstup pro typ certifikátu hello (pfx nebo pem nebo ss, pokud je certifikát podepsaný svým držitelem).</span><span class="sxs-lookup"><span data-stu-id="d524c-291">hello -ifile parameter can take a .pfx file or a .pem file as input, with hello certificate type (pfx or pem, or ss if it is a self-signed certificate).</span></span>
<span data-ttu-id="d524c-292">Hello parametr -h vytiskne textu hello nápovědy.</span><span class="sxs-lookup"><span data-stu-id="d524c-292">hello parameter -h prints out hello help text.</span></span>


<span data-ttu-id="d524c-293">Tento příkaz vrátí hello následující tři řetězce jako výstup hello:</span><span class="sxs-lookup"><span data-stu-id="d524c-293">This command returns hello following three strings as hello output:</span></span>

* <span data-ttu-id="d524c-294">SourceVaultID, což je hello ID pro hello se vytvoří nový KeyVault ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d524c-294">SourceVaultID, which is hello ID for hello new KeyVault ResourceGroup it created for you</span></span>
* <span data-ttu-id="d524c-295">CertificateUrl pro přístup k certifikátu hello</span><span class="sxs-lookup"><span data-stu-id="d524c-295">CertificateUrl for accessing hello certificate</span></span>
* <span data-ttu-id="d524c-296">Miniatura certifikátu, který se používá pro ověřování</span><span class="sxs-lookup"><span data-stu-id="d524c-296">CertificateThumbprint, which is used for authentication</span></span>

<span data-ttu-id="d524c-297">Hello následující příklad ukazuje, jak toouse hello příkaz:</span><span class="sxs-lookup"><span data-stu-id="d524c-297">hello following example shows how toouse hello command:</span></span>

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
<span data-ttu-id="d524c-298">Provádění hello předcházející příkaz poskytuje hello tři řetězce následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d524c-298">Executing hello preceding command gives you hello three strings as follows:</span></span>

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

<span data-ttu-id="d524c-299">Název subjektu certifikátu Hello musí odpovídat hello domény, že používáte cluster Service Fabric tooaccess hello.</span><span class="sxs-lookup"><span data-stu-id="d524c-299">hello certificate's subject name must match hello domain that you use tooaccess hello Service Fabric cluster.</span></span> <span data-ttu-id="d524c-300">Toto porovnání se vyžaduje tooprovide protokolem SSL pro koncové body správy protokolu HTTPS hello clusteru a Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="d524c-300">This match is required tooprovide an SSL for hello cluster's HTTPS management endpoints and Service Fabric Explorer.</span></span> <span data-ttu-id="d524c-301">Nelze získat certifikát SSL od certifikační Autority pro hello `.cloudapp.azure.com` domény.</span><span class="sxs-lookup"><span data-stu-id="d524c-301">You cannot obtain an SSL certificate from a CA for hello `.cloudapp.azure.com` domain.</span></span> <span data-ttu-id="d524c-302">Je nutné získat vlastní název domény pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="d524c-302">You must obtain a custom domain name for your cluster.</span></span> <span data-ttu-id="d524c-303">Pokud budete požadovat certifikát od certifikační Autority, hello názvu subjektu certifikátu musí odpovídat hello název vlastní domény, který používáte pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="d524c-303">When you request a certificate from a CA, hello certificate's subject name must match hello custom domain name that you use for your cluster.</span></span>

<span data-ttu-id="d524c-304">Tyto názvy subjektu jsou hello položky je třeba toocreate zabezpečení clusteru Service Fabric (bez Azure AD), jak je popsáno v [parametrů šablony Resource Manageru nakonfigurovat](#configure-arm).</span><span class="sxs-lookup"><span data-stu-id="d524c-304">These subject names are hello entries you need toocreate a secure Service Fabric cluster (without Azure AD), as described at [Configure Resource Manager template parameters](#configure-arm).</span></span> <span data-ttu-id="d524c-305">Zabezpečení clusteru toohello můžete připojit podle pokynů hello pro [ověřování clusteru tooa přístupu klienta](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="d524c-305">You can connect toohello secure cluster by following hello instructions for [authenticating client access tooa cluster](service-fabric-connect-to-secure-cluster.md).</span></span> <span data-ttu-id="d524c-306">Linux preview clustery nepodporují ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d524c-306">Linux preview clusters do not support Azure AD authentication.</span></span> <span data-ttu-id="d524c-307">Můžete přiřadit role správce a klienta, jak je popsáno v hello [přiřadit role toousers](#assign-roles) části.</span><span class="sxs-lookup"><span data-stu-id="d524c-307">You can assign admin and client roles as described in hello [Assign roles toousers](#assign-roles) section.</span></span> <span data-ttu-id="d524c-308">Když zadáte role správce a klienta pro Linux preview clusteru, máte tooprovide kryptografické otisky certifikátů pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="d524c-308">When you specify admin and client roles for a Linux preview cluster, you have tooprovide certificate thumbprints for authentication.</span></span> <span data-ttu-id="d524c-309">(Nezadáte název subjektu hello, protože žádné ověření řetězu nebo odvolání je prováděna v této verzi preview.)</span><span class="sxs-lookup"><span data-stu-id="d524c-309">(You do not provide hello subject name, because no chain validation or revocation is being performed in this preview release.)</span></span>

<span data-ttu-id="d524c-310">Pokud chcete toouse certifikát podepsaný svým držitelem pro testování, můžete použít stejné toogenerate skriptu, jeden hello.</span><span class="sxs-lookup"><span data-stu-id="d524c-310">If you want toouse a self-signed certificate for testing, you can use hello same script toogenerate one.</span></span> <span data-ttu-id="d524c-311">Potom můžete nahrát hello certifikát tooyour trezoru klíčů tím, že poskytuje hello příznak `ss` místo hello název a cesta k certifikátu certifikátu.</span><span class="sxs-lookup"><span data-stu-id="d524c-311">You can then upload hello certificate tooyour key vault by providing hello flag `ss` instead of providing hello certificate path and certificate name.</span></span> <span data-ttu-id="d524c-312">Například najdete hello následující příkaz pro vytvoření a odeslání certifikát podepsaný svým držitelem:</span><span class="sxs-lookup"><span data-stu-id="d524c-312">For example, see hello following command for creating and uploading a self-signed certificate:</span></span>

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net"
```
<span data-ttu-id="d524c-313">Tento příkaz vrátí hello stejné tři řetězce: SourceVault, CertificateUrl a CertificateThumbprint.</span><span class="sxs-lookup"><span data-stu-id="d524c-313">This command returns hello same three strings: SourceVault, CertificateUrl, and CertificateThumbprint.</span></span> <span data-ttu-id="d524c-314">Potom můžete hello řetězce toocreate zabezpečené Linux clusteru a umístění, kde je umístěn hello certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="d524c-314">You can then use hello strings toocreate both a secure Linux cluster and a location where hello self-signed certificate is placed.</span></span> <span data-ttu-id="d524c-315">Je nutné hello certifikát podepsaný svým držitelem tooconnect toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="d524c-315">You need hello self-signed certificate tooconnect toohello cluster.</span></span> <span data-ttu-id="d524c-316">Zabezpečení clusteru toohello můžete připojit podle pokynů hello pro [ověřování clusteru tooa přístupu klienta](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="d524c-316">You can connect toohello secure cluster by following hello instructions for [authenticating client access tooa cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

<span data-ttu-id="d524c-317">Název subjektu certifikátu Hello musí odpovídat hello domény, že používáte cluster Service Fabric tooaccess hello.</span><span class="sxs-lookup"><span data-stu-id="d524c-317">hello certificate's subject name must match hello domain that you use tooaccess hello Service Fabric cluster.</span></span> <span data-ttu-id="d524c-318">Toto porovnání se vyžaduje tooprovide protokolem SSL pro koncové body správy protokolu HTTPS hello clusteru a Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="d524c-318">This match is required tooprovide an SSL for hello cluster's HTTPS management endpoints and Service Fabric Explorer.</span></span> <span data-ttu-id="d524c-319">Nelze získat certifikát SSL od certifikační Autority pro hello `.cloudapp.azure.com` domény.</span><span class="sxs-lookup"><span data-stu-id="d524c-319">You cannot obtain an SSL certificate from a CA for hello `.cloudapp.azure.com` domain.</span></span> <span data-ttu-id="d524c-320">Je nutné získat vlastní název domény pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="d524c-320">You must obtain a custom domain name for your cluster.</span></span> <span data-ttu-id="d524c-321">Pokud budete požadovat certifikát od certifikační Autority, hello názvu subjektu certifikátu musí odpovídat hello název vlastní domény, který používáte pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="d524c-321">When you request a certificate from a CA, hello certificate's subject name must match hello custom domain name that you use for your cluster.</span></span>

<span data-ttu-id="d524c-322">Můžete vyplnit hello parametry z hello pomocná skript v hello portál Azure, jak je popsáno v hello [vytvořit cluster v hello portál Azure](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal) části.</span><span class="sxs-lookup"><span data-stu-id="d524c-322">You can fill hello parameters from hello helper script in hello Azure portal, as described in hello [Create a cluster in hello Azure portal](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal) section.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d524c-323">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d524c-323">Next steps</span></span>
<span data-ttu-id="d524c-324">V tuto chvíli máte zabezpečené cluster s poskytnete správu ověřování Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d524c-324">At this point, you have a secure cluster with Azure Active Directory providing management authentication.</span></span> <span data-ttu-id="d524c-325">Dále [připojení clusteru tooyour](service-fabric-connect-to-secure-cluster.md) a zjistěte, jak příliš[spravovat tajné klíče aplikace](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="d524c-325">Next, [connect tooyour cluster](service-fabric-connect-to-secure-cluster.md) and learn how too[manage application secrets](service-fabric-application-secret-management.md).</span></span>

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a><span data-ttu-id="d524c-326">Řešení potíží s nastavení Azure Active Directory pro ověřování klientů</span><span class="sxs-lookup"><span data-stu-id="d524c-326">Troubleshoot setting up Azure Active Directory for client authentication</span></span>
<span data-ttu-id="d524c-327">Pokud narazíte na problém při při instalaci Azure AD pro ověřování klientů, přečtěte si hello možná řešení v této části.</span><span class="sxs-lookup"><span data-stu-id="d524c-327">If you run into an issue while you're setting up Azure AD for client authentication, review hello potential solutions in this section.</span></span>

### <a name="service-fabric-explorer-prompts-you-tooselect-a-certificate"></a><span data-ttu-id="d524c-328">Service Fabric Explorer vyzve tooselect certifikát</span><span class="sxs-lookup"><span data-stu-id="d524c-328">Service Fabric Explorer prompts you tooselect a certificate</span></span>
#### <a name="problem"></a><span data-ttu-id="d524c-329">Problém</span><span class="sxs-lookup"><span data-stu-id="d524c-329">Problem</span></span>
<span data-ttu-id="d524c-330">Po přihlášení úspěšně tooAzure AD v Service Fabric Exploreru hello prohlížeče vrací toohello domovskou stránku, ale dotaz, zda tooselect certifikát.</span><span class="sxs-lookup"><span data-stu-id="d524c-330">After you sign in successfully tooAzure AD in Service Fabric Explorer, hello browser returns toohello home page but a message prompts you tooselect a certificate.</span></span>

![Vyberte certifikát SFX dialogové okno][sfx-select-certificate-dialog]

#### <a name="reason"></a><span data-ttu-id="d524c-332">Důvod</span><span class="sxs-lookup"><span data-stu-id="d524c-332">Reason</span></span>
<span data-ttu-id="d524c-333">uživatel Hello není přiřazenou roli v hello clusteru aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d524c-333">hello user isn’t assigned a role in hello Azure AD cluster application.</span></span> <span data-ttu-id="d524c-334">Ověření Azure AD se proto nezdaří na cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d524c-334">Thus, Azure AD authentication fails on Service Fabric cluster.</span></span> <span data-ttu-id="d524c-335">Service Fabric Explorer spadne zpět toocertificate ověřování.</span><span class="sxs-lookup"><span data-stu-id="d524c-335">Service Fabric Explorer falls back toocertificate authentication.</span></span>

#### <a name="solution"></a><span data-ttu-id="d524c-336">Řešení</span><span class="sxs-lookup"><span data-stu-id="d524c-336">Solution</span></span>
<span data-ttu-id="d524c-337">Postupujte podle pokynů hello pro nastavení služby Azure AD a přiřadit role uživatele.</span><span class="sxs-lookup"><span data-stu-id="d524c-337">Follow hello instructions for setting up Azure AD, and assign user roles.</span></span> <span data-ttu-id="d524c-338">Také doporučujeme zapnout "Uživatele přiřazení tooaccess požadované aplikace," jako `SetupApplications.ps1` nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="d524c-338">Also, we recommend that you turn on “User assignment required tooaccess app,” as `SetupApplications.ps1` does.</span></span>

### <a name="connection-with-powershell-fails-with-an-error-hello-specified-credentials-are-invalid"></a><span data-ttu-id="d524c-339">Připojení pomocí prostředí PowerShell selže s chybou: "hello zadané přihlašovací údaje jsou neplatné"</span><span class="sxs-lookup"><span data-stu-id="d524c-339">Connection with PowerShell fails with an error: "hello specified credentials are invalid"</span></span>
#### <a name="problem"></a><span data-ttu-id="d524c-340">Problém</span><span class="sxs-lookup"><span data-stu-id="d524c-340">Problem</span></span>
<span data-ttu-id="d524c-341">Pokud používáte PowerShell tooconnect toohello clusteru pomocí "Azureactivedirectory selhala" režim zabezpečení, po přihlášení úspěšně tooAzure AD, hello připojení se nezdaří s chybou: "hello zadat přihlašovací údaje jsou neplatné."</span><span class="sxs-lookup"><span data-stu-id="d524c-341">When you use PowerShell tooconnect toohello cluster by using “AzureActiveDirectory” security mode, after you sign in successfully tooAzure AD, hello connection fails with an error: "hello specified credentials are invalid."</span></span>

#### <a name="solution"></a><span data-ttu-id="d524c-342">Řešení</span><span class="sxs-lookup"><span data-stu-id="d524c-342">Solution</span></span>
<span data-ttu-id="d524c-343">Toto řešení je hello stejný jako hello předcházející jeden.</span><span class="sxs-lookup"><span data-stu-id="d524c-343">This solution is hello same as hello preceding one.</span></span>

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a><span data-ttu-id="d524c-344">Service Fabric Explorer vrátí chybu při přihlášení: "AADSTS50011"</span><span class="sxs-lookup"><span data-stu-id="d524c-344">Service Fabric Explorer returns a failure when you sign in: "AADSTS50011"</span></span>
#### <a name="problem"></a><span data-ttu-id="d524c-345">Problém</span><span class="sxs-lookup"><span data-stu-id="d524c-345">Problem</span></span>
<span data-ttu-id="d524c-346">Při pokusu o toosign v tooAzure AD v Service Fabric Exploreru, stránku hello vrátí chybu: "AADSTS50011: hello adresa pro odpověď &lt;url&gt; neodpovídá hello odpovědi adresy nakonfigurované pro aplikace hello: &lt;guid&gt;."</span><span class="sxs-lookup"><span data-stu-id="d524c-346">When you try toosign in tooAzure AD in Service Fabric Explorer, hello page returns a failure: "AADSTS50011: hello reply address &lt;url&gt; does not match hello reply addresses configured for hello application: &lt;guid&gt;."</span></span>

![Adresa pro odpověď SFX neodpovídá.][sfx-reply-address-not-match]

#### <a name="reason"></a><span data-ttu-id="d524c-348">Důvod</span><span class="sxs-lookup"><span data-stu-id="d524c-348">Reason</span></span>
<span data-ttu-id="d524c-349">aplikace Hello clusteru (web), která představuje Service Fabric Exploreru pokusí tooauthenticate služby Azure AD a jako součást požadavku hello poskytuje návratovou adresu URL přesměrování hello.</span><span class="sxs-lookup"><span data-stu-id="d524c-349">hello cluster (web) application that represents Service Fabric Explorer attempts tooauthenticate against Azure AD, and as part of hello request it provides hello redirect return URL.</span></span> <span data-ttu-id="d524c-350">Adresa URL hello není uvedena v aplikaci hello Azure AD, ale **adresa URL odpovědi** seznamu.</span><span class="sxs-lookup"><span data-stu-id="d524c-350">But hello URL is not listed in hello Azure AD application **REPLY URL** list.</span></span>

#### <a name="solution"></a><span data-ttu-id="d524c-351">Řešení</span><span class="sxs-lookup"><span data-stu-id="d524c-351">Solution</span></span>
<span data-ttu-id="d524c-352">Na hello **konfigurace** kartě hello clusteru aplikace (web), přidejte adresu URL Service Fabric Explorer toohello hello **adresa URL odpovědi** seznamu nebo nahradit hello položky v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="d524c-352">On hello **Configure** tab of hello cluster (web) application, add hello URL of Service Fabric Explorer toohello **REPLY URL** list or replace one of hello items in hello list.</span></span> <span data-ttu-id="d524c-353">Po dokončení, uložte změnu.</span><span class="sxs-lookup"><span data-stu-id="d524c-353">When you have finished, save your change.</span></span>

![Adresa url odpovědi pro webové aplikace][web-application-reply-url]

### <a name="connect-hello-cluster-by-using-azure-ad-authentication-via-powershell"></a><span data-ttu-id="d524c-355">Připojte hello cluster pomocí ověřování Azure AD pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d524c-355">Connect hello cluster by using Azure AD authentication via PowerShell</span></span>
<span data-ttu-id="d524c-356">tooconnect hello cluster Service Fabric, použijte hello následující ukázka příkazu prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="d524c-356">tooconnect hello Service Fabric cluster, use hello following PowerShell command example:</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

<span data-ttu-id="d524c-357">najdete v článku o rutině hello Connect-ServiceFabricCluster toolearn [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).</span><span class="sxs-lookup"><span data-stu-id="d524c-357">toolearn about hello Connect-ServiceFabricCluster cmdlet, see [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).</span></span>

### <a name="can-i-reuse-hello-same-azure-ad-tenant-in-multiple-clusters"></a><span data-ttu-id="d524c-358">Můžete opakovaně použít klienta hello stejnou službou Azure AD ve více clusterů?</span><span class="sxs-lookup"><span data-stu-id="d524c-358">Can I reuse hello same Azure AD tenant in multiple clusters?</span></span>
<span data-ttu-id="d524c-359">Ano.</span><span class="sxs-lookup"><span data-stu-id="d524c-359">Yes.</span></span> <span data-ttu-id="d524c-360">Ale mějte na paměti, tooadd hello URL Service Fabric Explorer tooyour clusteru (web) aplikace.</span><span class="sxs-lookup"><span data-stu-id="d524c-360">But remember tooadd hello URL of Service Fabric Explorer tooyour cluster (web) application.</span></span> <span data-ttu-id="d524c-361">Service Fabric Exploreru, jinak nebude fungovat.</span><span class="sxs-lookup"><span data-stu-id="d524c-361">Otherwise, Service Fabric Explorer doesn’t work.</span></span>

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a><span data-ttu-id="d524c-362">Proč stále potřebuji certifikát serveru je zapnuto Azure AD?</span><span class="sxs-lookup"><span data-stu-id="d524c-362">Why do I still need a server certificate while Azure AD is enabled?</span></span>
<span data-ttu-id="d524c-363">FabricClient a FabricGateway provést vzájemné ověření.</span><span class="sxs-lookup"><span data-stu-id="d524c-363">FabricClient and FabricGateway perform a mutual authentication.</span></span> <span data-ttu-id="d524c-364">Během ověřování Azure AD integrace Azure AD poskytuje klienta serveru toohello identit a certifikát serveru hello je identita serveru použité tooverify hello.</span><span class="sxs-lookup"><span data-stu-id="d524c-364">During Azure AD authentication, Azure AD integration provides a client identity toohello server, and hello server certificate is used tooverify hello server identity.</span></span> <span data-ttu-id="d524c-365">Další informace o certifikátech Service Fabric najdete v tématu [certifikáty X.509 a Service Fabric][x509-certificates-and-service-fabric].</span><span class="sxs-lookup"><span data-stu-id="d524c-365">For more information about Service Fabric certificates, see [X.509 certificates and Service Fabric][x509-certificates-and-service-fabric].</span></span>

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png

