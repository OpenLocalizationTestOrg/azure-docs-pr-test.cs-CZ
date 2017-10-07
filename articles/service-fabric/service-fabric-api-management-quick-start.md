---
title: "aaaAzure Service Fabric s úvodní API Management | Microsoft Docs"
description: "Tento průvodce vám ukáže, jak tooquickly Začínáme s Azure API Management a Service Fabric."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.openlocfilehash: f76f3f39a92f89892d6a02ecaab1ec3d343fe2a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-with-azure-api-management-quick-start"></a><span data-ttu-id="4bac6-103">Service Fabric s Azure API Management rychlý Start</span><span class="sxs-lookup"><span data-stu-id="4bac6-103">Service Fabric with Azure API Management Quick Start</span></span>

<span data-ttu-id="4bac6-104">Tento průvodce vám ukáže, jak tooset až Azure API Management s Service Fabric a konfigurace vašeho prvního rozhraní API operaci toosend provoz tooback endové služby v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="4bac6-104">This guide shows you how tooset up Azure API Management with Service Fabric and configure your first API operation toosend traffic tooback-end services in Service Fabric.</span></span> <span data-ttu-id="4bac6-105">toolearn Další informace o službě Azure API Management scénáře s Service Fabric, najdete v části hello [přehled](service-fabric-api-management-overview.md) článku.</span><span class="sxs-lookup"><span data-stu-id="4bac6-105">toolearn more about Azure API Management scenarios with Service Fabric, see hello [overview](service-fabric-api-management-overview.md) article.</span></span> 

## <a name="deploy-api-management-and-service-fabric-tooazure"></a><span data-ttu-id="4bac6-106">Nasazení tooAzure API Management a Service Fabric</span><span class="sxs-lookup"><span data-stu-id="4bac6-106">Deploy API Management and Service Fabric tooAzure</span></span>

<span data-ttu-id="4bac6-107">prvním krokem Hello je toodeploy API Management a tooAzure clusteru Service Fabric ve sdílené virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="4bac6-107">hello first step is toodeploy API Management and a Service Fabric cluster tooAzure in a shared Virtual Network.</span></span> <span data-ttu-id="4bac6-108">To umožňuje API Management toocommunicate přímo pomocí Service Fabric, aby ho provádět zjišťování služby, řešení oddílu služby a předat dál provoz přímo tooany back-end služby v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="4bac6-108">This allows API Management toocommunicate directly with Service Fabric so it can perform service discovery, service partition resolution, and forward traffic directly tooany backend service in Service Fabric.</span></span>

### <a name="topology"></a><span data-ttu-id="4bac6-109">topologie</span><span class="sxs-lookup"><span data-stu-id="4bac6-109">Topology</span></span>

<span data-ttu-id="4bac6-110">Tento průvodce nasadí hello následující topologie tooAzure, ve které jsou v podsítích v API Management a Service Fabric hello stejné virtuální síti:</span><span class="sxs-lookup"><span data-stu-id="4bac6-110">This guide deploys hello following topology tooAzure in which API Management and Service Fabric are in subnets of hello same Virtual Network:</span></span>

 ![Popisek obrázku][sf-apim-topology-overview]

