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
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-linux"></a>Vytvořit sadu škálování virtuálního počítače a nasazení vysoce dostupné aplikace v systému Linux
Škálovací sadu virtuálních počítačů vám umožní toodeploy a spravovat sadu identické, automatické škálování virtuálních počítačů. Můžete škálovat hello počet virtuálních počítačů v sadě škálování hello ručně nebo definovat tooautoscale pravidla na základě využití procesoru, paměti vyžádání nebo síťový provoz. V tomto kurzu nasadíte škálování virtuálních počítačů, nastavte v Azure. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Použít cloudové init toocreate tooscale aplikaci
> * Vytvoření sady škálování virtuálního počítače
> * Zvýšení nebo snížení hello počet instancí v sadě škálování
> * Zobrazit informace o připojení pro instance škálovací sady
> * Použití datových disků ve škálovací sadě


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="scale-set-overview"></a>Přehled sady škálování
Škálovací sadu virtuálních počítačů vám umožní toodeploy a spravovat sadu identické, automatické škálování virtuálních počítačů. Škálování nastaví hello použijte stejné komponenty jako Seznámili jste se v předchozí kurzu hello příliš[vytvořit vysoce dostupné virtuální počítače](tutorial-availability-sets.md). Virtuální počítače ve škálovací sadě jsou vytvořeny ve skupině dostupnosti nastavena a distribuovaných napříč doménami selhání a aktualizace logiku.

Podle potřeby ve škálovací sadě virtuálních počítačů vytvořeny. Můžete definovat pravidla toocontrol škálování jak a kdy se přidají nebo odeberou z hello škálovací sadu virtuálních počítačů. Tato pravidla můžete spustit na základě metriky například zatížení procesoru, využití paměti nebo síťový provoz.

Škálování nastaví podporu až too1 000 virtuálních počítačů, pokud používáte image platformy Azure. Pro úlohy v produkčním prostředí může příliš chcete[vytvořit vlastní image virtuálního počítače](tutorial-custom-images.md). Můžete vytvořit až too100 virtuálních počítačů v škálování nastavit při použití vlastní image.


## <a name="create-an-app-tooscale"></a>Vytvoření tooscale aplikaci
Pro použití v provozním prostředí, můžete příliš[vytvořit vlastní image virtuálního počítače](tutorial-custom-images.md) , který obsahuje aplikace nainstalována a nakonfigurována. V tomto kurzu umožňuje přizpůsobit hello virtuální počítače na první spouštěcí tooquickly najdete v části nastavení v akci škálování.

V předchozích kurzu jste se dozvěděli [jak toocustomize virtuální počítač s Linuxem na při prvním spuštění](tutorial-automate-vm-deployment.md) s inicializací cloudu. Můžete použít hello stejné cloudové init konfigurační soubor tooinstall NGINX a spusťte jednoduchou aplikaci Node.js "Hello, World". 

V aktuálním prostředí, vytvořte soubor s názvem *cloudu init.txt* a hello vložte následující konfigurace. Můžete například vytvořte soubor hello v hello cloudové prostředí není na místním počítači. Zadejte `sensible-editor cloud-init.txt` toocreate hello souboru a zobrazit seznam dostupných editory. Ujistěte se, že tento soubor celou cloudu init hello správně zkopírován, zejména hello první řádek:

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


## <a name="create-a-scale-set"></a>Vytvoření sady škálování
Před vytvořením sady škálování, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupScaleSet* v hello *eastus* umístění:

```azurecli-interactive 
az group create --name myResourceGroupScaleSet --location eastus
```

