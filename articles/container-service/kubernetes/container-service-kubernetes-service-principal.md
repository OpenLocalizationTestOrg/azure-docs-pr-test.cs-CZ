---
title: "objekt zabezpečení aaaService pro cluster Azure Kubernetes | Microsoft Docs"
description: "Vytvoření a správa instančního objektu služby Azure Active Directory pro cluster Kubernetes v Azure Container Service"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 7a01624c5ac3fa717dbcbd570e05ceb4d917c53a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-an-azure-ad-service-principal-for-a-kubernetes-cluster-in-container-service"></a><span data-ttu-id="63401-103">Nastavení instančního objektu služby Azure AD pro cluster Kubernetes ve službě Container Service</span><span class="sxs-lookup"><span data-stu-id="63401-103">Set up an Azure AD service principal for a Kubernetes cluster in Container Service</span></span>


<span data-ttu-id="63401-104">V Azure Container Service vyžaduje Kubernetes cluster [objektu služby Azure Active Directory](../../active-directory/develop/active-directory-application-objects.md) toointeract s rozhraními API Azure.</span><span class="sxs-lookup"><span data-stu-id="63401-104">In Azure Container Service, a Kubernetes cluster requires an [Azure Active Directory service principal](../../active-directory/develop/active-directory-application-objects.md) toointeract with Azure APIs.</span></span> <span data-ttu-id="63401-105">Hello instanční objekt je potřeba toodynamically spravovat prostředky, jako [trasy definované uživatelem](../../virtual-network/virtual-networks-udr-overview.md) a hello [pro vyrovnávání zatížení vrstvy 4 Azure](../../load-balancer/load-balancer-overview.md).</span><span class="sxs-lookup"><span data-stu-id="63401-105">hello service principal is needed toodynamically manage resources such as [user-defined routes](../../virtual-network/virtual-networks-udr-overview.md) and hello [Layer 4 Azure Load Balancer](../../load-balancer/load-balancer-overview.md).</span></span> 


