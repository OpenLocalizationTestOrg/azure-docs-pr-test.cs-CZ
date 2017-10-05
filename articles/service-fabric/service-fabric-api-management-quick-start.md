---
title: "Azure Service Fabric s úvodní API Management | Microsoft Docs"
description: "Tento průvodce vám ukáže, jak rychle začít s Azure API Management a Service Fabric."
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
ms.openlocfilehash: e9f44d8a43d274768f43261fea68f0da9c681ae1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="service-fabric-with-azure-api-management-quick-start"></a><span data-ttu-id="1e7f4-103">Service Fabric s Azure API Management rychlý Start</span><span class="sxs-lookup"><span data-stu-id="1e7f4-103">Service Fabric with Azure API Management Quick Start</span></span>

<span data-ttu-id="1e7f4-104">Tento průvodce vám ukáže postup nastavení Azure API Management s Service Fabric a konfigurace vašeho prvního rozhraní API operaci odesílat provoz do back endové služby v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-104">This guide shows you how to set up Azure API Management with Service Fabric and configure your first API operation to send traffic to back-end services in Service Fabric.</span></span> <span data-ttu-id="1e7f4-105">Další informace o scénářích Azure API Management s Service Fabric najdete v tématu [přehled](service-fabric-api-management-overview.md) článku.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-105">To learn more about Azure API Management scenarios with Service Fabric, see the [overview](service-fabric-api-management-overview.md) article.</span></span> 

## <a name="deploy-api-management-and-service-fabric-to-azure"></a><span data-ttu-id="1e7f4-106">Nasazení do Azure API Management a Service Fabric</span><span class="sxs-lookup"><span data-stu-id="1e7f4-106">Deploy API Management and Service Fabric to Azure</span></span>

<span data-ttu-id="1e7f4-107">Prvním krokem je k nasazení API Management a cluster Service Fabric na Azure ve sdílené virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-107">The first step is to deploy API Management and a Service Fabric cluster to Azure in a shared Virtual Network.</span></span> <span data-ttu-id="1e7f4-108">To umožňuje správu rozhraní API a komunikovat přímo s Service Fabric, aby mohl vykonávat zjišťování služby, řešení oddílu služby a předat dál provoz přímo do jakékoli back-end službu v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-108">This allows API Management to communicate directly with Service Fabric so it can perform service discovery, service partition resolution, and forward traffic directly to any backend service in Service Fabric.</span></span>

### <a name="topology"></a><span data-ttu-id="1e7f4-109">topologie</span><span class="sxs-lookup"><span data-stu-id="1e7f4-109">Topology</span></span>

<span data-ttu-id="1e7f4-110">Tento průvodce nasadí do Azure, ve které jsou v podsítě ve stejné virtuální síti API Management a Service Fabric následující topologie:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-110">This guide deploys the following topology to Azure in which API Management and Service Fabric are in subnets of the same Virtual Network:</span></span>

 ![Popisek obrázku][sf-apim-topology-overview]

