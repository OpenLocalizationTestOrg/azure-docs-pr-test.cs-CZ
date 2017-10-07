---
title: "aaaSetting se cluster Service Fabric pomocí sady Visual Studio | Microsoft Docs"
description: "Popisuje, jak clusteru tooset až Service Fabric pomocí šablony Azure Resource Manageru vytvořený projekt skupiny prostředků Azure v sadě Visual Studio"
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: adegeo
editor: 
ms.assetid: bd2c0511-36c9-4828-8dc3-69e4b6a70567
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/21/2017
ms.author: mikhegn
redirect_url: /azure/service-fabric/service-fabric-cluster-creation-via-arm
ms.openlocfilehash: adb0dd2169a28b46e832c6f06c998cbed0c473f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a>Nastavení clusteru Service Fabric pomocí sady Visual Studio
Tento článek popisuje, jak clusteru tooset nahoru Azure Service Fabric pomocí Visual Studia a šablonu Azure Resource Manager. Budeme používat šablony Visual Studio Azure prostředků skupiny projektu toocreate hello. Po vytvoření šablony hello, dá se nasadit přímo tooAzure ze sady Visual Studio. Může taky sloužit ze skriptu, nebo jako součást budovy průběžnou integraci (konfigurace).

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a>Vytvoření šablony clusteru Service Fabric pomocí projektu skupiny prostředků Azure.
spuštění tooget, otevřete Visual Studio a vytvoření projektu skupiny prostředků Azure (je k dispozici v hello **cloudu** složku):

![Dialogové okno Nový projekt s vybrané projekt skupiny prostředků Azure][1]

Můžete vytvořit nové řešení sady Visual Studio pro tento projekt nebo ho přidat tooan existující řešení.

> [!NOTE]
> Pokud nevidíte hello projekt skupiny prostředků Azure v rámci uzlu hello cloudu, není nutné hello nainstalované sady Azure SDK. Spuštění instalačního programu webové platformy ([ji teď nainstalovat](http://www.microsoft.com/web/downloads/platform.aspx) Pokud máte ještě není) a poté vyhledejte "Azure SDK pro .NET" a verze hello instalace, který je kompatibilní s vaší verzí sady Visual Studio.
> 
> 

Po kliknutí na tlačítko OK hello, Visual Studio se vás zeptá šablony Resource Manageru hello tooselect chcete toocreate:

![Vyberte šablonu Azure dialogové okno s vybraná šablona Service Fabric Cluster][2]

Vyberte šablonu hello Service Fabric Cluster a tlačítko přístupů hello OK znovu. Nyní byly vytvořeny Hello projekt a šablony Resource Manageru hello.

## <a name="prepare-hello-template-for-deployment"></a>Příprava hello šablony pro nasazení
Předtím, než je šablona hello nasazené toocreate hello clusteru, je nutné zadat hodnoty pro parametry hello požadované šablony. Tyto hodnoty parametrů se načítají z hello `ServiceFabricCluster.parameters.json` souboru, který je v hello `Templates` složky hello projekt skupiny prostředků. Otevřete soubor hello a zadejte hello následující hodnoty:

| Název parametru | Popis |
| --- | --- |
| adminUserName |Hello název účtu správce hello Service Fabric počítačů (uzlů). |
| certificateThumbprint |Hello kryptografický otisk certifikátu hello, který zabezpečuje hello clusteru. |
| sourceVaultResourceId |Hello *ID prostředku* hello klíče trezoru se uloží hello certifikát, který zabezpečuje hello clusteru. |
| certificateUrlValue |Adresa URL Hello hello certifikát zabezpečení clusteru. |

šablony Visual Studio Service Fabric Resource Manageru Hello vytvoří zabezpečené clusteru, který je chráněný pomocí certifikátu. Tento certifikát je určený podle hello poslední tři parametry šablony (`certificateThumbprint`, `sourceVaultValue`, a `certificateUrlValue`), a musí existovat v **Azure Key Vault**. Další informace o tom, jak toocreate hello certifikát zabezpečení clusteru najdete v tématu [scénáře zabezpečení clusteru Service Fabric](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) článku.

## <a name="optional-change-hello-cluster-name"></a>Volitelné: Změna názvu clusteru hello
Každý cluster Service Fabric obsahuje název. Při vytváření clusteru s podporou prostředků infrastruktury v Azure (spolu s hello oblasti Azure) určuje název clusteru hello systému DNS (Domain Name) název pro hello cluster. Například pokud použijete název clusteru `myBigCluster`a hello umístěním (oblastí Azure) hello skupiny prostředků, který bude hostitelem hello nového clusteru je Východ USA, bude název DNS hello hello clusteru `myBigCluster.eastus.cloudapp.azure.com`.

Ve výchozím nastavení je název clusteru hello generovány automaticky a jedinečný provedené připojení předponu náhodných příponu tooa "clusteru". Díky tomu šablony velmi snadné toouse hello jako součást **průběžnou integraci** systému (konfigurace). Pokud chcete, aby toouse určitý název pro váš cluster, ten, který je smysluplný tooyou, nastavte hodnotu hello hello `clusterName` proměnné v souboru šablony Resource Manageru hello (`ServiceFabricCluster.json`) tooyour zvolený název. Je hello první proměnné definované v tomto souboru.

## <a name="optional-add-public-application-ports"></a>Volitelné: přidejte veřejný aplikace porty
Můžete také toochange hello veřejné aplikace porty pro hello cluster před nasazením. Ve výchozím nastavení otevře se hello šablony ještě dvě veřejné porty TCP (80 a 8081). Pokud potřebujete více pro vaše aplikace, upravte hello nástroj pro vyrovnávání zatížení Azure v šabloně hello. definice Hello je uložený v souboru hlavní šablonu hello (`ServiceFabricCluster.json`). Otevřete tento soubor a vyhledejte `loadBalancedAppPort`. Každý z portů je spojena s tři artefakty:

1. Proměnné šablony, která definuje hello hodnota portu TCP pro hello port:
   
    ```json
    "loadBalancedAppPort1": "80"
    ```
2. A *testu* který definuje, jak často a jak dlouho hello nástroje pro vyrovnávání zatížení Azure přes tooanother jeden pokusů o zadání toouse konkrétním uzlu Service Fabric, než selže. sondy Hello jsou součástí hello prostředek pro vyrovnávání zatížení. Zde je definice testu hello hello první výchozí aplikace port:
   
    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[variables('loadBalancedAppPort1')]",
            "protocol": "Tcp"
        }
    }
    ```
3. A *pravidlo Vyrovnávání zatížení* který společně sváže hello port a hello kontroly, které umožňuje vyrovnávání napříč sada uzlů clusteru Service Fabric zatížení:
   
    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[variables('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[variables('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "Tcp"
        }
    }
    ```
   Pokud aplikace hello plánování toodeploy toohello clusteru potřebují další porty, můžete je přidat tak, že vytváření další kontroly a definicí pravidel Vyrovnávání zatížení. Další informace o tom, toowork s Vyrovnávání zatížení Azure pomocí šablony Resource Manageru, najdete v části [začínáte s vytvářením interní nástroj pomocí šablony](../load-balancer/load-balancer-get-started-ilb-arm-template.md).

