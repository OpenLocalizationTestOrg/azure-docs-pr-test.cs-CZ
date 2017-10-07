---
title: aaaCreate Service Fabric cluster v Azure | Microsoft Docs
description: "Zjistěte, jak toocreate Windows nebo Linux Service Fabric cluster v Azure pomocí prostředí PowerShell."
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
ms.openlocfilehash: e697e2a2e99f67cb02422efdb368c664c9fd5a97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-secure-cluster-on-azure-using-powershell"></a><span data-ttu-id="c1113-103">Vytvoření clusteru s podporou zabezpečení v Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c1113-103">Create a secure cluster on Azure using PowerShell</span></span>
<span data-ttu-id="c1113-104">Tento kurz ukazuje, jak toocreate Service Fabric clusteru (Windows nebo Linux) spuštěná v Azure.</span><span class="sxs-lookup"><span data-stu-id="c1113-104">This tutorial shows you how toocreate a Service Fabric cluster (Windows or Linux) running in Azure.</span></span> <span data-ttu-id="c1113-105">Jakmile budete hotovi, máte cluster se systémem hello cloudu, který můžete nasadit aplikace do.</span><span class="sxs-lookup"><span data-stu-id="c1113-105">When you're finished, you have a cluster running in hello cloud that you can deploy applications to.</span></span>

