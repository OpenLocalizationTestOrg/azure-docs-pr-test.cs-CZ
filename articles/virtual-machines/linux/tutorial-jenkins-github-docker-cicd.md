---
title: "Vytvoření kanálu vývoj v Azure pomocí volaných | Microsoft Docs"
description: "Naučte se vytvořit virtuální počítač volaných v Azure, který vrátí z webu GitHub na každém potvrzení kódu a vytvoří nový kontejner Docker ke spouštění vaší aplikace"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d9849b5e061dd7f2ae0744a3522dc2eb1fb37035
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-create-a-development-infrastructure-on-a-linux-vm-in-azure-with-jenkins-github-and-docker"></a><span data-ttu-id="24551-103">Postup vytvoření vývoj infrastruktury na virtuální počítač s Linuxem v Azure pomocí volaných Githubu a Docker</span><span class="sxs-lookup"><span data-stu-id="24551-103">How to create a development infrastructure on a Linux VM in Azure with Jenkins, GitHub, and Docker</span></span>
<span data-ttu-id="24551-104">K automatizaci fázi sestavení a testování pro vývoj aplikací, můžete použít průběžnou integraci a nasazení (CI/CD) kanálu.</span><span class="sxs-lookup"><span data-stu-id="24551-104">To automate the build and test phase of application development, you can use a continuous integration and deployment (CI/CD) pipeline.</span></span> <span data-ttu-id="24551-105">V tomto kurzu vytvoříte kanál CI/CD na virtuální počítač Azure včetně postup:</span><span class="sxs-lookup"><span data-stu-id="24551-105">In this tutorial, you create a CI/CD pipeline on an Azure VM including how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="24551-106">Vytvoření virtuálního počítače volaných</span><span class="sxs-lookup"><span data-stu-id="24551-106">Create a Jenkins VM</span></span>
> * <span data-ttu-id="24551-107">Instalace a konfigurace volaných</span><span class="sxs-lookup"><span data-stu-id="24551-107">Install and configure Jenkins</span></span>
> * <span data-ttu-id="24551-108">Vytvoření webhooku integrace mezi Githubu a volaných</span><span class="sxs-lookup"><span data-stu-id="24551-108">Create webhook integration between GitHub and Jenkins</span></span>
> * <span data-ttu-id="24551-109">Vytvořit a spustit úlohy sestavení volaných z Githubu potvrzení</span><span class="sxs-lookup"><span data-stu-id="24551-109">Create and trigger Jenkins build jobs from GitHub commits</span></span>
> * <span data-ttu-id="24551-110">Vytvoření bitové kopie Docker pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="24551-110">Create a Docker image for your app</span></span>
> * <span data-ttu-id="24551-111">Ověřte, že potvrzení Githubu vytvářet novou bitovou kopii Docker a spuštěné aplikace</span><span class="sxs-lookup"><span data-stu-id="24551-111">Verify GitHub commits build new Docker image and updates running app</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="24551-112">Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="24551-112">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="24551-113">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="24551-113">Run `az --version` to find the version.</span></span> <span data-ttu-id="24551-114">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="24551-114">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-jenkins-instance"></a><span data-ttu-id="24551-115">Vytvoření instance volaných</span><span class="sxs-lookup"><span data-stu-id="24551-115">Create Jenkins instance</span></span>
<span data-ttu-id="24551-116">V předchozích kurz [postup přizpůsobení virtuální počítač s Linuxem na při prvním spuštění](tutorial-automate-vm-deployment.md), jste se dozvěděli, jak automatizovat přizpůsobení virtuálního počítače s inicializací cloudu.</span><span class="sxs-lookup"><span data-stu-id="24551-116">In a previous tutorial on [How to customize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md), you learned how to automate VM customization with cloud-init.</span></span> <span data-ttu-id="24551-117">Tento kurz používá soubor cloudu init volaných a Docker nainstalujete na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="24551-117">This tutorial uses a cloud-init file to install Jenkins and Docker on a VM.</span></span> 

