---
title: aaaJenkins CI/CD s Kubernetes v Azure Container Service | Microsoft Docs
description: "Jak tooautomate CI/CD zpracovat volaných toodeploy a upgrade kontejnerizované aplikace na Kubernetes v Azure Container Service"
services: container-service
documentationcenter: 
author: chzbrgr71
manager: johny
editor: 
tags: acs, azure-container-service, jenkins
keywords: "Docker kontejnery, Kubernetes, Azure, volaných"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: briar
ms.custom: mvc
ms.openlocfilehash: e00e13bf06619bed73e82878777e55458ea3dd77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="jenkins-integration-with-azure-container-service-and-kubernetes"></a><span data-ttu-id="4a79b-104">Volaných integraci s Azure Container Service a Kubernetes</span><span class="sxs-lookup"><span data-stu-id="4a79b-104">Jenkins integration with Azure Container Service and Kubernetes</span></span> 
<span data-ttu-id="4a79b-105">V tomto kurzu jsme provede hello proces tooset až průběžnou integraci aplikace s více kontejnerů do Azure kontejneru služby Kubernetes pomocí platformy volaných hello.</span><span class="sxs-lookup"><span data-stu-id="4a79b-105">In this tutorial, we walk through hello process tooset up continuous integration of a multi-container application into Azure Container Service Kubernetes using hello Jenkins platform.</span></span> <span data-ttu-id="4a79b-106">pracovní postup Hello aktualizací bitové kopie hello kontejneru v úložiště Docker Hub a upgraduje hello Kubernetes pracovními stanicemi soustředěnými kolem pomocí zavedení nasazení.</span><span class="sxs-lookup"><span data-stu-id="4a79b-106">hello workflow updates hello container image in Docker Hub and upgrades hello Kubernetes pods using a deployment rollout.</span></span> 

## <a name="high-level-process"></a><span data-ttu-id="4a79b-107">Proces vysoké úrovně</span><span class="sxs-lookup"><span data-stu-id="4a79b-107">High level process</span></span>
<span data-ttu-id="4a79b-108">Hello základní kroky popsané v tomto článku jsou:</span><span class="sxs-lookup"><span data-stu-id="4a79b-108">hello basic steps detailed in this article are:</span></span> 
- <span data-ttu-id="4a79b-109">Instalace clusteru s podporou Kubernetes v kontejneru služby</span><span class="sxs-lookup"><span data-stu-id="4a79b-109">Install a Kubernetes cluster in Container Service</span></span>
- <span data-ttu-id="4a79b-110">Nastavení volaných a konfigurace přístupu tooContainer služby</span><span class="sxs-lookup"><span data-stu-id="4a79b-110">Set up Jenkins and configure access tooContainer Service</span></span>
- <span data-ttu-id="4a79b-111">Vytvoření pracovního postupu volaných</span><span class="sxs-lookup"><span data-stu-id="4a79b-111">Create a Jenkins workflow</span></span>
- <span data-ttu-id="4a79b-112">Testování tooend koncového procesu CI/CD hello</span><span class="sxs-lookup"><span data-stu-id="4a79b-112">Test hello CI/CD process end tooend</span></span>

## <a name="install-a-kubernetes-cluster"></a><span data-ttu-id="4a79b-113">Instalace clusteru s podporou Kubernetes</span><span class="sxs-lookup"><span data-stu-id="4a79b-113">Install a Kubernetes cluster</span></span>
    
