---
title: "Ověření pomocí Azure kontejneru registru z instancí Azure kontejneru"
description: "Zjistěte, jak poskytnout přístup k bitové kopie v registru vaší privátní kontejneru z instancí kontejneru Azure pomocí Azure Active Directory instanční objekt."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 01/24/2018
ms.author: marsma
ms.openlocfilehash: 00d9632a5d0c42eceee1b412f8963bbadbea651f
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/25/2018
---
# <a name="authenticate-with-azure-container-registry-from-azure-container-instances"></a>Ověření pomocí Azure kontejneru registru z instancí Azure kontejneru

Poskytnutí přístupu k vaší privátní kontejneru registrech v registru kontejner Azure můžete použít objektu služby Azure Active Directory (Azure AD).

V tomto článku se dozvíte, vytvořit a nakonfigurovat hlavní název služby Azure AD s *vyžádání* oprávnění k registru. Potom spusťte kontejner v Azure kontejner instancí (ACI) který žádá o jeho image z vaší privátní registru pro ověřování pomocí objektu služby.

## <a name="when-to-use-a-service-principal"></a>Kdy použít objekt služby

Objekt služby byste měli použít pro ověřování z ACI v **bezobslužných scénáře**, například aplikace nebo služby, které vytvoří kontejner instancí bezobslužné způsobem automatizované nebo jinak.

Například, pokud máte automatizované skript, který spouští každou noc a vytváří [instance založené na úlohách kontejneru](../container-instances/container-instances-restart-policy.md) zpracovat některá data, může použít objekt služby s vyžádanou (čtenáři) oprávnění k ověření do registru. Potom můžete otočit instanční objekt pověření nebo odvolat přístup zcela bez ovlivnění dalších služeb a aplikací.

Objekty služby by měl být také použít, když registru [uživatel s oprávněními správce](container-registry-authentication.md#admin-account) je zakázána.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="authenticate-using-the-service-principal"></a>Ověřování pomocí objektu služby

Spusťte kontejner v Azure kontejner instancí pomocí hlavní název služby, zadejte jeho ID pro `--registry-username`a jeho heslo pro `--registry-password`.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerregistry.azurecr.io/myimage:v1 \
    --registry-login-server mycontainerregistry.azurecr.io \
    --registry-username <service-principal-ID> \
    --registry-password <service-principal-password>
```

## <a name="next-steps"></a>Další postup

V následujících článcích obsahují další podrobnosti o práci s objekty služby a ACR:

* [Azure kontejneru registru ověřování s objekty služby](container-registry-auth-service-principal.md)
* [Ověření pomocí Azure kontejneru registru z Azure Container Service (AKS)](container-registry-auth-aks.md)

<!-- IMAGES -->

<!-- LINKS - External -->

<!-- LINKS - Internal -->
