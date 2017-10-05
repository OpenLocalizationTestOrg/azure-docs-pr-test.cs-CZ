---
title: "Přizpůsobení virtuálního počítače s Linuxem na při prvním spuštění v Azure | Microsoft Docs"
description: "Další informace o použití cloudu init a Key Vault pro virtuální počítače s Linuxem customze při prvním spuštění v Azure"
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
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 6adf4e43aa80c28c6f5f8d8a071966323ba85723
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-customize-a-linux-virtual-machine-on-first-boot"></a><span data-ttu-id="0f28d-103">Postup přizpůsobení virtuální počítač s Linuxem na při prvním spuštění</span><span class="sxs-lookup"><span data-stu-id="0f28d-103">How to customize a Linux virtual machine on first boot</span></span>
<span data-ttu-id="0f28d-104">V předchozích kurzu jste se dozvěděli, jak chcete SSH pro virtuální počítač (VM) a ručně nainstalujte NGINX.</span><span class="sxs-lookup"><span data-stu-id="0f28d-104">In a previous tutorial, you learned how to SSH to a virtual machine (VM) and manually install NGINX.</span></span> <span data-ttu-id="0f28d-105">Pokud chcete vytvořit virtuální počítače rychlý a konzistentním způsobem, je obvykle potřeby nějaký způsob automatizace.</span><span class="sxs-lookup"><span data-stu-id="0f28d-105">To create VMs in a quick and consistent manner, some form of automation is typically desired.</span></span> <span data-ttu-id="0f28d-106">Běžný postup přizpůsobení virtuálního počítače při prvním spuštění počítače se má používat [cloudu init](https://cloudinit.readthedocs.io).</span><span class="sxs-lookup"><span data-stu-id="0f28d-106">A common approach to customize a VM on first boot is to use [cloud-init](https://cloudinit.readthedocs.io).</span></span> <span data-ttu-id="0f28d-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="0f28d-107">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0f28d-108">Vytvořte soubor konfigurace cloudu init</span><span class="sxs-lookup"><span data-stu-id="0f28d-108">Create a cloud-init config file</span></span>
> * <span data-ttu-id="0f28d-109">Vytvořit virtuální počítač, který používá soubor init cloudu</span><span class="sxs-lookup"><span data-stu-id="0f28d-109">Create a VM that uses a cloud-init file</span></span>
> * <span data-ttu-id="0f28d-110">Zobrazit spuštěné aplikaci Node.js, po vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0f28d-110">View a running Node.js app after the VM is created</span></span>
> * <span data-ttu-id="0f28d-111">Pomocí Key Vault bezpečně uložit certifikáty</span><span class="sxs-lookup"><span data-stu-id="0f28d-111">Use Key Vault to securely store certificates</span></span>
> * <span data-ttu-id="0f28d-112">Automatizovat zabezpečená nasazení NGINX s inicializací cloudu</span><span class="sxs-lookup"><span data-stu-id="0f28d-112">Automate secure deployments of NGINX with cloud-init</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="0f28d-113">Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="0f28d-113">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="0f28d-114">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="0f28d-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="0f28d-115">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0f28d-115">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>  



## <a name="cloud-init-overview"></a><span data-ttu-id="0f28d-116">Init cloudu – přehled</span><span class="sxs-lookup"><span data-stu-id="0f28d-116">Cloud-init overview</span></span>
<span data-ttu-id="0f28d-117">[Init cloudu](https://cloudinit.readthedocs.io) je často používaný přístup k přizpůsobení virtuálního počítače s Linuxem, jako při prvním spuštění.</span><span class="sxs-lookup"><span data-stu-id="0f28d-117">[Cloud-init](https://cloudinit.readthedocs.io) is a widely used approach to customize a Linux VM as it boots for the first time.</span></span> <span data-ttu-id="0f28d-118">Init cloudu můžete použít k instalaci balíčků a zapisovat soubory nebo konfigurace zabezpečení a uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0f28d-118">You can use cloud-init to install packages and write files, or to configure users and security.</span></span> <span data-ttu-id="0f28d-119">Init cloudu běží během úvodního spouštění, nejsou žádné další kroky nebo požadované agenty použít konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="0f28d-119">As cloud-init runs during the initial boot process, there are no additional steps or required agents to apply your configuration.</span></span>

<span data-ttu-id="0f28d-120">Init cloudu také funguje v různých distribucí.</span><span class="sxs-lookup"><span data-stu-id="0f28d-120">Cloud-init also works across distributions.</span></span> <span data-ttu-id="0f28d-121">Například nepoužívejte **výstižný get instalace** nebo **yum nainstalovat** nainstalovat balíček.</span><span class="sxs-lookup"><span data-stu-id="0f28d-121">For example, you don't use **apt-get install** or **yum install** to install a package.</span></span> <span data-ttu-id="0f28d-122">Místo toho můžete definovat seznam balíčků pro instalaci.</span><span class="sxs-lookup"><span data-stu-id="0f28d-122">Instead you can define a list of packages to install.</span></span> <span data-ttu-id="0f28d-123">Init cloudu automaticky používá nástroj pro správu nativní balíčku pro distro, kterou vyberete.</span><span class="sxs-lookup"><span data-stu-id="0f28d-123">Cloud-init automatically uses the native package management tool for the distro you select.</span></span>

<span data-ttu-id="0f28d-124">Pracujeme s našimi partnery získat cloudu init zahrnuté a práci v bitové kopie, které poskytují do Azure.</span><span class="sxs-lookup"><span data-stu-id="0f28d-124">We are working with our partners to get cloud-init included and working in the images that they provide to Azure.</span></span> <span data-ttu-id="0f28d-125">Následující tabulka popisuje aktuální dostupnosti cloudu init Image platformy Azure:</span><span class="sxs-lookup"><span data-stu-id="0f28d-125">The following table outlines the current cloud-init availability on Azure platform images:</span></span>

| <span data-ttu-id="0f28d-126">Alias</span><span class="sxs-lookup"><span data-stu-id="0f28d-126">Alias</span></span> | <span data-ttu-id="0f28d-127">Vydavatel</span><span class="sxs-lookup"><span data-stu-id="0f28d-127">Publisher</span></span> | <span data-ttu-id="0f28d-128">Nabídka</span><span class="sxs-lookup"><span data-stu-id="0f28d-128">Offer</span></span> | <span data-ttu-id="0f28d-129">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="0f28d-129">SKU</span></span> | <span data-ttu-id="0f28d-130">Verze</span><span class="sxs-lookup"><span data-stu-id="0f28d-130">Version</span></span> |
|:--- |:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="0f28d-131">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="0f28d-131">UbuntuLTS</span></span> |<span data-ttu-id="0f28d-132">Canonical</span><span class="sxs-lookup"><span data-stu-id="0f28d-132">Canonical</span></span> |<span data-ttu-id="0f28d-133">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="0f28d-133">UbuntuServer</span></span> |<span data-ttu-id="0f28d-134">16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="0f28d-134">16.04-LTS</span></span> |<span data-ttu-id="0f28d-135">nejnovější</span><span class="sxs-lookup"><span data-stu-id="0f28d-135">latest</span></span> |
| <span data-ttu-id="0f28d-136">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="0f28d-136">UbuntuLTS</span></span> |<span data-ttu-id="0f28d-137">Canonical</span><span class="sxs-lookup"><span data-stu-id="0f28d-137">Canonical</span></span> |<span data-ttu-id="0f28d-138">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="0f28d-138">UbuntuServer</span></span> |<span data-ttu-id="0f28d-139">14.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="0f28d-139">14.04.5-LTS</span></span> |<span data-ttu-id="0f28d-140">nejnovější</span><span class="sxs-lookup"><span data-stu-id="0f28d-140">latest</span></span> |
| <span data-ttu-id="0f28d-141">CoreOS</span><span class="sxs-lookup"><span data-stu-id="0f28d-141">CoreOS</span></span> |<span data-ttu-id="0f28d-142">CoreOS</span><span class="sxs-lookup"><span data-stu-id="0f28d-142">CoreOS</span></span> |<span data-ttu-id="0f28d-143">CoreOS</span><span class="sxs-lookup"><span data-stu-id="0f28d-143">CoreOS</span></span> |<span data-ttu-id="0f28d-144">Stable</span><span class="sxs-lookup"><span data-stu-id="0f28d-144">Stable</span></span> |<span data-ttu-id="0f28d-145">nejnovější</span><span class="sxs-lookup"><span data-stu-id="0f28d-145">latest</span></span> |


## <a name="create-cloud-init-config-file"></a><span data-ttu-id="0f28d-146">Vytvoření cloudové init konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="0f28d-146">Create cloud-init config file</span></span>
<span data-ttu-id="0f28d-147">Informace o cloudu init v akci, vytvořte virtuální počítač, který nainstaluje NGINX a spustí jednoduchý "zdravím svět" aplikace Node.js.</span><span class="sxs-lookup"><span data-stu-id="0f28d-147">To see cloud-init in action, create a VM that installs NGINX and runs a simple 'Hello World' Node.js app.</span></span> <span data-ttu-id="0f28d-148">Následující konfigurace cloudu init nainstaluje požadované balíčky, vytvoří aplikace Node.js, potom inicializaci a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="0f28d-148">The following cloud-init configuration installs the required packages, creates a Node.js app, then initialize and starts the app.</span></span>

<span data-ttu-id="0f28d-149">V aktuálním prostředí, vytvořte soubor s názvem *cloudu init.txt* a vložte následující konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0f28d-149">In your current shell, create a file named *cloud-init.txt* and paste the following configuration.</span></span> <span data-ttu-id="0f28d-150">Například vytvoření souboru v prostředí cloudu není na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="0f28d-150">For example, create the file in the Cloud Shell not on your local machine.</span></span> <span data-ttu-id="0f28d-151">Můžete použít libovolný editor, které chcete.</span><span class="sxs-lookup"><span data-stu-id="0f28d-151">You can use any editor you wish.</span></span> <span data-ttu-id="0f28d-152">Zadejte `sensible-editor cloud-init.txt` k vytvoření tohoto souboru a zobrazit seznam dostupných editory.</span><span class="sxs-lookup"><span data-stu-id="0f28d-152">Enter `sensible-editor cloud-init.txt` to create the file and see a list of available editors.</span></span> <span data-ttu-id="0f28d-153">Ujistěte se, že je soubor celou cloudu init zkopírován správně, obzvláště první řádek:</span><span class="sxs-lookup"><span data-stu-id="0f28d-153">Make sure that the whole cloud-init file is copied correctly, especially the first line:</span></span>

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

<span data-ttu-id="0f28d-154">Další informace o možnostech konfigurace cloudu init najdete v tématu [příklady konfigurace cloudu init](https://cloudinit.readthedocs.io/en/latest/topics/examples.html).</span><span class="sxs-lookup"><span data-stu-id="0f28d-154">For more information about cloud-init configuration options, see [cloud-init config examples](https://cloudinit.readthedocs.io/en/latest/topics/examples.html).</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="0f28d-155">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0f28d-155">Create virtual machine</span></span>
<span data-ttu-id="0f28d-156">Před vytvořením virtuálního počítače, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="0f28d-156">Before you can create a VM, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="0f28d-157">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupAutomate* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="0f28d-157">The following example creates a resource group named *myResourceGroupAutomate* in the *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupAutomate --location eastus
```

<span data-ttu-id="0f28d-158">Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="0f28d-158">Now create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="0f28d-159">Použití `--custom-data` parametr předávat do cloudu init konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="0f28d-159">Use the `--custom-data` parameter to pass in your cloud-init config file.</span></span> <span data-ttu-id="0f28d-160">Zadejte úplnou cestu k *cloudu init.txt* konfigurace, pokud jste uložili soubor mimo pracovní adresář existuje.</span><span class="sxs-lookup"><span data-stu-id="0f28d-160">Provide the full path to the *cloud-init.txt* config if you saved the file outside of your present working directory.</span></span> <span data-ttu-id="0f28d-161">Následující příklad vytvoří virtuální počítač s názvem *myAutomatedVM*:</span><span class="sxs-lookup"><span data-stu-id="0f28d-161">The following example creates a VM named *myAutomatedVM*:</span></span>

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

<span data-ttu-id="0f28d-162">Trvá několik minut pro virtuální počítač, který se má vytvořit, balíčky určené k instalaci a aplikaci spusťte.</span><span class="sxs-lookup"><span data-stu-id="0f28d-162">It takes a few minutes for the VM to be created, the packages to install, and the app to start.</span></span> <span data-ttu-id="0f28d-163">Existují úlohy na pozadí, které dál běžet po rozhraní příkazového řádku Azure se vrátíte do řádku.</span><span class="sxs-lookup"><span data-stu-id="0f28d-163">There are background tasks that continue to run after the Azure CLI returns you to the prompt.</span></span> <span data-ttu-id="0f28d-164">To může být jiná několik minut, než může aplikaci používat.</span><span class="sxs-lookup"><span data-stu-id="0f28d-164">It may be another couple of minutes before you can access the app.</span></span> <span data-ttu-id="0f28d-165">Po vytvoření virtuálního počítače, poznamenejte si `publicIpAddress` zobrazí pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="0f28d-165">When the VM has been created, take note of the `publicIpAddress` displayed by the Azure CLI.</span></span> <span data-ttu-id="0f28d-166">Tato adresa se používá pro přístup k aplikaci Node.js pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="0f28d-166">This address is used to access the Node.js app via a web browser.</span></span>

<span data-ttu-id="0f28d-167">Povolit webový provoz připojit virtuální počítač, otevřete port 80 z Internetu s [az virtuálních počítačů open-port](/cli/azure/vm#open-port):</span><span class="sxs-lookup"><span data-stu-id="0f28d-167">To allow web traffic to reach your VM, open port 80 from the Internet with [az vm open-port](/cli/azure/vm#open-port):</span></span>

```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroupAutomate --name myVM
```

## <a name="test-web-app"></a><span data-ttu-id="0f28d-168">Test webové aplikace</span><span class="sxs-lookup"><span data-stu-id="0f28d-168">Test web app</span></span>
<span data-ttu-id="0f28d-169">Nyní můžete otevřít webový prohlížeč a zadejte *http://<publicIpAddress>*  na panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="0f28d-169">Now you can open a web browser and enter *http://<publicIpAddress>* in the address bar.</span></span> <span data-ttu-id="0f28d-170">Zadejte vlastní veřejná IP adresa z virtuálního počítače vytvořit proces.</span><span class="sxs-lookup"><span data-stu-id="0f28d-170">Provide your own public IP address from the VM create process.</span></span> <span data-ttu-id="0f28d-171">Aplikace Node.js se zobrazí jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="0f28d-171">Your Node.js app is displayed as in the following example:</span></span>

![Zobrazení spuštěných NGINX lokality](./media/tutorial-automate-vm-deployment/nginx.png)


## <a name="inject-certificates-from-key-vault"></a><span data-ttu-id="0f28d-173">Vložit certifikáty z Key Vault</span><span class="sxs-lookup"><span data-stu-id="0f28d-173">Inject certificates from Key Vault</span></span>
<span data-ttu-id="0f28d-174">Této volitelné části ukazuje, jak můžete bezpečně uložit certifikáty v Azure Key Vault a vložit je při nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0f28d-174">This optional section shows how you can securely store certificates in Azure Key Vault and inject them during the VM deployment.</span></span> <span data-ttu-id="0f28d-175">Místo použití vlastní image, která obsahuje certifikáty zaručená v, tento proces zajišťuje, že nejaktuálnější certifikáty jsou aplikován na virtuální počítač při prvním spuštění počítače.</span><span class="sxs-lookup"><span data-stu-id="0f28d-175">Rather than using a custom image that includes the certificates baked-in, this process ensures that the most up-to-date certificates are injected to a VM on first boot.</span></span> <span data-ttu-id="0f28d-176">Během procesu certifikát nikdy opustí platformy Azure nebo je vystaven ve skriptu, historie příkazového řádku nebo šablony.</span><span class="sxs-lookup"><span data-stu-id="0f28d-176">During the process, the certificate never leaves the Azure platform or is exposed in a script, command-line history, or template.</span></span>

<span data-ttu-id="0f28d-177">Azure Key Vault chrání kryptografické klíče a tajné klíče, jako je například certifikáty nebo hesla.</span><span class="sxs-lookup"><span data-stu-id="0f28d-177">Azure Key Vault safeguards cryptographic keys and secrets, such as certificates or passwords.</span></span> <span data-ttu-id="0f28d-178">Key Vault pomáhá zjednodušit proces správy klíčů a zajišťuje vám kontrolu nad klíči, které k přístupu a šifrování vaše data.</span><span class="sxs-lookup"><span data-stu-id="0f28d-178">Key Vault helps streamline the key management process and enables you to maintain control of keys that access and encrypt your data.</span></span> <span data-ttu-id="0f28d-179">Tento scénář jsou uvedené některé pojmy Key Vault, jak vytvořit a používat certifikát, ale není vyčerpávající přehled o tom, jak používat Key Vault.</span><span class="sxs-lookup"><span data-stu-id="0f28d-179">This scenario introduces some Key Vault concepts to create and use a certificate, though is not an exhaustive overview on how to use Key Vault.</span></span>

<span data-ttu-id="0f28d-180">Následující kroky ukazují, jak můžete:</span><span class="sxs-lookup"><span data-stu-id="0f28d-180">The following steps show how you can:</span></span>

- <span data-ttu-id="0f28d-181">Vytvoření Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0f28d-181">Create an Azure Key Vault</span></span>
- <span data-ttu-id="0f28d-182">Generovat nebo nahrajte certifikát do služby Key Vault</span><span class="sxs-lookup"><span data-stu-id="0f28d-182">Generate or upload a certificate to the Key Vault</span></span>
- <span data-ttu-id="0f28d-183">Vytvoření tajného klíče z certifikátu se vložit v pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="0f28d-183">Create a secret from the certificate to inject in to a VM</span></span>
- <span data-ttu-id="0f28d-184">Vytvoření virtuálního počítače a vložit certifikátu</span><span class="sxs-lookup"><span data-stu-id="0f28d-184">Create a VM and inject the certificate</span></span>

### <a name="create-an-azure-key-vault"></a><span data-ttu-id="0f28d-185">Vytvoření Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0f28d-185">Create an Azure Key Vault</span></span>
<span data-ttu-id="0f28d-186">Nejprve vytvořte Key Vault s [vytvořit az keyvault](/cli/azure/keyvault#create) a povolit pro použití při nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0f28d-186">First, create a Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable it for use when you deploy a VM.</span></span> <span data-ttu-id="0f28d-187">Každý Key Vault vyžaduje jedinečný název a musí být všechny malá písmena.</span><span class="sxs-lookup"><span data-stu-id="0f28d-187">Each Key Vault requires a unique name, and should be all lower case.</span></span> <span data-ttu-id="0f28d-188">Nahraďte *mykeyvault* v následujícím příkladu se svůj vlastní jedinečný název pro Key Vault:</span><span class="sxs-lookup"><span data-stu-id="0f28d-188">Replace *mykeyvault* in the following example with your own unique Key Vault name:</span></span>

```azurecli-interactive 
keyvault_name=mykeyvault
az keyvault create \
    --resource-group myResourceGroupAutomate \
    --name $keyvault_name \
    --enabled-for-deployment
```

### <a name="generate-certificate-and-store-in-key-vault"></a><span data-ttu-id="0f28d-189">Vygenerování certifikátu a uložit v Key Vault</span><span class="sxs-lookup"><span data-stu-id="0f28d-189">Generate certificate and store in Key Vault</span></span>
<span data-ttu-id="0f28d-190">Pro použití v provozním prostředí, měli byste importovat platný certifikát podepsaný službou důvěryhodného zprostředkovatele s [import certifikátu keyvault az](/cli/azure/keyvault/certificate#import).</span><span class="sxs-lookup"><span data-stu-id="0f28d-190">For production use, you should import a valid certificate signed by trusted provider with [az keyvault certificate import](/cli/azure/keyvault/certificate#import).</span></span> <span data-ttu-id="0f28d-191">V tomto kurzu následující příklad ukazuje, jak můžete vygenerovat certifikát podepsaný svým držitelem s [vytvoření certifikátu keyvault az](/cli/azure/keyvault/certificate#create) používající výchozí zásady certifikátu:</span><span class="sxs-lookup"><span data-stu-id="0f28d-191">For this tutorial, the following example shows how you can generate a self-signed certificate with [az keyvault certificate create](/cli/azure/keyvault/certificate#create) that uses the default certificate policy:</span></span>

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```


### <a name="prepare-certificate-for-use-with-vm"></a><span data-ttu-id="0f28d-192">Příprava certifikátu pro použití s virtuálním Počítačem.</span><span class="sxs-lookup"><span data-stu-id="0f28d-192">Prepare certificate for use with VM</span></span>
<span data-ttu-id="0f28d-193">K použití certifikátu během virtuálního počítače vytvořit proces, získat číslo ID vašeho certifikátu s [az keyvault verze sdíleného tajného klíče seznam-](/cli/azure/keyvault/secret#list-versions).</span><span class="sxs-lookup"><span data-stu-id="0f28d-193">To use the certificate during the VM create process, obtain the ID of your certificate with [az keyvault secret list-versions](/cli/azure/keyvault/secret#list-versions).</span></span> <span data-ttu-id="0f28d-194">Virtuální počítač, musí certifikát v určitém formátu vložit na spuštění, takže převést certifikát s [az virtuálních počítačů formát secret](/cli/azure/vm#format-secret).</span><span class="sxs-lookup"><span data-stu-id="0f28d-194">The VM needs the certificate in a certain format to inject it on boot, so convert the certificate with [az vm format-secret](/cli/azure/vm#format-secret).</span></span> <span data-ttu-id="0f28d-195">Následující příklad přiřadí proměnné pro snadné použití v dalších krocích výstup z těchto příkazů:</span><span class="sxs-lookup"><span data-stu-id="0f28d-195">The following example assigns the output of these commands to variables for ease of use in the next steps:</span></span>

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```


### <a name="create-cloud-init-config-to-secure-nginx"></a><span data-ttu-id="0f28d-196">Vytvoření konfigurace cloudu init zabezpečit NGINX</span><span class="sxs-lookup"><span data-stu-id="0f28d-196">Create cloud-init config to secure NGINX</span></span>
<span data-ttu-id="0f28d-197">Když vytvoříte virtuální počítač, certifikáty a klíče jsou uložené v chráněného */var/lib/příkazwaagent/* adresáře.</span><span class="sxs-lookup"><span data-stu-id="0f28d-197">When you create a VM, certificates and keys are stored in the protected */var/lib/waagent/* directory.</span></span> <span data-ttu-id="0f28d-198">K automatizaci certifikát se přidává do virtuálního počítače a konfigurace NGINX, můžete použít aktualizované cloudu init konfigurace z předchozího příkladu.</span><span class="sxs-lookup"><span data-stu-id="0f28d-198">To automate adding the certificate to the VM and configuring NGINX, you can use an updated cloud-init config from the previous example.</span></span>

<span data-ttu-id="0f28d-199">Vytvořte soubor s názvem *cloudu. init secured.txt* a vložte následující konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0f28d-199">Create a file named *cloud-init-secured.txt* and paste the following configuration.</span></span> <span data-ttu-id="0f28d-200">Znovu Pokud používáte cloudové prostředí, vytvořte konfigurační soubor cloudu init tam a není na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="0f28d-200">Again, if you use the Cloud Shell, create the cloud-init config file there and not on your local machine.</span></span> <span data-ttu-id="0f28d-201">Použití `sensible-editor cloud-init-secured.txt` k vytvoření tohoto souboru a zobrazit seznam dostupných editory.</span><span class="sxs-lookup"><span data-stu-id="0f28d-201">Use `sensible-editor cloud-init-secured.txt` to create the file and see a list of available editors.</span></span> <span data-ttu-id="0f28d-202">Ujistěte se, že je soubor celou cloudu init zkopírován správně, obzvláště první řádek:</span><span class="sxs-lookup"><span data-stu-id="0f28d-202">Make sure that the whole cloud-init file is copied correctly, especially the first line:</span></span>

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-secure-vm"></a><span data-ttu-id="0f28d-203">Vytvoření zabezpečeného virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0f28d-203">Create secure VM</span></span>
<span data-ttu-id="0f28d-204">Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="0f28d-204">Now create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="0f28d-205">Data certifikátu je vložili z trezoru klíčů `--secrets` parametr.</span><span class="sxs-lookup"><span data-stu-id="0f28d-205">The certificate data is injected from Key Vault with the `--secrets` parameter.</span></span> <span data-ttu-id="0f28d-206">Jako v předchozím příkladu můžete předat také v konfiguraci cloudu init s `--custom-data` parametr:</span><span class="sxs-lookup"><span data-stu-id="0f28d-206">As in the previous example, you also pass in the cloud-init config with the `--custom-data` parameter:</span></span>

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-secured.txt \
    --secrets "$vm_secret"
```

<span data-ttu-id="0f28d-207">Trvá několik minut pro virtuální počítač, který se má vytvořit, balíčky určené k instalaci a aplikaci spusťte.</span><span class="sxs-lookup"><span data-stu-id="0f28d-207">It takes a few minutes for the VM to be created, the packages to install, and the app to start.</span></span> <span data-ttu-id="0f28d-208">Existují úlohy na pozadí, které dál běžet po rozhraní příkazového řádku Azure se vrátíte do řádku.</span><span class="sxs-lookup"><span data-stu-id="0f28d-208">There are background tasks that continue to run after the Azure CLI returns you to the prompt.</span></span> <span data-ttu-id="0f28d-209">To může být jiná několik minut, než může aplikaci používat.</span><span class="sxs-lookup"><span data-stu-id="0f28d-209">It may be another couple of minutes before you can access the app.</span></span> <span data-ttu-id="0f28d-210">Po vytvoření virtuálního počítače, poznamenejte si `publicIpAddress` zobrazí pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="0f28d-210">When the VM has been created, take note of the `publicIpAddress` displayed by the Azure CLI.</span></span> <span data-ttu-id="0f28d-211">Tato adresa se používá pro přístup k aplikaci Node.js pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="0f28d-211">This address is used to access the Node.js app via a web browser.</span></span>

<span data-ttu-id="0f28d-212">Povolit zabezpečený webový provoz připojit virtuální počítač, otevřete port 443 z Internetu s [az virtuálních počítačů open-port](/cli/azure/vm#open-port):</span><span class="sxs-lookup"><span data-stu-id="0f28d-212">To allow secure web traffic to reach your VM, open port 443 from the Internet with [az vm open-port](/cli/azure/vm#open-port):</span></span>

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --port 443
```

### <a name="test-secure-web-app"></a><span data-ttu-id="0f28d-213">Testování zabezpečení webové aplikace</span><span class="sxs-lookup"><span data-stu-id="0f28d-213">Test secure web app</span></span>
<span data-ttu-id="0f28d-214">Nyní můžete otevřít webový prohlížeč a zadejte *https://<publicIpAddress>*  na panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="0f28d-214">Now you can open a web browser and enter *https://<publicIpAddress>* in the address bar.</span></span> <span data-ttu-id="0f28d-215">Zadejte vlastní veřejná IP adresa z virtuálního počítače vytvořit proces.</span><span class="sxs-lookup"><span data-stu-id="0f28d-215">Provide your own public IP address from the VM create process.</span></span> <span data-ttu-id="0f28d-216">Pokud použijete certifikát podepsaný svým držitelem, přijměte upozornění zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="0f28d-216">Accept the security warning if you used a self-signed certificate:</span></span>

![Přijetí upozornění zabezpečení webového prohlížeče](./media/tutorial-automate-vm-deployment/browser-warning.png)

<span data-ttu-id="0f28d-218">Zabezpečené NGINX lokality a Node.js aplikace se potom zobrazí jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="0f28d-218">Your secured NGINX site and Node.js app is then displayed as in the following example:</span></span>

![Zobrazit spuštěná zabezpečené NGINX Web](./media/tutorial-automate-vm-deployment/secured-nginx.png)


## <a name="next-steps"></a><span data-ttu-id="0f28d-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0f28d-220">Next steps</span></span>
<span data-ttu-id="0f28d-221">V tomto kurzu jste nakonfigurovali virtuální počítače při prvním spuštění počítače s inicializací cloudu.</span><span class="sxs-lookup"><span data-stu-id="0f28d-221">In this tutorial, you configured VMs on first boot with cloud-init.</span></span> <span data-ttu-id="0f28d-222">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="0f28d-222">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0f28d-223">Vytvořte soubor konfigurace cloudu init</span><span class="sxs-lookup"><span data-stu-id="0f28d-223">Create a cloud-init config file</span></span>
> * <span data-ttu-id="0f28d-224">Vytvořit virtuální počítač, který používá soubor init cloudu</span><span class="sxs-lookup"><span data-stu-id="0f28d-224">Create a VM that uses a cloud-init file</span></span>
> * <span data-ttu-id="0f28d-225">Zobrazit spuštěné aplikaci Node.js, po vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="0f28d-225">View a running Node.js app after the VM is created</span></span>
> * <span data-ttu-id="0f28d-226">Pomocí Key Vault bezpečně uložit certifikáty</span><span class="sxs-lookup"><span data-stu-id="0f28d-226">Use Key Vault to securely store certificates</span></span>
> * <span data-ttu-id="0f28d-227">Automatizovat zabezpečená nasazení NGINX s inicializací cloudu</span><span class="sxs-lookup"><span data-stu-id="0f28d-227">Automate secure deployments of NGINX with cloud-init</span></span>

<span data-ttu-id="0f28d-228">Přechodu na v dalším kurzu se dozvíte, jak vytvořit vlastní Image virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0f28d-228">Advance to the next tutorial to learn how to create custom VM images.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0f28d-229">Vytváření vlastních imagí virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0f28d-229">Create custom VM images</span></span>](./tutorial-custom-images.md)
