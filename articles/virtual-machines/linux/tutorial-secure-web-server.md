---
title: "Zabezpečení webového serveru pomocí certifikátů SSL v Azure | Microsoft Docs"
description: "Zjistěte, jak zabezpečit NGINX webového serveru pomocí certifikátů SSL na virtuální počítač s Linuxem v Azure"
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
ms.openlocfilehash: 181be35aeb61020db3abaeba22aa882848923c31
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="secure-a-web-server-with-ssl-certificates-on-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="49b80-103">Zabezpečení webového serveru pomocí certifikátů SSL na virtuální počítač s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="49b80-103">Secure a web server with SSL certificates on a Linux virtual machine in Azure</span></span>
<span data-ttu-id="49b80-104">Pro zabezpečení webové servery, certifikát později SSL (Secure Sockets) slouží k šifrování webový provoz.</span><span class="sxs-lookup"><span data-stu-id="49b80-104">To secure web servers, a Secure Sockets Later (SSL) certificate can be used to encrypt web traffic.</span></span> <span data-ttu-id="49b80-105">Tyto certifikáty SSL můžou být uložená v Azure Key Vault a povolit zabezpečená nasazení certifikátů na virtuálních počítačích (VM) s Linuxem v Azure.</span><span class="sxs-lookup"><span data-stu-id="49b80-105">These SSL certificates can be stored in Azure Key Vault, and allow secure deployments of certificates to Linux virtual machines (VMs) in Azure.</span></span> <span data-ttu-id="49b80-106">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="49b80-106">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="49b80-107">Vytvoření Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="49b80-107">Create an Azure Key Vault</span></span>
> * <span data-ttu-id="49b80-108">Generovat nebo nahrajte certifikát do služby Key Vault</span><span class="sxs-lookup"><span data-stu-id="49b80-108">Generate or upload a certificate to the Key Vault</span></span>
> * <span data-ttu-id="49b80-109">Vytvoření virtuálního počítače a nainstalovat webový server NGINX</span><span class="sxs-lookup"><span data-stu-id="49b80-109">Create a VM and install the NGINX web server</span></span>
> * <span data-ttu-id="49b80-110">Vložit certifikát do virtuálního počítače a konfigurace NGINX s vazbou SSL</span><span class="sxs-lookup"><span data-stu-id="49b80-110">Inject the certificate into the VM and configure NGINX with an SSL binding</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="49b80-111">Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="49b80-111">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="49b80-112">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="49b80-112">Run `az --version` to find the version.</span></span> <span data-ttu-id="49b80-113">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="49b80-113">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>  


## <a name="overview"></a><span data-ttu-id="49b80-114">Přehled</span><span class="sxs-lookup"><span data-stu-id="49b80-114">Overview</span></span>
<span data-ttu-id="49b80-115">Azure Key Vault chrání kryptografické klíče a tajné klíče, tyto certifikáty a hesla.</span><span class="sxs-lookup"><span data-stu-id="49b80-115">Azure Key Vault safeguards cryptographic keys and secrets, such certificates or passwords.</span></span> <span data-ttu-id="49b80-116">Key Vault pomáhá zjednodušit proces správy certifikátů a zajišťuje vám kontrolu nad klíči, které přístup těchto certifikátů.</span><span class="sxs-lookup"><span data-stu-id="49b80-116">Key Vault helps streamline the certificate management process and enables you to maintain control of keys that access those certificates.</span></span> <span data-ttu-id="49b80-117">Můžete vytvořit certifikát podepsaný svým držitelem v Key Vault nebo nahrát certifikát existující, důvěryhodné, který už vlastníte.</span><span class="sxs-lookup"><span data-stu-id="49b80-117">You can create a self-signed certificate inside Key Vault, or upload an existing, trusted certificate that you already own.</span></span>

