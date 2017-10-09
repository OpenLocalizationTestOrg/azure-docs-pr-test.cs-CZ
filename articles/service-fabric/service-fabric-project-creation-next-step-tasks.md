---
title: "Další kroky aaaService Fabric projektu vytvoření | Microsoft Docs"
description: "Tento článek obsahuje odkazy tooa sadu základní vývojářských úloh pro Service Fabric"
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 299d1f97-1ca9-440d-9f81-d1d0dd2bf4df
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: rwike77
ms.openlocfilehash: 45598bfabedf280fba8af449ef920f40b409a609
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="your-service-fabric-application-and-next-steps"></a>Vaše aplikace Service Fabric a další kroky
Vaše aplikace Azure Service Fabric byla vytvořena. Tento článek popisuje způsob vytvoření hello projektu a některé potenciální další kroky.

## <a name="your-application"></a>Vaše aplikace
Každé nové aplikace zahrnuje projekt aplikace. Může mít jeden nebo dva další projekty, v závislosti na typu hello služby vybrali.

### <a name="hello-application-project"></a>projekt aplikace Hello
projekt aplikace Hello se skládá z:

* Sada služeb toohello odkazy, které tvoří vaši aplikaci.
* Tři publikační profily (1uzlu místní, 5uzlu místní a cloudové), které můžete použít toomaintain předvolby pro práci s různých prostředích – například koncový bod clusteru související tooa předvolby a jestli tooperform upgradujte nasazení ve výchozím nastavení.
* Tři soubory parametrů aplikace (stejné jako výše), které můžete použít konfigurace toomaintain konkrétní prostředí aplikace, například hello počet oddílů toocreate pro službu.
* Skript nasazení, které můžete použít toodeploy aplikace z příkazového řádku hello nebo jako součást automatizované průběžnou integraci a nasazení kanálu.
* manifest aplikace Hello, která popisuje aplikace hello. Hello manifest najdete ve složce ApplicationPackageRoot hello.

### <a name="stateless-service"></a>Bezstavové služby
Při přidání nové bezstavové služby Visual Studio. přidá řešení tooyour projekt služby, který zahrnuje typ následníky `StatelessService`. Služba Hello zvýší místní proměnné v čítače.

### <a name="stateful-service"></a>Stavové služby
Když přidáte novou stavové služby, Visual Studio přidá řešení tooyour projekt služby, který zahrnuje typ následníky `StatefulService`. Hello služby zvýší čítače v jeho `RunAsync` metoda a úložišť hello vést `ReliableDictionary`.

### <a name="actor-service"></a>Služby objektu actor
Při přidání nového objektu actor spolehlivé, Visual Studio přidá dva projekty tooyour řešení: objektu actor projekt a projekt rozhraní.

poskytuje metody pro nastavení Hello objektu actor projektu a získávání hello hodnotu počítadla, která je spolehlivě uloženy v rámci stavu objektu actor hello. Hello rozhraní projektu poskytuje rozhraní, aby další služby můžete použít objektu actor tooinvoke hello.

### <a name="stateless-web-api"></a>Bezstavové webového rozhraní API
Hello bezstavové projekt webového rozhraní API poskytuje základní webové služby, které můžete použít tooopen vaši klienti tooexternal aplikace. Další informace o tom, jak hello projektu strukturovaná, najdete v části [Service Fabric webového rozhraní API služby s vlastním hostování OWIN](service-fabric-reliable-services-communication-webapi.md).


### <a name="aspnet-core"></a>Jádro ASP.NET
Hello Service Fabric SDK poskytuje stejné sady ASP.NET Core šablon, které jsou k dispozici pro samostatnou projektů ASP.NET Core hello: prázdná, [webového rozhraní API][aspnet-webapi], a [webové aplikace][aspnet-webapp].

### <a name="guest-executables-and-guest-containers"></a>Spustitelné soubory a hostů kontejnery hosta

Service Fabric, guest, je služba, která není vytvořené s nástroji programovací modely hello platformy. Binární soubory hello můžete balíček pro hosta buď [přímo v balíčku aplikace hello](service-fabric-deploy-existing-app.md) nebo [prostřednictvím bitovou kopii kontejneru](service-fabric-deploy-container.md). V obou případech Visual Studio vytvoří hello artefakty potřebné v hello **ApplicationPackageRoot** složku projekt aplikace hello. Visual Studio nebude vytvořit nový projekt služby, protože hello kód již existuje jinde. Pokud chcete toomanage vaše hosta projekty spolu s hello projekt aplikace Service Fabric, můžete je přidat toohello stejné řešení sady Visual Studio.

## <a name="next-steps"></a>Další kroky
### <a name="create-an-azure-cluster"></a>Vytvoření clusteru služby Azure
Hello Service Fabric SDK poskytuje místní cluster pro vývoj a testování. toocreate cluster v Azure, najdete v části [nastavení cluster Service Fabric z portálu Azure hello][create-cluster-in-portal].

### <a name="publish-your-application-tooazure"></a>Publikování tooAzure vaší aplikace
Můžete publikovat aplikaci přímo v sadě Visual Studio tooan clusteru Azure. Postup naleznete v tématu toolearn [publikování vaší aplikace tooAzure][publish-app-to-azure].

### <a name="use-service-fabric-explorer-toovisualize-your-cluster"></a>Použijte váš cluster Service Fabric Explorer toovisualize
Service Fabric Explorer nabízí snadný způsob toovisualize cluster, včetně nasazené aplikace a fyzické rozložení. Další, najdete v části toolearn [vizualizace vašeho clusteru pomocí Service Fabric Explorer][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Verze a upgradujte vašim službám
Service Fabric umožňuje nezávislé Správa verzí a upgrade nezávislých služeb v aplikaci. Další, najdete v části toolearn [Správa verzí a upgrade vašich služeb][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Konfigurace průběžnou integraci s Visual Studio Team Services
toolearn způsobu průběžnou integraci procesu můžete nastavit pro vaši aplikaci Service Fabric najdete v [nakonfigurovat průběžnou integraci s Visual Studio Team Services][ci-with-vso].

<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
