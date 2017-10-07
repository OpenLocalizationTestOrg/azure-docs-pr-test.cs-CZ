---
title: aaaDeploy tooa aplikace Azure Service Fabric strany clusteru | Microsoft Docs
description: "Zjistěte, jak clusteru strany toodeploy tooa aplikaci."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: mikhegn
ms.openlocfilehash: db16b6418fa2533ed915c8b6e612b7a8e7311bed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-tooa-party-cluster-in-azure"></a>Nasazení aplikace tooa strany clusteru v Azure
V tomto kurzu je součástí dvě řady a ukazuje, jak toodeploy tooa aplikace Azure Service Fabric strany clusteru v Azure.

V druhé části kurzu řady hello zjistíte, jak:
> [!div class="checklist"]
> * Nasazení clusteru vzdálené tooa aplikace pomocí sady Visual Studio
> * Odebrání aplikace z clusteru pomocí Service Fabric Exploreru

V této série kurzu zjistíte, jak:
> [!div class="checklist"]
> * [Sestavení aplikace .NET Service Fabric](service-fabric-tutorial-create-dotnet-app.md)
> * Nasazení vzdáleného clusteru tooa hello aplikace
> * [Konfigurace CI/CD pomocí Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu:
- Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Nainstalovat Visual Studio 2017](https://www.visualstudio.com/) a nainstalujte hello **Azure development** a **ASP.NET a webové vývoj** úlohy.
- [Nainstalujte hello Service Fabric SDK](service-fabric-get-started.md)

## <a name="download-hello-voting-sample-application"></a>Stáhněte si ukázkovou aplikaci Voting hello
Pokud není sestavení hello Voting ukázkovou aplikaci [součástí jeden z této série kurz](service-fabric-tutorial-create-dotnet-app.md), můžete ho stáhnout. V příkazovém okně spusťte následující příkaz tooclone hello ukázkové aplikace úložiště tooyour místního počítače hello.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="set-up-a-party-cluster"></a>Nastavení clusteru strany
Strany clustery jsou zdarma, časově omezené clusterů Service Fabric hostované v Azure a spusťte Service Fabric týmem hello kde každý, kdo můžete nasadit aplikace a další informace o platformě hello. Zdarma!

tooget přístup tooa strany clusteru, procházet toothis lokality: http://aka.ms/tryservicefabric a postupujte podle hello pokyny tooget tooa clusteru přístupu. Je nutné Facebooku nebo Githubu účet tooget přístup tooa strany clusteru.

> [!NOTE]
> Strany clustery není zabezpečená, aby vaše aplikace a všechna data, která jste uložili v nich může být viditelné tooothers. Nenasazujte nic nechcete, aby ostatní toosee. Být jisti tooread přes naše podmínky použití pro všechny podrobnosti hello.

## <a name="configure-hello-listening-port"></a>Nakonfigurujte port naslouchání hello
Když je vytvořen hello VotingWeb front-endová služba, Visual Studio náhodně zvolí port pro službu toolisten hello na.  Hello VotingWeb služba funguje jako front-end pro tuto aplikaci hello a přijímá externích přenosů, proto budeme vazby této služby tooa pevné a také znát port. V Průzkumníku řešení otevřete *VotingWeb/PackageRoot/ServiceManifest.xml*.  Najde hello **koncový bod** prostředku v hello **prostředky** části a změňte hello **Port** too80 hodnotu.

```xml
<Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port on which too
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="80" />
    </Endpoints>
  </Resources>
```

Také aktualizujte hodnotu vlastnosti hello adresa URL aplikace v projektu Voting hello, otevře se webový prohlížeč toohello správný port při ladění pomocí 'F5'.  V Průzkumníku řešení, vyberte hello **Voting** projekt a aktualizace hello **adresa URL aplikace** vlastnost.

![Adresa URL aplikace](./media/service-fabric-tutorial-deploy-app-to-party-cluster/application-url.png)

## <a name="deploy-hello-app-toohello-azure"></a>Nasazení aplikace toohello hello Azure
Teď, když aplikace hello je připraveno, můžete ho nasadit toohello strany clusteru přímé ze sady Visual Studio.

1. Klikněte pravým tlačítkem na **Voting** v hello Průzkumníku řešení a zvolte **publikovat**.

    ![Dialogové okno Publikovat](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)

2. Zadejte hello koncového bodu připojení hello strany clusteru v hello **koncového bodu připojení** pole a klikněte na tlačítko **publikovat**.

    Po publikování hello je dokončena, byste měli mít toosend k žádosti o toohello aplikaci prostřednictvím prohlížeče.

3. Otevřít vaše preferované prohlížeč a zadejte adresu clusteru hello (hello koncového bodu připojení bez informace o portu hello – například win1kw5649s.westus.cloudapp.azure.com).

    Teď byste měli vidět hello stejný výsledek jako jste viděli při místním spuštění aplikace hello.

    ![Rozhraní API odpověď z clusteru](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="remove-hello-application-from-a-cluster-using-service-fabric-explorer"></a>Hello aplikaci odebrat z clusteru pomocí Service Fabric Exploreru
Service Fabric Explorer je tooexplore grafické uživatelské rozhraní a spravovat aplikace v clusteru Service Fabric.

aplikace hello tooremove z hello strany clusteru:

1. Procházejte toohello Service Fabric Explorer pomocí odkazu hello poskytované stránku hello strany clusteru. Například http://win1kw5649s.westus.cloudapp.azure.com:19080/Explorer/index.html.

2. V Service Fabric Exploreru přejděte toohello **fabric://Voting** uzlu treeview hello na levé straně hello.

3. Klikněte na tlačítko hello **akce** tlačítko v pravém hello **Essentials** podokně a zvolte **odstranit aplikaci**. Potvrďte odstraňování hello instanci aplikace, který odebere hello instanci naše aplikace běžící v clusteru hello.

![Odstranění aplikace v Service Fabric Exploreru](./media/service-fabric-tutorial-deploy-app-to-party-cluster/delete-application.png)

## <a name="remove-hello-application-type-from-a-cluster-using-service-fabric-explorer"></a>Typ aplikace hello odebrat z clusteru pomocí Service Fabric Exploreru
Aplikace jsou nasazeny jako typy aplikací v clusteru Service Fabric, což umožňuje vám toohave více instancí a verzí hello aplikace běžící v rámci clusteru hello. Po odebrání hello spuštěna instance naše aplikace jsme rovněž můžete odebrat hello typu, čištění hello toocomplete hello nasazení.

Další informace o modelu hello aplikace v Service Fabric najdete v tématu [Model aplikace v Service Fabric](service-fabric-application-model.md).

1. Přejděte toohello **VotingType** uzlu hello treeview.

2. Klikněte na tlačítko hello **akce** tlačítko v pravém hello **Essentials** podokně a zvolte **Unprovision typu**. Potvrďte Rušení zřizování typ aplikace hello.

![Zrušit zřízení typu aplikace v Service Fabric Exploreru](./media/service-fabric-tutorial-deploy-app-to-party-cluster/unprovision-type.png)

Tím končí hello kurzu.

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Nasazení clusteru vzdálené tooa aplikace pomocí sady Visual Studio
> * Odebrání aplikace z clusteru pomocí Service Fabric Exploreru

ADVANCE toohello další kurz:
> [!div class="nextstepaction"]
> [Nastavte průběžnou integraci pomocí Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)