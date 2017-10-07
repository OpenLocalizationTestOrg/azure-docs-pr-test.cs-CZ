---
title: "aaaCreate vývoj kanál v Azure s volaných | Microsoft Docs"
description: "Zjistěte, jak toocreate volaných virtuální počítač v Azure, aby si z webu GitHub na každý kód potvrzení a vytvoří novou Docker kontejneru toorun vaší aplikace"
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
ms.openlocfilehash: c079e3c9186c9da0a3e51e1823215779c565e0dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-development-infrastructure-on-a-linux-vm-in-azure-with-jenkins-github-and-docker"></a><span data-ttu-id="e74c5-103">Jak toocreate vývoj infrastruktury na virtuální počítač s Linuxem v Azure pomocí volaných Githubu a Docker</span><span class="sxs-lookup"><span data-stu-id="e74c5-103">How toocreate a development infrastructure on a Linux VM in Azure with Jenkins, GitHub, and Docker</span></span>
<span data-ttu-id="e74c5-104">tooautomate hello sestavení a testování fáze vývoje aplikací, můžete použít průběžnou integraci a nasazení (CI/CD) kanálu.</span><span class="sxs-lookup"><span data-stu-id="e74c5-104">tooautomate hello build and test phase of application development, you can use a continuous integration and deployment (CI/CD) pipeline.</span></span> <span data-ttu-id="e74c5-105">V tomto kurzu vytvoříte kanál CI/CD na virtuální počítač Azure včetně postup:</span><span class="sxs-lookup"><span data-stu-id="e74c5-105">In this tutorial, you create a CI/CD pipeline on an Azure VM including how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e74c5-106">Vytvoření virtuálního počítače volaných</span><span class="sxs-lookup"><span data-stu-id="e74c5-106">Create a Jenkins VM</span></span>
> * <span data-ttu-id="e74c5-107">Instalace a konfigurace volaných</span><span class="sxs-lookup"><span data-stu-id="e74c5-107">Install and configure Jenkins</span></span>
> * <span data-ttu-id="e74c5-108">Vytvoření webhooku integrace mezi Githubu a volaných</span><span class="sxs-lookup"><span data-stu-id="e74c5-108">Create webhook integration between GitHub and Jenkins</span></span>
> * <span data-ttu-id="e74c5-109">Vytvořit a spustit úlohy sestavení volaných z Githubu potvrzení</span><span class="sxs-lookup"><span data-stu-id="e74c5-109">Create and trigger Jenkins build jobs from GitHub commits</span></span>
> * <span data-ttu-id="e74c5-110">Vytvoření bitové kopie Docker pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e74c5-110">Create a Docker image for your app</span></span>
> * <span data-ttu-id="e74c5-111">Ověřte, že potvrzení Githubu vytvářet novou bitovou kopii Docker a spuštěné aplikace</span><span class="sxs-lookup"><span data-stu-id="e74c5-111">Verify GitHub commits build new Docker image and updates running app</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="e74c5-112">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e74c5-112">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="e74c5-113">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="e74c5-113">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="e74c5-114">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e74c5-114">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-jenkins-instance"></a><span data-ttu-id="e74c5-115">Vytvoření instance volaných</span><span class="sxs-lookup"><span data-stu-id="e74c5-115">Create Jenkins instance</span></span>
<span data-ttu-id="e74c5-116">V předchozích kurzu na [jak toocustomize virtuální počítač s Linuxem na při prvním spuštění](tutorial-automate-vm-deployment.md), jste se naučili jak tooautomate přizpůsobení virtuálního počítače s inicializací cloudu.</span><span class="sxs-lookup"><span data-stu-id="e74c5-116">In a previous tutorial on [How toocustomize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md), you learned how tooautomate VM customization with cloud-init.</span></span> <span data-ttu-id="e74c5-117">Tento kurz používá cloudu init souboru tooinstall volaných a Docker na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="e74c5-117">This tutorial uses a cloud-init file tooinstall Jenkins and Docker on a VM.</span></span> 

