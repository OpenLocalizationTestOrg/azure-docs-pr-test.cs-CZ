---
title: "aaaCreate sady škálování virtuálního počítače pro Linux v Azure | Microsoft Docs"
description: "Vytvoření a nasazení vysoce dostupné aplikace na virtuální počítače s Linuxem pomocí škálovací sadu virtuálních počítačů"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 08/11/2017
ms.author: iainfou
ms.openlocfilehash: 00dd81043f9be46ef2dc6dfe97eefdb20944ee13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-linux"></a><span data-ttu-id="9404b-103">Vytvořit sadu škálování virtuálního počítače a nasazení vysoce dostupné aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="9404b-103">Create a Virtual Machine Scale Set and deploy a highly available app on Linux</span></span>
<span data-ttu-id="9404b-104">Škálovací sadu virtuálních počítačů vám umožní toodeploy a spravovat sadu identické, automatické škálování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9404b-104">A virtual machine scale set allows you toodeploy and manage a set of identical, auto-scaling virtual machines.</span></span> <span data-ttu-id="9404b-105">Můžete škálovat hello počet virtuálních počítačů v sadě škálování hello ručně nebo definovat tooautoscale pravidla na základě využití procesoru, paměti vyžádání nebo síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="9404b-105">You can scale hello number of VMs in hello scale set manually, or define rules tooautoscale based on CPU usage, memory demand, or network traffic.</span></span> <span data-ttu-id="9404b-106">V tomto kurzu nasadíte škálování virtuálních počítačů, nastavte v Azure.</span><span class="sxs-lookup"><span data-stu-id="9404b-106">In this tutorial, you deploy a virtual machine scale set in Azure.</span></span> <span data-ttu-id="9404b-107">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="9404b-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9404b-108">Použít cloudové init toocreate tooscale aplikaci</span><span class="sxs-lookup"><span data-stu-id="9404b-108">Use cloud-init toocreate an app tooscale</span></span>
> * <span data-ttu-id="9404b-109">Vytvoření sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="9404b-109">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="9404b-110">Zvýšení nebo snížení hello počet instancí v sadě škálování</span><span class="sxs-lookup"><span data-stu-id="9404b-110">Increase or decrease hello number of instances in a scale set</span></span>
> * <span data-ttu-id="9404b-111">Zobrazit informace o připojení pro instance škálovací sady</span><span class="sxs-lookup"><span data-stu-id="9404b-111">View connection info for scale set instances</span></span>
> * <span data-ttu-id="9404b-112">Použití datových disků ve škálovací sadě</span><span class="sxs-lookup"><span data-stu-id="9404b-112">Use data disks in a scale set</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="9404b-113">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="9404b-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="9404b-114">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="9404b-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="9404b-115">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9404b-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="scale-set-overview"></a><span data-ttu-id="9404b-116">Přehled sady škálování</span><span class="sxs-lookup"><span data-stu-id="9404b-116">Scale Set overview</span></span>
<span data-ttu-id="9404b-117">Škálovací sadu virtuálních počítačů vám umožní toodeploy a spravovat sadu identické, automatické škálování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9404b-117">A virtual machine scale set allows you toodeploy and manage a set of identical, auto-scaling virtual machines.</span></span> <span data-ttu-id="9404b-118">Škálování nastaví hello použijte stejné komponenty jako Seznámili jste se v předchozí kurzu hello příliš[vytvořit vysoce dostupné virtuální počítače](tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="9404b-118">Scale sets use hello same components as you learned about in hello previous tutorial too[Create highly available VMs](tutorial-availability-sets.md).</span></span> <span data-ttu-id="9404b-119">Virtuální počítače ve škálovací sadě jsou vytvořeny ve skupině dostupnosti nastavena a distribuovaných napříč doménami selhání a aktualizace logiku.</span><span class="sxs-lookup"><span data-stu-id="9404b-119">VMs in a scale set are created in an availability set and distributed across logic fault and update domains.</span></span>

