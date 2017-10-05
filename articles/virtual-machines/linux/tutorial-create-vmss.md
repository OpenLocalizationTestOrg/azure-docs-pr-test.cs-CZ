---
title: "Vytvoření sady škálování virtuálního počítače pro Linux v Azure | Microsoft Docs"
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
ms.openlocfilehash: 2b8d519e11f70eda164bd8f6e131a3989f242ab0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-linux"></a><span data-ttu-id="ad444-103">Vytvořit sadu škálování virtuálního počítače a nasazení vysoce dostupné aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="ad444-103">Create a Virtual Machine Scale Set and deploy a highly available app on Linux</span></span>
<span data-ttu-id="ad444-104">Škálovací sadu virtuálních počítačů můžete nasadit a spravovat sadu identické, automatické škálování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ad444-104">A virtual machine scale set allows you to deploy and manage a set of identical, auto-scaling virtual machines.</span></span> <span data-ttu-id="ad444-105">Můžete škálovat počet virtuálních počítačů v sadě škálování ručně, nebo definovat pravidla pro automatické škálování podle využití procesoru, paměti vyžádání nebo síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="ad444-105">You can scale the number of VMs in the scale set manually, or define rules to autoscale based on CPU usage, memory demand, or network traffic.</span></span> <span data-ttu-id="ad444-106">V tomto kurzu nasadíte škálování virtuálních počítačů, nastavte v Azure.</span><span class="sxs-lookup"><span data-stu-id="ad444-106">In this tutorial, you deploy a virtual machine scale set in Azure.</span></span> <span data-ttu-id="ad444-107">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="ad444-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ad444-108">Použít k vytvoření aplikace škálovat init cloudu</span><span class="sxs-lookup"><span data-stu-id="ad444-108">Use cloud-init to create an app to scale</span></span>
> * <span data-ttu-id="ad444-109">Vytvoření sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="ad444-109">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="ad444-110">Zvýšení nebo snížení počtu instancí v sadě škálování</span><span class="sxs-lookup"><span data-stu-id="ad444-110">Increase or decrease the number of instances in a scale set</span></span>
> * <span data-ttu-id="ad444-111">Zobrazit informace o připojení pro instance škálovací sady</span><span class="sxs-lookup"><span data-stu-id="ad444-111">View connection info for scale set instances</span></span>
> * <span data-ttu-id="ad444-112">Použití datových disků ve škálovací sadě</span><span class="sxs-lookup"><span data-stu-id="ad444-112">Use data disks in a scale set</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ad444-113">Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ad444-113">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="ad444-114">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="ad444-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="ad444-115">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ad444-115">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="scale-set-overview"></a><span data-ttu-id="ad444-116">Přehled sady škálování</span><span class="sxs-lookup"><span data-stu-id="ad444-116">Scale Set overview</span></span>
<span data-ttu-id="ad444-117">Škálovací sadu virtuálních počítačů můžete nasadit a spravovat sadu identické, automatické škálování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ad444-117">A virtual machine scale set allows you to deploy and manage a set of identical, auto-scaling virtual machines.</span></span> <span data-ttu-id="ad444-118">Sady škálování použijte stejné součásti, jako jste se naučili o v předchozí kurzu, který [vytvořit vysoce dostupné virtuální počítače](tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="ad444-118">Scale sets use the same components as you learned about in the previous tutorial to [Create highly available VMs](tutorial-availability-sets.md).</span></span> <span data-ttu-id="ad444-119">Virtuální počítače ve škálovací sadě jsou vytvořeny ve skupině dostupnosti nastavena a distribuovaných napříč doménami selhání a aktualizace logiku.</span><span class="sxs-lookup"><span data-stu-id="ad444-119">VMs in a scale set are created in an availability set and distributed across logic fault and update domains.</span></span>

<span data-ttu-id="ad444-120">Podle potřeby ve škálovací sadě virtuálních počítačů vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="ad444-120">VMs are created as needed in a scale set.</span></span> <span data-ttu-id="ad444-121">Můžete definovat pravidla škálování řídit, jak a kdy se přidají nebo odeberou ze sady škálování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ad444-121">You define autoscale rules to control how and when VMs are added or removed from the scale set.</span></span> <span data-ttu-id="ad444-122">Tato pravidla můžete spustit na základě metriky například zatížení procesoru, využití paměti nebo síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="ad444-122">These rules can trigger based on metrics such as CPU load, memory usage, or network traffic.</span></span>

