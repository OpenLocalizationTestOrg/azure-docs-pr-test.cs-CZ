---
title: "Nasazení do Azure OpenShift počátek | Microsoft Docs"
description: "Naučte se nasadit OpenShift původ na virtuálních počítačích Azure."
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
ms.openlocfilehash: e03da05625e440eab29ccc28a2343d3433fc7607
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-openshift-origin-to-azure-virtual-machines"></a><span data-ttu-id="ed9e6-103">Nasazení OpenShift původ na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="ed9e6-103">Deploy OpenShift Origin to Azure Virtual Machines</span></span> 

<span data-ttu-id="ed9e6-104">[Původ OpenShift](https://www.openshift.org/) je kontejner platforma s otevřeným zdrojem založený na [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="ed9e6-104">[OpenShift Origin](https://www.openshift.org/) is an open source container platform built on [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="ed9e6-105">Zjednodušuje proces nasazení, škálování a provozování víceklientským aplikacím.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-105">It simplifies the process of deploying, scaling, and operating multi-tenant applications.</span></span> 

<span data-ttu-id="ed9e6-106">Tato příručka popisuje postup nasazení OpenShift původ na virtuálních počítačích Azure pomocí rozhraní příkazového řádku Azure a šablon Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-106">This guide describes how to deploy OpenShift Origin on Azure Virtual Machines using the Azure CLI and Azure Resource Manager Templates.</span></span> <span data-ttu-id="ed9e6-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="ed9e6-107">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ed9e6-108">Vytvořte KeyVault ke správě klíčů SSH OpenShift clusteru.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-108">Create a KeyVault to manage SSH keys for the OpenShift cluster.</span></span>
> * <span data-ttu-id="ed9e6-109">Nasazení clusteru OpenShift na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-109">Deploy an OpenShift cluster on Azure VMs.</span></span> 
> * <span data-ttu-id="ed9e6-110">Instalace a konfigurace [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) ke správě clusteru.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-110">Install and configure the [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) to manage the cluster.</span></span>
> * <span data-ttu-id="ed9e6-111">Přizpůsobte OpenShift nasazení.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-111">Customize the OpenShift deployment.</span></span>

<span data-ttu-id="ed9e6-112">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-112">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="ed9e6-113">Tento úvodní vyžaduje Azure CLI verze 2.0.8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-113">This quick start requires the Azure CLI version 2.0.8 or later.</span></span> <span data-ttu-id="ed9e6-114">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-114">To find the version, run `az --version`.</span></span> <span data-ttu-id="ed9e6-115">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ed9e6-115">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="log-in-to-azure"></a><span data-ttu-id="ed9e6-116">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-116">Log in to Azure</span></span> 
<span data-ttu-id="ed9e6-117">Přihlaste se k předplatnému Azure s [az přihlášení](/cli/azure/#login) příkazů a postupujte podle na obrazovce pokynů nebo klikněte na **vyzkoušet** používat cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-117">Log in to your Azure subscription with the [az login](/cli/azure/#login) command and follow the on-screen directions or click **Try it** to use Cloud Shell.</span></span>

```azurecli 
az login
```
## <a name="create-a-resource-group"></a><span data-ttu-id="ed9e6-118">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="ed9e6-118">Create a resource group</span></span>

<span data-ttu-id="ed9e6-119">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="ed9e6-119">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="ed9e6-120">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-120">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="ed9e6-121">Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-121">The following example creates a resource group named *myResourceGroup* in the *eastus* location.</span></span>

```azurecli 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-key-vault"></a><span data-ttu-id="ed9e6-122">Vytvoření trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="ed9e6-122">Create a Key Vault</span></span>
<span data-ttu-id="ed9e6-123">Vytvoření KeyVault pro ukládání klíčů SSH pro cluster s [vytvořit az keyvault](/cli/azure/keyvault#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-123">Create a KeyVault to store the SSH keys for the cluster with the [az keyvault create](/cli/azure/keyvault#create) command.</span></span>  

```azurecli 
az keyvault create --resource-group myResourceGroup --name myKeyVault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a><span data-ttu-id="ed9e6-124">Vytvoření klíče SSH</span><span class="sxs-lookup"><span data-stu-id="ed9e6-124">Create an SSH key</span></span> 
<span data-ttu-id="ed9e6-125">Klíč SSH je potřeba zabezpečit přístup k počátku OpenShift clusteru.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-125">An SSH key is needed to secure access to the OpenShift Origin cluster.</span></span> <span data-ttu-id="ed9e6-126">Vytvoření dvojice klíč SSH pomocí `ssh-keygen` příkaz.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-126">Create an SSH key-pair using the `ssh-keygen` command.</span></span> 
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> <span data-ttu-id="ed9e6-127">Pár klíčů SSH, které vytvoříte nesmí mít přístupové heslo.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-127">The SSH key pair you create must not have a passphrase.</span></span>

<span data-ttu-id="ed9e6-128">Další informace o klíče SSH v systému Windows [vytvoření SSH klíčů v systému Windows](/azure/virtual-machines/linux/ssh-from-windows).</span><span class="sxs-lookup"><span data-stu-id="ed9e6-128">For more information on SSH keys on Windows, [How to create SSH keys on Windows](/azure/virtual-machines/linux/ssh-from-windows).</span></span>

## <a name="store-ssh-private-key-in-key-vault"></a><span data-ttu-id="ed9e6-129">Uložit privátní klíč SSH v Key Vault</span><span class="sxs-lookup"><span data-stu-id="ed9e6-129">Store SSH private key in Key Vault</span></span>
<span data-ttu-id="ed9e6-130">Nasazení OpenShift používá klíč SSH, které jste vytvořili pro zabezpečený přístup k hlavnímu serveru OpenShift.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-130">The OpenShift deployment uses the SSH key you created to secure access to the OpenShift master.</span></span> <span data-ttu-id="ed9e6-131">Pokud chcete povolit nasazení tak, aby bezpečně načítat klíč SSH, uložení klíče v Key Vault, pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-131">To enable the deployment to securely retrieve the SSH key, store the key in Key Vault using the following command.</span></span>

# <a name="enabled-for-template-deployment"></a><span data-ttu-id="ed9e6-132">Povolit pro nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="ed9e6-132">Enabled for template deployment</span></span>
```azurecli
az keyvault secret set --vault-name KeyVaultName --name OpenShiftKey --file ~/.ssh/openshift.rsa
```

## <a name="create-a-service-principal"></a><span data-ttu-id="ed9e6-133">Vytvoření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="ed9e6-133">Create a service principal</span></span> 
<span data-ttu-id="ed9e6-134">OpenShift komunikuje se službou Azure pomocí uživatelského jména a hesla nebo hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-134">OpenShift communicates with Azure using a username and password or a service principal.</span></span> <span data-ttu-id="ed9e6-135">Objektu zabezpečení služby Azure je identita zabezpečení, která můžete použít s aplikací, služeb a automatizace nástroje, například OpenShift.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-135">An Azure service principal is a security identity that you can use with apps, services, and automation tools like OpenShift.</span></span> <span data-ttu-id="ed9e6-136">Můžete řídit a definovat oprávnění, jaké operace objektu služby můžete provádět v Azure.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-136">You control and define the permissions as to what operations the service principal can perform in Azure.</span></span> <span data-ttu-id="ed9e6-137">Pokud chcete zvýšit zabezpečení přes právě poskytnutí uživatelského jména a hesla, tento příklad vytvoří základní služby hlavní.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-137">To improve security over just providing a username and password, this example creates a basic service principal.</span></span>

<span data-ttu-id="ed9e6-138">Vytvoření služby hlavní s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) a výstupní přihlašovací údaje, které potřebuje OpenShift:</span><span class="sxs-lookup"><span data-stu-id="ed9e6-138">Create a service principal with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) and output the credentials that OpenShift needs:</span></span>

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {strong password} \
          --scopes $(az group show --name myResourceGroup --query id)