<span data-ttu-id="9404b-120">Podle potřeby ve škálovací sadě virtuálních počítačů vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="9404b-120">VMs are created as needed in a scale set.</span></span> <span data-ttu-id="9404b-121">Můžete definovat pravidla toocontrol škálování jak a kdy se přidají nebo odeberou z hello škálovací sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9404b-121">You define autoscale rules toocontrol how and when VMs are added or removed from hello scale set.</span></span> <span data-ttu-id="9404b-122">Tato pravidla můžete spustit na základě metriky například zatížení procesoru, využití paměti nebo síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="9404b-122">These rules can trigger based on metrics such as CPU load, memory usage, or network traffic.</span></span>

<span data-ttu-id="9404b-123">Škálování nastaví podporu až too1 000 virtuálních počítačů, pokud používáte image platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="9404b-123">Scale sets support up too1,000 VMs when you use an Azure platform image.</span></span> <span data-ttu-id="9404b-124">Pro úlohy v produkčním prostředí může příliš chcete[vytvořit vlastní image virtuálního počítače](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="9404b-124">For production workloads, you may wish too[Create a custom VM image](tutorial-custom-images.md).</span></span> <span data-ttu-id="9404b-125">Můžete vytvořit až too100 virtuálních počítačů v škálování nastavit při použití vlastní image.</span><span class="sxs-lookup"><span data-stu-id="9404b-125">You can create up too100 VMs in a scale set when using a custom image.</span></span>


## <a name="create-an-app-tooscale"></a><span data-ttu-id="9404b-126">Vytvoření tooscale aplikaci</span><span class="sxs-lookup"><span data-stu-id="9404b-126">Create an app tooscale</span></span>
<span data-ttu-id="9404b-127">Pro použití v provozním prostředí, můžete příliš[vytvořit vlastní image virtuálního počítače](tutorial-custom-images.md) , který obsahuje aplikace nainstalována a nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="9404b-127">For production use, you may wish too[Create a custom VM image](tutorial-custom-images.md) that includes your application installed and configured.</span></span> <span data-ttu-id="9404b-128">V tomto kurzu umožňuje přizpůsobit hello virtuální počítače na první spouštěcí tooquickly najdete v části nastavení v akci škálování.</span><span class="sxs-lookup"><span data-stu-id="9404b-128">For this tutorial, lets customize hello VMs on first boot tooquickly see a scale set in action.</span></span>

<span data-ttu-id="9404b-129">V předchozích kurzu jste se dozvěděli [jak toocustomize virtuální počítač s Linuxem na při prvním spuštění](tutorial-automate-vm-deployment.md) s inicializací cloudu.</span><span class="sxs-lookup"><span data-stu-id="9404b-129">In a previous tutorial, you learned [How toocustomize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md) with cloud-init.</span></span> <span data-ttu-id="9404b-130">Můžete použít hello stejné cloudové init konfigurační soubor tooinstall NGINX a spusťte jednoduchou aplikaci Node.js "Hello, World".</span><span class="sxs-lookup"><span data-stu-id="9404b-130">You can use hello same cloud-init configuration file tooinstall NGINX and run a simple 'Hello World' Node.js app.</span></span> 

<span data-ttu-id="9404b-131">V aktuálním prostředí, vytvořte soubor s názvem *cloudu init.txt* a hello vložte následující konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9404b-131">In your current shell, create a file named *cloud-init.txt* and paste hello following configuration.</span></span> <span data-ttu-id="9404b-132">Můžete například vytvořte soubor hello v hello cloudové prostředí není na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="9404b-132">For example, create hello file in hello Cloud Shell not on your local machine.</span></span> <span data-ttu-id="9404b-133">Zadejte `sensible-editor cloud-init.txt` toocreate hello souboru a zobrazit seznam dostupných editory.</span><span class="sxs-lookup"><span data-stu-id="9404b-133">Enter `sensible-editor cloud-init.txt` toocreate hello file and see a list of available editors.</span></span> <span data-ttu-id="9404b-134">Ujistěte se, že tento soubor celou cloudu init hello správně zkopírován, zejména hello první řádek:</span><span class="sxs-lookup"><span data-stu-id="9404b-134">Make sure that hello whole cloud-init file is copied correctly, especially hello first line:</span></span>

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


## <a name="create-a-scale-set"></a><span data-ttu-id="9404b-135">Vytvoření sady škálování</span><span class="sxs-lookup"><span data-stu-id="9404b-135">Create a scale set</span></span>
<span data-ttu-id="9404b-136">Před vytvořením sady škálování, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="9404b-136">Before you can create a scale set, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="9404b-137">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupScaleSet* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="9404b-137">hello following example creates a resource group named *myResourceGroupScaleSet* in hello *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupScaleSet --location eastus
```

<span data-ttu-id="9404b-138">Nyní vytvoří škálování virtuálního počítače s [vytvořit az vmss](/cli/azure/vmss#create).</span><span class="sxs-lookup"><span data-stu-id="9404b-138">Now create a virtual machine scale set with [az vmss create](/cli/azure/vmss#create).</span></span> <span data-ttu-id="9404b-139">Hello následující příklad vytvoří škálování nastavení s názvem *myScaleSet*, používá hello cloudu init souboru toocustomize hello virtuálních počítačů a generuje klíče SSH, pokud neexistují:</span><span class="sxs-lookup"><span data-stu-id="9404b-139">hello following example creates a scale set named *myScaleSet*, uses hello cloud-init file toocustomize hello VM, and generates SSH keys if they do not exist:</span></span>

```azurecli-interactive 
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys      
```

<span data-ttu-id="9404b-140">Přebírá toocreate pár minut a konfigurace všech hello škálování sadu prostředků a virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="9404b-140">It takes a few minutes toocreate and configure all hello scale set resources and VMs.</span></span> <span data-ttu-id="9404b-141">Existují úlohy na pozadí, které pokračovat toorun po hello příkazového řádku Azure CLI vrátí, toohello řádku.</span><span class="sxs-lookup"><span data-stu-id="9404b-141">There are background tasks that continue toorun after hello Azure CLI returns you toohello prompt.</span></span> <span data-ttu-id="9404b-142">To může být jiná několik minut před přístupem k aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="9404b-142">It may be another couple of minutes before you can access hello app.</span></span>


## <a name="allow-web-traffic"></a><span data-ttu-id="9404b-143">Povolit webový provoz</span><span class="sxs-lookup"><span data-stu-id="9404b-143">Allow web traffic</span></span>
<span data-ttu-id="9404b-144">Nástroj pro vyrovnávání zatížení se vytvoří automaticky v rámci sady škálování virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="9404b-144">A load balancer was created automatically as part of hello virtual machine scale set.</span></span> <span data-ttu-id="9404b-145">Nástroj pro vyrovnávání zatížení Hello rozděluje zatížení mezi sadu definované virtuálních počítačů pomocí pravidla nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="9404b-145">hello load balancer distributes traffic across a set of defined VMs using load balancer rules.</span></span> <span data-ttu-id="9404b-146">Další informace o konceptech služby Vyrovnávání zatížení a konfiguraci v dalším kurzu hello, [jak tooload vyvážit virtuálních počítačů v Azure](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="9404b-146">You can learn more about load balancer concepts and configuration in hello next tutorial, [How tooload balance virtual machines in Azure](tutorial-load-balancer.md).</span></span>

<span data-ttu-id="9404b-147">tooallow provoz tooreach hello webové aplikace, vytvořte pravidlo s [vytvořit pravidlo vyrovnáváním zatížení sítě az](/cli/azure/network/lb/rule#create).</span><span class="sxs-lookup"><span data-stu-id="9404b-147">tooallow traffic tooreach hello web app, create a rule with [az network lb rule create](/cli/azure/network/lb/rule#create).</span></span> <span data-ttu-id="9404b-148">Hello následující příklad vytvoří pravidlo s názvem *myLoadBalancerRuleWeb*:</span><span class="sxs-lookup"><span data-stu-id="9404b-148">hello following example creates a rule named *myLoadBalancerRuleWeb*:</span></span>

```azurecli-interactive 
az network lb rule create \
  --resource-group myResourceGroupScaleSet \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```

## <a name="test-your-app"></a><span data-ttu-id="9404b-149">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="9404b-149">Test your app</span></span>
<span data-ttu-id="9404b-150">toosee aplikace Node.js na webu hello získat hello veřejnou IP adresu nástroj pro vyrovnávání zatížení s [az sítě veřejné ip zobrazit](/cli/azure/network/public-ip#show).</span><span class="sxs-lookup"><span data-stu-id="9404b-150">toosee your Node.js app on hello web, obtain hello public IP address of your load balancer with [az network public-ip show](/cli/azure/network/public-ip#show).</span></span> <span data-ttu-id="9404b-151">Hello následující příklad získá hello IP adresu pro *myScaleSetLBPublicIP* vytvořen jako součást sady škálování hello:</span><span class="sxs-lookup"><span data-stu-id="9404b-151">hello following example obtains hello IP address for *myScaleSetLBPublicIP* created as part of hello scale set:</span></span>

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetLBPublicIP \
    --query [ipAddress] \
    --output tsv
```

<span data-ttu-id="9404b-152">Zadejte ve webovém prohlížeči tooa hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="9404b-152">Enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="9404b-153">Zobrazí se aplikace Hello, včetně hello název hostitele virtuálních počítačů této hello načíst vyrovnávání distribuované provoz hello:</span><span class="sxs-lookup"><span data-stu-id="9404b-153">hello app is displayed, including hello hostname of hello VM that hello load balancer distributed traffic to:</span></span>

![Spuštěné aplikace Node.js](./media/tutorial-create-vmss/running-nodejs-app.png)

<span data-ttu-id="9404b-155">sad v akci škálování hello toosee, můžete můžete vynutit obnovení webové prohlížeče toosee hello zatížení vyrovnávání provoz distribuovat do všech virtuálních počítačů hello používající vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9404b-155">toosee hello scale set in action, you can force-refresh your web browser toosee hello load balancer distribute traffic across all hello VMs running your app.</span></span>


## <a name="management-tasks"></a><span data-ttu-id="9404b-156">Úlohy správy</span><span class="sxs-lookup"><span data-stu-id="9404b-156">Management tasks</span></span>
<span data-ttu-id="9404b-157">V průběhu cyklu hello škálovací sady hello, může být nutné toorun jedné nebo více úloh správy.</span><span class="sxs-lookup"><span data-stu-id="9404b-157">Throughout hello lifecycle of hello scale set, you may need toorun one or more management tasks.</span></span> <span data-ttu-id="9404b-158">Kromě toho můžete toocreate skripty, které automatizují různé úlohy životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="9404b-158">Additionally, you may want toocreate scripts that automate various lifecycle-tasks.</span></span> <span data-ttu-id="9404b-159">Hello 2.0 rozhraní příkazového řádku Azure poskytuje rychlý způsob toodo tyto úlohy.</span><span class="sxs-lookup"><span data-stu-id="9404b-159">hello Azure CLI 2.0 provides a quick way toodo those tasks.</span></span> <span data-ttu-id="9404b-160">Tady jsou několik běžných úloh.</span><span class="sxs-lookup"><span data-stu-id="9404b-160">Here are a few common tasks.</span></span>

### <a name="view-vms-in-a-scale-set"></a><span data-ttu-id="9404b-161">Zobrazení virtuální počítače ve škálovací sadě</span><span class="sxs-lookup"><span data-stu-id="9404b-161">View VMs in a scale set</span></span>
<span data-ttu-id="9404b-162">nastavit seznam virtuálních počítačů spuštěných ve vašem škálování tooview, použijte [az vmss seznamu instance](/cli/azure/vmss#list-instances) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9404b-162">tooview a list of VMs running in your scale set, use [az vmss list-instances](/cli/azure/vmss#list-instances) as follows:</span></span>

```azurecli-interactive 
az vmss list-instances \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --output table
```

<span data-ttu-id="9404b-163">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="9404b-163">hello output is similar toohello following example:</span></span>

```azurecli-interactive 
  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup            VmId
------------  --------------------  ----------  ------------  -------------------  -----------------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUPSCALESET  c72ddc34-6c41-4a53-b89e-dd24f27b30ab
           3  True                  eastus      myScaleSet_3  Succeeded            MYRESOURCEGROUPSCALESET  44266022-65c3-49c5-92dd-88ffa64f95da
```


### <a name="increase-or-decrease-vm-instances"></a><span data-ttu-id="9404b-164">Zvýšení nebo snížení instance virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="9404b-164">Increase or decrease VM instances</span></span>
<span data-ttu-id="9404b-165">toosee hello počet instancí aktuálně v škálování nastavíte, použijte [az vmss zobrazit](/cli/azure/vmss#show) a dotazovat se na *sku.capacity*:</span><span class="sxs-lookup"><span data-stu-id="9404b-165">toosee hello number of instances you currently have in a scale set, use [az vmss show](/cli/azure/vmss#show) and query on *sku.capacity*:</span></span>

```azurecli-interactive 
az vmss show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

<span data-ttu-id="9404b-166">Potom můžete ručně zvýšit nebo snížit hello počet virtuálních počítačů v hello škálování s [az vmss škálování](/cli/azure/vmss#scale).</span><span class="sxs-lookup"><span data-stu-id="9404b-166">You can then manually increase or decrease hello number of virtual machines in hello scale set with [az vmss scale](/cli/azure/vmss#scale).</span></span> <span data-ttu-id="9404b-167">Hello následující příklad nastaví hello počet virtuálních počítačů ve vaší příliš sad škálování*5*:</span><span class="sxs-lookup"><span data-stu-id="9404b-167">hello following example sets hello number of VMs in your scale set too*5*:</span></span>

```azurecli-interactive 
az vmss scale \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --new-capacity 5
```

<span data-ttu-id="9404b-168">Škálování pravidla umožňují definovat, jak nastavit tooscale nahoru nebo dolů hello počet virtuálních počítačů ve vaší škálování v odpovědi toodemand například síťový provoz nebo využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="9404b-168">Autoscale rules let you define how tooscale up or down hello number of VMs in your scale set in response toodemand such as network traffic or CPU usage.</span></span> <span data-ttu-id="9404b-169">Tato pravidla v současné době nelze nastavit v Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="9404b-169">Currently, these rules cannot be set in Azure CLI 2.0.</span></span> <span data-ttu-id="9404b-170">Použití hello [portál Azure](https://portal.azure.com) tooconfigure škálování.</span><span class="sxs-lookup"><span data-stu-id="9404b-170">Use hello [Azure portal](https://portal.azure.com) tooconfigure autoscale.</span></span>

### <a name="get-connection-info"></a><span data-ttu-id="9404b-171">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="9404b-171">Get connection info</span></span>
<span data-ttu-id="9404b-172">informace o připojení tooobtain o hello v škálovací sady virtuálních počítačů, použijte [az vmss seznamu--připojení-informace o instanci](/cli/azure/vmss#list-instance-connection-info).</span><span class="sxs-lookup"><span data-stu-id="9404b-172">tooobtain connection information about hello VMs in your scale sets, use [az vmss list-instance-connection-info](/cli/azure/vmss#list-instance-connection-info).</span></span> <span data-ttu-id="9404b-173">Tento příkaz vypíše hello veřejnou IP adresu a port pro každý virtuální počítač, který vám umožní tooconnect pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="9404b-173">This command outputs hello public IP address and port for each VM that allows you tooconnect with SSH:</span></span>

```azurecli-interactive 
az vmss list-instance-connection-info \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet
```


## <a name="use-data-disks-with-scale-sets"></a><span data-ttu-id="9404b-174">Použití datových disků s sady škálování</span><span class="sxs-lookup"><span data-stu-id="9404b-174">Use data disks with scale sets</span></span>
<span data-ttu-id="9404b-175">Můžete vytvořit a používat datových disků s sady škálování.</span><span class="sxs-lookup"><span data-stu-id="9404b-175">You can create and use data disks with scale sets.</span></span> <span data-ttu-id="9404b-176">V předchozích kurzu jste se dozvěděli, jak příliš[spravovat Azure disky](tutorial-manage-disks.md) , jsou podrobněji popsány dále hello osvědčené postupy a vylepšení výkonu pro vytváření aplikací na datové disky, nikoli disku hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="9404b-176">In a previous tutorial, you learned how too[Manage Azure disks](tutorial-manage-disks.md) that outlines hello best practices and performance improvements for building apps on data disks rather than hello OS disk.</span></span>

### <a name="create-scale-set-with-data-disks"></a><span data-ttu-id="9404b-177">Vytvořit sadu škálování s datovými disky</span><span class="sxs-lookup"><span data-stu-id="9404b-177">Create scale set with data disks</span></span>
<span data-ttu-id="9404b-178">toocreate škálování nastavte a připojte datových disků, přidejte hello `--data-disk-sizes-gb` parametr toohello [vytvořit az vmss](/cli/azure/vmss#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="9404b-178">toocreate a scale set and attach data disks, add hello `--data-disk-sizes-gb` parameter toohello [az vmss create](/cli/azure/vmss#create) command.</span></span> <span data-ttu-id="9404b-179">Hello následující příklad vytvoří škálování s *50*Gb datové disky připojené tooeach instance:</span><span class="sxs-lookup"><span data-stu-id="9404b-179">hello following example creates a scale set with *50*Gb data disks attached tooeach instance:</span></span>

```azurecli-interactive 
az vmss create \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetDisks \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --custom-data cloud-init.txt \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50
```

<span data-ttu-id="9404b-180">Při odebrání instance ze sady škálování, budou odebrány také všechny připojené datových disků.</span><span class="sxs-lookup"><span data-stu-id="9404b-180">When instances are removed from a scale set, any attached data disks are also removed.</span></span>

### <a name="add-data-disks"></a><span data-ttu-id="9404b-181">Přidat datových disků</span><span class="sxs-lookup"><span data-stu-id="9404b-181">Add data disks</span></span>
<span data-ttu-id="9404b-182">nastavení tooadd tooinstances disku data ve vašem měřítku, použijte [připojit az vmss disk](/cli/azure/vmss/disk#attach).</span><span class="sxs-lookup"><span data-stu-id="9404b-182">tooadd a data disk tooinstances in your scale set, use [az vmss disk attach](/cli/azure/vmss/disk#attach).</span></span> <span data-ttu-id="9404b-183">Hello následující příklad přidá *50*instance tooeach disku Gb:</span><span class="sxs-lookup"><span data-stu-id="9404b-183">hello following example adds a *50*Gb disk tooeach instance:</span></span>

```azurecli-interactive 
az vmss disk attach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --size-gb 50 \
    --lun 2
```

### <a name="detach-data-disks"></a><span data-ttu-id="9404b-184">Odpojit datových disků</span><span class="sxs-lookup"><span data-stu-id="9404b-184">Detach data disks</span></span>
<span data-ttu-id="9404b-185">nastavení tooremove tooinstances disku data ve vašem měřítku, použijte [odpojit az vmss disk](/cli/azure/vmss/disk#detach).</span><span class="sxs-lookup"><span data-stu-id="9404b-185">tooremove a data disk tooinstances in your scale set, use [az vmss disk detach](/cli/azure/vmss/disk#detach).</span></span> <span data-ttu-id="9404b-186">Hello následující příklad odebere hello datový disk na logické jednotce *2* z každé instance:</span><span class="sxs-lookup"><span data-stu-id="9404b-186">hello following example removes hello data disk at LUN *2* from each instance:</span></span>

```azurecli-interactive 
az vmss disk detach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --lun 2
```


## <a name="next-steps"></a><span data-ttu-id="9404b-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9404b-187">Next steps</span></span>
<span data-ttu-id="9404b-188">V tomto kurzu jste vytvořili škálovací sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9404b-188">In this tutorial, you created a virtual machine scale set.</span></span> <span data-ttu-id="9404b-189">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="9404b-189">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9404b-190">Použít cloudové init toocreate tooscale aplikaci</span><span class="sxs-lookup"><span data-stu-id="9404b-190">Use cloud-init toocreate an app tooscale</span></span>
> * <span data-ttu-id="9404b-191">Vytvoření sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="9404b-191">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="9404b-192">Zvýšení nebo snížení hello počet instancí v sadě škálování</span><span class="sxs-lookup"><span data-stu-id="9404b-192">Increase or decrease hello number of instances in a scale set</span></span>
> * <span data-ttu-id="9404b-193">Zobrazit informace o připojení pro instance škálovací sady</span><span class="sxs-lookup"><span data-stu-id="9404b-193">View connection info for scale set instances</span></span>
> * <span data-ttu-id="9404b-194">Použití datových disků ve škálovací sadě</span><span class="sxs-lookup"><span data-stu-id="9404b-194">Use data disks in a scale set</span></span>

<span data-ttu-id="9404b-195">Posunutí toohello další kurz toolearn informace o konceptech pro virtuální počítače Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="9404b-195">Advance toohello next tutorial toolearn more about load balancing concepts for virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="9404b-196">Vyrovnávat zatížení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="9404b-196">Load balance virtual machines</span></span>](tutorial-load-balancer.md)