<span data-ttu-id="49b80-118">Místo použití vlastní image virtuálního počítače, který zahrnuje certifikáty zaručená v, vložit certifikáty do spuštěného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="49b80-118">Rather than using a custom VM image that includes certificates baked-in, you inject certificates into a running VM.</span></span> <span data-ttu-id="49b80-119">Tento proces zajišťuje, že nejaktuálnější certifikáty jsou nainstalovány na webovém serveru během nasazení.</span><span class="sxs-lookup"><span data-stu-id="49b80-119">This process ensures that the most up-to-date certificates are installed on a web server during deployment.</span></span> <span data-ttu-id="49b80-120">Je-li obnovit nebo nahradit certifikát, nemáte také vytvořit novou vlastní imagi virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="49b80-120">If you renew or replace a certificate, you don't also have to create a new custom VM image.</span></span> <span data-ttu-id="49b80-121">Nejnovější certifikáty jsou automaticky vložit, jako je vytváření dalších virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="49b80-121">The latest certificates are automatically injected as you create additional VMs.</span></span> <span data-ttu-id="49b80-122">Během celého procesu certifikáty nikdy nechte platformy Azure nebo jsou viditelné ve skriptu, historie příkazového řádku nebo šablony.</span><span class="sxs-lookup"><span data-stu-id="49b80-122">During the whole process, the certificates never leave the Azure platform or are exposed in a script, command-line history, or template.</span></span>


## <a name="create-an-azure-key-vault"></a><span data-ttu-id="49b80-123">Vytvoření Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="49b80-123">Create an Azure Key Vault</span></span>
<span data-ttu-id="49b80-124">Před vytvořením Key Vault a certifikáty, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="49b80-124">Before you can create a Key Vault and certificates, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="49b80-125">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupSecureWeb* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="49b80-125">The following example creates a resource group named *myResourceGroupSecureWeb* in the *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupSecureWeb --location eastus
```

<span data-ttu-id="49b80-126">Dále vytvořte Key Vault s [vytvořit az keyvault](/cli/azure/keyvault#create) a povolit pro použití při nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="49b80-126">Next, create a Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable it for use when you deploy a VM.</span></span> <span data-ttu-id="49b80-127">Každý Key Vault vyžaduje jedinečný název a musí být všechny malá písmena.</span><span class="sxs-lookup"><span data-stu-id="49b80-127">Each Key Vault requires a unique name, and should be all lower case.</span></span> <span data-ttu-id="49b80-128">Nahraďte  *<mykeyvault>*  v následujícím příkladu se svůj vlastní jedinečný název pro Key Vault:</span><span class="sxs-lookup"><span data-stu-id="49b80-128">Replace *<mykeyvault>* in the following example with your own unique Key Vault name:</span></span>

```azurecli-interactive 
keyvault_name=<mykeyvault>
az keyvault create \
    --resource-group myResourceGroupSecureWeb \
    --name $keyvault_name \
    --enabled-for-deployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a><span data-ttu-id="49b80-129">Vygenerování certifikátu a uložit v Key Vault</span><span class="sxs-lookup"><span data-stu-id="49b80-129">Generate a certificate and store in Key Vault</span></span>