```
<span data-ttu-id="ed9e6-139">Poznamenejte si vlastnost appId vrácená z příkazu.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-139">Take note of the appId property returned from the command.</span></span>
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
 > <span data-ttu-id="ed9e6-140">Nevytvářejte nezabezpečené heslo.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-140">Don't create an insecure password.</span></span>  <span data-ttu-id="ed9e6-141">Postupujte podle pokynů v tématu [Pravidla a omezení pro hesla Azure AD](/azure/active-directory/active-directory-passwords-policy).</span><span class="sxs-lookup"><span data-stu-id="ed9e6-141">Follow the [Azure AD password rules and restrictions](/azure/active-directory/active-directory-passwords-policy) guidance.</span></span>

<span data-ttu-id="ed9e6-142">Další informace o objekty služby najdete v tématu [vytvořit objekt služby Azure pomocí Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)</span><span class="sxs-lookup"><span data-stu-id="ed9e6-142">For more information on service principals, see [Create an Azure service principal with Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)</span></span>

## <a name="deploy-the-openshift-origin-template"></a><span data-ttu-id="ed9e6-143">Nasazení šablony OpenShift původu</span><span class="sxs-lookup"><span data-stu-id="ed9e6-143">Deploy the OpenShift Origin template</span></span>
<span data-ttu-id="ed9e6-144">Nasaďte další OpenShift počátek pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-144">Next deploy OpenShift Origin using an Azure Resource Manager template.</span></span> 

> [!NOTE] 
> <span data-ttu-id="ed9e6-145">Tento příkaz vyžaduje az rozhraní příkazového řádku 2.0.8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-145">The following command requires az CLI 2.0.8 or later.</span></span> <span data-ttu-id="ed9e6-146">Můžete ověřit az CLI verze se `az --version` příkaz.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-146">You can verify the az CLI version with the `az --version` command.</span></span> <span data-ttu-id="ed9e6-147">K aktualizaci verze rozhraní příkazového řádku, najdete v části [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ed9e6-147">To update the CLI version, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="ed9e6-148">Použití `appId` hodnotu z objektu služby, který jste dříve vytvořili pro `aadClientId` parametr.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-148">Use the `appId` value from the service principal you created earlier for the `aadClientId` parameter.</span></span>

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
<span data-ttu-id="ed9e6-149">Nasazení může trvat až 20 minut.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-149">The deployment may take up to 20 minutes to complete.</span></span> <span data-ttu-id="ed9e6-150">Adresa URL konzoly OpenShift a název DNS je hlavní server OpenShift vytiskne na terminálu po dokončení nasazení.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-150">The URL of the OpenShift console and DNS name of the OpenShift master is printed to the terminal when the deployment completes.</span></span>

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ocpadmin@myopenshiftmaster.cloudapp.azure.com"
}
```
## <a name="connect-to-the-openshift-cluster"></a><span data-ttu-id="ed9e6-151">Připojte se ke clusteru OpenShift</span><span class="sxs-lookup"><span data-stu-id="ed9e6-151">Connect to the OpenShift cluster</span></span>
<span data-ttu-id="ed9e6-152">Po dokončení nasazení připojit ke konzole OpenShift pomocí prohlížeče `OpenShift Console Uri`.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-152">When the deployment completes, connect to the OpenShift console using the browser using the `OpenShift Console Uri`.</span></span> <span data-ttu-id="ed9e6-153">Alternativně můžete připojit k hlavnímu OpenShift pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-153">Alternatively, you can connect to the OpenShift master using the following command.</span></span>