<span data-ttu-id="e74c5-118">V aktuálním prostředí, vytvořte soubor s názvem *cloudu init.txt* a hello vložte následující konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e74c5-118">In your current shell, create a file named *cloud-init.txt* and paste hello following configuration.</span></span> <span data-ttu-id="e74c5-119">Můžete například vytvořte soubor hello v hello cloudové prostředí není na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="e74c5-119">For example, create hello file in hello Cloud Shell not on your local machine.</span></span> <span data-ttu-id="e74c5-120">Zadejte `sensible-editor cloud-init-jenkins.txt` toocreate hello souboru a zobrazit seznam dostupných editory.</span><span class="sxs-lookup"><span data-stu-id="e74c5-120">Enter `sensible-editor cloud-init-jenkins.txt` toocreate hello file and see a list of available editors.</span></span> <span data-ttu-id="e74c5-121">Ujistěte se, že tento soubor celou cloudu init hello správně zkopírován, zejména hello první řádek:</span><span class="sxs-lookup"><span data-stu-id="e74c5-121">Make sure that hello whole cloud-init file is copied correctly, especially hello first line:</span></span>

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

<span data-ttu-id="e74c5-122">Před vytvořením virtuálního počítače, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="e74c5-122">Before you can create a VM, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="e74c5-123">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupJenkins* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="e74c5-123">hello following example creates a resource group named *myResourceGroupJenkins* in hello *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupJenkins --location eastus
```

<span data-ttu-id="e74c5-124">Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="e74c5-124">Now create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="e74c5-125">Použití hello `--custom-data` parametr toopass v souboru config init cloudu.</span><span class="sxs-lookup"><span data-stu-id="e74c5-125">Use hello `--custom-data` parameter toopass in your cloud-init config file.</span></span> <span data-ttu-id="e74c5-126">Zadejte úplnou cestu hello příliš*cloudu. init jenkins.txt* Pokud jste uložili soubor hello mimo pracovní adresář existuje.</span><span class="sxs-lookup"><span data-stu-id="e74c5-126">Provide hello full path too*cloud-init-jenkins.txt* if you saved hello file outside of your present working directory.</span></span>

```azurecli-interactive 
az vm create --resource-group myResourceGroupJenkins \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-jenkins.txt
```

<span data-ttu-id="e74c5-127">Jak dlouho trvá několik minut, než hello virtuálních počítačů toobe vytvořený a nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="e74c5-127">It takes a few minutes for hello VM toobe created and configured.</span></span>

<span data-ttu-id="e74c5-128">tooallow virtuálního počítače, použijte webový provoz tooreach [open-port az virtuálního](/cli/azure/vm#open-port) tooopen port *8080* volaných provozu a portů *1337* pro aplikaci Node.js hello, který je použité toorun ukázkovou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="e74c5-128">tooallow web traffic tooreach your VM, use [az vm open-port](/cli/azure/vm#open-port) tooopen port *8080* for Jenkins traffic and port *1337* for hello Node.js app that is used toorun a sample app:</span></span>

```azurecli-interactive 
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 8080 --priority 1001
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 1337 --priority 1002
```


## <a name="configure-jenkins"></a><span data-ttu-id="e74c5-129">Konfigurace volaných</span><span class="sxs-lookup"><span data-stu-id="e74c5-129">Configure Jenkins</span></span>
<span data-ttu-id="e74c5-130">tooaccess vaše volaných instance, získejte hello veřejnou IP adresu vašeho virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="e74c5-130">tooaccess your Jenkins instance, obtain hello public IP address of your VM:</span></span>

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

<span data-ttu-id="e74c5-131">Z bezpečnostních důvodů musíte heslo tooenter hello počáteční správce, který je uložený v textovém souboru na váš virtuální počítač toostart hello volaných nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="e74c5-131">For security purposes, you need tooenter hello initial admin password that is stored in a text file on your VM toostart hello Jenkins install.</span></span> <span data-ttu-id="e74c5-132">Použijte hello veřejnou IP adresu získat v hello předchozí krok tooSSH tooyour virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="e74c5-132">Use hello public IP address obtained in hello previous step tooSSH tooyour VM:</span></span>

```bash
ssh azureuser@<publicIps>
```

<span data-ttu-id="e74c5-133">Zobrazení hello `initialAdminPassword` pro vaše volaných nainstalovat a zkopírujte ho:</span><span class="sxs-lookup"><span data-stu-id="e74c5-133">View hello `initialAdminPassword` for your Jenkins install and copy it:</span></span>

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

<span data-ttu-id="e74c5-134">Pokud ještě není k dispozici soubor hello, počkejte několik minut na cloudu init toocomplete hello volaných a Docker instalaci.</span><span class="sxs-lookup"><span data-stu-id="e74c5-134">If hello file isn't available yet, wait a couple more minutes for cloud-init toocomplete hello Jenkins and Docker install.</span></span>

<span data-ttu-id="e74c5-135">Nyní otevřete webový prohlížeč a přejděte příliš`http://<publicIps>:8080`.</span><span class="sxs-lookup"><span data-stu-id="e74c5-135">Now open a web browser and go too`http://<publicIps>:8080`.</span></span> <span data-ttu-id="e74c5-136">Dokončete počáteční nastavení volaných hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e74c5-136">Complete hello initial Jenkins setup as follows:</span></span>

