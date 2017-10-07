---
title: "aaaPublish aplikace tooa vzdáleného clusteru služby pomocí sady Visual Studio | Microsoft Docs"
description: "Zjistěte, jak toopublish aplikaci tooa vzdálené služby fabric clusteru pomocí sady Visual Studio."
services: service-fabric
documentationcenter: na
author: cawams
manager: timlt
editor: 
ms.assetid: faecd892-eb54-4d9c-8023-c67442afb8e8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/29/2016
ms.author: cawa
ms.openlocfilehash: d0f06f120cc7e22f3f8e73ce0970e1da5823e647
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-remove-applications-using-visual-studio"></a>Nasazení a odebírat aplikace pomocí sady Visual Studio
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Visual Studio](service-fabric-publish-app-remote-cluster.md)
> * [Rozhraní API FabricClient](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

Hello Azure Service Fabric rozšíření pro Visual Studio poskytuje snadný, opakovatelné a Skriptovatelná způsob toopublish clusteru Service Fabric tooa služby aplikací.

## <a name="hello-artifacts-required-for-publishing"></a>Hello artefakty potřebné pro publikování
### <a name="deploy-fabricapplicationps1"></a>Nasazení FabricApplication.ps1
Toto je skript prostředí PowerShell, který používá cestu k profilu publikování jako parametr pro publikování aplikací Service Fabric. Vzhledem k tomu, že tento skript je součástí aplikace, se úvodní toomodify jej podle potřeby pro vaši aplikaci.

### <a name="publish-profiles"></a>Publikační profily
Složka v projektu aplikace Service Fabric hello název **PublishProfiles** obsahuje soubory XML, které obsahují informace nezbytné pro publikování aplikace, jako například:

* Parametry připojení clusteru Service Fabric
* Soubor parametrů aplikace tooan cesta
* Nastavení upgradu

Ve výchozím nastavení, bude vaše aplikace obsahovat tři publikační profily: Local.1Node.xml, Local.5Node.xml a Cloud.xml. Můžete přidat další profily pomocí kopírování a vkládání mezi hello výchozí soubory.

### <a name="application-parameter-files"></a>Soubory parametrů aplikace
Složka v projektu aplikace Service Fabric hello název **ApplicationParameters** obsahuje soubory XML hodnoty parametrů manifestu aplikace definované uživatelem. Soubory manifestu aplikace lze nastavit parametry, takže můžete použít jiné hodnoty pro nastavení nasazení. toolearn Další informace o Parametrizace vaší aplikace, najdete v části [spravovat prostředí s více v Service Fabric](service-fabric-manage-multiple-environment-app-configuration.md).

> [!NOTE]
> Pro služby objektu actor, měli byste vytvořit projekt hello nejprve před pokusem o tooedit hello souboru v editoru nebo prostřednictvím hello dialogové okno publikování. To je proto součástí souborů manifestu hello se budou generovat během vytváření sestavení hello.

## <a name="toopublish-an-application-using-hello-publish-service-fabric-application-dialog-box"></a>toopublish aplikaci pomocí dialogového okna publikovat aplikace Service Fabric hello
Hello následující kroky ukazují, jak toopublish aplikace pomocí hello **publikovat aplikace Service Fabric** dialogové okno poskytované hello prostředků infrastruktury nástroje sady Visual Studio Service.

1. V místní nabídce hello hello projektu aplikace Service Fabric, zvolte **publikování...** tooview hello **publikovat aplikace Service Fabric** dialogové okno.
   
    ![Hello ** publikování Service Fabric aplikace ** dialogové okno][0]
   
    Hello souboru vybraného v hello **cíle profil** rozevíracím seznamu je tam, kde všechna nastavení hello, s výjimkou **Manifest verze**, se uloží. Můžete znovu použít stávající profil nebo vytvořit novou výběrem **< spravovat profily... >** v hello **cíle profil** rozevíracím seznamu. Pokud vyberete profil publikování, její obsah se zobrazí v hello odpovídající polích dialogového okna hello. toosave změny kdykoli, zvolte hello **uložit profil** odkaz.    
2. V hello **koncového bodu připojení** , určete místního nebo vzdáleného clusteru Service Fabric na publikování koncový bod. tooadd nebo změňte hello koncového bodu připojení, klikněte na hello **koncového bodu připojení** rozevíracího seznamu. Hello seznamu zobrazuje hello k dispozici Service Fabric clusteru připojení koncové body toowhich, které můžete publikovat v závislosti na vašich předplatných Azure. Všimněte si, že pokud se ještě nepřihlásili tooVisual Studio, bude výzvami toodo tak.
   
    Použijte hello clusteru výběr dialogové okno pole toochoose z hello sadu dostupných předplatných a clustery.
   
    ![Hello ** vyberte Service Fabric clusteru ** dialogové okno][1]
   
   > [!NOTE]
   > Pokud chcete toopublish tooan libovolný koncový bod (například strany clusteru), přečtěte si téma hello **publikování koncový bod libovolný clusteru tooan** části níže.
   > 
   > 
   
    Jakmile zvolíte koncového bodu, ověří Visual Studio cluster Service Fabric toohello vybrané připojení hello. Pokud hello cluster není zabezpečený, Visual Studio můžete připojit tooit okamžitě. Ale pokud hello clusteru je bezpečné, budete potřebovat tooinstall certifikát v místním počítači, než budete pokračovat. V tématu [jak tooconfigure zabezpečené připojení](service-fabric-visualstudio-configure-secure-connections.md) Další informace. Až budete hotoví, zvolte hello **OK** tlačítko. vybraný cluster Hello se zobrazí v hello **publikovat aplikace Service Fabric** dialogové okno.
3. V hello **soubor parametrů aplikace** rozevíracího seznamu pole, najděte soubor parametrů tooan aplikace. Parametr souboru aplikace obsahuje zadán uživatel hodnoty pro parametry v souboru manifestu aplikace hello. tooadd nebo změňte parametr, zvolte hello **upravit** tlačítko. Zadejte nebo změňte hodnotu parametru hello v hello **parametry** mřížky. Až budete hotoví, zvolte hello **Uložit** tlačítko.
   
    ![Hello ** upravit parametry ** dialogové okno][2]
4. Použití hello **upgradu hello aplikace** toospecify zaškrtávací políčko, zda tuto publikování akce je upgrade. Upgrade publikování akce se liší od normální publikování akce. V tématu [upgradu aplikace Service Fabric](service-fabric-application-upgrade.md) seznam rozdíly. nastavení upgradu tooconfigure, zvolte hello **nakonfigurovat nastavení upgradu** odkaz. Zobrazí se editor parametr upgradu Hello. V tématu [konfigurovat hello upgrade aplikace Service Fabric](service-fabric-visualstudio-configure-upgrade.md) toolearn Další informace o upgradu parametry.
5. Zvolte hello **verze manifestu...** tlačítko tooview hello **upravit verze** dialogové okno. Budete potřebovat tooupdate aplikace a verze aktualizace service pro upgradu tootake místo. V tématu [kurz upgradu aplikace Service Fabric](service-fabric-application-upgrade-tutorial.md) toolearn jak aplikace a verze manifestu služby vliv upgradu.
   
    ![Hello ** upravit verze ** dialogové okno][3]
   
    Pokud aplikace hello a verze aktualizace service použít sémantické verze jako je například 1.0.0 nebo číselné hodnoty ve formátu hello 1.0.0.0, vyberte hello **automatické aktualizaci aplikace a verze aktualizace service** možnost. Když zvolíte tuto možnost, služba hello a čísel verzí aplikace se automaticky aktualizují vždy, když kód, konfigurace, nebo verze balíčku dat se aktualizuje. Pokud dáváte přednost tooedit hello verze ručně, zrušte tuto funkci toodisable hello zaškrtávací políčko.
   
   > [!NOTE]
   > Pro všechny položky tooappear balíčků pro projekt objektu actor nejprve vytvořte hello projektu toogenerate hello položek v Service Manifest soubory hello.
   > 
   > 
6. Po dokončení zadání všechna potřebná nastavení hello, zvolte hello **publikovat** tlačítko toopublish vaší aplikace toohello vybraný cluster Service Fabric. Hello nastavení, které jste zadali, se použijí toohello publikování procesu.

## <a name="publish-tooan-arbitrary-cluster-endpoint-including-party-clusters"></a>Publikování tooan libovolný clusteru koncového bodu (včetně clusterů strany)
Hello publikování prostředí sady Visual Studio je optimalizovaná pro publikování tooremote clustery přiřazený k některé z vašich předplatných Azure. Je však možné toopublish tooarbitrary koncových bodů (například clusterů Service Fabric strany) podle přímo úpravy hello Publikovat profil XML. Jak je popsáno výše, tři publikační profily jsou dostupné ve výchozím nastavení--**Local.1Node.xml**, **Local.5Node.xml**, a **Cloud.xml**–, ale jsou úvodní toocreate Další profily pro různá prostředí. Například můžete toocreate profil pro publikování tooparty clusterů, případně s názvem **Party.xml**.

Pokud se připojujete tooan nezabezpečenou clusteru, všechny, které je nutné hello koncový bod připojení clusteru, je třeba `partycluster1.eastus.cloudapp.azure.com:19000`. V tomto případě koncového bodu připojení hello v hello Publikovat profil by vypadat přibližně takto:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Pokud se připojujete tooa zabezpečené clusteru, musíte se také podrobnosti hello tooprovide hello klientský certifikát z toobe místního úložiště hello používá k ověřování. Další podrobnosti najdete v tématu [cluster Service Fabric tooa konfigurace zabezpečeného připojení](service-fabric-visualstudio-configure-secure-connections.md).

  Až nastavíte svůj profil publikování, který je odkazujete v hello dialogové okno publikování, jak je uvedeno níže.

  ![Nový profil publikování v dialogové okno publikování][4]

  Všimněte si, že v tomto případě hello nový profil publikování bodů tooone soubory parametrů aplikace hello výchozí. To je vhodné, pokud chcete toopublish hello prostředí stejné číslo tooa konfigurace aplikace. Naopak v případech místo toohave různé konfigurace pro každé prostředí, který chcete toopublish k jeho by mít smysl toocreate odpovídající parametr soubor aplikace.

## <a name="next-steps"></a>Další kroky
jak zjistit, proces publikování tooautomate hello v prostředí průběžnou integraci toolearn [nastavte průběžnou integraci Service Fabric](service-fabric-set-up-continuous-integration.md).

[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
