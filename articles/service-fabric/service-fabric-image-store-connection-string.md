---
title: "aaaAzure Service Fabric image store řetězci | Microsoft Docs"
description: "Pochopení hello image store připojovací řetězec"
services: service-fabric
documentationcenter: .net
author: alexwun
manager: timlt
editor: 
ms.assetid: 00f8059d-9d53-4cb8-b44a-b25149de3030
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: alexwun
ms.openlocfilehash: 83f5ad75b5df07726997da3173722028255b8cae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-hello-imagestoreconnectionstring-setting"></a>Pochopení hello parametr ImageStoreConnectionString nastavení

V některé dokumentaci jsme stručně zmínili hello existenci parametru "Parametr ImageStoreConnectionString" bez popisující, co opravdu znamená. A poté, co projde článek jako [nasazení a odeberte aplikací pomocí prostředí PowerShell][10], vypadá všechny uděláte je hodnota hello kopírování a vkládání, jak se objevuje v manifestu clusteru hello hello cíle cluster. Takže hello nastavení musí být konfigurovat každý cluster, ale při vytváření clusteru prostřednictvím hello [portál Azure][11], neexistuje žádná možnost tooconfigure tato nastavení a jeho vždy "fabric: úložiště bitových kopií". Co je toto nastavení pak účel hello?

![Manifestu clusteru][img_cm]

Service Fabric spuštěna jako platformu pro interní používání Microsoft mnoha různými týmy, takže některé aspekty jsou vysoce přizpůsobitelné – hello "Image Store" je jeden takový aspekt. V podstatě hello úložiště Image Store je modulární úložiště pro ukládání balíčky aplikací. Když je aplikace nasazená tooa uzlu v clusteru hello, stáhne tento uzel hello obsah balíčku aplikace hello úložiště bitových kopií. Parametr ImageStoreConnectionString Hello je nastavení, která zahrnuje všechny hello potřebné informace pro klienty a uzly toofind hello správné úložiště Image Store pro daný cluster.

Aktuálně existují tři možné typy poskytovatelů úložiště Image Store a jejich odpovídající připojovací řetězce jsou následující:

1. Služba úložiště Image Store: "fabric: úložiště bitových kopií"

2. Systém souborů: "file:[file systému cesta]"

3. Azure Storage: "xstore:DefaultEndpointsProtocol = https; AccountName = [...]; AccountKey = [...]; Kontejner = [...] "

Typ zprostředkovatele Hello použít v produkčním prostředí je hello služby úložiště bitové kopie, což je stavová trvalou systémová služba, která se zobrazí v Service Fabric Exploreru. 

![Služby úložiště bitových kopií][img_is]

Hostování hello úložiště bitové kopie v systému služby v rámci samotného clusteru hello eliminuje vnější závislosti pro úložiště balíčků hello a nám dává větší kontrolu nad hello polohu úložiště. Budoucí vylepšení kolem hello úložiště Image Store zprostředkovatele úložiště Image Store hello pravděpodobně tootarget jsou první, není-li výhradně. Hello připojovací řetězec pro zprostředkovatele služby úložiště bitové kopie hello nemá žádné jedinečné informace od klienta hello je už připojené toohello cílový cluster. Hello klient potřebuje pouze tooknow, že má být použita protokoly cílení na službu system hello.

Hello zprostředkovatele systému souborů se používá namísto hello služby úložiště bitové kopie pro místní clustery jeden pole během vývoj toobootstrap hello clusteru mírně rychlejší. Hello rozdíl je obvykle malý, ale je užitečné optimalizace pro většinu zaměstnance během vývoje. Jeho možných toodeploy místní pole jeden cluster s hello dalších úložiště zprostředkovatele typů a, ale obvykle neexistuje žádný důvod toodo tak vzhledem k tomu, že zůstane pracovního postupu pro vývoj/testování hello hello stejný bez ohledu na to zprostředkovatele. Než toto použití zprostředkovatele systému souborů a Azure Storage hello pouze existovat podporuje starší verze.

Proto při konfigurovat hello parametr ImageStoreConnectionString obecně právě používáte hello výchozí nastavení. Při publikování tooAzure prostřednictvím [Visual Studio][12], parametr hello se nastaví automaticky, odpovídajícím způsobem. Pro programové nasazení tooclusters hostované v Azure hello připojovací řetězec je vždy "fabric: úložiště bitových kopií". V případě, že pokud máte pochybnosti, jeho hodnota vždy ověřovány načítání manifestu clusteru hello podle [prostředí PowerShell](https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricclustermanifest), [.NET](https://msdn.microsoft.com/library/azure/mt161375.aspx), nebo [REST](https://docs.microsoft.com/rest/api/servicefabric/get-a-cluster-manifest). Místní testování a produkčních clusterů by měla být vždy nakonfigurované toouse hello Image Store poskytovatele služeb také.

### <a name="next-steps"></a>Další kroky
[Nasazení a odebírat aplikace pomocí prostředí PowerShell][10]

<!--Image references-->
[img_is]: ./media/service-fabric-image-store-connection-string/image_store_service.png
[img_cm]: ./media/service-fabric-image-store-connection-string/cluster_manifest.png

[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-cluster-creation-via-portal.md
[12]: service-fabric-publish-app-remote-cluster.md