<span data-ttu-id="1e7f4-112">Abyste mohli rychle začít, šablony správce prostředků slouží pro každého kroku nasazení:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-112">To get started quickly, Resource Manager templates are provided for each deployment step:</span></span>

 - <span data-ttu-id="1e7f4-113">Topologie sítě:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-113">Network topology:</span></span>
    - <span data-ttu-id="1e7f4-114">[Network.JSON][network-arm]</span><span class="sxs-lookup"><span data-stu-id="1e7f4-114">[network.json][network-arm]</span></span>
    - <span data-ttu-id="1e7f4-115">[Network.Parameters.JSON][network-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="1e7f4-115">[network.parameters.json][network-parameters-arm]</span></span>
 - <span data-ttu-id="1e7f4-116">Cluster Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-116">Service Fabric cluster:</span></span>
    - <span data-ttu-id="1e7f4-117">[cluster.JSON][cluster-arm]</span><span class="sxs-lookup"><span data-stu-id="1e7f4-117">[cluster.json][cluster-arm]</span></span>
    - <span data-ttu-id="1e7f4-118">[cluster.Parameters.JSON][cluster-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="1e7f4-118">[cluster.parameters.json][cluster-parameters-arm]</span></span>
 - <span data-ttu-id="1e7f4-119">API Management:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-119">API Management:</span></span>
    - <span data-ttu-id="1e7f4-120">[APIM.JSON][apim-arm]</span><span class="sxs-lookup"><span data-stu-id="1e7f4-120">[apim.json][apim-arm]</span></span>
    - <span data-ttu-id="1e7f4-121">[APIM.Parameters.JSON][apim-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="1e7f4-121">[apim.parameters.json][apim-parameters-arm]</span></span>

### <a name="sign-in-to-azure-and-select-your-subscription"></a><span data-ttu-id="1e7f4-122">Přihlaste se k Azure a vybrat své předplatné</span><span class="sxs-lookup"><span data-stu-id="1e7f4-122">Sign in to Azure and select your subscription</span></span>

<span data-ttu-id="1e7f4-123">Tato příručka používá [prostředí Azure PowerShell][azure-powershell].</span><span class="sxs-lookup"><span data-stu-id="1e7f4-123">This guide uses [Azure PowerShell][azure-powershell].</span></span> <span data-ttu-id="1e7f4-124">Když spustíte novou relaci prostředí PowerShell, přihlaste se k účtu Azure a vybrat své předplatné před spuštěním příkazů Azure.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-124">When you start a new PowerShell session, sign in to your Azure account and select your subscription before you execute Azure commands.</span></span>
 
<span data-ttu-id="1e7f4-125">Přihlaste se k účtu Azure:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-125">Sign in to your Azure account:</span></span>

```powershell
PS > Login-AzureRmAccount
```

<span data-ttu-id="1e7f4-126">Vyberte předplatné:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-126">Select your subscription:</span></span>

```powershell
PS > Get-AzureRmSubscription
PS > Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="create-a-resource-group"></a><span data-ttu-id="1e7f4-127">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="1e7f4-127">Create a resource group</span></span>

<span data-ttu-id="1e7f4-128">Vytvořte novou skupinu prostředků pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-128">Create a new resource group for your deployment.</span></span> <span data-ttu-id="1e7f4-129">Poskytněte název a umístění.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-129">Give it a name and a location.</span></span>

```powershell
PS > New-AzureRmResourceGroup -Name <my-resource-group> -Location westus
```

### <a name="deploy-the-network-topology"></a><span data-ttu-id="1e7f4-130">Nasazení síťové topologie</span><span class="sxs-lookup"><span data-stu-id="1e7f4-130">Deploy the network topology</span></span>

<span data-ttu-id="1e7f4-131">Prvním krokem je nastavit topologie sítě, do které bude nasazen API Management a cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-131">The first step is to set up the network topology to which API Management and the Service Fabric cluster will be deployed.</span></span> <span data-ttu-id="1e7f4-132">[Network.json] [ network-arm] šablony Resource Manageru nastaven tak, aby vytvořit virtuální síť (VNET) se dvěma podsítěmi a skupiny zabezpečení dvě sítě (NSG).</span><span class="sxs-lookup"><span data-stu-id="1e7f4-132">The [network.json][network-arm] Resource Manager template is configured to create a Virtual Network (VNET) with two subnets and two Network Security Groups (NSG).</span></span> 

<span data-ttu-id="1e7f4-133">[Network.parameters.json] [ network-parameters-arm] soubor parametrů obsahuje názvy podsítí a skupin Nsg, které API Management a Service Fabric se nasadí do.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-133">The [network.parameters.json][network-parameters-arm] parameters file contains the names of the subnets and NSGs that API Management and Service Fabric will be deployed to.</span></span> <span data-ttu-id="1e7f4-134">Tato příručka hodnoty parametru není nutné změnit.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-134">For this guide, the parameter values do not need to be changed.</span></span> <span data-ttu-id="1e7f4-135">Použití šablony API Management a služby Správce prostředků infrastruktury tyto hodnoty, takže pokud jsou upraveny zde, je třeba je změnit v jiných šablonách Resource Manageru odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-135">The API Management and Service Fabric Resource Manager templates use these values, so if they are modified here, you must modify them in the other Resource Manager templates accordingly.</span></span> 

 1. <span data-ttu-id="1e7f4-136">Stáhněte si následující soubor šablony a parametry Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-136">Download the following Resource Manager template and parameters file:</span></span>

    - <span data-ttu-id="1e7f4-137">[Network.JSON][network-arm]</span><span class="sxs-lookup"><span data-stu-id="1e7f4-137">[network.json][network-arm]</span></span>
    - <span data-ttu-id="1e7f4-138">[Network.Parameters.JSON][network-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="1e7f4-138">[network.parameters.json][network-parameters-arm]</span></span>

 2. <span data-ttu-id="1e7f4-139">Použijte následující příkaz prostředí PowerShell pro nasazení Resource Manager soubory šablony a parametr pro nastavení sítě:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-139">Use the following PowerShell command to deploy the Resource Manager template and parameter files for the network setup:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\network.json -TemplateParameterFile .\network.parameters.json -Verbose
    ```

### <a name="deploy-the-service-fabric-cluster"></a><span data-ttu-id="1e7f4-140">Nasazení clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="1e7f4-140">Deploy the Service Fabric cluster</span></span>

<span data-ttu-id="1e7f4-141">Po dokončení síťové prostředky nasazení, dalším krokem je k nasazení clusteru Service Fabric v podsíti virtuální sítě a NSG určené pro cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-141">Once the network resources have finished deploying, the next step is to deploy a Service Fabric cluster to the VNET in the subnet and NSG designated for the Service Fabric cluster.</span></span> <span data-ttu-id="1e7f4-142">V tomto kurzu je předem nakonfigurovaný k použití názvy virtuální sítě, podsítě a NSG, které jste nastavili v předchozím kroku šablony služby Správce prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-142">For this tutorial, the Service Fabric Resource Manager template is pre-configured to use the names of the VNET, subnet, and NSG that you set up in the previous step.</span></span> 

<span data-ttu-id="1e7f4-143">Šablony Resource Manageru clusteru Service Fabric je nakonfigurován k vytvoření clusteru s podporou zabezpečení s certifikát zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-143">The Service Fabric cluster Resource Manager template is configured to create a secure cluster with certificate security.</span></span> <span data-ttu-id="1e7f4-144">Certifikát se používá k zabezpečení komunikace mezi uzly clusteru a ke správě přístupu uživatelů ke svému clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-144">The certificate is used to secure node-to-node communication for your cluster and to manage user access to your Service Fabric cluster.</span></span> <span data-ttu-id="1e7f4-145">API Management používá tento certifikát pro přístup ke službě názvy prostředků infrastruktury služby pro zjišťování služby.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-145">API Management uses this certificate to access  the Service Fabric Naming Service for service discovery.</span></span>

<span data-ttu-id="1e7f4-146">Tento krok vyžaduje použití certifikátu v Key Vault pro zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-146">This step requires having a certificate in Key Vault for cluster security.</span></span> <span data-ttu-id="1e7f4-147">Další informace o nastavení zabezpečení clusteru s Key Vault najdete v tématu [Tato příručka týkající se vytvoření clusteru v Azure pomocí Resource Manager](service-fabric-cluster-creation-via-arm.md)</span><span class="sxs-lookup"><span data-stu-id="1e7f4-147">For more information on setting up a secure cluster with Key Vault, see [this guide on creating a cluster in Azure using Resource Manager](service-fabric-cluster-creation-via-arm.md)</span></span>

> [!NOTE]
> <span data-ttu-id="1e7f4-148">Můžete přidat ověřování Azure Active Directory kromě certifikát používaný pro přístup ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-148">You may add Azure Active Directory authentication in addition to the certificate used for cluster access.</span></span> <span data-ttu-id="1e7f4-149">Azure Active Directory je doporučeným způsobem, jak spravovat přístup uživatelů k cluster Service Fabric, ale není nutné k dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-149">Azure Active Directory is the recommended way to manage user access to your Service Fabric cluster, but is not necessary to complete this tutorial.</span></span> <span data-ttu-id="1e7f4-150">Certifikát je nutný v obou případech pro zabezpečení mezi uzly clusteru a pro ověřování Azure API Management, které aktuálně nepodporuje ověřování pomocí služby Azure Active Directory pro Service Fabric back-end.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-150">A certificate is required either way for cluster node-to-node security and for Azure API Management authentication, which currently does not support authenticating with Azure Active Directory for a Service Fabric backend.</span></span>

 1. <span data-ttu-id="1e7f4-151">Stáhněte si následující soubor šablony a parametry Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-151">Download the following Resource Manager template and parameters file:</span></span>
 
    - <span data-ttu-id="1e7f4-152">[cluster.JSON][cluster-arm]</span><span class="sxs-lookup"><span data-stu-id="1e7f4-152">[cluster.json][cluster-arm]</span></span>
    - <span data-ttu-id="1e7f4-153">[cluster.Parameters.JSON][cluster-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="1e7f4-153">[cluster.parameters.json][cluster-parameters-arm]</span></span>

 2. <span data-ttu-id="1e7f4-154">Vyplňte prázdné parametry v `cluster.parameters.json` soubor pro vaše nasazení, včetně [Key Vault informace](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) pro váš cluster certifikát.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-154">Fill in the empty parameters in the `cluster.parameters.json` file for your deployment, including the [Key Vault information](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) for your cluster certificate.</span></span>

 3. <span data-ttu-id="1e7f4-155">Použijte následující příkaz prostředí PowerShell pro nasazení Resource Manager šablony a parametr soubory a vytvořte cluster Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-155">Use the following PowerShell command to deploy the Resource Manager template and parameter files to create the Service Fabric cluster:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\cluster.json -TemplateParameterFile .\cluster.parameters.json -Verbose
    ```

### <a name="deploy-api-management"></a><span data-ttu-id="1e7f4-156">Nasadit službu API Management</span><span class="sxs-lookup"><span data-stu-id="1e7f4-156">Deploy API Management</span></span>

<span data-ttu-id="1e7f4-157">Nakonec do virtuální sítě v podsíti nasazení API Management a NSG určená pro API Management.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-157">Finally, deploy API Management to the VNET in the subnet and NSG designated for API Management.</span></span> <span data-ttu-id="1e7f4-158">Nemusíte čekat na nasazení clusteru Service Fabric na Dokončit před nasazením API Management.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-158">You do not need to wait for the Service Fabric cluster deployment to finish before deploying API Management.</span></span> 

<span data-ttu-id="1e7f4-159">V tomto kurzu šablony Resource Manageru rozhraní API Management je předem nakonfigurovaný použít názvy virtuální sítě, podsítě a NSG, které jste nastavili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-159">For this tutorial, the API Management Resource Manager template is pre-configured to use the names of the VNET, subnet, and NSG that you set up in the previous step.</span></span> 

 1. <span data-ttu-id="1e7f4-160">Stáhněte si následující soubor šablony a parametry Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-160">Download the following Resource Manager template and parameters file:</span></span>
 
    - <span data-ttu-id="1e7f4-161">[APIM.JSON][apim-arm]</span><span class="sxs-lookup"><span data-stu-id="1e7f4-161">[apim.json][apim-arm]</span></span>
    - <span data-ttu-id="1e7f4-162">[APIM.Parameters.JSON][apim-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="1e7f4-162">[apim.parameters.json][apim-parameters-arm]</span></span>

 2. <span data-ttu-id="1e7f4-163">Vyplňte prázdné parametry v `apim.parameters.json` pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-163">Fill in the empty parameters in the `apim.parameters.json` for your deployment.</span></span>

 3. <span data-ttu-id="1e7f4-164">Použijte následující příkaz prostředí PowerShell k nasazení Resource Manager soubory šablony a parametrů pro API Management:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-164">Use the following PowerShell command to deploy the Resource Manager template and parameter files for API Management:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
    ```

## <a name="configure-api-management"></a><span data-ttu-id="1e7f4-165">Konfigurace správy rozhraní API</span><span class="sxs-lookup"><span data-stu-id="1e7f4-165">Configure API Management</span></span>

<span data-ttu-id="1e7f4-166">Jakmile cluster API Management a Service Fabric se nasazuje, můžete nakonfigurovat back-end Service Fabric ve službě API Management.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-166">Once your API Management and Service Fabric cluster are deployed, you can configure a Service Fabric backend in API Management.</span></span> <span data-ttu-id="1e7f4-167">Umožňuje vytvořit zásadu služby back-end, která odešle přenosů do clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-167">This allows you to create a backend service policy that sends traffic to your Service Fabric cluster.</span></span>

### <a name="api-management-security"></a><span data-ttu-id="1e7f4-168">Zabezpečení rozhraní API Management</span><span class="sxs-lookup"><span data-stu-id="1e7f4-168">API Management Security</span></span>

<span data-ttu-id="1e7f4-169">Konfigurace back-end Service Fabric, musíte nejprve konfigurovat nastavení zabezpečení rozhraní API správy.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-169">To configure the Service Fabric backend, you first need to configure API Management security settings.</span></span> <span data-ttu-id="1e7f4-170">Chcete-li nakonfigurovat nastavení zabezpečení, přejděte do služby API Management na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-170">To configure security settings, go to your API Management service in the Azure portal.</span></span>

#### <a name="enable-the-api-management-rest-api"></a><span data-ttu-id="1e7f4-171">Povolení správy rozhraní API REST API</span><span class="sxs-lookup"><span data-stu-id="1e7f4-171">Enable the API Management REST API</span></span>

<span data-ttu-id="1e7f4-172">Rozhraní API REST API správy je aktuálně jedinou možností, jak konfigurovat back-end službu.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-172">The API Management REST API is currently the only way to configure a backend service.</span></span> <span data-ttu-id="1e7f4-173">Prvním krokem je povolit rozhraní API REST API pro správu a zabezpečte ji.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-173">The first step is to enable the API Management REST API and secure it.</span></span>

 1. <span data-ttu-id="1e7f4-174">Ve službě API Management vyberte **rozhraní API pro správu - PREVIEW** pod **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-174">In the API Management service, select **Management API - PREVIEW** under **Security**.</span></span>
 2. <span data-ttu-id="1e7f4-175">Zkontrolujte **povolit rozhraní API REST API pro správu** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-175">Check the **Enable API Management REST API** checkbox.</span></span>
 3. <span data-ttu-id="1e7f4-176">Poznamenejte si adresu URL rozhraní API správy – Toto je adresa URL použijeme později nastavit back-end Service Fabric</span><span class="sxs-lookup"><span data-stu-id="1e7f4-176">Note the Management API URL - this is the URL we'll use later to set up the Service Fabric backend</span></span>
 4. <span data-ttu-id="1e7f4-177">Generovat **přístupový Token** tak, že vyberete datum vypršení platnosti a klíč, klikněte **generování** tlačítko ve spodní části stránky.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-177">Generate an **access Token** by selecting an expiry date and a key, then click the **Generate** button toward the bottom of the page.</span></span>
 5. <span data-ttu-id="1e7f4-178">Kopírování **přístupový token** a uložte ho – použijeme ho v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-178">Copy the **access token** and save it - we'll use this in the following steps.</span></span> <span data-ttu-id="1e7f4-179">Všimněte si, že se to neliší od primární klíč a sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-179">Note this is different from the primary key and secondary key.</span></span>

#### <a name="upload-a-service-fabric-client-certificate"></a><span data-ttu-id="1e7f4-180">Nahrajte certifikát klienta Service Fabric</span><span class="sxs-lookup"><span data-stu-id="1e7f4-180">Upload a Service Fabric client certificate</span></span>

<span data-ttu-id="1e7f4-181">API Management musí ověřit k vašemu clusteru Service Fabric pro zjišťování služby pomocí klientského certifikátu, který má přístup ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-181">API Management must authenticate with your Service Fabric cluster for service discovery using a client certificate that has access to your cluster.</span></span> <span data-ttu-id="1e7f4-182">Pro zjednodušení tento kurz používá stejný certifikát zadaný při vytváření clusteru Service Fabric, která ve výchozím nastavení může být použit pro přístup clusteru.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-182">For simplicity, this tutorial uses the same certificate specified when creating the Service Fabric cluster, which by default can be used to access your cluster.</span></span>

 1. <span data-ttu-id="1e7f4-183">Ve službě API Management vyberte **klientské certifikáty - PREVIEW** pod **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-183">In the API Management service, select **Client certificates - PREVIEW** under **Security**.</span></span>
 2. <span data-ttu-id="1e7f4-184">Klikněte **+ přidat** tlačítko</span><span class="sxs-lookup"><span data-stu-id="1e7f4-184">Click the **+ Add** button</span></span>
 2. <span data-ttu-id="1e7f4-185">Vyberte privátní klíč souboru (.pfx) clusteru certifikátu, který jste zadali při vytváření clusteru Service Fabric, zadejte jeho název a zadejte heslo soukromého klíče.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-185">Select the private key file (.pfx) of the cluster certificate that you specified when creating your Service Fabric cluster, give it a name, and provide the private key password.</span></span>

> [!NOTE]
> <span data-ttu-id="1e7f4-186">Tento kurz používá stejný certifikát pro ověřování klientů a zabezpečení mezi uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-186">This tutorial uses the same certificate for client authentication and cluster node-to-node security.</span></span> <span data-ttu-id="1e7f4-187">Pokud nemáte nakonfigurovaná pro přístup k vaší cluster Service Fabric, můžete použít samostatné klientský certifikát.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-187">You may use a separate client certificate if you have one configured to access your Service Fabric cluster.</span></span>

### <a name="configure-the-backend"></a><span data-ttu-id="1e7f4-188">Konfigurace back-end</span><span class="sxs-lookup"><span data-stu-id="1e7f4-188">Configure the backend</span></span>

<span data-ttu-id="1e7f4-189">Teď, když je nakonfigurovaná zabezpečení rozhraní API Management, můžete nakonfigurovat back-end Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-189">Now that API Management security is configured, you can configure the Service Fabric backend.</span></span> <span data-ttu-id="1e7f4-190">Cluster Service Fabric Service Fabric back-EndY, je back-end, nikoli konkrétní službu Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-190">For Service Fabric backends, the Service Fabric cluster is the backend, rather than a specific Service Fabric service.</span></span> <span data-ttu-id="1e7f4-191">To umožňuje jedna zásada směrování do více než jedna služba v clusteru.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-191">This allows a single policy to route to more than one service in the cluster.</span></span>

<span data-ttu-id="1e7f4-192">Tento krok vyžaduje přístupový token, který jste vygenerovali dříve a kryptografický otisk pro svůj cluster certifikát, který jste nahráli do rozhraní API správy v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-192">This step requires the access token you generated earlier and the thumbprint for your cluster certificate you uploaded to API Management in the previous step.</span></span>

> [!NOTE]
> <span data-ttu-id="1e7f4-193">Pokud jste použili samostatný klientský certifikát v předchozím kroku pro rozhraní API Management, musíte kryptografický otisk certifikátu klienta se kromě clusteru kryptografický otisk certifikátu v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-193">If you used a separate client certificate in the previous step for API Management, you need the thumbprint for the client certificate in addition to the cluster certificate thumbprint in this step.</span></span>

<span data-ttu-id="1e7f4-194">Odeslání žádosti o následující HTTP PUT na adresu URL rozhraní API správy rozhraní API, že jste si předtím poznamenali při povolování REST API pro správu rozhraní API konfigurace back-end službu Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-194">Send the following HTTP PUT request to the API Management API URL you noted earlier when enabling the API Management REST API to configure the Service Fabric backend service.</span></span> <span data-ttu-id="1e7f4-195">Měli byste vidět `HTTP 201 Created` odpovědi při úspěšném provedení příkazu.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-195">You should see an `HTTP 201 Created` response when the command succeeds.</span></span> <span data-ttu-id="1e7f4-196">Další informace o každé pole v rozhraní API pro správu [referenční dokumentace rozhraní API back-end](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).</span><span class="sxs-lookup"><span data-stu-id="1e7f4-196">For more information on each field, see the API Management [backend API reference documentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).</span></span>

<span data-ttu-id="1e7f4-197">Příkaz HTTP a adresy URL:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-197">HTTP command and URL:</span></span>
```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
```

<span data-ttu-id="1e7f4-198">Hlavičky požadavku:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-198">Request headers:</span></span>
```http
Authorization: SharedAccessSignature <your access token>
Content-Type: application/json
```

<span data-ttu-id="1e7f4-199">Text žádosti:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-199">Request body:</span></span>
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

<span data-ttu-id="1e7f4-200">**Url** parametr tady je služba plně kvalifikovaný název služby v clusteru, všechny požadavky jsou směrovány do ve výchozím nastavení, pokud není zadán žádný název služby v zásadách back-end.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-200">The **url** parameter here is a fully-qualified service name of a service in your cluster that all requests are routed to by default if no service name is specified in a backend policy.</span></span> <span data-ttu-id="1e7f4-201">Můžete použít název falešných služby, jako například "fabric: / falešných/služby" Pokud nemáte v úmyslu tak, aby měl záložní služby.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-201">You may use a fake service name, such as "fabric:/fake/service" if you do not intend to have a fallback service.</span></span>

<span data-ttu-id="1e7f4-202">Odkazovat na rozhraní API pro správu [referenční dokumentace rozhraní API back-end](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) další podrobnosti o každé pole.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-202">Refer to the API Management [backend API reference documentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) for more details on each field.</span></span>

#### <a name="example"></a><span data-ttu-id="1e7f4-203">Příklad</span><span class="sxs-lookup"><span data-stu-id="1e7f4-203">Example</span></span>

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

## <a name="deploy-a-service-fabric-back-end-service"></a><span data-ttu-id="1e7f4-204">Nasazení služby back-end Service Fabric</span><span class="sxs-lookup"><span data-stu-id="1e7f4-204">Deploy a Service Fabric back-end service</span></span>

<span data-ttu-id="1e7f4-205">Teď, když máte Service Fabric nakonfigurovaný jako back-end pro API Management, můžete vytvořit zásady back-end pro vaše rozhraní API, který odesílá data na vaše služby Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-205">Now that you have the Service Fabric configured as a backend to API Management, you can author backend policies for your APIs that send traffic to your Service Fabric services.</span></span> <span data-ttu-id="1e7f4-206">Ale nejdřív je potřeba služby spuštěné v Service Fabric tak, aby přijímal požadavky.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-206">But first you need a service running in Service Fabric to accept requests.</span></span>

### <a name="create-a-service-fabric-service-with-an-http-endpoint"></a><span data-ttu-id="1e7f4-207">Vytvoření služby Service Fabric se koncový bod HTTP</span><span class="sxs-lookup"><span data-stu-id="1e7f4-207">Create a Service Fabric service with an HTTP endpoint</span></span>

<span data-ttu-id="1e7f4-208">V tomto kurzu vytvoříme základní bezstavové ASP.NET Core spolehlivé služby pomocí výchozí šablona projektu webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-208">For this tutorial, we'll create a basic stateless ASP.NET Core Reliable Service using the default Web API project template.</span></span> <span data-ttu-id="1e7f4-209">Tím se vytvoří koncový bod HTTP pro vaši službu, která budete vystavit pomocí Azure API Management:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-209">This creates an HTTP endpoint for your service, which you'll expose through Azure API Management:</span></span>

```
/api/values
```

<span data-ttu-id="1e7f4-210">Začněte tím, že [nastavení vývojového prostředí pro vývoj ASP.NET Core](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).</span><span class="sxs-lookup"><span data-stu-id="1e7f4-210">Start by [setting up your development environment for ASP.NET Core development](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).</span></span>

<span data-ttu-id="1e7f4-211">Až nastavíte vývojového prostředí Visual Studio spustíte jako správce a vytvoření služby ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-211">Once your development environment is set up, start Visual Studio as Administrator and create an ASP.NET Core service:</span></span>

 1. <span data-ttu-id="1e7f4-212">V sadě Visual Studio vyberte soubor -> Nový projekt.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-212">In Visual Studio, select File -> New Project.</span></span>
 2. <span data-ttu-id="1e7f4-213">Vyberte šablonu aplikace Service Fabric v cloudu a pojmenujte ji **"ApiApplication"**.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-213">Select the Service Fabric Application template under Cloud and name it **"ApiApplication"**.</span></span>
 3. <span data-ttu-id="1e7f4-214">Vyberte šablonu služby ASP.NET Core a název projektu **"WebApiService"**.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-214">Select the ASP.NET Core service template and name the project **"WebApiService"**.</span></span>
 4. <span data-ttu-id="1e7f4-215">Výběr šablony projektu webového rozhraní API ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-215">Select the Web API ASP.NET Core 1.1 project template.</span></span>
 5. <span data-ttu-id="1e7f4-216">Po vytvoření projektu otevřete `PackageRoot\ServiceManifest.xml` a odebrat `Port` atribut z prostředků konfigurace koncového bodu:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-216">Once the project is created, open `PackageRoot\ServiceManifest.xml` and remove the `Port` attribute from the endpoint resource configuration:</span></span>
 
    ```xml
    <Resources>
      <Endpoints>
        <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" />
      </Endpoints>
    </Resources>
    ```

    <span data-ttu-id="1e7f4-217">To umožňuje Service Fabric zadejte port dynamicky z rozsahu portů aplikace, který jsme otevřít prostřednictvím skupinu zabezpečení sítě v šabloně Resource Manager clusteru, umožňuje provoz mají být předány z API Management.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-217">This allows Service Fabric to specify a port dynamically from the application port range, which we opened through the Network Security Group in the cluster Resource Manager template, allowing traffic to flow to it from API Management.</span></span>
 
 6. <span data-ttu-id="1e7f4-218">Stisknutím klávesy F5 ve Visual Studiu ověřte webové rozhraní API není k dispozici místně.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-218">Press F5 in Visual Studio to verify the web API is available locally.</span></span> 

    <span data-ttu-id="1e7f4-219">Otevřete Service Fabric Explorer a podrobnostem konkrétní instanci služby ASP.NET Core najdete v části Základní adresa služby naslouchá na.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-219">Open Service Fabric Explorer and drill down to a specific instance of the ASP.NET Core service to see the base address the service is listening on.</span></span> <span data-ttu-id="1e7f4-220">Přidat `/api/values` s jejím základem adresy a otevřete v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-220">Add `/api/values` to the base address and open it in a browser.</span></span> <span data-ttu-id="1e7f4-221">Tím se spustí metodu Get na ValuesController v šabloně webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-221">This invokes the Get method on the ValuesController in the Web API template.</span></span> <span data-ttu-id="1e7f4-222">Vrátí výchozí odpovědi, které poskytuje šablony, pole JSON, který obsahuje dva řetězce:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-222">It returns the default response that is provided by the template, a JSON array that contains two strings:</span></span>

    ```json
    ["value1", "value2"]`
    ```

    <span data-ttu-id="1e7f4-223">Toto je koncový bod, který budete vystavit přes správu rozhraní API v Azure.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-223">This is the endpoint that you'll expose through API Management in Azure.</span></span>

 7. <span data-ttu-id="1e7f4-224">Nakonec nasazení aplikace na cluster v Azure.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-224">Finally, deploy the application to your cluster in Azure.</span></span> <span data-ttu-id="1e7f4-225">[Pomocí sady Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), klikněte pravým tlačítkem na projekt aplikace a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-225">[Using Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), right-click the Application project and select **Publish**.</span></span> <span data-ttu-id="1e7f4-226">Zadejte svůj koncový bod clusteru (například `mycluster.westus.cloudapp.azure.com:19000`) k nasazení aplikace na cluster Service Fabric v Azure.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-226">Provide your cluster endpoint (for example, `mycluster.westus.cloudapp.azure.com:19000`) to deploy the application to your Service Fabric cluster in Azure.</span></span>

<span data-ttu-id="1e7f4-227">Bezstavové služby ASP.NET Core s názvem `fabric:/ApiApplication/WebApiService` teď by měl být spuštěn v clusteru Service Fabric v Azure.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-227">An ASP.NET Core stateless service named `fabric:/ApiApplication/WebApiService` should now be running in your Service Fabric cluster in Azure.</span></span>

## <a name="create-an-api-operation"></a><span data-ttu-id="1e7f4-228">Vytvoření operace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="1e7f4-228">Create an API operation</span></span>

<span data-ttu-id="1e7f4-229">Nyní je připraven k vytvoření operace ve službě API Management, který externí klienti používat pro komunikaci se službou ASP.NET Core bezstavové spuštěných v clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-229">Now we're ready to create an operation in API Management that external clients use to communicate with the ASP.NET Core stateless service running in the Service Fabric cluster.</span></span>

 1. <span data-ttu-id="1e7f4-230">Přihlaste se k portálu Azure a přejděte do vašeho nasazení služby API Management.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-230">Log in to the Azure portal and navigate to your API Management service deployment.</span></span>
 2. <span data-ttu-id="1e7f4-231">V okně služby API Management vyberte **rozhraní API – náhled**</span><span class="sxs-lookup"><span data-stu-id="1e7f4-231">In the API Management service blade, select **APIs - Preview**</span></span>
 3. <span data-ttu-id="1e7f4-232">Přidání nového rozhraní API kliknutím **prázdné API** pole a vyplníte dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-232">Add a new API by clicking the **Blank API** box and filling out the dialog box:</span></span>

     - <span data-ttu-id="1e7f4-233">**Adresu URL webové služby**: pro Service Fabric back-EndY, není tato hodnota adresy URL používá.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-233">**Web service URL**: For Service Fabric backends, this URL value is not used.</span></span> <span data-ttu-id="1e7f4-234">Zde můžete zadejte libovolnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-234">You can put any value here.</span></span> <span data-ttu-id="1e7f4-235">V tomto kurzu použít: `http://servicefabric`.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-235">For this tutorial, use: `http://servicefabric`.</span></span>
     - <span data-ttu-id="1e7f4-236">**Název**: zadat libovolný název pro rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-236">**Name**: Provide any name for your API.</span></span> <span data-ttu-id="1e7f4-237">V tomto kurzu použít `Service Fabric App`.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-237">For this tutorial, use `Service Fabric App`.</span></span>
     - <span data-ttu-id="1e7f4-238">**Schéma adresy URL**: Vyberte HTTP, HTTPS nebo obě možnosti.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-238">**URL scheme**: Select either HTTP, HTTPS, or both.</span></span> <span data-ttu-id="1e7f4-239">V tomto kurzu použít `both`.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-239">For this tutorial, use `both`.</span></span>
     - <span data-ttu-id="1e7f4-240">**Přípona adresy URL rozhraní API**: Zadejte příponu pro naše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-240">**API URL Suffix**: Provide a suffix for our API.</span></span> <span data-ttu-id="1e7f4-241">V tomto kurzu použít `myapp`.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-241">For this tutorial, use `myapp`.</span></span>
 
 4. <span data-ttu-id="1e7f4-242">Po vytvoření rozhraní API klikněte na tlačítko **+ operace přidání** přidat front-end operace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-242">Once the API is created, click **+ Add operation** to add a front-end API operation.</span></span> <span data-ttu-id="1e7f4-243">Vyplňte hodnoty:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-243">Fill out the values:</span></span>
    
     - <span data-ttu-id="1e7f4-244">**Adresa URL**: vyberte `GET` a zadejte cestu adresy URL pro rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-244">**URL**: Select `GET` and provide a URL path for the API.</span></span> <span data-ttu-id="1e7f4-245">V tomto kurzu použít `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-245">For this tutorial, use `/api/values`.</span></span>
     
       <span data-ttu-id="1e7f4-246">Ve výchozím nastavení zadat cestu adresy URL zde bude zaslána cestu adresy URL back-end služby Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-246">By default, the URL path specified here is the URL path sent to the backend Service Fabric service.</span></span> <span data-ttu-id="1e7f4-247">Pokud používáte stejnou adresu URL cestu, zde která používá službu, v takovém případě `/api/values`, pak operaci funguje bez další úpravy.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-247">If you use the same URL path here that your service uses, in this case `/api/values`, then the operation works without further modification.</span></span> <span data-ttu-id="1e7f4-248">Můžete také určit cestu adresy URL tady, se liší od cesty URL používá váš back-end služby Service Fabric, v takovém případě bude také musíte zadat cestu přepisování v zásadě operaci později.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-248">You may also specify a URL path here that is different from the URL path used by your backend Service Fabric service, in which case you will also need to specify a path rewrite in your operation policy later.</span></span>
     - <span data-ttu-id="1e7f4-249">**Zobrazovaný název**: zadat libovolný název pro rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-249">**Display name**: Provide any name for the API.</span></span> <span data-ttu-id="1e7f4-250">V tomto kurzu použít `Values`.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-250">For this tutorial, use `Values`.</span></span>

## <a name="configure-a-backend-policy"></a><span data-ttu-id="1e7f4-251">Nakonfigurujte zásady back-end</span><span class="sxs-lookup"><span data-stu-id="1e7f4-251">Configure a backend policy</span></span>

<span data-ttu-id="1e7f4-252">Zásady back-end sváže všechno, co společně.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-252">The backend policy ties everything together.</span></span> <span data-ttu-id="1e7f4-253">Toto je, kde můžete konfigurovat službě back-end Service Fabric, do které směrují požadavky.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-253">This is where you configure the backend Service Fabric service to which requests are routed.</span></span> <span data-ttu-id="1e7f4-254">Tyto zásady můžete použít pro všechny operace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-254">You can apply this policy to any API operation.</span></span> <span data-ttu-id="1e7f4-255">[Konfiguraci back-end pro Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) poskytuje následující žádosti o směrování ovládací prvky:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-255">The [backend configuration for Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) provides the following request routing controls:</span></span> 
 - <span data-ttu-id="1e7f4-256">Služba instance výběru zadáním Service Fabric název instance služby, buď pevně zakódované (například `"fabric:/myapp/myservice"`) nebo vygenerovat z požadavku HTTP (například `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).</span><span class="sxs-lookup"><span data-stu-id="1e7f4-256">Service instance selection by specifying a Service Fabric service instance name, either hardcoded (for example, `"fabric:/myapp/myservice"`) or generated from the HTTP request (for example, `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).</span></span>
 - <span data-ttu-id="1e7f4-257">Oddíl rozlišení vygenerováním klíč oddílu pomocí žádné schéma rozdělení oddílů Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-257">Partition resolution by generating a partition key using any Service Fabric partitioning scheme.</span></span>
 - <span data-ttu-id="1e7f4-258">Výběr repliky pro stavové služby.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-258">Replica selection for stateful services.</span></span>
 - <span data-ttu-id="1e7f4-259">Řešení opakování podmínky, které můžete určit podmínky pro přeložit umístění služby a odešlete žádost.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-259">Resolution retry conditions that allow you to specify the conditions for re-resolving a service location and resending a request.</span></span>

<span data-ttu-id="1e7f4-260">V tomto kurzu vytvořte zásadu back-end, který směruje požadavky přímo na bezstavové služby ASP.NET Core dříve nasazené:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-260">For this tutorial, create a backend policy that routes requests directly to the ASP.NET Core stateless service deployed earlier:</span></span>

 1. <span data-ttu-id="1e7f4-261">Vyberte a upravit **příchozí zásady** pro `Values` operaci klepnutím na ikonu pro úpravu a potom výběrem **zobrazení kódu**.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-261">Select and edit the **inbound policies** for the `Values` operation by clicking the edit icon, and then selecting **Code View**.</span></span>
 2. <span data-ttu-id="1e7f4-262">V editoru zásad kódu přidat `set-backend-service` zásad v části příchozí zásady, jak je vidět tady a klikněte na tlačítko **Uložit** tlačítko:</span><span class="sxs-lookup"><span data-stu-id="1e7f4-262">In the policy code editor, add a `set-backend-service` policy under inbound policies as shown here and click the **Save** button:</span></span>
    
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

<span data-ttu-id="1e7f4-263">Úplnou sadu atributů Service Fabric back-end zásady, najdete v části [dokumentace ke službě back endové rozhraní API Management](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)</span><span class="sxs-lookup"><span data-stu-id="1e7f4-263">For a full set of Service Fabric back-end policy attributes, refer to the [API Management back-end documentation](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)</span></span>

### <a name="add-the-api-to-a-product"></a><span data-ttu-id="1e7f4-264">Přidání rozhraní API do produktu.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-264">Add the API to a product.</span></span> 

<span data-ttu-id="1e7f4-265">Než bude možné volat rozhraní API, musí být přidané pro určitý produkt, kde můžete udělit přístup uživatelům.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-265">Before you can call the API, it must be added to a product where you can grant access to users.</span></span> 

 1. <span data-ttu-id="1e7f4-266">Ve službě API Management vyberte **produkty - PREVIEW**.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-266">In the API Management service, select **Products - PREVIEW**.</span></span>
 2. <span data-ttu-id="1e7f4-267">Ve výchozím nastavení, produkty dva poskytovatelé pro správu rozhraní API: úvodní a bez omezení.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-267">By default, API Management providers two products: Starter and Unlimited.</span></span> <span data-ttu-id="1e7f4-268">Vyberte neomezená produktu.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-268">Select the Unlimited product.</span></span>
 3. <span data-ttu-id="1e7f4-269">Vyberte rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-269">Select APIs.</span></span>
 4. <span data-ttu-id="1e7f4-270">Klikněte **+ přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-270">Click the **+ Add** button.</span></span>
 5. <span data-ttu-id="1e7f4-271">Vyberte `Service Fabric App` rozhraní API, které jste vytvořili v předchozích krocích a klikněte na **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-271">Select the `Service Fabric App` API you created in the previous steps and click the **Select** button.</span></span>

### <a name="test-it"></a><span data-ttu-id="1e7f4-272">otestovat</span><span class="sxs-lookup"><span data-stu-id="1e7f4-272">Test it</span></span>

<span data-ttu-id="1e7f4-273">Nyní můžete zkusit odesílání požadavku do back-end služby v Service Fabric pomocí rozhraní API správy přímo z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-273">You can now try sending a request to your back-end service in Service Fabric through API Management directly from the Azure portal.</span></span>

 1. <span data-ttu-id="1e7f4-274">Ve službě API Management vyberte **API – PREVIEW**.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-274">In the API Management service, select **API - PREVIEW**.</span></span>
 2. <span data-ttu-id="1e7f4-275">V `Service Fabric App` rozhraní API, které jste vytvořili v předchozím kroku, vyberte **Test** kartě.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-275">In the `Service Fabric App` API you created in the previous steps, select the **Test** tab.</span></span>
 3. <span data-ttu-id="1e7f4-276">Klikněte **odeslat** tlačítko poslat žádost o test ke službě back-end.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-276">Click the **Send** button to send a test request to the backend service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e7f4-277">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1e7f4-277">Next steps</span></span>

<span data-ttu-id="1e7f4-278">V tomto okamžiku byste měli mít základní instalaci pomocí Service Fabric a API Management.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-278">At this point, you should have a basic setup with Service Fabric and API Management.</span></span>

<span data-ttu-id="1e7f4-279">Tento kurz používá ověřování basic uživatelů na základě certifikátů pro váš cluster Service Fabric si rychle nastavit.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-279">This tutorial uses basic certificate-based user authentication for your Service Fabric cluster to get you set up quickly.</span></span> <span data-ttu-id="1e7f4-280">Pokročilejší ověřování uživatelů pro váš cluster Service Fabric je vhodnější s [ověřování Azure Active Directory](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication).</span><span class="sxs-lookup"><span data-stu-id="1e7f4-280">More advanced user authentication for your Service Fabric cluster is preferable with [Azure Active Directory authentication](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication).</span></span> 

<span data-ttu-id="1e7f4-281">Dále [vytvoření a konfigurace pokročilých nastavení produktu v Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) Příprava aplikace pro provoz skutečném světě.</span><span class="sxs-lookup"><span data-stu-id="1e7f4-281">Next, [create and configure advanced product settings in Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) to prepare your application for real world traffic.</span></span>

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
