---
title: "Instanční objekt pro cluster Azure Kubernetes"
description: "Vytvoření a správa instančního objektu služby Azure Active Directory pro cluster Kubernetes v AKS"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: get-started-article
ms.date: 11/30/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 9814dca53f1a410f4d1e95cc18b98373f27f9802
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="service-principals-with-azure-container-service-aks"></a>Instanční objekty se službou Azure Container Service (AKS)

Cluster AKS vyžaduje [instanční objekt služby Azure Active Directory][aad-service-principal] pro interakci s rozhraními API Azure. Instanční objekt je potřeba k dynamické správě prostředků, jako jsou například [uživatelem definované trasy][user-defined-routes] a [vrstva 4 služby Azure Load Balancer][azure-load-balancer-overview].

Tento článek ukazuje různé možnosti nastavení instančního objektu pro cluster Kubernetes ve službě AKS.

## <a name="before-you-begin"></a>Než začnete


Abyste mohli vytvořit instanční objekt služby Azure AD, musíte mít oprávnění k registraci aplikace v tenantu Azure AD a přiřazení aplikace k roli v předplatném. Pokud nemáte potřebná oprávnění, možná budete muset požádat správce služby Azure AD nebo předplatného o jejich přiřazení nebo vytvořit instanční objekt pro cluster Kubernetes předem.

Také musíte mít nainstalované a nakonfigurované rozhraní Azure CLI verze 2.0.21 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI][install-azure-cli].

## <a name="create-sp-with-aks-cluster"></a>Vytvoření instančního objektu s clusterem AKS

Při nasazování clusteru AKS pomocí příkazu `az aks create` máte možnost automaticky vygenerovat instanční objekt.

V následujícím příkladu se vytvoří cluster AKS, a protože není zadaný existující instanční objekt, vytvoří se pro cluster nový instanční objekt. Pro dokončení této operace musí mít váš účet příslušná práva k vytvoření instančního objektu.

```azurecli
az aks create --name myK8SCluster --resource-group myResourceGroup --generate-ssh-keys
```

## <a name="use-an-existing-sp"></a>Použití existujícího instančního objektu

Pro použití s clusterem AKS je možné použít existující instanční objekt služby Azure AD nebo ho předem vytvořit. To je užitečné při nasazování clusteru z webu Azure Portal, kdy se vyžaduje zadání informací o instančním objektu. Při použití existujícího instančního objektu musí být tajný klíč klienta nakonfigurovaný jako heslo.

## <a name="pre-create-a-new-sp"></a>Vytvoření nového instančního objektu předem

Pokud chcete vytvořit instanční objekt pomocí Azure CLI, použijte příkaz [az ad sp create-for-rbac][az-ad-sp-create].

```azurecli
az ad sp create-for-rbac --skip-assignment
```

Výstup je podobný tomuto. Poznamenejte si hodnoty `appId` a `password`. Tyto hodnoty se používají při vytváření clusteru AKS.

```json
{
  "appId": "7248f250-0000-0000-0000-dbdeb8400d85",
  "displayName": "azure-cli-2017-10-15-02-20-15",
  "name": "http://azure-cli-2017-10-15-02-20-15",
  "password": "77851d2c-0000-0000-0000-cb3ebc97975a",
  "tenant": "72f988bf-0000-0000-0000-2d7cd011db47"
}
```

## <a name="use-an-existing-sp"></a>Použití existujícího instančního objektu

Pokud chcete použít předem vytvořený instanční objekt, zadejte `appId` a `password` jako hodnoty argumentů příkazu `az aks create`.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myK8SCluster --service-principal <appId> --client-secret <password>
```

Pokud nasazujete cluster AKS pomocí webu Azure Portal, zadejte ve formuláři konfigurace clusteru AKS hodnotu `appId` do pole **ID klienta instančního objektu** a hodnotu `password` do pole **Tajný kód klienta instančního objektu**.

![Obrázek přechodu na aplikaci Azure Vote](media/container-service-kubernetes-service-principal/sp-portal.png)

## <a name="additional-considerations"></a>Další aspekty

Při práci s instančními objekty služeb Azure AD a AKS mějte na paměti následující informace.

* Instanční objekt pro Kubernetes je součástí konfigurace clusteru. K nasazení clusteru ale nepoužívejte identitu.
* Každý instanční objekt je přidružený k aplikaci Azure AD. Instanční objekt pro cluster Kubernetes může být přidružený k jakémukoli platnému názvu aplikace Azure AD (například `https://www.contoso.org/example`). Adresa URL aplikace nemusí být skutečný koncový bod.
* Při zadávání **ID klienta** instančního objektu můžete použít hodnotu `appId` (jak je ukázáno v tomto článku) nebo odpovídající `name` instančního objektu (například `https://www.contoso.org/example`).
* Na hlavním virtuálním počítači a virtuálních počítačích uzlů v clusteru Kubernetes jsou pověření instančního objektu uložená v souboru `/etc/kubernetes/azure.json`.
* Pokud použijete příkaz `az aks create` k automatickému vygenerování instančního objektu, zapíší se přihlašovací údaje instančního objektu do souboru `~/.azure/acsServicePrincipal.json` na počítači, který jste ke spuštění příkazu použili.
* Pokud použijete příkaz `az aks create` k automatickému generování instančního objektu, může tento instanční objekt k ověřování také použít službu [Azure Container Registry][acr-intro] vytvořenou ve stejném předplatném.
* Při odstraňování clusteru AKS vytvořeného příkazem `az aks create` se instanční objekt, který se vytvořil automaticky, neodstraní. K jeho odstranění můžete použít příkaz `az ad sp delete --id $clientID`.

## <a name="next-steps"></a>Další kroky

Další informace o instančních objektech služby Azure Active Directory najdete v dokumentaci k aplikacím Azure AD.

> [!div class="nextstepaction"]
> [Aplikační a instanční objekty][service-principal]

<!-- LINKS - internal -->
[aad-service-principal]: ../active-directory/develop/active-directory-application-objects.md
[acr-intro]: ../container-registry/container-registry-intro.md
[az-ad-sp-create]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[azure-load-balancer-overview]: ../load-balancer/load-balancer-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[service-principal]: ../active-directory/develop/active-directory-application-objects.md
[user-defined-routes]: ../load-balancer/load-balancer-overview.md