---
title: "aaaSecure certifikáty webového serveru pomocí protokolu SSL v Azure | Microsoft Docs"
description: "Zjistěte, jak certifikáty toosecure hello NGINX webového serveru pomocí protokolu SSL na virtuální počítač s Linuxem v Azure"
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
ms.date: 07/17/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d3a62d77ac05c9aa2a44356b7c8e44cb485b81aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-web-server-with-ssl-certificates-on-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="c9796-103">Zabezpečení webového serveru pomocí certifikátů SSL na virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="c9796-103">Secure a web server with SSL certificates on a Linux virtual machine in Azure</span></span>
<span data-ttu-id="c9796-104">toosecure webových serverů, může být certifikát později SSL (Secure Sockets) používá tooencrypt webový provoz.</span><span class="sxs-lookup"><span data-stu-id="c9796-104">toosecure web servers, a Secure Sockets Later (SSL) certificate can be used tooencrypt web traffic.</span></span> <span data-ttu-id="c9796-105">Tyto certifikáty SSL můžou být uložená v Azure Key Vault a povolit zabezpečená nasazení certifikátů tooLinux virtuálních počítačů (VM) v Azure.</span><span class="sxs-lookup"><span data-stu-id="c9796-105">These SSL certificates can be stored in Azure Key Vault, and allow secure deployments of certificates tooLinux virtual machines (VMs) in Azure.</span></span> <span data-ttu-id="c9796-106">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="c9796-106">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c9796-107">Vytvoření Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c9796-107">Create an Azure Key Vault</span></span>
> * <span data-ttu-id="c9796-108">Generovat nebo nahrát certifikát toohello Key Vault</span><span class="sxs-lookup"><span data-stu-id="c9796-108">Generate or upload a certificate toohello Key Vault</span></span>
> * <span data-ttu-id="c9796-109">Vytvoření virtuálního počítače a instalaci hello NGINX webového serveru</span><span class="sxs-lookup"><span data-stu-id="c9796-109">Create a VM and install hello NGINX web server</span></span>
> * <span data-ttu-id="c9796-110">Vložit hello certifikát do hello virtuálního počítače a konfigurace NGINX s vazbou SSL</span><span class="sxs-lookup"><span data-stu-id="c9796-110">Inject hello certificate into hello VM and configure NGINX with an SSL binding</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c9796-111">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c9796-111">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="c9796-112">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="c9796-112">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="c9796-113">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c9796-113">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>  


## <a name="overview"></a><span data-ttu-id="c9796-114">Přehled</span><span class="sxs-lookup"><span data-stu-id="c9796-114">Overview</span></span>
<span data-ttu-id="c9796-115">Azure Key Vault chrání kryptografické klíče a tajné klíče, tyto certifikáty a hesla.</span><span class="sxs-lookup"><span data-stu-id="c9796-115">Azure Key Vault safeguards cryptographic keys and secrets, such certificates or passwords.</span></span> <span data-ttu-id="c9796-116">Key Vault pomáhá zjednodušit proces správy certifikátů hello a umožní vám toomaintain kontrolu nad klíči, které přístup těchto certifikátů.</span><span class="sxs-lookup"><span data-stu-id="c9796-116">Key Vault helps streamline hello certificate management process and enables you toomaintain control of keys that access those certificates.</span></span> <span data-ttu-id="c9796-117">Můžete vytvořit certifikát podepsaný svým držitelem v Key Vault nebo nahrát certifikát existující, důvěryhodné, který už vlastníte.</span><span class="sxs-lookup"><span data-stu-id="c9796-117">You can create a self-signed certificate inside Key Vault, or upload an existing, trusted certificate that you already own.</span></span>

