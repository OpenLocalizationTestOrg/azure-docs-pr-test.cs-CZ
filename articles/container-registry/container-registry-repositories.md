---
title: "aaaAzure kontejneru registru úložiště | Microsoft Docs"
description: "Jak toouse registru Azure kontejner úložiště pro Docker bitové kopie"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: cristyg
ms.openlocfilehash: 108622c565e41777fbb1fc9da9a01168abc7a7fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-registry-repositories"></a>Kontejner Azure registru úložiště

Kontejner Azure registru umožňuje toostore obrázků kontejneru v úložiště. A to uložením do úložiště bitové kopie, může mít skupin bitové kopie (nebo verzi bitové kopie) v izolovaném prostředí. Tyto úložiště můžete určit, když push registru tooyour bitové kopie.


## <a name="prerequisites"></a>Požadavky
* **Registr kontejnerů Azure** – Vytvořte registr kontejnerů ve svém předplatném Azure. Například použít hello [portál Azure](container-registry-get-started-portal.md) nebo hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).
* **Rozhraní příkazového řádku dockeru** -tooset do místního počítače jako Docker hostitele a přístup hello příkazového řádku Dockeru příkazy, nainstalujte [modulu Docker](https://docs.docker.com/engine/installation/).
* **Obrázek pro vyžádání obsahu** – bitovou kopii pro vyžádání obsahu z hello veřejného úložiště Docker Hub registru, se značkami a poslat ho tooyour registru. Návod push a bitové kopie pro vyžádání obsahu, najdete v části [Docker nabízené image tooAzure privátní registru](container-registry-get-started-docker-cli.md).


## <a name="viewing-repositories-in-hello-portal"></a>Zobrazení úložiště v hello portálu

Jakmile máte nabídnutých registru kontejneru tooyour bitové kopie, uvidíte seznam hello úložiště hostování hello obrázků v hello portálu Azure.

Pokud jste postupovali podle kroků hello v hello [Push Docker image tooAzure privátní registru](container-registry-get-started-docker-cli.md) článku teď byste měli mít bitovou kopii Nginx v registru kontejneru. Jako součást hello pokyny by měl mít zadat obor názvů pro bitové kopie hello. V příkladu hello níže hello příkaz nabízených oznámení hello NGinx image toohello "samples" úložiště:

```
docker push myregistry.azurecr.io/samples/nginx
```
 Azure Container Registry podporuje víceúrovňové obory názvů úložiště. Tato funkce umožňuje vám toogroup kolekce obrázků související tooa konkrétní aplikaci nebo kolekci toospecific vývoj aplikací nebo provozní týmy. tooread Další informace o úložiště v registrech kontejneru, najdete v části [registrech kontejner Docker privátní v Azure](container-registry-intro.md).

tooview hello kontejneru registru úložiště:

1. Přihlaste se toohello portálu Azure
2. Na hello **registru kontejner Azure** okně, vyberte hello registru chcete tooinspect
3. V okně hello registru, klikněte na tlačítko **úložiště** toosee seznam všech hello úložiště a jejich obrázků
4. (Volitelné) Vyberte konkrétní image toosee značky

![Úložiště hello portálu](./media/container-registry-repositories/container-registry-repositories.png)


## <a name="next-steps"></a>Další kroky
Nyní když znáte hello základy, jste připravené toostart pomocí vašeho klíče registru! Například začít s nasazením Image tooan kontejneru [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) clusteru.