<span data-ttu-id="ad444-123">Pokud používáte image platformy Azure sadách škálování podporu až 1 000 virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ad444-123">Scale sets support up to 1,000 VMs when you use an Azure platform image.</span></span> <span data-ttu-id="ad444-124">Pro úlohy v produkčním prostředí, budete pravděpodobně chtít [vytvořit vlastní image virtuálního počítače](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="ad444-124">For production workloads, you may wish to [Create a custom VM image](tutorial-custom-images.md).</span></span> <span data-ttu-id="ad444-125">Můžete vytvořit až 100 virtuálními počítači v škálování nastavit při použití vlastní image.</span><span class="sxs-lookup"><span data-stu-id="ad444-125">You can create up to 100 VMs in a scale set when using a custom image.</span></span>


## <a name="create-an-app-to-scale"></a><span data-ttu-id="ad444-126">Vytvoření aplikace pro škálování</span><span class="sxs-lookup"><span data-stu-id="ad444-126">Create an app to scale</span></span>
<span data-ttu-id="ad444-127">Pro použití v provozním prostředí, můžete chtít [vytvořit vlastní image virtuálního počítače](tutorial-custom-images.md) , který obsahuje aplikace nainstalována a nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="ad444-127">For production use, you may wish to [Create a custom VM image](tutorial-custom-images.md) that includes your application installed and configured.</span></span> <span data-ttu-id="ad444-128">V tomto kurzu umožňuje přizpůsobit virtuální počítače na při prvním spuštění rychle zobrazit nastavení v akci škálování.</span><span class="sxs-lookup"><span data-stu-id="ad444-128">For this tutorial, lets customize the VMs on first boot to quickly see a scale set in action.</span></span>

<span data-ttu-id="ad444-129">V předchozích kurzu jste se dozvěděli [postup přizpůsobení virtuální počítač s Linuxem na při prvním spuštění](tutorial-automate-vm-deployment.md) s inicializací cloudu.</span><span class="sxs-lookup"><span data-stu-id="ad444-129">In a previous tutorial, you learned [How to customize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md) with cloud-init.</span></span> <span data-ttu-id="ad444-130">Konfiguračního souboru stejné cloudové init můžete použít k instalaci NGINX a spuštění jednoduchou aplikaci Node.js "Zdravím svět".</span><span class="sxs-lookup"><span data-stu-id="ad444-130">You can use the same cloud-init configuration file to install NGINX and run a simple 'Hello World' Node.js app.</span></span> 

<span data-ttu-id="ad444-131">V aktuálním prostředí, vytvořte soubor s názvem *cloudu init.txt* a vložte následující konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ad444-131">In your current shell, create a file named *cloud-init.txt* and paste the following configuration.</span></span> <span data-ttu-id="ad444-132">Například vytvoření souboru v prostředí cloudu není na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="ad444-132">For example, create the file in the Cloud Shell not on your local machine.</span></span> <span data-ttu-id="ad444-133">Zadejte `sensible-editor cloud-init.txt` k vytvoření tohoto souboru a zobrazit seznam dostupných editory.</span><span class="sxs-lookup"><span data-stu-id="ad444-133">Enter `sensible-editor cloud-init.txt` to create the file and see a list of available editors.</span></span> <span data-ttu-id="ad444-134">Ujistěte se, že je soubor celou cloudu init zkopírován správně, obzvláště první řádek:</span><span class="sxs-lookup"><span data-stu-id="ad444-134">Make sure that the whole cloud-init file is copied correctly, especially the first line:</span></span>

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