<span data-ttu-id="24551-118">V aktuálním prostředí, vytvořte soubor s názvem *cloudu init.txt* a vložte následující konfigurace.</span><span class="sxs-lookup"><span data-stu-id="24551-118">In your current shell, create a file named *cloud-init.txt* and paste the following configuration.</span></span> <span data-ttu-id="24551-119">Například vytvoření souboru v prostředí cloudu není na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="24551-119">For example, create the file in the Cloud Shell not on your local machine.</span></span> <span data-ttu-id="24551-120">Zadejte `sensible-editor cloud-init-jenkins.txt` k vytvoření tohoto souboru a zobrazit seznam dostupných editory.</span><span class="sxs-lookup"><span data-stu-id="24551-120">Enter `sensible-editor cloud-init-jenkins.txt` to create the file and see a list of available editors.</span></span> <span data-ttu-id="24551-121">Ujistěte se, že je soubor celou cloudu init zkopírován správně, obzvláště první řádek:</span><span class="sxs-lookup"><span data-stu-id="24551-121">Make sure that the whole cloud-init file is copied correctly, especially the first line:</span></span>

```yaml
#cloud-config
package_upgrade: true
write_files:
  - path: /etc/systemd/system/docker.service.d/docker.conf
    content: |
      [Service]
        ExecStart=
        ExecStart=/usr/bin/dockerd
  - path: /etc/docker/daemon.json
    content: |
      {
        "hosts": ["fd://","tcp://127.0.0.1:2375"]
      }
runcmd:
  - wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | apt-key add -
  - sh -c 'echo deb http://pkg.jenkins-ci.org/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
  - apt-get update && apt-get install jenkins -y
  - curl -sSL https://get.docker.com/ | sh
  - usermod -aG docker azureuser
  - usermod -aG docker jenkins
  - service jenkins restart
```