<span data-ttu-id="c1113-106">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="c1113-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c1113-107">Vytvoření zabezpečení clusteru Service Fabric v Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c1113-107">Create a secure Service Fabric cluster in Azure using PowerShell</span></span>
> * <span data-ttu-id="c1113-108">Zabezpečení clusteru hello společně s certifikátem X.509</span><span class="sxs-lookup"><span data-stu-id="c1113-108">Secure hello cluster with an X.509 certificate</span></span>
> * <span data-ttu-id="c1113-109">Připojte toohello cluster pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c1113-109">Connect toohello cluster using PowerShell</span></span>
> * <span data-ttu-id="c1113-110">Odebrat cluster</span><span class="sxs-lookup"><span data-stu-id="c1113-110">Remove a cluster</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1113-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c1113-111">Prerequisites</span></span>
<span data-ttu-id="c1113-112">Před zahájením tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="c1113-112">Before you begin this tutorial:</span></span>
- <span data-ttu-id="c1113-113">Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span><span class="sxs-lookup"><span data-stu-id="c1113-113">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span></span>
- <span data-ttu-id="c1113-114">Nainstalujte hello [modul Service Fabric SDK a prostředí PowerShell](service-fabric-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="c1113-114">Install hello [Service Fabric SDK and PowerShell module](service-fabric-get-started.md)</span></span>
- <span data-ttu-id="c1113-115">Nainstalujte hello [prostředí Azure Powershell verze modulu 4.1 nebo vyšší](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)</span><span class="sxs-lookup"><span data-stu-id="c1113-115">Install hello [Azure Powershell module version 4.1 or higher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)</span></span>

<span data-ttu-id="c1113-116">Hello následující postup vytvoří cluster Service Fabric náhled jeden uzel (jeden virtuální počítač).</span><span class="sxs-lookup"><span data-stu-id="c1113-116">hello following procedure creates a single-node (single virtual machine) preview Service Fabric cluster.</span></span> <span data-ttu-id="c1113-117">Hello clusteru je zabezpečená službou certifikát podepsaný svým držitelem, který získá vytvořen společně s hello clusteru a pak umístit do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="c1113-117">hello cluster is secured by a self-signed certificate that gets created along with hello cluster and then placed in a key vault.</span></span> <span data-ttu-id="c1113-118">Clustery s jedním uzlem nelze škálovat nad rámec jednoho virtuálního počítače a clustery preview nemůže být toonewer upgradovaná verze.</span><span class="sxs-lookup"><span data-stu-id="c1113-118">Single-node clusters cannot be scaled beyond one virtual machine and preview clusters cannot be upgraded toonewer versions.</span></span>

<span data-ttu-id="c1113-119">toocalculate náklady způsobené spuštění clusteru Service Fabric v Azure pomocí hello [cenové kalkulačce pro Azure](https://azure.microsoft.com/pricing/calculator/).</span><span class="sxs-lookup"><span data-stu-id="c1113-119">toocalculate cost incurred by running a Service Fabric cluster in Azure use hello [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/).</span></span>
<span data-ttu-id="c1113-120">Další informace o vytváření clusterů Service Fabric najdete v tématu [vytvořit cluster Service Fabric pomocí Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="c1113-120">For more information on creating Service Fabric clusters, see [Create a Service Fabric cluster by using Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span></span>

## <a name="create-hello-cluster-using-azure-powershell"></a><span data-ttu-id="c1113-121">Vytvoření clusteru hello pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c1113-121">Create hello cluster using Azure PowerShell</span></span>
1. <span data-ttu-id="c1113-122">Stáhněte si místní kopii šablony Azure Resource Manageru hello a soubor parametrů hello z hello [šablony Azure Resource Manageru pro Service Fabric](https://aka.ms/securepreviewonelineclustertemplate) úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="c1113-122">Download a local copy of hello Azure Resource Manager template and hello parameter file from hello [Azure Resource Manager template for Service Fabric](https://aka.ms/securepreviewonelineclustertemplate) GitHub repository.</span></span>  <span data-ttu-id="c1113-123">*azuredeploy.JSON* je hello šablony Azure Resource Manageru, která definuje cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="c1113-123">*azuredeploy.json* is hello Azure Resource Manager template that defines a Service Fabric cluster.</span></span> <span data-ttu-id="c1113-124">*azuredeploy.Parameters.JSON* je soubor parametrů pro vás nasazení clusteru toocustomize hello.</span><span class="sxs-lookup"><span data-stu-id="c1113-124">*azuredeploy.parameters.json* is a parameters file for you toocustomize hello cluster deployment.</span></span>

2. <span data-ttu-id="c1113-125">Přizpůsobení hello následující parametry v hello *azuredeploy.parameters.json* soubor parametrů:</span><span class="sxs-lookup"><span data-stu-id="c1113-125">Customize hello following parameters in hello *azuredeploy.parameters.json* parameters file:</span></span>

   | <span data-ttu-id="c1113-126">Parametr</span><span class="sxs-lookup"><span data-stu-id="c1113-126">Parameter</span></span>       | <span data-ttu-id="c1113-127">Popis</span><span class="sxs-lookup"><span data-stu-id="c1113-127">Description</span></span> | <span data-ttu-id="c1113-128">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="c1113-128">Suggested Value</span></span> |
   | --------------- | ----------- | --------------- |
   | <span data-ttu-id="c1113-129">clusterLocation</span><span class="sxs-lookup"><span data-stu-id="c1113-129">clusterLocation</span></span> | <span data-ttu-id="c1113-130">Hello oblast Azure toowhich toodeploy hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="c1113-130">hello Azure region toowhich toodeploy hello cluster.</span></span> | <span data-ttu-id="c1113-131">*například westeurope, eastasia, eastus*</span><span class="sxs-lookup"><span data-stu-id="c1113-131">*for example, westeurope, eastasia, eastus*</span></span> |
   | <span data-ttu-id="c1113-132">Název clusteru</span><span class="sxs-lookup"><span data-stu-id="c1113-132">clusterName</span></span>     | <span data-ttu-id="c1113-133">Název clusteru hello chcete toocreate.</span><span class="sxs-lookup"><span data-stu-id="c1113-133">Name of hello cluster you want toocreate.</span></span> | <span data-ttu-id="c1113-134">*například honzuv sfpreviewcluster*</span><span class="sxs-lookup"><span data-stu-id="c1113-134">*for example, bobs-sfpreviewcluster*</span></span> |
   | <span data-ttu-id="c1113-135">adminUserName</span><span class="sxs-lookup"><span data-stu-id="c1113-135">adminUserName</span></span>   | <span data-ttu-id="c1113-136">Hello účet místního správce na hello clusteru virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c1113-136">hello local admin account on hello cluster virtual machines.</span></span> | <span data-ttu-id="c1113-137">*Všechny platné uživatelské jméno systému Windows Server*</span><span class="sxs-lookup"><span data-stu-id="c1113-137">*Any valid Windows Server username*</span></span> |
   | <span data-ttu-id="c1113-138">adminPassword</span><span class="sxs-lookup"><span data-stu-id="c1113-138">adminPassword</span></span>   | <span data-ttu-id="c1113-139">Heslo účtu místního správce hello na hello clusteru virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c1113-139">Password of hello local admin account on hello cluster virtual machines.</span></span> | <span data-ttu-id="c1113-140">*Všechny platné heslo systému Windows Server*</span><span class="sxs-lookup"><span data-stu-id="c1113-140">*Any valid Windows Server password*</span></span> |
   | <span data-ttu-id="c1113-141">clusterCodeVersion</span><span class="sxs-lookup"><span data-stu-id="c1113-141">clusterCodeVersion</span></span> | <span data-ttu-id="c1113-142">toorun verze Service Fabric Hello.</span><span class="sxs-lookup"><span data-stu-id="c1113-142">hello Service Fabric version toorun.</span></span> <span data-ttu-id="c1113-143">(255.255.X.255 jsou verze preview).</span><span class="sxs-lookup"><span data-stu-id="c1113-143">(255.255.X.255 are preview versions).</span></span> | <span data-ttu-id="c1113-144">**255.255.5718.255**</span><span class="sxs-lookup"><span data-stu-id="c1113-144">**255.255.5718.255**</span></span> |
   | <span data-ttu-id="c1113-145">vmInstanceCount</span><span class="sxs-lookup"><span data-stu-id="c1113-145">vmInstanceCount</span></span> | <span data-ttu-id="c1113-146">Hello počet virtuálních počítačů v clusteru (může být 1 nebo 3-99).</span><span class="sxs-lookup"><span data-stu-id="c1113-146">hello number of virtual machines in your cluster (can be 1 or 3-99).</span></span> | <span data-ttu-id="c1113-147">**1**</span><span class="sxs-lookup"><span data-stu-id="c1113-147">**1**</span></span> | <span data-ttu-id="c1113-148">*Pro cluster s podporou preview zadejte jenom jeden virtuální počítač*</span><span class="sxs-lookup"><span data-stu-id="c1113-148">*For a preview cluster specify only one virtual machine*</span></span> |

3. <span data-ttu-id="c1113-149">Otevřete konzolu prostředí PowerShell, tooAzure přihlášení a vyberte hello odběr, který chcete v clusteru toodeploy hello:</span><span class="sxs-lookup"><span data-stu-id="c1113-149">Open a PowerShell console, login tooAzure, and select hello subscription you want toodeploy hello cluster in:</span></span>

   ```powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```
4. <span data-ttu-id="c1113-150">Vytváření a šifrování heslo pro toobe certifikát hello používá Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="c1113-150">Create and encrypt a password for hello certificate toobe used by Service Fabric.</span></span>

   ```powershell
   $pwd = "<your password>" | ConvertTo-SecureString -AsPlainText -Force
   ```
5. <span data-ttu-id="c1113-151">Vytvoření clusteru hello a svůj certifikát spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c1113-151">Create hello cluster and its certificate by running hello following command:</span></span>

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
   ><span data-ttu-id="c1113-152">Hello `-CertificateSubjectName` parametr by měl zarovnané s hello clusterName parametr zadaný v souboru parametrů hello, stejně jako hello domény svázané toohello oblast Azure jste zvolili, jako například: `clustername.eastus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="c1113-152">hello `-CertificateSubjectName` parameter should align with hello clusterName parameter specified in hello parameters file, as well as hello domain tied toohello Azure region you chose, such as: `clustername.eastus.cloudapp.azure.com`.</span></span>

<span data-ttu-id="c1113-153">Po dokončení konfigurace hello výstupy informace o clusteru hello vytvoří v Azure.</span><span class="sxs-lookup"><span data-stu-id="c1113-153">Once hello configuration finishes, it outputs information about hello cluster created in Azure.</span></span> <span data-ttu-id="c1113-154">Také zkopíruje hello clusteru certifikát toohello - CertificateOutputFolder adresáře na hello cesta, kterou jste zadali pro tento parametr.</span><span class="sxs-lookup"><span data-stu-id="c1113-154">It also copies hello cluster certificate toohello -CertificateOutputFolder directory on hello path you specified for this parameter.</span></span> <span data-ttu-id="c1113-155">Je nutné tento certifikát tooaccess Service Fabric Explorer a zobrazení stavu hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="c1113-155">You need this certificate tooaccess Service Fabric Explorer and view hello health of your cluster.</span></span>

<span data-ttu-id="c1113-156">Poznamenejte si adresu URL hello pro váš cluster, který může být podobné toohello následující adresu URL: *https://mycluster.westeurope.cloudapp.azure.com:19080*</span><span class="sxs-lookup"><span data-stu-id="c1113-156">Take note of hello URL for your cluster, which may be similar toohello following URL: *https://mycluster.westeurope.cloudapp.azure.com:19080*</span></span>

## <a name="modify-hello-certificate--access-service-fabric-explorer"></a><span data-ttu-id="c1113-157">Upravit certifikát pro hello & přístup Service Fabric Exploreru</span><span class="sxs-lookup"><span data-stu-id="c1113-157">Modify hello certificate & access Service Fabric Explorer</span></span> ##

1. <span data-ttu-id="c1113-158">Dvakrát klikněte na hello certifikát tooopen hello Průvodce importem certifikátu.</span><span class="sxs-lookup"><span data-stu-id="c1113-158">Double-click hello certificate tooopen hello Certificate Import Wizard.</span></span>

2. <span data-ttu-id="c1113-159">Použít výchozí nastavení, ale ujistěte se, zda text hello toocheck **označit tento klíč jako exportovatelný.**</span><span class="sxs-lookup"><span data-stu-id="c1113-159">Use default settings, but make sure toocheck hello **Mark this key as exportable.**</span></span> <span data-ttu-id="c1113-160">Zaškrtněte políčko v hello **ochranu privátního klíče** krok.</span><span class="sxs-lookup"><span data-stu-id="c1113-160">check box, in hello **private key protection** step.</span></span> <span data-ttu-id="c1113-161">Visual Studio musí certifikát hello tooexport při konfiguraci ověřování clusteru infrastruktury Azure kontejneru registru tooService později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c1113-161">Visual Studio needs tooexport hello certificate when configuring Azure Container Registry tooService Fabric Cluster authentication later in this tutorial.</span></span>

3. <span data-ttu-id="c1113-162">Service Fabric Explorer nyní lze otevřít v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c1113-162">You can now open Service Fabric Explorer in a browser.</span></span> <span data-ttu-id="c1113-163">toodo tedy přejděte toohello **ManagementEndpoint** adresu URL pro váš cluster pomocí webového prohlížeče a vyberte hello certifikát, který byl uložen ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c1113-163">toodo so, navigate toohello **ManagementEndpoint** URL for your cluster using a web browser, and select hello certificate that was saved on your machine.</span></span>

>[!NOTE]
><span data-ttu-id="c1113-164">Při otevírání Service Fabric Explorer, zobrazí chyba certifikátu, jak používáte certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="c1113-164">When opening Service Fabric Explorer, you see a certificate error, as you are using a self-signed certificate.</span></span> <span data-ttu-id="c1113-165">V Edgi máte tooclick *podrobnosti* a pak hello *přejděte na webové stránce toohello* odkaz.</span><span class="sxs-lookup"><span data-stu-id="c1113-165">In Edge, you have tooclick *Details* and then hello *Go on toohello webpage* link.</span></span> <span data-ttu-id="c1113-166">V prohlížeči Chrome, máte tooclick *Upřesnit* a pak hello *pokračovat* odkaz.</span><span class="sxs-lookup"><span data-stu-id="c1113-166">In Chrome, you have tooclick *Advanced* and then hello *proceed* link.</span></span>

>[!NOTE]
><span data-ttu-id="c1113-167">Pokud hello vytváření clusteru selže, může vždy znovu hello příkaz, který aktualizuje již nasazené prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="c1113-167">If hello cluster creation fails, you can always rerun hello command, which updates hello resources already deployed.</span></span> <span data-ttu-id="c1113-168">Pokud certifikát byl vytvořen jako součást nasazení hello se nezdařilo, vygeneruje se nový.</span><span class="sxs-lookup"><span data-stu-id="c1113-168">If a certificate was created as part of hello failed deployment, a new one is generated.</span></span> <span data-ttu-id="c1113-169">Vytvoření clusteru tootroubleshoot, najdete v části [vytvořit cluster Service Fabric pomocí Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="c1113-169">tootroubleshoot cluster creation, see [Create a Service Fabric cluster by using Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span></span>

## <a name="connect-toohello-secure-cluster"></a><span data-ttu-id="c1113-170">Připojte toohello zabezpečené cluster</span><span class="sxs-lookup"><span data-stu-id="c1113-170">Connect toohello secure cluster</span></span>
<span data-ttu-id="c1113-171">Připojte toohello clusteru pomocí modulu Service Fabric prostředí PowerShell hello nainstalované s hello Service Fabric SDK.</span><span class="sxs-lookup"><span data-stu-id="c1113-171">Connect toohello cluster using hello Service Fabric PowerShell module installed with hello Service Fabric SDK.</span></span>  <span data-ttu-id="c1113-172">Nejprve nainstalujte hello certifikát do úložiště osobních (My) hello hello aktuální uživatel ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c1113-172">First, install hello certificate into hello Personal (My) store of hello current user on your computer.</span></span>  <span data-ttu-id="c1113-173">Spusťte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="c1113-173">Run hello following PowerShell command:</span></span>

```powershell
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

<span data-ttu-id="c1113-174">Nyní je připraven tooconnect tooyour zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="c1113-174">You are now ready tooconnect tooyour secure cluster.</span></span>

<span data-ttu-id="c1113-175">Hello **Service Fabric** modulu PowerShell poskytuje mnoho rutiny pro správu clusterů Service Fabric, aplikace a služby.</span><span class="sxs-lookup"><span data-stu-id="c1113-175">hello **Service Fabric** PowerShell module provides many cmdlets for managing Service Fabric clusters, applications, and services.</span></span>  <span data-ttu-id="c1113-176">Použití hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) rutiny tooconnect toohello zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="c1113-176">Use hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) cmdlet tooconnect toohello secure cluster.</span></span> <span data-ttu-id="c1113-177">Hello kryptografický otisk certifikátu a podrobnosti koncový bod připojení se nacházejí v hello výstup z předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="c1113-177">hello certificate thumbprint and connection endpoint details are found in hello output from a previous step.</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="c1113-178">Zkontrolujte, zda jste připojeni a hello clusteru je v pořádku, pomocí hello [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) rutiny.</span><span class="sxs-lookup"><span data-stu-id="c1113-178">Check that you are connected and hello cluster is healthy using hello [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) cmdlet.</span></span>

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a><span data-ttu-id="c1113-179">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="c1113-179">Clean up resources</span></span>

<span data-ttu-id="c1113-180">Cluster se skládá z jiných prostředků Azure kromě toohello prostředek clusteru, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="c1113-180">A cluster is made up of other Azure resources in addition toohello cluster resource itself.</span></span> <span data-ttu-id="c1113-181">Hello nejjednodušší způsob, jak toodelete hello clusteru a všechny prostředky hello, které budou využívat je skupina prostředků toodelete hello.</span><span class="sxs-lookup"><span data-stu-id="c1113-181">hello simplest way toodelete hello cluster and all hello resources it consumes is toodelete hello resource group.</span></span>

<span data-ttu-id="c1113-182">Přihlaste se tooAzure a vybrat ID předplatného hello, pro který chcete tooremove hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="c1113-182">Log in tooAzure and select hello subscription ID with which you want tooremove hello cluster.</span></span>  <span data-ttu-id="c1113-183">Můžete najít svoje ID předplatného přihlášením toohello [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c1113-183">You can find your subscription ID by logging in toohello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="c1113-184">Odstranit skupinu prostředků hello a všechny prostředky clusteru hello pomocí hello [rutinu Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="c1113-184">Delete hello resource group and all hello cluster resources using hello [Remove-AzureRMResourceGroup cmdlet](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId "Subcription ID"

$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="next-steps"></a><span data-ttu-id="c1113-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c1113-185">Next steps</span></span>
<span data-ttu-id="c1113-186">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="c1113-186">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c1113-187">Vytvořit cluster Service Fabric v Azure</span><span class="sxs-lookup"><span data-stu-id="c1113-187">Create a Service Fabric cluster in Azure</span></span>
> * <span data-ttu-id="c1113-188">Zabezpečení clusteru hello společně s certifikátem X.509</span><span class="sxs-lookup"><span data-stu-id="c1113-188">Secure hello cluster with an X.509 certificate</span></span>
> * <span data-ttu-id="c1113-189">Připojte toohello cluster pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c1113-189">Connect toohello cluster using PowerShell</span></span>
> * <span data-ttu-id="c1113-190">Odebrat cluster</span><span class="sxs-lookup"><span data-stu-id="c1113-190">Remove a cluster</span></span>

<span data-ttu-id="c1113-191">V dalším kroku zálohy toohello následující kurz toolearn jak toodeploy stávající aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1113-191">Next, advance toohello following tutorial toolearn how toodeploy an existing application.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="c1113-192">Nasazení aplikace .NET existující pomocí Docker Compose</span><span class="sxs-lookup"><span data-stu-id="c1113-192">Deploy an existing .NET application with Docker Compose</span></span>](service-fabric-host-app-in-a-container.md)