<span data-ttu-id="4a79b-114">Nasazení clusteru Kubernetes hello v Azure Container Service pomocí hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="4a79b-114">Deploy hello Kubernetes cluster in Azure Container Service using hello following steps.</span></span> <span data-ttu-id="4a79b-115">Úplnou dokumentaci se nachází [zde](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="4a79b-115">Full documentation is located [here](container-service-kubernetes-walkthrough.md).</span></span>

### <a name="step-1-create-a-resource-group"></a><span data-ttu-id="4a79b-116">Krok 1: Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="4a79b-116">Step 1: Create a resource group</span></span>
```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus

az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="step-2-deploy-hello-cluster"></a><span data-ttu-id="4a79b-117">Krok 2: Nasazení clusteru hello</span><span class="sxs-lookup"><span data-stu-id="4a79b-117">Step 2: Deploy hello cluster</span></span>
> [!NOTE]
> <span data-ttu-id="4a79b-118">Hello následující kroky vyžadují místní veřejný klíč SSH uložen ve složce ~/.ssh hello.</span><span class="sxs-lookup"><span data-stu-id="4a79b-118">hello following steps require a local SSH public key stored in hello ~/.ssh folder.</span></span>
>

```azurecli
RESOURCE_GROUP=my-resource-group
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name

az acs create \
--orchestrator-type=kubernetes \
--resource-group $RESOURCE_GROUP \
--name=$CLUSTER_NAME \
--dns-prefix=$DNS_PREFIX \
--ssh-key-value ~/.ssh/id_rsa.pub \
--admin-username=azureuser \
--master-count=1 \
--agent-count=5 \
--agent-vm-size=Standard_D1_v2
```

## <a name="set-up-jenkins-and-configure-access-toocontainer-service"></a><span data-ttu-id="4a79b-119">Nastavení volaných a konfigurace přístupu tooContainer služby</span><span class="sxs-lookup"><span data-stu-id="4a79b-119">Set up Jenkins and configure access tooContainer Service</span></span>

### <a name="step-1-install-jenkins"></a><span data-ttu-id="4a79b-120">Krok 1: Instalace volaných</span><span class="sxs-lookup"><span data-stu-id="4a79b-120">Step 1: Install Jenkins</span></span>
1. <span data-ttu-id="4a79b-121">Vytvořte virtuální počítač Azure s Ubuntu 16.04 LTS.</span><span class="sxs-lookup"><span data-stu-id="4a79b-121">Create an Azure VM with Ubuntu 16.04 LTS.</span></span>  <span data-ttu-id="4a79b-122">Vzhledem k tomu, že dál v hello kroky nutné tooconnect toothis virtuální počítač pomocí na místním počítači, sada hello "Ověřování typu" too'SSH veřejný klíč bash budou a vložte hello veřejný klíč SSH místně uložená ve složce ~/.ssh.</span><span class="sxs-lookup"><span data-stu-id="4a79b-122">Since later in hello steps you will need tooconnect toothis VM using bash on your local machine, set hello 'Authentication type' too'SSH public key' and paste hello SSH public key that is stored locally in your ~/.ssh folder.</span></span>  <span data-ttu-id="4a79b-123">Také si poznamenejte hello "uživatelské jméno, že zadáváte vzhledem k tomu, že toto uživatelské jméno bude potřebné tooview hello volaných řídicí panel a pro připojení toohello volaných virtuálních počítačů v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="4a79b-123">Also, take note of hello 'User name' that you specify since this user name will be needed tooview hello Jenkins dashboard and for connecting toohello Jenkins VM in later steps.</span></span>
2. <span data-ttu-id="4a79b-124">Nainstalujte volaných prostřednictvím těchto [pokyny](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu).</span><span class="sxs-lookup"><span data-stu-id="4a79b-124">Install Jenkins via these [instructions](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu).</span></span> <span data-ttu-id="4a79b-125">Podrobnější kurz je určen v [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).</span><span class="sxs-lookup"><span data-stu-id="4a79b-125">A more detailed tutorial is at [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).</span></span>
3. <span data-ttu-id="4a79b-126">tooview hello volaných řídicího panelu na místním počítači, aktualizovat port tooallow skupiny zabezpečení sítě Azure hello 8080 přidáním pravidlo pro příchozí data, která umožňuje přístup tooport 8080.</span><span class="sxs-lookup"><span data-stu-id="4a79b-126">tooview hello Jenkins dashboard on your local machine, update hello Azure network security group tooallow port 8080 by adding an inbound rule that allows access tooport 8080.</span></span>  <span data-ttu-id="4a79b-127">Alternativně může nastavit přesměrování portu tak, že spustíte tento příkaz:`ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`</span><span class="sxs-lookup"><span data-stu-id="4a79b-127">Alternatively, you may setup port forwarding by running this command: `ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`</span></span>
4. <span data-ttu-id="4a79b-128">Připojení serveru volaných tooyour pomocí prohlížeče hello přechodem toohello veřejnou IP adresu (http:// < your_jenkins_public_ip >: 8080) a odemknutí hello volaných řídicím panelu hello poprvé heslem hello počáteční správce.</span><span class="sxs-lookup"><span data-stu-id="4a79b-128">Connect tooyour Jenkins server using hello browser by navigating toohello public IP (http://<your_jenkins_public_ip>:8080) and unlock hello Jenkins dashboard for hello first time with hello initial admin password.</span></span>  <span data-ttu-id="4a79b-129">heslo správce Hello je uložený v /var/lib/jenkins/secrets/initialAdminPassword na hello volaných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4a79b-129">hello admin password is stored at /var/lib/jenkins/secrets/initialAdminPassword on hello Jenkins VM.</span></span>  <span data-ttu-id="4a79b-130">Snadný způsob tooget toto heslo je tooSSH do hello volaných virtuálních počítačů: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span><span class="sxs-lookup"><span data-stu-id="4a79b-130">An easy way tooget this password is tooSSH into hello Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span></span>  <span data-ttu-id="4a79b-131">Potom spustíte: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.</span><span class="sxs-lookup"><span data-stu-id="4a79b-131">Next, run: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.</span></span>
5. <span data-ttu-id="4a79b-132">Nainstalujte na počítač volaných hello prostřednictvím těchto Docker [pokyny](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts).</span><span class="sxs-lookup"><span data-stu-id="4a79b-132">Install Docker on hello Jenkins machine via these [instructions](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts).</span></span> <span data-ttu-id="4a79b-133">To umožňuje toobe Docker příkazy spouštět ve volaných úlohy.</span><span class="sxs-lookup"><span data-stu-id="4a79b-133">This allows for Docker commands toobe run in Jenkins jobs.</span></span>
6. <span data-ttu-id="4a79b-134">Nakonfigurujte Docker oprávnění tooallow volaných tooaccess hello Docker koncový bod.</span><span class="sxs-lookup"><span data-stu-id="4a79b-134">Configure Docker permissions tooallow Jenkins tooaccess hello Docker endpoint.</span></span>

    ```bash
    sudo chmod 777 /run/docker.sock
    ```
8. <span data-ttu-id="4a79b-135">Nainstalujte `kubectl` rozhraní příkazového řádku na volaných.</span><span class="sxs-lookup"><span data-stu-id="4a79b-135">Install `kubectl` CLI on Jenkins.</span></span> <span data-ttu-id="4a79b-136">Další podrobnosti najdete v [instalace a nastavení kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).</span><span class="sxs-lookup"><span data-stu-id="4a79b-136">More details are at [Installing and Setting up kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).</span></span>  <span data-ttu-id="4a79b-137">Úlohy volaných použije 'kubectl' toomanage a nasazení toohello Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="4a79b-137">Jenkins jobs will use 'kubectl' toomanage and deploy toohello Kubernetes cluster.</span></span>

    ```bash
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

    chmod +x ./kubectl

    sudo mv ./kubectl /usr/local/bin/kubectl
    ```

### <a name="step-2-set-up-access-toohello-kubernetes-cluster"></a><span data-ttu-id="4a79b-138">Krok 2: Nastavení clusteru Kubernetes toohello přístupu</span><span class="sxs-lookup"><span data-stu-id="4a79b-138">Step 2: Set up access toohello Kubernetes cluster</span></span>

> [!NOTE]
> <span data-ttu-id="4a79b-139">Existuje několik přístupů tooaccomplishing hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="4a79b-139">There are multiple approaches tooaccomplishing hello following steps.</span></span> <span data-ttu-id="4a79b-140">Použijte hello přístup, který je pro vás nejjednodušší.</span><span class="sxs-lookup"><span data-stu-id="4a79b-140">Use hello approach that is easiest for you.</span></span>
>

1. <span data-ttu-id="4a79b-141">Kopírování hello `kubectl` konfigurační soubor toohello volaných počítač tak, aby volaných úloh clusteru Kubernetes toohello přístupu.</span><span class="sxs-lookup"><span data-stu-id="4a79b-141">Copy hello `kubectl` config file toohello Jenkins machine so that Jenkins jobs have access toohello Kubernetes cluster.</span></span> <span data-ttu-id="4a79b-142">Tyto pokyny předpokládají, že používáte bash z jiný počítač, než hello volaných virtuálních počítačů a že místní veřejný klíč SSH je uložen ve složce ~/.ssh hello počítače.</span><span class="sxs-lookup"><span data-stu-id="4a79b-142">These instructions assume that you are using bash from a different machine than hello Jenkins VM and that a local SSH public key is stored in hello machine's ~/.ssh folder.</span></span>

```bash
export KUBE_MASTER=<your_cluster_master_fqdn>
export JENKINS_USER=<your_jenkins_user>
export JENKINS_SERVER=<your_jenkins_public_ip>
sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir -m 777 /home/$JENKINS_USER/.kube/ \
&& sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir /var/lib/jenkins/.kube/ \
&& sudo scp -3 -i ~/.ssh/id_rsa azureuser@$KUBE_MASTER:.kube/config $JENKINS_USER@$JENKINS_SERVER:~/.kube/config \
&& sudo ssh -i ~/.ssh/id_rsa $JENKINS_USER@$JENKINS_SERVER sudo cp /home/$JENKINS_USER/.kube/config /var/lib/jenkins/.kube/config \
```
        
2. <span data-ttu-id="4a79b-143">Ověření z volaných této hello Kubernetes clusteru je přístupný.</span><span class="sxs-lookup"><span data-stu-id="4a79b-143">Validate from Jenkins that hello Kubernetes cluster is accessible.</span></span>  <span data-ttu-id="4a79b-144">toodo se SSH do hello volaných virtuálních počítačů: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span><span class="sxs-lookup"><span data-stu-id="4a79b-144">toodo this, SSH into hello Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span></span>  <span data-ttu-id="4a79b-145">Dále ověřte volaných můžete úspěšně připojit tooyour cluster: `kubectl cluster-info`.</span><span class="sxs-lookup"><span data-stu-id="4a79b-145">Next, verify Jenkins can successfully connect tooyour cluster: `kubectl cluster-info`.</span></span>
    

## <a name="create-a-jenkins-workflow"></a><span data-ttu-id="4a79b-146">Vytvoření pracovního postupu volaných</span><span class="sxs-lookup"><span data-stu-id="4a79b-146">Create a Jenkins workflow</span></span>

### <a name="prerequisites"></a><span data-ttu-id="4a79b-147">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4a79b-147">Prerequisites</span></span>

- <span data-ttu-id="4a79b-148">Účet GitHub pro kód úložišti.</span><span class="sxs-lookup"><span data-stu-id="4a79b-148">GitHub account for code repo.</span></span>
- <span data-ttu-id="4a79b-149">Docker Hub účet toostore a aktualizaci bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="4a79b-149">Docker Hub account toostore and update images.</span></span>
- <span data-ttu-id="4a79b-150">Kontejnerizované aplikace, které je možné znovu sestavit a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="4a79b-150">Containerized application that can be rebuilt and updated.</span></span> <span data-ttu-id="4a79b-151">Můžete použít této ukázkové kontejneru aplikace napsané v Golang: https://github.com/chzbrgr71/go-web</span><span class="sxs-lookup"><span data-stu-id="4a79b-151">You can use this sample container app written in Golang: https://github.com/chzbrgr71/go-web</span></span> 

> [!NOTE]
> <span data-ttu-id="4a79b-152">Hello následující kroky je potřeba provést v účtu Githubu.</span><span class="sxs-lookup"><span data-stu-id="4a79b-152">hello following steps must be performed in your own GitHub account.</span></span> <span data-ttu-id="4a79b-153">Myslíte, že volné tooclone hello výše úložiště, ale musíte použít vlastní účet tooconfigure hello webhooky a volaných přístup.</span><span class="sxs-lookup"><span data-stu-id="4a79b-153">Feel free tooclone hello above repo, but you must use your own account tooconfigure hello webhooks and Jenkins access.</span></span>
>

### <a name="step-1-deploy-initial-v1-of-application"></a><span data-ttu-id="4a79b-154">Krok 1: Nasazení počáteční v1 aplikace</span><span class="sxs-lookup"><span data-stu-id="4a79b-154">Step 1: Deploy initial v1 of application</span></span>
1. <span data-ttu-id="4a79b-155">Sestavení aplikace hello z počítače vývojáře hello s hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="4a79b-155">Build hello app from hello developer machine with hello following commands.</span></span> <span data-ttu-id="4a79b-156">Nahraďte `myrepo` vlastními.</span><span class="sxs-lookup"><span data-stu-id="4a79b-156">Replace `myrepo` with your own.</span></span>
    
    ```bash
    git clone https://github.com/chzbrgr71/go-web.git
    cd go-web
    docker build -t myrepo/go-web .
    ```

2. <span data-ttu-id="4a79b-157">Push image tooDocker rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="4a79b-157">Push image tooDocker Hub.</span></span>

    ```bash
    docker login
    docker push myrepo/go-web
    ```

3. <span data-ttu-id="4a79b-158">Nasazení clusteru Kubernetes toohello.</span><span class="sxs-lookup"><span data-stu-id="4a79b-158">Deploy toohello Kubernetes cluster.</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="4a79b-159">Upravit hello `go-web.yaml` souboru tooupdate kontejneru image a úložišti.</span><span class="sxs-lookup"><span data-stu-id="4a79b-159">Edit hello `go-web.yaml` file tooupdate your container image and repo.</span></span>
    >
        
    ```bash
    kubectl create -f ./go-web.yaml --record
    ```
### <a name="step-2-configure-jenkins-system"></a><span data-ttu-id="4a79b-160">Krok 2: Konfigurace volaných systému</span><span class="sxs-lookup"><span data-stu-id="4a79b-160">Step 2: Configure Jenkins system</span></span>
1. <span data-ttu-id="4a79b-161">Klikněte na tlačítko **spravovat volaných** > **konfiguraci systému**.</span><span class="sxs-lookup"><span data-stu-id="4a79b-161">Click **Manage Jenkins** > **Configure System**.</span></span>
2. <span data-ttu-id="4a79b-162">V části **Githubu**, vyberte **přidat Server Githubu**.</span><span class="sxs-lookup"><span data-stu-id="4a79b-162">Under **GitHub**, select **Add GitHub Server**.</span></span>
3. <span data-ttu-id="4a79b-163">Nechte **adresy URL rozhraní API** jako výchozí.</span><span class="sxs-lookup"><span data-stu-id="4a79b-163">Leave **API URL** as default.</span></span>
4. <span data-ttu-id="4a79b-164">V části **pověření**, přidat pomocí přihlašovacích údajů volaných **tajný text**.</span><span class="sxs-lookup"><span data-stu-id="4a79b-164">Under **Credentials**, add a Jenkins credential using **Secret text**.</span></span> <span data-ttu-id="4a79b-165">Doporučujeme používat Githubu osobní přístupové tokeny, které jsou nakonfigurované v nastavení účtu uživatele Githubu.</span><span class="sxs-lookup"><span data-stu-id="4a79b-165">We recommend using GitHub personal access tokens, which are configured in your GitHub user account settings.</span></span> <span data-ttu-id="4a79b-166">Další informace o to [sem.](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)</span><span class="sxs-lookup"><span data-stu-id="4a79b-166">More details on this [here.](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)</span></span>
5. <span data-ttu-id="4a79b-167">Klikněte na tlačítko **testovací připojení** tooensure to správně nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="4a79b-167">Click **Test connection** tooensure this is configured correctly.</span></span>
6. <span data-ttu-id="4a79b-168">V části **vlastnosti globálních**, přidejte proměnná prostředí `DOCKER_HUB` a zadejte heslo úložiště Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="4a79b-168">Under **Global Properties**, add an environment variable `DOCKER_HUB` and provide your Docker Hub password.</span></span> <span data-ttu-id="4a79b-169">(To je užitečné v této ukázce, ale produkční scénář by vyžadovaly bezpečnější přístup.)</span><span class="sxs-lookup"><span data-stu-id="4a79b-169">(This is useful in this demo, but a production scenario would require a more secure approach.)</span></span>
7. <span data-ttu-id="4a79b-170">Uložte.</span><span class="sxs-lookup"><span data-stu-id="4a79b-170">Save.</span></span>

![Přístup volaných Githubu](./media/container-service-kubernetes-jenkins/jenkins-github-access.png)

### <a name="step-3-create-hello-jenkins-workflow"></a><span data-ttu-id="4a79b-172">Krok 3: Vytvoření pracovního postupu volaných hello</span><span class="sxs-lookup"><span data-stu-id="4a79b-172">Step 3: Create hello Jenkins workflow</span></span>
1. <span data-ttu-id="4a79b-173">Vytvořte položku volaných.</span><span class="sxs-lookup"><span data-stu-id="4a79b-173">Create a Jenkins item.</span></span>
2. <span data-ttu-id="4a79b-174">Zadejte název (například "přejděte – web") a vyberte **volný styl projektu**.</span><span class="sxs-lookup"><span data-stu-id="4a79b-174">Provide a name (for example, "go-web") and select **Freestyle Project**.</span></span> 
3. <span data-ttu-id="4a79b-175">Zkontrolujte **Githubu projektu** a zadat úložiště GitHub tooyour hello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="4a79b-175">Check **GitHub project** and provide hello URL tooyour GitHub repo.</span></span>
4. <span data-ttu-id="4a79b-176">V **správu zdrojového kódu**, zadejte adresu URL úložiště GitHub hello a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="4a79b-176">In **Source Code Management**, provide hello GitHub repo URL and credentials.</span></span> 
5. <span data-ttu-id="4a79b-177">Přidat **sestavení krok** typu **spustit prostředí** a hello použijte následující text:</span><span class="sxs-lookup"><span data-stu-id="4a79b-177">Add a **Build Step** of type **Execute shell** and use hello following text:</span></span>

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME .
    docker login -u <your-dockerhub-username> -p ${DOCKER_HUB}
    docker push $WEB_IMAGE_NAME
    ```

6. <span data-ttu-id="4a79b-178">Přidejte další **sestavení krok** typu **spustit prostředí** a hello použijte následující text:</span><span class="sxs-lookup"><span data-stu-id="4a79b-178">Add another **Build Step** of type **Execute shell** and use hello following text:</span></span>

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    kubectl set image deployment/go-web go-web=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
    ```

![Kroky sestavení volaných](./media/container-service-kubernetes-jenkins/jenkins-build-steps.png)
    
7. <span data-ttu-id="4a79b-180">Uložit položky volaných hello a testování pomocí **sestavení teď**.</span><span class="sxs-lookup"><span data-stu-id="4a79b-180">Save hello Jenkins item and test with **Build Now**.</span></span>

### <a name="step-4-connect-github-webhook"></a><span data-ttu-id="4a79b-181">Krok 4: Připojení webhook Githubu</span><span class="sxs-lookup"><span data-stu-id="4a79b-181">Step 4: Connect GitHub webhook</span></span>
1. <span data-ttu-id="4a79b-182">V položce volaných hello jste vytvořili, klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="4a79b-182">In hello Jenkins item you created, click **Configure**.</span></span>
2. <span data-ttu-id="4a79b-183">V části **sestavení aktivační události**, vyberte **Githubu háku aktivační událost pro dotazování GITScm** a **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4a79b-183">Under **Build Triggers**, select **GitHub hook trigger for GITScm polling** and **Save**.</span></span> <span data-ttu-id="4a79b-184">Tím se automaticky nakonfiguruje webhook Githubu hello.</span><span class="sxs-lookup"><span data-stu-id="4a79b-184">This automatically configures hello GitHub webhook.</span></span>
3. <span data-ttu-id="4a79b-185">V úložiště GitHub pro přejděte web, klikněte na **Nastavení > Webhooky**.</span><span class="sxs-lookup"><span data-stu-id="4a79b-185">In your GitHub repo for go-web, click **Settings > Webhooks**.</span></span>
4. <span data-ttu-id="4a79b-186">Ověřte, že hello volaných webhooku, které adresu URL byl úspěšně přidán.</span><span class="sxs-lookup"><span data-stu-id="4a79b-186">Verify that hello Jenkins webhook URL was added successfully.</span></span> <span data-ttu-id="4a79b-187">Adresa URL Hello musí končit "webhook githubu".</span><span class="sxs-lookup"><span data-stu-id="4a79b-187">hello URL should end in "github-webhook".</span></span>

![Konfigurace webhooku volaných](./media/container-service-kubernetes-jenkins/jenkins-webhook.png)

## <a name="test-hello-cicd-process-end-tooend"></a><span data-ttu-id="4a79b-189">Testování tooend koncového procesu CI/CD hello</span><span class="sxs-lookup"><span data-stu-id="4a79b-189">Test hello CI/CD process end tooend</span></span>

1. <span data-ttu-id="4a79b-190">Aktualizujte kód pro hello úložišti a nabízených nebo synchronizace s úložišti GitHub hello.</span><span class="sxs-lookup"><span data-stu-id="4a79b-190">Update code for hello repo and push/synch with hello GitHub repository.</span></span>
2. <span data-ttu-id="4a79b-191">Z konzoly volaných hello, zkontrolujte hello **sestavení historie** a ověřte, že hello úloha byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="4a79b-191">From hello Jenkins console, check hello **Build History** and validate that hello job has run.</span></span> <span data-ttu-id="4a79b-192">Zobrazení podrobností toosee výstupu konzoly.</span><span class="sxs-lookup"><span data-stu-id="4a79b-192">View console output toosee details.</span></span>
3. <span data-ttu-id="4a79b-193">Zobrazení podrobností hello z Kubernetes, upgradovat nasazení:</span><span class="sxs-lookup"><span data-stu-id="4a79b-193">From Kubernetes, view details of hello upgraded deployment:</span></span>

    ```bash
    kubectl rollout history deployment/go-web
    ```

## <a name="next-steps"></a><span data-ttu-id="4a79b-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4a79b-194">Next steps</span></span>

- <span data-ttu-id="4a79b-195">Nasaďte registru kontejner Azure a uložení bitové kopie v zabezpečené úložiště.</span><span class="sxs-lookup"><span data-stu-id="4a79b-195">Deploy Azure Container Registry and store images in a secure repository.</span></span> <span data-ttu-id="4a79b-196">V tématu [dokumentace Azure kontejneru registru](https://docs.microsoft.com/azure/container-registry).</span><span class="sxs-lookup"><span data-stu-id="4a79b-196">See [Azure Container Registry docs](https://docs.microsoft.com/azure/container-registry).</span></span>
- <span data-ttu-id="4a79b-197">Vytvořte složitější pracovní postup, který obsahuje vedle sebe nasazení a automatizovaných testů ve volaných.</span><span class="sxs-lookup"><span data-stu-id="4a79b-197">Build a more complex workflow that includes side-by-side deployment and automated tests in Jenkins.</span></span>
- <span data-ttu-id="4a79b-198">Další informace o CI/CD s volaných a Kubernetes najdete v tématu hello [volaných blog](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).</span><span class="sxs-lookup"><span data-stu-id="4a79b-198">For more information about CI/CD with Jenkins and Kubernetes, see hello [Jenkins blog](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).</span></span>