## <a name="create-a-scale-set"></a><span data-ttu-id="ad444-135">Vytvoření sady škálování</span><span class="sxs-lookup"><span data-stu-id="ad444-135">Create a scale set</span></span>
<span data-ttu-id="ad444-136">Před vytvořením sady škálování, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="ad444-136">Before you can create a scale set, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="ad444-137">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupScaleSet* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="ad444-137">The following example creates a resource group named *myResourceGroupScaleSet* in the *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupScaleSet --location eastus
```

<span data-ttu-id="ad444-138">Nyní vytvoří škálování virtuálního počítače s [vytvořit az vmss](/cli/azure/vmss#create).</span><span class="sxs-lookup"><span data-stu-id="ad444-138">Now create a virtual machine scale set with [az vmss create](/cli/azure/vmss#create).</span></span> <span data-ttu-id="ad444-139">Následující příklad vytvoří škálování nastavení s názvem *myScaleSet*, použije cloudu init soubor pro přizpůsobení virtuálního počítače a generuje klíče SSH, pokud neexistují:</span><span class="sxs-lookup"><span data-stu-id="ad444-139">The following example creates a scale set named *myScaleSet*, uses the cloud-init file to customize the VM, and generates SSH keys if they do not exist:</span></span>

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

<span data-ttu-id="ad444-140">Trvá několik minut vytvořit a nakonfigurovat všechny škálovací sadu prostředků a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ad444-140">It takes a few minutes to create and configure all the scale set resources and VMs.</span></span> <span data-ttu-id="ad444-141">Existují úlohy na pozadí, které dál běžet po rozhraní příkazového řádku Azure se vrátíte do řádku.</span><span class="sxs-lookup"><span data-stu-id="ad444-141">There are background tasks that continue to run after the Azure CLI returns you to the prompt.</span></span> <span data-ttu-id="ad444-142">To může být jiná několik minut, než může aplikaci používat.</span><span class="sxs-lookup"><span data-stu-id="ad444-142">It may be another couple of minutes before you can access the app.</span></span>


## <a name="allow-web-traffic"></a><span data-ttu-id="ad444-143">Povolit webový provoz</span><span class="sxs-lookup"><span data-stu-id="ad444-143">Allow web traffic</span></span>
<span data-ttu-id="ad444-144">Nástroj pro vyrovnávání zatížení byl vytvořen automaticky jako součást sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ad444-144">A load balancer was created automatically as part of the virtual machine scale set.</span></span> <span data-ttu-id="ad444-145">Nástroje pro vyrovnávání zatížení rozděluje zatížení mezi sadu definované virtuálních počítačů pomocí pravidla nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="ad444-145">The load balancer distributes traffic across a set of defined VMs using load balancer rules.</span></span> <span data-ttu-id="ad444-146">Další informace o konceptech služby Vyrovnávání zatížení a konfiguraci v dalším kurzu [postup vyrovnávat zatížení virtuálních počítačů v Azure](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="ad444-146">You can learn more about load balancer concepts and configuration in the next tutorial, [How to load balance virtual machines in Azure](tutorial-load-balancer.md).</span></span>

<span data-ttu-id="ad444-147">Pokud chcete povolit přenosy k dosažení webové aplikace, vytvořte pravidlo s [vytvořit pravidlo vyrovnáváním zatížení sítě az](/cli/azure/network/lb/rule#create).</span><span class="sxs-lookup"><span data-stu-id="ad444-147">To allow traffic to reach the web app, create a rule with [az network lb rule create](/cli/azure/network/lb/rule#create).</span></span> <span data-ttu-id="ad444-148">Následující příklad vytvoří pravidlo s názvem *myLoadBalancerRuleWeb*:</span><span class="sxs-lookup"><span data-stu-id="ad444-148">The following example creates a rule named *myLoadBalancerRuleWeb*:</span></span>

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

## <a name="test-your-app"></a><span data-ttu-id="ad444-149">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="ad444-149">Test your app</span></span>
<span data-ttu-id="ad444-150">Chcete-li aplikace Node.js najdete na webu získat veřejnou IP adresu nástroj pro vyrovnávání zatížení s [az sítě veřejné ip zobrazit](/cli/azure/network/public-ip#show).</span><span class="sxs-lookup"><span data-stu-id="ad444-150">To see your Node.js app on the web, obtain the public IP address of your load balancer with [az network public-ip show](/cli/azure/network/public-ip#show).</span></span> <span data-ttu-id="ad444-151">Následující příklad, získá IP adresu pro *myScaleSetLBPublicIP* vytvořené jako součást sady škálování:</span><span class="sxs-lookup"><span data-stu-id="ad444-151">The following example obtains the IP address for *myScaleSetLBPublicIP* created as part of the scale set:</span></span>

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetLBPublicIP \
    --query [ipAddress] \
    --output tsv
```

