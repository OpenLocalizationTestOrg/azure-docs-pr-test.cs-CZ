---
title: "aaaVisualizing do clusteru pomocí Service Fabric Explorer | Microsoft Docs"
description: "Service Fabric Explorer je webový nástroj pro kontrolu a správa cloudových aplikací a uzly v clusteru s podporou Microsoft Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: ryanwi
ms.openlocfilehash: 73adc4fc254cf6b949b4419b02a046cee3f6a83d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>Vizualizujte cluster pomocí Service Fabric Exploreru
Service Fabric Explorer je webový nástroj pro kontrolu a Správa aplikací a uzly v clusteru služby Azure Service Fabric. Service Fabric Explorer je hostován přímo v rámci clusteru hello, tak, aby byl vždy k dispozici, bez ohledu na to, kde běží vaše clusteru.

## <a name="video-tutorial"></a>Videokurz

toolearn, jak sledovat toouse Service Fabric Explorer hello následující video Microsoft Virtual Academy:

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="connect-tooservice-fabric-explorer"></a>Připojit tooService Fabric Exploreru
Pokud jste postupovali podle pokynů hello příliš[Příprava vývojového prostředí](service-fabric-get-started.md), Service Fabric Explorer můžete spustit v místním clusteru tak, že přejdete toohttp://localhost:19080 / Explorer.

## <a name="understand-hello-service-fabric-explorer-layout"></a>Pochopení hello Service Fabric Explorer rozložení
Pomocí stromové struktury hello na levé straně hello můžete přejít pomocí Service Fabric Exploreru. V kořenovém hello hello stromu řídicí panel clusteru hello obsahuje přehled clusteru, včetně shrnutí stavu uzlu a aplikace.

![Řídicí panel clusteru Service Fabric Exploreru][sfx-cluster-dashboard]

### <a name="view-hello-clusters-layout"></a>Zobrazit rozložení hello clusteru
Uzly v clusteru Service Fabric se umístí napříč dvourozměrná mřížku domén selhání a upgradu domény. Toto umístění zajistí, že vaše aplikace zůstanou dostupné i v hello přítomnost selhání hardwaru a upgrady aplikací. Můžete zobrazit, jak aktuální cluster hello rozložená pomocí hello clusteru mapy.

![Mapa clusteru Service Fabric Exploreru][sfx-cluster-map]

### <a name="view-applications-and-services"></a>Zobrazení aplikace a služby
Hello cluster obsahuje dva podstromy: jeden pro aplikace a druhý pro uzly.

Můžete použít toonavigate zobrazení aplikace hello prostřednictvím hierarchie logické Service Fabric: aplikace, služby, oddíly a repliky.

V příkladu hello níže hello aplikace **Moje aplikace** se skládá ze dvou služeb **MyStatefulService** a **WebService**. Vzhledem k tomu **MyStatefulService** je stavová, obsahuje oddíl s jeden primární a dva sekundární repliky. Naopak WebSvcService je bezstavové a obsahuje jednu instanci.

![Zobrazení aplikace Service Fabric Exploreru][sfx-application-tree]

Na každé úrovni stromu hello hello hlavním podokně zobrazí příslušné informace o položce hello. Například můžete zobrazit stav hello a verze pro konkrétní službu.

![Podokno essentials Service Fabric Exploreru][sfx-service-essentials]

### <a name="view-hello-clusters-nodes"></a>Zobrazení uzlů clusteru hello
zobrazení uzlu Hello zobrazuje fyzické rozložení hello hello clusteru. Pro daný uzel můžete zjistit, které aplikace mají v uzlu nasazený kód. Přesněji řečeno uvidíte, které repliky jsou aktuálně spuštěné existuje.

## <a name="actions"></a>Akce
Service Fabric Explorer nabízí rychlý způsob tooinvoke akce na uzly, aplikace a služby v rámci clusteru.

Například toodelete instance aplikace zvolte aplikace hello ze stromu hello na levé straně hello a potom zvolte **akce** > **odstranit aplikaci**.

![Odstranění aplikace v Service Fabric Exploreru][sfx-delete-application]

> [!TIP]
> Můžete provést klepnutím na další prvek tooeach hello třemi tečkami hello stejné akce.
>
>

Hello následující tabulka uvádí hello akce dostupné u jednotlivých entit:

| **Entity** | **Akce** | **Popis** |
| --- | --- | --- |
| Typ aplikace |Zrušit zřízení typu |Odebere balíček aplikace hello z úložiště bitových kopií hello clusteru. Vyžaduje všechny aplikace toobe tento typ nejdřív odstranit. |
| Aplikace |Odstranění aplikace |Odstraňte hello aplikace, včetně všech jejích služeb a jejich stavu (pokud existuje). |
| Služba |Odstranění služby |Odstraňte hello service a její stav (pokud existuje). |
| Node |Aktivovat |Aktivujte hello uzlu. |
| Node | Deaktivovat (Pozastavit) | Pozastavení hello uzel v jejím aktuálním stavu. Služby pokračovat toorun, ale Service Fabric nepřesouvá proaktivně nic na nebo vypněte ho Pokud je požadovaná tooprevent výpadku nebo data můžou být nekonzistentní. Tato akce je obvykle používanými tooenable ladění služeb na tooensure konkrétním uzlu, který není přesunout během kontroly. | |
| Node | Deaktivovat (restartovat) | Bezpečně přesunete všechny služby v paměti vypnout uzlu a trvalé services zavřete. Obvykle se používá při hello hostitelské procesy nebo toobe nutné počítač restartovat. | |
| Node | Deaktivovat (odebrat data) | Bezpečně zavřete všechny služby běží na uzlu hello po sestavení dostatečná k výměně za chodu repliky. Typicky používá při uzlu (nebo aspoň jeho úložiště) jsou mimo Komise trvale přijata. | |
| Node | Odeberte stav uzlu | Odeberte znalosti repliky uzlu z clusteru hello. Obvykle používá, když už selhání uzlu se považuje neopravitelné. | |
| Node | Restartování | Simulace selhání uzlu restartováním hello uzlu. Další informace [sem](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps) | |

Vzhledem k tomu, že mnoho akce jsou destruktivní, můžete být vyzváni tooconfirm vašich představ, před dokončením akce hello.

> [!TIP]
> Každou akci, která lze provádět pomocí Service Fabric Explorer můžete provést i pomocí prostředí PowerShell nebo rozhraní REST API, tooenable automatizace.
>
>

Můžete taky instancí aplikace Service Fabric Explorer toocreate pro daný typ a verze aplikace. Zvolte typ aplikace hello hello stromové zobrazení, a klikněte na hello **vytvořit instanci aplikace** další verze toohello odkaz byste chtěli v pravém podokně hello.

![Vytvoření instance aplikace v Service Fabric Exploreru][sfx-create-app-instance]

> [!NOTE]
> Instance aplikace vytvořené pomocí Service Fabric Explorer nemůže být aktuálně parametry. Jsou vytvořeny pomocí výchozí hodnoty parametrů.
>
>

## <a name="connect-tooa-remote-service-fabric-cluster"></a>Připojte tooa vzdálený cluster Service Fabric
Pokud znáte koncový bod hello clusteru a že máte dostatečná oprávnění k Service Fabric Explorer máte přístup z libovolného prohlížeče. Je to proto Service Fabric Explorer je právě jiné službě, která běží v clusteru hello.

### <a name="discover-hello-service-fabric-explorer-endpoint-for-a-remote-cluster"></a>Zjistit koncový bod Service Fabric Explorer hello vzdáleného clusteru
tooreach Service Fabric Explorer pro daný cluster, přejděte v prohlížeči na:

http://&lt;vašeho clusteru endpoint&gt;: 19080/Explorer

Azure v rámci clusterů hello úplnou adresu URL je také k dispozici v podokně essentials clusteru hello hello portálu Azure.

### <a name="connect-tooa-secure-cluster"></a>Připojte tooa zabezpečené cluster
Můžete ovládat klienta přístup tooyour cluster Service Fabric pomocí certifikátů nebo pomocí Azure Active Directory (AAD).

Když zkusíte tooconnect tooService Fabric Explorer na clusteru s podporou zabezpečení, pak v závislosti na konfiguraci clusteru hello máte být toopresent vyžaduje klientský certifikát nebo přihlášení pomocí AAD.

## <a name="next-steps"></a>Další kroky
* [Přehled testovatelnosti](service-fabric-testability-overview.md)
* [Správu aplikací Service Fabric v sadě Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Nasazení aplikace Service Fabric pomocí prostředí PowerShell](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