- <span data-ttu-id="e74c5-137">Zadejte hello *initialAdminPassword* získané z hello virtuálních počítačů v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="e74c5-137">Enter hello *initialAdminPassword* obtained from hello VM in hello previous step.</span></span>
- <span data-ttu-id="e74c5-138">Klikněte na tlačítko **vyberte tooinstall moduly plug-in**</span><span class="sxs-lookup"><span data-stu-id="e74c5-138">Click **Select plugins tooinstall**</span></span>
- <span data-ttu-id="e74c5-139">Vyhledejte *Githubu* hello textového pole v horní části hello, vyberte hello *modulu plug-in Githubu*, pak klikněte na tlačítko **instalace**</span><span class="sxs-lookup"><span data-stu-id="e74c5-139">Search for *GitHub* in hello text box across hello top, select hello *GitHub plugin*, then click **Install**</span></span>
- <span data-ttu-id="e74c5-140">toocreate volaných uživatelský účet, vyplňte formulář hello podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="e74c5-140">toocreate a Jenkins user account, fill out hello form as desired.</span></span> <span data-ttu-id="e74c5-141">Z hlediska zabezpečení byste měli vytvořit tohoto prvního uživatele volaných spíše než budete pokračovat jako hello výchozího správce účtu.</span><span class="sxs-lookup"><span data-stu-id="e74c5-141">From a security perspective, you should create this first Jenkins user rather than continuing as hello default admin account.</span></span>
- <span data-ttu-id="e74c5-142">Po dokončení klikněte na tlačítko **začít používat volaných**</span><span class="sxs-lookup"><span data-stu-id="e74c5-142">When finished, click **Start using Jenkins**</span></span>