<span data-ttu-id="4bac6-112">tooget rychle začít, šablony správce prostředků slouží pro každého kroku nasazení:</span><span class="sxs-lookup"><span data-stu-id="4bac6-112">tooget started quickly, Resource Manager templates are provided for each deployment step:</span></span>

 - <span data-ttu-id="4bac6-113">Topologie sítě:</span><span class="sxs-lookup"><span data-stu-id="4bac6-113">Network topology:</span></span>
    - <span data-ttu-id="4bac6-114">[Network.JSON][network-arm]</span><span class="sxs-lookup"><span data-stu-id="4bac6-114">[network.json][network-arm]</span></span>
    - <span data-ttu-id="4bac6-115">[Network.Parameters.JSON][network-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="4bac6-115">[network.parameters.json][network-parameters-arm]</span></span>
 - <span data-ttu-id="4bac6-116">Cluster Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="4bac6-116">Service Fabric cluster:</span></span>
    - <span data-ttu-id="4bac6-117">[cluster.JSON][cluster-arm]</span><span class="sxs-lookup"><span data-stu-id="4bac6-117">[cluster.json][cluster-arm]</span></span>
    - <span data-ttu-id="4bac6-118">[cluster.Parameters.JSON][cluster-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="4bac6-118">[cluster.parameters.json][cluster-parameters-arm]</span></span>
 - <span data-ttu-id="4bac6-119">API Management:</span><span class="sxs-lookup"><span data-stu-id="4bac6-119">API Management:</span></span>
    - <span data-ttu-id="4bac6-120">[APIM.JSON][apim-arm]</span><span class="sxs-lookup"><span data-stu-id="4bac6-120">[apim.json][apim-arm]</span></span>
    - <span data-ttu-id="4bac6-121">[APIM.Parameters.JSON][apim-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="4bac6-121">[apim.parameters.json][apim-parameters-arm]</span></span>

### <a name="sign-in-tooazure-and-select-your-subscription"></a><span data-ttu-id="4bac6-122">Přihlaste se tooAzure a vybrat své předplatné</span><span class="sxs-lookup"><span data-stu-id="4bac6-122">Sign in tooAzure and select your subscription</span></span>

<span data-ttu-id="4bac6-123">Tato příručka používá [prostředí Azure PowerShell][azure-powershell].</span><span class="sxs-lookup"><span data-stu-id="4bac6-123">This guide uses [Azure PowerShell][azure-powershell].</span></span> <span data-ttu-id="4bac6-124">Po spuštění novou relaci prostředí PowerShell se přihlaste tooyour účet Azure a vybrat své předplatné před spuštěním příkazů Azure.</span><span class="sxs-lookup"><span data-stu-id="4bac6-124">When you start a new PowerShell session, sign in tooyour Azure account and select your subscription before you execute Azure commands.</span></span>
 
<span data-ttu-id="4bac6-125">Přihlaste se tooyour účet Azure:</span><span class="sxs-lookup"><span data-stu-id="4bac6-125">Sign in tooyour Azure account:</span></span>

```powershell
PS > Login-AzureRmAccount
```

<span data-ttu-id="4bac6-126">Vyberte předplatné:</span><span class="sxs-lookup"><span data-stu-id="4bac6-126">Select your subscription:</span></span>

```powershell
PS > Get-AzureRmSubscription
PS > Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="create-a-resource-group"></a><span data-ttu-id="4bac6-127">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="4bac6-127">Create a resource group</span></span>

<span data-ttu-id="4bac6-128">Vytvořte novou skupinu prostředků pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="4bac6-128">Create a new resource group for your deployment.</span></span> <span data-ttu-id="4bac6-129">Poskytněte název a umístění.</span><span class="sxs-lookup"><span data-stu-id="4bac6-129">Give it a name and a location.</span></span>

```powershell
PS > New-AzureRmResourceGroup -Name <my-resource-group> -Location westus
```

### <a name="deploy-hello-network-topology"></a><span data-ttu-id="4bac6-130">Nasazení hello síťové topologie</span><span class="sxs-lookup"><span data-stu-id="4bac6-130">Deploy hello network topology</span></span>

<span data-ttu-id="4bac6-131">prvním krokem Hello je tooset až hello síťové topologie toowhich API Management a nasadí se cluster Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="4bac6-131">hello first step is tooset up hello network topology toowhich API Management and hello Service Fabric cluster will be deployed.</span></span> <span data-ttu-id="4bac6-132">Hello [network.json] [ network-arm] šablony Resource Manageru je nakonfigurované toocreate virtuální sítě (virtuální sítě) se dvěma podsítěmi a skupiny zabezpečení dvě sítě (NSG).</span><span class="sxs-lookup"><span data-stu-id="4bac6-132">hello [network.json][network-arm] Resource Manager template is configured toocreate a Virtual Network (VNET) with two subnets and two Network Security Groups (NSG).</span></span> 

<span data-ttu-id="4bac6-133">Hello [network.parameters.json] [ network-parameters-arm] soubor parametrů obsahuje názvy hello hello podsítě a skupin Nsg, které API Management a Service Fabric se nasadí do.</span><span class="sxs-lookup"><span data-stu-id="4bac6-133">hello [network.parameters.json][network-parameters-arm] parameters file contains hello names of hello subnets and NSGs that API Management and Service Fabric will be deployed to.</span></span> <span data-ttu-id="4bac6-134">Tato příručka hodnoty parametrů hello není nutné toobe změnit.</span><span class="sxs-lookup"><span data-stu-id="4bac6-134">For this guide, hello parameter values do not need toobe changed.</span></span> <span data-ttu-id="4bac6-135">API Management a služby Správce prostředků infrastruktury šablony Hello používají tyto hodnoty, pokud jsou upraveny zde, je nutné upravit je do jiné šablony Resource Manageru hello odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="4bac6-135">hello API Management and Service Fabric Resource Manager templates use these values, so if they are modified here, you must modify them in hello other Resource Manager templates accordingly.</span></span> 

 1. <span data-ttu-id="4bac6-136">Stáhněte si následující soubor šablony a parametry Resource Manager hello:</span><span class="sxs-lookup"><span data-stu-id="4bac6-136">Download hello following Resource Manager template and parameters file:</span></span>

    - <span data-ttu-id="4bac6-137">[Network.JSON][network-arm]</span><span class="sxs-lookup"><span data-stu-id="4bac6-137">[network.json][network-arm]</span></span>
    - <span data-ttu-id="4bac6-138">[Network.Parameters.JSON][network-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="4bac6-138">[network.parameters.json][network-parameters-arm]</span></span>

 2. <span data-ttu-id="4bac6-139">Použijte následující prostředí PowerShell příkaz toodeploy hello Resource Manager šablony a parametr soubory pro nastavení sítě hello hello:</span><span class="sxs-lookup"><span data-stu-id="4bac6-139">Use hello following PowerShell command toodeploy hello Resource Manager template and parameter files for hello network setup:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\network.json -TemplateParameterFile .\network.parameters.json -Verbose
    ```

### <a name="deploy-hello-service-fabric-cluster"></a><span data-ttu-id="4bac6-140">Nasazení clusteru Service Fabric hello</span><span class="sxs-lookup"><span data-stu-id="4bac6-140">Deploy hello Service Fabric cluster</span></span>

<span data-ttu-id="4bac6-141">Po dokončení hello síťovým prostředkům nasazení, hello dalším krokem je toodeploy toohello clusteru Service Fabric virtuální sítě v podsíti hello a NSG určené pro cluster Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="4bac6-141">Once hello network resources have finished deploying, hello next step is toodeploy a Service Fabric cluster toohello VNET in hello subnet and NSG designated for hello Service Fabric cluster.</span></span> <span data-ttu-id="4bac6-142">V tomto kurzu Šablona služby Správce prostředků infrastruktury hello je předem nakonfigurovaný toouse hello názvy hello virtuální sítě, podsítě a NSG, které jste nastavili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="4bac6-142">For this tutorial, hello Service Fabric Resource Manager template is pre-configured toouse hello names of hello VNET, subnet, and NSG that you set up in hello previous step.</span></span> 

<span data-ttu-id="4bac6-143">šablony Resource Manageru clusteru Service Fabric Hello je nakonfigurované toocreate zabezpečení clusteru se certifikát zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="4bac6-143">hello Service Fabric cluster Resource Manager template is configured toocreate a secure cluster with certificate security.</span></span> <span data-ttu-id="4bac6-144">certifikát Hello je použité toosecure komunikaci mezi uzly pro cluster a cluster Service Fabric tooyour přístup uživatele toomanage.</span><span class="sxs-lookup"><span data-stu-id="4bac6-144">hello certificate is used toosecure node-to-node communication for your cluster and toomanage user access tooyour Service Fabric cluster.</span></span> <span data-ttu-id="4bac6-145">API Management používá tento certifikát tooaccess hello Service Fabric Naming Service pro zjišťování služby.</span><span class="sxs-lookup"><span data-stu-id="4bac6-145">API Management uses this certificate tooaccess  hello Service Fabric Naming Service for service discovery.</span></span>

<span data-ttu-id="4bac6-146">Tento krok vyžaduje použití certifikátu v Key Vault pro zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="4bac6-146">This step requires having a certificate in Key Vault for cluster security.</span></span> <span data-ttu-id="4bac6-147">Další informace o nastavení zabezpečení clusteru s Key Vault najdete v tématu [Tato příručka týkající se vytvoření clusteru v Azure pomocí Resource Manager](service-fabric-cluster-creation-via-arm.md)</span><span class="sxs-lookup"><span data-stu-id="4bac6-147">For more information on setting up a secure cluster with Key Vault, see [this guide on creating a cluster in Azure using Resource Manager](service-fabric-cluster-creation-via-arm.md)</span></span>

> [!NOTE]
> <span data-ttu-id="4bac6-148">Můžete přidat ověřování Azure Active Directory přidání toohello certifikát používaný pro přístup ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="4bac6-148">You may add Azure Active Directory authentication in addition toohello certificate used for cluster access.</span></span> <span data-ttu-id="4bac6-149">Azure Active Directory je hello doporučená způsob toomanage uživatel přístup tooyour cluster Service Fabric, ale je v tomto kurzu není nutné toocomplete.</span><span class="sxs-lookup"><span data-stu-id="4bac6-149">Azure Active Directory is hello recommended way toomanage user access tooyour Service Fabric cluster, but is not necessary toocomplete this tutorial.</span></span> <span data-ttu-id="4bac6-150">Certifikát je nutný v obou případech pro zabezpečení mezi uzly clusteru a pro ověřování Azure API Management, které aktuálně nepodporuje ověřování pomocí služby Azure Active Directory pro Service Fabric back-end.</span><span class="sxs-lookup"><span data-stu-id="4bac6-150">A certificate is required either way for cluster node-to-node security and for Azure API Management authentication, which currently does not support authenticating with Azure Active Directory for a Service Fabric backend.</span></span>

 1. <span data-ttu-id="4bac6-151">Stáhněte si následující soubor šablony a parametry Resource Manager hello:</span><span class="sxs-lookup"><span data-stu-id="4bac6-151">Download hello following Resource Manager template and parameters file:</span></span>
 
    - <span data-ttu-id="4bac6-152">[cluster.JSON][cluster-arm]</span><span class="sxs-lookup"><span data-stu-id="4bac6-152">[cluster.json][cluster-arm]</span></span>
    - <span data-ttu-id="4bac6-153">[cluster.Parameters.JSON][cluster-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="4bac6-153">[cluster.parameters.json][cluster-parameters-arm]</span></span>

 2. <span data-ttu-id="4bac6-154">Vyplňte hello prázdné parametry v hello `cluster.parameters.json` soubor pro vaše nasazení, včetně hello [Key Vault informace](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) pro váš cluster certifikát.</span><span class="sxs-lookup"><span data-stu-id="4bac6-154">Fill in hello empty parameters in hello `cluster.parameters.json` file for your deployment, including hello [Key Vault information](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) for your cluster certificate.</span></span>

 3. <span data-ttu-id="4bac6-155">Použijte následující prostředí PowerShell příkaz toodeploy hello Resource Manager šablony a parametr soubory toocreate hello cluster Service Fabric hello:</span><span class="sxs-lookup"><span data-stu-id="4bac6-155">Use hello following PowerShell command toodeploy hello Resource Manager template and parameter files toocreate hello Service Fabric cluster:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\cluster.json -TemplateParameterFile .\cluster.parameters.json -Verbose
    ```

### <a name="deploy-api-management"></a><span data-ttu-id="4bac6-156">Nasadit službu API Management</span><span class="sxs-lookup"><span data-stu-id="4bac6-156">Deploy API Management</span></span>

<span data-ttu-id="4bac6-157">Nakonec nasazení API Management toohello virtuální sítě v podsíti hello a NSG určená pro API Management.</span><span class="sxs-lookup"><span data-stu-id="4bac6-157">Finally, deploy API Management toohello VNET in hello subnet and NSG designated for API Management.</span></span> <span data-ttu-id="4bac6-158">Není nutné toowait pro toofinish nasazení clusteru Service Fabric hello před nasazením API Management.</span><span class="sxs-lookup"><span data-stu-id="4bac6-158">You do not need toowait for hello Service Fabric cluster deployment toofinish before deploying API Management.</span></span> 

<span data-ttu-id="4bac6-159">V tomto kurzu hello rozhraní API správy Resource Manager šablona je předem nakonfigurovaný toouse hello názvy hello virtuální sítě, podsítě a NSG, které jste nastavili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="4bac6-159">For this tutorial, hello API Management Resource Manager template is pre-configured toouse hello names of hello VNET, subnet, and NSG that you set up in hello previous step.</span></span> 

 1. <span data-ttu-id="4bac6-160">Stáhněte si následující soubor šablony a parametry Resource Manager hello:</span><span class="sxs-lookup"><span data-stu-id="4bac6-160">Download hello following Resource Manager template and parameters file:</span></span>
 
    - <span data-ttu-id="4bac6-161">[APIM.JSON][apim-arm]</span><span class="sxs-lookup"><span data-stu-id="4bac6-161">[apim.json][apim-arm]</span></span>
    - <span data-ttu-id="4bac6-162">[APIM.Parameters.JSON][apim-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="4bac6-162">[apim.parameters.json][apim-parameters-arm]</span></span>

 2. <span data-ttu-id="4bac6-163">Vyplňte hello prázdné parametry v hello `apim.parameters.json` pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="4bac6-163">Fill in hello empty parameters in hello `apim.parameters.json` for your deployment.</span></span>

 3. <span data-ttu-id="4bac6-164">Použijte následující prostředí PowerShell příkaz toodeploy hello Resource Manager šablony a parametr soubory pro API Management hello:</span><span class="sxs-lookup"><span data-stu-id="4bac6-164">Use hello following PowerShell command toodeploy hello Resource Manager template and parameter files for API Management:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
    ```

## <a name="configure-api-management"></a><span data-ttu-id="4bac6-165">Konfigurace správy rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4bac6-165">Configure API Management</span></span>

<span data-ttu-id="4bac6-166">Jakmile cluster API Management a Service Fabric se nasazuje, můžete nakonfigurovat back-end Service Fabric ve službě API Management.</span><span class="sxs-lookup"><span data-stu-id="4bac6-166">Once your API Management and Service Fabric cluster are deployed, you can configure a Service Fabric backend in API Management.</span></span> <span data-ttu-id="4bac6-167">To vám umožní toocreate zásadu služby back-end, která odešle cluster Service Fabric tooyour provoz.</span><span class="sxs-lookup"><span data-stu-id="4bac6-167">This allows you toocreate a backend service policy that sends traffic tooyour Service Fabric cluster.</span></span>

### <a name="api-management-security"></a><span data-ttu-id="4bac6-168">Zabezpečení rozhraní API Management</span><span class="sxs-lookup"><span data-stu-id="4bac6-168">API Management Security</span></span>

<span data-ttu-id="4bac6-169">tooconfigure hello Service Fabric back-end, musíte nejdřív nastavení zabezpečení tooconfigure API Management.</span><span class="sxs-lookup"><span data-stu-id="4bac6-169">tooconfigure hello Service Fabric backend, you first need tooconfigure API Management security settings.</span></span> <span data-ttu-id="4bac6-170">nastavení zabezpečení tooconfigure, přejděte tooyour rozhraní API správy služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4bac6-170">tooconfigure security settings, go tooyour API Management service in hello Azure portal.</span></span>

#### <a name="enable-hello-api-management-rest-api"></a><span data-ttu-id="4bac6-171">Povolit hello rozhraní API REST API pro správu</span><span class="sxs-lookup"><span data-stu-id="4bac6-171">Enable hello API Management REST API</span></span>

<span data-ttu-id="4bac6-172">Hello rozhraní API REST API pro správu je aktuálně hello pouze způsob tooconfigure back-end službu.</span><span class="sxs-lookup"><span data-stu-id="4bac6-172">hello API Management REST API is currently hello only way tooconfigure a backend service.</span></span> <span data-ttu-id="4bac6-173">prvním krokem Hello je tooenable hello rozhraní API REST API pro správu a zabezpečte ji.</span><span class="sxs-lookup"><span data-stu-id="4bac6-173">hello first step is tooenable hello API Management REST API and secure it.</span></span>

 1. <span data-ttu-id="4bac6-174">V hello služba API Management, vyberte **rozhraní API pro správu - PREVIEW** pod **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="4bac6-174">In hello API Management service, select **Management API - PREVIEW** under **Security**.</span></span>
 2. <span data-ttu-id="4bac6-175">Zkontrolujte hello **povolit rozhraní API REST API pro správu** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="4bac6-175">Check hello **Enable API Management REST API** checkbox.</span></span>
 3. <span data-ttu-id="4bac6-176">Poznámka: adresa URL rozhraní API správy hello – Toto je adresa URL hello použijeme novější tooset až hello Service Fabric back-end</span><span class="sxs-lookup"><span data-stu-id="4bac6-176">Note hello Management API URL - this is hello URL we'll use later tooset up hello Service Fabric backend</span></span>
 4. <span data-ttu-id="4bac6-177">Generovat **přístupový Token** výběrem datum vypršení platnosti a klíč klikněte hello **generování** tlačítko směrem k hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="4bac6-177">Generate an **access Token** by selecting an expiry date and a key, then click hello **Generate** button toward hello bottom of hello page.</span></span>
 5. <span data-ttu-id="4bac6-178">Kopírování hello **přístupový token** a uložte ho – použijeme ho v hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="4bac6-178">Copy hello **access token** and save it - we'll use this in hello following steps.</span></span> <span data-ttu-id="4bac6-179">Všimněte si, že se to neliší od hello primární klíč a sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="4bac6-179">Note this is different from hello primary key and secondary key.</span></span>

#### <a name="upload-a-service-fabric-client-certificate"></a><span data-ttu-id="4bac6-180">Nahrajte certifikát klienta Service Fabric</span><span class="sxs-lookup"><span data-stu-id="4bac6-180">Upload a Service Fabric client certificate</span></span>

<span data-ttu-id="4bac6-181">API Management musí ověřit k vašemu clusteru Service Fabric pro zjišťování služby pomocí klientského certifikátu, který má přístup tooyour clusteru.</span><span class="sxs-lookup"><span data-stu-id="4bac6-181">API Management must authenticate with your Service Fabric cluster for service discovery using a client certificate that has access tooyour cluster.</span></span> <span data-ttu-id="4bac6-182">Tento kurz používá pro jednoduchost, hello stejný certifikát zadaný při vytváření clusteru Service Fabric hello, který ve výchozím nastavení může být použité tooaccess vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="4bac6-182">For simplicity, this tutorial uses hello same certificate specified when creating hello Service Fabric cluster, which by default can be used tooaccess your cluster.</span></span>

 1. <span data-ttu-id="4bac6-183">V hello služba API Management, vyberte **klientské certifikáty - PREVIEW** pod **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="4bac6-183">In hello API Management service, select **Client certificates - PREVIEW** under **Security**.</span></span>
 2. <span data-ttu-id="4bac6-184">Klikněte na tlačítko hello **+ přidat** tlačítko</span><span class="sxs-lookup"><span data-stu-id="4bac6-184">Click hello **+ Add** button</span></span>
 2. <span data-ttu-id="4bac6-185">Vyberte hello soubor privátního klíče (.pfx) hello clusteru certifikátu, který jste zadali při vytváření clusteru Service Fabric, zadejte jeho název a zadejte heslo soukromého klíče hello.</span><span class="sxs-lookup"><span data-stu-id="4bac6-185">Select hello private key file (.pfx) of hello cluster certificate that you specified when creating your Service Fabric cluster, give it a name, and provide hello private key password.</span></span>

> [!NOTE]
> <span data-ttu-id="4bac6-186">Tento kurz používá hello stejný certifikát pro klienta ověřování a clusteru mezi uzly zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="4bac6-186">This tutorial uses hello same certificate for client authentication and cluster node-to-node security.</span></span> <span data-ttu-id="4bac6-187">Pokud máte jeden nakonfigurované tooaccess cluster Service Fabric můžete použít samostatný klientský certifikát.</span><span class="sxs-lookup"><span data-stu-id="4bac6-187">You may use a separate client certificate if you have one configured tooaccess your Service Fabric cluster.</span></span>

### <a name="configure-hello-backend"></a><span data-ttu-id="4bac6-188">Konfigurace back-end hello</span><span class="sxs-lookup"><span data-stu-id="4bac6-188">Configure hello backend</span></span>

<span data-ttu-id="4bac6-189">Teď, když je nakonfigurovaná zabezpečení rozhraní API Management, můžete nakonfigurovat hello Service Fabric back-end.</span><span class="sxs-lookup"><span data-stu-id="4bac6-189">Now that API Management security is configured, you can configure hello Service Fabric backend.</span></span> <span data-ttu-id="4bac6-190">Pro Service Fabric back-EndY je cluster Service Fabric hello hello back-end, nikoli konkrétní službu Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="4bac6-190">For Service Fabric backends, hello Service Fabric cluster is hello backend, rather than a specific Service Fabric service.</span></span> <span data-ttu-id="4bac6-191">To umožňuje jedna zásada tooroute toomore než jedna služba v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="4bac6-191">This allows a single policy tooroute toomore than one service in hello cluster.</span></span>

<span data-ttu-id="4bac6-192">Tento krok vyžaduje hello přístupový token, který jste vygenerovali dříve a hello kryptografický otisk certifikátu vašeho clusteru, že jste nahráli tooAPI správy v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="4bac6-192">This step requires hello access token you generated earlier and hello thumbprint for your cluster certificate you uploaded tooAPI Management in hello previous step.</span></span>

> [!NOTE]
> <span data-ttu-id="4bac6-193">Pokud samostatný klientský certifikát se používá v předchozím kroku hello pro API Management, musíte hello kryptografický otisk certifikátu klienta hello v přidání toohello clusteru kryptografický otisk certifikátu v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="4bac6-193">If you used a separate client certificate in hello previous step for API Management, you need hello thumbprint for hello client certificate in addition toohello cluster certificate thumbprint in this step.</span></span>

<span data-ttu-id="4bac6-194">Odešlete hello následující požadavek HTTP PUT toohello rozhraní API správy rozhraní API adresu URL jste si předtím poznamenali při povolování hello rozhraní API REST API správy tooconfigure hello Service Fabric back-end službu.</span><span class="sxs-lookup"><span data-stu-id="4bac6-194">Send hello following HTTP PUT request toohello API Management API URL you noted earlier when enabling hello API Management REST API tooconfigure hello Service Fabric backend service.</span></span> <span data-ttu-id="4bac6-195">Měli byste vidět `HTTP 201 Created` odpovědi při úspěšném provedení příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="4bac6-195">You should see an `HTTP 201 Created` response when hello command succeeds.</span></span> <span data-ttu-id="4bac6-196">Další informace o každé pole, najdete v části hello API Management [referenční dokumentace rozhraní API back-end](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).</span><span class="sxs-lookup"><span data-stu-id="4bac6-196">For more information on each field, see hello API Management [backend API reference documentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).</span></span>

<span data-ttu-id="4bac6-197">Příkaz HTTP a adresy URL:</span><span class="sxs-lookup"><span data-stu-id="4bac6-197">HTTP command and URL:</span></span>
```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
```

<span data-ttu-id="4bac6-198">Hlavičky požadavku:</span><span class="sxs-lookup"><span data-stu-id="4bac6-198">Request headers:</span></span>
```http
Authorization: SharedAccessSignature <your access token>
Content-Type: application/json
```

<span data-ttu-id="4bac6-199">Text žádosti:</span><span class="sxs-lookup"><span data-stu-id="4bac6-199">Request body:</span></span>
```http
{
    "description": "<description>",
    "url": "<fallback service name>",
    "protocol": "http",
    "resourceId": "<cluster HTTP management endpoint>",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": [ "<cluster HTTP management endpoint>" ],
            "clientCertificateThumbprint": "<client cert thumbprint>",
            "serverCertificateThumbprints": [ "<cluster cert thumbprint>" ],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

<span data-ttu-id="4bac6-200">Hello **url** parametr tady je služba plně kvalifikovaný název služby v clusteru, jsou všechny požadavky směrovány tooby výchozí, pokud není zadán žádný název služby v zásadách back-end.</span><span class="sxs-lookup"><span data-stu-id="4bac6-200">hello **url** parameter here is a fully-qualified service name of a service in your cluster that all requests are routed tooby default if no service name is specified in a backend policy.</span></span> <span data-ttu-id="4bac6-201">Můžete použít název falešných služby, jako například "fabric: / falešných/služby" Pokud nemáte v úmyslu toohave záložní služby.</span><span class="sxs-lookup"><span data-stu-id="4bac6-201">You may use a fake service name, such as "fabric:/fake/service" if you do not intend toohave a fallback service.</span></span>

<span data-ttu-id="4bac6-202">Odkazovat toohello API Management [referenční dokumentace rozhraní API back-end](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) další podrobnosti o každé pole.</span><span class="sxs-lookup"><span data-stu-id="4bac6-202">Refer toohello API Management [backend API reference documentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) for more details on each field.</span></span>

#### <a name="example"></a><span data-ttu-id="4bac6-203">Příklad</span><span class="sxs-lookup"><span data-stu-id="4bac6-203">Example</span></span>

```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
Authorization: SharedAccessSignature 230948023984&Ld93cRGcNU6KZ4uVz7JlfTec4eX43Q9Nu8ndatOgBzs6+f559Pkf3iHX2cSge+r42pn35qGY3TitjrIl13hwcQ==
Content-Type: application/json

{
    "description": "My Service Fabric backend",
    "url": "fabric:/myapp/myservice",
    "protocol": "http",
    "resourceId": "https://your-cluster.westus.cloudapp.azure.com:19080",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": ["https://your-cluster.westus.cloudapp.azure.com:19080"],
            "clientCertificateThumbprint": "57bc463aba3aea3a12a18f36f44154f819f0fe32",
            "serverCertificateThumbprints": ["57bc463aba3aea3a12a18f36f44154f819f0fe32"],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

## <a name="deploy-a-service-fabric-back-end-service"></a><span data-ttu-id="4bac6-204">Nasazení služby back-end Service Fabric</span><span class="sxs-lookup"><span data-stu-id="4bac6-204">Deploy a Service Fabric back-end service</span></span>

<span data-ttu-id="4bac6-205">Teď, když máte hello Service Fabric nakonfigurovat jako back-end tooAPI správy, můžete vytvořit zásady back-end pro vaše rozhraní API, která odesílat provoz služby tooyour Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="4bac6-205">Now that you have hello Service Fabric configured as a backend tooAPI Management, you can author backend policies for your APIs that send traffic tooyour Service Fabric services.</span></span> <span data-ttu-id="4bac6-206">Ale nejdřív je potřeba služby spuštěné v Service Fabric tooaccept požadavky.</span><span class="sxs-lookup"><span data-stu-id="4bac6-206">But first you need a service running in Service Fabric tooaccept requests.</span></span>

### <a name="create-a-service-fabric-service-with-an-http-endpoint"></a><span data-ttu-id="4bac6-207">Vytvoření služby Service Fabric se koncový bod HTTP</span><span class="sxs-lookup"><span data-stu-id="4bac6-207">Create a Service Fabric service with an HTTP endpoint</span></span>

<span data-ttu-id="4bac6-208">V tomto kurzu vytvoříme základní bezstavové ASP.NET Core spolehlivé služby pomocí hello výchozí šablona projektu webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4bac6-208">For this tutorial, we'll create a basic stateless ASP.NET Core Reliable Service using hello default Web API project template.</span></span> <span data-ttu-id="4bac6-209">Tím se vytvoří koncový bod HTTP pro vaši službu, která budete vystavit pomocí Azure API Management:</span><span class="sxs-lookup"><span data-stu-id="4bac6-209">This creates an HTTP endpoint for your service, which you'll expose through Azure API Management:</span></span>

```
/api/values
```

<span data-ttu-id="4bac6-210">Začněte tím, že [nastavení vývojového prostředí pro vývoj ASP.NET Core](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).</span><span class="sxs-lookup"><span data-stu-id="4bac6-210">Start by [setting up your development environment for ASP.NET Core development](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).</span></span>

<span data-ttu-id="4bac6-211">Až nastavíte vývojového prostředí Visual Studio spustíte jako správce a vytvoření služby ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="4bac6-211">Once your development environment is set up, start Visual Studio as Administrator and create an ASP.NET Core service:</span></span>

 1. <span data-ttu-id="4bac6-212">V sadě Visual Studio vyberte soubor -> Nový projekt.</span><span class="sxs-lookup"><span data-stu-id="4bac6-212">In Visual Studio, select File -> New Project.</span></span>
 2. <span data-ttu-id="4bac6-213">Vyberte šablonu hello aplikace Service Fabric v cloudu a pojmenujte ji **"ApiApplication"**.</span><span class="sxs-lookup"><span data-stu-id="4bac6-213">Select hello Service Fabric Application template under Cloud and name it **"ApiApplication"**.</span></span>
 3. <span data-ttu-id="4bac6-214">Vyberte hello ASP.NET Core šablony služby a název projektu hello **"WebApiService"**.</span><span class="sxs-lookup"><span data-stu-id="4bac6-214">Select hello ASP.NET Core service template and name hello project **"WebApiService"**.</span></span>
 4. <span data-ttu-id="4bac6-215">Výběr šablony projektu hello webové rozhraní API ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="4bac6-215">Select hello Web API ASP.NET Core 1.1 project template.</span></span>
 5. <span data-ttu-id="4bac6-216">Po vytvoření projektu hello otevřete `PackageRoot\ServiceManifest.xml` a odeberte hello `Port` atribut z hello koncový bod prostředků konfigurace:</span><span class="sxs-lookup"><span data-stu-id="4bac6-216">Once hello project is created, open `PackageRoot\ServiceManifest.xml` and remove hello `Port` attribute from hello endpoint resource configuration:</span></span>
 
    ```xml
    <Resources>
      <Endpoints>
        <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" />
      </Endpoints>
    </Resources>
    ```

    <span data-ttu-id="4bac6-217">To umožňuje Service Fabric toospecify port dynamicky z rozsahu portů aplikace hello, který jsme otevřít prostřednictvím hello skupinu zabezpečení sítě v modulu snap-in Šablony Resource Manageru clusteru hello povolení tooit tooflow provoz z API Management.</span><span class="sxs-lookup"><span data-stu-id="4bac6-217">This allows Service Fabric toospecify a port dynamically from hello application port range, which we opened through hello Network Security Group in hello cluster Resource Manager template, allowing traffic tooflow tooit from API Management.</span></span>
 
 6. <span data-ttu-id="4bac6-218">Stisknutím klávesy F5 ve Visual Studio tooverify hello webového rozhraní API není k dispozici místně.</span><span class="sxs-lookup"><span data-stu-id="4bac6-218">Press F5 in Visual Studio tooverify hello web API is available locally.</span></span> 

    <span data-ttu-id="4bac6-219">Otevřete Service Fabric Explorer a rozbalení tooa konkrétní instanci hello ASP.NET Core toosee hello základní adresa hello služby naslouchá na.</span><span class="sxs-lookup"><span data-stu-id="4bac6-219">Open Service Fabric Explorer and drill down tooa specific instance of hello ASP.NET Core service toosee hello base address hello service is listening on.</span></span> <span data-ttu-id="4bac6-220">Přidat `/api/values` toohello základní adresa a otevřete v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="4bac6-220">Add `/api/values` toohello base address and open it in a browser.</span></span> <span data-ttu-id="4bac6-221">Tím se spustí hello metodu Get na hello ValuesController v šabloně hello webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4bac6-221">This invokes hello Get method on hello ValuesController in hello Web API template.</span></span> <span data-ttu-id="4bac6-222">Vrátí hello výchozí odpovědi, které poskytuje hello šablony, pole JSON, který obsahuje dva řetězce:</span><span class="sxs-lookup"><span data-stu-id="4bac6-222">It returns hello default response that is provided by hello template, a JSON array that contains two strings:</span></span>

    ```json
    ["value1", "value2"]`
    ```

    <span data-ttu-id="4bac6-223">Toto je hello koncový bod, který budete vystavit přes správu rozhraní API v Azure.</span><span class="sxs-lookup"><span data-stu-id="4bac6-223">This is hello endpoint that you'll expose through API Management in Azure.</span></span>

 7. <span data-ttu-id="4bac6-224">Nakonec nasazení clusteru tooyour hello aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="4bac6-224">Finally, deploy hello application tooyour cluster in Azure.</span></span> <span data-ttu-id="4bac6-225">[Pomocí sady Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), klikněte pravým tlačítkem na projekt aplikace hello a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="4bac6-225">[Using Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), right-click hello Application project and select **Publish**.</span></span> <span data-ttu-id="4bac6-226">Zadejte svůj koncový bod clusteru (například `mycluster.westus.cloudapp.azure.com:19000`) toodeploy hello aplikace tooyour Service Fabric cluster v Azure.</span><span class="sxs-lookup"><span data-stu-id="4bac6-226">Provide your cluster endpoint (for example, `mycluster.westus.cloudapp.azure.com:19000`) toodeploy hello application tooyour Service Fabric cluster in Azure.</span></span>

<span data-ttu-id="4bac6-227">Bezstavové služby ASP.NET Core s názvem `fabric:/ApiApplication/WebApiService` teď by měl být spuštěn v clusteru Service Fabric v Azure.</span><span class="sxs-lookup"><span data-stu-id="4bac6-227">An ASP.NET Core stateless service named `fabric:/ApiApplication/WebApiService` should now be running in your Service Fabric cluster in Azure.</span></span>

## <a name="create-an-api-operation"></a><span data-ttu-id="4bac6-228">Vytvoření operace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4bac6-228">Create an API operation</span></span>

<span data-ttu-id="4bac6-229">Teď máme připraven toocreate operace ve službě API Management hello toocommunicate použití tohoto externích klientů s běžící v clusteru Service Fabric hello bezstavové služby ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4bac6-229">Now we're ready toocreate an operation in API Management that external clients use toocommunicate with hello ASP.NET Core stateless service running in hello Service Fabric cluster.</span></span>

 1. <span data-ttu-id="4bac6-230">Přihlaste se toohello portál Azure a přejděte tooyour nasazení služby API Management.</span><span class="sxs-lookup"><span data-stu-id="4bac6-230">Log in toohello Azure portal and navigate tooyour API Management service deployment.</span></span>
 2. <span data-ttu-id="4bac6-231">V okně služby API Management hello vyberte **rozhraní API – náhled**</span><span class="sxs-lookup"><span data-stu-id="4bac6-231">In hello API Management service blade, select **APIs - Preview**</span></span>
 3. <span data-ttu-id="4bac6-232">Přidání nového rozhraní API kliknutím hello **prázdné API** pole a vyplníte dialogové okno hello:</span><span class="sxs-lookup"><span data-stu-id="4bac6-232">Add a new API by clicking hello **Blank API** box and filling out hello dialog box:</span></span>

     - <span data-ttu-id="4bac6-233">**Adresu URL webové služby**: pro Service Fabric back-EndY, není tato hodnota adresy URL používá.</span><span class="sxs-lookup"><span data-stu-id="4bac6-233">**Web service URL**: For Service Fabric backends, this URL value is not used.</span></span> <span data-ttu-id="4bac6-234">Zde můžete zadejte libovolnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4bac6-234">You can put any value here.</span></span> <span data-ttu-id="4bac6-235">V tomto kurzu použít: `http://servicefabric`.</span><span class="sxs-lookup"><span data-stu-id="4bac6-235">For this tutorial, use: `http://servicefabric`.</span></span>
     - <span data-ttu-id="4bac6-236">**Název**: zadat libovolný název pro rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4bac6-236">**Name**: Provide any name for your API.</span></span> <span data-ttu-id="4bac6-237">V tomto kurzu použít `Service Fabric App`.</span><span class="sxs-lookup"><span data-stu-id="4bac6-237">For this tutorial, use `Service Fabric App`.</span></span>
     - <span data-ttu-id="4bac6-238">**Schéma adresy URL**: Vyberte HTTP, HTTPS nebo obě možnosti.</span><span class="sxs-lookup"><span data-stu-id="4bac6-238">**URL scheme**: Select either HTTP, HTTPS, or both.</span></span> <span data-ttu-id="4bac6-239">V tomto kurzu použít `both`.</span><span class="sxs-lookup"><span data-stu-id="4bac6-239">For this tutorial, use `both`.</span></span>
     - <span data-ttu-id="4bac6-240">**Přípona adresy URL rozhraní API**: Zadejte příponu pro naše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4bac6-240">**API URL Suffix**: Provide a suffix for our API.</span></span> <span data-ttu-id="4bac6-241">V tomto kurzu použít `myapp`.</span><span class="sxs-lookup"><span data-stu-id="4bac6-241">For this tutorial, use `myapp`.</span></span>
 
 4. <span data-ttu-id="4bac6-242">Po vytvoření hello rozhraní API klikněte na tlačítko **+ operace přidání** operace tooadd front-endové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4bac6-242">Once hello API is created, click **+ Add operation** tooadd a front-end API operation.</span></span> <span data-ttu-id="4bac6-243">Vyplňte hodnoty hello:</span><span class="sxs-lookup"><span data-stu-id="4bac6-243">Fill out hello values:</span></span>
    
     - <span data-ttu-id="4bac6-244">**Adresa URL**: vyberte `GET` a zadejte cestu adresy URL pro hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4bac6-244">**URL**: Select `GET` and provide a URL path for hello API.</span></span> <span data-ttu-id="4bac6-245">V tomto kurzu použít `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="4bac6-245">For this tutorial, use `/api/values`.</span></span>
     
       <span data-ttu-id="4bac6-246">Ve výchozím nastavení zadat cestu adresy URL hello tady se cesty URL hello odesílají toohello back-end Service Fabric službu.</span><span class="sxs-lookup"><span data-stu-id="4bac6-246">By default, hello URL path specified here is hello URL path sent toohello backend Service Fabric service.</span></span> <span data-ttu-id="4bac6-247">Pokud používáte hello stejné cesty URL, zde kterou používá službu, v takovém případě `/api/values`, pak operaci hello funguje bez další úpravy.</span><span class="sxs-lookup"><span data-stu-id="4bac6-247">If you use hello same URL path here that your service uses, in this case `/api/values`, then hello operation works without further modification.</span></span> <span data-ttu-id="4bac6-248">Můžete také určit cestu adresy URL tady, se liší od cesty URL hello používá váš back-end službu Service Fabric, v takovém případě budete také toospecify nutnost přepisu cestu v zásadě operaci později.</span><span class="sxs-lookup"><span data-stu-id="4bac6-248">You may also specify a URL path here that is different from hello URL path used by your backend Service Fabric service, in which case you will also need toospecify a path rewrite in your operation policy later.</span></span>
     - <span data-ttu-id="4bac6-249">**Zobrazovaný název**: zadat libovolný název pro rozhraní API hello.</span><span class="sxs-lookup"><span data-stu-id="4bac6-249">**Display name**: Provide any name for hello API.</span></span> <span data-ttu-id="4bac6-250">V tomto kurzu použít `Values`.</span><span class="sxs-lookup"><span data-stu-id="4bac6-250">For this tutorial, use `Values`.</span></span>

## <a name="configure-a-backend-policy"></a><span data-ttu-id="4bac6-251">Nakonfigurujte zásady back-end</span><span class="sxs-lookup"><span data-stu-id="4bac6-251">Configure a backend policy</span></span>

<span data-ttu-id="4bac6-252">zásady back-end Hello sváže všechno, co společně.</span><span class="sxs-lookup"><span data-stu-id="4bac6-252">hello backend policy ties everything together.</span></span> <span data-ttu-id="4bac6-253">Toto je, kde konfigurujete hello back-end Service Fabric, které směrují požadavky toowhich služby.</span><span class="sxs-lookup"><span data-stu-id="4bac6-253">This is where you configure hello backend Service Fabric service toowhich requests are routed.</span></span> <span data-ttu-id="4bac6-254">Můžete provést tuto operaci zásad tooany rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4bac6-254">You can apply this policy tooany API operation.</span></span> <span data-ttu-id="4bac6-255">Hello [konfiguraci back-end pro Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) poskytuje následujících hello požadavku směrování ovládací prvky:</span><span class="sxs-lookup"><span data-stu-id="4bac6-255">hello [backend configuration for Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) provides hello following request routing controls:</span></span> 
 - <span data-ttu-id="4bac6-256">Služba instance výběru zadáním Service Fabric název instance služby, buď pevně zakódované (například `"fabric:/myapp/myservice"`) nebo vygenerovat z požadavku hello HTTP (například `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).</span><span class="sxs-lookup"><span data-stu-id="4bac6-256">Service instance selection by specifying a Service Fabric service instance name, either hardcoded (for example, `"fabric:/myapp/myservice"`) or generated from hello HTTP request (for example, `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).</span></span>
 - <span data-ttu-id="4bac6-257">Oddíl rozlišení vygenerováním klíč oddílu pomocí žádné schéma rozdělení oddílů Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="4bac6-257">Partition resolution by generating a partition key using any Service Fabric partitioning scheme.</span></span>
 - <span data-ttu-id="4bac6-258">Výběr repliky pro stavové služby.</span><span class="sxs-lookup"><span data-stu-id="4bac6-258">Replica selection for stateful services.</span></span>
 - <span data-ttu-id="4bac6-259">Řešení opakujte podmínky, které vám umožňují toospecify hello podmínky pro přeložit umístění služby a odešlete žádost.</span><span class="sxs-lookup"><span data-stu-id="4bac6-259">Resolution retry conditions that allow you toospecify hello conditions for re-resolving a service location and resending a request.</span></span>

<span data-ttu-id="4bac6-260">V tomto kurzu vytvořte zásadu back-end trasy požadavky přímo toohello ASP.NET Core bezstavové služby dříve nasazené:</span><span class="sxs-lookup"><span data-stu-id="4bac6-260">For this tutorial, create a backend policy that routes requests directly toohello ASP.NET Core stateless service deployed earlier:</span></span>

 1. <span data-ttu-id="4bac6-261">Vyberte a upravit hello **příchozí zásady** pro hello `Values` operaci kliknutím na ikonu pro úpravu hello, a potom výběrem **zobrazení kódu**.</span><span class="sxs-lookup"><span data-stu-id="4bac6-261">Select and edit hello **inbound policies** for hello `Values` operation by clicking hello edit icon, and then selecting **Code View**.</span></span>
 2. <span data-ttu-id="4bac6-262">V editoru kódu hello zásady, přidejte `set-backend-service` zásad v části příchozí zásady, jak je vidět tady a klikněte na tlačítko hello **Uložit** tlačítko:</span><span class="sxs-lookup"><span data-stu-id="4bac6-262">In hello policy code editor, add a `set-backend-service` policy under inbound policies as shown here and click hello **Save** button:</span></span>
    
    ```xml
    <policies>
      <inbound>
        <base/>
        <set-backend-service 
           backend-id="servicefabric"
           sf-service-instance-name="fabric:/ApiApplication/WebApiService"
           sf-resolve-condition="@((int)context.Response.StatusCode != 200)" />
      </inbound>
      <backend>
        <base/>
      </backend>
      <outbound>
        <base/>
      </outbound>
    </policies>
    ```

<span data-ttu-id="4bac6-263">Úplnou sadu atributů Service Fabric back-end zásady, najdete v části toohello [dokumentace ke službě back endové rozhraní API Management](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)</span><span class="sxs-lookup"><span data-stu-id="4bac6-263">For a full set of Service Fabric back-end policy attributes, refer toohello [API Management back-end documentation](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)</span></span>

### <a name="add-hello-api-tooa-product"></a><span data-ttu-id="4bac6-264">Přidání produktu tooa hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4bac6-264">Add hello API tooa product.</span></span> 

<span data-ttu-id="4bac6-265">Než bude možné volat rozhraní API hello, musí být přidané tooa produktu, kde můžete udělit přístup toousers.</span><span class="sxs-lookup"><span data-stu-id="4bac6-265">Before you can call hello API, it must be added tooa product where you can grant access toousers.</span></span> 

 1. <span data-ttu-id="4bac6-266">V hello služba API Management, vyberte **produkty - PREVIEW**.</span><span class="sxs-lookup"><span data-stu-id="4bac6-266">In hello API Management service, select **Products - PREVIEW**.</span></span>
 2. <span data-ttu-id="4bac6-267">Ve výchozím nastavení, produkty dva poskytovatelé pro správu rozhraní API: úvodní a bez omezení.</span><span class="sxs-lookup"><span data-stu-id="4bac6-267">By default, API Management providers two products: Starter and Unlimited.</span></span> <span data-ttu-id="4bac6-268">Vyberte hello neomezená produktu.</span><span class="sxs-lookup"><span data-stu-id="4bac6-268">Select hello Unlimited product.</span></span>
 3. <span data-ttu-id="4bac6-269">Vyberte rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4bac6-269">Select APIs.</span></span>
 4. <span data-ttu-id="4bac6-270">Klikněte na tlačítko hello **+ přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4bac6-270">Click hello **+ Add** button.</span></span>
 5. <span data-ttu-id="4bac6-271">Vyberte hello `Service Fabric App` rozhraní API, které jste vytvořili v předchozích krocích hello a klikněte na tlačítko hello **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4bac6-271">Select hello `Service Fabric App` API you created in hello previous steps and click hello **Select** button.</span></span>

### <a name="test-it"></a><span data-ttu-id="4bac6-272">otestovat</span><span class="sxs-lookup"><span data-stu-id="4bac6-272">Test it</span></span>

<span data-ttu-id="4bac6-273">Nyní můžete zkusit odesílání požadavku tooyour back-end služby v Service Fabric pomocí rozhraní API správy přímo z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4bac6-273">You can now try sending a request tooyour back-end service in Service Fabric through API Management directly from hello Azure portal.</span></span>

 1. <span data-ttu-id="4bac6-274">V hello služba API Management, vyberte **API – PREVIEW**.</span><span class="sxs-lookup"><span data-stu-id="4bac6-274">In hello API Management service, select **API - PREVIEW**.</span></span>
 2. <span data-ttu-id="4bac6-275">V hello `Service Fabric App` rozhraní API, které jste vytvořili v předchozích krocích hello vyberte hello **Test** kartě.</span><span class="sxs-lookup"><span data-stu-id="4bac6-275">In hello `Service Fabric App` API you created in hello previous steps, select hello **Test** tab.</span></span>
 3. <span data-ttu-id="4bac6-276">Klikněte na tlačítko hello **odeslat** toosend tlačítko test požadavek toohello back-end službu.</span><span class="sxs-lookup"><span data-stu-id="4bac6-276">Click hello **Send** button toosend a test request toohello backend service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4bac6-277">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4bac6-277">Next steps</span></span>

<span data-ttu-id="4bac6-278">V tomto okamžiku byste měli mít základní instalaci pomocí Service Fabric a API Management.</span><span class="sxs-lookup"><span data-stu-id="4bac6-278">At this point, you should have a basic setup with Service Fabric and API Management.</span></span>

<span data-ttu-id="4bac6-279">Tento kurz používá ověřování basic uživatele na základě certifikátů pro váš tooget clusteru Service Fabric, kterou vytvoříte rychle.</span><span class="sxs-lookup"><span data-stu-id="4bac6-279">This tutorial uses basic certificate-based user authentication for your Service Fabric cluster tooget you set up quickly.</span></span> <span data-ttu-id="4bac6-280">Pokročilejší ověřování uživatelů pro váš cluster Service Fabric je vhodnější s [ověřování Azure Active Directory](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication).</span><span class="sxs-lookup"><span data-stu-id="4bac6-280">More advanced user authentication for your Service Fabric cluster is preferable with [Azure Active Directory authentication](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication).</span></span> 

<span data-ttu-id="4bac6-281">Dále [vytvoření a konfigurace pokročilých nastavení produktu v Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) tooprepare aplikace pro provoz skutečném světě.</span><span class="sxs-lookup"><span data-stu-id="4bac6-281">Next, [create and configure advanced product settings in Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) tooprepare your application for real world traffic.</span></span>

<!-- links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[apim-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.json
[apim-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json


<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-api-management-quickstart/sf-apim-topology-overview.png
