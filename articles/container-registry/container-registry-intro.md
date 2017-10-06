---
title: registrech kontejner Docker aaaPrivate v Azure | Microsoft Docs
description: "Úvod toohello služba Azure kontejneru registru, poskytující založené na cloudu, spravované, privátní Docker registrech."
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: ee2b652b-fb7c-455b-8275-b8d4d08ffeb3
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f6edcf0bf947b7770ee0a4e4a5cfbf4ef8b7392a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooprivate-docker-container-registries"></a>Registrech kontejner Docker tooprivate Úvod


Azure registru kontejneru je spravované [Docker registru](https://docs.docker.com/registry/) služby založené na hello open-source Docker registru 2.0. Vytvořit a udržovat toostore registrech kontejner Azure a spravovat váš privátní [kontejner Docker](https://www.docker.com/what-docker) bitové kopie. Použijte registrech kontejneru v Azure s existující kontejner vývoj a nasazení kanálů a kreslení v textu hello Docker komunity odborných znalostí.

Související informace o Dockeru a kontejnerech najdete v tématech:

* [Uživatelská příručka Dockeru](https://docs.docker.com/engine/userguide/)




## <a name="use-cases"></a>Případy použití
Bitové kopie lze načítat z Azure kontejneru cílů nasazení toovarious registru:

* **Škálovatelné systémy orchestrace**, které spravují kontejnerizované aplikace napříč clustery hostitelů, včetně [DC/OS](https://docs.mesosphere.com/), [Dockeru Swarm](https://docs.docker.com/swarm/) a [Kubernetes](http://kubernetes.io/docs/).
* **Služby Azure**, které podporují vytváření a spouštění škálovaných aplikací, včetně [Container Service](../container-service/index.yml), [App Service](/app-service/index.md), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/) a dalších.

Vývojářům můžete také push tooa registru kontejneru jako součást pracovního postupu vývoj kontejneru. Mohou například určit registr kontejnerů jako cíl v nástroji pro nasazení a nástroji průběžné integrace, jako je například [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) nebo [Jenkins](https://jenkins.io/).





## <a name="key-concepts"></a>Klíčové koncepty
* **Registr** – Vytvořte jeden nebo více registrů kontejnerů ve svém předplatném Azure. Každý registru je zálohovaný díky standardní Azure [účet úložiště](../storage/common/storage-introduction.md) v hello stejné umístění. Využívat výhod úložiště místní, síťové zavřít obrázků kontejneru vytvořením registru v hello stejného umístění Azure jako vaše nasazení. Název plně kvalifikovaný registru má hello formuláře `myregistry.azurecr.io`.

  Můžete [řízení přístupu](container-registry-authentication.md) tooa kontejneru registru pomocí Azure Active Directory zálohovaná [instanční objekt](../active-directory/active-directory-application-objects.md) nebo účet zadaný správce. Spuštění hello standardní `docker login` tooauthenticate příkaz s registru.

* **Spravovaný registr** – Vrstva, která nabízí další možnosti pro registry ve třech skladových položkách (SKU) – Basic, Standard a Premium. Hello obrázků v těchto SKU jsou uloženy v účtech úložiště spravuje hello registrech kontejner Azure službou, která zvyšuje spolehlivost a umožňuje nové funkce. Nové možnosti zahrnují integraci webhooků, ověřování úložiště pomocí Azure Active Directory a podporu funkce odstraňování. Uživatelé mají možnost toochoose hello mezi spravované registrech nebo toocreate registru zajištěna jejich vlastních účtů úložiště při vytváření registrech.

* **Úložiště** – Registr obsahuje jedno nebo několik úložišť, což jsou skupiny imagí kontejnerů. Azure Container Registry podporuje víceúrovňové obory názvů úložiště. Tato funkce umožňuje vám toogroup kolekce obrázků související tooa konkrétní aplikaci nebo kolekci toospecific vývoj aplikací nebo provozní týmy. Například:

  * `myregistry.azurecr.io/aspnetcore:1.0.1` představuje image pro celý podnik.
  * `myregistry.azurecr.io/warrantydept/dotnet-build`představuje používá bitová kopie aplikace .NET toobuild, sdílená mezi oddělení záruky hello
  * `myregistry.azrecr.io/warrantydept/customersubmissions/web`představuje bitovou kopii webové seskupeny v rámci hello zákazníka odesílání aplikace, která je vlastníkem hello záruky oddělení

* **Image** – Každá image uložená v úložišti je snímkem kontejneru Dockeru jen pro čtení. Registry kontejnerů Azure mohou zahrnovat image systémů Windows i Linux. Názvy imagí pro všechna nasazení kontejnerů určujete vy. Použijte standardní [Docker příkazy](https://docs.docker.com/engine/reference/commandline/) toopush bitové kopie do úložiště nebo přijetí změn bitovou kopii z úložiště.

* **Kontejner** – Kontejner definuje softwarovou aplikaci a její závislosti zabalené do kompletního systému souborů, včetně kódu, modulu runtime, systémových nástrojů a knihoven. Spouštějte kontejnery Dockeru na základě imagí systémů Windows nebo Linux, které si stáhnete z registru kontejnerů. Kontejnery, které jsou spuštěné v jednom počítači sdílet jádra hello operačního systému. Kontejnery docker jsou plně přenositelné tooall hlavní distribucích systému Linux, Mac a Windows.




## <a name="next-steps"></a>Další kroky
* [Vytvoření kontejneru registru pomocí hello portálu Azure](container-registry-get-started-portal.md)
* [Vytvoření kontejneru registru pomocí hello rozhraní příkazového řádku Azure](container-registry-get-started-azure-cli.md)
* [Push vaší první image pomocí příkazového řádku Dockeru hello](container-registry-get-started-docker-cli.md)
* toobuild průběžnou integraci a pracovní postup nasazení pomocí Visual Studio Team Services, Azure Container Service a Azure kontejneru registr, najdete v části [v tomto kurzu](../container-service/dcos-swarm/container-service-docker-swarm-setup-ci-cd.md).
* Pokud chcete tooset vlastní Docker privátní registr systému Azure (bez veřejný koncový bod), najdete v části [nasazení vaše vlastní privátní Docker registru na platformě Azure](../virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md).
