---
title: aaaDeploy kontejner Docker cluster v Azure | Microsoft Docs
description: "Nasazení řešení Kubernetes, DC/OS nebo Docker Swarm v Azure Container Service pomocí hello portál Azure nebo šablony Resource Manageru."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kontejnery, Mikroslužby, Mesos, Azure, dcos, swarm, kubernetes, azure container service, acs"
ms.service: container-service
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: rogardle
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 26e3a7d0af9d71acd8b5c85fd667fcf7d84cef66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-docker-container-hosting-solution-using-hello-azure-portal"></a><span data-ttu-id="ecc24-104">Nasazení řešení pomocí portálu Azure hello hostování kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="ecc24-104">Deploy a Docker container hosting solution using hello Azure portal</span></span>



<span data-ttu-id="ecc24-105">Azure Container Service umožňuje rychlé nasazení oblíbených open-source řešení pro clustering a orchestraci kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="ecc24-105">Azure Container Service provides rapid deployment of popular open-source container clustering and orchestration solutions.</span></span> <span data-ttu-id="ecc24-106">Tento dokument vás provede nasazením clusteru Azure Container Service pomocí hello portál Azure nebo rychlý start šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ecc24-106">This document walks you through deploying an Azure Container Service cluster by using hello Azure portal or an Azure Resource Manager quickstart template.</span></span> 

<span data-ttu-id="ecc24-107">Můžete také nasazení clusteru Azure Container Service pomocí hello [Azure CLI 2.0](container-service-create-acs-cluster-cli.md) nebo hello rozhraní API Správce Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="ecc24-107">You can also deploy an Azure Container Service cluster by using hello [Azure CLI 2.0](container-service-create-acs-cluster-cli.md) or hello Azure Container Service APIs.</span></span>

