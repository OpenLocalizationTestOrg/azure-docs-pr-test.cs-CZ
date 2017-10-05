---
title: "CI volaných disku CD s Kubernetes v Azure Container Service | Microsoft Docs"
description: "Jak automatizovat proces CI/CD s volaných k nasazení a upgrade kontejnerizované aplikace na Kubernetes v Azure Container Service"
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
ms.openlocfilehash: 2078d0694fc4dd6e83ecd2792588b4254980cd78
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="jenkins-integration-with-azure-container-service-and-kubernetes"></a><span data-ttu-id="6aa5a-104">Volaných integraci s Azure Container Service a Kubernetes</span><span class="sxs-lookup"><span data-stu-id="6aa5a-104">Jenkins integration with Azure Container Service and Kubernetes</span></span> 
<span data-ttu-id="6aa5a-105">V tomto kurzu jsme provede proces nastavte průběžnou integraci aplikace s více kontejnerů do Azure kontejneru služby Kubernetes použití volaných platformy.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-105">In this tutorial, we walk through the process to set up continuous integration of a multi-container application into Azure Container Service Kubernetes using the Jenkins platform.</span></span> <span data-ttu-id="6aa5a-106">Pracovní postup aktualizací bitové kopie kontejneru v úložiště Docker Hub a upgraduje Kubernetes pracovními stanicemi soustředěnými kolem pomocí zavedení nasazení.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-106">The workflow updates the container image in Docker Hub and upgrades the Kubernetes pods using a deployment rollout.</span></span> 

## <a name="high-level-process"></a><span data-ttu-id="6aa5a-107">Proces vysoké úrovně</span><span class="sxs-lookup"><span data-stu-id="6aa5a-107">High level process</span></span>
<span data-ttu-id="6aa5a-108">Toto jsou základní kroky popsané v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="6aa5a-108">The basic steps detailed in this article are:</span></span> 
- <span data-ttu-id="6aa5a-109">Instalace clusteru s podporou Kubernetes v kontejneru služby</span><span class="sxs-lookup"><span data-stu-id="6aa5a-109">Install a Kubernetes cluster in Container Service</span></span>
- <span data-ttu-id="6aa5a-110">Nastavit volaných a konfigurovat přístup ke kontejneru služby</span><span class="sxs-lookup"><span data-stu-id="6aa5a-110">Set up Jenkins and configure access to Container Service</span></span>
- <span data-ttu-id="6aa5a-111">Vytvoření pracovního postupu volaných</span><span class="sxs-lookup"><span data-stu-id="6aa5a-111">Create a Jenkins workflow</span></span>
- <span data-ttu-id="6aa5a-112">Otestujte proces CI/CD koncová</span><span class="sxs-lookup"><span data-stu-id="6aa5a-112">Test the CI/CD process end to end</span></span>

## <a name="install-a-kubernetes-cluster"></a><span data-ttu-id="6aa5a-113">Instalace clusteru s podporou Kubernetes</span><span class="sxs-lookup"><span data-stu-id="6aa5a-113">Install a Kubernetes cluster</span></span>
    
<span data-ttu-id="6aa5a-114">Nasazení clusteru Kubernetes v Azure Container Service pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-114">Deploy the Kubernetes cluster in Azure Container Service using the following steps.</span></span> <span data-ttu-id="6aa5a-115">Úplnou dokumentaci se nachází [zde](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="6aa5a-115">Full documentation is located [here](container-service-kubernetes-walkthrough.md).</span></span>

