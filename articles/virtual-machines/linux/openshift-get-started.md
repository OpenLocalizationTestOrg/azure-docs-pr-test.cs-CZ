---
title: "aaaDeploy OpenShift původu tooAzure | Microsoft Docs"
description: "Přečtěte si toodeploy OpenShift původu tooAzure virtuálních počítačů."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: jbinder
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: jbinder
ms.openlocfilehash: a67450c46da41134a5f6c669a9e54e14773ac5b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-openshift-origin-tooazure-virtual-machines"></a>Nasazení OpenShift původu tooAzure virtuální počítače 

[Původ OpenShift](https://www.openshift.org/) je kontejner platforma s otevřeným zdrojem založený na [Kubernetes](https://kubernetes.io/). Zjednodušuje proces hello nasazení, škálování a provozování víceklientským aplikacím. 

Tato příručka popisuje, jak hello toodeploy OpenShift původ na virtuálních počítačích Azure pomocí rozhraní příkazového řádku Azure a šablon Azure Resource Manageru. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření klíčů SSH pro hello OpenShift cluster KeyVault toomanage.
> * Nasazení clusteru OpenShift na virtuálních počítačích Azure. 
> * Instalace a konfigurace hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello clusteru.
> * Přizpůsobte hello OpenShift nasazení.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

Tento úvodní vyžaduje hello Azure CLI verze 2.0.8 nebo novější. verze hello toofind, spusťte `az --version`. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure 
Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů nebo klikněte na tlačítko **vyzkoušet** toouse cloudové prostředí.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění.

```azurecli 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-key-vault"></a>Vytvoření trezoru klíčů
Vytvoření klíčů SSH pro hello cluster hello toostore KeyVault s hello [vytvořit az keyvault](/cli/azure/keyvault#create) příkaz.  

```azurecli 
az keyvault create --resource-group myResourceGroup --name myKeyVault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>Vytvoření klíče SSH 
Klíč SSH je potřebné toosecure přístup toohello OpenShift původní cluster. Vytvoření SSH dvojici klíčů pomocí hello `ssh-keygen` příkaz. 
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> heslo nesmí mít Hello pár klíčů SSH, které vytvoříte.

Další informace o klíče SSH v systému Windows [jak toocreate SSH klíčů v systému Windows](/azure/virtual-machines/linux/ssh-from-windows).

## <a name="store-ssh-private-key-in-key-vault"></a>Uložit privátní klíč SSH v Key Vault
Hello OpenShift nasazení používá jste vytvořili toosecure přístup toohello OpenShift hlavní klíč SSH hello. tooenable hello nasazení toosecurely načíst klíč SSH hello, uložení klíče hello v Key Vault pomocí hello následující příkaz.

# <a name="enabled-for-template-deployment"></a>Povolit pro nasazení šablony
```azurecli
az keyvault secret set --vault-name KeyVaultName --name OpenShiftKey --file ~/.ssh/openshift.rsa
```

## <a name="create-a-service-principal"></a>Vytvoření instančního objektu 
OpenShift komunikuje se službou Azure pomocí uživatelského jména a hesla nebo hlavní název služby. Objektu zabezpečení služby Azure je identita zabezpečení, která můžete použít s aplikací, služeb a automatizace nástroje, například OpenShift. Můžete řídit a definovat hello oprávnění objektu služby hello toowhat operace můžete provádět v rámci Azure. tooimprove zabezpečení přes právě poskytnutí uživatelského jména a hesla, tento příklad vytvoří základní služby hlavní.

Vytvoření služby hlavní s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) a výstup hello pověření, které potřebuje OpenShift:

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {strong password} \
          --scopes $(az group show --name myResourceGroup --query id)
```
Poznamenejte si vlastnosti appId hello vrácená z příkazu hello.
```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {strong password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > Nevytvářejte nezabezpečené heslo.  Postupujte podle pokynů v tématu [Pravidla a omezení pro hesla Azure AD](/azure/active-directory/active-directory-passwords-policy).

Další informace o objekty služby najdete v tématu [vytvořit objekt služby Azure pomocí Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

## <a name="deploy-hello-openshift-origin-template"></a>Nasazení hello OpenShift počátek šablony
Nasaďte další OpenShift počátek pomocí šablony Azure Resource Manager. 

> [!NOTE] 
> Hello následující příkaz vyžaduje az rozhraní příkazového řádku 2.0.8 nebo novější. Můžete ověřit hello az CLI verze s hello `az --version` příkaz. hello tooupdate verze rozhraní příkazového řádku najdete v části [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).

Použití hello `appId` hodnotu z objektu služby hello jste dříve vytvořili pro hello `aadClientId` parametr.

```azurecli 
az group deployment create --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --params \ 
        openshiftMasterPublicIpDnsLabel=myopenshiftmaster \
        infraLbPublicIpDnsLabel=myopenshiftlb \
        openshiftPassword=Pass@word!
        sshPublicKey=~/.ssh/openshift_rsa.pub \
        keyVaultResourceGroup=myResourceGroup \
        keyVaultName=myKeyVault \
        keyVaultSecret=OpenShiftKey \
        aadClientId={appId} \
        aadClientSecret={strong password} 
```
Hello nasazení může trvat až toocomplete too20 minut. Hello adresu URL konzoly OpenShift hello a název DNS hello OpenShift hlavní server je vytištěny toohello Terminálové po dokončení nasazení hello.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ocpadmin@myopenshiftmaster.cloudapp.azure.com"
}
```
## <a name="connect-toohello-openshift-cluster"></a>Připojte toohello OpenShift cluster
Po dokončení nasazení hello připojení konzole OpenShift toohello pomocí prohlížeče hello pomocí hello `OpenShift Console Uri`. Alternativně můžete připojit toohello OpenShift hlavní server pomocí hello následující příkaz.

```bash
$ ssh ocpadmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Vyčištění prostředků
Pokud již nepotřebujete, můžete použít hello [odstranění skupiny az](/cli/azure/group#delete) příkaz skupiny prostředků hello tooremove, OpenShift clusteru a všechny související prostředky.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tento kurz, zjištěné postup:
> [!div class="checklist"]
> * Vytvoření klíčů SSH pro hello OpenShift cluster KeyVault toomanage.
> * Nasazení clusteru OpenShift na virtuálních počítačích Azure. 
> * Instalace a konfigurace hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello clusteru.

Nyní je nasazený tento OpenShift původní cluster. Můžete postupovat podle OpenShift kurzy toolearn jak toodeploy vaší první aplikace a použití hello OpenShift nástroje. V tématu [Začínáme s OpenShift původu](https://docs.openshift.org/latest/getting_started/index.html) tooget spuštěna. 
