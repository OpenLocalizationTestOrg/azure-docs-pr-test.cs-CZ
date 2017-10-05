---
title: "Vytvořit cluster Service Fabric v Azure | Microsoft Docs"
description: "Zjistěte, jak vytvořit cluster Windows nebo Linux Service Fabric v Azure pomocí prostředí PowerShell."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/13/2017
ms.author: ryanwi
ms.openlocfilehash: f62249417552b9cf840bfac3b94c27f63bd7064e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-secure-cluster-on-azure-using-powershell"></a><span data-ttu-id="7a9fb-103">Vytvoření clusteru s podporou zabezpečení v Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="7a9fb-103">Create a secure cluster on Azure using PowerShell</span></span>
<span data-ttu-id="7a9fb-104">V tomto kurzu se dozvíte, jak vytvořit cluster Service Fabric (Windows nebo Linux) běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-104">This tutorial shows you how to create a Service Fabric cluster (Windows or Linux) running in Azure.</span></span> <span data-ttu-id="7a9fb-105">Jakmile budete hotovi, máte cluster se systémem, kterou můžete nasadit aplikace do cloudu.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-105">When you're finished, you have a cluster running in the cloud that you can deploy applications to.</span></span>

<span data-ttu-id="7a9fb-106">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="7a9fb-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7a9fb-107">Vytvoření zabezpečení clusteru Service Fabric v Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="7a9fb-107">Create a secure Service Fabric cluster in Azure using PowerShell</span></span>
> * <span data-ttu-id="7a9fb-108">Zabezpečení clusteru společně s certifikátem X.509</span><span class="sxs-lookup"><span data-stu-id="7a9fb-108">Secure the cluster with an X.509 certificate</span></span>
> * <span data-ttu-id="7a9fb-109">Připojení ke clusteru pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="7a9fb-109">Connect to the cluster using PowerShell</span></span>
> * <span data-ttu-id="7a9fb-110">Odebrat cluster</span><span class="sxs-lookup"><span data-stu-id="7a9fb-110">Remove a cluster</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7a9fb-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7a9fb-111">Prerequisites</span></span>
<span data-ttu-id="7a9fb-112">Před zahájením tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="7a9fb-112">Before you begin this tutorial:</span></span>
- <span data-ttu-id="7a9fb-113">Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span><span class="sxs-lookup"><span data-stu-id="7a9fb-113">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span></span>
- <span data-ttu-id="7a9fb-114">Nainstalujte [modul Service Fabric SDK a prostředí PowerShell](service-fabric-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="7a9fb-114">Install the [Service Fabric SDK and PowerShell module](service-fabric-get-started.md)</span></span>
- <span data-ttu-id="7a9fb-115">Nainstalujte [prostředí Azure Powershell verze modulu 4.1 nebo vyšší](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)</span><span class="sxs-lookup"><span data-stu-id="7a9fb-115">Install the [Azure Powershell module version 4.1 or higher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)</span></span>

<span data-ttu-id="7a9fb-116">Následující postup vytvoří cluster Service Fabric náhled jeden uzel (jeden virtuální počítač).</span><span class="sxs-lookup"><span data-stu-id="7a9fb-116">The following procedure creates a single-node (single virtual machine) preview Service Fabric cluster.</span></span> <span data-ttu-id="7a9fb-117">Cluster je zabezpečená službou certifikát podepsaný svým držitelem, který získá vytvořen společně s clusteru a pak umístit do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-117">The cluster is secured by a self-signed certificate that gets created along with the cluster and then placed in a key vault.</span></span> <span data-ttu-id="7a9fb-118">Clustery s jedním uzlem nelze škálovat nad rámec jednoho virtuálního počítače a clustery preview nelze upgradovat na novější verze.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-118">Single-node clusters cannot be scaled beyond one virtual machine and preview clusters cannot be upgraded to newer versions.</span></span>

<span data-ttu-id="7a9fb-119">Chcete-li vypočítat náklady způsobené spuštění clusteru Service Fabric v Azure pomocí [cenové kalkulačce pro Azure](https://azure.microsoft.com/pricing/calculator/).</span><span class="sxs-lookup"><span data-stu-id="7a9fb-119">To calculate cost incurred by running a Service Fabric cluster in Azure use the [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/).</span></span>
<span data-ttu-id="7a9fb-120">Další informace o vytváření clusterů Service Fabric najdete v tématu [vytvořit cluster Service Fabric pomocí Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="7a9fb-120">For more information on creating Service Fabric clusters, see [Create a Service Fabric cluster by using Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span></span>

## <a name="create-the-cluster-using-azure-powershell"></a><span data-ttu-id="7a9fb-121">Vytvoření clusteru pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7a9fb-121">Create the cluster using Azure PowerShell</span></span>
1. <span data-ttu-id="7a9fb-122">Stáhněte si místní kopii šablony Azure Resource Manageru a soubor parametrů z [šablony Azure Resource Manageru pro Service Fabric](https://aka.ms/securepreviewonelineclustertemplate) úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-122">Download a local copy of the Azure Resource Manager template and the parameter file from the [Azure Resource Manager template for Service Fabric](https://aka.ms/securepreviewonelineclustertemplate) GitHub repository.</span></span>  <span data-ttu-id="7a9fb-123">*azuredeploy.JSON* je šablonu Azure Resource Manager, která definuje cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-123">*azuredeploy.json* is the Azure Resource Manager template that defines a Service Fabric cluster.</span></span> <span data-ttu-id="7a9fb-124">*azuredeploy.Parameters.JSON* je soubor parametrů můžete přizpůsobit nasazení clusteru.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-124">*azuredeploy.parameters.json* is a parameters file for you to customize the cluster deployment.</span></span>

2. <span data-ttu-id="7a9fb-125">Přizpůsobení následující parametry v *azuredeploy.parameters.json* soubor parametrů:</span><span class="sxs-lookup"><span data-stu-id="7a9fb-125">Customize the following parameters in the *azuredeploy.parameters.json* parameters file:</span></span>

   | <span data-ttu-id="7a9fb-126">Parametr</span><span class="sxs-lookup"><span data-stu-id="7a9fb-126">Parameter</span></span>       | <span data-ttu-id="7a9fb-127">Popis</span><span class="sxs-lookup"><span data-stu-id="7a9fb-127">Description</span></span> | <span data-ttu-id="7a9fb-128">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="7a9fb-128">Suggested Value</span></span> |
   | --------------- | ----------- | --------------- |
   | <span data-ttu-id="7a9fb-129">clusterLocation</span><span class="sxs-lookup"><span data-stu-id="7a9fb-129">clusterLocation</span></span> | <span data-ttu-id="7a9fb-130">Oblast Azure do které chcete nasadit cluster.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-130">The Azure region to which to deploy the cluster.</span></span> | <span data-ttu-id="7a9fb-131">*například westeurope, eastasia, eastus*</span><span class="sxs-lookup"><span data-stu-id="7a9fb-131">*for example, westeurope, eastasia, eastus*</span></span> |
   | <span data-ttu-id="7a9fb-132">Název clusteru</span><span class="sxs-lookup"><span data-stu-id="7a9fb-132">clusterName</span></span>     | <span data-ttu-id="7a9fb-133">Název clusteru, který chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-133">Name of the cluster you want to create.</span></span> | <span data-ttu-id="7a9fb-134">*například honzuv sfpreviewcluster*</span><span class="sxs-lookup"><span data-stu-id="7a9fb-134">*for example, bobs-sfpreviewcluster*</span></span> |
   | <span data-ttu-id="7a9fb-135">adminUserName</span><span class="sxs-lookup"><span data-stu-id="7a9fb-135">adminUserName</span></span>   | <span data-ttu-id="7a9fb-136">Účet místního správce na virtuální počítače clusteru.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-136">The local admin account on the cluster virtual machines.</span></span> | <span data-ttu-id="7a9fb-137">*Všechny platné uživatelské jméno systému Windows Server*</span><span class="sxs-lookup"><span data-stu-id="7a9fb-137">*Any valid Windows Server username*</span></span> |
   | <span data-ttu-id="7a9fb-138">adminPassword</span><span class="sxs-lookup"><span data-stu-id="7a9fb-138">adminPassword</span></span>   | <span data-ttu-id="7a9fb-139">Heslo účtu místního správce na virtuální počítače clusteru.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-139">Password of the local admin account on the cluster virtual machines.</span></span> | <span data-ttu-id="7a9fb-140">*Všechny platné heslo systému Windows Server*</span><span class="sxs-lookup"><span data-stu-id="7a9fb-140">*Any valid Windows Server password*</span></span> |
   | <span data-ttu-id="7a9fb-141">clusterCodeVersion</span><span class="sxs-lookup"><span data-stu-id="7a9fb-141">clusterCodeVersion</span></span> | <span data-ttu-id="7a9fb-142">Verze Service Fabric ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-142">The Service Fabric version to run.</span></span> <span data-ttu-id="7a9fb-143">(255.255.X.255 jsou verze preview).</span><span class="sxs-lookup"><span data-stu-id="7a9fb-143">(255.255.X.255 are preview versions).</span></span> | <span data-ttu-id="7a9fb-144">**255.255.5718.255**</span><span class="sxs-lookup"><span data-stu-id="7a9fb-144">**255.255.5718.255**</span></span> |
   | <span data-ttu-id="7a9fb-145">vmInstanceCount</span><span class="sxs-lookup"><span data-stu-id="7a9fb-145">vmInstanceCount</span></span> | <span data-ttu-id="7a9fb-146">Počet virtuálních počítačů v clusteru (může být 1 nebo 3-99).</span><span class="sxs-lookup"><span data-stu-id="7a9fb-146">The number of virtual machines in your cluster (can be 1 or 3-99).</span></span> | <span data-ttu-id="7a9fb-147">**1**</span><span class="sxs-lookup"><span data-stu-id="7a9fb-147">**1**</span></span> | <span data-ttu-id="7a9fb-148">*Pro cluster s podporou preview zadejte jenom jeden virtuální počítač*</span><span class="sxs-lookup"><span data-stu-id="7a9fb-148">*For a preview cluster specify only one virtual machine*</span></span> |

3. <span data-ttu-id="7a9fb-149">Otevřete konzolu prostředí PowerShell, přihlášení k Azure a vyberte předplatné, které chcete nasadit v clusteru:</span><span class="sxs-lookup"><span data-stu-id="7a9fb-149">Open a PowerShell console, login to Azure, and select the subscription you want to deploy the cluster in:</span></span>

   ```powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```
4. <span data-ttu-id="7a9fb-150">Vytváření a šifrování heslo pro certifikát, který chcete používat Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-150">Create and encrypt a password for the certificate to be used by Service Fabric.</span></span>

   ```powershell
   $pwd = "<your password>" | ConvertTo-SecureString -AsPlainText -Force
   ```
5. <span data-ttu-id="7a9fb-151">Vytvoření clusteru a svůj certifikát spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="7a9fb-151">Create the cluster and its certificate by running the following command:</span></span>

   ```powershell
      New-AzureRmServiceFabricCluster
          -TemplateFile C:\Users\me\Desktop\azuredeploy.json `
          -ParameterFile C:\Users\me\Desktop\azuredeploy.parameters.json `
          -CertificateOutputFolder C:\Users\me\Desktop\ `
          -CertificatePassword $pwd `
          -CertificateSubjectName "mycluster.westeurope.cloudapp.azure.com" `
          -ResourceGroupName myclusterRG
   ```

   >[!NOTE]
   ><span data-ttu-id="7a9fb-152">`-CertificateSubjectName` Parametr by měl zarovnané s clusterName parametr zadaný v souboru parametrů, stejně jako domény vázaný na oblast Azure, které jste zvolili, jako například: `clustername.eastus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-152">The `-CertificateSubjectName` parameter should align with the clusterName parameter specified in the parameters file, as well as the domain tied to the Azure region you chose, such as: `clustername.eastus.cloudapp.azure.com`.</span></span>

<span data-ttu-id="7a9fb-153">Po dokončení konfigurace výstupy informace o clusteru vytvoří v Azure.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-153">Once the configuration finishes, it outputs information about the cluster created in Azure.</span></span> <span data-ttu-id="7a9fb-154">Také zkopíruje certifikát clusteru k adresáři - CertificateOutputFolder na cestu, kterou jste zadali pro tento parametr.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-154">It also copies the cluster certificate to the -CertificateOutputFolder directory on the path you specified for this parameter.</span></span> <span data-ttu-id="7a9fb-155">Je nutné tento certifikát pro přístup k Service Fabric Explorer a zobrazení stavu služby clusteru.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-155">You need this certificate to access Service Fabric Explorer and view the health of your cluster.</span></span>

<span data-ttu-id="7a9fb-156">Poznamenejte si adresu URL pro váš cluster, který může být podobná následující adresu URL: *https://mycluster.westeurope.cloudapp.azure.com:19080*</span><span class="sxs-lookup"><span data-stu-id="7a9fb-156">Take note of the URL for your cluster, which may be similar to the following URL: *https://mycluster.westeurope.cloudapp.azure.com:19080*</span></span>

## <a name="modify-the-certificate--access-service-fabric-explorer"></a><span data-ttu-id="7a9fb-157">Změnit certifikát & přístup Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="7a9fb-157">Modify the certificate & access Service Fabric Explorer</span></span> ##

1. <span data-ttu-id="7a9fb-158">Dvakrát klikněte na certifikát, který chcete otevřít Průvodce importem certifikátu.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-158">Double-click the certificate to open the Certificate Import Wizard.</span></span>

2. <span data-ttu-id="7a9fb-159">Použít výchozí nastavení, ale nezapomeňte zaškrtnout **označit tento klíč jako exportovatelný.**</span><span class="sxs-lookup"><span data-stu-id="7a9fb-159">Use default settings, but make sure to check the **Mark this key as exportable.**</span></span> <span data-ttu-id="7a9fb-160">Zaškrtněte políčko v **ochranu privátního klíče** krok.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-160">check box, in the **private key protection** step.</span></span> <span data-ttu-id="7a9fb-161">Visual Studio je potřeba exportovat certifikát při konfiguraci registru kontejner Azure k ověřování Service Fabric Cluster později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-161">Visual Studio needs to export the certificate when configuring Azure Container Registry to Service Fabric Cluster authentication later in this tutorial.</span></span>

3. <span data-ttu-id="7a9fb-162">Service Fabric Explorer nyní lze otevřít v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-162">You can now open Service Fabric Explorer in a browser.</span></span> <span data-ttu-id="7a9fb-163">Chcete-li to provést, přejděte na **ManagementEndpoint** adresu URL pro váš cluster pomocí webového prohlížeče a vyberte certifikát, který byl uložen ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-163">To do so, navigate to the **ManagementEndpoint** URL for your cluster using a web browser, and select the certificate that was saved on your machine.</span></span>

>[!NOTE]
><span data-ttu-id="7a9fb-164">Při otevírání Service Fabric Explorer, zobrazí chyba certifikátu, jak používáte certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-164">When opening Service Fabric Explorer, you see a certificate error, as you are using a self-signed certificate.</span></span> <span data-ttu-id="7a9fb-165">V Edge, budete muset kliknout na tlačítko *podrobnosti* a potom *přejděte na webovou stránku* odkaz.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-165">In Edge, you have to click *Details* and then the *Go on to the webpage* link.</span></span> <span data-ttu-id="7a9fb-166">V prohlížeči Chrome, budete muset kliknout na tlačítko *Upřesnit* a potom *pokračovat* odkaz.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-166">In Chrome, you have to click *Advanced* and then the *proceed* link.</span></span>

>[!NOTE]
><span data-ttu-id="7a9fb-167">Pokud vytvoření clusteru selže, je vždy znovu spustit příkaz, který aktualizuje již nasazené prostředky.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-167">If the cluster creation fails, you can always rerun the command, which updates the resources already deployed.</span></span> <span data-ttu-id="7a9fb-168">Pokud certifikát byl vytvořen jako součást neúspěšné nasazení, vygeneruje se nový.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-168">If a certificate was created as part of the failed deployment, a new one is generated.</span></span> <span data-ttu-id="7a9fb-169">K vyřešení vytváření clusteru najdete v tématu [vytvořit cluster Service Fabric pomocí Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="7a9fb-169">To troubleshoot cluster creation, see [Create a Service Fabric cluster by using Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span></span>

## <a name="connect-to-the-secure-cluster"></a><span data-ttu-id="7a9fb-170">Připojte se ke zabezpečení clusteru</span><span class="sxs-lookup"><span data-stu-id="7a9fb-170">Connect to the secure cluster</span></span>
<span data-ttu-id="7a9fb-171">Připojte ke clusteru pomocí Service Fabric prostředí PowerShell modul nainstalovaný pomocí Service Fabric SDK.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-171">Connect to the cluster using the Service Fabric PowerShell module installed with the Service Fabric SDK.</span></span>  <span data-ttu-id="7a9fb-172">Nejprve nainstalujte certifikát do úložiště osobních (My) aktuálního uživatele ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-172">First, install the certificate into the Personal (My) store of the current user on your computer.</span></span>  <span data-ttu-id="7a9fb-173">Spusťte následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="7a9fb-173">Run the following PowerShell command:</span></span>

```powershell
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

<span data-ttu-id="7a9fb-174">Nyní jste připraveni připojit se ke svému zabezpečenému clusteru.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-174">You are now ready to connect to your secure cluster.</span></span>

<span data-ttu-id="7a9fb-175">**Service Fabric** modulu PowerShell poskytuje mnoho rutiny pro správu clusterů Service Fabric, aplikace a služby.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-175">The **Service Fabric** PowerShell module provides many cmdlets for managing Service Fabric clusters, applications, and services.</span></span>  <span data-ttu-id="7a9fb-176">Použití [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) pro připojení k zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-176">Use the [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) cmdlet to connect to the secure cluster.</span></span> <span data-ttu-id="7a9fb-177">Podrobnosti koncový bod připojení a kryptografický otisk certifikátu se nacházejí ve výstupu z předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-177">The certificate thumbprint and connection endpoint details are found in the output from a previous step.</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="7a9fb-178">Zkontrolujte, zda jste připojeni, a je v pořádku clusteru pomocí [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) rutiny.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-178">Check that you are connected and the cluster is healthy using the [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) cmdlet.</span></span>

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a><span data-ttu-id="7a9fb-179">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="7a9fb-179">Clean up resources</span></span>

<span data-ttu-id="7a9fb-180">Cluster se kromě vlastního prostředku clusteru skládá z dalších prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-180">A cluster is made up of other Azure resources in addition to the cluster resource itself.</span></span> <span data-ttu-id="7a9fb-181">Nejjednodušší způsob, jak odstranit cluster a všechny prostředky, které využívá, je odstranit příslušnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-181">The simplest way to delete the cluster and all the resources it consumes is to delete the resource group.</span></span>

<span data-ttu-id="7a9fb-182">Přihlaste se k Azure a vybrat ID předplatného, pro který chcete odebrat cluster.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-182">Log in to Azure and select the subscription ID with which you want to remove the cluster.</span></span>  <span data-ttu-id="7a9fb-183">Můžete najít svoje ID předplatného přihlášením k [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7a9fb-183">You can find your subscription ID by logging in to the [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="7a9fb-184">Odstranit skupinu prostředků a všechny prostředky clusteru pomocí [rutinu Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="7a9fb-184">Delete the resource group and all the cluster resources using the [Remove-AzureRMResourceGroup cmdlet](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId "Subcription ID"

$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="next-steps"></a><span data-ttu-id="7a9fb-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7a9fb-185">Next steps</span></span>
<span data-ttu-id="7a9fb-186">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="7a9fb-186">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7a9fb-187">Vytvořit cluster Service Fabric v Azure</span><span class="sxs-lookup"><span data-stu-id="7a9fb-187">Create a Service Fabric cluster in Azure</span></span>
> * <span data-ttu-id="7a9fb-188">Zabezpečení clusteru společně s certifikátem X.509</span><span class="sxs-lookup"><span data-stu-id="7a9fb-188">Secure the cluster with an X.509 certificate</span></span>
> * <span data-ttu-id="7a9fb-189">Připojení ke clusteru pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="7a9fb-189">Connect to the cluster using PowerShell</span></span>
> * <span data-ttu-id="7a9fb-190">Odebrat cluster</span><span class="sxs-lookup"><span data-stu-id="7a9fb-190">Remove a cluster</span></span>

<span data-ttu-id="7a9fb-191">V dalším kroku přechodu na následující kurzu se dozvíte, jak nasadit existující aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7a9fb-191">Next, advance to the following tutorial to learn how to deploy an existing application.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="7a9fb-192">Nasazení aplikace .NET existující pomocí Docker Compose</span><span class="sxs-lookup"><span data-stu-id="7a9fb-192">Deploy an existing .NET application with Docker Compose</span></span>](service-fabric-host-app-in-a-container.md)
