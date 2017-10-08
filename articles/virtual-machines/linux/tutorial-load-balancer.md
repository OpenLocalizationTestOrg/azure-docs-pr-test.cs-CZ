---
title: "aaaHow tooload vyvážit virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello Azure zatížení vyrovnávání toocreate vysoká dostupnost a zabezpečení aplikací v rámci tři virtuální počítače s Linuxem"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: f01752c3caec3489ee13e63000775769f3236e11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooload-balance-linux-virtual-machines-in-azure-toocreate-a-highly-available-application"></a>Jak tooload vyvážit virtuální počítače s Linuxem v Azure toocreate vysoce dostupné aplikace
Vyrovnávání zatížení poskytuje vyšší úroveň dostupnosti rozloží příchozí žádosti napříč více virtuálních počítačů. V tomto kurzu informace o hello různé součásti nástroje pro vyrovnávání zatížení Azure hello které distribuci přenosů a zajištění vysoké dostupnosti. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvoření pro vyrovnávání zatížení Azure
> * Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.
> * Vytvoření pravidla pro provoz nástroj pro vyrovnávání zatížení
> * Použít cloudové init toocreate základní aplikaci Node.js
> * Vytváření virtuálních počítačů a připojte tooa nástroj pro vyrovnávání zatížení
> * Zobrazit nástroj pro vyrovnávání zatížení v akci
> * Přidání a odebrání virtuálních počítačů z pro vyrovnávání zatížení


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="azure-load-balancer-overview"></a>Přehled nástroje pro vyrovnávání zatížení Azure
K nástroji pro vyrovnávání zatížení Azure je Vyrovnávání zatížení vrstvy 4 (TCP, UDP), která poskytuje vysokou dostupnost distribucí příchozí provoz mezi virtuálními počítači v pořádku. Stav sondu nástroje pro vyrovnávání zatížení monitoruje zadaný port pro každý virtuální počítač a pouze distribuuje provoz tooan provozní virtuálních počítačů.

Můžete definovat na front-endové konfiguraci protokolu IP, která obsahuje jeden nebo více veřejné IP adresy. Tuto konfiguraci front-end IP adresy umožňuje vaší zatížení vyrovnávání a aplikace toobe přístupné přes hello Internet. 

Virtuální počítače připojit nástroj pro vyrovnávání zatížení tooa pomocí jejich virtuální síťová karta (NIC). toodistribute toohello přenosy virtuálních počítačů, fond back-end adres obsahuje hello IP adresy služby Vyrovnávání zatížení připojená toohello virtuální (NIC) hello.

toocontrol hello tok přenosů, můžete definovat pravidla nástroje pro vyrovnávání zatížení pro určité porty a protokoly, které mapují tooyour virtuálních počítačů.

Pokud jste postupovali podle předchozích kurzu hello příliš[vytvořit škálovací sadu virtuálních počítačů](tutorial-create-vmss.md), nástroj pro vyrovnávání zatížení pro vás vytvořil. Všechny tyto součásti byly nakonfigurovány pro vás v rámci sady škálování hello.


## <a name="create-azure-load-balancer"></a>Vytvořit nástroj pro vyrovnávání zatížení Azure
V této části podrobně popisuje, jak můžete vytvořit a nakonfigurovat jednotlivé komponenty služby Vyrovnávání zatížení hello. Než bude možné vytvořit nástroj pro vyrovnávání zatížení, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupLoadBalancer* v hello *eastus* umístění:

```azurecli-interactive 
az group create --name myResourceGroupLoadBalancer --location eastus
```

### <a name="create-a-public-ip-address"></a>Vytvoření veřejné IP adresy
tooaccess hello vaší aplikace na Internetu, musíte na veřejnou IP adresu pro nástroj pro vyrovnávání zatížení hello. Vytvoření veřejné IP adresy s [vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create). Hello následující příklad vytvoří veřejnou IP adresu s názvem *myPublicIP* v hello *myResourceGroupLoadBalancer* skupiny prostředků:

```azurecli-interactive 
az network public-ip create \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP
```