Nyní vytvoří škálování virtuálního počítače s [vytvořit az vmss](/cli/azure/vmss#create). Hello následující příklad vytvoří škálování nastavení s názvem *myScaleSet*, používá hello cloudu init souboru toocustomize hello virtuálních počítačů a generuje klíče SSH, pokud neexistují:

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

Přebírá toocreate pár minut a konfigurace všech hello škálování sadu prostředků a virtuální počítače. Existují úlohy na pozadí, které pokračovat toorun po hello příkazového řádku Azure CLI vrátí, toohello řádku. To může být jiná několik minut před přístupem k aplikaci hello.


## <a name="allow-web-traffic"></a>Povolit webový provoz
Nástroj pro vyrovnávání zatížení se vytvoří automaticky v rámci sady škálování virtuálního počítače hello. Nástroj pro vyrovnávání zatížení Hello rozděluje zatížení mezi sadu definované virtuálních počítačů pomocí pravidla nástroje pro vyrovnávání zatížení. Další informace o konceptech služby Vyrovnávání zatížení a konfiguraci v dalším kurzu hello, [jak tooload vyvážit virtuálních počítačů v Azure](tutorial-load-balancer.md).

tooallow provoz tooreach hello webové aplikace, vytvořte pravidlo s [vytvořit pravidlo vyrovnáváním zatížení sítě az](/cli/azure/network/lb/rule#create). Hello následující příklad vytvoří pravidlo s názvem *myLoadBalancerRuleWeb*:

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

## <a name="test-your-app"></a>Testování aplikace
toosee aplikace Node.js na webu hello získat hello veřejnou IP adresu nástroj pro vyrovnávání zatížení s [az sítě veřejné ip zobrazit](/cli/azure/network/public-ip#show). Hello následující příklad získá hello IP adresu pro *myScaleSetLBPublicIP* vytvořen jako součást sady škálování hello:

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetLBPublicIP \
    --query [ipAddress] \
    --output tsv
```

Zadejte ve webovém prohlížeči tooa hello veřejnou IP adresu. Zobrazí se aplikace Hello, včetně hello název hostitele virtuálních počítačů této hello načíst vyrovnávání distribuované provoz hello:

![Spuštěné aplikace Node.js](./media/tutorial-create-vmss/running-nodejs-app.png)

sad v akci škálování hello toosee, můžete můžete vynutit obnovení webové prohlížeče toosee hello zatížení vyrovnávání provoz distribuovat do všech virtuálních počítačů hello používající vaši aplikaci.


## <a name="management-tasks"></a>Úlohy správy
V průběhu cyklu hello škálovací sady hello, může být nutné toorun jedné nebo více úloh správy. Kromě toho můžete toocreate skripty, které automatizují různé úlohy životního cyklu. Hello 2.0 rozhraní příkazového řádku Azure poskytuje rychlý způsob toodo tyto úlohy. Tady jsou několik běžných úloh.

### <a name="view-vms-in-a-scale-set"></a>Zobrazení virtuální počítače ve škálovací sadě
nastavit seznam virtuálních počítačů spuštěných ve vašem škálování tooview, použijte [az vmss seznamu instance](/cli/azure/vmss#list-instances) následujícím způsobem:

```azurecli-interactive 
az vmss list-instances \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --output table
```

Hello výstup je podobné toohello následující ukázka:

```azurecli-interactive 
  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup            VmId
------------  --------------------  ----------  ------------  -------------------  -----------------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUPSCALESET  c72ddc34-6c41-4a53-b89e-dd24f27b30ab
           3  True                  eastus      myScaleSet_3  Succeeded            MYRESOURCEGROUPSCALESET  44266022-65c3-49c5-92dd-88ffa64f95da
```


### <a name="increase-or-decrease-vm-instances"></a>Zvýšení nebo snížení instance virtuálních počítačů
toosee hello počet instancí aktuálně v škálování nastavíte, použijte [az vmss zobrazit](/cli/azure/vmss#show) a dotazovat se na *sku.capacity*:

```azurecli-interactive 
az vmss show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

Potom můžete ručně zvýšit nebo snížit hello počet virtuálních počítačů v hello škálování s [az vmss škálování](/cli/azure/vmss#scale). Hello následující příklad nastaví hello počet virtuálních počítačů ve vaší příliš sad škálování*5*:

```azurecli-interactive 
az vmss scale \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --new-capacity 5
```

Škálování pravidla umožňují definovat, jak nastavit tooscale nahoru nebo dolů hello počet virtuálních počítačů ve vaší škálování v odpovědi toodemand například síťový provoz nebo využití procesoru. Tato pravidla v současné době nelze nastavit v Azure CLI 2.0. Použití hello [portál Azure](https://portal.azure.com) tooconfigure škálování.

### <a name="get-connection-info"></a>Získání informací o připojení
informace o připojení tooobtain o hello v škálovací sady virtuálních počítačů, použijte [az vmss seznamu--připojení-informace o instanci](/cli/azure/vmss#list-instance-connection-info). Tento příkaz vypíše hello veřejnou IP adresu a port pro každý virtuální počítač, který vám umožní tooconnect pomocí protokolu SSH:

```azurecli-interactive 
az vmss list-instance-connection-info \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet
```


## <a name="use-data-disks-with-scale-sets"></a>Použití datových disků s sady škálování
Můžete vytvořit a používat datových disků s sady škálování. V předchozích kurzu jste se dozvěděli, jak příliš[spravovat Azure disky](tutorial-manage-disks.md) , jsou podrobněji popsány dále hello osvědčené postupy a vylepšení výkonu pro vytváření aplikací na datové disky, nikoli disku hello operačního systému.

### <a name="create-scale-set-with-data-disks"></a>Vytvořit sadu škálování s datovými disky
toocreate škálování nastavte a připojte datových disků, přidejte hello `--data-disk-sizes-gb` parametr toohello [vytvořit az vmss](/cli/azure/vmss#create) příkaz. Hello následující příklad vytvoří škálování s *50*Gb datové disky připojené tooeach instance:

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

Při odebrání instance ze sady škálování, budou odebrány také všechny připojené datových disků.

### <a name="add-data-disks"></a>Přidat datových disků
nastavení tooadd tooinstances disku data ve vašem měřítku, použijte [připojit az vmss disk](/cli/azure/vmss/disk#attach). Hello následující příklad přidá *50*instance tooeach disku Gb:

```azurecli-interactive 
az vmss disk attach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --size-gb 50 \
    --lun 2
```

### <a name="detach-data-disks"></a>Odpojit datových disků
nastavení tooremove tooinstances disku data ve vašem měřítku, použijte [odpojit az vmss disk](/cli/azure/vmss/disk#detach). Hello následující příklad odebere hello datový disk na logické jednotce *2* z každé instance:

```azurecli-interactive 
az vmss disk detach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --lun 2
```


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste vytvořili škálovací sadu virtuálních počítačů. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Použít cloudové init toocreate tooscale aplikaci
> * Vytvoření sady škálování virtuálního počítače
> * Zvýšení nebo snížení hello počet instancí v sadě škálování
> * Zobrazit informace o připojení pro instance škálovací sady
> * Použití datových disků ve škálovací sadě

Posunutí toohello další kurz toolearn informace o konceptech pro virtuální počítače Vyrovnávání zatížení.

> [!div class="nextstepaction"]
> [Vyrovnávat zatížení virtuálních počítačů](tutorial-load-balancer.md)