## <a name="create-github-webhook"></a><span data-ttu-id="e74c5-143">Vytvoření webhook Githubu</span><span class="sxs-lookup"><span data-stu-id="e74c5-143">Create GitHub webhook</span></span>
<span data-ttu-id="e74c5-144">tooconfigure hello integrace s Githubu, otevřete hello [ukázkovou aplikaci Node.js Hello, World](https://github.com/Azure-Samples/nodejs-docs-hello-world) z hello ukázek Azure úložišti.</span><span class="sxs-lookup"><span data-stu-id="e74c5-144">tooconfigure hello integration with GitHub, open hello [Node.js Hello World sample app](https://github.com/Azure-Samples/nodejs-docs-hello-world) from hello Azure samples repo.</span></span> <span data-ttu-id="e74c5-145">toofork hello úložišti tooyour vlastní účet GitHub, klikněte na tlačítko hello **rozvětvení** tlačítko v horním pravém rohu hello.</span><span class="sxs-lookup"><span data-stu-id="e74c5-145">toofork hello repo tooyour own GitHub account, click hello **Fork** button in hello top right-hand corner.</span></span>

<span data-ttu-id="e74c5-146">Vytvoření webhooku uvnitř hello rozvětvení, kterou jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="e74c5-146">Create a webhook inside hello fork you created:</span></span>

- <span data-ttu-id="e74c5-147">Klikněte na tlačítko **nastavení**, pak vyberte **integrace & služby** na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="e74c5-147">Click **Settings**, then select **Integrations & services** on hello left-hand side.</span></span>
- <span data-ttu-id="e74c5-148">Klikněte na tlačítko **přidat službu**, pak zadejte *volaných* pole filtru.</span><span class="sxs-lookup"><span data-stu-id="e74c5-148">Click **Add service**, then enter *Jenkins* in filter box.</span></span>
- <span data-ttu-id="e74c5-149">Vyberte *volaných (modul plug-in Githubu)*</span><span class="sxs-lookup"><span data-stu-id="e74c5-149">Select *Jenkins (GitHub plugin)*</span></span>
- <span data-ttu-id="e74c5-150">Pro hello **volaných napojit URL**, zadejte `http://<publicIps>:8080/github-webhook/`.</span><span class="sxs-lookup"><span data-stu-id="e74c5-150">For hello **Jenkins hook URL**, enter `http://<publicIps>:8080/github-webhook/`.</span></span> <span data-ttu-id="e74c5-151">Zajistěte, aby zahrnete hello koncové nebo</span><span class="sxs-lookup"><span data-stu-id="e74c5-151">Make sure you include hello trailing /</span></span>
- <span data-ttu-id="e74c5-152">Klikněte na tlačítko **přidat službu**</span><span class="sxs-lookup"><span data-stu-id="e74c5-152">Click **Add service**</span></span>

![Přidání úložiště tooyour forked webhooku GitHub](media/tutorial-jenkins-github-docker-cicd/github_webhook.png)


## <a name="create-jenkins-job"></a><span data-ttu-id="e74c5-154">Vytvořit úlohu volaných</span><span class="sxs-lookup"><span data-stu-id="e74c5-154">Create Jenkins job</span></span>
<span data-ttu-id="e74c5-155">toohave volaných reakce tooan událost v Githubu například potvrzením kódu, vytvořit úlohu volaných.</span><span class="sxs-lookup"><span data-stu-id="e74c5-155">toohave Jenkins respond tooan event in GitHub such as committing code, create a Jenkins job.</span></span> 

<span data-ttu-id="e74c5-156">Ve vašem webu volaných, klikněte na tlačítko **vytvoření nové úlohy** z hello domovské stránky:</span><span class="sxs-lookup"><span data-stu-id="e74c5-156">In your Jenkins website, click **Create new jobs** from hello home page:</span></span>

- <span data-ttu-id="e74c5-157">Zadejte *HelloWorld* jako název úlohy.</span><span class="sxs-lookup"><span data-stu-id="e74c5-157">Enter *HelloWorld* as job name.</span></span> <span data-ttu-id="e74c5-158">Zvolte **volný styl projektu**, pak vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="e74c5-158">Choose **Freestyle project**, then select **OK**.</span></span>
- <span data-ttu-id="e74c5-159">V části hello **Obecné** vyberte **Githubu** projektu a zadejte URL forked úložiště, jako například *https://github.com/iainfoulds/nodejs-docs-hello-world*</span><span class="sxs-lookup"><span data-stu-id="e74c5-159">Under hello **General** section, select **GitHub** project and enter your forked repo URL, such as *https://github.com/iainfoulds/nodejs-docs-hello-world*</span></span>
- <span data-ttu-id="e74c5-160">V části hello **zdrojového kódu správy** vyberte **Git**, zadejte vaše forked úložišti *.git* adresu URL, například *https://github.com/iainfoulds/nodejs-docs-hello-world.git*</span><span class="sxs-lookup"><span data-stu-id="e74c5-160">Under hello **Source code management** section, select **Git**, enter your forked repo *.git* URL, such as *https://github.com/iainfoulds/nodejs-docs-hello-world.git*</span></span>
- <span data-ttu-id="e74c5-161">V části hello **sestavení aktivační události** vyberte **Githubu háku aktivační událost pro dotazování GITscm**.</span><span class="sxs-lookup"><span data-stu-id="e74c5-161">Under hello **Build Triggers** section, select **GitHub hook trigger for GITscm polling**.</span></span>
- <span data-ttu-id="e74c5-162">V části hello **sestavení** zvolte **přidat krok sestavení**.</span><span class="sxs-lookup"><span data-stu-id="e74c5-162">Under hello **Build** section, choose **Add build step**.</span></span> <span data-ttu-id="e74c5-163">Vyberte **spustit prostředí**, pak zadejte `echo "Testing"` v okně toocommand.</span><span class="sxs-lookup"><span data-stu-id="e74c5-163">Select **Execute shell**, then enter `echo "Testing"` in toocommand window.</span></span>
- <span data-ttu-id="e74c5-164">Klikněte na tlačítko **Uložit** v hello dolní části okna úloh hello.</span><span class="sxs-lookup"><span data-stu-id="e74c5-164">Click **Save** at hello bottom of hello jobs window.</span></span>


## <a name="test-github-integration"></a><span data-ttu-id="e74c5-165">Testování integrace Githubu</span><span class="sxs-lookup"><span data-stu-id="e74c5-165">Test GitHub integration</span></span>
<span data-ttu-id="e74c5-166">hello tootest Githubu integrace s volaných, Potvrdit změnu vaší rozvětvení.</span><span class="sxs-lookup"><span data-stu-id="e74c5-166">tootest hello GitHub integration with Jenkins, commit a change in your fork.</span></span> 

<span data-ttu-id="e74c5-167">Zpět v Githubu webové uživatelské rozhraní, vyberte vaše forked úložiště a pak klikněte hello **index.js** souboru.</span><span class="sxs-lookup"><span data-stu-id="e74c5-167">Back in GitHub web UI, select your forked repo, and then click hello **index.js** file.</span></span> <span data-ttu-id="e74c5-168">Klikněte na tlačítko tooedit ikonu tužky hello tento soubor tak přečte řádek 6:</span><span class="sxs-lookup"><span data-stu-id="e74c5-168">Click hello pencil icon tooedit this file so line 6 reads:</span></span>

```nodejs
response.end("Hello World!");
```

<span data-ttu-id="e74c5-169">toocommit změny, klikněte na tlačítko hello **potvrzení změn** tlačítko dole v hello.</span><span class="sxs-lookup"><span data-stu-id="e74c5-169">toocommit your changes, click hello **Commit changes** button at hello bottom.</span></span>

<span data-ttu-id="e74c5-170">Ve volaných, spustí nového sestavení v části hello **sestavení historie** části hello levém dolním rohu stránku úlohy.</span><span class="sxs-lookup"><span data-stu-id="e74c5-170">In Jenkins, a new build starts under hello **Build history** section of hello bottom left-hand corner of your job page.</span></span> <span data-ttu-id="e74c5-171">Klikněte na odkaz číslo sestavení hello a vyberte **konzole výstup** na levé straně velikosti hello.</span><span class="sxs-lookup"><span data-stu-id="e74c5-171">Click hello build number link and select **Console output** on hello left-hand size.</span></span> <span data-ttu-id="e74c5-172">Můžete zobrazit kroky hello volaných přebírá jako kód pocházejí z Githubu a hello sestavení akce výstupy uvítací zprávu `Testing` toohello konzoly.</span><span class="sxs-lookup"><span data-stu-id="e74c5-172">You can view hello steps Jenkins takes as your code is pulled from GitHub and hello build action outputs hello message `Testing` toohello console.</span></span> <span data-ttu-id="e74c5-173">Pokaždé, když je potvrzení změn provedených v Githubu, webhooku hello spojí tooJenkins a aktivovat nové sestavení tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="e74c5-173">Each time a commit is made in GitHub, hello webhook reaches out tooJenkins and trigger a new build in this way.</span></span>


## <a name="define-docker-build-image"></a><span data-ttu-id="e74c5-174">Definujte image Docker sestavení</span><span class="sxs-lookup"><span data-stu-id="e74c5-174">Define Docker build image</span></span>
<span data-ttu-id="e74c5-175">aplikaci Node.js hello toosee spuštěnou na základě na vaše potvrzení Githubu umožňuje sestavení Docker aplikace hello toorun bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="e74c5-175">toosee hello Node.js app running based on your GitHub commits, lets build a Docker image toorun hello app.</span></span> <span data-ttu-id="e74c5-176">Obrázek Hello je sestaven z soubor Docker, která definuje, jak tooconfigure hello kontejneru, který spouští aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="e74c5-176">hello image is built from a Dockerfile that defines how tooconfigure hello container that runs hello app.</span></span> 

<span data-ttu-id="e74c5-177">Z hello SSH připojení tooyour virtuálních počítačů změňte toohello volaných prostoru adresář s názvem po hello úlohy, kterou jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="e74c5-177">From hello SSH connection tooyour VM, change toohello Jenkins workspace directory named after hello job you created in a previous step.</span></span> <span data-ttu-id="e74c5-178">V našem příkladu, který byl s názvem *HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="e74c5-178">In our example, that was named *HelloWorld*.</span></span>

```bash
cd /var/lib/jenkins/workspace/HelloWorld
```

<span data-ttu-id="e74c5-179">Vytvořte soubor s v tomto adresáři prostoru s `sudo sensible-editor Dockerfile` a hello vložte následující obsah.</span><span class="sxs-lookup"><span data-stu-id="e74c5-179">Create a file with in this workspace directory with `sudo sensible-editor Dockerfile` and paste hello following contents.</span></span> <span data-ttu-id="e74c5-180">Ujistěte se, že hello celý soubor Docker je zkopírován správně, zejména hello první řádek:</span><span class="sxs-lookup"><span data-stu-id="e74c5-180">Make sure that hello whole Dockerfile is copied correctly, especially hello first line:</span></span>

```yaml
FROM node:alpine

EXPOSE 1337

WORKDIR /var/www
COPY package.json /var/www/
RUN npm install
COPY index.js /var/www/
```

<span data-ttu-id="e74c5-181">Tento soubor Docker používá základní image Node.js hello pomocí Alpine Linux, zpřístupňuje port 1337 hello Hello World aplikace běží, pak zkopíruje soubory aplikace hello a inicializaci.</span><span class="sxs-lookup"><span data-stu-id="e74c5-181">This Dockerfile uses hello base Node.js image using Alpine Linux, exposes port 1337 that hello Hello World app runs on, then copies hello app files and initializes it.</span></span>


## <a name="create-jenkins-build-rules"></a><span data-ttu-id="e74c5-182">Vytvoření pravidla volaných sestavení</span><span class="sxs-lookup"><span data-stu-id="e74c5-182">Create Jenkins build rules</span></span>
<span data-ttu-id="e74c5-183">V předchozím kroku jste vytvořili základní pravidlo volaných sestavení, které výstup konzoly toohello zprávy.</span><span class="sxs-lookup"><span data-stu-id="e74c5-183">In a previous step, you created a basic Jenkins build rule that output a message toohello console.</span></span> <span data-ttu-id="e74c5-184">Umožňuje vytvořit naše soubor Docker toouse krok hello sestavení a spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="e74c5-184">Lets create hello build step toouse our Dockerfile and run hello app.</span></span>

<span data-ttu-id="e74c5-185">Zpět v instanci volaných vyberte hello úlohy, kterou jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="e74c5-185">Back in your Jenkins instance, select hello job you created in a previous step.</span></span> <span data-ttu-id="e74c5-186">Klikněte na tlačítko **konfigurace** na levé straně hello a přejděte dolů toohello **sestavení** části:</span><span class="sxs-lookup"><span data-stu-id="e74c5-186">Click **Configure** on hello left-hand side and scroll down toohello **Build** section:</span></span>

- <span data-ttu-id="e74c5-187">Odeberte stávající `echo "Test"` kroku sestavení.</span><span class="sxs-lookup"><span data-stu-id="e74c5-187">Remove your existing `echo "Test"` build step.</span></span> <span data-ttu-id="e74c5-188">Klikněte na tlačítko hello red křížové v horním pravém rohu hello hello existující sestavení krok pole.</span><span class="sxs-lookup"><span data-stu-id="e74c5-188">Click hello red cross on hello top right-hand corner of hello existing build step box.</span></span>
- <span data-ttu-id="e74c5-189">Klikněte na tlačítko **přidat krok sestavení**, pak vyberte **spustit prostředí**</span><span class="sxs-lookup"><span data-stu-id="e74c5-189">Click **Add build step**, then select **Execute shell**</span></span>
- <span data-ttu-id="e74c5-190">V hello **příkaz** pole, zadejte následující příkazy Docker hello a potom vyberte **Uložit**:</span><span class="sxs-lookup"><span data-stu-id="e74c5-190">In hello **Command** box, enter hello following Docker commands, then select **Save**:</span></span>

  ```bash
  docker build --tag helloworld:$BUILD_NUMBER .
  docker stop helloworld && docker rm helloworld
  docker run --name helloworld -p 1337:1337 helloworld:$BUILD_NUMBER node /var/www/index.js &
  ```

<span data-ttu-id="e74c5-191">kroky sestavení Docker Hello vytvoření bitové kopie a značky její hello volaných číslo sestavení tak spravovat historii bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="e74c5-191">hello Docker build steps create an image and tag it with hello Jenkins build number so you can maintain a history of images.</span></span> <span data-ttu-id="e74c5-192">Žádné existující kontejnery systémem hello aplikace jsou zastavena a pak odebral.</span><span class="sxs-lookup"><span data-stu-id="e74c5-192">Any existing containers running hello app are stopped and then removed.</span></span> <span data-ttu-id="e74c5-193">Nový kontejner je pak spuštěna pomocí bitové kopie hello a spouští aplikace Node.js založené na nejnovější potvrzení hello v Githubu.</span><span class="sxs-lookup"><span data-stu-id="e74c5-193">A new container is then started using hello image and runs your Node.js app based on hello latest commits in GitHub.</span></span>


## <a name="test-your-pipeline"></a><span data-ttu-id="e74c5-194">Testování vaší kanálu</span><span class="sxs-lookup"><span data-stu-id="e74c5-194">Test your pipeline</span></span>
<span data-ttu-id="e74c5-195">toosee hello celého kanálu v akci, upravit hello *index.js* v vaší forked úložiště GitHub znovu soubor a klikněte na tlačítko **Potvrdit změnu**.</span><span class="sxs-lookup"><span data-stu-id="e74c5-195">toosee hello whole pipeline in action, edit hello *index.js* file in your forked GitHub repo again and click **Commit change**.</span></span> <span data-ttu-id="e74c5-196">Nová úloha se spustí v volaných podle hello webhooku pro GitHub.</span><span class="sxs-lookup"><span data-stu-id="e74c5-196">A new job starts in Jenkins based on hello webhook for GitHub.</span></span> <span data-ttu-id="e74c5-197">Trvá několik sekund toocreate hello Docker image a spusťte aplikaci v nový kontejner.</span><span class="sxs-lookup"><span data-stu-id="e74c5-197">It takes a few seconds toocreate hello Docker image and start your app in a new container.</span></span>

<span data-ttu-id="e74c5-198">V případě potřeby znovu získejte hello veřejnou IP adresu vašeho virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="e74c5-198">If needed, obtain hello public IP address of your VM again:</span></span>

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

<span data-ttu-id="e74c5-199">Otevřete webový prohlížeč a zadejte `http://<publicIps>:1337`.</span><span class="sxs-lookup"><span data-stu-id="e74c5-199">Open a web browser and enter `http://<publicIps>:1337`.</span></span> <span data-ttu-id="e74c5-200">Aplikace Node.js se zobrazí a odráží hello nejnovější potvrzení v vaší Githubu rozvětvení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e74c5-200">Your Node.js app is displayed and reflects hello latest commits in your GitHub fork as follows:</span></span>

![Spuštěné aplikace Node.js](media/tutorial-jenkins-github-docker-cicd/running_nodejs_app.png)

<span data-ttu-id="e74c5-202">Nyní provést další úpravy toohello *index.js* souboru v Githubu a potvrzení změn hello.</span><span class="sxs-lookup"><span data-stu-id="e74c5-202">Now make another edit toohello *index.js* file in GitHub and commit hello change.</span></span> <span data-ttu-id="e74c5-203">Počkejte několik sekund pro úlohy toocomplete hello ve volaných, a poté obnovte verze webového prohlížeče toosee hello aktualizovat aplikace spuštěné v nový kontejner následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e74c5-203">Wait a few seconds for hello job toocomplete in Jenkins, then refresh your web browser toosee hello updated version of your app running in a new container as follows:</span></span>

![Spuštění aplikace Node.js po další potvrzení Githubu](media/tutorial-jenkins-github-docker-cicd/another_running_nodejs_app.png)


## <a name="next-steps"></a><span data-ttu-id="e74c5-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e74c5-205">Next steps</span></span>
<span data-ttu-id="e74c5-206">V tomto kurzu jste nakonfigurovali Githubu toorun úlohu sestavení volaných na každém potvrzení kódu a pak nasadit tootest kontejner Docker vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e74c5-206">In this tutorial, you configured GitHub toorun a Jenkins build job on each code commit and then deploy a Docker container tootest your app.</span></span> <span data-ttu-id="e74c5-207">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="e74c5-207">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e74c5-208">Vytvoření virtuálního počítače volaných</span><span class="sxs-lookup"><span data-stu-id="e74c5-208">Create a Jenkins VM</span></span>
> * <span data-ttu-id="e74c5-209">Instalace a konfigurace volaných</span><span class="sxs-lookup"><span data-stu-id="e74c5-209">Install and configure Jenkins</span></span>
> * <span data-ttu-id="e74c5-210">Vytvoření webhooku integrace mezi Githubu a volaných</span><span class="sxs-lookup"><span data-stu-id="e74c5-210">Create webhook integration between GitHub and Jenkins</span></span>
> * <span data-ttu-id="e74c5-211">Vytvořit a spustit úlohy sestavení volaných z Githubu potvrzení</span><span class="sxs-lookup"><span data-stu-id="e74c5-211">Create and trigger Jenkins build jobs from GitHub commits</span></span>
> * <span data-ttu-id="e74c5-212">Vytvoření bitové kopie Docker pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e74c5-212">Create a Docker image for your app</span></span>
> * <span data-ttu-id="e74c5-213">Ověřte, že potvrzení Githubu vytvářet novou bitovou kopii Docker a spuštěné aplikace</span><span class="sxs-lookup"><span data-stu-id="e74c5-213">Verify GitHub commits build new Docker image and updates running app</span></span>

<span data-ttu-id="e74c5-214">Posunutí toohello další kurz toolearn informace o toointegrate volaných s Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="e74c5-214">Advance toohello next tutorial toolearn more about how toointegrate Jenkins with Visual Studio Team Services.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e74c5-215">Nasazení aplikací s volaných a Team Services</span><span class="sxs-lookup"><span data-stu-id="e74c5-215">Deploy apps with Jenkins and Team Services</span></span>](tutorial-build-deploy-jenkins.md)