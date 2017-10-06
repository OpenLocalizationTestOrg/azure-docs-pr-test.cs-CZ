---
title: "aaaQuickly nasazení stávajícího clusteru Azure Service Fabric tooan aplikace"
description: "Pomocí Azure Service Fabric clusteru toohost stávající aplikace Node.js pomocí sady Visual Studio."
services: service-fabric
documentationcenter: nodejs
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/13/2017
ms.author: adegeo
ms.openlocfilehash: 20a3eb4a9206ba465acf96d0976ba241b07158bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="host-a-nodejs-application-on-azure-service-fabric"></a>Hostování aplikace Node.js na platformě Azure Service Fabric

Tento rychlý start vám pomůže nasadit existující aplikace (Node.js v tomto příkladu) tooa Service Fabric cluster spuštěné v Azure.

## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, že máte [nastavené vývojové prostředí](service-fabric-get-started.md). Které zahrnuje instalaci hello Service Fabric SDK a Visual Studio 2017 nebo 2015.

Budete také potřebovat toohave existující aplikaci Node.js pro nasazení. V tomto rychlém startu se používá jednoduchý web v Node.js, který je ke stažení [zde][download-sample]. Extrahování tento soubor tooyour `<path-to-project>\ApplicationPackageRoot\<package-name>\Code\` složky Po vytvoření projektu hello v dalším kroku hello.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet][create-account].

## <a name="create-hello-service"></a>Vytvoření služby hello

Spusťte sadu Visual Studio jako **správce**.

Vytvořte projekt pomocí klávesové zkratky `CTRL`+`SHIFT`+`N`.

V hello **nový projekt** dialogovém okně, vyberte **Cloud > aplikace Service Fabric**.

Název aplikace hello **MyGuestApp** a stiskněte klávesu **OK**.

>[!IMPORTANT]
>Node.js můžete snadno rozdělit hello 260 znaků limit pro cesty, které má windows. Například použijte krátký cestu samotném projektu hello **c:\code\svc1**.
   
![Dialogové okno Nový projekt ve Visual Studiu][new-project]

Můžete vytvořit jakýkoli typ služby Service Fabric z dialogového okna Další hello. Pro účely tohoto Rychlého startu zvolte **Spustitelný soubor typu Host**.

Název služby hello **MyGuestService** a možnosti pro sadu hello na pravém toohello hello následující hodnoty:

| Nastavení                   | Hodnota |
| ------------------------- | ------ |
| Složka balíčku kódu       | _&lt;Složka Hello se aplikace Node.js&gt;_ |
| Chování balíčku kódu     | Zkopírujte složku obsah tooproject |
| Program                   | node.exe |
| Argumenty                 | server.js |
| Pracovní složka            | CodePackage |

Stiskněte **OK**.

![Dialogové okno Nová služba ve Visual Studiu][new-service]

Visual Studio vytvoří projekt aplikace hello a projekt služby objektu actor hello a zobrazí je v Průzkumníku řešení.

projekt aplikace Hello (**MyGuestApp**) přímo neobsahuje žádný kód. Odkazuje ale na sadu projektů služeb. Kromě toho obsahuje další tři typy obsahu:

* **Profily publikování**  
Předvolby nástrojů pro různá prostředí.

* **Skripty**  
Skript PowerShellu pro nasazení/upgrade aplikace.

* **Definice aplikace**  
Obsahuje manifest aplikace hello *ApplicationPackageRoot*. Soubory parametrů přidružené aplikace jsou v části *ApplicationParameters*, které definují hello aplikace a umožní vám tooconfigure ji speciálně pro dané prostředí.
    
Přehled hello obsah hello projektu služby najdete v tématu [Začínáme se službami Reliable Services](service-fabric-reliable-services-quick-start.md).

## <a name="set-up-networking"></a>Nastavení síťových služeb

Příklad Hello aplikace Node.js jsme nasazovali používá port **80** a potřebujeme tootell Service Fabric, která je třeba, že port zpřístupněný.

Otevřete hello **ServiceManifest.xml** soubor v projektu hello. V dolní části hello hello manifestu, je `<Resources> \ <Endpoints>` se záznamem již definována. Upravit tuto položku tooadd `Port`, `Protocol`, a `Type`. 

```xml
  <Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port on which too
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="MyGuestAppServiceTypeEndpoint" Port="80" Protocol="http" Type="Input" />
    </Endpoints>
  </Resources>
```

## <a name="deploy-tooazure"></a>Nasazení tooAzure

Pokud vyberete **F5** a spusťte projekt hello, je nasazené toohello místní cluster. Ale nyní nasadíme tooAzure místo.

Klikněte pravým tlačítkem na projekt hello a zvolte **publikování...**  který otevře tooAzure toopublish dialogové okno.

![Publikování tooazure dialogové okno pro službu service fabric][publish]

Vyberte hello **PublishProfiles\Cloud.xml** cíle profilu.

Pokud jste to ještě dříve, zvolte toodeploy účtu Azure k. Pokud ještě žádný nemáte, [zaregistrujte si bezplatný účet][create-account].

V části **koncového bodu připojení**, vyberte hello toodeploy clusteru Service Fabric na. Pokud nemáte jeden, vyberte  **&lt;vytvoření nového clusteru... &gt;**  který otevře webový prohlížeč okno toohello portálu Azure. Další informace najdete v tématu [vytvoření clusteru s podporou portálu hello](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal). 

Když vytvoříte cluster Service Fabric hello, ujistěte se, zda text hello tooset **vlastní koncové body** nastavení příliš**80**.

![Konfigurace typu uzlu Service Fabric s vlastním koncovým bodem][custom-endpoint]

Vytvoření nového clusteru Service Fabric trvá některé toocomplete čas. Poté, co byl vytvořený, přejděte zpět toohello dialogového okna publikování a vyberte  **&lt;aktualizovat&gt;**. nový cluster Hello je uvedena v rozevíracím seznamu hello; vyberte ho.

Stiskněte klávesu **publikovat** a počkejte toofinish nasazení hello.

Může to trvat několik minut. Po jeho dokončení může trvat několik minut pro toobe aplikace hello plně k dispozici.

## <a name="test-hello-website"></a>Test hello webu

Jakmile bude vaše služba publikována, otestujte ji ve webovém prohlížeči. 

První otevřete hello portál Azure a vyhledání služby Service Fabric.

Zkontrolujte okno Přehled hello adresu služby hello. Použijte název domény hello z hello _koncového bodu připojení klienta_ vlastnost. Například, `http://mysvcfab1.westus2.cloudapp.azure.com`.

![Okno Přehled Service fabric na hello portálu Azure][overview]

Přejděte na adresu toothis, kde uvidíte hello `HELLO WORLD` odpovědi.

## <a name="delete-hello-cluster"></a>Odstranění clusteru hello

Nevynechali toodelete všechny hello prostředky, které jste vytvořili pro tento rychlý start, jako je budou účtovat tyto prostředky.

## <a name="next-steps"></a>Další kroky
Další informace o [spustitelných souborech typu Host](service-fabric-deploy-existing-app.md).

<!-- Image References -->

[new-project]: ./media/quickstart-guest-app/new-project.png
[new-service]: ./media/quickstart-guest-app/template.png
[solution-exp]: ./media/quickstart-guest-app/solution-explorer.png
[publish]: ./media/quickstart-guest-app/publish.png
[overview]: ./media/quickstart-guest-app/overview.png
[custom-endpoint]: ./media/quickstart-guest-app/custom-endpoint.png

[download-sample]: https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/service-fabric-node-website.zip
[create-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F