<span data-ttu-id="ad444-152">Zadejte veřejnou IP adresu ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ad444-152">Enter the public IP address in to a web browser.</span></span> <span data-ttu-id="ad444-153">Aplikace se zobrazí, včetně názvu hostitele virtuálního počítače, který nástroje pro vyrovnávání zatížení distribuován provoz:</span><span class="sxs-lookup"><span data-stu-id="ad444-153">The app is displayed, including the hostname of the VM that the load balancer distributed traffic to:</span></span>

![Spuštěné aplikace Node.js](./media/tutorial-create-vmss/running-nodejs-app.png)

<span data-ttu-id="ad444-155">Sad v akci škálování najdete můžete můžete vynutit aktualizujte webový prohlížeč zobrazit nástroje pro vyrovnávání zatížení provoz distribuovat mezi všechny virtuální počítače používající vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ad444-155">To see the scale set in action, you can force-refresh your web browser to see the load balancer distribute traffic across all the VMs running your app.</span></span>


## <a name="management-tasks"></a><span data-ttu-id="ad444-156">Úlohy správy</span><span class="sxs-lookup"><span data-stu-id="ad444-156">Management tasks</span></span>
<span data-ttu-id="ad444-157">V průběhu cyklu škálovací sady můžete spustit jeden nebo více úloh správy.</span><span class="sxs-lookup"><span data-stu-id="ad444-157">Throughout the lifecycle of the scale set, you may need to run one or more management tasks.</span></span> <span data-ttu-id="ad444-158">Kromě toho můžete vytvořit skripty, které automatizují různé úlohy životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="ad444-158">Additionally, you may want to create scripts that automate various lifecycle-tasks.</span></span> <span data-ttu-id="ad444-159">Azure CLI 2.0 poskytuje rychlý způsob, jak provést tyto úlohy.</span><span class="sxs-lookup"><span data-stu-id="ad444-159">The Azure CLI 2.0 provides a quick way to do those tasks.</span></span> <span data-ttu-id="ad444-160">Tady jsou několik běžných úloh.</span><span class="sxs-lookup"><span data-stu-id="ad444-160">Here are a few common tasks.</span></span>

