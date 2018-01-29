---
title: "Správě Azure Kubernetes clusteru pomocí webového uživatelského rozhraní"
description: "Pomocí řídicího panelu Kubernetes v AKS"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 11/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: ca828dab7bdb47e41596be2717598cfe828953ca
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="kubernetes-dashboard-with-azure-container-service-aks"></a>Řídicí panel Kubernetes s Azure Container Service (AKS)

Rozhraní příkazového řádku Azure lze spustit Kubernetes řídicího panelu. Tento dokument provede řídicím panelu Kubernetes počínaje rozhraní příkazového řádku Azure a také provede některých operací základní řídicího panelu. Další informace o řídicím panelu najdete Kubernetes [Kubernetes webového uživatelského rozhraní řídicí panel][kubernetes-dashboard].

## <a name="before-you-begin"></a>Než začnete

Podrobně popsané kroky v tomto dokumentu předpokládají, že jste vytvořili cluster AKS a navázali s ním připojení přes kubectl. Pokud budete potřebovat tyto položky zobrazit, [rychlý start AKS][aks-quickstart].

Také musíte mít nainstalované a nakonfigurované rozhraní Azure CLI verze 2.0.21 nebo novější. Pokud chcete zjistit verzi, spusťte příkaz az --version. Pokud je potřeba nainstalovat nebo upgradovat najdete v tématu [nainstalovat rozhraní příkazového řádku Azure][install-azure-cli].

## <a name="start-kubernetes-dashboard"></a>Spustit Kubernetes řídicí panel

Použití `az aks browse` příkaz ke spuštění Kubernetes řídicího panelu. Při spuštění tohoto příkazu, nahraďte název skupiny a cluster prostředků.

```azurecli
az aks browse --resource-group myResourceGroup --name myK8SCluster
```

Tento příkaz vytvoří server proxy mezi váš vývojový systém a rozhraní API Kubernetes a otevře se webový prohlížeč na řídicím panelu Kubernetes.

## <a name="run-an-application"></a>Spuštění aplikace

Na řídicím panelu Kubernetes klikněte na tlačítko **vytvořit** tlačítko v horním pravém okně. Zadejte název nasazení `nginx` a zadejte `nginx:latest` pro název bitové kopie. V části **služby**, vyberte **externí** a zadejte `80` pro port a cílový port.

Až budete připravení, klikněte na tlačítko **nasadit** vytvořte nasazení.

![Dialogové okno Vytvořit Kubernetes služby](./media/container-service-kubernetes-ui/create-deployment.png)

## <a name="view-the-application"></a>Zobrazení aplikace

Stav aplikace můžete zobrazit na řídicím panelu Kubernetes. Jakmile je aplikace spuštěna, každá komponenta má zelená zaškrtávací políčko vedle sebe.

![Kubernetes pracovními stanicemi soustředěnými kolem](./media/container-service-kubernetes-ui/complete-deployment.png)

Chcete-li zobrazit další informace o pracovními stanicemi soustředěnými kolem aplikace, klikněte na **pracovními stanicemi soustředěnými kolem** v levé nabídce a potom vyberte **NGINX** pod. Zde se zobrazí pod konkrétní informace, jako je spotřeby prostředků.

![Kubernetes prostředky](./media/container-service-kubernetes-ui/running-pods.png)

Chcete-li najít IP adresu aplikace, klikněte na **služby** v levé nabídce a potom vyberte **NGINX** služby.

![nginx zobrazení](./media/container-service-kubernetes-ui/nginx-service.png)

## <a name="edit-the-application"></a>Upravte aplikace

Kromě vytváření a aplikace pro zobrazení, řídicí panel Kubernetes slouží k upravte a aktualizujte nasazení aplikací.

Chcete-li upravit nasazení, klikněte na tlačítko **nasazení** v levé nabídce a potom vyberte **NGINX** nasazení. Nakonec vyberte **upravit** v pravém horním navigačním panelu.

![Upravit Kubernetes](./media/container-service-kubernetes-ui/view-deployment.png)

Vyhledejte `spec.replica` tuto hodnotu změnit hodnotu, která by měla být 1, 3. Při tom repliky nasazení NGINX zvýší se od 1 do 3.

Vyberte **aktualizace** Jakmile budete připraveni.

![Upravit Kubernetes](./media/container-service-kubernetes-ui/edit-deployment.png)

## <a name="next-steps"></a>Další kroky

Další informace o řídicím panelu Kubernetes najdete v dokumentaci k Kubernetes.

> [!div class="nextstepaction"]
> [Řídicí panel Kubernetes webového uživatelského rozhraní][kubernetes-dashboard]

<!-- LINKS - external -->
[kubernetes-dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[install-azure-cli]: /cli/azure/install-azure-cli