<span data-ttu-id="63401-106">Tento článek popisuje různé možnosti tooset až služba hlavní Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="63401-106">This article shows different options tooset up a service principal for your Kubernetes cluster.</span></span> <span data-ttu-id="63401-107">Například, pokud jste nainstalovali ale nastavení hello [Azure CLI 2.0](/cli/azure/install-az-cli2), můžete spustit hello [ `az acs create` ](/cli/azure/acs#create) příkaz toocreate hello Kubernetes clusteru a hello instančního objektu v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="63401-107">For example, if you installed and set up hello [Azure CLI 2.0](/cli/azure/install-az-cli2), you can run hello [`az acs create`](/cli/azure/acs#create) command toocreate hello Kubernetes cluster and hello service principal at hello same time.</span></span>


## <a name="requirements-for-hello-service-principal"></a><span data-ttu-id="63401-108">Požadavky pro hello instančního objektu</span><span class="sxs-lookup"><span data-stu-id="63401-108">Requirements for hello service principal</span></span>

<span data-ttu-id="63401-109">Můžete použít existující Azure AD instanční objekt zda splňuje hello následující požadavky, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="63401-109">You can use an existing Azure AD service principal that meets hello following requirements, or create a new one.</span></span>

* <span data-ttu-id="63401-110">**Obor**: hello skupiny prostředků v předplatném hello použít toodeploy hello Kubernetes clusteru nebo (méně restriktivně) hello předplatné použít toodeploy hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="63401-110">**Scope**: hello resource group in hello subscription used toodeploy hello Kubernetes cluster, or (less restrictively) hello subscription used toodeploy hello cluster.</span></span>

* <span data-ttu-id="63401-111">**Role:****Přispěvatel**</span><span class="sxs-lookup"><span data-stu-id="63401-111">**Role**: **Contributor**</span></span>

* <span data-ttu-id="63401-112">**Tajný kód klienta:** Musí se jednat o heslo.</span><span class="sxs-lookup"><span data-stu-id="63401-112">**Client secret**: must be a password.</span></span> <span data-ttu-id="63401-113">V současné době nemůžete použít instanční objekt nastavený pro ověření certifikátu.</span><span class="sxs-lookup"><span data-stu-id="63401-113">Currently, you can't use a service principal set up for certificate authentication.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="63401-114">toocreate instančního objektu, musíte mít oprávnění tooregister aplikace pomocí klienta Azure AD a tooassign hello aplikace tooa role v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="63401-114">toocreate a service principal, you must have permissions tooregister an application with your Azure AD tenant, and tooassign hello application tooa role in your subscription.</span></span> <span data-ttu-id="63401-115">toosee, pokud máte hello požadované oprávnění [změnami hello portál](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions).</span><span class="sxs-lookup"><span data-stu-id="63401-115">toosee if you have hello required permissions, [check in hello Portal](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions).</span></span> 
>

## <a name="option-1-create-a-service-principal-in-azure-ad"></a><span data-ttu-id="63401-116">Možnost 1: Vytvoření instančního objektu v Azure AD</span><span class="sxs-lookup"><span data-stu-id="63401-116">Option 1: Create a service principal in Azure AD</span></span>

<span data-ttu-id="63401-117">Pokud před nasazením clusteru Kubernetes chcete toocreate objektu služby Azure AD, Azure poskytuje několik metod.</span><span class="sxs-lookup"><span data-stu-id="63401-117">If you want toocreate an Azure AD service principal before you deploy your Kubernetes cluster, Azure provides several methods.</span></span> 

<span data-ttu-id="63401-118">Následující příklady příkazů Hello ukazují, jak toodo o hello [Azure CLI 2.0](../../azure-resource-manager/resource-group-authenticate-service-principal-cli.md).</span><span class="sxs-lookup"><span data-stu-id="63401-118">hello following example commands show you how toodo this with hello [Azure CLI 2.0](../../azure-resource-manager/resource-group-authenticate-service-principal-cli.md).</span></span> <span data-ttu-id="63401-119">Případně můžete vytvořit službu objektu zabezpečení pomocí [prostředí Azure PowerShell](../../azure-resource-manager/resource-group-authenticate-service-principal.md), hello [portál](../../azure-resource-manager/resource-group-create-service-principal-portal.md), nebo jiné metody.</span><span class="sxs-lookup"><span data-stu-id="63401-119">You can alternatively create a service principal using [Azure PowerShell](../../azure-resource-manager/resource-group-authenticate-service-principal.md), hello [portal](../../azure-resource-manager/resource-group-create-service-principal-portal.md), or other methods.</span></span>

```azurecli
az login

az account set --subscription "mySubscriptionID"

az group create -n "myResourceGroupName" -l "westus"

az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/mySubscriptionID/resourceGroups/myResourceGroupName"
```

<span data-ttu-id="63401-120">Výstup se podobně jako následující toohello (tady zobrazené zredigované):</span><span class="sxs-lookup"><span data-stu-id="63401-120">Output is similar toohello following (shown here redacted):</span></span>

![Vytvoření instančního objektu](./media/container-service-kubernetes-service-principal/service-principal-creds.png)

<span data-ttu-id="63401-122">Zvýrazněná jsou hello **ID klienta** (`appId`) a hello **tajný klíč klienta** (`password`), použít jako parametry hlavní služby pro nasazení clusteru.</span><span class="sxs-lookup"><span data-stu-id="63401-122">Highlighted are hello **client ID** (`appId`) and hello **client secret** (`password`) that you use as service principal parameters for cluster deployment.</span></span>


### <a name="specify-service-principal-when-creating-hello-kubernetes-cluster"></a><span data-ttu-id="63401-123">Při vytváření clusteru Kubernetes hello zadat instančního objektu</span><span class="sxs-lookup"><span data-stu-id="63401-123">Specify service principal when creating hello Kubernetes cluster</span></span>

<span data-ttu-id="63401-124">Zadejte hello **ID klienta** (také nazývané hello `appId`, pro ID aplikace) a **tajný klíč klienta** (`password`) z existující službu hlavní jako parametry při vytváření hello Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="63401-124">Provide hello **client ID** (also called hello `appId`, for Application ID) and **client secret** (`password`) of an existing service principal as parameters when you create hello Kubernetes cluster.</span></span> <span data-ttu-id="63401-125">Ujistěte se, že hello instanční objekt splňuje požadavky hello v hello od tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="63401-125">Make sure hello service principal meets hello requirements at hello beginning this article.</span></span>

<span data-ttu-id="63401-126">Tyto parametry můžete zadat při nasazování clusteru Kubernetes hello pomocí hello [rozhraní příkazového řádku Azure (CLI) 2.0](container-service-kubernetes-walkthrough.md), [portál Azure](../dcos-swarm/container-service-deployment.md), nebo jiné metody.</span><span class="sxs-lookup"><span data-stu-id="63401-126">You can specify these parameters when deploying hello Kubernetes cluster using hello [Azure Command-Line Interface (CLI) 2.0](container-service-kubernetes-walkthrough.md), [Azure portal](../dcos-swarm/container-service-deployment.md), or other methods.</span></span>

>[!TIP] 
><span data-ttu-id="63401-127">Při zadávání hello **ID klienta**, se, zda text hello toouse `appId`, není hello `ObjectId`, z hello instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="63401-127">When specifying hello **client ID**, be sure toouse hello `appId`, not hello `ObjectId`, of hello service principal.</span></span>
>

<span data-ttu-id="63401-128">Hello následující příklad ukazuje jeden ze způsobů toopass hello parametry s hello 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="63401-128">hello following example shows one way toopass hello parameters with hello Azure CLI 2.0.</span></span> <span data-ttu-id="63401-129">Tento příklad používá hello [šablony rychlý start Kubernetes](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes).</span><span class="sxs-lookup"><span data-stu-id="63401-129">This example uses hello [Kubernetes quickstart template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes).</span></span>

1. <span data-ttu-id="63401-130">[Stáhněte si](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json) soubor parametrů šablony hello `azuredeploy.parameters.json` z Githubu.</span><span class="sxs-lookup"><span data-stu-id="63401-130">[Download](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json) hello template parameters file `azuredeploy.parameters.json` from GitHub.</span></span>

2. <span data-ttu-id="63401-131">Služba hello toospecify hlavní, zadejte hodnoty pro `servicePrincipalClientId` a `servicePrincipalClientSecret` v souboru hello.</span><span class="sxs-lookup"><span data-stu-id="63401-131">toospecify hello service principal, enter values for `servicePrincipalClientId` and `servicePrincipalClientSecret` in hello file.</span></span> <span data-ttu-id="63401-132">(Je také nutné tooprovide vlastních hodnot pro `dnsNamePrefix` a `sshRSAPublicKey`.</span><span class="sxs-lookup"><span data-stu-id="63401-132">(You also need tooprovide your own values for `dnsNamePrefix` and `sshRSAPublicKey`.</span></span> <span data-ttu-id="63401-133">Hello druhé je hello SSH veřejného klíče tooaccess hello cluster). Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="63401-133">hello latter is hello SSH public key tooaccess hello cluster.) Save hello file.</span></span>

    ![Předání parametrů instančního objektu](./media/container-service-kubernetes-service-principal/service-principal-params.png)

3. <span data-ttu-id="63401-135">Hello spusťte následující příkaz, pomocí `--parameters` tooset hello cestě toohello azuredeploy.parameters.json souboru.</span><span class="sxs-lookup"><span data-stu-id="63401-135">Run hello following command, using `--parameters` tooset hello path toohello azuredeploy.parameters.json file.</span></span> <span data-ttu-id="63401-136">Tento příkaz nasadí hello clusteru ve skupině prostředků vytvoříte volané `myResourceGroup` v oblasti západní USA hello.</span><span class="sxs-lookup"><span data-stu-id="63401-136">This command deploys hello cluster in a resource group you create called `myResourceGroup` in hello West US region.</span></span>

    ```azurecli
    az login

    az account set --subscription "mySubscriptionID"

    az group create --name "myResourceGroup" --location "westus" 
    
    az group deployment create -g "myResourceGroup" --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.json" --parameters @azuredeploy.parameters.json
    ```


## <a name="option-2-generate-a-service-principal-when-creating-hello-cluster-with-az-acs-create"></a><span data-ttu-id="63401-137">Možnost 2: Vygenerování hlavní název služby, při vytváření clusteru hello s`az acs create`</span><span class="sxs-lookup"><span data-stu-id="63401-137">Option 2: Generate a service principal when creating hello cluster with `az acs create`</span></span>

<span data-ttu-id="63401-138">Pokud spustíte hello [ `az acs create` ](/cli/azure/acs#create) toocreate příkaz hello Kubernetes clusteru, máte možnost toogenerate hello objekt služby automaticky.</span><span class="sxs-lookup"><span data-stu-id="63401-138">If you run hello [`az acs create`](/cli/azure/acs#create) command toocreate hello Kubernetes cluster, you have hello option toogenerate a service principal automatically.</span></span>

<span data-ttu-id="63401-139">Stejně jako u ostatních možností vytvoření clusteru Kubernetes můžete při spuštění příkazu `az acs create` určit parametry pro existující instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="63401-139">As with other Kubernetes cluster creation options, you can specify parameters for an existing service principal when you run `az acs create`.</span></span> <span data-ttu-id="63401-140">Ale pokud vynecháte tyto parametry, hello rozhraní příkazového řádku Azure vytvoří automaticky pro použití s Container Service.</span><span class="sxs-lookup"><span data-stu-id="63401-140">However, when you omit these parameters, hello Azure CLI creates one automatically for use with Container Service.</span></span> <span data-ttu-id="63401-141">To probíhá transparentně během nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="63401-141">This takes place transparently during hello deployment.</span></span> 

<span data-ttu-id="63401-142">Hello následující příkaz vytvoří Kubernetes cluster a vygeneruje klíčů SSH a pověření hlavní služby:</span><span class="sxs-lookup"><span data-stu-id="63401-142">hello following command creates a Kubernetes cluster and generates both SSH keys and service principal credentials:</span></span>

```console
az acs create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys --orchestrator-type kubernetes
```

> [!IMPORTANT]
> <span data-ttu-id="63401-143">Pokud váš účet nemá hello Azure AD a předplatné oprávnění toocreate hlavní název služby, hello příkaz vygeneruje chybu podobné příliš`Insufficient privileges toocomplete hello operation.`</span><span class="sxs-lookup"><span data-stu-id="63401-143">If your account doesn't have hello Azure AD and subscription permissions toocreate a service principal, hello command generates an error similar too`Insufficient privileges toocomplete hello operation.`</span></span>
> 

## <a name="additional-considerations"></a><span data-ttu-id="63401-144">Další aspekty</span><span class="sxs-lookup"><span data-stu-id="63401-144">Additional considerations</span></span>

* <span data-ttu-id="63401-145">Pokud nemáte oprávnění toocreate objekt služby v rámci vašeho předplatného, bude pravděpodobně nutné tooask služby Azure AD nebo předplatné správce tooassign hello potřebná oprávnění, nebo požádejte je o hlavní toouse s Azure Container Service služby.</span><span class="sxs-lookup"><span data-stu-id="63401-145">If you don't have permissions toocreate a service principal in your subscription, you might need tooask your Azure AD or subscription administrator tooassign hello necessary permissions, or ask them for a service principal toouse with Azure Container Service.</span></span> 

* <span data-ttu-id="63401-146">Hello instanční objekt pro Kubernetes je součástí konfigurace clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="63401-146">hello service principal for Kubernetes is a part of hello cluster configuration.</span></span> <span data-ttu-id="63401-147">Nepoužívejte však hello identity toodeploy hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="63401-147">However, don't use hello identity toodeploy hello cluster.</span></span>

* <span data-ttu-id="63401-148">Každý instanční objekt je přidružený k aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="63401-148">Every service principal is associated with an Azure AD application.</span></span> <span data-ttu-id="63401-149">Hello objekt služby pro cluster s podporou Kubernetes mohou být přidruženy žádné platné Azure AD název aplikace (například: `https://www.contoso.org/example`).</span><span class="sxs-lookup"><span data-stu-id="63401-149">hello service principal for a Kubernetes cluster can be associated with any valid Azure AD application name (for example: `https://www.contoso.org/example`).</span></span> <span data-ttu-id="63401-150">Hello adresa URL pro aplikaci hello nemá toobe skutečné koncový bod.</span><span class="sxs-lookup"><span data-stu-id="63401-150">hello URL for hello application doesn't have toobe a real endpoint.</span></span>

* <span data-ttu-id="63401-151">Při určování hello instanční objekt **ID klienta**, můžete použít hodnotu hello hello `appId` (jak je uvedeno v tomto článku) nebo hello odpovídající instanční objekt `name` (například`https://www.contoso.org/example`).</span><span class="sxs-lookup"><span data-stu-id="63401-151">When specifying hello service principal **Client ID**, you can use hello value of hello `appId` (as shown in this article) or hello corresponding service principal `name` (for example,`https://www.contoso.org/example`).</span></span>

* <span data-ttu-id="63401-152">Na hlavní server hello a agenta virtuálních počítačů v clusteru Kubernetes hello hlavní přihlašovací údaje služby hello ukládají v souboru /etc/kubernetes/azure.json hello.</span><span class="sxs-lookup"><span data-stu-id="63401-152">On hello master and agent VMs in hello Kubernetes cluster, hello service principal credentials are stored in hello file /etc/kubernetes/azure.json.</span></span>

* <span data-ttu-id="63401-153">Při použití hello `az acs create` příkaz toogenerate hello instanční objekt automaticky, hlavní přihlašovací údaje služby hello se zapisují toohello ~/.azure/acsServicePrincipal.json soubor na počítači hello používá příkaz toorun hello.</span><span class="sxs-lookup"><span data-stu-id="63401-153">When you use hello `az acs create` command toogenerate hello service principal automatically, hello service principal credentials are written toohello file ~/.azure/acsServicePrincipal.json on hello machine used toorun hello command.</span></span> 

* <span data-ttu-id="63401-154">Při použití hello `az acs create` příkaz toogenerate hello instanční objekt automaticky, hello instanční objekt lze také ověřovat pomocí [kontejner Azure registru](../../container-registry/container-registry-intro.md) vytvořené v hello stejné předplatné.</span><span class="sxs-lookup"><span data-stu-id="63401-154">When you use hello `az acs create` command toogenerate hello service principal automatically, hello service principal can also authenticate with an [Azure container registry](../../container-registry/container-registry-intro.md) created in hello same subscription.</span></span>




## <a name="next-steps"></a><span data-ttu-id="63401-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="63401-155">Next steps</span></span>

* <span data-ttu-id="63401-156">[Začněte používat Kubernetes](container-service-kubernetes-walkthrough.md) v clusteru služby kontejneru.</span><span class="sxs-lookup"><span data-stu-id="63401-156">[Get started with Kubernetes](container-service-kubernetes-walkthrough.md) in your container service cluster.</span></span>

* <span data-ttu-id="63401-157">tootroubleshoot hello instanční objekt pro Kubernetes, najdete v části hello [ACS modul dokumentaci](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes.md#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="63401-157">tootroubleshoot hello service principal for Kubernetes, see hello [ACS Engine documentation](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes.md#troubleshooting).</span></span>