### <a name="create-a-load-balancer"></a>Vytvoření nástroje pro vyrovnávání zatížení
Vytvořit nástroj pro vyrovnávání zatížení s [az sítě lb vytvořit](/cli/azure/network/lb#create). Hello následující příklad vytvoří nástroj pro vyrovnávání zatížení s názvem *myLoadBalancer* a přiřadí hello *myPublicIP* konfiguraci front-end IP adresy toohello:

```azurecli-interactive 
az network lb create \
    --resource-group myResourceGroupLoadBalancer \
    --name myLoadBalancer \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --public-ip-address myPublicIP
```

### <a name="create-a-health-probe"></a>Vytvoření test stavu
tooallow hello stavu služby Vyrovnávání zatížení toomonitor hello vaší aplikace, použijte Test stavu. Test stavu Hello dynamicky přidá nebo odebere virtuálních počítačů z otočení nástroje pro vyrovnávání zatížení hello podle jejich kontroly toohealth odpovědi. Ve výchozím nastavení odeberou se virtuální počítač z distribuce nástroje pro vyrovnávání zatížení hello po dvě po sobě jdoucích selhání v intervalech 15 sekund. Můžete vytvořit test stavu na základě protokolu nebo na stránce Kontrola specifickém stavu pro vaši aplikaci. 

Hello následující ukázka vytvoří sondou TCP. Můžete také vytvořit vlastní sondy HTTP pro další kontroly podrobné stavu. Pokud používáte vlastní sondu HTTP, musíte vytvořit hello stránka pro kontrolu stavu, jako například *healthcheck.js*. Test Hello musí vrátit **HTTP 200 OK** odpovědi pro hello zatížení vyrovnávání tookeep hello hostitele v otočení.

toocreate sondou TCP stavu, můžete použít [vytvoření testu vyrovnáváním zatížení sítě az](/cli/azure/network/lb/probe#create). Hello následující příklad vytvoří sondu stavu s názvem *myHealthProbe*:

```azurecli-interactive 
az network lb probe create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-a-load-balancer-rule"></a>Vytvořit pravidlo Vyrovnávání zatížení.
Pravidlo Vyrovnávání zatížení je použité toodefine jak přenosy jsou distribuované toohello virtuálních počítačů. Můžete definovat hello front-endové konfiguraci protokolu IP pro příchozí provoz hello a hello back-end fondu tooreceive hello provozu IP, spolu s hello požadované zdrojový a cílový port. toomake se, že virtuální počítače pouze v pořádku přijímat přenosy, také definovat toouse test stavu hello.

Vytvořit pravidlo Vyrovnávání zatížení s [vytvořit pravidlo vyrovnáváním zatížení sítě az](/cli/azure/network/lb/rule#create). Hello následující příklad vytvoří pravidlo s názvem *myLoadBalancerRule*, používá hello *myHealthProbe* test stavu a zůstatky přenosy na portu *80*:

```azurecli-interactive 
az network lb rule create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myLoadBalancerRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe
```


## <a name="configure-virtual-network"></a>Konfigurace virtuální sítě
Před nasazením některé virtuální počítače a nástroj pro vyrovnávání můžete otestovat, vytvořte hello podpora prostředky virtuální sítě. Další informace o virtuálních sítích najdete v tématu hello [spravovat virtuální sítě Azure](tutorial-virtual-network.md) kurzu.

### <a name="create-network-resources"></a>Vytvoření síťové prostředky
Vytvoření virtuální sítě s [vytvoření sítě vnet az](/cli/azure/network/vnet#create). Hello následující příklad vytvoří virtuální síť s názvem *myVnet* s podsítí s názvem *mySubnet*:

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupLoadBalancer \
    --name myVnet \
    --subnet-name mySubnet
```

tooadd skupinu zabezpečení sítě, můžete použít [vytvořit az sítě nsg](/cli/azure/network/nsg#create). Hello následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:

```azurecli-interactive 
az network nsg create \
    --resource-group myResourceGroupLoadBalancer \
    --name myNetworkSecurityGroup
```

Vytvoření pravidla skupiny zabezpečení sítě s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create). Hello následující příklad vytvoří pravidlo skupiny zabezpečení sítě s názvem *myNetworkSecurityGroupRule*:

```azurecli-interactive 
az network nsg rule create \
    --resource-group myResourceGroupLoadBalancer \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --priority 1001 \
    --protocol tcp \
    --destination-port-range 80
```

Virtuální síťové adaptéry jsou vytvořeny pomocí [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create). Hello následující příklad vytvoří tři virtuálních síťových karet. (Jeden virtuální síťovou kartu pro každý virtuální počítač vytvoříte pro aplikace v rámci hello následující kroky). Můžete kdykoli vytvořit další virtuální síťové karty a virtuální počítače a přidat je nástroj pro vyrovnávání zatížení toohello:

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupLoadBalancer \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --network-security-group myNetworkSecurityGroup \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

## <a name="create-virtual-machines"></a>Vytváření virtuálních počítačů

### <a name="create-cloud-init-config"></a>Vytvoření konfigurace cloudu init
V předchozích kurzu na [jak toocustomize virtuální počítač s Linuxem na při prvním spuštění](tutorial-automate-vm-deployment.md), jste se naučili jak tooautomate přizpůsobení virtuálního počítače s inicializací cloudu. Můžete použít hello stejné cloudové init konfigurační soubor tooinstall NGINX a spusťte jednoduchou aplikaci Node.js "Hello, World".

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

### <a name="create-virtual-machines"></a>Vytváření virtuálních počítačů
tooimprove hello vysokou dostupnost vaší aplikace, umístit virtuální počítače v nastavení dostupnosti. Další informace o nastavení dostupnosti, najdete v části hello předchozí [jak toocreate vysoce dostupných virtuálních počítačů](tutorial-availability-sets.md) kurzu.

Vytvořit sadu s dostupnosti [az virtuálních počítačů sady dostupnosti. vytváření](/cli/azure/vm/availability-set#create). Hello následující příklad vytvoří sadu s názvem dostupnosti *myAvailabilitySet*:

```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupLoadBalancer \
    --name myAvailabilitySet
```

Nyní můžete vytvořit hello virtuálních počítačů s [vytvořit virtuální počítač az](/cli/azure/vm#create). Hello následující příklad vytvoří tři virtuální počítače a generuje klíče SSH, pokud už neexistují:

```bash
for i in `seq 1 3`; do
    az vm create \
        --resource-group myResourceGroupLoadBalancer \
        --name myVM$i \
        --availability-set myAvailabilitySet \
        --nics myNic$i \
        --image UbuntuLTS \
        --admin-username azureuser \
        --generate-ssh-keys \
        --custom-data cloud-init.txt \
        --no-wait
done
```

Existují úlohy na pozadí, které pokračovat toorun po hello příkazového řádku Azure CLI vrátí, toohello řádku. Hello `--no-wait` parametr nečeká pro všechny úlohy toocomplete hello. To může být jiná několik minut před přístupem k aplikaci hello. Hello test stavu nástroje pro vyrovnávání zatížení automaticky zjišťuje, když na každém virtuálním počítači běží aplikace hello. Jakmile hello aplikace běží, se spustí pravidlo Vyrovnávání zatížení hello toodistribute provoz.


## <a name="test-load-balancer"></a>Nástroj pro vyrovnávání zatížení testu
Získat hello veřejnou IP adresu nástroj pro vyrovnávání zatížení s [az sítě veřejné ip zobrazit](/cli/azure/network/public-ip#show). Hello následující příklad získá hello IP adresu pro *myPublicIP* vytvořili dříve:

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```

Potom můžete zadat hello veřejnou IP adresu ve webovém prohlížeči tooa. Mějte na paměti, – trvá několik minut hello hello virtuální počítače toobe připravené před spuštěním nástroje pro vyrovnávání zatížení hello toodistribute provoz toothem. Hello aplikace se zobrazí, včetně hello název hostitele virtuálního počítače hello tento nástroj pro vyrovnávání zatížení hello distribuované tooas provoz v hello následující ukázka:

![Spuštěné aplikace Node.js](./media/tutorial-load-balancer/running-nodejs-app.png)

Nástroj pro vyrovnávání zatížení toosee hello distribuuje provoz přes všechny tři virtuální počítače spuštěné aplikace, můžete můžete vynutit obnovení webového prohlížeče.


## <a name="add-and-remove-vms"></a>Přidání a odebrání virtuálních počítačů
Může být nutné tooperform údržby na hello virtuální počítače používající vaši aplikaci, například při instalaci aktualizace operačního systému. toodeal zvýšení provozu tooyour aplikace, může být nutné tooadd dalších virtuálních počítačů. V této části se dozvíte, jak tooremove nebo přidat virtuální počítač z nástroje pro vyrovnávání zatížení hello.

### <a name="remove-a-vm-from-hello-load-balancer"></a>Odebrat virtuální počítač z nástroje pro vyrovnávání zatížení hello
Virtuální počítač můžete odebrat z fondu adres back-end hello s [odebrat az sítě síťový adaptér ip-config fond adres](/cli/azure/network/nic/ip-config/address-pool#remove). Následující příklad odebere Hello hello virtuální síťovou kartu pro **Můjvp2** z *myLoadBalancer*:

```azurecli-interactive 
az network nic ip-config address-pool remove \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool 
```

Nástroj pro vyrovnávání zatížení toosee hello distribuuje provoz přes hello zbývající dva virtuální počítače používající vaši aplikaci je můžete vynutit obnovení webového prohlížeče. Nyní můžete provést údržbu na hello virtuálních počítačů, jako je instalace aktualizací operačního systému nebo provádění restartování virtuálního počítače.

### <a name="add-a-vm-toohello-load-balancer"></a>Přidání toohello virtuálního počítače, služby pro vyrovnávání zatížení
Po provedení údržby virtuálních počítačů, nebo pokud potřebujete tooexpand kapacitu, můžete přidat fond adres back-end toohello virtuálních počítačů s [az sítě síťový adaptér ip-config fond adres přidat](/cli/azure/network/nic/ip-config/address-pool#add). Hello následující příklad přidá hello virtuální síťovou kartu pro **Můjvp2** příliš*myLoadBalancer*:

```azurecli-interactive 
az network nic ip-config address-pool add \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool
```


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste vytvořili pro vyrovnávání zatížení a připojené tooit virtuálních počítačů. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření pro vyrovnávání zatížení Azure
> * Vytvoření stavu sondu nástroje pro vyrovnávání zatížení.
> * Vytvoření pravidla pro provoz nástroj pro vyrovnávání zatížení
> * Použít cloudové init toocreate základní aplikaci Node.js
> * Vytváření virtuálních počítačů a připojte tooa nástroj pro vyrovnávání zatížení
> * Zobrazit nástroj pro vyrovnávání zatížení v akci
> * Přidání a odebrání virtuálních počítačů z pro vyrovnávání zatížení

Posunutí toohello další kurz toolearn informace o komponentách virtuální síť Azure.

> [!div class="nextstepaction"]
> [Správa virtuálních počítačů a virtuálních sítí](tutorial-virtual-network.md)