<span data-ttu-id="24551-122">Před vytvořením virtuálního počítače, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="24551-122">Before you can create a VM, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="24551-123">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupJenkins* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="24551-123">The following example creates a resource group named *myResourceGroupJenkins* in the *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupJenkins --location eastus
```

<span data-ttu-id="24551-124">Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="24551-124">Now create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="24551-125">Použití `--custom-data` parametr předávat do cloudu init konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="24551-125">Use the `--custom-data` parameter to pass in your cloud-init config file.</span></span> <span data-ttu-id="24551-126">Zadejte úplnou cestu k *cloudu. init jenkins.txt* Pokud jste uložili soubor mimo pracovní adresář existuje.</span><span class="sxs-lookup"><span data-stu-id="24551-126">Provide the full path to *cloud-init-jenkins.txt* if you saved the file outside of your present working directory.</span></span>

```azurecli-interactive 
az vm create --resource-group myResourceGroupJenkins \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-jenkins.txt
```

<span data-ttu-id="24551-127">Trvá několik minut pro virtuální počítač pro vytvoření a konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="24551-127">It takes a few minutes for the VM to be created and configured.</span></span>

<span data-ttu-id="24551-128">Povolit webový provoz připojit virtuální počítač, použijte [az virtuálních počítačů open-port](/cli/azure/vm#open-port) otevřít port *8080* volaných provozu a portů *1337* pro aplikaci Node.js, která se používá ke spuštění ukázkové aplikace:</span><span class="sxs-lookup"><span data-stu-id="24551-128">To allow web traffic to reach your VM, use [az vm open-port](/cli/azure/vm#open-port) to open port *8080* for Jenkins traffic and port *1337* for the Node.js app that is used to run a sample app:</span></span>

```azurecli-interactive 
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 8080 --priority 1001
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 1337 --priority 1002
```


## <a name="configure-jenkins"></a><span data-ttu-id="24551-129">Konfigurace volaných</span><span class="sxs-lookup"><span data-stu-id="24551-129">Configure Jenkins</span></span>
<span data-ttu-id="24551-130">Chcete-li získat přístup k vaší instanci volaných, získejte veřejnou IP adresu vašeho virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="24551-130">To access your Jenkins instance, obtain the public IP address of your VM:</span></span>

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

<span data-ttu-id="24551-131">Z bezpečnostních důvodů musíte zadat heslo počáteční správce, které je uložený v textovém souboru na vašem virtuálním počítači spusťte instalaci volaných.</span><span class="sxs-lookup"><span data-stu-id="24551-131">For security purposes, you need to enter the initial admin password that is stored in a text file on your VM to start the Jenkins install.</span></span> <span data-ttu-id="24551-132">Použijte veřejnou IP adresu získaných v předchozím kroku do SSH pro virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="24551-132">Use the public IP address obtained in the previous step to SSH to your VM:</span></span>

```bash
ssh azureuser@<publicIps>
```

<span data-ttu-id="24551-133">Zobrazení `initialAdminPassword` pro vaše volaných nainstalovat a zkopírujte ho:</span><span class="sxs-lookup"><span data-stu-id="24551-133">View the `initialAdminPassword` for your Jenkins install and copy it:</span></span>

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

<span data-ttu-id="24551-134">Pokud se soubor ještě není k dispozici, počkejte několik minut cloudu init pro dokončení instalace volaných a Docker.</span><span class="sxs-lookup"><span data-stu-id="24551-134">If the file isn't available yet, wait a couple more minutes for cloud-init to complete the Jenkins and Docker install.</span></span>

<span data-ttu-id="24551-135">Nyní otevřete webový prohlížeč a přejděte na `http://<publicIps>:8080`.</span><span class="sxs-lookup"><span data-stu-id="24551-135">Now open a web browser and go to `http://<publicIps>:8080`.</span></span> <span data-ttu-id="24551-136">Dokončete počáteční nastavení volaných následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="24551-136">Complete the initial Jenkins setup as follows:</span></span>

- <span data-ttu-id="24551-137">Zadejte *initialAdminPassword* získané z virtuálního počítače v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="24551-137">Enter the *initialAdminPassword* obtained from the VM in the previous step.</span></span>
- <span data-ttu-id="24551-138">Klikněte na tlačítko **vyberte modulů plug-in pro instalaci**</span><span class="sxs-lookup"><span data-stu-id="24551-138">Click **Select plugins to install**</span></span>
- <span data-ttu-id="24551-139">Vyhledejte *Githubu* do textového pole v horní části, vyberte *modulu plug-in Githubu*, pak klikněte na tlačítko **instalace**</span><span class="sxs-lookup"><span data-stu-id="24551-139">Search for *GitHub* in the text box across the top, select the *GitHub plugin*, then click **Install**</span></span>
- <span data-ttu-id="24551-140">Pokud chcete vytvořit uživatelský účet volaných, vyplňte formulář podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="24551-140">To create a Jenkins user account, fill out the form as desired.</span></span> <span data-ttu-id="24551-141">Z hlediska zabezpečení byste měli vytvořit tohoto prvního uživatele volaných spíše než budete pokračovat, jako výchozí účet správce.</span><span class="sxs-lookup"><span data-stu-id="24551-141">From a security perspective, you should create this first Jenkins user rather than continuing as the default admin account.</span></span>
- <span data-ttu-id="24551-142">Po dokončení klikněte na tlačítko **začít používat volaných**</span><span class="sxs-lookup"><span data-stu-id="24551-142">When finished, click **Start using Jenkins**</span></span>