<span data-ttu-id="c9796-118">Místo použití vlastní image virtuálního počítače, který zahrnuje certifikáty zaručená v, vložit certifikáty do spuštěného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c9796-118">Rather than using a custom VM image that includes certificates baked-in, you inject certificates into a running VM.</span></span> <span data-ttu-id="c9796-119">Tento proces zajišťuje instalaci hello nejaktuálnější certifikáty na webovém serveru během nasazení.</span><span class="sxs-lookup"><span data-stu-id="c9796-119">This process ensures that hello most up-to-date certificates are installed on a web server during deployment.</span></span> <span data-ttu-id="c9796-120">Je-li obnovit nebo nahradit certifikát, také nemáte toocreate novou vlastní bitovou kopii virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c9796-120">If you renew or replace a certificate, you don't also have toocreate a new custom VM image.</span></span> <span data-ttu-id="c9796-121">Hello nejnovější certifikáty jsou automaticky vložit, jako je vytváření dalších virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c9796-121">hello latest certificates are automatically injected as you create additional VMs.</span></span> <span data-ttu-id="c9796-122">Během celého procesu hello nikdy certifikáty hello nechte hello platformy Azure nebo jsou viditelné ve skriptu, historie příkazového řádku nebo šablony.</span><span class="sxs-lookup"><span data-stu-id="c9796-122">During hello whole process, hello certificates never leave hello Azure platform or are exposed in a script, command-line history, or template.</span></span>


## <a name="create-an-azure-key-vault"></a><span data-ttu-id="c9796-123">Vytvoření Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c9796-123">Create an Azure Key Vault</span></span>
<span data-ttu-id="c9796-124">Před vytvořením Key Vault a certifikáty, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="c9796-124">Before you can create a Key Vault and certificates, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="c9796-125">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupSecureWeb* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="c9796-125">hello following example creates a resource group named *myResourceGroupSecureWeb* in hello *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupSecureWeb --location eastus
```

<span data-ttu-id="c9796-126">Dále vytvořte Key Vault s [vytvořit az keyvault](/cli/azure/keyvault#create) a povolit pro použití při nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c9796-126">Next, create a Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable it for use when you deploy a VM.</span></span> <span data-ttu-id="c9796-127">Každý Key Vault vyžaduje jedinečný název a musí být všechny malá písmena.</span><span class="sxs-lookup"><span data-stu-id="c9796-127">Each Key Vault requires a unique name, and should be all lower case.</span></span> <span data-ttu-id="c9796-128">Nahraďte  *<mykeyvault>*  v hello následující příklad s svůj vlastní jedinečný název pro Key Vault:</span><span class="sxs-lookup"><span data-stu-id="c9796-128">Replace *<mykeyvault>* in hello following example with your own unique Key Vault name:</span></span>

```azurecli-interactive 
keyvault_name=<mykeyvault>
az keyvault create \
    --resource-group myResourceGroupSecureWeb \
    --name $keyvault_name \
    --enabled-for-deployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a><span data-ttu-id="c9796-129">Vygenerování certifikátu a uložit v Key Vault</span><span class="sxs-lookup"><span data-stu-id="c9796-129">Generate a certificate and store in Key Vault</span></span>
