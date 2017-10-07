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
# <a name="how-toocustomize-a-linux-virtual-machine-on-first-boot"></a>Jak toocustomize virtuální počítač s Linuxem na při prvním spuštění
V předchozích kurzu jste se naučili jak tooSSH tooa virtuální počítač (VM) a ručně nainstalujte NGINX. Obvykle se požaduje toocreate virtuální počítače rychlý a konzistentním způsobem, nějaký způsob automatizace. Běžné toocustomize přístup virtuální počítač na při prvním spuštění je toouse [cloudu init](https://cloudinit.readthedocs.io). V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvořte soubor konfigurace cloudu init
> * Vytvořit virtuální počítač, který používá soubor init cloudu
> * Zobrazit spuštěné aplikaci Node.js po hello je vytvořena virtuálních počítačů
> * Pomocí Key Vault toosecurely úložiště certifikátů
> * Automatizovat zabezpečená nasazení NGINX s inicializací cloudu


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).  



## <a name="cloud-init-overview"></a>Init cloudu – přehled
[Init cloudu](https://cloudinit.readthedocs.io) je často používaný přístup toocustomize virtuálního počítače s Linuxem jako spustí pro hello poprvé. Můžete používat cloudové init tooinstall balíčky a zapisovat soubory, nebo tooconfigure uživatele a zabezpečení. Init cloudu běží během počáteční spouštění hello, neexistují žádné další kroky nebo požadováno agenty tooapply konfiguraci.

Init cloudu také funguje v různých distribucí. Například nepoužívejte **výstižný get instalace** nebo **yum nainstalovat** tooinstall balíčku. Místo toho můžete definovat seznam tooinstall balíčky. Init cloudu automaticky používá nástroj pro správu hello nativní balíčku pro hello distro, kterou vyberete.

Snažíme se práce s našich partnerů tooget cloudu inicializací zahrnuté a práci v hello bitové kopie, aby umožňovala tooAzure. Hello následující tabulka popisuje hello aktuální dostupnosti cloudu init Image platformy Azure:

| Alias | Vydavatel | Nabídka | Skladová jednotka (SKU) | Verze |
|:--- |:--- |:--- |:--- |:--- |:--- |
| UbuntuLTS |Canonical |UbuntuServer |16.04 LTS |nejnovější |
| UbuntuLTS |Canonical |UbuntuServer |14.04.5-LTS |nejnovější |
| CoreOS |CoreOS |CoreOS |Stable |nejnovější |


## <a name="create-cloud-init-config-file"></a>Vytvoření cloudové init konfiguračního souboru
toosee cloudu inicializací v akci, vytvořte virtuální počítač, který nainstaluje NGINX a spustí jednoduchou aplikaci Node.js "Zdravím svět". Hello následující konfigurace cloudu init nainstaluje hello požadované balíčky, vytvoří aplikaci Node.js, pak inicializaci a spuštění aplikace hello.

V aktuálním prostředí, vytvořte soubor s názvem *cloudu init.txt* a hello vložte následující konfigurace. Můžete například vytvořte soubor hello v hello cloudové prostředí není na místním počítači. Můžete použít libovolný editor, které chcete. Zadejte `sensible-editor cloud-init.txt` toocreate hello souboru a zobrazit seznam dostupných editory. Ujistěte se, že tento soubor celou cloudu init hello správně zkopírován, zejména hello první řádek:

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

Další informace o možnostech konfigurace cloudu init najdete v tématu [příklady konfigurace cloudu init](https://cloudinit.readthedocs.io/en/latest/topics/examples.html).

## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače
Před vytvořením virtuálního počítače, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupAutomate* v hello *eastus* umístění:

```azurecli-interactive 
az group create --name myResourceGroupAutomate --location eastus
```

Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create). Použití hello `--custom-data` parametr toopass v souboru config init cloudu. Zadejte úplnou cestu toohello hello *cloudu init.txt* konfigurace, pokud jste uložili soubor hello mimo pracovní adresář existuje. Hello následující příklad vytvoří virtuální počítač s názvem *myAutomatedVM*:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

Jak dlouho trvá několik minut, než toobe hello virtuálního počítače vytvořena, hello balíčky tooinstall a toostart aplikace hello. Existují úlohy na pozadí, které pokračovat toorun po hello příkazového řádku Azure CLI vrátí, toohello řádku. To může být jiná několik minut před přístupem k aplikaci hello. Po vytvoření hello virtuálních počítačů, poznamenejte si hello `publicIpAddress` zobrazí hello rozhraní příkazového řádku Azure. Tato adresa se aplikace Node.js hello použité tooaccess prostřednictvím webového prohlížeče.

tooallow webový provoz tooreach virtuální počítač, otevřete port 80 z hello Internet s [az virtuálních počítačů open-port](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroupAutomate --name myVM
```

## <a name="test-web-app"></a>Test webové aplikace
Nyní můžete otevřít webový prohlížeč a zadejte *http://<publicIpAddress>*  v panelu Adresa hello. Zadejte vlastní veřejná IP adresa z hello virtuálního počítače vytvořit proces. Aplikace Node.js se zobrazí jako hello následující ukázka:

![Zobrazení spuštěných NGINX lokality](./media/tutorial-automate-vm-deployment/nginx.png)


## <a name="inject-certificates-from-key-vault"></a>Vložit certifikáty z Key Vault
Této volitelné části ukazuje, jak můžete bezpečně uložit certifikáty v Azure Key Vault a vložit je během hello nasazení virtuálního počítače. Místo použití vlastní image, která obsahuje certifikáty hello zaručená v, tento proces zajišťuje, že hello nejaktuálnější certifikáty jsou vložit tooa virtuální počítač při prvním spuštění počítače. Během procesu hello hello certifikát nikdy opustí hello platformy Azure nebo je vystaven ve skriptu, historie příkazového řádku nebo šablony.

Azure Key Vault chrání kryptografické klíče a tajné klíče, jako je například certifikáty nebo hesla. Key Vault pomáhá zjednodušit proces správy klíčů hello a umožní vám toomaintain kontrolu nad klíči, které k přístupu a šifrování dat. Tento scénář představuje některé koncepty toocreate Key Vault a použít certifikát, ale není vyčerpávající přehled o tom, toouse Key Vault.

Hello následující kroky ukazují, jak můžete:

- Vytvoření Azure Key Vault
- Generovat nebo nahrát certifikát toohello Key Vault
- Vytvoření tajného klíče z certifikátu tooinject hello v tooa virtuálních počítačů
- Vytvoření virtuálního počítače a vložit hello certifikátu

### <a name="create-an-azure-key-vault"></a>Vytvoření Azure Key Vault
Nejprve vytvořte Key Vault s [vytvořit az keyvault](/cli/azure/keyvault#create) a povolit pro použití při nasazení virtuálního počítače. Každý Key Vault vyžaduje jedinečný název a musí být všechny malá písmena. Nahraďte *mykeyvault* v hello následující příklad s svůj vlastní jedinečný název pro Key Vault:

```azurecli-interactive 
keyvault_name=mykeyvault
az keyvault create \
    --resource-group myResourceGroupAutomate \
    --name $keyvault_name \
    --enabled-for-deployment
```

### <a name="generate-certificate-and-store-in-key-vault"></a>Vygenerování certifikátu a uložit v Key Vault
Pro použití v provozním prostředí, měli byste importovat platný certifikát podepsaný službou důvěryhodného zprostředkovatele s [import certifikátu keyvault az](/cli/azure/keyvault/certificate#import). V tomto kurzu hello následující příklad ukazuje, jak můžete vygenerovat certifikát podepsaný svým držitelem s [vytvoření certifikátu keyvault az](/cli/azure/keyvault/certificate#create) používající zásady certifikátů výchozí hello:

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```


### <a name="prepare-certificate-for-use-with-vm"></a>Příprava certifikátu pro použití s virtuálním Počítačem.
certifikát hello toouse během hello virtuálního počítače vytvořit proces, získejte hello ID vašeho certifikátu s [az keyvault verze sdíleného tajného klíče seznam-](/cli/azure/keyvault/secret#list-versions). potřebuje certifikát hello Hello virtuálního počítače v určité formátu tooinject ho na spuštění, takže převést hello certifikát s [az virtuálních počítačů formát secret](/cli/azure/vm#format-secret). Následující ukázka Hello přiřadí výstup hello toovariables tyto příkazy pro snadné použití v hello další kroky:

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```


### <a name="create-cloud-init-config-toosecure-nginx"></a>Vytvoření cloudové init konfigurace toosecure NGINX
Když vytvoříte virtuální počítač, certifikáty a klíče jsou uložené v hello chráněné */var/lib/příkazwaagent/* adresáře. tooautomate přidání hello certifikát toohello virtuálního počítače a konfigurace NGINX, můžete použít aktualizované cloudu init konfigurace z předchozí příklad hello.

Vytvořte soubor s názvem *cloudu. init secured.txt* a hello vložte následující konfigurace. Znovu Pokud používáte hello cloudové prostředí, vytvořte hello cloudu init konfiguračního souboru tam a není na místním počítači. Použití `sensible-editor cloud-init-secured.txt` toocreate hello souboru a zobrazit seznam dostupných editory. Ujistěte se, že tento soubor celou cloudu init hello správně zkopírován, zejména hello první řádek:

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

### <a name="create-secure-vm"></a>Vytvoření zabezpečeného virtuálního počítače
Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create). data certifikátu Hello je vložili z Key Vault s hello `--secrets` parametr. Jako v předchozím příkladu hello také předáte v konfiguraci cloudu init hello s hello `--custom-data` parametr:

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

Jak dlouho trvá několik minut, než toobe hello virtuálního počítače vytvořena, hello balíčky tooinstall a toostart aplikace hello. Existují úlohy na pozadí, které pokračovat toorun po hello příkazového řádku Azure CLI vrátí, toohello řádku. To může být jiná několik minut před přístupem k aplikaci hello. Po vytvoření hello virtuálních počítačů, poznamenejte si hello `publicIpAddress` zobrazí hello rozhraní příkazového řádku Azure. Tato adresa se aplikace Node.js hello použité tooaccess prostřednictvím webového prohlížeče.

tooallow zabezpečit webové přenosy tooreach virtuální počítač, otevřete port 443 ze hello Internet s [az virtuálních počítačů open-port](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --port 443
```

### <a name="test-secure-web-app"></a>Testování zabezpečení webové aplikace
Nyní můžete otevřít webový prohlížeč a zadejte *https://<publicIpAddress>*  v panelu Adresa hello. Zadejte vlastní veřejná IP adresa z hello virtuálního počítače vytvořit proces. Přijměte upozornění zabezpečení hello, pokud se používá certifikát podepsaný svým držitelem:

![Přijetí upozornění zabezpečení webového prohlížeče](./media/tutorial-automate-vm-deployment/browser-warning.png)

Zabezpečené NGINX lokality a Node.js aplikace se potom zobrazí jako hello následující ukázka:

![Zobrazit spuštěná zabezpečené NGINX Web](./media/tutorial-automate-vm-deployment/secured-nginx.png)


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali virtuální počítače při prvním spuštění počítače s inicializací cloudu. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvořte soubor konfigurace cloudu init
> * Vytvořit virtuální počítač, který používá soubor init cloudu
> * Zobrazit spuštěné aplikaci Node.js po hello je vytvořena virtuálních počítačů
> * Pomocí Key Vault toosecurely úložiště certifikátů
> * Automatizovat zabezpečená nasazení NGINX s inicializací cloudu

Jak zálohy další kurz toolearn toohello toocreate vlastní Image virtuálních počítačů.

> [!div class="nextstepaction"]
> [Vytváření vlastních imagí virtuálních počítačů](./tutorial-custom-images.md)