### <a name="view-vms-in-a-scale-set"></a><span data-ttu-id="ad444-161">Zobrazení virtuální počítače ve škálovací sadě</span><span class="sxs-lookup"><span data-stu-id="ad444-161">View VMs in a scale set</span></span>
<span data-ttu-id="ad444-162">Chcete-li zobrazit seznam virtuálních počítačů spuštěných ve škálovací sadě, použijte [az vmss seznamu instance](/cli/azure/vmss#list-instances) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ad444-162">To view a list of VMs running in your scale set, use [az vmss list-instances](/cli/azure/vmss#list-instances) as follows:</span></span>

```azurecli-interactive 
az vmss list-instances \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --output table
```

<span data-ttu-id="ad444-163">Výstup se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="ad444-163">The output is similar to the following example:</span></span>

```azurecli-interactive 
  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup            VmId
------------  --------------------  ----------  ------------  -------------------  -----------------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUPSCALESET  c72ddc34-6c41-4a53-b89e-dd24f27b30ab
           3  True                  eastus      myScaleSet_3  Succeeded            MYRESOURCEGROUPSCALESET  44266022-65c3-49c5-92dd-88ffa64f95da
```


### <a name="increase-or-decrease-vm-instances"></a><span data-ttu-id="ad444-164">Zvýšení nebo snížení instance virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="ad444-164">Increase or decrease VM instances</span></span>
<span data-ttu-id="ad444-165">Pokud chcete zobrazit počet instancí, které máte aktuálně v sadě škálování, použijte [az vmss zobrazit](/cli/azure/vmss#show) a dotazovat se na *sku.capacity*:</span><span class="sxs-lookup"><span data-stu-id="ad444-165">To see the number of instances you currently have in a scale set, use [az vmss show](/cli/azure/vmss#show) and query on *sku.capacity*:</span></span>

```azurecli-interactive 
az vmss show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

<span data-ttu-id="ad444-166">Potom můžete ručně zvýšení nebo snížení počtu virtuálních počítačů v sad s škálování [az vmss škálování](/cli/azure/vmss#scale).</span><span class="sxs-lookup"><span data-stu-id="ad444-166">You can then manually increase or decrease the number of virtual machines in the scale set with [az vmss scale](/cli/azure/vmss#scale).</span></span> <span data-ttu-id="ad444-167">Následující příklad nastaví počet virtuálních počítačů ve vaší škálování nastavena na *5*:</span><span class="sxs-lookup"><span data-stu-id="ad444-167">The following example sets the number of VMs in your scale set to *5*:</span></span>

```azurecli-interactive 
az vmss scale \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --new-capacity 5
```

<span data-ttu-id="ad444-168">Škálování pravidla umožňují definovat, jak se škálovat nahoru nebo dolů počet virtuálních počítačů ve vaší škálování nastavit v reakci na poptávku například síťový provoz nebo využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="ad444-168">Autoscale rules let you define how to scale up or down the number of VMs in your scale set in response to demand such as network traffic or CPU usage.</span></span> <span data-ttu-id="ad444-169">Tato pravidla v současné době nelze nastavit v Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="ad444-169">Currently, these rules cannot be set in Azure CLI 2.0.</span></span> <span data-ttu-id="ad444-170">Použití [portál Azure](https://portal.azure.com) ke konfiguraci automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="ad444-170">Use the [Azure portal](https://portal.azure.com) to configure autoscale.</span></span>

### <a name="get-connection-info"></a><span data-ttu-id="ad444-171">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="ad444-171">Get connection info</span></span>
<span data-ttu-id="ad444-172">Chcete-li získat informace o připojení o virtuální počítače ve škálovací sady, použijte [az vmss seznamu--připojení-informace o instanci](/cli/azure/vmss#list-instance-connection-info).</span><span class="sxs-lookup"><span data-stu-id="ad444-172">To obtain connection information about the VMs in your scale sets, use [az vmss list-instance-connection-info](/cli/azure/vmss#list-instance-connection-info).</span></span> <span data-ttu-id="ad444-173">Tento příkaz vypíše veřejnou IP adresu a port pro každý virtuální počítač, který umožňuje připojit pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="ad444-173">This command outputs the public IP address and port for each VM that allows you to connect with SSH:</span></span>

```azurecli-interactive 
az vmss list-instance-connection-info \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet
```


## <a name="use-data-disks-with-scale-sets"></a><span data-ttu-id="ad444-174">Použití datových disků s sady škálování</span><span class="sxs-lookup"><span data-stu-id="ad444-174">Use data disks with scale sets</span></span>
<span data-ttu-id="ad444-175">Můžete vytvořit a používat datových disků s sady škálování.</span><span class="sxs-lookup"><span data-stu-id="ad444-175">You can create and use data disks with scale sets.</span></span> <span data-ttu-id="ad444-176">V předchozích kurzu jste se dozvěděli, jak [disky spravovat Azure](tutorial-manage-disks.md) , popisuje osvědčené postupy a vylepšení výkonu pro vytváření aplikací na datové disky, nikoli disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="ad444-176">In a previous tutorial, you learned how to [Manage Azure disks](tutorial-manage-disks.md) that outlines the best practices and performance improvements for building apps on data disks rather than the OS disk.</span></span>

### <a name="create-scale-set-with-data-disks"></a><span data-ttu-id="ad444-177">Vytvořit sadu škálování s datovými disky</span><span class="sxs-lookup"><span data-stu-id="ad444-177">Create scale set with data disks</span></span>
<span data-ttu-id="ad444-178">Chcete-li vytvořit sadu škálování a připojte datových disků, přidejte `--data-disk-sizes-gb` parametru [az vmss vytvořit](/cli/azure/vmss#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="ad444-178">To create a scale set and attach data disks, add the `--data-disk-sizes-gb` parameter to the [az vmss create](/cli/azure/vmss#create) command.</span></span> <span data-ttu-id="ad444-179">Následující příklad vytvoří škálování s *50*Gb dat disků připojených ke každé instanci:</span><span class="sxs-lookup"><span data-stu-id="ad444-179">The following example creates a scale set with *50*Gb data disks attached to each instance:</span></span>

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

<span data-ttu-id="ad444-180">Při odebrání instance ze sady škálování, budou odebrány také všechny připojené datových disků.</span><span class="sxs-lookup"><span data-stu-id="ad444-180">When instances are removed from a scale set, any attached data disks are also removed.</span></span>

### <a name="add-data-disks"></a><span data-ttu-id="ad444-181">Přidat datových disků</span><span class="sxs-lookup"><span data-stu-id="ad444-181">Add data disks</span></span>
<span data-ttu-id="ad444-182">Chcete-li přidat datový disk k instancím ve škálovací sadě, použijte [připojit az vmss disk](/cli/azure/vmss/disk#attach).</span><span class="sxs-lookup"><span data-stu-id="ad444-182">To add a data disk to instances in your scale set, use [az vmss disk attach](/cli/azure/vmss/disk#attach).</span></span> <span data-ttu-id="ad444-183">Následující příklad přidá *50*GB místa na disku k jednotlivým instancím:</span><span class="sxs-lookup"><span data-stu-id="ad444-183">The following example adds a *50*Gb disk to each instance:</span></span>

```azurecli-interactive 
az vmss disk attach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --size-gb 50 \
    --lun 2
```

### <a name="detach-data-disks"></a><span data-ttu-id="ad444-184">Odpojit datových disků</span><span class="sxs-lookup"><span data-stu-id="ad444-184">Detach data disks</span></span>
<span data-ttu-id="ad444-185">Chcete-li odebrat datový disk k instancím ve škálovací sadě, použijte [odpojit az vmss disk](/cli/azure/vmss/disk#detach).</span><span class="sxs-lookup"><span data-stu-id="ad444-185">To remove a data disk to instances in your scale set, use [az vmss disk detach](/cli/azure/vmss/disk#detach).</span></span> <span data-ttu-id="ad444-186">Následující příklad odebere datový disk na logické jednotce *2* z každé instance:</span><span class="sxs-lookup"><span data-stu-id="ad444-186">The following example removes the data disk at LUN *2* from each instance:</span></span>

```azurecli-interactive 
az vmss disk detach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --lun 2
```


## <a name="next-steps"></a><span data-ttu-id="ad444-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ad444-187">Next steps</span></span>
<span data-ttu-id="ad444-188">V tomto kurzu jste vytvořili škálovací sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ad444-188">In this tutorial, you created a virtual machine scale set.</span></span> <span data-ttu-id="ad444-189">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="ad444-189">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ad444-190">Použít k vytvoření aplikace škálovat init cloudu</span><span class="sxs-lookup"><span data-stu-id="ad444-190">Use cloud-init to create an app to scale</span></span>
> * <span data-ttu-id="ad444-191">Vytvoření sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="ad444-191">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="ad444-192">Zvýšení nebo snížení počtu instancí v sadě škálování</span><span class="sxs-lookup"><span data-stu-id="ad444-192">Increase or decrease the number of instances in a scale set</span></span>
> * <span data-ttu-id="ad444-193">Zobrazit informace o připojení pro instance škálovací sady</span><span class="sxs-lookup"><span data-stu-id="ad444-193">View connection info for scale set instances</span></span>
> * <span data-ttu-id="ad444-194">Použití datových disků ve škálovací sadě</span><span class="sxs-lookup"><span data-stu-id="ad444-194">Use data disks in a scale set</span></span>

<span data-ttu-id="ad444-195">Přechodu na v dalším kurzu se dozvíte další informace o konceptech pro virtuální počítače Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="ad444-195">Advance to the next tutorial to learn more about load balancing concepts for virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ad444-196">Vyrovnávat zatížení virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="ad444-196">Load balance virtual machines</span></span>](tutorial-load-balancer.md)