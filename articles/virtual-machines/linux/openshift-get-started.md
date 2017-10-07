---
title: "aaaDeploy OpenShift původu tooAzure | Microsoft Docs"
description: "Přečtěte si toodeploy OpenShift původu tooAzure virtuálních počítačů."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: jbinder
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: jbinder
ms.openlocfilehash: a67450c46da41134a5f6c669a9e54e14773ac5b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-openshift-origin-tooazure-virtual-machines"></a><span data-ttu-id="80c11-103">Nasazení OpenShift původu tooAzure virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="80c11-103">Deploy OpenShift Origin tooAzure Virtual Machines</span></span> 

<span data-ttu-id="80c11-104">[Původ OpenShift](https://www.openshift.org/) je kontejner platforma s otevřeným zdrojem založený na [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="80c11-104">[OpenShift Origin](https://www.openshift.org/) is an open source container platform built on [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="80c11-105">Zjednodušuje proces hello nasazení, škálování a provozování víceklientským aplikacím.</span><span class="sxs-lookup"><span data-stu-id="80c11-105">It simplifies hello process of deploying, scaling, and operating multi-tenant applications.</span></span> 

<span data-ttu-id="80c11-106">Tato příručka popisuje, jak hello toodeploy OpenShift původ na virtuálních počítačích Azure pomocí rozhraní příkazového řádku Azure a šablon Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="80c11-106">This guide describes how toodeploy OpenShift Origin on Azure Virtual Machines using hello Azure CLI and Azure Resource Manager Templates.</span></span> <span data-ttu-id="80c11-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="80c11-107">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="80c11-108">Vytvoření klíčů SSH pro hello OpenShift cluster KeyVault toomanage.</span><span class="sxs-lookup"><span data-stu-id="80c11-108">Create a KeyVault toomanage SSH keys for hello OpenShift cluster.</span></span>
> * <span data-ttu-id="80c11-109">Nasazení clusteru OpenShift na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="80c11-109">Deploy an OpenShift cluster on Azure VMs.</span></span> 
> * <span data-ttu-id="80c11-110">Instalace a konfigurace hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="80c11-110">Install and configure hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello cluster.</span></span>
> * <span data-ttu-id="80c11-111">Přizpůsobte hello OpenShift nasazení.</span><span class="sxs-lookup"><span data-stu-id="80c11-111">Customize hello OpenShift deployment.</span></span>

<span data-ttu-id="80c11-112">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="80c11-112">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="80c11-113">Tento úvodní vyžaduje hello Azure CLI verze 2.0.8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="80c11-113">This quick start requires hello Azure CLI version 2.0.8 or later.</span></span> <span data-ttu-id="80c11-114">verze hello toofind, spusťte `az --version`.</span><span class="sxs-lookup"><span data-stu-id="80c11-114">toofind hello version, run `az --version`.</span></span> <span data-ttu-id="80c11-115">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="80c11-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="log-in-tooazure"></a><span data-ttu-id="80c11-116">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="80c11-116">Log in tooAzure</span></span> 
<span data-ttu-id="80c11-117">Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů nebo klikněte na tlačítko **vyzkoušet** toouse cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="80c11-117">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions or click **Try it** toouse Cloud Shell.</span></span>

```azurecli 
az login
```
## <a name="create-a-resource-group"></a><span data-ttu-id="80c11-118">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="80c11-118">Create a resource group</span></span>

<span data-ttu-id="80c11-119">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="80c11-119">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="80c11-120">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="80c11-120">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="80c11-121">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění.</span><span class="sxs-lookup"><span data-stu-id="80c11-121">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-key-vault"></a><span data-ttu-id="80c11-122">Vytvoření trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="80c11-122">Create a Key Vault</span></span>
<span data-ttu-id="80c11-123">Vytvoření klíčů SSH pro hello cluster hello toostore KeyVault s hello [vytvořit az keyvault](/cli/azure/keyvault#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="80c11-123">Create a KeyVault toostore hello SSH keys for hello cluster with hello [az keyvault create](/cli/azure/keyvault#create) command.</span></span>  

```azurecli 
az keyvault create --resource-group myResourceGroup --name myKeyVault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a><span data-ttu-id="80c11-124">Vytvoření klíče SSH</span><span class="sxs-lookup"><span data-stu-id="80c11-124">Create an SSH key</span></span> 
<span data-ttu-id="80c11-125">Klíč SSH je potřebné toosecure přístup toohello OpenShift původní cluster.</span><span class="sxs-lookup"><span data-stu-id="80c11-125">An SSH key is needed toosecure access toohello OpenShift Origin cluster.</span></span> <span data-ttu-id="80c11-126">Vytvoření SSH dvojici klíčů pomocí hello `ssh-keygen` příkaz.</span><span class="sxs-lookup"><span data-stu-id="80c11-126">Create an SSH key-pair using hello `ssh-keygen` command.</span></span> 
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> <span data-ttu-id="80c11-127">heslo nesmí mít Hello pár klíčů SSH, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="80c11-127">hello SSH key pair you create must not have a passphrase.</span></span>

<span data-ttu-id="80c11-128">Další informace o klíče SSH v systému Windows [jak toocreate SSH klíčů v systému Windows](/azure/virtual-machines/linux/ssh-from-windows).</span><span class="sxs-lookup"><span data-stu-id="80c11-128">For more information on SSH keys on Windows, [How toocreate SSH keys on Windows](/azure/virtual-machines/linux/ssh-from-windows).</span></span>

## <a name="store-ssh-private-key-in-key-vault"></a><span data-ttu-id="80c11-129">Uložit privátní klíč SSH v Key Vault</span><span class="sxs-lookup"><span data-stu-id="80c11-129">Store SSH private key in Key Vault</span></span>
<span data-ttu-id="80c11-130">Hello OpenShift nasazení používá jste vytvořili toosecure přístup toohello OpenShift hlavní klíč SSH hello.</span><span class="sxs-lookup"><span data-stu-id="80c11-130">hello OpenShift deployment uses hello SSH key you created toosecure access toohello OpenShift master.</span></span> <span data-ttu-id="80c11-131">tooenable hello nasazení toosecurely načíst klíč SSH hello, uložení klíče hello v Key Vault pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="80c11-131">tooenable hello deployment toosecurely retrieve hello SSH key, store hello key in Key Vault using hello following command.</span></span>

# <a name="enabled-for-template-deployment"></a><span data-ttu-id="80c11-132">Povolit pro nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="80c11-132">Enabled for template deployment</span></span>
```azurecli
az keyvault secret set --vault-name KeyVaultName --name OpenShiftKey --file ~/.ssh/openshift.rsa
```

## <a name="create-a-service-principal"></a><span data-ttu-id="80c11-133">Vytvoření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="80c11-133">Create a service principal</span></span> 
<span data-ttu-id="80c11-134">OpenShift komunikuje se službou Azure pomocí uživatelského jména a hesla nebo hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="80c11-134">OpenShift communicates with Azure using a username and password or a service principal.</span></span> <span data-ttu-id="80c11-135">Objektu zabezpečení služby Azure je identita zabezpečení, která můžete použít s aplikací, služeb a automatizace nástroje, například OpenShift.</span><span class="sxs-lookup"><span data-stu-id="80c11-135">An Azure service principal is a security identity that you can use with apps, services, and automation tools like OpenShift.</span></span> <span data-ttu-id="80c11-136">Můžete řídit a definovat hello oprávnění objektu služby hello toowhat operace můžete provádět v rámci Azure.</span><span class="sxs-lookup"><span data-stu-id="80c11-136">You control and define hello permissions as toowhat operations hello service principal can perform in Azure.</span></span> <span data-ttu-id="80c11-137">tooimprove zabezpečení přes právě poskytnutí uživatelského jména a hesla, tento příklad vytvoří základní služby hlavní.</span><span class="sxs-lookup"><span data-stu-id="80c11-137">tooimprove security over just providing a username and password, this example creates a basic service principal.</span></span>

<span data-ttu-id="80c11-138">Vytvoření služby hlavní s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) a výstup hello pověření, které potřebuje OpenShift:</span><span class="sxs-lookup"><span data-stu-id="80c11-138">Create a service principal with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) and output hello credentials that OpenShift needs:</span></span>

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {strong password} \
          --scopes $(az group show --name myResourceGroup --query id)
```
<span data-ttu-id="80c11-139">Poznamenejte si vlastnosti appId hello vrácená z příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="80c11-139">Take note of hello appId property returned from hello command.</span></span>
```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {strong password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > <span data-ttu-id="80c11-140">Nevytvářejte nezabezpečené heslo.</span><span class="sxs-lookup"><span data-stu-id="80c11-140">Don't create an insecure password.</span></span>  <span data-ttu-id="80c11-141">Postupujte podle pokynů v tématu [Pravidla a omezení pro hesla Azure AD](/azure/active-directory/active-directory-passwords-policy).</span><span class="sxs-lookup"><span data-stu-id="80c11-141">Follow the [Azure AD password rules and restrictions](/azure/active-directory/active-directory-passwords-policy) guidance.</span></span>

<span data-ttu-id="80c11-142">Další informace o objekty služby najdete v tématu [vytvořit objekt služby Azure pomocí Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)</span><span class="sxs-lookup"><span data-stu-id="80c11-142">For more information on service principals, see [Create an Azure service principal with Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)</span></span>

## <a name="deploy-hello-openshift-origin-template"></a><span data-ttu-id="80c11-143">Nasazení hello OpenShift počátek šablony</span><span class="sxs-lookup"><span data-stu-id="80c11-143">Deploy hello OpenShift Origin template</span></span>
<span data-ttu-id="80c11-144">Nasaďte další OpenShift počátek pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="80c11-144">Next deploy OpenShift Origin using an Azure Resource Manager template.</span></span> 

> [!NOTE] 
> <span data-ttu-id="80c11-145">Hello následující příkaz vyžaduje az rozhraní příkazového řádku 2.0.8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="80c11-145">hello following command requires az CLI 2.0.8 or later.</span></span> <span data-ttu-id="80c11-146">Můžete ověřit hello az CLI verze s hello `az --version` příkaz.</span><span class="sxs-lookup"><span data-stu-id="80c11-146">You can verify hello az CLI version with hello `az --version` command.</span></span> <span data-ttu-id="80c11-147">hello tooupdate verze rozhraní příkazového řádku najdete v části [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="80c11-147">tooupdate hello CLI version, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="80c11-148">Použití hello `appId` hodnotu z objektu služby hello jste dříve vytvořili pro hello `aadClientId` parametr.</span><span class="sxs-lookup"><span data-stu-id="80c11-148">Use hello `appId` value from hello service principal you created earlier for hello `aadClientId` parameter.</span></span>

```azurecli 
az group deployment create --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --params \ 
        openshiftMasterPublicIpDnsLabel=myopenshiftmaster \
        infraLbPublicIpDnsLabel=myopenshiftlb \
        openshiftPassword=Pass@word!
        sshPublicKey=~/.ssh/openshift_rsa.pub \
        keyVaultResourceGroup=myResourceGroup \
        keyVaultName=myKeyVault \
        keyVaultSecret=OpenShiftKey \
        aadClientId={appId} \
        aadClientSecret={strong password} 
```
<span data-ttu-id="80c11-149">Hello nasazení může trvat až toocomplete too20 minut.</span><span class="sxs-lookup"><span data-stu-id="80c11-149">hello deployment may take up too20 minutes toocomplete.</span></span> <span data-ttu-id="80c11-150">Hello adresu URL konzoly OpenShift hello a název DNS hello OpenShift hlavní server je vytištěny toohello Terminálové po dokončení nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="80c11-150">hello URL of hello OpenShift console and DNS name of hello OpenShift master is printed toohello terminal when hello deployment completes.</span></span>

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ocpadmin@myopenshiftmaster.cloudapp.azure.com"
}
```
## <a name="connect-toohello-openshift-cluster"></a><span data-ttu-id="80c11-151">Připojte toohello OpenShift cluster</span><span class="sxs-lookup"><span data-stu-id="80c11-151">Connect toohello OpenShift cluster</span></span>
<span data-ttu-id="80c11-152">Po dokončení nasazení hello připojení konzole OpenShift toohello pomocí prohlížeče hello pomocí hello `OpenShift Console Uri`.</span><span class="sxs-lookup"><span data-stu-id="80c11-152">When hello deployment completes, connect toohello OpenShift console using hello browser using hello `OpenShift Console Uri`.</span></span> <span data-ttu-id="80c11-153">Alternativně můžete připojit toohello OpenShift hlavní server pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="80c11-153">Alternatively, you can connect toohello OpenShift master using hello following command.</span></span>

```bash
$ ssh ocpadmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a><span data-ttu-id="80c11-154">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="80c11-154">Clean up resources</span></span>
<span data-ttu-id="80c11-155">Pokud již nepotřebujete, můžete použít hello [odstranění skupiny az](/cli/azure/group#delete) příkaz skupiny prostředků hello tooremove, OpenShift clusteru a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="80c11-155">When no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, OpenShift cluster, and all related resources.</span></span>

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="80c11-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="80c11-156">Next steps</span></span>

<span data-ttu-id="80c11-157">V tento kurz, zjištěné postup:</span><span class="sxs-lookup"><span data-stu-id="80c11-157">In this tutorial, learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="80c11-158">Vytvoření klíčů SSH pro hello OpenShift cluster KeyVault toomanage.</span><span class="sxs-lookup"><span data-stu-id="80c11-158">Create a KeyVault toomanage SSH keys for hello OpenShift cluster.</span></span>
> * <span data-ttu-id="80c11-159">Nasazení clusteru OpenShift na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="80c11-159">Deploy an OpenShift cluster on Azure VMs.</span></span> 
> * <span data-ttu-id="80c11-160">Instalace a konfigurace hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="80c11-160">Install and configure hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello cluster.</span></span>

<span data-ttu-id="80c11-161">Nyní je nasazený tento OpenShift původní cluster.</span><span class="sxs-lookup"><span data-stu-id="80c11-161">Now that OpenShift Origin cluster is deployed.</span></span> <span data-ttu-id="80c11-162">Můžete postupovat podle OpenShift kurzy toolearn jak toodeploy vaší první aplikace a použití hello OpenShift nástroje.</span><span class="sxs-lookup"><span data-stu-id="80c11-162">You can follow OpenShift tutorials toolearn how toodeploy your first application and use hello OpenShift tools.</span></span> <span data-ttu-id="80c11-163">V tématu [Začínáme s OpenShift původu](https://docs.openshift.org/latest/getting_started/index.html) tooget spuštěna.</span><span class="sxs-lookup"><span data-stu-id="80c11-163">See [Getting Started with OpenShift Origin](https://docs.openshift.org/latest/getting_started/index.html) tooget started.</span></span> 
