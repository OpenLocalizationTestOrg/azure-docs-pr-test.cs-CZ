---
title: "Nastavení clusteru Service Fabric pomocí sady Visual Studio | Microsoft Docs"
description: "Popisuje, jak nastavit cluster Service Fabric pomocí šablony Azure Resource Manageru vytvořený projekt skupiny prostředků Azure v sadě Visual Studio"
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
ms.openlocfilehash: c43145b96cdbdfaa7e1893e50d027321fe4c0510
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a>Nastavení clusteru Service Fabric pomocí sady Visual Studio
Tento článek popisuje, jak nastavit cluster služby Azure Service Fabric pomocí Visual Studia a šablonu Azure Resource Manager. Použijeme projektu skupiny prostředků Visual Studio Azure k vytvoření šablony. Po vytvoření šablony, dá se nasadit přímo do Azure ze sady Visual Studio. Může taky sloužit ze skriptu, nebo jako součást budovy průběžnou integraci (konfigurace).

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a>Vytvoření šablony clusteru Service Fabric pomocí projektu skupiny prostředků Azure.
Abyste mohli začít, otevřete Visual Studio a vytvoření projektu skupiny prostředků Azure (je k dispozici v **cloudu** složku):

![Dialogové okno Nový projekt s vybrané projekt skupiny prostředků Azure][1]

Můžete vytvořit nové řešení sady Visual Studio pro tento projekt nebo přidat do existujícího řešení.