## <a name="deploy-hello-template-by-using-visual-studio"></a>Nasazení šablony hello pomocí sady Visual Studio
Po uložení všech hello požadovaný parametr hodnoty v`ServiceFabricCluster.param.dev.json` souboru, jsou připravené toodeploy hello šablony a vytvořit cluster Service Fabric. Klikněte pravým tlačítkem na projekt skupiny prostředků hello v Průzkumníku řešení Visual Studio a zvolte **nasadit | Nové nasazení...** . Pokud potřeby Visual Studio se zobrazí hello **nasazení tooResource skupiny** dialogového okna, s tooauthenticate tooAzure:

![Nasazení skupiny tooResource dialogové okno][3]

Dialogové okno Hello způsobem můžete vybrat existující skupinu prostředků Resource Manager hello clusteru a poskytuje hello toocreate možnost Nový. Za normálních okolností má smysl toouse skupinu samostatné prostředků pro cluster Service Fabric.

Po kliknutí na tlačítko nasadit hello, Visual Studio zobrazí výzvu hodnot parametrů šablony tooconfirm hello. Přístupů hello **Uložit** tlačítko. Jeden parametr nemá hodnotu trvalou: hello heslo účtu správce pro hello cluster. Když Visual Studio vás vyzve k zadání jednoho musíte tooprovide hodnotu heslo.

> [!NOTE]
> Počínaje sadu Azure SDK 2.9, Visual Studio podporuje čtení hesla z **Azure Key Vault** během nasazení. V dialogovém okně Parametry šablony hello Všimněte si, že hello `adminPassword` parametr textového pole má malé ikony "klíč" na hello správné. Tato ikona umožňuje tooselect existujícím tajným klíčem trezoru klíčů jako hello hesla pro správu pro hello cluster. Ujistěte se, toofirst povolení přístupu Azure Resource Manageru pro nasazení šablony hello pokročilé zásady přístupu trezoru klíčů. 
> 
> 

Můžete sledovat průběh procesu nasazení hello v okně výstupu sady Visual Studio hello hello. Po dokončení nasazení šablony hello nového clusteru je připravené toouse!

> [!NOTE]
> Pokud PowerShell nikdy použité tooadminister Azure z hello počítače, který teď používáte, musíte toodo trochu housekeeping.
> 
> 1. Povolit skriptování spuštěním hello prostředí PowerShell [ `Set-ExecutionPolicy` ](https://technet.microsoft.com/library/hh849812.aspx) příkaz. Pro počítače, vývoj je obvykle přijatelné "neomezený" zásady.
> 2. Rozhodněte, jestli tooallow shromažďování diagnostických dat z příkazů prostředí Azure PowerShell a spusťte [ `Enable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619303.aspx) nebo [ `Disable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619236.aspx) podle potřeby. Tím se vyhnete nepotřebné výzvy během nasazení šablony.
> 
> 

Pokud nejsou žádné chyby, přejděte toohello [portál Azure](https://portal.azure.com/) a otevřete hello skupinu prostředků, které jste nasadili. Klikněte na tlačítko **všechna nastavení**, pak klikněte na tlačítko **nasazení** v okně Nastavení hello. Selhání nasazení skupiny prostředků existuje zanechává podrobné diagnostické informace.

> [!NOTE]
> Clusterů Service Fabric vyžadují určitý počet uzlů toobe dostupnost toomaintain a zachovávají stav – odkazované tooas "údržbu kvora." Proto není bezpečné tooshut dolů všechny hello počítače v clusteru hello Pokud jste provedli nejprve [úplného zálohování vaší stavu](service-fabric-reliable-services-backup-restore.md).
> 
> 

## <a name="next-steps"></a>Další kroky
* [Další informace o nastavení clusteru Service Fabric pomocí hello portálu Azure](service-fabric-cluster-creation-via-portal.md)
* [Zjistěte, jak toomanage a nasazování aplikací Service Fabric pomocí sady Visual Studio](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