### <a name="step-1-create-a-resource-group"></a><span data-ttu-id="6aa5a-116">Krok 1: Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="6aa5a-116">Step 1: Create a resource group</span></span>
```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus

az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="step-2-deploy-the-cluster"></a><span data-ttu-id="6aa5a-117">Krok 2: Nasazení clusteru</span><span class="sxs-lookup"><span data-stu-id="6aa5a-117">Step 2: Deploy the cluster</span></span>
> [!NOTE]
> <span data-ttu-id="6aa5a-118">Následující kroky vyžadují místní veřejný klíč SSH uložen ve složce ~/.ssh.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-118">The following steps require a local SSH public key stored in the ~/.ssh folder.</span></span>
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

## <a name="set-up-jenkins-and-configure-access-to-container-service"></a><span data-ttu-id="6aa5a-119">Nastavit volaných a konfigurovat přístup ke kontejneru služby</span><span class="sxs-lookup"><span data-stu-id="6aa5a-119">Set up Jenkins and configure access to Container Service</span></span>

### <a name="step-1-install-jenkins"></a><span data-ttu-id="6aa5a-120">Krok 1: Instalace volaných</span><span class="sxs-lookup"><span data-stu-id="6aa5a-120">Step 1: Install Jenkins</span></span>
1. <span data-ttu-id="6aa5a-121">Vytvořte virtuální počítač Azure s Ubuntu 16.04 LTS.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-121">Create an Azure VM with Ubuntu 16.04 LTS.</span></span>  <span data-ttu-id="6aa5a-122">Vzhledem k tomu, že později v krocích budete muset připojit k tomuto virtuálnímu počítači pomocí bash na místním počítači, nastavení ověřování typu SSH veřejným klíčem a vložte veřejný klíč SSH, který je místně uložené ve složce ~/.ssh.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-122">Since later in the steps you will need to connect to this VM using bash on your local machine, set the 'Authentication type' to 'SSH public key' and paste the SSH public key that is stored locally in your ~/.ssh folder.</span></span>  <span data-ttu-id="6aa5a-123">Také si poznamenejte uživatelské jméno zadáte vzhledem k tomu, že toto uživatelské jméno bude potřeba k zobrazení řídicího panelu volaných a pro připojení k virtuálnímu počítači volaných v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-123">Also, take note of the 'User name' that you specify since this user name will be needed to view the Jenkins dashboard and for connecting to the Jenkins VM in later steps.</span></span>
2. <span data-ttu-id="6aa5a-124">Nainstalujte volaných prostřednictvím těchto [pokyny](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu).</span><span class="sxs-lookup"><span data-stu-id="6aa5a-124">Install Jenkins via these [instructions](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu).</span></span> <span data-ttu-id="6aa5a-125">Podrobnější kurz je určen v [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).</span><span class="sxs-lookup"><span data-stu-id="6aa5a-125">A more detailed tutorial is at [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).</span></span>
3. <span data-ttu-id="6aa5a-126">Chcete-li zobrazit řídicí panel volaných na místním počítači, aktualizujte na skupinu zabezpečení sítě Azure povolte port 8080 přidáním příchozí pravidlo, které umožňuje přístup k portu 8080.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-126">To view the Jenkins dashboard on your local machine, update the Azure network security group to allow port 8080 by adding an inbound rule that allows access to port 8080.</span></span>  <span data-ttu-id="6aa5a-127">Alternativně může nastavit přesměrování portu tak, že spustíte tento příkaz:`ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`</span><span class="sxs-lookup"><span data-stu-id="6aa5a-127">Alternatively, you may setup port forwarding by running this command: `ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`</span></span>
4. <span data-ttu-id="6aa5a-128">Připojení k serveru volaných pomocí prohlížeče přechodem na veřejné IP adresy (http:// < your_jenkins_public_ip >: 8080) a odemknutí řídicím panelu volaných poprvé heslem počáteční správce.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-128">Connect to your Jenkins server using the browser by navigating to the public IP (http://<your_jenkins_public_ip>:8080) and unlock the Jenkins dashboard for the first time with the initial admin password.</span></span>  <span data-ttu-id="6aa5a-129">Heslo správce je uložený v /var/lib/jenkins/secrets/initialAdminPassword volaných virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-129">The admin password is stored at /var/lib/jenkins/secrets/initialAdminPassword on the Jenkins VM.</span></span>  <span data-ttu-id="6aa5a-130">Snadný způsob, jak získat toto heslo je SSH do virtuálního počítače volaných: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-130">An easy way to get this password is to SSH into the Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span></span>  <span data-ttu-id="6aa5a-131">Potom spustíte: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-131">Next, run: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.</span></span>
5. <span data-ttu-id="6aa5a-132">Nainstalujte do počítače volaných prostřednictvím těchto Docker [pokyny](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts).</span><span class="sxs-lookup"><span data-stu-id="6aa5a-132">Install Docker on the Jenkins machine via these [instructions](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts).</span></span> <span data-ttu-id="6aa5a-133">To umožňuje Docker příkazy ke spuštění v volaných úlohy.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-133">This allows for Docker commands to be run in Jenkins jobs.</span></span>
6. <span data-ttu-id="6aa5a-134">Nakonfigurujte oprávnění Docker umožňující volaných pro přístup k Docker koncový bod.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-134">Configure Docker permissions to allow Jenkins to access the Docker endpoint.</span></span>

    ```bash
    sudo chmod 777 /run/docker.sock
    ```
8. <span data-ttu-id="6aa5a-135">Nainstalujte `kubectl` rozhraní příkazového řádku na volaných.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-135">Install `kubectl` CLI on Jenkins.</span></span> <span data-ttu-id="6aa5a-136">Další podrobnosti najdete v [instalace a nastavení kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).</span><span class="sxs-lookup"><span data-stu-id="6aa5a-136">More details are at [Installing and Setting up kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).</span></span>  <span data-ttu-id="6aa5a-137">Úlohy volaných použije ke správě a nasazení do clusteru Kubernetes 'kubectl'.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-137">Jenkins jobs will use 'kubectl' to manage and deploy to the Kubernetes cluster.</span></span>

    ```bash
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

    chmod +x ./kubectl

    sudo mv ./kubectl /usr/local/bin/kubectl
    ```

### <a name="step-2-set-up-access-to-the-kubernetes-cluster"></a><span data-ttu-id="6aa5a-138">Krok 2: Nastavení přístup ke clusteru Kubernetes</span><span class="sxs-lookup"><span data-stu-id="6aa5a-138">Step 2: Set up access to the Kubernetes cluster</span></span>

> [!NOTE]
> <span data-ttu-id="6aa5a-139">Existuje několik přístupů k provedení následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-139">There are multiple approaches to accomplishing the following steps.</span></span> <span data-ttu-id="6aa5a-140">Použijte postup, který je pro vás nejjednodušší.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-140">Use the approach that is easiest for you.</span></span>
>

1. <span data-ttu-id="6aa5a-141">Kopírování `kubectl` konfigurační soubor do počítače volaných tak, aby měli přístup ke clusteru Kubernetes volaných úlohy.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-141">Copy the `kubectl` config file to the Jenkins machine so that Jenkins jobs have access to the Kubernetes cluster.</span></span> <span data-ttu-id="6aa5a-142">Tyto pokyny předpokládají, že používáte bash z jiný počítač než volaných virtuálního počítače a místní veřejný klíč SSH je uložen ve složce ~/.ssh počítače.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-142">These instructions assume that you are using bash from a different machine than the Jenkins VM and that a local SSH public key is stored in the machine's ~/.ssh folder.</span></span>

```bash
export KUBE_MASTER=<your_cluster_master_fqdn>
export JENKINS_USER=<your_jenkins_user>
export JENKINS_SERVER=<your_jenkins_public_ip>
sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir -m 777 /home/$JENKINS_USER/.kube/ \
&& sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir /var/lib/jenkins/.kube/ \
&& sudo scp -3 -i ~/.ssh/id_rsa azureuser@$KUBE_MASTER:.kube/config $JENKINS_USER@$JENKINS_SERVER:~/.kube/config \
&& sudo ssh -i ~/.ssh/id_rsa $JENKINS_USER@$JENKINS_SERVER sudo cp /home/$JENKINS_USER/.kube/config /var/lib/jenkins/.kube/config \
```
        
2. <span data-ttu-id="6aa5a-143">Z volaných ověřte, že Kubernetes cluster je přístupný.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-143">Validate from Jenkins that the Kubernetes cluster is accessible.</span></span>  <span data-ttu-id="6aa5a-144">K tomu, SSH do virtuálního počítače volaných: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-144">To do this, SSH into the Jenkins VM: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.</span></span>  <span data-ttu-id="6aa5a-145">Dále ověřte volaných můžete úspěšně připojit ke clusteru: `kubectl cluster-info`.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-145">Next, verify Jenkins can successfully connect to your cluster: `kubectl cluster-info`.</span></span>
    

## <a name="create-a-jenkins-workflow"></a><span data-ttu-id="6aa5a-146">Vytvoření pracovního postupu volaných</span><span class="sxs-lookup"><span data-stu-id="6aa5a-146">Create a Jenkins workflow</span></span>

### <a name="prerequisites"></a><span data-ttu-id="6aa5a-147">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6aa5a-147">Prerequisites</span></span>

- <span data-ttu-id="6aa5a-148">Účet GitHub pro kód úložišti.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-148">GitHub account for code repo.</span></span>
- <span data-ttu-id="6aa5a-149">Docker Hub účet k ukládání a aktualizaci bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-149">Docker Hub account to store and update images.</span></span>
- <span data-ttu-id="6aa5a-150">Kontejnerizované aplikace, které je možné znovu sestavit a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-150">Containerized application that can be rebuilt and updated.</span></span> <span data-ttu-id="6aa5a-151">Můžete použít této ukázkové kontejneru aplikace napsané v Golang: https://github.com/chzbrgr71/go-web</span><span class="sxs-lookup"><span data-stu-id="6aa5a-151">You can use this sample container app written in Golang: https://github.com/chzbrgr71/go-web</span></span> 

> [!NOTE]
> <span data-ttu-id="6aa5a-152">Je třeba provést následující kroky v účtu Githubu.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-152">The following steps must be performed in your own GitHub account.</span></span> <span data-ttu-id="6aa5a-153">Nebojte se naklonujte výše úložiště, ale svůj vlastní účet musíte použít ke konfiguraci webhooků, které a volaných přístup.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-153">Feel free to clone the above repo, but you must use your own account to configure the webhooks and Jenkins access.</span></span>
>

### <a name="step-1-deploy-initial-v1-of-application"></a><span data-ttu-id="6aa5a-154">Krok 1: Nasazení počáteční v1 aplikace</span><span class="sxs-lookup"><span data-stu-id="6aa5a-154">Step 1: Deploy initial v1 of application</span></span>
1. <span data-ttu-id="6aa5a-155">Sestavení aplikace z počítače vývojáře pomocí následujících příkazů.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-155">Build the app from the developer machine with the following commands.</span></span> <span data-ttu-id="6aa5a-156">Nahraďte `myrepo` vlastními.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-156">Replace `myrepo` with your own.</span></span>
    
    ```bash
    git clone https://github.com/chzbrgr71/go-web.git
    cd go-web
    docker build -t myrepo/go-web .
    ```

2. <span data-ttu-id="6aa5a-157">Doručte bitové kopie do úložiště Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-157">Push image to Docker Hub.</span></span>

    ```bash
    docker login
    docker push myrepo/go-web
    ```

3. <span data-ttu-id="6aa5a-158">Nasaďte do Kubernetes clusteru.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-158">Deploy to the Kubernetes cluster.</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="6aa5a-159">Upravit `go-web.yaml` souboru k aktualizaci kontejneru image a úložišti.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-159">Edit the `go-web.yaml` file to update your container image and repo.</span></span>
    >
        
    ```bash
    kubectl create -f ./go-web.yaml --record
    ```
### <a name="step-2-configure-jenkins-system"></a><span data-ttu-id="6aa5a-160">Krok 2: Konfigurace volaných systému</span><span class="sxs-lookup"><span data-stu-id="6aa5a-160">Step 2: Configure Jenkins system</span></span>
1. <span data-ttu-id="6aa5a-161">Klikněte na tlačítko **spravovat volaných** > **konfiguraci systému**.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-161">Click **Manage Jenkins** > **Configure System**.</span></span>
2. <span data-ttu-id="6aa5a-162">V části **Githubu**, vyberte **přidat Server Githubu**.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-162">Under **GitHub**, select **Add GitHub Server**.</span></span>
3. <span data-ttu-id="6aa5a-163">Nechte **adresy URL rozhraní API** jako výchozí.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-163">Leave **API URL** as default.</span></span>
4. <span data-ttu-id="6aa5a-164">V části **pověření**, přidat pomocí přihlašovacích údajů volaných **tajný text**.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-164">Under **Credentials**, add a Jenkins credential using **Secret text**.</span></span> <span data-ttu-id="6aa5a-165">Doporučujeme používat Githubu osobní přístupové tokeny, které jsou nakonfigurované v nastavení účtu uživatele Githubu.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-165">We recommend using GitHub personal access tokens, which are configured in your GitHub user account settings.</span></span> <span data-ttu-id="6aa5a-166">Další informace o to [sem.](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)</span><span class="sxs-lookup"><span data-stu-id="6aa5a-166">More details on this [here.](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)</span></span>
5. <span data-ttu-id="6aa5a-167">Klikněte na tlačítko **testovací připojení** Chcete-li to správně nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-167">Click **Test connection** to ensure this is configured correctly.</span></span>
6. <span data-ttu-id="6aa5a-168">V části **vlastnosti globálních**, přidejte proměnná prostředí `DOCKER_HUB` a zadejte heslo úložiště Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-168">Under **Global Properties**, add an environment variable `DOCKER_HUB` and provide your Docker Hub password.</span></span> <span data-ttu-id="6aa5a-169">(To je užitečné v této ukázce, ale produkční scénář by vyžadovaly bezpečnější přístup.)</span><span class="sxs-lookup"><span data-stu-id="6aa5a-169">(This is useful in this demo, but a production scenario would require a more secure approach.)</span></span>
7. <span data-ttu-id="6aa5a-170">Uložte.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-170">Save.</span></span>

![Přístup volaných Githubu](./media/container-service-kubernetes-jenkins/jenkins-github-access.png)

### <a name="step-3-create-the-jenkins-workflow"></a><span data-ttu-id="6aa5a-172">Krok 3: Vytvoření pracovního postupu volaných</span><span class="sxs-lookup"><span data-stu-id="6aa5a-172">Step 3: Create the Jenkins workflow</span></span>
1. <span data-ttu-id="6aa5a-173">Vytvořte položku volaných.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-173">Create a Jenkins item.</span></span>
2. <span data-ttu-id="6aa5a-174">Zadejte název (například "přejděte – web") a vyberte **volný styl projektu**.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-174">Provide a name (for example, "go-web") and select **Freestyle Project**.</span></span> 
3. <span data-ttu-id="6aa5a-175">Zkontrolujte **Githubu projektu** a zadejte adresu URL vašeho úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-175">Check **GitHub project** and provide the URL to your GitHub repo.</span></span>
4. <span data-ttu-id="6aa5a-176">V **správu zdrojového kódu**, zadejte adresu URL úložiště GitHub a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-176">In **Source Code Management**, provide the GitHub repo URL and credentials.</span></span> 
5. <span data-ttu-id="6aa5a-177">Přidat **sestavení krok** typu **spustit prostředí** a použijte následující text:</span><span class="sxs-lookup"><span data-stu-id="6aa5a-177">Add a **Build Step** of type **Execute shell** and use the following text:</span></span>

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME .
    docker login -u <your-dockerhub-username> -p ${DOCKER_HUB}
    docker push $WEB_IMAGE_NAME
    ```

