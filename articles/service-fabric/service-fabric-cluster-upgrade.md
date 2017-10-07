---
title: "aaaUpgrade clusteru služby Azure Service Fabric | Microsoft Docs"
description: "Upgradujte hello Service Fabric kód nebo konfigurace používající cluster Service Fabric, včetně nastavení režimu aktualizace clusteru upgrade certifikáty, přidání aplikace porty, provádění oprav operačního systému, a tak dále. Co můžete očekávat, když se provádí upgrade hello?"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 15190ace-31ed-491f-a54b-b5ff61e718db
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/10/2017
ms.author: chackdan
ms.openlocfilehash: 94ac3833ec0810f79de06ecb50f254028fa90408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-an-azure-service-fabric-cluster"></a>Upgrade clusteru služby Azure Service Fabric
> [!div class="op_single_selector"]
> * [Azure clusteru](service-fabric-cluster-upgrade.md)
> * [Samostatné clusteru](service-fabric-cluster-upgrade-windows-server.md)
> 
> 

Pro všechny moderní systém je návrh pro zdokonalovány klíče tooachieving dlouhodobý úspěch vašeho produktu. Cluster služby Azure Service Fabric je na prostředek, který vlastníte, ale je částečně spravováno společností Microsoft. Tento článek popisuje, co je spravováno automaticky a co můžete nakonfigurovat sami.

## <a name="controlling-hello-fabric-version-that-runs-on-your-cluster"></a>Řízení hello verze fabric, která běží na clusteru
Můžete nastavit architektury clusteru tooreceive automatické upgrady jako vydané společností Microsoft nebo můžete vybrat verzi podporované prostředků infrastruktury, které chcete toobe vašeho clusteru na.

To provedete pomocí nastavení hello "upgradeMode" konfigurace clusteru na hello portálu nebo pomocí Správce prostředků v době vytvoření hello nebo později na clusteru s podporou za provozu 

