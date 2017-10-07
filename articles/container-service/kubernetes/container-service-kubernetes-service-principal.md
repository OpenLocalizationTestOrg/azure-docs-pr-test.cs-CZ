---
title: "objekt zabezpečení aaaService pro cluster Azure Kubernetes | Microsoft Docs"
description: "Vytvoření a správa instančního objektu služby Azure Active Directory pro cluster Kubernetes v Azure Container Service"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 7a01624c5ac3fa717dbcbd570e05ceb4d917c53a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-an-azure-ad-service-principal-for-a-kubernetes-cluster-in-container-service"></a>Nastavení instančního objektu služby Azure AD pro cluster Kubernetes ve službě Container Service


V Azure Container Service vyžaduje Kubernetes cluster [objektu služby Azure Active Directory](../../active-directory/develop/active-directory-application-objects.md) toointeract s rozhraními API Azure. Hello instanční objekt je potřeba toodynamically spravovat prostředky, jako [trasy definované uživatelem](../../virtual-network/virtual-networks-udr-overview.md) a hello [pro vyrovnávání zatížení vrstvy 4 Azure](../../load-balancer/load-balancer-overview.md). 


Tento článek popisuje různé možnosti tooset až služba hlavní Kubernetes clusteru. Například, pokud jste nainstalovali ale nastavení hello [Azure CLI 2.0](/cli/azure/install-az-cli2), můžete spustit hello [ `az acs create` ](/cli/azure/acs#create) příkaz toocreate hello Kubernetes clusteru a hello instančního objektu v hello stejnou dobu.


## <a name="requirements-for-hello-service-principal"></a>Požadavky pro hello instančního objektu

Můžete použít existující Azure AD instanční objekt zda splňuje hello následující požadavky, nebo vytvořte novou.

* **Obor**: hello skupiny prostředků v předplatném hello použít toodeploy hello Kubernetes clusteru nebo (méně restriktivně) hello předplatné použít toodeploy hello clusteru.

* **Role:****Přispěvatel**

* **Tajný kód klienta:** Musí se jednat o heslo. V současné době nemůžete použít instanční objekt nastavený pro ověření certifikátu.

> [!IMPORTANT] 
> toocreate instančního objektu, musíte mít oprávnění tooregister aplikace pomocí klienta Azure AD a tooassign hello aplikace tooa role v rámci vašeho předplatného. toosee, pokud máte hello požadované oprávnění [změnami hello portál](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions). 
>

## <a name="option-1-create-a-service-principal-in-azure-ad"></a>Možnost 1: Vytvoření instančního objektu v Azure AD

Pokud před nasazením clusteru Kubernetes chcete toocreate objektu služby Azure AD, Azure poskytuje několik metod. 

Následující příklady příkazů Hello ukazují, jak toodo o hello [Azure CLI 2.0](../../azure-resource-manager/resource-group-authenticate-service-principal-cli.md). Případně můžete vytvořit službu objektu zabezpečení pomocí [prostředí Azure PowerShell](../../azure-resource-manager/resource-group-authenticate-service-principal.md), hello [portál](../../azure-resource-manager/resource-group-create-service-principal-portal.md), nebo jiné metody.

```azurecli
az login

az account set --subscription "mySubscriptionID"

az group create -n "myResourceGroupName" -l "westus"

az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/mySubscriptionID/resourceGroups/myResourceGroupName"
```

Výstup se podobně jako následující toohello (tady zobrazené zredigované):

![Vytvoření instančního objektu](./media/container-service-kubernetes-service-principal/service-principal-creds.png)

Zvýrazněná jsou hello **ID klienta** (`appId`) a hello **tajný klíč klienta** (`password`), použít jako parametry hlavní služby pro nasazení clusteru.


### <a name="specify-service-principal-when-creating-hello-kubernetes-cluster"></a>Při vytváření clusteru Kubernetes hello zadat instančního objektu

Zadejte hello **ID klienta** (také nazývané hello `appId`, pro ID aplikace) a **tajný klíč klienta** (`password`) z existující službu hlavní jako parametry při vytváření hello Kubernetes clusteru. Ujistěte se, že hello instanční objekt splňuje požadavky hello v hello od tohoto článku.

Tyto parametry můžete zadat při nasazování clusteru Kubernetes hello pomocí hello [rozhraní příkazového řádku Azure (CLI) 2.0](container-service-kubernetes-walkthrough.md), [portál Azure](../dcos-swarm/container-service-deployment.md), nebo jiné metody.

>[!TIP] 
>Při zadávání hello **ID klienta**, se, zda text hello toouse `appId`, není hello `ObjectId`, z hello instanční objekt.
>

Hello následující příklad ukazuje jeden ze způsobů toopass hello parametry s hello 2.0 rozhraní příkazového řádku Azure. Tento příklad používá hello [šablony rychlý start Kubernetes](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes).

1. [Stáhněte si](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json) soubor parametrů šablony hello `azuredeploy.parameters.json` z Githubu.

2. Služba hello toospecify hlavní, zadejte hodnoty pro `servicePrincipalClientId` a `servicePrincipalClientSecret` v souboru hello. (Je také nutné tooprovide vlastních hodnot pro `dnsNamePrefix` a `sshRSAPublicKey`. Hello druhé je hello SSH veřejného klíče tooaccess hello cluster). Uložte soubor hello.

    ![Předání parametrů instančního objektu](./media/container-service-kubernetes-service-principal/service-principal-params.png)

3. Hello spusťte následující příkaz, pomocí `--parameters` tooset hello cestě toohello azuredeploy.parameters.json souboru. Tento příkaz nasadí hello clusteru ve skupině prostředků vytvoříte volané `myResourceGroup` v oblasti západní USA hello.

    ```azurecli
    az login

    az account set --subscription "mySubscriptionID"

    az group create --name "myResourceGroup" --location "westus" 
    
    az group deployment create -g "myResourceGroup" --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.json" --parameters @azuredeploy.parameters.json
    ```


## <a name="option-2-generate-a-service-principal-when-creating-hello-cluster-with-az-acs-create"></a>Možnost 2: Vygenerování hlavní název služby, při vytváření clusteru hello s`az acs create`

Pokud spustíte hello [ `az acs create` ](/cli/azure/acs#create) toocreate příkaz hello Kubernetes clusteru, máte možnost toogenerate hello objekt služby automaticky.

Stejně jako u ostatních možností vytvoření clusteru Kubernetes můžete při spuštění příkazu `az acs create` určit parametry pro existující instanční objekt. Ale pokud vynecháte tyto parametry, hello rozhraní příkazového řádku Azure vytvoří automaticky pro použití s Container Service. To probíhá transparentně během nasazení hello. 

Hello následující příkaz vytvoří Kubernetes cluster a vygeneruje klíčů SSH a pověření hlavní služby:

```console
az acs create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys --orchestrator-type kubernetes
```

> [!IMPORTANT]
> Pokud váš účet nemá hello Azure AD a předplatné oprávnění toocreate hlavní název služby, hello příkaz vygeneruje chybu podobné příliš`Insufficient privileges toocomplete hello operation.`
> 

## <a name="additional-considerations"></a>Další aspekty

* Pokud nemáte oprávnění toocreate objekt služby v rámci vašeho předplatného, bude pravděpodobně nutné tooask služby Azure AD nebo předplatné správce tooassign hello potřebná oprávnění, nebo požádejte je o hlavní toouse s Azure Container Service služby. 

* Hello instanční objekt pro Kubernetes je součástí konfigurace clusteru hello. Nepoužívejte však hello identity toodeploy hello clusteru.

* Každý instanční objekt je přidružený k aplikaci Azure AD. Hello objekt služby pro cluster s podporou Kubernetes mohou být přidruženy žádné platné Azure AD název aplikace (například: `https://www.contoso.org/example`). Hello adresa URL pro aplikaci hello nemá toobe skutečné koncový bod.

* Při určování hello instanční objekt **ID klienta**, můžete použít hodnotu hello hello `appId` (jak je uvedeno v tomto článku) nebo hello odpovídající instanční objekt `name` (například`https://www.contoso.org/example`).

* Na hlavní server hello a agenta virtuálních počítačů v clusteru Kubernetes hello hlavní přihlašovací údaje služby hello ukládají v souboru /etc/kubernetes/azure.json hello.

* Při použití hello `az acs create` příkaz toogenerate hello instanční objekt automaticky, hlavní přihlašovací údaje služby hello se zapisují toohello ~/.azure/acsServicePrincipal.json soubor na počítači hello používá příkaz toorun hello. 

* Při použití hello `az acs create` příkaz toogenerate hello instanční objekt automaticky, hello instanční objekt lze také ověřovat pomocí [kontejner Azure registru](../../container-registry/container-registry-intro.md) vytvořené v hello stejné předplatné.




## <a name="next-steps"></a>Další kroky

* [Začněte používat Kubernetes](container-service-kubernetes-walkthrough.md) v clusteru služby kontejneru.

* tootroubleshoot hello instanční objekt pro Kubernetes, najdete v části hello [ACS modul dokumentaci](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes.md#troubleshooting).