6. <span data-ttu-id="6aa5a-178">Přidejte další **sestavení krok** typu **spustit prostředí** a použijte následující text:</span><span class="sxs-lookup"><span data-stu-id="6aa5a-178">Add another **Build Step** of type **Execute shell** and use the following text:</span></span>

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    kubectl set image deployment/go-web go-web=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
    ```

![Kroky sestavení volaných](./media/container-service-kubernetes-jenkins/jenkins-build-steps.png)
    
7. <span data-ttu-id="6aa5a-180">Uložit položky volaných a testování pomocí **sestavení teď**.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-180">Save the Jenkins item and test with **Build Now**.</span></span>

### <a name="step-4-connect-github-webhook"></a><span data-ttu-id="6aa5a-181">Krok 4: Připojení webhook Githubu</span><span class="sxs-lookup"><span data-stu-id="6aa5a-181">Step 4: Connect GitHub webhook</span></span>
1. <span data-ttu-id="6aa5a-182">V položce volaných jste vytvořili, klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-182">In the Jenkins item you created, click **Configure**.</span></span>
2. <span data-ttu-id="6aa5a-183">V části **sestavení aktivační události**, vyberte **Githubu háku aktivační událost pro dotazování GITScm** a **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-183">Under **Build Triggers**, select **GitHub hook trigger for GITScm polling** and **Save**.</span></span> <span data-ttu-id="6aa5a-184">Tím se automaticky nakonfiguruje webhook Githubu.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-184">This automatically configures the GitHub webhook.</span></span>
3. <span data-ttu-id="6aa5a-185">V úložiště GitHub pro přejděte web, klikněte na **Nastavení > Webhooky**.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-185">In your GitHub repo for go-web, click **Settings > Webhooks**.</span></span>
4. <span data-ttu-id="6aa5a-186">Ověřte, že adresa URL webhooku volaných byl úspěšně přidán.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-186">Verify that the Jenkins webhook URL was added successfully.</span></span> <span data-ttu-id="6aa5a-187">Adresa URL musí končit "webhook githubu".</span><span class="sxs-lookup"><span data-stu-id="6aa5a-187">The URL should end in "github-webhook".</span></span>

![Konfigurace webhooku volaných](./media/container-service-kubernetes-jenkins/jenkins-webhook.png)

## <a name="test-the-cicd-process-end-to-end"></a><span data-ttu-id="6aa5a-189">Otestujte proces CI/CD koncová</span><span class="sxs-lookup"><span data-stu-id="6aa5a-189">Test the CI/CD process end to end</span></span>

1. <span data-ttu-id="6aa5a-190">Aktualizujte kód pro úložiště a nabízených nebo synchronizace s úložištěm GitHub.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-190">Update code for the repo and push/synch with the GitHub repository.</span></span>
2. <span data-ttu-id="6aa5a-191">Z konzoly volaných, zkontrolujte **sestavení historie** a ověřit, jestli se úloha spustila.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-191">From the Jenkins console, check the **Build History** and validate that the job has run.</span></span> <span data-ttu-id="6aa5a-192">Výstup konzoly zobrazení zobrazíte podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-192">View console output to see details.</span></span>
3. <span data-ttu-id="6aa5a-193">Z Kubernetes zobrazte podrobnosti o upgradované nasazení:</span><span class="sxs-lookup"><span data-stu-id="6aa5a-193">From Kubernetes, view details of the upgraded deployment:</span></span>

    ```bash
    kubectl rollout history deployment/go-web
    ```

## <a name="next-steps"></a><span data-ttu-id="6aa5a-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6aa5a-194">Next steps</span></span>

- <span data-ttu-id="6aa5a-195">Nasaďte registru kontejner Azure a uložení bitové kopie v zabezpečené úložiště.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-195">Deploy Azure Container Registry and store images in a secure repository.</span></span> <span data-ttu-id="6aa5a-196">V tématu [dokumentace Azure kontejneru registru](https://docs.microsoft.com/azure/container-registry).</span><span class="sxs-lookup"><span data-stu-id="6aa5a-196">See [Azure Container Registry docs](https://docs.microsoft.com/azure/container-registry).</span></span>
- <span data-ttu-id="6aa5a-197">Vytvořte složitější pracovní postup, který obsahuje vedle sebe nasazení a automatizovaných testů ve volaných.</span><span class="sxs-lookup"><span data-stu-id="6aa5a-197">Build a more complex workflow that includes side-by-side deployment and automated tests in Jenkins.</span></span>
- <span data-ttu-id="6aa5a-198">Další informace o CI/CD s volaných a Kubernetes najdete v tématu [volaných blog](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).</span><span class="sxs-lookup"><span data-stu-id="6aa5a-198">For more information about CI/CD with Jenkins and Kubernetes, see the [Jenkins blog](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).</span></span>