<span data-ttu-id="49b80-130">Pro použití v provozním prostředí, měli byste importovat platný certifikát podepsaný službou důvěryhodného zprostředkovatele s [import certifikátu keyvault az](/cli/azure/certificate#import).</span><span class="sxs-lookup"><span data-stu-id="49b80-130">For production use, you should import a valid certificate signed by trusted provider with [az keyvault certificate import](/cli/azure/certificate#import).</span></span> <span data-ttu-id="49b80-131">V tomto kurzu následující příklad ukazuje, jak můžete vygenerovat certifikát podepsaný svým držitelem s [vytvoření certifikátu keyvault az](/cli/azure/certificate#create) používající výchozí zásady certifikátu:</span><span class="sxs-lookup"><span data-stu-id="49b80-131">For this tutorial, the following example shows how you can generate a self-signed certificate with [az keyvault certificate create](/cli/azure/certificate#create) that uses the default certificate policy:</span></span>

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```

### <a name="prepare-a-certificate-for-use-with-a-vm"></a><span data-ttu-id="49b80-132">Příprava certifikát pro použití s virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="49b80-132">Prepare a certificate for use with a VM</span></span>
<span data-ttu-id="49b80-133">K použití certifikátu během virtuálního počítače vytvořit proces, získat číslo ID vašeho certifikátu s [az keyvault verze sdíleného tajného klíče seznam-](/cli/azure/keyvault/secret#list-versions).</span><span class="sxs-lookup"><span data-stu-id="49b80-133">To use the certificate during the VM create process, obtain the ID of your certificate with [az keyvault secret list-versions](/cli/azure/keyvault/secret#list-versions).</span></span> <span data-ttu-id="49b80-134">Převést certifikát s [az virtuálních počítačů formát secret](/cli/azure/vm#format-secret).</span><span class="sxs-lookup"><span data-stu-id="49b80-134">Convert the certificate with [az vm format-secret](/cli/azure/vm#format-secret).</span></span> <span data-ttu-id="49b80-135">Následující příklad přiřadí proměnné pro snadné použití v dalších krocích výstup z těchto příkazů:</span><span class="sxs-lookup"><span data-stu-id="49b80-135">The following example assigns the output of these commands to variables for ease of use in the next steps:</span></span>

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```

### <a name="create-a-cloud-init-config-to-secure-nginx"></a><span data-ttu-id="49b80-136">Vytvoření konfigurace cloudu init zabezpečit NGINX</span><span class="sxs-lookup"><span data-stu-id="49b80-136">Create a cloud-init config to secure NGINX</span></span>
<span data-ttu-id="49b80-137">[Init cloudu](https://cloudinit.readthedocs.io) je často používaný přístup k přizpůsobení virtuálního počítače s Linuxem, jako při prvním spuštění.</span><span class="sxs-lookup"><span data-stu-id="49b80-137">[Cloud-init](https://cloudinit.readthedocs.io) is a widely used approach to customize a Linux VM as it boots for the first time.</span></span> <span data-ttu-id="49b80-138">Init cloudu můžete použít k instalaci balíčků a zapisovat soubory nebo konfigurace zabezpečení a uživatelů.</span><span class="sxs-lookup"><span data-stu-id="49b80-138">You can use cloud-init to install packages and write files, or to configure users and security.</span></span> <span data-ttu-id="49b80-139">Init cloudu běží během úvodního spouštění, nejsou žádné další kroky nebo požadované agenty použít konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="49b80-139">As cloud-init runs during the initial boot process, there are no additional steps or required agents to apply your configuration.</span></span>

<span data-ttu-id="49b80-140">Když vytvoříte virtuální počítač, certifikáty a klíče jsou uložené v chráněného */var/lib/příkazwaagent/* adresáře.</span><span class="sxs-lookup"><span data-stu-id="49b80-140">When you create a VM, certificates and keys are stored in the protected */var/lib/waagent/* directory.</span></span> <span data-ttu-id="49b80-141">Pokud chcete automatizovat certifikát se přidává do virtuálního počítače a konfigurace webového serveru, použijte init cloudu.</span><span class="sxs-lookup"><span data-stu-id="49b80-141">To automate adding the certificate to the VM and configuring the web server, use cloud-init.</span></span> <span data-ttu-id="49b80-142">V tomto příkladu jsme nainstalujte a nakonfigurujte NGINX webový server.</span><span class="sxs-lookup"><span data-stu-id="49b80-142">In this example, we install and configure the NGINX web server.</span></span> <span data-ttu-id="49b80-143">Stejný postup můžete použít k instalaci a konfiguraci Apache.</span><span class="sxs-lookup"><span data-stu-id="49b80-143">You can use the same process to install and configure Apache.</span></span> 

<span data-ttu-id="49b80-144">Vytvořte soubor s názvem *cloudu init webové server.txt* a vložte následující konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="49b80-144">Create a file named *cloud-init-web-server.txt* and paste the following configuration:</span></span>

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

### <a name="create-a-secure-vm"></a><span data-ttu-id="49b80-145">Vytvoření zabezpečeného virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="49b80-145">Create a secure VM</span></span>
<span data-ttu-id="49b80-146">Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="49b80-146">Now create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="49b80-147">Data certifikátu je vložili z trezoru klíčů `--secrets` parametr.</span><span class="sxs-lookup"><span data-stu-id="49b80-147">The certificate data is injected from Key Vault with the `--secrets` parameter.</span></span> <span data-ttu-id="49b80-148">Předáte v konfiguraci cloudu init s `--custom-data` parametr:</span><span class="sxs-lookup"><span data-stu-id="49b80-148">You pass in the cloud-init config with the `--custom-data` parameter:</span></span>

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

<span data-ttu-id="49b80-149">Trvá několik minut pro virtuální počítač, který se má vytvořit, balíčky určené k instalaci a aplikaci spusťte.</span><span class="sxs-lookup"><span data-stu-id="49b80-149">It takes a few minutes for the VM to be created, the packages to install, and the app to start.</span></span> <span data-ttu-id="49b80-150">Po vytvoření virtuálního počítače, poznamenejte si `publicIpAddress` zobrazí pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="49b80-150">When the VM has been created, take note of the `publicIpAddress` displayed by the Azure CLI.</span></span> <span data-ttu-id="49b80-151">Tato adresa se používá k přístupu k webu ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="49b80-151">This address is used to access your site in a web browser.</span></span>

<span data-ttu-id="49b80-152">Povolit zabezpečený webový provoz připojit virtuální počítač, otevřete port 443 z Internetu s [az virtuálních počítačů open-port](/cli/azure/vm#open-port):</span><span class="sxs-lookup"><span data-stu-id="49b80-152">To allow secure web traffic to reach your VM, open port 443 from the Internet with [az vm open-port](/cli/azure/vm#open-port):</span></span>

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --port 443
```


### <a name="test-the-secure-web-app"></a><span data-ttu-id="49b80-153">Testování zabezpečení webové aplikace</span><span class="sxs-lookup"><span data-stu-id="49b80-153">Test the secure web app</span></span>
<span data-ttu-id="49b80-154">Nyní můžete otevřít webový prohlížeč a zadejte *https://<publicIpAddress>*  na panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="49b80-154">Now you can open a web browser and enter *https://<publicIpAddress>* in the address bar.</span></span> <span data-ttu-id="49b80-155">Zadejte vlastní veřejná IP adresa z virtuálního počítače vytvořit proces.</span><span class="sxs-lookup"><span data-stu-id="49b80-155">Provide your own public IP address from the VM create process.</span></span> <span data-ttu-id="49b80-156">Pokud použijete certifikát podepsaný svým držitelem, přijměte upozornění zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="49b80-156">Accept the security warning if you used a self-signed certificate:</span></span>

![Přijetí upozornění zabezpečení webového prohlížeče](./media/tutorial-secure-web-server/browser-warning.png)

<span data-ttu-id="49b80-158">Zabezpečené NGINX webu se následně zobrazí jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="49b80-158">Your secured NGINX site is then displayed as in the following example:</span></span>

![Zobrazit spuštěná zabezpečené NGINX Web](./media/tutorial-secure-web-server/secured-nginx.png)


## <a name="next-steps"></a><span data-ttu-id="49b80-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="49b80-160">Next steps</span></span>

<span data-ttu-id="49b80-161">V tomto kurzu webového serveru se službou NGINX zabezpečená certifikátem SSL uložené v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="49b80-161">In this tutorial, you secured an NGINX web server with an SSL certificate stored in Azure Key Vault.</span></span> <span data-ttu-id="49b80-162">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="49b80-162">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="49b80-163">Vytvoření Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="49b80-163">Create an Azure Key Vault</span></span>
> * <span data-ttu-id="49b80-164">Generovat nebo nahrajte certifikát do služby Key Vault</span><span class="sxs-lookup"><span data-stu-id="49b80-164">Generate or upload a certificate to the Key Vault</span></span>
> * <span data-ttu-id="49b80-165">Vytvoření virtuálního počítače a nainstalovat webový server NGINX</span><span class="sxs-lookup"><span data-stu-id="49b80-165">Create a VM and install the NGINX web server</span></span>
> * <span data-ttu-id="49b80-166">Vložit certifikát do virtuálního počítače a konfigurace NGINX s vazbou SSL</span><span class="sxs-lookup"><span data-stu-id="49b80-166">Inject the certificate into the VM and configure NGINX with an SSL binding</span></span>

<span data-ttu-id="49b80-167">Tento odkaz zobrazíte ukázky skriptu předdefinovaných virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="49b80-167">Follow this link to see pre-built virtual machine script samples.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="49b80-168">Ukázky skriptu Windows virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="49b80-168">Windows virtual machine script samples</span></span>](./cli-samples.md)