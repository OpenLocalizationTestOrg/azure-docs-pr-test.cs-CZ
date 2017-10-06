---
title: aaaAuthenticate s registru kontejner Azure | Microsoft Docs
description: "Jak služba toolog v registru tooan kontejner Azure pomocí Azure Active Directory objekt zabezpečení nebo účet správce"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 128a937a-766a-415b-b9fc-35a6c2f27417
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a0b0462e8432b2567689debca322e2426baa7fa2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-a-private-docker-container-registry"></a>Ověření pomocí privátní registru kontejner Docker
toowork s obrázky kontejneru v registru kontejner Azure, můžete přihlásit pomocí hello `docker login` příkaz. Můžete se přihlásit pomocí  **[objektu služby Azure Active Directory](../active-directory/active-directory-application-objects.md)**  nebo konkrétního registru **účet správce**. Tento článek obsahuje více podrobností o těchto identit.



## <a name="service-principal"></a>Instanční objekt

Můžete [přiřadit objekt služby](container-registry-get-started-azure-cli.md#assign-a-service-principal) tooyour registru a použít jej pro základní ověřování Docker. Při používání objektu služby se doporučuje pro většinu scénářů. Zadejte ID aplikace hello a heslo hello služby hlavní toohello `docker login` příkaz, jak je znázorněno v hello následující ukázka:

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Po přihlášení, Docker ukládá do mezipaměti přihlašovací údaje hello, takže není nutné ID tooremember hello aplikace.

> [!TIP]
> Pokud chcete, můžete obnovit heslo hello objekt služby spuštěním hello `az ad sp reset-credentials` příkaz.
>


Objekty služby povolit [přístupu podle rolí](../active-directory/role-based-access-control-configure.md) tooa registru. Dostupné role jsou:
  * Čtečka (pouze přístup k vyžádání obsahu).
  * Přispěvatel (pull a push).
  * Vlastník (pull, nabízená oznámení a přiřadit role tooother uživatele).

Anonymní přístup není k dispozici na registrech kontejner Azure. Pro veřejné bitové kopie můžete použít [úložiště Docker Hub](https://docs.docker.com/docker-hub/).

Můžete přiřadit více objekty služby tooa registru, která vám umožní přístup toodefine pro různé uživatele nebo aplikace. Objekty služby také povolit "bezobslužných" připojení k registru tooa vývojáře nebo DevOps scénáře, jako je hello následující příklady:

  * Nasazení kontejnerů ze systémů registru tooorchestration včetně DC/OS, Docker Swarm a Kubernetes. Můžete také posunout toorelated registrech kontejneru služby Azure, jako [Container Service](../container-service/index.yml), [služby App Service](../app-service/index.md), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/)a další.

  * Průběžnou integraci a nasazení řešení (například Visual Studio Team Services nebo volaných) vytvářet bitové kopie kontejneru a vložit je tooa registru.





## <a name="admin-account"></a>Účet správce
S každou registru, které vytvoříte se automaticky vytvoří účet správce. Ve výchozím nastavení je účet hello zakázat, ale můžete ji povolit a spravovat hello přihlašovací údaje, například prostřednictvím hello [portál](container-registry-get-started-portal.md#manage-registry-settings) nebo pomocí hello [příkazy Azure CLI 2.0](container-registry-get-started-azure-cli.md#manage-admin-credentials). Každý účet správce se s dvě hesla, které mohou vytvořit znovu. dvě hesla Hello povolit toomaintain připojení toohello registru s využitím jedno heslo, zatímco si znovu vygenerujete hello jiné heslo. Pokud je povoleno hello účet, můžete předat hello uživatelské jméno a buď heslo toohello `docker login` příkazu pro základní ověřování toohello registru. Například:

```
docker login myregistry.azurecr.io -u myAdminName -p myPassword1
```

> [!IMPORTANT]
> účet správce Hello je určená pro registru hello tooaccess jednoho uživatele, především pro účely testování. Není doporučeno tooshare přihlašovací údaje účtu správce hello mezi jiných uživatelů. Všichni uživatelé se zobrazí jako soubor registru toohello jednoho uživatele. Změna nebo zakázání účtu zakáže přístup k registru pro všechny uživatele, kteří používají přihlašovací údaje hello.
>


### <a name="next-steps"></a>Další kroky
* [Push vaší první image pomocí příkazového řádku Dockeru hello](container-registry-get-started-docker-cli.md).
* Další informace o ověřování ve verzi preview hello kontejneru registru najdete v tématu hello [příspěvku na blogu](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/).