## <a name="create-github-webhook"></a><span data-ttu-id="24551-143">Vytvoření webhook Githubu</span><span class="sxs-lookup"><span data-stu-id="24551-143">Create GitHub webhook</span></span>
<span data-ttu-id="24551-144">Chcete-li nakonfigurovat integraci s Githubu, otevřete [ukázkovou aplikaci Node.js Hello World](https://github.com/Azure-Samples/nodejs-docs-hello-world) z úložiště ukázek Azure.</span><span class="sxs-lookup"><span data-stu-id="24551-144">To configure the integration with GitHub, open the [Node.js Hello World sample app](https://github.com/Azure-Samples/nodejs-docs-hello-world) from the Azure samples repo.</span></span> <span data-ttu-id="24551-145">Chcete-li rozvětvit úložiště k účtu GitHub, klikněte **rozvětvení** tlačítko v horním pravém rohu.</span><span class="sxs-lookup"><span data-stu-id="24551-145">To fork the repo to your own GitHub account, click the **Fork** button in the top right-hand corner.</span></span>

<span data-ttu-id="24551-146">Vytvoření webhooku uvnitř rozvětvení, kterou jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="24551-146">Create a webhook inside the fork you created:</span></span>

- <span data-ttu-id="24551-147">Klikněte na tlačítko **nastavení**, pak vyberte **integrace & služby** na levé straně.</span><span class="sxs-lookup"><span data-stu-id="24551-147">Click **Settings**, then select **Integrations & services** on the left-hand side.</span></span>
- <span data-ttu-id="24551-148">Klikněte na tlačítko **přidat službu**, pak zadejte *volaných* pole filtru.</span><span class="sxs-lookup"><span data-stu-id="24551-148">Click **Add service**, then enter *Jenkins* in filter box.</span></span>
- <span data-ttu-id="24551-149">Vyberte *volaných (modul plug-in Githubu)*</span><span class="sxs-lookup"><span data-stu-id="24551-149">Select *Jenkins (GitHub plugin)*</span></span>
- <span data-ttu-id="24551-150">Pro **volaných napojit URL**, zadejte `http://<publicIps>:8080/github-webhook/`.</span><span class="sxs-lookup"><span data-stu-id="24551-150">For the **Jenkins hook URL**, enter `http://<publicIps>:8080/github-webhook/`.</span></span> <span data-ttu-id="24551-151">Zajistěte, aby zahrnout koncový znak /</span><span class="sxs-lookup"><span data-stu-id="24551-151">Make sure you include the trailing /</span></span>
- <span data-ttu-id="24551-152">Klikněte na tlačítko **přidat službu**</span><span class="sxs-lookup"><span data-stu-id="24551-152">Click **Add service**</span></span>

![Přidat do vašeho úložiště forked webhook Githubu](media/tutorial-jenkins-github-docker-cicd/github_webhook.png)


## <a name="create-jenkins-job"></a><span data-ttu-id="24551-154">Vytvořit úlohu volaných</span><span class="sxs-lookup"><span data-stu-id="24551-154">Create Jenkins job</span></span>
<span data-ttu-id="24551-155">Pokud chcete, aby volaných reakce na události na Githubu, například potvrzením kódu, vytvořte úlohu volaných.</span><span class="sxs-lookup"><span data-stu-id="24551-155">To have Jenkins respond to an event in GitHub such as committing code, create a Jenkins job.</span></span> 

<span data-ttu-id="24551-156">Ve vašem webu volaných, klikněte na tlačítko **vytvoření nové úlohy** z domovské stránky:</span><span class="sxs-lookup"><span data-stu-id="24551-156">In your Jenkins website, click **Create new jobs** from the home page:</span></span>

- <span data-ttu-id="24551-157">Zadejte *HelloWorld* jako název úlohy.</span><span class="sxs-lookup"><span data-stu-id="24551-157">Enter *HelloWorld* as job name.</span></span> <span data-ttu-id="24551-158">Zvolte **volný styl projektu**, pak vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="24551-158">Choose **Freestyle project**, then select **OK**.</span></span>
- <span data-ttu-id="24551-159">V části **Obecné** vyberte **Githubu** projektu a zadejte URL forked úložiště, jako například *https://github.com/iainfoulds/nodejs-docs-hello-world*</span><span class="sxs-lookup"><span data-stu-id="24551-159">Under the **General** section, select **GitHub** project and enter your forked repo URL, such as *https://github.com/iainfoulds/nodejs-docs-hello-world*</span></span>
- <span data-ttu-id="24551-160">V části **zdrojového kódu správu** vyberte **Git**, zadejte vaše forked úložišti *.git* adresu URL, například *https://github.com/iainfoulds/nodejs-docs-hello-world.git*</span><span class="sxs-lookup"><span data-stu-id="24551-160">Under the **Source code management** section, select **Git**, enter your forked repo *.git* URL, such as *https://github.com/iainfoulds/nodejs-docs-hello-world.git*</span></span>
- <span data-ttu-id="24551-161">V části **sestavení aktivační události** vyberte **Githubu háku aktivační událost pro dotazování GITscm**.</span><span class="sxs-lookup"><span data-stu-id="24551-161">Under the **Build Triggers** section, select **GitHub hook trigger for GITscm polling**.</span></span>
- <span data-ttu-id="24551-162">V části **sestavení** zvolte **přidat krok sestavení**.</span><span class="sxs-lookup"><span data-stu-id="24551-162">Under the **Build** section, choose **Add build step**.</span></span> <span data-ttu-id="24551-163">Vyberte **spustit prostředí**, pak zadejte `echo "Testing"` v příkazovém okně.</span><span class="sxs-lookup"><span data-stu-id="24551-163">Select **Execute shell**, then enter `echo "Testing"` in to command window.</span></span>
- <span data-ttu-id="24551-164">Klikněte na tlačítko **Uložit** v dolní části okna úlohy.</span><span class="sxs-lookup"><span data-stu-id="24551-164">Click **Save** at the bottom of the jobs window.</span></span>


## <a name="test-github-integration"></a><span data-ttu-id="24551-165">Testování integrace Githubu</span><span class="sxs-lookup"><span data-stu-id="24551-165">Test GitHub integration</span></span>
<span data-ttu-id="24551-166">K testování Githubu integraci s volaných, potvrďte změnu ve vašem rozvětvení.</span><span class="sxs-lookup"><span data-stu-id="24551-166">To test the GitHub integration with Jenkins, commit a change in your fork.</span></span> 

<span data-ttu-id="24551-167">Zpět v Githubu webové uživatelské rozhraní, vyberte vaše forked úložišti a klikněte **index.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="24551-167">Back in GitHub web UI, select your forked repo, and then click the **index.js** file.</span></span> <span data-ttu-id="24551-168">Klikněte na ikonu tužky k úpravám tohoto souboru, takže přečte řádek 6:</span><span class="sxs-lookup"><span data-stu-id="24551-168">Click the pencil icon to edit this file so line 6 reads:</span></span>

```nodejs
response.end("Hello World!");
```

<span data-ttu-id="24551-169">Chcete-li potvrdit změny, klikněte na tlačítko **potvrzení změn** tlačítko dole.</span><span class="sxs-lookup"><span data-stu-id="24551-169">To commit your changes, click the **Commit changes** button at the bottom.</span></span>

<span data-ttu-id="24551-170">Ve volaných, spustí nového sestavení v části **sestavení historie** části levém dolním rohu stránku úlohy.</span><span class="sxs-lookup"><span data-stu-id="24551-170">In Jenkins, a new build starts under the **Build history** section of the bottom left-hand corner of your job page.</span></span> <span data-ttu-id="24551-171">Klikněte na odkaz číslo sestavení a vyberte **konzole výstup** na levé straně velikosti.</span><span class="sxs-lookup"><span data-stu-id="24551-171">Click the build number link and select **Console output** on the left-hand size.</span></span> <span data-ttu-id="24551-172">Můžete si zobrazit kroky volaných přebírá jako kód pocházejí z Githubu a akce sestavení výstupy zprávu `Testing` ke konzole.</span><span class="sxs-lookup"><span data-stu-id="24551-172">You can view the steps Jenkins takes as your code is pulled from GitHub and the build action outputs the message `Testing` to the console.</span></span> <span data-ttu-id="24551-173">Pokaždé, když je potvrzení změn provedených v Githubu, webhooku spojí do volaných a aktivovat nové sestavení tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="24551-173">Each time a commit is made in GitHub, the webhook reaches out to Jenkins and trigger a new build in this way.</span></span>


## <a name="define-docker-build-image"></a><span data-ttu-id="24551-174">Definujte image Docker sestavení</span><span class="sxs-lookup"><span data-stu-id="24551-174">Define Docker build image</span></span>
<span data-ttu-id="24551-175">Umožňuje zobrazit spuštěné na základě na vaše potvrzení Githubu aplikace Node.js sestavení Docker bitové kopie a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="24551-175">To see the Node.js app running based on your GitHub commits, lets build a Docker image to run the app.</span></span> <span data-ttu-id="24551-176">Obrázek je sestaven z soubor Docker, která definuje konfiguraci kontejneru, který spouští aplikaci.</span><span class="sxs-lookup"><span data-stu-id="24551-176">The image is built from a Dockerfile that defines how to configure the container that runs the app.</span></span> 

<span data-ttu-id="24551-177">Ze SSH připojení k virtuálnímu počítači přejděte do adresáře prostoru volaných s názvem po úkol, který jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="24551-177">From the SSH connection to your VM, change to the Jenkins workspace directory named after the job you created in a previous step.</span></span> <span data-ttu-id="24551-178">V našem příkladu, který byl s názvem *HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="24551-178">In our example, that was named *HelloWorld*.</span></span>

```bash
cd /var/lib/jenkins/workspace/HelloWorld
```

<span data-ttu-id="24551-179">Vytvořte soubor s v tomto adresáři prostoru s `sudo sensible-editor Dockerfile` a vložte následující obsah.</span><span class="sxs-lookup"><span data-stu-id="24551-179">Create a file with in this workspace directory with `sudo sensible-editor Dockerfile` and paste the following contents.</span></span> <span data-ttu-id="24551-180">Ujistěte se, jestli je správně, zkopírovali celý soubor Docker obzvláště první řádek:</span><span class="sxs-lookup"><span data-stu-id="24551-180">Make sure that the whole Dockerfile is copied correctly, especially the first line:</span></span>

```yaml
FROM node:alpine

EXPOSE 1337

WORKDIR /var/www
COPY package.json /var/www/
RUN npm install
COPY index.js /var/www/
```

<span data-ttu-id="24551-181">Tento soubor Docker používá základní Node.js bitové kopie pomocí Alpine Linux, port zpřístupňuje 1337, který běží aplikace Hello World, pak zkopíruje soubory aplikace a inicializaci.</span><span class="sxs-lookup"><span data-stu-id="24551-181">This Dockerfile uses the base Node.js image using Alpine Linux, exposes port 1337 that the Hello World app runs on, then copies the app files and initializes it.</span></span>


## <a name="create-jenkins-build-rules"></a><span data-ttu-id="24551-182">Vytvoření pravidla volaných sestavení</span><span class="sxs-lookup"><span data-stu-id="24551-182">Create Jenkins build rules</span></span>
<span data-ttu-id="24551-183">V předchozím kroku jste vytvořili základní pravidlo volaných sestavení, které na výstup zprávu do konzoly.</span><span class="sxs-lookup"><span data-stu-id="24551-183">In a previous step, you created a basic Jenkins build rule that output a message to the console.</span></span> <span data-ttu-id="24551-184">Umožňuje vytvořit krok sestavení pomocí našich soubor Docker a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="24551-184">Lets create the build step to use our Dockerfile and run the app.</span></span>

<span data-ttu-id="24551-185">Zpět v instanci volaných vyberte úlohu, kterou jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="24551-185">Back in your Jenkins instance, select the job you created in a previous step.</span></span> <span data-ttu-id="24551-186">Klikněte na tlačítko **konfigurace** na levé straně a posuňte se dolů **sestavení** části:</span><span class="sxs-lookup"><span data-stu-id="24551-186">Click **Configure** on the left-hand side and scroll down to the **Build** section:</span></span>

- <span data-ttu-id="24551-187">Odeberte stávající `echo "Test"` kroku sestavení.</span><span class="sxs-lookup"><span data-stu-id="24551-187">Remove your existing `echo "Test"` build step.</span></span> <span data-ttu-id="24551-188">Klikněte na tlačítko červený křížek v pravém horním rohu pole pro stávající krok sestavení.</span><span class="sxs-lookup"><span data-stu-id="24551-188">Click the red cross on the top right-hand corner of the existing build step box.</span></span>
- <span data-ttu-id="24551-189">Klikněte na tlačítko **přidat krok sestavení**, pak vyberte **spustit prostředí**</span><span class="sxs-lookup"><span data-stu-id="24551-189">Click **Add build step**, then select **Execute shell**</span></span>
- <span data-ttu-id="24551-190">V **příkaz** pole, zadejte následující příkazy Docker a potom vyberte **Uložit**:</span><span class="sxs-lookup"><span data-stu-id="24551-190">In the **Command** box, enter the following Docker commands, then select **Save**:</span></span>

  ```bash
  docker build --tag helloworld:$BUILD_NUMBER .
  docker stop helloworld && docker rm helloworld
  docker run --name helloworld -p 1337:1337 helloworld:$BUILD_NUMBER node /var/www/index.js &
  ```

<span data-ttu-id="24551-191">Kroky sestavení Docker vytvoření bitové kopie a značky její volaných číslo sestavení tak spravovat historii bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="24551-191">The Docker build steps create an image and tag it with the Jenkins build number so you can maintain a history of images.</span></span> <span data-ttu-id="24551-192">Žádné existující kontejnery spuštění aplikace jsou zastavena a pak odebral.</span><span class="sxs-lookup"><span data-stu-id="24551-192">Any existing containers running the app are stopped and then removed.</span></span> <span data-ttu-id="24551-193">Nový kontejner je pak spuštěna pomocí image a spouští aplikace Node.js založené na nejnovější potvrzení v Githubu.</span><span class="sxs-lookup"><span data-stu-id="24551-193">A new container is then started using the image and runs your Node.js app based on the latest commits in GitHub.</span></span>


## <a name="test-your-pipeline"></a><span data-ttu-id="24551-194">Testování vaší kanálu</span><span class="sxs-lookup"><span data-stu-id="24551-194">Test your pipeline</span></span>
<span data-ttu-id="24551-195">Chcete-li zobrazit celý kanálu v akci, upravte *index.js* v vaší forked úložiště GitHub znovu soubor a klikněte na tlačítko **Potvrdit změnu**.</span><span class="sxs-lookup"><span data-stu-id="24551-195">To see the whole pipeline in action, edit the *index.js* file in your forked GitHub repo again and click **Commit change**.</span></span> <span data-ttu-id="24551-196">Nová úloha se spustí v volaných podle webhooku pro GitHub.</span><span class="sxs-lookup"><span data-stu-id="24551-196">A new job starts in Jenkins based on the webhook for GitHub.</span></span> <span data-ttu-id="24551-197">Jak dlouho trvá několik sekund pro vytvoření bitové kopie Docker a spusťte aplikaci v nový kontejner.</span><span class="sxs-lookup"><span data-stu-id="24551-197">It takes a few seconds to create the Docker image and start your app in a new container.</span></span>

<span data-ttu-id="24551-198">V případě potřeby znovu získejte veřejnou IP adresu vašeho virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="24551-198">If needed, obtain the public IP address of your VM again:</span></span>

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

<span data-ttu-id="24551-199">Otevřete webový prohlížeč a zadejte `http://<publicIps>:1337`.</span><span class="sxs-lookup"><span data-stu-id="24551-199">Open a web browser and enter `http://<publicIps>:1337`.</span></span> <span data-ttu-id="24551-200">Aplikace Node.js se zobrazí a odráží nejnovější potvrzení v vaší Githubu rozvětvení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="24551-200">Your Node.js app is displayed and reflects the latest commits in your GitHub fork as follows:</span></span>

![Spuštěné aplikace Node.js](media/tutorial-jenkins-github-docker-cicd/running_nodejs_app.png)

<span data-ttu-id="24551-202">Nyní provést jiný úprava *index.js* souborů na webu GitHub a potvrďte změny.</span><span class="sxs-lookup"><span data-stu-id="24551-202">Now make another edit to the *index.js* file in GitHub and commit the change.</span></span> <span data-ttu-id="24551-203">Počkejte několik sekund pro úlohu dokončit v volaných a potom aktualizujte webový prohlížeč zobrazíte aktualizovanou verzi vaší aplikace spuštěné v nový kontejner následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="24551-203">Wait a few seconds for the job to complete in Jenkins, then refresh your web browser to see the updated version of your app running in a new container as follows:</span></span>

![Spuštění aplikace Node.js po další potvrzení Githubu](media/tutorial-jenkins-github-docker-cicd/another_running_nodejs_app.png)


## <a name="next-steps"></a><span data-ttu-id="24551-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="24551-205">Next steps</span></span>
<span data-ttu-id="24551-206">V tomto kurzu jste nakonfigurovali Githubu spustit úlohu volaných sestavení v každém potvrzení kódu a pak nasadit kontejner Docker k testování aplikace.</span><span class="sxs-lookup"><span data-stu-id="24551-206">In this tutorial, you configured GitHub to run a Jenkins build job on each code commit and then deploy a Docker container to test your app.</span></span> <span data-ttu-id="24551-207">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="24551-207">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="24551-208">Vytvoření virtuálního počítače volaných</span><span class="sxs-lookup"><span data-stu-id="24551-208">Create a Jenkins VM</span></span>
> * <span data-ttu-id="24551-209">Instalace a konfigurace volaných</span><span class="sxs-lookup"><span data-stu-id="24551-209">Install and configure Jenkins</span></span>
> * <span data-ttu-id="24551-210">Vytvoření webhooku integrace mezi Githubu a volaných</span><span class="sxs-lookup"><span data-stu-id="24551-210">Create webhook integration between GitHub and Jenkins</span></span>
> * <span data-ttu-id="24551-211">Vytvořit a spustit úlohy sestavení volaných z Githubu potvrzení</span><span class="sxs-lookup"><span data-stu-id="24551-211">Create and trigger Jenkins build jobs from GitHub commits</span></span>
> * <span data-ttu-id="24551-212">Vytvoření bitové kopie Docker pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="24551-212">Create a Docker image for your app</span></span>
> * <span data-ttu-id="24551-213">Ověřte, že potvrzení Githubu vytvářet novou bitovou kopii Docker a spuštěné aplikace</span><span class="sxs-lookup"><span data-stu-id="24551-213">Verify GitHub commits build new Docker image and updates running app</span></span>

<span data-ttu-id="24551-214">Přechodu na v dalším kurzu se dozvíte více o tom, jak integrovat volaných Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="24551-214">Advance to the next tutorial to learn more about how to integrate Jenkins with Visual Studio Team Services.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="24551-215">Nasazení aplikací s volaných a Team Services</span><span class="sxs-lookup"><span data-stu-id="24551-215">Deploy apps with Jenkins and Team Services</span></span>](tutorial-build-deploy-jenkins.md)