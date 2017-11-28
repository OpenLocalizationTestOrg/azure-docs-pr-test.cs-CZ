---
title: "aaaCustomize virtuálního počítače s Linuxem na při prvním spuštění v Azure | Microsoft Docs"
description: "Zjistěte, jak cloudové inicializací toouse a Key Vault toocustomze virtuální počítače s Linuxem hello prvním spuštění v Azure"
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
ms.openlocfilehash: 280189723ac0205226f9c0068bd605da13249ace
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocustomize-a-linux-virtual-machine-on-first-boot"></a><span data-ttu-id="74765-103">Jak toocustomize virtuální počítač s Linuxem na při prvním spuštění</span><span class="sxs-lookup"><span data-stu-id="74765-103">How toocustomize a Linux virtual machine on first boot</span></span>
<span data-ttu-id="74765-104">V předchozích kurzu jste se naučili jak tooSSH tooa virtuální počítač (VM) a ručně nainstalujte NGINX.</span><span class="sxs-lookup"><span data-stu-id="74765-104">In a previous tutorial, you learned how tooSSH tooa virtual machine (VM) and manually install NGINX.</span></span> <span data-ttu-id="74765-105">Obvykle se požaduje toocreate virtuální počítače rychlý a konzistentním způsobem, nějaký způsob automatizace.</span><span class="sxs-lookup"><span data-stu-id="74765-105">toocreate VMs in a quick and consistent manner, some form of automation is typically desired.</span></span> <span data-ttu-id="74765-106">Běžné toocustomize přístup virtuální počítač na při prvním spuštění je toouse [cloudu init](https://cloudinit.readthedocs.io).</span><span class="sxs-lookup"><span data-stu-id="74765-106">A common approach toocustomize a VM on first boot is toouse [cloud-init](https://cloudinit.readthedocs.io).</span></span> <span data-ttu-id="74765-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="74765-107">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="74765-108">Vytvořte soubor konfigurace cloudu init</span><span class="sxs-lookup"><span data-stu-id="74765-108">Create a cloud-init config file</span></span>
> * <span data-ttu-id="74765-109">Vytvořit virtuální počítač, který používá soubor init cloudu</span><span class="sxs-lookup"><span data-stu-id="74765-109">Create a VM that uses a cloud-init file</span></span>
> * <span data-ttu-id="74765-110">Zobrazit spuštěné aplikaci Node.js po hello je vytvořena virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="74765-110">View a running Node.js app after hello VM is created</span></span>
> * <span data-ttu-id="74765-111">Pomocí Key Vault toosecurely úložiště certifikátů</span><span class="sxs-lookup"><span data-stu-id="74765-111">Use Key Vault toosecurely store certificates</span></span>
> * <span data-ttu-id="74765-112">Automatizovat zabezpečená nasazení NGINX s inicializací cloudu</span><span class="sxs-lookup"><span data-stu-id="74765-112">Automate secure deployments of NGINX with cloud-init</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="74765-113">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="74765-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="74765-114">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="74765-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="74765-115">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="74765-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>  



## <a name="cloud-init-overview"></a><span data-ttu-id="74765-116">Init cloudu – přehled</span><span class="sxs-lookup"><span data-stu-id="74765-116">Cloud-init overview</span></span>
<span data-ttu-id="74765-117">[Init cloudu](https://cloudinit.readthedocs.io) je často používaný přístup toocustomize virtuálního počítače s Linuxem jako spustí pro hello poprvé.</span><span class="sxs-lookup"><span data-stu-id="74765-117">[Cloud-init](https://cloudinit.readthedocs.io) is a widely used approach toocustomize a Linux VM as it boots for hello first time.</span></span> <span data-ttu-id="74765-118">Můžete používat cloudové init tooinstall balíčky a zapisovat soubory, nebo tooconfigure uživatele a zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="74765-118">You can use cloud-init tooinstall packages and write files, or tooconfigure users and security.</span></span> <span data-ttu-id="74765-119">Init cloudu běží během počáteční spouštění hello, neexistují žádné další kroky nebo požadováno agenty tooapply konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="74765-119">As cloud-init runs during hello initial boot process, there are no additional steps or required agents tooapply your configuration.</span></span>

<span data-ttu-id="74765-120">Init cloudu také funguje v různých distribucí.</span><span class="sxs-lookup"><span data-stu-id="74765-120">Cloud-init also works across distributions.</span></span> <span data-ttu-id="74765-121">Například nepoužívejte **výstižný get instalace** nebo **yum nainstalovat** tooinstall balíčku.</span><span class="sxs-lookup"><span data-stu-id="74765-121">For example, you don't use **apt-get install** or **yum install** tooinstall a package.</span></span> <span data-ttu-id="74765-122">Místo toho můžete definovat seznam tooinstall balíčky.</span><span class="sxs-lookup"><span data-stu-id="74765-122">Instead you can define a list of packages tooinstall.</span></span> <span data-ttu-id="74765-123">Init cloudu automaticky používá nástroj pro správu hello nativní balíčku pro hello distro, kterou vyberete.</span><span class="sxs-lookup"><span data-stu-id="74765-123">Cloud-init automatically uses hello native package management tool for hello distro you select.</span></span>

<span data-ttu-id="74765-124">Snažíme se práce s našich partnerů tooget cloudu inicializací zahrnuté a práci v hello bitové kopie, aby umožňovala tooAzure.</span><span class="sxs-lookup"><span data-stu-id="74765-124">We are working with our partners tooget cloud-init included and working in hello images that they provide tooAzure.</span></span> <span data-ttu-id="74765-125">Hello následující tabulka popisuje hello aktuální dostupnosti cloudu init Image platformy Azure:</span><span class="sxs-lookup"><span data-stu-id="74765-125">hello following table outlines hello current cloud-init availability on Azure platform images:</span></span>

| <span data-ttu-id="74765-126">Alias</span><span class="sxs-lookup"><span data-stu-id="74765-126">Alias</span></span> | <span data-ttu-id="74765-127">Vydavatel</span><span class="sxs-lookup"><span data-stu-id="74765-127">Publisher</span></span> | <span data-ttu-id="74765-128">Nabídka</span><span class="sxs-lookup"><span data-stu-id="74765-128">Offer</span></span> | <span data-ttu-id="74765-129">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="74765-129">SKU</span></span> | <span data-ttu-id="74765-130">Verze</span><span class="sxs-lookup"><span data-stu-id="74765-130">Version</span></span> |
|:--- |:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="74765-131">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="74765-131">UbuntuLTS</span></span> |<span data-ttu-id="74765-132">Canonical</span><span class="sxs-lookup"><span data-stu-id="74765-132">Canonical</span></span> |<span data-ttu-id="74765-133">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="74765-133">UbuntuServer</span></span> |<span data-ttu-id="74765-134">16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="74765-134">16.04-LTS</span></span> |<span data-ttu-id="74765-135">nejnovější</span><span class="sxs-lookup"><span data-stu-id="74765-135">latest</span></span> |
| <span data-ttu-id="74765-136">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="74765-136">UbuntuLTS</span></span> |<span data-ttu-id="74765-137">Canonical</span><span class="sxs-lookup"><span data-stu-id="74765-137">Canonical</span></span> |<span data-ttu-id="74765-138">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="74765-138">UbuntuServer</span></span> |<span data-ttu-id="74765-139">14.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="74765-139">14.04.5-LTS</span></span> |<span data-ttu-id="74765-140">nejnovější</span><span class="sxs-lookup"><span data-stu-id="74765-140">latest</span></span> |
| <span data-ttu-id="74765-141">CoreOS</span><span class="sxs-lookup"><span data-stu-id="74765-141">CoreOS</span></span> |<span data-ttu-id="74765-142">CoreOS</span><span class="sxs-lookup"><span data-stu-id="74765-142">CoreOS</span></span> |<span data-ttu-id="74765-143">CoreOS</span><span class="sxs-lookup"><span data-stu-id="74765-143">CoreOS</span></span> |<span data-ttu-id="74765-144">Stable</span><span class="sxs-lookup"><span data-stu-id="74765-144">Stable</span></span> |<span data-ttu-id="74765-145">nejnovější</span><span class="sxs-lookup"><span data-stu-id="74765-145">latest</span></span> |


## <a name="create-cloud-init-config-file"></a><span data-ttu-id="74765-146">Vytvoření cloudové init konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="74765-146">Create cloud-init config file</span></span>
<span data-ttu-id="74765-147">toosee cloudu inicializací v akci, vytvořte virtuální počítač, který nainstaluje NGINX a spustí jednoduchou aplikaci Node.js "Zdravím svět".</span><span class="sxs-lookup"><span data-stu-id="74765-147">toosee cloud-init in action, create a VM that installs NGINX and runs a simple 'Hello World' Node.js app.</span></span> <span data-ttu-id="74765-148">Hello následující konfigurace cloudu init nainstaluje hello požadované balíčky, vytvoří aplikaci Node.js, pak inicializaci a spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="74765-148">hello following cloud-init configuration installs hello required packages, creates a Node.js app, then initialize and starts hello app.</span></span>

<span data-ttu-id="74765-149">V aktuálním prostředí, vytvořte soubor s názvem *cloudu init.txt* a hello vložte následující konfigurace.</span><span class="sxs-lookup"><span data-stu-id="74765-149">In your current shell, create a file named *cloud-init.txt* and paste hello following configuration.</span></span> <span data-ttu-id="74765-150">Můžete například vytvořte soubor hello v hello cloudové prostředí není na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="74765-150">For example, create hello file in hello Cloud Shell not on your local machine.</span></span> <span data-ttu-id="74765-151">Můžete použít libovolný editor, které chcete.</span><span class="sxs-lookup"><span data-stu-id="74765-151">You can use any editor you wish.</span></span> <span data-ttu-id="74765-152">Zadejte `sensible-editor cloud-init.txt` toocreate hello souboru a zobrazit seznam dostupných editory.</span><span class="sxs-lookup"><span data-stu-id="74765-152">Enter `sensible-editor cloud-init.txt` toocreate hello file and see a list of available editors.</span></span> <span data-ttu-id="74765-153">Ujistěte se, že tento soubor celou cloudu init hello správně zkopírován, zejména hello první řádek:</span><span class="sxs-lookup"><span data-stu-id="74765-153">Make sure that hello whole cloud-init file is copied correctly, especially hello first line:</span></span>

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

<span data-ttu-id="74765-154">Další informace o možnostech konfigurace cloudu init najdete v tématu [příklady konfigurace cloudu init](https://cloudinit.readthedocs.io/en/latest/topics/examples.html).</span><span class="sxs-lookup"><span data-stu-id="74765-154">For more information about cloud-init configuration options, see [cloud-init config examples](https://cloudinit.readthedocs.io/en/latest/topics/examples.html).</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="74765-155">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="74765-155">Create virtual machine</span></span>
<span data-ttu-id="74765-156">Před vytvořením virtuálního počítače, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="74765-156">Before you can create a VM, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="74765-157">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupAutomate* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="74765-157">hello following example creates a resource group named *myResourceGroupAutomate* in hello *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupAutomate --location eastus
```

<span data-ttu-id="74765-158">Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="74765-158">Now create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="74765-159">Použití hello `--custom-data` parametr toopass v souboru config init cloudu.</span><span class="sxs-lookup"><span data-stu-id="74765-159">Use hello `--custom-data` parameter toopass in your cloud-init config file.</span></span> <span data-ttu-id="74765-160">Zadejte úplnou cestu toohello hello *cloudu init.txt* konfigurace, pokud jste uložili soubor hello mimo pracovní adresář existuje.</span><span class="sxs-lookup"><span data-stu-id="74765-160">Provide hello full path toohello *cloud-init.txt* config if you saved hello file outside of your present working directory.</span></span> <span data-ttu-id="74765-161">Hello následující příklad vytvoří virtuální počítač s názvem *myAutomatedVM*:</span><span class="sxs-lookup"><span data-stu-id="74765-161">hello following example creates a VM named *myAutomatedVM*:</span></span>

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

<span data-ttu-id="74765-162">Jak dlouho trvá několik minut, než toobe hello virtuálního počítače vytvořena, hello balíčky tooinstall a toostart aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="74765-162">It takes a few minutes for hello VM toobe created, hello packages tooinstall, and hello app toostart.</span></span> <span data-ttu-id="74765-163">Existují úlohy na pozadí, které pokračovat toorun po hello příkazového řádku Azure CLI vrátí, toohello řádku.</span><span class="sxs-lookup"><span data-stu-id="74765-163">There are background tasks that continue toorun after hello Azure CLI returns you toohello prompt.</span></span> <span data-ttu-id="74765-164">To může být jiná několik minut před přístupem k aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="74765-164">It may be another couple of minutes before you can access hello app.</span></span> <span data-ttu-id="74765-165">Po vytvoření hello virtuálních počítačů, poznamenejte si hello `publicIpAddress` zobrazí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="74765-165">When hello VM has been created, take note of hello `publicIpAddress` displayed by hello Azure CLI.</span></span> <span data-ttu-id="74765-166">Tato adresa se aplikace Node.js hello použité tooaccess prostřednictvím webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="74765-166">This address is used tooaccess hello Node.js app via a web browser.</span></span>

<span data-ttu-id="74765-167">tooallow webový provoz tooreach virtuální počítač, otevřete port 80 z hello Internet s [az virtuálních počítačů open-port](/cli/azure/vm#open-port):</span><span class="sxs-lookup"><span data-stu-id="74765-167">tooallow web traffic tooreach your VM, open port 80 from hello Internet with [az vm open-port](/cli/azure/vm#open-port):</span></span>

```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroupAutomate --name myVM
```

## <a name="test-web-app"></a><span data-ttu-id="74765-168">Test webové aplikace</span><span class="sxs-lookup"><span data-stu-id="74765-168">Test web app</span></span>
<span data-ttu-id="74765-169">Nyní můžete otevřít webový prohlížeč a zadejte *http://<publicIpAddress>*  v panelu Adresa hello.</span><span class="sxs-lookup"><span data-stu-id="74765-169">Now you can open a web browser and enter *http://<publicIpAddress>* in hello address bar.</span></span> <span data-ttu-id="74765-170">Zadejte vlastní veřejná IP adresa z hello virtuálního počítače vytvořit proces.</span><span class="sxs-lookup"><span data-stu-id="74765-170">Provide your own public IP address from hello VM create process.</span></span> <span data-ttu-id="74765-171">Aplikace Node.js se zobrazí jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="74765-171">Your Node.js app is displayed as in hello following example:</span></span>

![Zobrazení spuštěných NGINX lokality](./media/tutorial-automate-vm-deployment/nginx.png)


## <a name="inject-certificates-from-key-vault"></a><span data-ttu-id="74765-173">Vložit certifikáty z Key Vault</span><span class="sxs-lookup"><span data-stu-id="74765-173">Inject certificates from Key Vault</span></span>
<span data-ttu-id="74765-174">Této volitelné části ukazuje, jak můžete bezpečně uložit certifikáty v Azure Key Vault a vložit je během hello nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="74765-174">This optional section shows how you can securely store certificates in Azure Key Vault and inject them during hello VM deployment.</span></span> <span data-ttu-id="74765-175">Místo použití vlastní image, která obsahuje certifikáty hello zaručená v, tento proces zajišťuje, že hello nejaktuálnější certifikáty jsou vložit tooa virtuální počítač při prvním spuštění počítače.</span><span class="sxs-lookup"><span data-stu-id="74765-175">Rather than using a custom image that includes hello certificates baked-in, this process ensures that hello most up-to-date certificates are injected tooa VM on first boot.</span></span> <span data-ttu-id="74765-176">Během procesu hello hello certifikát nikdy opustí hello platformy Azure nebo je vystaven ve skriptu, historie příkazového řádku nebo šablony.</span><span class="sxs-lookup"><span data-stu-id="74765-176">During hello process, hello certificate never leaves hello Azure platform or is exposed in a script, command-line history, or template.</span></span>

<span data-ttu-id="74765-177">Azure Key Vault chrání kryptografické klíče a tajné klíče, jako je například certifikáty nebo hesla.</span><span class="sxs-lookup"><span data-stu-id="74765-177">Azure Key Vault safeguards cryptographic keys and secrets, such as certificates or passwords.</span></span> <span data-ttu-id="74765-178">Key Vault pomáhá zjednodušit proces správy klíčů hello a umožní vám toomaintain kontrolu nad klíči, které k přístupu a šifrování dat.</span><span class="sxs-lookup"><span data-stu-id="74765-178">Key Vault helps streamline hello key management process and enables you toomaintain control of keys that access and encrypt your data.</span></span> <span data-ttu-id="74765-179">Tento scénář představuje některé koncepty toocreate Key Vault a použít certifikát, ale není vyčerpávající přehled o tom, toouse Key Vault.</span><span class="sxs-lookup"><span data-stu-id="74765-179">This scenario introduces some Key Vault concepts toocreate and use a certificate, though is not an exhaustive overview on how toouse Key Vault.</span></span>

<span data-ttu-id="74765-180">Hello následující kroky ukazují, jak můžete:</span><span class="sxs-lookup"><span data-stu-id="74765-180">hello following steps show how you can:</span></span>

- <span data-ttu-id="74765-181">Vytvoření Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="74765-181">Create an Azure Key Vault</span></span>
- <span data-ttu-id="74765-182">Generovat nebo nahrát certifikát toohello Key Vault</span><span class="sxs-lookup"><span data-stu-id="74765-182">Generate or upload a certificate toohello Key Vault</span></span>
- <span data-ttu-id="74765-183">Vytvoření tajného klíče z certifikátu tooinject hello v tooa virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="74765-183">Create a secret from hello certificate tooinject in tooa VM</span></span>
- <span data-ttu-id="74765-184">Vytvoření virtuálního počítače a vložit hello certifikátu</span><span class="sxs-lookup"><span data-stu-id="74765-184">Create a VM and inject hello certificate</span></span>

### <a name="create-an-azure-key-vault"></a><span data-ttu-id="74765-185">Vytvoření Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="74765-185">Create an Azure Key Vault</span></span>
<span data-ttu-id="74765-186">Nejprve vytvořte Key Vault s [vytvořit az keyvault](/cli/azure/keyvault#create) a povolit pro použití při nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="74765-186">First, create a Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable it for use when you deploy a VM.</span></span> <span data-ttu-id="74765-187">Každý Key Vault vyžaduje jedinečný název a musí být všechny malá písmena.</span><span class="sxs-lookup"><span data-stu-id="74765-187">Each Key Vault requires a unique name, and should be all lower case.</span></span> <span data-ttu-id="74765-188">Nahraďte *mykeyvault* v hello následující příklad s svůj vlastní jedinečný název pro Key Vault:</span><span class="sxs-lookup"><span data-stu-id="74765-188">Replace *mykeyvault* in hello following example with your own unique Key Vault name:</span></span>

```azurecli-interactive 
keyvault_name=mykeyvault
az keyvault create \
    --resource-group myResourceGroupAutomate \
    --name $keyvault_name \
    --enabled-for-deployment
```

### <a name="generate-certificate-and-store-in-key-vault"></a><span data-ttu-id="74765-189">Vygenerování certifikátu a uložit v Key Vault</span><span class="sxs-lookup"><span data-stu-id="74765-189">Generate certificate and store in Key Vault</span></span>
<span data-ttu-id="74765-190">Pro použití v provozním prostředí, měli byste importovat platný certifikát podepsaný službou důvěryhodného zprostředkovatele s [import certifikátu keyvault az](/cli/azure/keyvault/certificate#import).</span><span class="sxs-lookup"><span data-stu-id="74765-190">For production use, you should import a valid certificate signed by trusted provider with [az keyvault certificate import](/cli/azure/keyvault/certificate#import).</span></span> <span data-ttu-id="74765-191">V tomto kurzu hello následující příklad ukazuje, jak můžete vygenerovat certifikát podepsaný svým držitelem s [vytvoření certifikátu keyvault az](/cli/azure/keyvault/certificate#create) používající zásady certifikátů výchozí hello:</span><span class="sxs-lookup"><span data-stu-id="74765-191">For this tutorial, hello following example shows how you can generate a self-signed certificate with [az keyvault certificate create](/cli/azure/keyvault/certificate#create) that uses hello default certificate policy:</span></span>

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```


### <a name="prepare-certificate-for-use-with-vm"></a><span data-ttu-id="74765-192">Příprava certifikátu pro použití s virtuálním Počítačem.</span><span class="sxs-lookup"><span data-stu-id="74765-192">Prepare certificate for use with VM</span></span>
<span data-ttu-id="74765-193">certifikát hello toouse během hello virtuálního počítače vytvořit proces, získejte hello ID vašeho certifikátu s [az keyvault verze sdíleného tajného klíče seznam-](/cli/azure/keyvault/secret#list-versions).</span><span class="sxs-lookup"><span data-stu-id="74765-193">toouse hello certificate during hello VM create process, obtain hello ID of your certificate with [az keyvault secret list-versions](/cli/azure/keyvault/secret#list-versions).</span></span> <span data-ttu-id="74765-194">potřebuje certifikát hello Hello virtuálního počítače v určité formátu tooinject ho na spuštění, takže převést hello certifikát s [az virtuálních počítačů formát secret](/cli/azure/vm#format-secret).</span><span class="sxs-lookup"><span data-stu-id="74765-194">hello VM needs hello certificate in a certain format tooinject it on boot, so convert hello certificate with [az vm format-secret](/cli/azure/vm#format-secret).</span></span> <span data-ttu-id="74765-195">Následující ukázka Hello přiřadí výstup hello toovariables tyto příkazy pro snadné použití v hello další kroky:</span><span class="sxs-lookup"><span data-stu-id="74765-195">hello following example assigns hello output of these commands toovariables for ease of use in hello next steps:</span></span>

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```


### <a name="create-cloud-init-config-toosecure-nginx"></a><span data-ttu-id="74765-196">Vytvoření cloudové init konfigurace toosecure NGINX</span><span class="sxs-lookup"><span data-stu-id="74765-196">Create cloud-init config toosecure NGINX</span></span>
<span data-ttu-id="74765-197">Když vytvoříte virtuální počítač, certifikáty a klíče jsou uložené v hello chráněné */var/lib/příkazwaagent/* adresáře.</span><span class="sxs-lookup"><span data-stu-id="74765-197">When you create a VM, certificates and keys are stored in hello protected */var/lib/waagent/* directory.</span></span> <span data-ttu-id="74765-198">tooautomate přidání hello certifikát toohello virtuálního počítače a konfigurace NGINX, můžete použít aktualizované cloudu init konfigurace z předchozí příklad hello.</span><span class="sxs-lookup"><span data-stu-id="74765-198">tooautomate adding hello certificate toohello VM and configuring NGINX, you can use an updated cloud-init config from hello previous example.</span></span>

<span data-ttu-id="74765-199">Vytvořte soubor s názvem *cloudu. init secured.txt* a hello vložte následující konfigurace.</span><span class="sxs-lookup"><span data-stu-id="74765-199">Create a file named *cloud-init-secured.txt* and paste hello following configuration.</span></span> <span data-ttu-id="74765-200">Znovu Pokud používáte hello cloudové prostředí, vytvořte hello cloudu init konfiguračního souboru tam a není na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="74765-200">Again, if you use hello Cloud Shell, create hello cloud-init config file there and not on your local machine.</span></span> <span data-ttu-id="74765-201">Použití `sensible-editor cloud-init-secured.txt` toocreate hello souboru a zobrazit seznam dostupných editory.</span><span class="sxs-lookup"><span data-stu-id="74765-201">Use `sensible-editor cloud-init-secured.txt` toocreate hello file and see a list of available editors.</span></span> <span data-ttu-id="74765-202">Ujistěte se, že tento soubor celou cloudu init hello správně zkopírován, zejména hello první řádek:</span><span class="sxs-lookup"><span data-stu-id="74765-202">Make sure that hello whole cloud-init file is copied correctly, especially hello first line:</span></span>

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

### <a name="create-secure-vm"></a><span data-ttu-id="74765-203">Vytvoření zabezpečeného virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="74765-203">Create secure VM</span></span>
<span data-ttu-id="74765-204">Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="74765-204">Now create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="74765-205">data certifikátu Hello je vložili z Key Vault s hello `--secrets` parametr.</span><span class="sxs-lookup"><span data-stu-id="74765-205">hello certificate data is injected from Key Vault with hello `--secrets` parameter.</span></span> <span data-ttu-id="74765-206">Jako v předchozím příkladu hello také předáte v konfiguraci cloudu init hello s hello `--custom-data` parametr:</span><span class="sxs-lookup"><span data-stu-id="74765-206">As in hello previous example, you also pass in hello cloud-init config with hello `--custom-data` parameter:</span></span>

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

<span data-ttu-id="74765-207">Jak dlouho trvá několik minut, než toobe hello virtuálního počítače vytvořena, hello balíčky tooinstall a toostart aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="74765-207">It takes a few minutes for hello VM toobe created, hello packages tooinstall, and hello app toostart.</span></span> <span data-ttu-id="74765-208">Existují úlohy na pozadí, které pokračovat toorun po hello příkazového řádku Azure CLI vrátí, toohello řádku.</span><span class="sxs-lookup"><span data-stu-id="74765-208">There are background tasks that continue toorun after hello Azure CLI returns you toohello prompt.</span></span> <span data-ttu-id="74765-209">To může být jiná několik minut před přístupem k aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="74765-209">It may be another couple of minutes before you can access hello app.</span></span> <span data-ttu-id="74765-210">Po vytvoření hello virtuálních počítačů, poznamenejte si hello `publicIpAddress` zobrazí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="74765-210">When hello VM has been created, take note of hello `publicIpAddress` displayed by hello Azure CLI.</span></span> <span data-ttu-id="74765-211">Tato adresa se aplikace Node.js hello použité tooaccess prostřednictvím webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="74765-211">This address is used tooaccess hello Node.js app via a web browser.</span></span>

<span data-ttu-id="74765-212">tooallow zabezpečit webové přenosy tooreach virtuální počítač, otevřete port 443 ze hello Internet s [az virtuálních počítačů open-port](/cli/azure/vm#open-port):</span><span class="sxs-lookup"><span data-stu-id="74765-212">tooallow secure web traffic tooreach your VM, open port 443 from hello Internet with [az vm open-port](/cli/azure/vm#open-port):</span></span>

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --port 443
```

### <a name="test-secure-web-app"></a><span data-ttu-id="74765-213">Testování zabezpečení webové aplikace</span><span class="sxs-lookup"><span data-stu-id="74765-213">Test secure web app</span></span>
<span data-ttu-id="74765-214">Nyní můžete otevřít webový prohlížeč a zadejte *https://<publicIpAddress>*  v panelu Adresa hello.</span><span class="sxs-lookup"><span data-stu-id="74765-214">Now you can open a web browser and enter *https://<publicIpAddress>* in hello address bar.</span></span> <span data-ttu-id="74765-215">Zadejte vlastní veřejná IP adresa z hello virtuálního počítače vytvořit proces.</span><span class="sxs-lookup"><span data-stu-id="74765-215">Provide your own public IP address from hello VM create process.</span></span> <span data-ttu-id="74765-216">Přijměte upozornění zabezpečení hello, pokud se používá certifikát podepsaný svým držitelem:</span><span class="sxs-lookup"><span data-stu-id="74765-216">Accept hello security warning if you used a self-signed certificate:</span></span>

![Přijetí upozornění zabezpečení webového prohlížeče](./media/tutorial-automate-vm-deployment/browser-warning.png)

<span data-ttu-id="74765-218">Zabezpečené NGINX lokality a Node.js aplikace se potom zobrazí jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="74765-218">Your secured NGINX site and Node.js app is then displayed as in hello following example:</span></span>

![Zobrazit spuštěná zabezpečené NGINX Web](./media/tutorial-automate-vm-deployment/secured-nginx.png)


## <a name="next-steps"></a><span data-ttu-id="74765-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74765-220">Next steps</span></span>
<span data-ttu-id="74765-221">V tomto kurzu jste nakonfigurovali virtuální počítače při prvním spuštění počítače s inicializací cloudu.</span><span class="sxs-lookup"><span data-stu-id="74765-221">In this tutorial, you configured VMs on first boot with cloud-init.</span></span> <span data-ttu-id="74765-222">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="74765-222">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="74765-223">Vytvořte soubor konfigurace cloudu init</span><span class="sxs-lookup"><span data-stu-id="74765-223">Create a cloud-init config file</span></span>
> * <span data-ttu-id="74765-224">Vytvořit virtuální počítač, který používá soubor init cloudu</span><span class="sxs-lookup"><span data-stu-id="74765-224">Create a VM that uses a cloud-init file</span></span>
> * <span data-ttu-id="74765-225">Zobrazit spuštěné aplikaci Node.js po hello je vytvořena virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="74765-225">View a running Node.js app after hello VM is created</span></span>
> * <span data-ttu-id="74765-226">Pomocí Key Vault toosecurely úložiště certifikátů</span><span class="sxs-lookup"><span data-stu-id="74765-226">Use Key Vault toosecurely store certificates</span></span>
> * <span data-ttu-id="74765-227">Automatizovat zabezpečená nasazení NGINX s inicializací cloudu</span><span class="sxs-lookup"><span data-stu-id="74765-227">Automate secure deployments of NGINX with cloud-init</span></span>

<span data-ttu-id="74765-228">Jak zálohy další kurz toolearn toohello toocreate vlastní Image virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="74765-228">Advance toohello next tutorial toolearn how toocreate custom VM images.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="74765-229">Vytváření vlastních imagí virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="74765-229">Create custom VM images</span></span>](./tutorial-custom-images.md)