> [!NOTE]
> Ujistěte se, že tookeep cluster vždy používá verzi podporovanou prostředků infrastruktury. Jak a kdy jsme oznamujeme hello vydání nové verze aplikace service fabric, předchozí verze hello je označen pro ukončení podpory po 60 dnů od tohoto dne. Hello hello nové verze není zmínka [na blogu týmu prostředků infrastruktury služby hello](https://blogs.msdn.microsoft.com/azureservicefabric/). pak je k dispozici toochoose Hello nová verze. 
> 
> 

14 dnů před toohello vypršení platnosti hello verze vašeho clusteru je spuštěn, událost stavu se vygeneruje, který umísťuje clusteru do stavu upozornění. Hello cluster zůstane ve stavu upozornění, dokud neprovedete upgrade verze tooa podporované fabric.

### <a name="setting-hello-upgrade-mode-via-portal"></a>Nastavení režimu upgradu hello prostřednictvím portálu
Hello clusteru tooautomatic nebo ruční můžete nastavit při vytváření clusteru hello.

![Create_Manualmode][Create_Manualmode]

Můžete nastavit hello clusteru tooautomatic nebo ruční v clusteru s podporou za provozu, pomocí hello Správa prostředí. 

#### <a name="upgrading-tooa-new-version-on-a-cluster-that-is-set-toomanual-mode-via-portal"></a>Upgrade tooa novou verzi v clusteru, který je nastavený režim tooManual přes portál.
tooupgrade tooa je nová verze, stačí toodo vyberte z rozevíracího seznamu hello hello k dispozici verzi a uložit. upgrade pro Fabric Hello získá spuštěna automaticky. Hello zásady stavu clusteru (kombinaci uzlu stavu a stavu hello všechny hello aplikace běžící v clusteru hello) jsou zachovány tooduring hello upgradu.

Pokud zásady stavu hello clusteru nejsou splněny, hello upgrade vrácena zpět. Projděte dolů tento dokument tooread Další informace o tom tooset tyto zásady vlastní stavu. 

Po opravě hello problémy, které výsledkem hello vrácení musíte tooinitiate hello upgradu znovu podle následujících hello stejné kroky jako předtím.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-hello-upgrade-mode-via-a-resource-manager-template"></a>Nastavení režimu upgradu hello pomocí šablony Resource Manageru
Přidejte hello definice prostředků Microsoft.ServiceFabric/clusters toohello konfigurace "upgradeMode" a sadu hello "clusterCodeVersion" tooone Dobrý den podporované verze prostředků infrastruktury, jak je uvedeno níže a pak nasaďte šablonu hello. jsou platné hodnoty "upgradeMode" Hello "Ruční" nebo "Automatické"

![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-tooa-new-version-on-a-cluster-that-is-set-toomanual-mode-via-a-resource-manager-template"></a>Upgrade tooa novou verzi v clusteru, který je nastavený režim tooManual pomocí šablony Resource Manageru.
Po hello clusteru v ručním režimu tooupgrade tooa novou verzi, změňte hello "clusterCodeVersion" tooa podporovanou verzi a nasadit. nasazení Hello hello šablony, zejména kopance o upgrade pro Fabric hello získá spuštěna automaticky. Hello zásady stavu clusteru (kombinaci uzlu stavu a stavu hello všechny hello aplikace běžící v clusteru hello) jsou zachovány tooduring hello upgradu.

Pokud zásady stavu hello clusteru nejsou splněny, hello upgrade vrácena zpět. Projděte dolů tento dokument tooread Další informace o tom tooset tyto zásady vlastní stavu. 

Po opravě hello problémy, které výsledkem hello vrácení musíte tooinitiate hello upgradu znovu podle následujících hello stejné kroky jako předtím.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Získejte seznam všech dostupných verze pro všechna prostředí pro dané předplatné
Spustit hello následující příkaz a měli byste obdržet podobné toothis výstupu.

"supportExpiryUtc" informuje vaší Pokud danou verzi vyprší nebo vypršela platnost. Hello nejnovější verze nemá platné datum - má hodnotu "9999-12-31T23:59:59.9999999", což právě znamená, že datum vypršení platnosti hello ještě není nainstalovaný.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/locations/{{location}}/clusterVersions?api-version=2016-09-01

Example: https://management.azure.com/subscriptions/1857f442-3bce-4b96-ad95-627f76437a67/providers/Microsoft.ServiceFabric/locations/eastus/clusterVersions?api-version=2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-hello-cluster-upgrade-mode-is-automatic"></a>Fabric upgradu chování, když hello cluster upgradovat režimu je automatický
Společnost Microsoft udržuje kód hello prostředků infrastruktury a konfigurace, která běží v clusteru služby Azure. Nemůžeme provést automatické upgrady monitorovaných toohello softwaru podle potřeby. Tyto upgrady může být kódu, konfigurace nebo obojí. toomake se, že vaše aplikace vyskytne žádný dopad nebo minimálním dopadem kvůli toothese upgrady, proveďte jsme hello upgrady v hello následující fáze:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>Fáze 1: Upgrade se provádí pomocí všechny zásady stavu clusteru
V průběhu této fáze upgradu hello pokračovat jednu upgradovací doménu najednou a hello aplikace, které byly spuštěny v clusteru hello pokračovat toorun bez odstávky. Hello zásady stavu clusteru (kombinaci uzlu stavu a stavu hello všechny hello aplikace běžící v clusteru hello) jsou zachovány tooduring hello upgradu.

Pokud zásady stavu hello clusteru nejsou splněny, hello upgrade vrácena zpět. E-mailu se pak odešlou toohello vlastníka předplatného hello. e-mailu Hello obsahuje hello následující informace:

* Oznámení, že jsme měli tooroll zpět na upgrade clusteru.
* Navrhované nápravné akce, pokud existuje.
* Hello počet dní (ne), dokud jsme provést fáze 2.

Pokusíme tooexecute hello stejný upgrade několik vícekrát v případě, že jakéhokoli upgradu se nezdařilo z důvodů infrastruktury. Po hello byla odeslána n dní od hello datum hello e-mailu, budeme pokračovat tooPhase 2.

Pokud jsou splněny zásady stavu hello clusteru, je hello upgrade považuje za úspěšnou a označeny jako dokončené. Může dojít během upgradu počáteční hello ani v žádné z opakování upgradu hello v této fázi. Neexistuje žádné potvrzení e-mailu úspěšně spustit. Toto je tooavoid odesílání, že jste příliš mnoho e-mailů – příjem e-mailu měla by se zobrazit jako toonormal k výjimce. Očekáváme, že většina toosucceed upgrady hello clusteru bez dopadu na dostupnost vaší aplikace.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>Fáze 2: Upgrade se provádí pomocí výchozího stavu pouze zásady
Hello stavu v této fázi jsou zásady nastavené tak, aby zůstala hello počet aplikací, které byly od začátku hello hello upgradu v pořádku pro stejný hello dobu trvání procesu upgradu hello hello. Jako fáze 1 hello fáze 2 upgrady pokračovat jednu upgradovací doménu najednou a hello aplikace, které byly spuštěny v clusteru hello pokračovat toorun bez odstávky. zásady stavu clusteru Hello (kombinaci uzlu stavu a stavu hello všechny hello aplikace běžící v clusteru hello) jsou nalepeného toofor hello trvání upgradu hello.

Pokud platí zásady stavu hello clusteru nejsou splněny, hello upgrade vrácena zpět. E-mailu se pak odešlou toohello vlastníka předplatného hello. e-mailu Hello obsahuje hello následující informace:

* Oznámení, že jsme měli tooroll zpět na upgrade clusteru.
* Navrhované nápravné akce, pokud existuje.
* Hello počet dní (ne), dokud jsme provést fáze 3.

Pokusíme tooexecute hello stejný upgrade několik vícekrát v případě, že jakéhokoli upgradu se nezdařilo z důvodů infrastruktury. Připomenutí e-mail je odeslán během několika dní, než n dní jsou. Po hello byla odeslána n dní od hello datum hello e-mailu, budeme pokračovat tooPhase 3. Hello e-mailů, které vám můžeme poslat v fáze 2 bude nutno vážně a musí být přijata nápravné akce.

Pokud jsou splněny zásady stavu hello clusteru, je hello upgrade považuje za úspěšnou a označeny jako dokončené. Může dojít během upgradu počáteční hello ani v žádné z opakování upgradu hello v této fázi. Neexistuje žádné potvrzení e-mailu úspěšně spustit.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>Fáze 3: Upgrade se provádí na základě zásad agresivní stavu
Tyto zásady stavu v této fázi jsou s ohledem na dokončení upgradu hello spíše než hello stav hello aplikací. V této fázi skončili jen několik upgradů clusteru. Pokud váš cluster získá toothis fáze, je velmi pravděpodobné, že vaše aplikace se změní na není v pořádku nebo ztrátu dostupnosti.

Podobně jako toohello další dvě fáze 3 fáze upgradu pokračovat jednu upgradovací doménu najednou.

Pokud zásady stavu hello clusteru nejsou splněny, hello upgrade vrácena zpět. Pokusíme tooexecute hello stejný upgrade několik vícekrát v případě, že jakéhokoli upgradu se nezdařilo z důvodů infrastruktury. Potom je ukotvena hello clusteru, tak, aby se už nebude podporu nebo upgrady.

Vlastník předplatného toohello, společně s hello nápravné akce se odeslal e-mail s tyto informace. Není Očekáváme, že všechny clustery tooget do stavu, kde se nezdařilo fáze 3.

Pokud jsou splněny zásady stavu hello clusteru, je hello upgrade považuje za úspěšnou a označeny jako dokončené. Může dojít během upgradu počáteční hello ani v žádné z opakování upgradu hello v této fázi. Neexistuje žádné potvrzení e-mailu úspěšně spustit.

## <a name="cluster-configurations-that-you-control"></a>Konfigurace clusteru, které řídíte
Kromě toho toohello možnost tooset hello cluster upgradovat režimu, tady jsou hello konfigurace, které můžete změnit na cluster za provozu.

### <a name="certificates"></a>Certifikáty
Můžete přidat nové nebo snadno odstranit certifikáty pro hello clusteru a klientů přes portál hello. Odkazovat příliš[tento dokument podrobné pokyny](service-fabric-cluster-security-update-certs-azure.md)

![Snímek obrazovky, který zobrazuje kryptografické otisky certifikátu v hello portálu Azure.][CertificateUpgrade]

### <a name="application-ports"></a>Porty aplikace
Porty aplikace můžete změnit změnou hello nástroj pro vyrovnávání zatížení prostředků vlastnosti, které jsou přidružené typ uzlu hello. Můžete použít portál hello nebo můžete použít Správce prostředků PowerShell přímo.

tooopen nový port na všech virtuálních počítačích v uzlu typu hello následující:

1. Přidejte nového testu toohello odpovídající služba Vyrovnávání zatížení.
   
    Pokud jste nasadili cluster pomocí portálu hello, jsou nástroje pro vyrovnávání zatížení hello s názvem "LB-název skupiny prostředků hello-NodeTypename", jednu pro každý typ uzlu. Vzhledem k tomu, že jsou názvy nástroje pro vyrovnávání zatížení hello pouze v rámci skupiny prostředků jedinečný, je nejlepší Pokud vyhledávání v rámci určité skupiny zdrojů.
   
    ![Snímek obrazovky zobrazující přidání tooa testu vyrovnávání zátěže hello portálu.][AddingProbes]
2. Přidáte nové pravidlo Vyrovnávání zatížení toohello.
   
    Přidáte nové pravidlo toohello stejný nástroj pro vyrovnávání zatížení pomocí hello kontrolu, kterou jste vytvořili v předchozím kroku hello.
   
    ![Přidání nové pravidlo Vyrovnávání zatížení tooa hello portálu.][AddingLBRules]

### <a name="placement-properties"></a>Vlastnosti umístění
Pro každý typ uzlu hello můžete přidat vlastní umístění vlastnosti chcete toouse ve svých aplikacích. Typ uzlu je ve výchozím nastavení, kterou můžete použít bez přidání explicitně.

> [!NOTE]
> Podrobnosti o použití hello omezení umístění, vlastnosti uzlu, a jak toodefine, najdete v části toohello "Omezení umístění a vlastnosti uzlu" v hello dokumentu správce prostředků clusteru Service Fabric na [popisující vaše clusteru ](service-fabric-cluster-resource-manager-cluster-description.md).
> 
> 

### <a name="capacity-metrics"></a>Metriky kapacity
Pro každý typ uzlu hello můžete přidat metriky vlastní kapacity chcete toouse v tooreport zatížení aplikací. Podrobnosti o použití hello zatížení tooreport metriky kapacity naleznete toohello dokumenty správce prostředků clusteru Service Fabric v [popisující vaše clusteru](service-fabric-cluster-resource-manager-cluster-description.md) a [metriky a zatížení](service-fabric-cluster-resource-manager-metrics.md).

### <a name="fabric-upgrade-settings---health-polices"></a>Upgrade nastavení prostředků infrastruktury – zásady stavu
Můžete zadat, že stav vlastní zásady pro upgrade pro fabric. Pokud jste nastavili vašeho clusteru tooAutomatic upgrady prostředků infrastruktury, získat tyto zásady použité toohello fáze 1 upgradu hello automatické prostředků infrastruktury.
Pokud jste nastavili clusteru pro ruční fabric upgrady, pak tyto zásady použije pokaždé, když vyberete novou verzi aktivován hello systému tookick vypnout upgrade pro fabric hello v clusteru. Pokud není přepsat hello zásady, budou použity výchozí hodnoty hello.

Můžete určit zásady vlastní stavu hello nebo zkontrolujte hello aktuální nastavení v okně "upgrade pro fabric" hello, výběrem hello Upřesnit nastavení upgradu. Zkontrolujte následující obrázek o tom, jak hello. 

![Správa vlastní stav zásad][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>Přizpůsobení nastavení prostředků infrastruktury pro váš cluster
Odkazovat příliš[nastavení prostředků infrastruktury clusteru prostředků infrastruktury služby](service-fabric-cluster-fabric-settings.md) na co a jak je můžete přizpůsobit.

### <a name="os-patches-on-hello-vms-that-make-up-hello-cluster"></a>Opravy operačního systému na hello virtuálních počítačů, které tvoří hello clusteru
Odkazovat příliš[použití opravy Orchestration](service-fabric-patch-orchestration-application.md) které by mohl být nasazen na váš clusteru tooinstall opravy ze služby Windows Update orchestrated způsobem, sledovat dostupnost služeb hello všechny hello čas. 

### <a name="os-upgrades-on-hello-vms-that-make-up-hello-cluster"></a>Upgrade operačního systému na hello virtuálních počítačů, které tvoří hello clusteru
Pokud musíte provést upgrade hello bitové kopie operačního systému na virtuálních počítačích hello hello clusteru, je nutné ji jeden virtuální počítač najednou. Jste zodpovědní pro tento upgrade – v současné době není žádné automation pro tento.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak toocustomize některé hello [nastavení prostředků infrastruktury clusteru prostředků infrastruktury služby](service-fabric-cluster-fabric-settings.md)
* Zjistěte, jak příliš[škálování vašeho clusteru a odhlášení](service-fabric-cluster-scale-up-down.md)
* Další informace o [upgradů aplikací](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
