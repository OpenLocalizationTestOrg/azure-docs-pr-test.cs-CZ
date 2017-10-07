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
# <a name="secure-a-web-server-with-ssl-certificates-on-a-linux-virtual-machine-in-azure"></a>Zabezpečení webového serveru pomocí certifikátů SSL na virtuální počítač s Linuxem v Azure
toosecure webových serverů, může být certifikát později SSL (Secure Sockets) používá tooencrypt webový provoz. Tyto certifikáty SSL můžou být uložená v Azure Key Vault a povolit zabezpečená nasazení certifikátů tooLinux virtuálních počítačů (VM) v Azure. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření Azure Key Vault
> * Generovat nebo nahrát certifikát toohello Key Vault
> * Vytvoření virtuálního počítače a instalaci hello NGINX webového serveru
> * Vložit hello certifikát do hello virtuálního počítače a konfigurace NGINX s vazbou SSL

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).  


## <a name="overview"></a>Přehled
Azure Key Vault chrání kryptografické klíče a tajné klíče, tyto certifikáty a hesla. Key Vault pomáhá zjednodušit proces správy certifikátů hello a umožní vám toomaintain kontrolu nad klíči, které přístup těchto certifikátů. Můžete vytvořit certifikát podepsaný svým držitelem v Key Vault nebo nahrát certifikát existující, důvěryhodné, který už vlastníte.

Místo použití vlastní image virtuálního počítače, který zahrnuje certifikáty zaručená v, vložit certifikáty do spuštěného virtuálního počítače. Tento proces zajišťuje instalaci hello nejaktuálnější certifikáty na webovém serveru během nasazení. Je-li obnovit nebo nahradit certifikát, také nemáte toocreate novou vlastní bitovou kopii virtuálního počítače. Hello nejnovější certifikáty jsou automaticky vložit, jako je vytváření dalších virtuálních počítačů. Během celého procesu hello nikdy certifikáty hello nechte hello platformy Azure nebo jsou viditelné ve skriptu, historie příkazového řádku nebo šablony.


## <a name="create-an-azure-key-vault"></a>Vytvoření Azure Key Vault
Před vytvořením Key Vault a certifikáty, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupSecureWeb* v hello *eastus* umístění:

```azurecli-interactive 
az group create --name myResourceGroupSecureWeb --location eastus
```

Dále vytvořte Key Vault s [vytvořit az keyvault](/cli/azure/keyvault#create) a povolit pro použití při nasazení virtuálního počítače. Každý Key Vault vyžaduje jedinečný název a musí být všechny malá písmena. Nahraďte  *<mykeyvault>*  v hello následující příklad s svůj vlastní jedinečný název pro Key Vault:

```azurecli-interactive 
keyvault_name=<mykeyvault>
az keyvault create \
    --resource-group myResourceGroupSecureWeb \
    --name $keyvault_name \
    --enabled-for-deployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Vygenerování certifikátu a uložit v Key Vault
Pro použití v provozním prostředí, měli byste importovat platný certifikát podepsaný službou důvěryhodného zprostředkovatele s [import certifikátu keyvault az](/cli/azure/certificate#import). V tomto kurzu hello následující příklad ukazuje, jak můžete vygenerovat certifikát podepsaný svým držitelem s [vytvoření certifikátu keyvault az](/cli/azure/certificate#create) používající zásady certifikátů výchozí hello:

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```

### <a name="prepare-a-certificate-for-use-with-a-vm"></a>Příprava certifikát pro použití s virtuálního počítače
certifikát hello toouse během hello virtuálního počítače vytvořit proces, získejte hello ID vašeho certifikátu s [az keyvault verze sdíleného tajného klíče seznam-](/cli/azure/keyvault/secret#list-versions). Převést hello certifikát s [az virtuálních počítačů formát secret](/cli/azure/vm#format-secret). Následující ukázka Hello přiřadí výstup hello toovariables tyto příkazy pro snadné použití v hello další kroky:

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```

### <a name="create-a-cloud-init-config-toosecure-nginx"></a>Vytvoření toosecure konfigurace cloudu init NGINX
[Init cloudu](https://cloudinit.readthedocs.io) je často používaný přístup toocustomize virtuálního počítače s Linuxem jako spustí pro hello poprvé. Můžete používat cloudové init tooinstall balíčky a zapisovat soubory, nebo tooconfigure uživatele a zabezpečení. Init cloudu běží během počáteční spouštění hello, neexistují žádné další kroky nebo požadováno agenty tooapply konfiguraci.

Když vytvoříte virtuální počítač, certifikáty a klíče jsou uložené v hello chráněné */var/lib/příkazwaagent/* adresáře. Přidání tooautomate hello certifikát toohello virtuálního počítače a konfigurace hello webový server, použijte cloudu init. V tomto příkladu jsme nainstalujte a nakonfigurujte hello NGINX webový server. Můžete použít hello stejné zpracovat tooinstall a nakonfigurujte Apache. 

Vytvořte soubor s názvem *cloudu init webové server.txt* a hello vložte následující konfigurace:

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

### <a name="create-a-secure-vm"></a>Vytvoření zabezpečeného virtuálního počítače
Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create). data certifikátu Hello je vložili z Key Vault s hello `--secrets` parametr. V konfiguraci cloudu init hello s hello předáte `--custom-data` parametr:

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

Jak dlouho trvá několik minut, než toobe hello virtuálního počítače vytvořena, hello balíčky tooinstall a toostart aplikace hello. Po vytvoření hello virtuálních počítačů, poznamenejte si hello `publicIpAddress` zobrazí hello rozhraní příkazového řádku Azure. Tato adresa je použité tooaccess váš web ve webovém prohlížeči.

tooallow zabezpečit webové přenosy tooreach virtuální počítač, otevřete port 443 ze hello Internet s [az virtuálních počítačů open-port](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --port 443
```


### <a name="test-hello-secure-web-app"></a>Testování hello zabezpečení webové aplikace
Nyní můžete otevřít webový prohlížeč a zadejte *https://<publicIpAddress>*  v panelu Adresa hello. Zadejte vlastní veřejná IP adresa z hello virtuálního počítače vytvořit proces. Přijměte upozornění zabezpečení hello, pokud se používá certifikát podepsaný svým držitelem:

![Přijetí upozornění zabezpečení webového prohlížeče](./media/tutorial-secure-web-server/browser-warning.png)

Zabezpečené NGINX webu se následně zobrazí jako hello následující ukázka:

![Zobrazit spuštěná zabezpečené NGINX Web](./media/tutorial-secure-web-server/secured-nginx.png)


## <a name="next-steps"></a>Další kroky

V tomto kurzu webového serveru se službou NGINX zabezpečená certifikátem SSL uložené v Azure Key Vault. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření Azure Key Vault
> * Generovat nebo nahrát certifikát toohello Key Vault
> * Vytvoření virtuálního počítače a instalaci hello NGINX webového serveru
> * Vložit hello certifikát do hello virtuálního počítače a konfigurace NGINX s vazbou SSL

Použijte tento odkaz toosee předem vytvořené ukázky skript virtuálního počítače.

> [!div class="nextstepaction"]
> [Ukázky skriptu Windows virtuálního počítače](./cli-samples.md)