<span data-ttu-id="ecc24-108">Související informace najdete v tématu [Úvod do služby Azure Container Service](../container-service-intro.md).</span><span class="sxs-lookup"><span data-stu-id="ecc24-108">For background, see [Azure Container Service introduction](../container-service-intro.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="ecc24-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ecc24-109">Prerequisites</span></span>

* <span data-ttu-id="ecc24-110">**Předplatné Azure:** Pokud žádné nemáte, můžete se zaregistrovat k [bezplatné zkušební verzi](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).</span><span class="sxs-lookup"><span data-stu-id="ecc24-110">**Azure subscription**: If you don't have one, sign up for a [free trial](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).</span></span> <span data-ttu-id="ecc24-111">Pro větší cluster zvažte předplatné s průběžnými platbami nebo jiné možnosti nákupu.</span><span class="sxs-lookup"><span data-stu-id="ecc24-111">For a larger cluster, consider a pay-as-you go subscription or other purchase options.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ecc24-112">Využití vašeho předplatného Azure a [kvótou prostředků](../../azure-subscription-service-limits.md), jako je například kvóty jader, můžete omezit hello velikost clusteru hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="ecc24-112">Your Azure subscription usage and [resource quotas](../../azure-subscription-service-limits.md), such as cores quotas, can limit hello size of hello cluster you deploy.</span></span> <span data-ttu-id="ecc24-113">toorequest zvýšení kvóty, otevřete [žádost o podporu online zákazníka](../../azure-supportability/how-to-create-azure-support-request.md) zdarma.</span><span class="sxs-lookup"><span data-stu-id="ecc24-113">toorequest a quota increase, open an [online customer support request](../../azure-supportability/how-to-create-azure-support-request.md) at no charge.</span></span>
    >

* <span data-ttu-id="ecc24-114">**Veřejný klíč SSH RSA**: Pokud nasazujete prostřednictvím portálu hello nebo jeden z šablony Azure rychlý start hello, potřebujete tooprovide hello veřejný klíč pro ověřování před virtuálními počítači Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="ecc24-114">**SSH RSA public key**: When deploying through hello portal or one of hello Azure quickstart templates, you need tooprovide hello public key for authentication against Azure Container Service virtual machines.</span></span> <span data-ttu-id="ecc24-115">toocreate klíče RSA Secure Shell (SSH), najdete v části hello [OS X a Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) nebo [Windows](../../virtual-machines/linux/ssh-from-windows.md) pokyny.</span><span class="sxs-lookup"><span data-stu-id="ecc24-115">toocreate Secure Shell (SSH) RSA keys, see hello [OS X and Linux](../../virtual-machines/linux/mac-create-ssh-keys.md) or [Windows](../../virtual-machines/linux/ssh-from-windows.md) guidance.</span></span> 

* <span data-ttu-id="ecc24-116">**Služba ID objektu zabezpečení klienta a tajný klíč** (pouze Kubernetes): Další informace a pokyny toocreate objektu služby Azure Active Directory, naleznete v části [o hello instanční objekt pro cluster s podporou Kubernetes](../kubernetes/container-service-kubernetes-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="ecc24-116">**Service principal client ID and secret** (Kubernetes only): For more information and guidance toocreate an Azure Active Directory service principal, see [About hello service principal for a Kubernetes cluster](../kubernetes/container-service-kubernetes-service-principal.md).</span></span>



## <a name="create-a-cluster-by-using-hello-azure-portal"></a><span data-ttu-id="ecc24-117">Vytvoření clusteru pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ecc24-117">Create a cluster by using hello Azure portal</span></span>
1. <span data-ttu-id="ecc24-118">Vyberte přihlašovací toohello portál Azure, **nový**a hledání hello Azure Marketplace pro **Azure Container Service**.</span><span class="sxs-lookup"><span data-stu-id="ecc24-118">Sign in toohello Azure portal, select **New**, and search hello Azure Marketplace for **Azure Container Service**.</span></span>

    ![Azure Container Service na Marketplace](./media/container-service-deployment/acs-portal1.png)  <br />

2. <span data-ttu-id="ecc24-120">Klikněte na **Azure Container Service** a potom na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ecc24-120">Click **Azure Container Service**, and click **Create**.</span></span>

3. <span data-ttu-id="ecc24-121">Na hello **Základy** okno, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="ecc24-121">On hello **Basics** blade, enter hello following information:</span></span>

    * <span data-ttu-id="ecc24-122">**Orchestrator**: vyberte jednu z hello kontejneru orchestrators toodeploy v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-122">**Orchestrator**: Select one of hello container orchestrators toodeploy on hello cluster.</span></span>
        * <span data-ttu-id="ecc24-123">**DC/OS:** Nasadí cluster DC/OS.</span><span class="sxs-lookup"><span data-stu-id="ecc24-123">**DC/OS**: Deploys a DC/OS cluster.</span></span>
        * <span data-ttu-id="ecc24-124">**Swarm:** Nasadí cluster Docker Swarm.</span><span class="sxs-lookup"><span data-stu-id="ecc24-124">**Swarm**: Deploys a Docker Swarm cluster.</span></span>
        * <span data-ttu-id="ecc24-125">**Kubernetes**: Nasadí cluster Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="ecc24-125">**Kubernetes**: Deploys a Kubernetes cluster.</span></span>
    * <span data-ttu-id="ecc24-126">**Předplatné:** Vyberte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="ecc24-126">**Subscription**: Select an Azure subscription.</span></span>
    * <span data-ttu-id="ecc24-127">**Skupina prostředků**: Zadejte název hello novou skupinu prostředků pro nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-127">**Resource group**: Enter hello name of a new resource group for hello deployment.</span></span>
    * <span data-ttu-id="ecc24-128">**Umístění**: Vyberte oblast Azure pro nasazení Azure Container Service hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-128">**Location**: Select an Azure region for hello Azure Container Service deployment.</span></span> <span data-ttu-id="ecc24-129">Informace o dostupnosti najdete v tématu [Dostupné produkty v jednotlivých oblastech](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="ecc24-129">For availability, check [Products available by region](https://azure.microsoft.com/regions/services/).</span></span>
    
    ![Základní nastavení](./media/container-service-deployment/acs-portal3.png)  <br />
    
    <span data-ttu-id="ecc24-131">Klikněte na tlačítko **OK** po tooproceed připraven.</span><span class="sxs-lookup"><span data-stu-id="ecc24-131">Click **OK** when you're ready tooproceed.</span></span>

4. <span data-ttu-id="ecc24-132">Na hello **hlavní konfigurace** okno, zadejte následující nastavení pro hlavní uzel hello Linux nebo uzly v clusteru hello (některá nastavení jsou konkrétní tooeach orchestrator) hello:</span><span class="sxs-lookup"><span data-stu-id="ecc24-132">On hello **Master configuration** blade, enter hello following settings for hello Linux master node or nodes in hello cluster (some settings are specific tooeach orchestrator):</span></span>

    * <span data-ttu-id="ecc24-133">**Název DNS hlavní**: toocreate Předpona použitá hello jedinečný plně kvalifikovaný název domény (FQDN) pro hlavní server hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-133">**Master DNS name**: hello prefix used toocreate a unique fully qualified domain name (FQDN) for hello master.</span></span> <span data-ttu-id="ecc24-134">Hello hlavní plně kvalifikovaný název domény má podobu hello *předponu*správu*umístění*. cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="ecc24-134">hello master FQDN is of hello form *prefix*mgmt.*location*.cloudapp.azure.com.</span></span>
    * <span data-ttu-id="ecc24-135">**Uživatelské jméno**: hello uživatelské jméno pro účet na všech virtuálních počítačů Linux hello v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-135">**User name**: hello user name for an account on each of hello Linux virtual machines in hello cluster.</span></span>
    * <span data-ttu-id="ecc24-136">**Veřejný klíč SSH RSA**: přidejte veřejný klíč toobe hello používá k ověřování pro virtuální počítače s Linuxem hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-136">**SSH RSA public key**: Add hello public key toobe used for authentication against hello Linux virtual machines.</span></span> <span data-ttu-id="ecc24-137">Je důležité, aby tento klíč neobsahoval žádné konce řádků a obsahuje hello `ssh-rsa` předponu.</span><span class="sxs-lookup"><span data-stu-id="ecc24-137">It is important that this key contains no line breaks, and it includes hello `ssh-rsa` prefix.</span></span> <span data-ttu-id="ecc24-138">Hello `username@domain` operátory je volitelný.</span><span class="sxs-lookup"><span data-stu-id="ecc24-138">hello `username@domain` postfix is optional.</span></span> <span data-ttu-id="ecc24-139">Hello klíč by měl vypadat podobně jako následující hello: **ssh-rsa AAAAB3Nz... <>...... UcyupgH azureuser@linuxvm** .</span><span class="sxs-lookup"><span data-stu-id="ecc24-139">hello key should look something like hello following: **ssh-rsa AAAAB3Nz...<...>...UcyupgH azureuser@linuxvm**.</span></span> 
    * <span data-ttu-id="ecc24-140">**Instanční objekt**: Pokud jste vybrali hello Kubernetes orchestrator, zadejte Azure Active Directory **služby ID objektu zabezpečení klienta** (také nazývané hello appId) a **služby hlavní tajný klíč klienta pro** (heslo).</span><span class="sxs-lookup"><span data-stu-id="ecc24-140">**Service principal**: If you selected hello Kubernetes orchestrator, enter an Azure Active Directory **Service principal client ID** (also called hello appId) and **Service principal client secret** (password).</span></span> <span data-ttu-id="ecc24-141">Další informace najdete v tématu [o hello instanční objekt pro cluster s podporou Kubernetes](../kubernetes/container-service-kubernetes-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="ecc24-141">For more information, see [About hello service principal for a Kubernetes cluster](../kubernetes/container-service-kubernetes-service-principal.md).</span></span>
    * <span data-ttu-id="ecc24-142">**Hlavní počet**: počet hlavních serverů v clusteru hello hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-142">**Master count**: hello number of masters in hello cluster.</span></span>
    * <span data-ttu-id="ecc24-143">**Diagnostika virtuálních počítačů**: pro některé orchestrators, můžete povolit Diagnostika virtuálních počítačů na hello hlavních serverů.</span><span class="sxs-lookup"><span data-stu-id="ecc24-143">**VM diagnostics**: For some orchestrators, you can enable VM diagnostics on hello masters.</span></span>

    ![Konfigurace hlavních uzlů](./media/container-service-deployment/acs-portal4.png)  <br />

    <span data-ttu-id="ecc24-145">Klikněte na tlačítko **OK** po tooproceed připraven.</span><span class="sxs-lookup"><span data-stu-id="ecc24-145">Click **OK** when you're ready tooproceed.</span></span>

5. <span data-ttu-id="ecc24-146">Na hello **konfigurace agenta** okno, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="ecc24-146">On hello **Agent configuration** blade, enter hello following information:</span></span>

    * <span data-ttu-id="ecc24-147">**Počet agentů**: pro Docker Swarm a Kubernetes, tato hodnota je počáteční počet agentů v sadě škálování agentů hello hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-147">**Agent count**: For Docker Swarm and Kubernetes, this value is hello initial number of agents in hello agent scale set.</span></span> <span data-ttu-id="ecc24-148">Pro DC/OS je počáteční počet agentů v privátní sadě škálování hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-148">For DC/OS, it is hello initial number of agents in a private scale set.</span></span> <span data-ttu-id="ecc24-149">Kromě toho se pro DC/OS vytvoří škálovací sada obsahující předem určený počet agentů.</span><span class="sxs-lookup"><span data-stu-id="ecc24-149">Additionally, a public scale set is created for DC/OS, which contains a predetermined number of agents.</span></span> <span data-ttu-id="ecc24-150">Hello počet agentů v této veřejné sadě škálování je dáno hello počet hlavních serverů v clusteru hello: jeden veřejný agent pro jeden z nich a dvě veřejné agentů pro tři nebo pět hlavních serverů.</span><span class="sxs-lookup"><span data-stu-id="ecc24-150">hello number of agents in this public scale set is determined by hello number of masters in hello cluster: one public agent for one master, and two public agents for three or five masters.</span></span>
    * <span data-ttu-id="ecc24-151">**Velikost virtuálního počítače Agent**: hello velikost hello agenta virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ecc24-151">**Agent virtual machine size**: hello size of hello agent virtual machines.</span></span>
    * <span data-ttu-id="ecc24-152">**Operační systém**: Toto nastavení je aktuálně k dispozici pouze v případě, že jste vybrali hello Kubernetes orchestrator.</span><span class="sxs-lookup"><span data-stu-id="ecc24-152">**Operating system**: This setting is currently available only if you selected hello Kubernetes orchestrator.</span></span> <span data-ttu-id="ecc24-153">Zvolte distribuční Linux nebo toorun operační systém Windows Server na agentech hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-153">Choose either a Linux distribution or a Windows Server operating system toorun on hello agents.</span></span> <span data-ttu-id="ecc24-154">Toto nastavení určuje, jestli v clusteru bude možné spouštět aplikace typu kontejner Linux nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="ecc24-154">This setting determines whether your cluster can run Linux or Windows container apps.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="ecc24-155">Podpora kontejnerů Windows je pro clustery Kubernetes ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="ecc24-155">Windows container support is in preview for Kubernetes clusters.</span></span> <span data-ttu-id="ecc24-156">V clusterech DC/OS a Swarm jsou v současné době ve službě Azure Container Service podporováni pouze linuxoví agenti.</span><span class="sxs-lookup"><span data-stu-id="ecc24-156">On DC/OS and Swarm clusters, only Linux agents are currently supported in Azure Container Service.</span></span>

    * <span data-ttu-id="ecc24-157">**Přihlašovací údaje agenta**: Pokud jste vybrali operační systém Windows hello, zadejte správce **uživatelské jméno** a **heslo** pro agenta hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ecc24-157">**Agent credentials**: If you selected hello Windows operating system, enter an administrator **User name** and **Password** for hello agent VMs.</span></span> 

    ![Konfigurace agentů](./media/container-service-deployment/acs-portal5.png)  <br />

    <span data-ttu-id="ecc24-159">Klikněte na tlačítko **OK** po tooproceed připraven.</span><span class="sxs-lookup"><span data-stu-id="ecc24-159">Click **OK** when you're ready tooproceed.</span></span>

6. <span data-ttu-id="ecc24-160">Až se dokončí ověření služby, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ecc24-160">After service validation finishes, click **OK**.</span></span>

    ![Ověření](./media/container-service-deployment/acs-portal6.png)  <br />

7. <span data-ttu-id="ecc24-162">Přečtěte si podmínky hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-162">Review hello terms.</span></span> <span data-ttu-id="ecc24-163">proces nasazení hello toostart, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ecc24-163">toostart hello deployment process, click **Create**.</span></span>

    <span data-ttu-id="ecc24-164">Pokud jste zvolen toopin hello nasazení toohello portálu Azure, uvidíte stav nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-164">If you've elected toopin hello deployment toohello Azure portal, you can see hello deployment status.</span></span>

    ![Stav nasazení](./media/container-service-deployment/acs-portal8.png)  <br />

<span data-ttu-id="ecc24-166">nasazení Hello trvá několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="ecc24-166">hello deployment takes several minutes toocomplete.</span></span> <span data-ttu-id="ecc24-167">Potom clusteru Azure Container Service hello je připravený k použití.</span><span class="sxs-lookup"><span data-stu-id="ecc24-167">Then, hello Azure Container Service cluster is ready for use.</span></span>


## <a name="create-a-cluster-by-using-a-quickstart-template"></a><span data-ttu-id="ecc24-168">Vytvoření clusteru pomocí šablony pro rychlý start</span><span class="sxs-lookup"><span data-stu-id="ecc24-168">Create a cluster by using a quickstart template</span></span>
<span data-ttu-id="ecc24-169">Šablony Azure rychlý Start jsou k dispozici toodeploy cluster Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="ecc24-169">Azure quickstart templates are available toodeploy a cluster in Azure Container Service.</span></span> <span data-ttu-id="ecc24-170">Hello, pokud šablony rychlý start může být upravený tooinclude další nebo pokročilá konfigurace Azure.</span><span class="sxs-lookup"><span data-stu-id="ecc24-170">hello provided quickstart templates can be modified tooinclude additional or advanced Azure configuration.</span></span> <span data-ttu-id="ecc24-171">toocreate clusteru Azure Container Service pomocí šablony Azure rychlý start, potřebujete předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="ecc24-171">toocreate an Azure Container Service cluster by using an Azure quickstart template, you need an Azure subscription.</span></span> <span data-ttu-id="ecc24-172">Pokud žádné nemáte, můžete se zaregistrovat k [bezplatné zkušební verzi](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).</span><span class="sxs-lookup"><span data-stu-id="ecc24-172">If you don't have one, then sign up for a [free trial](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).</span></span> 

<span data-ttu-id="ecc24-173">Postupujte podle těchto kroků toodeploy clusteru pomocí šablony a hello 2.0 rozhraní příkazového řádku Azure (viz [pokyny k instalaci a instalaci](/cli/azure/install-az-cli2)).</span><span class="sxs-lookup"><span data-stu-id="ecc24-173">Follow these steps toodeploy a cluster using a template and hello Azure CLI 2.0 (see [installation and setup instructions](/cli/azure/install-az-cli2)).</span></span>

> [!NOTE] 
> <span data-ttu-id="ecc24-174">Pokud jste v systému Windows, můžete použít podobné kroky toodeploy šablonu pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ecc24-174">If you're on a Windows system, you can use similar steps toodeploy a template using Azure PowerShell.</span></span> <span data-ttu-id="ecc24-175">Postup najdete dál v této části.</span><span class="sxs-lookup"><span data-stu-id="ecc24-175">See steps later in this section.</span></span> <span data-ttu-id="ecc24-176">Můžete taky nasadit šablonu prostřednictvím hello [portál](../../azure-resource-manager/resource-group-template-deploy-portal.md) nebo jiné metody.</span><span class="sxs-lookup"><span data-stu-id="ecc24-176">You can also deploy a template through hello [portal](../../azure-resource-manager/resource-group-template-deploy-portal.md) or other methods.</span></span>

1. <span data-ttu-id="ecc24-177">toodeploy cluster DC/OS, Docker Swarm nebo Kubernetes, vyberte jednu z hello k dispozici rychlý start šablony z Githubu.</span><span class="sxs-lookup"><span data-stu-id="ecc24-177">toodeploy a DC/OS, Docker Swarm, or Kubernetes cluster, select one of hello available quickstart templates from GitHub.</span></span> <span data-ttu-id="ecc24-178">Následuje dílčí seznam.</span><span class="sxs-lookup"><span data-stu-id="ecc24-178">A partial list follows.</span></span> <span data-ttu-id="ecc24-179">Hello DC/OS a Swarm šablony jsou hello stejné, s výjimkou hello výchozí orchestrator výběr.</span><span class="sxs-lookup"><span data-stu-id="ecc24-179">hello DC/OS and Swarm templates are hello same, except for hello default orchestrator selection.</span></span>

    * [<span data-ttu-id="ecc24-180">Šablona DC/OS</span><span class="sxs-lookup"><span data-stu-id="ecc24-180">DC/OS template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [<span data-ttu-id="ecc24-181">Šablona Swarm</span><span class="sxs-lookup"><span data-stu-id="ecc24-181">Swarm template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [<span data-ttu-id="ecc24-182">Šablona Kubernetes</span><span class="sxs-lookup"><span data-stu-id="ecc24-182">Kubernetes template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. <span data-ttu-id="ecc24-183">Přihlaste se tooyour účet Azure (`az login`) a ujistěte se, že hello rozhraní příkazového řádku Azure je připojený tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="ecc24-183">Log in tooyour Azure account (`az login`), and make sure that hello Azure CLI is connected tooyour Azure subscription.</span></span> <span data-ttu-id="ecc24-184">Hello výchozí předplatné můžete zobrazit pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ecc24-184">You can see hello default subscription by using hello following command:</span></span>

    ```azurecli
    az account show
    ```
    
    <span data-ttu-id="ecc24-185">Pokud máte více než jeden odběr a nutnosti tooset různé výchozí předplatné, spusťte `az account set --subscription` a zadejte název nebo ID odběru hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-185">If you have more than one subscription and need tooset a different default subscription, run `az account set --subscription` and specify hello subscription ID or name.</span></span>

3. <span data-ttu-id="ecc24-186">Jako osvědčený postup použijte pro nasazení hello novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="ecc24-186">As a best practice, use a new resource group for hello deployment.</span></span> <span data-ttu-id="ecc24-187">toocreate skupinu prostředků použijte hello `az group create` příkaz zadejte název skupiny prostředků a umístění:</span><span class="sxs-lookup"><span data-stu-id="ecc24-187">toocreate a resource group, use hello `az group create` command specify a resource group name and location:</span></span> 

    ```azurecli
    az group create --name "RESOURCE_GROUP" --location "LOCATION"
    ```

4. <span data-ttu-id="ecc24-188">Vytvořte soubor obsahující hello požadované šablonu JSON parametry.</span><span class="sxs-lookup"><span data-stu-id="ecc24-188">Create a JSON file containing hello required template parameters.</span></span> <span data-ttu-id="ecc24-189">Stažení hello parametry soubor s názvem `azuredeploy.parameters.json` doprovodný šablony Azure Container Service hello `azuredeploy.json` v Githubu.</span><span class="sxs-lookup"><span data-stu-id="ecc24-189">Download hello parameters file named `azuredeploy.parameters.json` that accompanies hello Azure Container Service template `azuredeploy.json` in GitHub.</span></span> <span data-ttu-id="ecc24-190">Zadejte požadované hodnoty parametrů pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="ecc24-190">Enter required parameter values for your cluster.</span></span> 

    <span data-ttu-id="ecc24-191">Například toouse hello [šablona DC/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos), zadejte hodnoty parametrů pro `dnsNamePrefix` a `sshRSAPublicKey`.</span><span class="sxs-lookup"><span data-stu-id="ecc24-191">For example, toouse hello [DC/OS template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos), supply parameter values for `dnsNamePrefix` and `sshRSAPublicKey`.</span></span> <span data-ttu-id="ecc24-192">V tématu hello popis v `azuredeploy.json` a možnosti pro další parametry.</span><span class="sxs-lookup"><span data-stu-id="ecc24-192">See hello descriptions in `azuredeploy.json` and options for other parameters.</span></span>  
 

5. <span data-ttu-id="ecc24-193">Vytvořit cluster Container Service pomocí předání soubor parametrů nasazení hello s hello následující příkaz, kde:</span><span class="sxs-lookup"><span data-stu-id="ecc24-193">Create a Container Service cluster by passing hello deployment parameters file with hello following command, where:</span></span>

    * <span data-ttu-id="ecc24-194">**RESOURCE_GROUP** je hello název skupiny prostředků hello, který jste vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-194">**RESOURCE_GROUP** is hello name of hello resource group that you created in hello previous step.</span></span>
    * <span data-ttu-id="ecc24-195">**DEPLOYMENT_NAME** (volitelné) je název poskytnout toohello nasazení.</span><span class="sxs-lookup"><span data-stu-id="ecc24-195">**DEPLOYMENT_NAME** (optional) is a name you give toohello deployment.</span></span>
    * <span data-ttu-id="ecc24-196">**TEMPLATE_URI** je hello umístění souboru nasazení hello `azuredeploy.json`.</span><span class="sxs-lookup"><span data-stu-id="ecc24-196">**TEMPLATE_URI** is hello location of hello deployment file `azuredeploy.json`.</span></span> <span data-ttu-id="ecc24-197">Tento identifikátor URI musí být soubor Raw hello, ne to ukazatel toohello uživatelského rozhraní Githubu.</span><span class="sxs-lookup"><span data-stu-id="ecc24-197">This URI must be hello Raw file, not a pointer toohello GitHub UI.</span></span> <span data-ttu-id="ecc24-198">toofind tento identifikátor URI, vyberte hello `azuredeploy.json` souboru v Githubu a klikněte na tlačítko hello **Raw** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ecc24-198">toofind this URI, select hello `azuredeploy.json` file in GitHub, and click hello **Raw** button.</span></span>  

    ```azurecli
    az group deployment create -g RESOURCE_GROUP -n DEPLOYMENT_NAME --template-uri TEMPLATE_URI --parameters @azuredeploy.parameters.json
    ```

    <span data-ttu-id="ecc24-199">Parametry můžete zadat taky jako řetězec formátu JSON na příkazovém řádku hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-199">You can also provide parameters as a JSON-formatted string on hello command line.</span></span> <span data-ttu-id="ecc24-200">Použijte podobné toohello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ecc24-200">Use a command similar toohello following:</span></span>

    ```azurecli
    az group deployment create -g RESOURCE_GROUP -n DEPLOYMENT_NAME --template-uri TEMPLATE_URI --parameters "{ \"param1\": {\"value1\"} … }"
    ```

    > [!NOTE]
    > <span data-ttu-id="ecc24-201">nasazení Hello trvá několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="ecc24-201">hello deployment takes several minutes toocomplete.</span></span>
    > 

### <a name="equivalent-powershell-commands"></a><span data-ttu-id="ecc24-202">Ekvivalentní příkazy PowerShellu</span><span class="sxs-lookup"><span data-stu-id="ecc24-202">Equivalent PowerShell commands</span></span>
<span data-ttu-id="ecc24-203">Šablonu clusteru Azure Container Service můžete také nasadit v PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="ecc24-203">You can also deploy an Azure Container Service cluster template with PowerShell.</span></span> <span data-ttu-id="ecc24-204">Tento dokument je založen na hello verze 1.0 [modul Azure PowerShell](https://azure.microsoft.com/blog/azps-1-0/).</span><span class="sxs-lookup"><span data-stu-id="ecc24-204">This document is based on hello version 1.0 [Azure PowerShell module](https://azure.microsoft.com/blog/azps-1-0/).</span></span>

1. <span data-ttu-id="ecc24-205">toodeploy cluster DC/OS, Docker Swarm nebo Kubernetes, vyberte jednu z hello k dispozici rychlý start šablony z Githubu.</span><span class="sxs-lookup"><span data-stu-id="ecc24-205">toodeploy a DC/OS, Docker Swarm, or Kubernetes cluster, select one of hello available quickstart templates from GitHub.</span></span> <span data-ttu-id="ecc24-206">Následuje dílčí seznam.</span><span class="sxs-lookup"><span data-stu-id="ecc24-206">A partial list follows.</span></span> <span data-ttu-id="ecc24-207">Všimněte si, že hello DC/OS a Swarm šablony jsou hello stejné, s výjimkou hello hello výchozí orchestrator výběr.</span><span class="sxs-lookup"><span data-stu-id="ecc24-207">Note that hello DC/OS and Swarm templates are hello same, with hello exception of hello default orchestrator selection.</span></span>

    * [<span data-ttu-id="ecc24-208">Šablona DC/OS</span><span class="sxs-lookup"><span data-stu-id="ecc24-208">DC/OS template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [<span data-ttu-id="ecc24-209">Šablona Swarm</span><span class="sxs-lookup"><span data-stu-id="ecc24-209">Swarm template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [<span data-ttu-id="ecc24-210">Šablona Kubernetes</span><span class="sxs-lookup"><span data-stu-id="ecc24-210">Kubernetes template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. <span data-ttu-id="ecc24-211">Před vytvořením clusteru v rámci vašeho předplatného Azure, ověřte, že relace prostředí PowerShell je přihlášena tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ecc24-211">Before creating a cluster in your Azure subscription, verify that your PowerShell session has been signed in tooAzure.</span></span> <span data-ttu-id="ecc24-212">To lze provést pomocí hello `Get-AzureRMSubscription` příkaz:</span><span class="sxs-lookup"><span data-stu-id="ecc24-212">You can do this with hello `Get-AzureRMSubscription` command:</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

3. <span data-ttu-id="ecc24-213">Pokud potřebujete toosign v tooAzure, použijte hello `Login-AzureRMAccount` příkaz:</span><span class="sxs-lookup"><span data-stu-id="ecc24-213">If you need toosign in tooAzure, use hello `Login-AzureRMAccount` command:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

4. <span data-ttu-id="ecc24-214">Jako osvědčený postup použijte pro nasazení hello novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="ecc24-214">As a best practice, use a new resource group for hello deployment.</span></span> <span data-ttu-id="ecc24-215">toocreate skupinu prostředků použijte hello `New-AzureRmResourceGroup` příkaz a zadejte oblast název a cíl skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="ecc24-215">toocreate a resource group, use hello `New-AzureRmResourceGroup` command, and specify a resource group name and destination region:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
    ```

5. <span data-ttu-id="ecc24-216">Po vytvoření skupiny prostředků, můžete vytvořit cluster s hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="ecc24-216">After you create a resource group, you can create your cluster with hello following command.</span></span> <span data-ttu-id="ecc24-217">Hello URI hello potřeby je zadaná šablona s hello `-TemplateUri` parametr.</span><span class="sxs-lookup"><span data-stu-id="ecc24-217">hello URI of hello desired template is specified with hello `-TemplateUri` parameter.</span></span> <span data-ttu-id="ecc24-218">Při spuštění tohoto příkazu vás PowerShell vyzve k zadání hodnot parametrů nasazení.</span><span class="sxs-lookup"><span data-stu-id="ecc24-218">When you run this command, PowerShell prompts you for deployment parameter values.</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
    ```

#### <a name="provide-template-parameters"></a><span data-ttu-id="ecc24-219">Zadání parametrů šablony</span><span class="sxs-lookup"><span data-stu-id="ecc24-219">Provide template parameters</span></span>
<span data-ttu-id="ecc24-220">Pokud jste obeznámeni s prostředím PowerShell, víte, že můžete přepínat mezi hello dostupné parametry rutiny zadáním znaménka minus (-) a stisknutím klávesy tabulátor hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-220">If you're familiar with PowerShell, you know that you can cycle through hello available parameters for a cmdlet by typing a minus sign (-) and then pressing hello TAB key.</span></span> <span data-ttu-id="ecc24-221">Stejně to funguje i při práci s parametry, které definujete v šabloně.</span><span class="sxs-lookup"><span data-stu-id="ecc24-221">This same functionality also works with parameters that you define in your template.</span></span> <span data-ttu-id="ecc24-222">Jakmile zadáte název šablony hello, hello rutina načte šablonu hello, analyzuje hello parametrů a dynamicky přidá příkaz toohello parametry šablony hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-222">As soon as you type hello template name, hello cmdlet fetches hello template, parses hello parameters, and adds hello template parameters toohello command dynamically.</span></span> <span data-ttu-id="ecc24-223">Díky tomu hodnot parametrů šablony snadno toospecify hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-223">This makes it easy toospecify hello template parameter values.</span></span> <span data-ttu-id="ecc24-224">A pokud zapomenete hodnotu požadovaného parametru, prostředí PowerShell vás vyzve k zadání hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="ecc24-224">And, if you forget a required parameter value, PowerShell prompts you for hello value.</span></span>

<span data-ttu-id="ecc24-225">Zde je celý příkaz hello i s vloženými parametry.</span><span class="sxs-lookup"><span data-stu-id="ecc24-225">Here is hello full command, with parameters included.</span></span> <span data-ttu-id="ecc24-226">Zadejte vlastní hodnoty pro názvy hello hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="ecc24-226">Provide your own values for hello names of hello resources.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a><span data-ttu-id="ecc24-227">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ecc24-227">Next steps</span></span>
<span data-ttu-id="ecc24-228">Nyní když máte funkční cluster, nahlédněte do těchto dokumentů, kde naleznete podrobnosti týkající se připojení a správy:</span><span class="sxs-lookup"><span data-stu-id="ecc24-228">Now that you have a functioning cluster, see these documents for connection and management details:</span></span>

* [<span data-ttu-id="ecc24-229">Připojení clusteru Azure Container Service tooan</span><span class="sxs-lookup"><span data-stu-id="ecc24-229">Connect tooan Azure Container Service cluster</span></span>](../container-service-connect.md)
* [<span data-ttu-id="ecc24-230">Práce se službou Azure Container Service a DC/OS</span><span class="sxs-lookup"><span data-stu-id="ecc24-230">Work with Azure Container Service and DC/OS</span></span>](container-service-mesos-marathon-rest.md)
* [<span data-ttu-id="ecc24-231">Práce se službou Azure Container Service a nástrojem Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="ecc24-231">Work with Azure Container Service and Docker Swarm</span></span>](container-service-docker-swarm.md)
* [<span data-ttu-id="ecc24-232">Práce s Azure Container Service a Kubernetes</span><span class="sxs-lookup"><span data-stu-id="ecc24-232">Work with Azure Container Service and Kubernetes</span></span>](../kubernetes/container-service-kubernetes-walkthrough.md)