<span data-ttu-id="c9796-130">Pro použití v provozním prostředí, měli byste importovat platný certifikát podepsaný službou důvěryhodného zprostředkovatele s [import certifikátu keyvault az](/cli/azure/certificate#import).</span><span class="sxs-lookup"><span data-stu-id="c9796-130">For production use, you should import a valid certificate signed by trusted provider with [az keyvault certificate import](/cli/azure/certificate#import).</span></span> <span data-ttu-id="c9796-131">V tomto kurzu hello následující příklad ukazuje, jak můžete vygenerovat certifikát podepsaný svým držitelem s [vytvoření certifikátu keyvault az](/cli/azure/certificate#create) používající zásady certifikátů výchozí hello:</span><span class="sxs-lookup"><span data-stu-id="c9796-131">For this tutorial, hello following example shows how you can generate a self-signed certificate with [az keyvault certificate create](/cli/azure/certificate#create) that uses hello default certificate policy:</span></span>

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```

### <a name="prepare-a-certificate-for-use-with-a-vm"></a><span data-ttu-id="c9796-132">Příprava certifikát pro použití s virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c9796-132">Prepare a certificate for use with a VM</span></span>
<span data-ttu-id="c9796-133">certifikát hello toouse během hello virtuálního počítače vytvořit proces, získejte hello ID vašeho certifikátu s [az keyvault verze sdíleného tajného klíče seznam-](/cli/azure/keyvault/secret#list-versions).</span><span class="sxs-lookup"><span data-stu-id="c9796-133">toouse hello certificate during hello VM create process, obtain hello ID of your certificate with [az keyvault secret list-versions](/cli/azure/keyvault/secret#list-versions).</span></span> <span data-ttu-id="c9796-134">Převést hello certifikát s [az virtuálních počítačů formát secret](/cli/azure/vm#format-secret).</span><span class="sxs-lookup"><span data-stu-id="c9796-134">Convert hello certificate with [az vm format-secret](/cli/azure/vm#format-secret).</span></span> <span data-ttu-id="c9796-135">Následující ukázka Hello přiřadí výstup hello toovariables tyto příkazy pro snadné použití v hello další kroky:</span><span class="sxs-lookup"><span data-stu-id="c9796-135">hello following example assigns hello output of these commands toovariables for ease of use in hello next steps:</span></span>

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```

### <a name="create-a-cloud-init-config-toosecure-nginx"></a><span data-ttu-id="c9796-136">Vytvoření toosecure konfigurace cloudu init NGINX</span><span class="sxs-lookup"><span data-stu-id="c9796-136">Create a cloud-init config toosecure NGINX</span></span>
<span data-ttu-id="c9796-137">[Init cloudu](https://cloudinit.readthedocs.io) je často používaný přístup toocustomize virtuálního počítače s Linuxem jako spustí pro hello poprvé.</span><span class="sxs-lookup"><span data-stu-id="c9796-137">[Cloud-init](https://cloudinit.readthedocs.io) is a widely used approach toocustomize a Linux VM as it boots for hello first time.</span></span> <span data-ttu-id="c9796-138">Můžete používat cloudové init tooinstall balíčky a zapisovat soubory, nebo tooconfigure uživatele a zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="c9796-138">You can use cloud-init tooinstall packages and write files, or tooconfigure users and security.</span></span> <span data-ttu-id="c9796-139">Init cloudu běží během počáteční spouštění hello, neexistují žádné další kroky nebo požadováno agenty tooapply konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="c9796-139">As cloud-init runs during hello initial boot process, there are no additional steps or required agents tooapply your configuration.</span></span>

<span data-ttu-id="c9796-140">Když vytvoříte virtuální počítač, certifikáty a klíče jsou uložené v hello chráněné */var/lib/příkazwaagent/* adresáře.</span><span class="sxs-lookup"><span data-stu-id="c9796-140">When you create a VM, certificates and keys are stored in hello protected */var/lib/waagent/* directory.</span></span> <span data-ttu-id="c9796-141">Přidání tooautomate hello certifikát toohello virtuálního počítače a konfigurace hello webový server, použijte cloudu init.</span><span class="sxs-lookup"><span data-stu-id="c9796-141">tooautomate adding hello certificate toohello VM and configuring hello web server, use cloud-init.</span></span> <span data-ttu-id="c9796-142">V tomto příkladu jsme nainstalujte a nakonfigurujte hello NGINX webový server.</span><span class="sxs-lookup"><span data-stu-id="c9796-142">In this example, we install and configure hello NGINX web server.</span></span> <span data-ttu-id="c9796-143">Můžete použít hello stejné zpracovat tooinstall a nakonfigurujte Apache.</span><span class="sxs-lookup"><span data-stu-id="c9796-143">You can use hello same process tooinstall and configure Apache.</span></span> 

<span data-ttu-id="c9796-144">Vytvořte soubor s názvem *cloudu init webové server.txt* a hello vložte následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="c9796-144">Create a file named *cloud-init-web-server.txt* and paste hello following configuration:</span></span>

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
      }
runcmd:
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
```

### <a name="create-a-secure-vm"></a><span data-ttu-id="c9796-145">Vytvoření zabezpečeného virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c9796-145">Create a secure VM</span></span>
<span data-ttu-id="c9796-146">Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="c9796-146">Now create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="c9796-147">data certifikátu Hello je vložili z Key Vault s hello `--secrets` parametr.</span><span class="sxs-lookup"><span data-stu-id="c9796-147">hello certificate data is injected from Key Vault with hello `--secrets` parameter.</span></span> <span data-ttu-id="c9796-148">V konfiguraci cloudu init hello s hello předáte `--custom-data` parametr:</span><span class="sxs-lookup"><span data-stu-id="c9796-148">You pass in hello cloud-init config with hello `--custom-data` parameter:</span></span>

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-web-server.txt \
    --secrets "$vm_secret"
```

<span data-ttu-id="c9796-149">Jak dlouho trvá několik minut, než toobe hello virtuálního počítače vytvořena, hello balíčky tooinstall a toostart aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c9796-149">It takes a few minutes for hello VM toobe created, hello packages tooinstall, and hello app toostart.</span></span> <span data-ttu-id="c9796-150">Po vytvoření hello virtuálních počítačů, poznamenejte si hello `publicIpAddress` zobrazí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="c9796-150">When hello VM has been created, take note of hello `publicIpAddress` displayed by hello Azure CLI.</span></span> <span data-ttu-id="c9796-151">Tato adresa je použité tooaccess váš web ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c9796-151">This address is used tooaccess your site in a web browser.</span></span>

<span data-ttu-id="c9796-152">tooallow zabezpečit webové přenosy tooreach virtuální počítač, otevřete port 443 ze hello Internet s [az virtuálních počítačů open-port](/cli/azure/vm#open-port):</span><span class="sxs-lookup"><span data-stu-id="c9796-152">tooallow secure web traffic tooreach your VM, open port 443 from hello Internet with [az vm open-port](/cli/azure/vm#open-port):</span></span>

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --port 443
```


### <a name="test-hello-secure-web-app"></a><span data-ttu-id="c9796-153">Testování hello zabezpečení webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c9796-153">Test hello secure web app</span></span>
<span data-ttu-id="c9796-154">Nyní můžete otevřít webový prohlížeč a zadejte *https://<publicIpAddress>*  v panelu Adresa hello.</span><span class="sxs-lookup"><span data-stu-id="c9796-154">Now you can open a web browser and enter *https://<publicIpAddress>* in hello address bar.</span></span> <span data-ttu-id="c9796-155">Zadejte vlastní veřejná IP adresa z hello virtuálního počítače vytvořit proces.</span><span class="sxs-lookup"><span data-stu-id="c9796-155">Provide your own public IP address from hello VM create process.</span></span> <span data-ttu-id="c9796-156">Přijměte upozornění zabezpečení hello, pokud se používá certifikát podepsaný svým držitelem:</span><span class="sxs-lookup"><span data-stu-id="c9796-156">Accept hello security warning if you used a self-signed certificate:</span></span>

![Přijetí upozornění zabezpečení webového prohlížeče](./media/tutorial-secure-web-server/browser-warning.png)

<span data-ttu-id="c9796-158">Zabezpečené NGINX webu se následně zobrazí jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="c9796-158">Your secured NGINX site is then displayed as in hello following example:</span></span>

![Zobrazit spuštěná zabezpečené NGINX Web](./media/tutorial-secure-web-server/secured-nginx.png)


## <a name="next-steps"></a><span data-ttu-id="c9796-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c9796-160">Next steps</span></span>

<span data-ttu-id="c9796-161">V tomto kurzu webového serveru se službou NGINX zabezpečená certifikátem SSL uložené v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="c9796-161">In this tutorial, you secured an NGINX web server with an SSL certificate stored in Azure Key Vault.</span></span> <span data-ttu-id="c9796-162">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="c9796-162">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c9796-163">Vytvoření Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c9796-163">Create an Azure Key Vault</span></span>
> * <span data-ttu-id="c9796-164">Generovat nebo nahrát certifikát toohello Key Vault</span><span class="sxs-lookup"><span data-stu-id="c9796-164">Generate or upload a certificate toohello Key Vault</span></span>
> * <span data-ttu-id="c9796-165">Vytvoření virtuálního počítače a instalaci hello NGINX webového serveru</span><span class="sxs-lookup"><span data-stu-id="c9796-165">Create a VM and install hello NGINX web server</span></span>
> * <span data-ttu-id="c9796-166">Vložit hello certifikát do hello virtuálního počítače a konfigurace NGINX s vazbou SSL</span><span class="sxs-lookup"><span data-stu-id="c9796-166">Inject hello certificate into hello VM and configure NGINX with an SSL binding</span></span>

<span data-ttu-id="c9796-167">Použijte tento odkaz toosee předem vytvořené ukázky skript virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c9796-167">Follow this link toosee pre-built virtual machine script samples.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c9796-168">Ukázky skriptu Windows virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="c9796-168">Windows virtual machine script samples</span></span>](./cli-samples.md)