```bash
$ ssh ocpadmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a><span data-ttu-id="ed9e6-154">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="ed9e6-154">Clean up resources</span></span>
<span data-ttu-id="ed9e6-155">Pokud již nepotřebujete, můžete použít [odstranění skupiny az](/cli/azure/group#delete) příkaz, který má-li odebrat skupinu prostředků, OpenShift clusteru a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-155">When no longer needed, you can use the [az group delete](/cli/azure/group#delete) command to remove the resource group, OpenShift cluster, and all related resources.</span></span>

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="ed9e6-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ed9e6-156">Next steps</span></span>

<span data-ttu-id="ed9e6-157">V tento kurz, zjištěné postup:</span><span class="sxs-lookup"><span data-stu-id="ed9e6-157">In this tutorial, learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="ed9e6-158">Vytvořte KeyVault ke správě klíčů SSH OpenShift clusteru.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-158">Create a KeyVault to manage SSH keys for the OpenShift cluster.</span></span>
> * <span data-ttu-id="ed9e6-159">Nasazení clusteru OpenShift na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-159">Deploy an OpenShift cluster on Azure VMs.</span></span> 
> * <span data-ttu-id="ed9e6-160">Instalace a konfigurace [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) ke správě clusteru.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-160">Install and configure the [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) to manage the cluster.</span></span>

<span data-ttu-id="ed9e6-161">Nyní je nasazený tento OpenShift původní cluster.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-161">Now that OpenShift Origin cluster is deployed.</span></span> <span data-ttu-id="ed9e6-162">Zjistěte, jak nasadit svoji první aplikaci a používat nástroje OpenShift můžete výukové OpenShift.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-162">You can follow OpenShift tutorials to learn how to deploy your first application and use the OpenShift tools.</span></span> <span data-ttu-id="ed9e6-163">V tématu [Začínáme s OpenShift původu](https://docs.openshift.org/latest/getting_started/index.html) začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="ed9e6-163">See [Getting Started with OpenShift Origin](https://docs.openshift.org/latest/getting_started/index.html) to get started.</span></span> 