> [!NOTE]
> Pokud nevidíte projekt skupiny prostředků Azure pod uzlem cloudu, nemáte sadu Azure SDK nainstalována. Spuštění instalačního programu webové platformy ([ji teď nainstalovat](http://www.microsoft.com/web/downloads/platform.aspx) Pokud máte ještě není) a poté vyhledejte "Azure SDK pro .NET" a nainstalujte verzi, která je kompatibilní s vaší verzí sady Visual Studio.
> 
> 

Po kliknutí na tlačítko OK, Visual Studio se zeptá, chcete-li vybrat šablonu Resource Manager, kterou chcete vytvořit:

![Vyberte šablonu Azure dialogové okno s vybraná šablona Service Fabric Cluster][2]

Vyberte šablonu, Service Fabric Cluster a znovu stiskněte tlačítko OK. Nyní byly vytvořeny projekt a šablony Resource Manageru.

## <a name="prepare-the-template-for-deployment"></a>Příprava pro nasazení šablony
Před nasazením šablony k vytvoření clusteru, je nutné zadat hodnoty pro parametry požadované šablony. Tyto hodnoty parametrů se načítají z `ServiceFabricCluster.parameters.json` souboru, který je v `Templates` složku projekt skupiny prostředků. Otevřete soubor a zadejte následující hodnoty:

| Název parametru | Popis |
| --- | --- |
| adminUserName |Název účtu správce pro Service Fabric počítačů (uzlů). |
| certificateThumbprint |Kryptografický otisk certifikátu, který zabezpečuje clusteru. |
| sourceVaultResourceId |*ID prostředku* služby key vault, kde je uložen certifikát, který zabezpečuje clusteru. |
| certificateUrlValue |Adresa URL certifikátu zabezpečení clusteru. |

Šablony Visual Studio Service Fabric Resource Manageru vytvoří zabezpečené clusteru, který je chráněný pomocí certifikátu. Tento certifikát je určený podle parametry poslední tři šablony (`certificateThumbprint`, `sourceVaultValue`, a `certificateUrlValue`), a musí existovat v **Azure Key Vault**. Další informace o tom, jak vytvořit certifikát zabezpečení clusteru najdete v tématu [scénáře zabezpečení clusteru Service Fabric](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) článku.

## <a name="optional-change-the-cluster-name"></a>Volitelné: Změna názvu clusteru
Každý cluster Service Fabric obsahuje název. Při vytváření clusteru s podporou prostředků infrastruktury v Azure název clusteru určuje (spolu s oblasti Azure) systému DNS (Domain Name) název pro cluster. Například pokud použijete název clusteru `myBigCluster`a umístění (oblasti Azure) skupiny prostředků, který bude nový cluster hostitelem je Východ USA, bude název DNS clusteru `myBigCluster.eastus.cloudapp.azure.com`.

Ve výchozím nastavení je název clusteru, generuje automaticky a jedinečný provedené příponu náhodných se připojuje k předponu "clusteru". Díky tomu je velmi snadné použít šablonu v rámci **průběžnou integraci** systému (konfigurace). Pokud chcete použít konkrétní název pro váš cluster, ten, který má smysl, nastavte hodnotu `clusterName` proměnné v souboru šablony Resource Manageru (`ServiceFabricCluster.json`) pro zvolený název. Je první proměnné definované v tomto souboru.

## <a name="optional-add-public-application-ports"></a>Volitelné: přidejte veřejný aplikace porty
Můžete také změnit porty veřejné aplikace pro cluster před nasazením. Ve výchozím nastavení otevře se šablony ještě dvě veřejné porty TCP (80 a 8081). Pokud potřebujete více pro vaše aplikace, upravte definici Azure nástroj pro vyrovnávání zatížení v šabloně. Definice je uložený v souboru hlavní šablony (`ServiceFabricCluster.json`). Otevřete tento soubor a vyhledejte `loadBalancedAppPort`. Každý z portů je spojena s tři artefakty:

1. Proměnná šablony, který definuje hodnotu portu TCP pro port:
   
    ```json
    "loadBalancedAppPort1": "80"
    ```
2. A *testu* který definuje, jak často a jak dlouho Azure zátěže vyrovnávání pokusí použít konkrétním uzlu Service Fabric před převzetí služeb při selhání na jiný. Sondy jsou součástí prostředek pro vyrovnávání zatížení. Tady je definici testu pro první výchozí port aplikace:
   
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
3. A *pravidlo Vyrovnávání zatížení* který společně sváže port a kontroly, které umožňuje vyrovnávání napříč sada uzlů clusteru Service Fabric zatížení:
   
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
   Pokud aplikace, které plánujete nasadit do clusteru potřebovat více porty, můžete je přidat tak, že vytváření další kontroly a definicí pravidel Vyrovnávání zatížení. Další informace o tom, jak pracovat s Vyrovnávání zatížení Azure pomocí šablony Resource Manageru najdete v tématu [začínáte s vytvářením interní nástroj pomocí šablony](../load-balancer/load-balancer-get-started-ilb-arm-template.md).

## <a name="deploy-the-template-by-using-visual-studio"></a>Nasazení šablony pomocí sady Visual Studio
Po uložení všech hodnot požadovaný parametr v`ServiceFabricCluster.param.dev.json` souboru, jste připraveni k nasazení šablony a vytvořit cluster Service Fabric. Klikněte pravým tlačítkem na projekt skupiny prostředků v Průzkumníku řešení Visual Studio a zvolte **nasadit | Nové nasazení...** . Pokud potřeby Visual Studio se zobrazí **nasadit do skupiny prostředků** dialogové okno, žádostí o ověřování v Azure:

![Nasazení do skupiny prostředků dialogového okna][3]

Dialogové okno umožňuje vybrat existující skupinu prostředků Resource Manageru pro cluster a vám dává možnost vytvořit novou. Za normálních okolností má smysl použít skupinu samostatné prostředků pro cluster Service Fabric.

Po kliknutí na tlačítko nasadit, Visual Studio zobrazí výzvu k potvrzení hodnot parametrů šablony. Stiskněte tlačítko **Uložit** tlačítko. Jeden parametr nemá hodnotu trvalou: heslo k účtu pro správu pro cluster. Je třeba zadat hodnotu heslo, když Visual Studio vás vyzve k zadání jednoho.

> [!NOTE]
> Počínaje sadu Azure SDK 2.9, Visual Studio podporuje čtení hesla z **Azure Key Vault** během nasazení. V dialogovém okně Parametry šablony Všimněte si, že `adminPassword` parametr textového pole má moc "klíč" ikonu na pravé straně. Tato ikona umožňuje vybrat existujícím tajným klíčem trezoru klíčů jako heslo správce pro cluster. Ujistěte se však na první povolit přístup správce prostředků Azure pro nasazení šablony v Pokročilé zásady přístupu k trezoru klíčů. 
> 
> 

Můžete sledovat průběh procesu nasazení v okně výstupu sady Visual Studio. Po dokončení nasazení šablony je připravený k použití nového clusteru!

> [!NOTE]
> Pokud prostředí PowerShell nebyl nikdy ke správě Azure z počítače, který teď používáte, musíte udělat ještě housekeeping.
> 
> 1. Povolit spuštěním skriptů prostředí PowerShell [ `Set-ExecutionPolicy` ](https://technet.microsoft.com/library/hh849812.aspx) příkaz. Pro počítače, vývoj je obvykle přijatelné "neomezený" zásady.
> 2. Rozhodněte, jestli chcete povolit shromažďování diagnostických dat z příkazů prostředí Azure PowerShell a spusťte [ `Enable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619303.aspx) nebo [ `Disable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619236.aspx) podle potřeby. Tím se vyhnete nepotřebné výzvy během nasazení šablony.
> 
> 

Pokud nejsou žádné chyby, přejděte k [portál Azure](https://portal.azure.com/) a otevřete skupinu prostředků, které jste nasadili. Klikněte na tlačítko **všechna nastavení**, pak klikněte na tlačítko **nasazení** v okně nastavení. Selhání nasazení skupiny prostředků existuje zanechává podrobné diagnostické informace.

> [!NOTE]
> Clusterů Service Fabric vyžadují určitý počet uzlů na si zachovat dostupnost a zároveň zachovat stav – označuje jako "údržbu kvora." Proto není bezpečné a ukončí se všechny počítače v clusteru Pokud jste provedli nejprve [úplného zálohování vaší stavu](service-fabric-reliable-services-backup-restore.md).
> 
> 

## <a name="next-steps"></a>Další kroky
* [Další informace o nastavení cluster Service Fabric pomocí portálu Azure](service-fabric-cluster-creation-via-portal.md)
* [Zjistěte, jak spravovat a nasazovat aplikace Service Fabric pomocí sady Visual Studio](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
